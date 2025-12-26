### SAN - CFGSHOW to COMMANDS
---
---


```python
#!/usr/bin/python3
# coding:utf-8

# COPYRIGHT
# © 2022 Hitachi Vantara LLC. All rights reserved.


import sys, re

def writeCmd(cmd, name, items):
    global cfgCmd, zoneCmd, aliasCmd, neAlias, neAliasCmd
    if cmd == 'alicreate':
        wwnList =  ''
        if wwnPresent:
            for item in items:
                if not re.search(item, wwn):
                    wwnList = wwnList + ' ' + item.strip()
        if wwnList:
            neAlias = neAlias + name + " (" + wwnList.strip() + ")\n"
            neAliasCmd = neAliasCmd + "alidelete '" + name + "'\n"
        else:                
            aliasCmd = aliasCmd + cmd + " '" + name + "', '" + '; '.join(items)  + "'\n"
    elif cmd == 'zonecreate':
        zoneCmd = zoneCmd + cmd + " '" + name + "', '" + '; '.join(items)  + "'\n"
    elif cmd == 'cfgenable':
        cfgCmd = cfgCmd + cmd + " '" + name + "'\n"
    else:
        if items:
            cfgCmd = cfgCmd + cmd + " '" + name +  "', '" + items[0] + "'\n"
            cfgCmd = cfgCmd + "cfgsave\n"
            for i in items[1:]:
                cfgCmd = cfgCmd + "cfgadd '" + name +  "', '" + i + "'\n"
            cfgCmd = cfgCmd + "cfgsave\n"
           
def finalize():
    zoneChkCmd = ''
    neZone = ''
    neZoneCmd = ''
    neCfgCmd = ''
    for zLine in zoneCmd.splitlines():
        aliasList = ''
        zAttrs = zLine.split("'")
        zItems = zAttrs[3].split(';')
        for zItem in zItems:
            chkItem = "'" + zItem.strip() + "'"
            if not re.search(chkItem, aliasCmd):
                if re.search(zItem.strip(), neAlias):
                    aliasList = aliasList + ' ' + zItem.strip()
                else:
                    aliasList = aliasList + ' ' + zItem.strip() + '(N/A)'
        if aliasList:
            neZone = neZone + zAttrs[1] + " (" + aliasList.strip() + ")\n"
            neZoneCmd = neZoneCmd + "zonedelete '" + zAttrs[1] + "'\n"
        else:
            zoneChkCmd = zoneChkCmd + zLine + '\n'
    
    cfgChkCmd = ''
    for zLine in cfgCmd.splitlines():
        zAttrs = zLine.split("'")
        if len(zAttrs) > 3:
            chkItem = "'" + zAttrs[3].strip() + "'"
            if not re.search(chkItem, zoneChkCmd):
                neCfgCmd = neCfgCmd + "cfgremove '" + zAttrs[1] + "', " + chkItem + '\n'
            else:
                cfgChkCmd = cfgChkCmd + zLine + '\n'    
        else:
            cfgChkCmd = cfgChkCmd + zLine + '\n' 
            
    if not re.search('cfgcreate', cfgChkCmd):
        cfgChkCmd = re.sub('cfgsave\n','',cfgChkCmd,1)
        cfgChkCmd = cfgChkCmd.replace('cfgadd', 'cfgcreate', 1)
        cfgChkCmd = cfgChkCmd.replace('cfgadd', 'cfgsave\ncfgadd', 1)
    
    if cfgChkCmd:
        cfgChkCmd = cfgChkCmd + 'cfgsave'
    
    outFile = cfgFile.split('.')[0] + '.out'
    f = open(outFile, 'w')
    f.write(aliasCmd)
    f.write('\n')
    f.write(zoneChkCmd)
    f.write('\n')
    f.write(cfgChkCmd)
    f.write('\n')
    print(outFile + ' is generated.')
    f.close()    

    outFile = cfgFile.split('.')[0] + 'aliasremove.out'
    f = open(outFile, 'w')
    f.write('EXCLUDED ALIASES\n-----------------------------------\n')
    f.write(neAlias)
    f.write('\n')
    f.write(neAliasCmd)
    f.write('\n')
    print(outFile + ' is generated.')
    f.close()    

    outFile = cfgFile.split('.')[0] + 'zoneremove.out'
    f = open(outFile, 'w')
    f.write('EXCLUDED ZONES\n-----------------------------------\n')
    f.write(neZone)
    f.write('\n')
    f.write(neCfgCmd)
    f.write('\n')
    f.write(neZoneCmd)
    f.write('\n')
    print(outFile + ' is generated.')
    f.close()    

sys.argv.pop(0)
print('Files to be processed:' + str(sys.argv))


for cfgFile in sys.argv:
    effective = False
    cmdOut = ''
    aliasCmd = ''
    neAlias = ''
    neAliasCmd = ''
    zoneCmd = ''
    cfgCmd = ''
    items = []
    try:
        f = open(cfgFile, 'r', encoding='utf-8-sig')
        cfg = f.read()
        cfg = re.sub(r'\n+', '\n', cfg).strip('\n')
        f.close()
        try:
           wwnFile = cfgFile.split('.')[0] + '.wwn'
           f = open(wwnFile, 'r', encoding='utf-8-sig')
           wwn = f.read()
           wwn = wwn.strip()
           f.close()
           wwnPresent = True
        except OSError:
           wwnPresent = False
        for line in cfg.splitlines():
            pLine = line.strip()
            attrs = pLine.split()
            if attrs[0] == 'cfg:' or attrs[0] == 'zone:' or attrs[0] == 'alias:':
                if effective:
                    writeCmd('cfgenable', attrs[1], items)
                    finalize()
                    break
                if items:
                    writeCmd(command, name, items) 
                name = attrs[1]
                if attrs[0] == 'cfg:':
                    command = 'cfgcreate'
                elif attrs[0] == 'zone:':
                    command = 'zonecreate'
                elif attrs[0] == 'alias:':
                    command = 'alicreate'
                items = []
                fitems = attrs[2:]
                newAttrs = [w.replace(';', '') for w in fitems]
                items = items + newAttrs
            elif attrs[0] == 'Effective':
                writeCmd(command, name, items)
                effective = True
                items = []
            elif attrs[0] == 'Defined':
                None
            else:
                newAttrs = [w.replace(';', '') for w in attrs]
                items = items + newAttrs
    except OSError:
        print('Could not open/read file:' + cfgFile)
```