#### CLEAR SNAP GARBAGE
---
---


```python
#!/usr/bin/python3
# coding:utf-8

# COPYRIGHT
# © 2022 Hitachi Vantara LLC. All rights reserved.

# Usage   python3 snap_garbage.py 865
# 865 is the instance number which shows horcm865.conf file under /etc

import re
import subprocess
import argparse

def extract_ldevs_with_snap_garbage(file_path):
	selected_ldevs = []

	with open(file_path, 'r') as file:
		content = file.read()

	# Split the content into individual LDEV sections
	ldev_sections = content.split("Serial#")

	for section in ldev_sections:
		if "SNAP_GARBAGE(MB)" in section:
			# Extract LDEV number
			ldev_match = re.search(r"LDEV\s+:\s+(\d+)", section)
			# Extract snap_garbage value
			snap_garbage_match = re.search(r"SNAP_GARBAGE\(MB\)\s+:\s+(\d+)", section)

			if ldev_match and snap_garbage_match:
				ldev = ldev_match.group(1)
				snap_garbage = int(snap_garbage_match.group(1))

				if snap_garbage > 0:
					selected_ldevs.append((ldev, snap_garbage))

	return selected_ldevs

def run_raidcom_command(command,instance):
	try:
		# Execute the command
		result = subprocess.run(
			command,
			shell=True,  # Enables shell features
			capture_output=True,  # Captures stdout and stderr
			text=True  # Ensures the output is a string
		)
        
		# Check for errors
		if result.returncode == 0:
			print('raidcom Command executed successfully! for instance '+ instance)
            
		else:
			print("Error executing command:")
			print("Error Output:\n", result.stderr)
	except Exception as e:
		print("Exception occurred:", str(e))

def run_raidcom_garbage(command):
	try:
		# Execute the command
		result = subprocess.run(
			command,
			shell=True,  # Enables shell features
			capture_output=True,  # Captures stdout and stderr
			text=True  # Ensures the output is a string
		)
        
		# Check for errors
		if result.returncode == 0:
			print(command)
            
		else:
			print(command)
			print("Error executing command:")
			print("Error Output:\n", result.stderr)
	except Exception as e:
		print("Exception occurred:", str(e))

def main(instance_no):
# Example RAIDCOM command
	raidcom_command =  "raidcom get ldev -ldev_list mapped -I"+instance_no+" > ldev_list_"+instance_no+".txt"

# Run the command
	run_raidcom_command(raidcom_command,instance_no)


	file_path = "ldev_list_"+instance_no+".txt" 
	ldevs_with_snap_garbage = extract_ldevs_with_snap_garbage(file_path)

	for ldev, snap_garbage in ldevs_with_snap_garbage:
		 print('Deleting '+  f"LDEV: {ldev}, SNAP_GARBAGE(MB): {snap_garbage}")
		 raidcom_command1 = "raidcom modify snapshot  -ldev_id " + ldev + " -snapshot_data delete_garbage -I" + instance_no
		 run_raidcom_garbage(raidcom_command1)
 
    

if name == "main":
	# Set up argument parser
	parser = argparse.ArgumentParser(description="Running command for Instance")
	parser.add_argument('instance_no', type=str)
    
	# Parse the arguments
	args = parser.parse_args()
    
	# Call the main function with the file path
	main(args.instance_no)
    
    

```
