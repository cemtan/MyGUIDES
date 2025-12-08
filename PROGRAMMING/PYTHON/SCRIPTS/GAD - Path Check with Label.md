#### GAD - PATH CHECK WITH LABEL
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
import os
#SMTP Related Modules
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders
# Disable SSL Warnings
from requests.packages.urllib3.exceptions import InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)


# - DEFINITIONS
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
		}]
headers = {"Content-Type": "application/json", "Accept": "application/json"}
# SMTP Settings
port = 25
smtp_server = "mailserver.domain.com"
login = ""
password = ""
sender_email = "pathchecker@hitachivantara.com"
receiver_email = "groupmail@domain.com"
path_warning = 31
path_error = 32



# - FUNCTIONS
def api_get(f_url, f_headers, f_title):
	try:
		if "ldevs" in f_url:
			ask = {'count': '2000', 'ldevOption': 'luMapped'}
			f_response = requests.get(url=f_url, headers=f_headers, verify=False, timeout=300, params=ask)
		else:
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
	f_dg = pd.DataFrame()
	url = "https://{}/ConfigurationManager/v1/objects/storages/".format(f_svp['ip'])
	auth = get_auth(f_svp['user'], f_svp['password'])
	headers['Authorization'] = 'Basic {}'.format(auth)
	response = api_get(url, headers, 'Finding storageDeviceId of ' + f_svp['serial']).json()
	storageDeviceId = response['data'][0]['storageDeviceId']
	url = "https://{}/ConfigurationManager/v1/objects/storages/{}/sessions".format(f_svp['ip'], storageDeviceId)
	response = api_post(url, headers, 'Getting token for ' + f_svp['serial']).json()
	if(len(response) > 0):
		token = response['token']
		headers['Authorization'] = 'Session {}'.format(token)
		url = "https://{}/ConfigurationManager/v1/objects/storages/{}/remote-replications".format(f_svp['ip'], storageDeviceId)
		response = api_get(url, headers, 'Getting replcation information from ' + f_svp['serial']).json()
		f_df = pd.json_normalize(response['data'])
		url = "https://{}/ConfigurationManager/v1/objects/storages/{}/ldevs".format(f_svp['ip'], storageDeviceId)
		response = api_get(url, headers, 'Getting LDEV information from ' + f_svp['serial']).json()
		f_dg = pd.json_normalize(response['data'])
		return f_df, f_dg
	else:
		print("Token error: Returned token is empty for " + f_svp['serial'])


def send_email(e_type):
	message = MIMEMultipart()
	message["From"] = sender_email
	message["To"] = receiver_email
	if e_type == "warning":
		subject = "Path Warning: Some disks already have {} or more paths.".format(path_warning)
		body = "Hi,\n\nThis is an email from path checker. Some GAD disks already have {} or more paths. Exceeding 32 paths may cause a problem.\nPlease check the attachment.\n\nBest Regards,\nHitachi Vantara".format(path_warning)
		filename = "Path_Warning.csv"
	else:
		subject = "Critical Path Issue: Path number of some disks exceeded {} path limit.".format(path_error)
		body = "Hi,\n\nThis is an email from path checker. Some GAD disks have more than {} paths. Exceeding 32 paths will cause a problem.\nPlease check the attachment.\n\nBest Regards,\nHitachi Vantara".format(path_error)
		filename = "Path_Error.csv"
	message = MIMEMultipart()
	message["From"] = sender_email
	message["To"] = receiver_email
	message["Subject"] = subject
	message.attach(MIMEText(body, "plain"))
	with open(filename, "rb") as attachment:
		part = MIMEBase("application", "octet-stream")
		part.set_payload(attachment.read())
	encoders.encode_base64(part)
	part.add_header("Content-Disposition", f"attachment; filename= {filename}")
	message.attach(part)
	with smtplib.SMTP(smtp_server, port) as server:
		if port != 25:
			server.starttls()
		if login != "" and password != "":
			server.login(login, password)
		server.sendmail(sender_email, receiver_email, message.as_string())
	print("Sent an email for path " + e_type)

# - MAIN CODE
df1 = pd.DataFrame()
df2 = pd.DataFrame()
dg1 = pd.DataFrame()
dg2 = pd.DataFrame()

try:
	os.remove('Path_Warning.csv')
	os.remove('Path_Error.csv')
except OSError:
	pass

df1, dg1 = get_replication(svps[0])
df2, dg2 = get_replication(svps[1])


if not df1.empty and not dg1.empty and not df2.empty and not dg2.empty:
	df11 = df1[df1['replicationType'].str.match('GAD')]
	df21 = df2[df2['replicationType'].str.match('GAD')]
	df12 = df11'pvolStorageSerial', 'pvolLdevId', 'svolStorageSerial', 'svolLdevId'
	df22 = df21'pvolStorageSerial', 'pvolLdevId', 'svolStorageSerial', 'svolLdevId'
	dg11 = dg1'ldevId', 'numOfPorts', 'label'
	dg21 = dg2'ldevId', 'numOfPorts', 'label'
	dg11.columns = ['pvolLdevId', 'pvolNumOfPorts', 'pvolLabel']
	dg21.columns = ['svolLdevId', 'svolNumOfPorts', 'svolLabel']
    
    
	df51 = df12[df12['pvolStorageSerial'].str.match(svps[0]['serial'])]
	df61 = df12[df12['pvolStorageSerial'].str.match(svps[1]['serial'])]


	dh11 = df51.merge(dg11, how = 'left', on = ['pvolLdevId'])
	dh12 = dh11.merge(dg21, how = 'left', on = ['svolLdevId'])

	dg11.columns = ['svolLdevId', 'svolNumOfPorts', 'svolLabel']
	dg21.columns = ['pvolLdevId', 'pvolNumOfPorts', 'pvolLabel']
	dh21 = df61.merge(dg21, how = 'left', on = ['pvolLdevId'])
	dh22 = dh21.merge(dg11, how = 'left', on = ['svolLdevId'])
    
	dc = pd.concat([dh12, dh22])
	dc['pvolNumOfPorts'] = dc['pvolNumOfPorts'].fillna(0)
	dc['svolNumOfPorts'] = dc['svolNumOfPorts'].fillna(0)
	dc['pvolNumOfPorts'] = dc['pvolNumOfPorts'].astype(int)
	dc['svolNumOfPorts'] = dc['svolNumOfPorts'].astype(int)
	dc['totalNumOfPorts'] = dc['pvolNumOfPorts'] + dc['svolNumOfPorts']
    
	dw = dc[dc['totalNumOfPorts'] >= path_warning ]
	de = dc[dc['totalNumOfPorts'] > path_error ]
    
	if not dw.empty:
		dw.to_csv('Path_Warning.csv', index=False)
		send_email('warning')
		print("Path check ended with warning.")

	if not de.empty:
		dw.to_csv('Path_Error.csv', index=False)
		send_email('error')
		print("Path check ended with critical error.")
        
	print("Path check completed successfuly")
else:
	print("API error: Unable to get replication information from SVP")
	SystemExit(1)

```