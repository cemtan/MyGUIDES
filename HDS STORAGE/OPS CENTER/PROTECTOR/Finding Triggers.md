#### FINDING TRIGGERS
---
---



##### Login
---
→ login.json
null
```bash
curl -s -X POST -k -c mysession -o login.json -d "username=snpuser&password=snpuser123&space=master" https://10.10.20.137:20964/HDID/master/UIController/services/User/actions/login/invoke
```

##### Nodes
---
curl --insecure --cookie mysession  https://10.10.20.137:20964/API/7.1/master/NodeManager/objects/Nodes

##### DataFlows
---
curl --insecure --cookie mysession  https://10.10.20.137:20964/API/7.1/master/NodeManager/objects/DataFlows

##### Triggers
---
curl --insecure --cookie mysession  https://10.10.20.137:20964/API/7.1/master/TriggerHandler/objects/Triggers
