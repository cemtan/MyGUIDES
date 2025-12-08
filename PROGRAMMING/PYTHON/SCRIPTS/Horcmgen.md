#### HORCMGEN
---
---


```python
#!/usr/bin/python3
# coding:utf-8

# COPYRIGHT
# © 2022 Hitachi Vantara LLC. All rights reserved.

import base64
import requests
import json
import pandas as pd


svps = [{
            "ip": "10.0.0.1",
            "serial": "60026",
            "user": "maintenance",
            "password": "raid-maintenance"
        },
        {
            "ip": "10.0.0.2",
            "serial": "60019",
            "user": "maintenance",
            "password": "raid-maintenance"
        },
        {
            "ip": "10.0.0.3",
            "serial": "60022",
            "user": "maintenance",
            "password": "raid-maintenance"
        }]


horcm = """HORCM_MON
#ip_address	service	poll(10ms)		timeout(10ms)
localhost		{}	1000			3000

HORCM_CMD
# dev_name
#\\.\IPCMD-{}-31001

HORCM_LDEV
#dev_group              dev_name        Serial#   CU:LDEV(LDEV#)   MU#

"""


service_port_prefix = '3110'
headers = {"Content-Type": "application/json", "Accept": "application/json"}
format1 = ['{' + f':<{i}' + '}' for i in [15, 15, 15, 15, 15]]
format2 = ['{' + f':<{i}' + '}' for i in [15, 15, 15]]


df0 = pd.DataFrame()
df1 = pd.DataFrame()
df01g = pd.DataFrame()
df02g = pd.DataFrame()
df01h = pd.DataFrame()
df031h = pd.DataFrame()
df02h = pd.DataFrame()
df032h = pd.DataFrame()


def api_get(f_url, f_headers, f_title):
    try:
        f_response = requests.get(url=f_url, headers=f_headers, verify=False, timeout=300)
        print('TASK: ' + f_title  + ' | GET API RESPONSE : ' + str(f_response.status_code))
        return f_response
    except requests.exceptions.HTTPError as error:
        if error.response.status_code == 408 or error.response.status_code == 503:
            print('TASK: ' + f_title  + ' | STATE: Failed | GET: ' +  f_url)
        else:
            raise SystemExit(error)

def api_post(f_url, f_headers, f_title):
    try:
        f_response = requests.post(url=f_url, headers=f_headers, data='', verify=False, timeout=300)
        print('TASK: ' + f_title  + ' | POST API RESPONSE : ' + str(f_response.status_code))
        return f_response
    except requests.exceptions.HTTPError as error:
        if error.response.status_code == 408 or error.response.status_code == 503:
            print('TASK: ' + f_title  + ' | STATE: Failed | POST: ' +  f_url)
        else:
            raise SystemExit(error)
        

def get_auth(f_user, f_password):
    f_up = "{}:{}".format(f_user, f_password)
    f_auth = base64.b64encode(f_up.encode()).decode()
    return f_auth
    

def get_replication(f_svp):
    f_df = pd.DataFrame()
    url = "https://{}/ConfigurationManager/v1/objects/storages/".format(f_svp['ip'])
    auth = get_auth(f_svp['user'], f_svp['password'])
    headers['Authorization'] = 'Basic {}'.format(auth)
    response = api_get(url, headers, 'Finding storageDeviceId of ' + f_svp['serial'])
    storageDeviceId = response['data'][0]['storageDeviceId']
    url = "https://{}/ConfigurationManager/v1/objects/storages/{}/sessions".format(f_svp['ip'], storageDeviceId)
    response = api_post(url, headers, 'Getting token for ' + f_svp['serial'])
    if(len(response) > 0):
        token = response['token']
        headers['Authorization'] = 'Session {}'.format(token)
        url = "https://{}/ConfigurationManager/v1/objects/storages/{}/remote-replications".format(f_svp['ip'], storageDeviceId)
        response = api_get(url, headers, 'Getting replcation information from ' + f_svp['serial'])
        f_df = pd.json_normalize(response['data'])
        return f_df
    else:
        print("Token error: Returned token is empty for " + f_svp['serial'])


for id, svp in enumerate(svps):
    if id < len(svps) - 2:
        df0 = get_replication(svp) if id == 1 else df1 = get_replication(svp)


if not df0.empty:
    df0 = df0.sort_values(['replicationType', 'muNumber', 'consistencyGroupId'])
    df0['groupName'] = 'GRP' + df0['consistencyGroupId'].astype(str) + '_' + df0['replicationType'] + df0['muNumber'].astype(str)
    df0['ldevName'] = 'LDEV_' + df0['pvolLdevId'].astype(str)
    df0['muNumber'] = 'h' + df0['muNumber'].astype(str)
    
    df01 = df0[df0['replicationType'].str.match('GAD')]
    
    if not df01.empty:
        df01p = df01[df01['pvolStorageSerial'].str.match(svps[0]['serial'])].copy()
        df01p = df01p[['groupName', 'ldevName', 'pvolStorageSerial', 'pvolLdevId', 'muNumber']]
        df01p.rename(columns={"pvolStorageSerial": "storageSerial", "pvolLdevId": "ldevId"}, inplace=True)
        
        df01s = df01[df01['svolStorageSerial'].str.match(svps[0]['serial'])].copy()
        df01s = df01s[['groupName', 'ldevName', 'svolStorageSerial', 'svolLdevId', 'muNumber']]
        df01s.rename(columns={"svolStorageSerial": "storageSerial", "svolLdevId": "ldevId"}, inplace=True)
        
        df01g = pd.concat([df01p, df01s], axis=0)
        df01g = df01g.sort_values(['groupName', 'ldevId'])
        
        if len(svps) == 3:
            df02p = df0[df0['pvolStorageSerial'].str.match(svps[1]['serial'])].copy()
            df02p = df02p[['groupName', 'ldevName', 'pvolStorageSerial', 'pvolLdevId', 'muNumber']]
            df02p.rename(columns={"pvolStorageSerial": "storageSerial", "pvolLdevId": "ldevId"}, inplace=True)
            
            df02s = df0[df0['svolStorageSerial'].str.match(svps[1]['serial'])].copy()
            df02s = df02s[['groupName', 'ldevName', 'svolStorageSerial', 'svolLdevId', 'muNumber']]
            df02s.rename(columns={"svolStorageSerial": "storageSerial", "svolLdevId": "ldevId"}, inplace=True)
            
            df02g = pd.concat([df02p, df02s], axis=0)
            df02g = df02g.sort_values(['groupName', 'ldevId'])
    
    df02 = df0[df0['replicationType'].str.match('UR')]
    
    if not df02.empty:
        df01h = df02[df02['pvolStorageSerial'].str.match(svps[0]['serial'])].copy()
        df01h = df01h[['groupName', 'ldevName', 'pvolStorageSerial', 'pvolLdevId', 'muNumber']]
        df01h.rename(columns={"pvolStorageSerial": "storageSerial", "pvolLdevId": "ldevId"}, inplace=True)
        df01h = df01h.sort_values(['groupName', 'ldevId'])

        if len(svps) == 3:
            df031h = df02[df02['svolStorageSerial'].str.match(svps[2]['serial'])].copy()
            df031h = df031h[['groupName', 'ldevName', 'svolStorageSerial', 'svolLdevId', 'muNumber']]
            df031h.rename(columns={"svolStorageSerial": "storageSerial", "svolLdevId": "ldevId"}, inplace=True)
            df031h = df031h.sort_values(['groupName', 'ldevId'])
        elif len(svps) == 2:
            df031h = df02[df02['svolStorageSerial'].str.match(svps[1]['serial'])].copy()
            df031h = df031h[['groupName', 'ldevName', 'svolStorageSerial', 'svolLdevId', 'muNumber']]
            df031h.rename(columns={"svolStorageSerial": "storageSerial", "svolLdevId": "ldevId"}, inplace=True)
            df031h = df031h.sort_values(['groupName', 'ldevId'])

        
else:
    print("API error: Unable to get replication information from SVP")
    SystemExit(1)

if not df1.empty:
    df1 = df1.sort_values(['replicationType', 'muNumber', 'consistencyGroupId'])
    df1['groupName'] = 'GRP' + df1['consistencyGroupId'].astype(str) + '_' + df1['replicationType'] + df1['muNumber'].astype(str)
    df1['ldevName'] = 'LDEV_' + df1['pvolLdevId'].astype(str)
    df1['muNumber'] = 'h' + df1['muNumber'].astype(str)

    df03 = df1[df1['replicationType'].str.match('UR')]
    
    if not df03.empty:
        df02h = df03[df03['pvolStorageSerial'].str.match(svps[1]['serial'])].copy()
        df02h = df02h[['groupName', 'ldevName', 'pvolStorageSerial', 'pvolLdevId', 'muNumber']]
        df02h.rename(columns={"pvolStorageSerial": "storageSerial", "pvolLdevId": "ldevId"}, inplace=True)
        df02h = df02h.sort_values(['groupName', 'ldevId'])
        
        df032h = df03[df03['svolStorageSerial'].str.match(svps[2]['serial'])].copy()
        df032h = df032h[['groupName', 'ldevName', 'svolStorageSerial', 'svolLdevId', 'muNumber']]
        df032h.rename(columns={"svolStorageSerial": "storageSerial", "svolLdevId": "ldevId"}, inplace=True)
        df032h = df032h.sort_values(['groupName', 'ldevId'])

if df1.empty and len(svps) == 3:
    print("API error: Unable to get replication information from SVP")
    SystemExit(1)

for id, svp in enumerate(svps):
    if id == 0:
        horcm0 = horcm.format(service_port_prefix + str(id), svp['ip'])
        if not df01g.empty:
            horcm0 = horcm0 + '#GAD REPLICATION\n'
            horcm0 = horcm0 + df01g.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df01g['host'] = 'localhost'
            df01g['port'] = service_port_prefix + '1'
            df01gf = df01g[['groupName', 'host', 'port']]
        if not df01h.empty:
            horcm0 = horcm0 + '\n#HUR REPLICATION\n'
            horcm0 = horcm0 + df01h.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df01h['host'] = 'localhost'
            if len(svps) == 2:
                df01h['port'] = service_port_prefix + '1'
            else:
                df01h['port'] = service_port_prefix + '2'
            df01hf = df01h[['groupName', 'host', 'port']]
        horcm0 = horcm0 + '\nHORCM_INST\n'
        horcm0 = horcm0 + '#dev_group      ip_address      service\n'
        if not df01g.empty:
            df01gf = df01gf.drop_duplicates(subset="groupName")
            horcm0 = horcm0 + df01gf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
        if not df01h.empty:
            df01hf = df01hf.drop_duplicates(subset="groupName")
            horcm0 = horcm0 + df01hf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
    elif id == 1:
        horcm1 = horcm.format(service_port_prefix + str(id), svp['ip'])
        if not df02g.empty:
            horcm1 = horcm1 + '#GAD REPLICATION\n'
            horcm1 = horcm1 + df02g.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df02g['host'] = 'localhost'
            df02g['port'] = service_port_prefix + '0'
            df02gf = df02g[['groupName', 'host', 'port']]
        if not df02h.empty:
            horcm1 = horcm1 + '\n#HUR REPLICATION\n'
            horcm1 = horcm1 + df02h.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df02h['host'] = 'localhost'
            df02h['port'] = service_port_prefix + '2'
            df02hf = df02h[['groupName', 'host', 'port']]
        if len(svps) == 2 and not df031h.empty:
            horcm1 = horcm1 + df031h.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df031h['host'] = 'localhost'
            df031h['port'] = service_port_prefix + '0'
            df031hf = df031h[['groupName', 'host', 'port']]
        horcm1 = horcm1 + '\nHORCM_INST\n'
        horcm1 = horcm1 + '#dev_group      ip_address      service\n'
        if not df02g.empty:
            df02gf = df02gf.drop_duplicates(subset="groupName")
            horcm1 = horcm1 + df02gf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
        if not df02h.empty:
            df02hf = df02hf.drop_duplicates(subset="groupName")
            horcm1 = horcm1 + df02hf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
        if len(svps) == 2 and not df031h.empty:
            df031hf = df031hf.drop_duplicates(subset="groupName")
            horcm1 = horcm1 + df031hf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
    elif id == 2:
        horcm2 = horcm.format(service_port_prefix + str(id), svp['ip'])
        if not df031h.empty:
            horcm2 = horcm2 + df031h.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df031h['host'] = 'localhost'
            df031h['port'] = service_port_prefix + '0'
            df031hf = df031h[['groupName', 'host', 'port']]
        if not df032h.empty:
            horcm2 = horcm2 + df032h.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format1], justify='left') + '\n'
            df032h['host'] = 'localhost'
            df032h['port'] = service_port_prefix + '1'
            df032hf = df032h[['groupName', 'host', 'port']]
        horcm2 = horcm2 + '\nHORCM_INST\n'
        horcm2 = horcm2 + '#dev_group      ip_address      service\n'
        if not df01g.empty:
            df031hf = df031hf.drop_duplicates(subset="groupName")
            horcm2 = horcm2 + df031hf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
        if not df01h.empty:
            df032hf = df032hf.drop_duplicates(subset="groupName")
            horcm2 = horcm2 + df032hf.to_string(header=False, index=False, formatters=[(lambda x: fmt.format(x)) for fmt in format2], justify='left') + '\n'
            
            
if 'horcm0' in globals():
    with open("horcm0.conf","w+") as f:
        f.writelines(horcm0)
if 'horcm1' in globals():
    with open("horcm1.conf","w+") as f:
        f.writelines(horcm1)
if 'horcm2' in globals():
    with open("horcm2.conf","w+") as f:
        f.writelines(horcm2)


```