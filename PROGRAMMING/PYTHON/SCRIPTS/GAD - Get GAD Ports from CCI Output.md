#### GAD - GET GAD PORTS FROM CCI OUTPUT
---
---


```python
#!/usr/bin/python3
# coding:utf-8

# COPYRIGHT
# © 2022 Hitachi Vantara LLC. All rights reserved.

#pip install pandas openpyxl    if not already installed.
import pandas as pd

def convert_to_excel(file_path):
	# Initialize a list to store data for each row
	data = []

	# Initialize a dictionary to store current data block
	current_data = {}

	file = open(file_path, 'r')
	file = file.read().replace(' VIR_LDEV', '\nVIR_LDEV')
	# Open the file in read mode
	for line in file.splitlines():
		line = line.strip()  # Remove any leading/trailing whitespace
        
		# If a new 'Serial#' starts, store the previous data block and reset
		if line.startswith("Serial#"):
			if current_data:  # If current_data is not empty, store it
				data.append(current_data)
				current_data = {}  # Reset for the next block
			current_data['Serial#'] = line.split(":")[1].strip()
        
		else:
			# For any other field, add it to the current data block
			if ':' in line:
				key, value = line.split(":", 1)
				current_data[key.strip()] = value.strip()

	# Append the last data block if exists
	if current_data:
		data.append(current_data)

	# Create a DataFrame from the collected data
	df = pd.DataFrame(data)
	df = df[df['VOL_ATTR'].str.contains(": GAD")].iloc[:, [2, 7]].sort_values(by=['VIR_LDEV'])
	df['NUM_PORT'] = df['NUM_PORT'].astype(int)
	df = df.groupby('VIR_LDEV').sum()
	df.reset_index(inplace=True)


	# Write the DataFrame to an Excel file
	df.to_excel('ldev_info.xlsx', index=False)
	print("Data has been successfully written to 'ldev_info.xlsx'.")

# Replace 'ldev_list.txt' with the actual file path if needed
convert_to_excel('ldev_list_gad.txt')

```