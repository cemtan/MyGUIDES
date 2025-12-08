#### PORT LOGIN INFO
---
---


```python
#!/usr/bin/python3
# coding:utf-8

# COPYRIGHT
# © 2022 Hitachi Vantara LLC. All rights reserved.


# To Get login list#
# Get token from SVP
#  curl --insecure -v -H "Accept:application/json" -H "Content-Type:application/json" -u opsadmin:Opscenter123 -X POST https://10.97.238.39/ConfigurationManager/v1/objects/storages/900000041003/sessions/ -d ""

# use returned token 
#curl --insecure -v -H "Accept:application/json" -X GET https://10.97.238.39/ConfigurationManager/v1/objects/ports?detailInfoType=logins -H "Authorization:Session bb03eb1e-ad0b-49df-8f19-5f145df4ee6f" >port_login_41003.json

#script usage
# Usage python port_login_v3.py  xxxxx.json (from output of upper command)
# python port_login_v3.py port_login_41003.json


import json
import argparse
import pandas as pd

# Load the JSON file


def convert_to_excel(file_path):

	with open(file_path, 'r') as file:
		data = json.load(file)
	
	# Initialize a list to hold login details
	login_data = []
	
	# Iterate through the data and extract required fields
	for port in data["data"]:
		for login in port.get("logins", []):
			login_data.append({
				"Port": port.get("portId"),
				"loginWwn": login.get("loginWwn"),
				"wwnNickName": login.get("wwnNickName", "-"),
				"hostGroupId": login.get("hostGroupId", "-"),
				"isLoggedIn": login.get("isLoggedIn", False)
			})
	
	# Create a DataFrame
	df = pd.DataFrame(login_data)
	
	# Save to Excel
	
	file_path1=file_path.replace('.json', '.xlsx')
	df.to_excel((file_path1), index=False)
	print("Data has been successfully written to " + file_path1)

def main(file_path):
	convert_to_excel(file_path)

if name == "main":
	# Set up argument parser
	parser = argparse.ArgumentParser(description="Convert a text file to Excel.")
	parser.add_argument('file_path', type=str, help='The path to the text file to be converted to Excel')
	
	# Parse the arguments
	args = parser.parse_args()
	
	# Call the main function with the file path
	main(args.file_path)

```