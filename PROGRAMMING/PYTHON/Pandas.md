#### PANDAS
---
---



	>>> import pandas as pd
	>>> df = pd.read_csv("test.csv")
	>>> df["date"] = pd.to_datetime(df["date"])
	df["date"] = df["date"] - pd.Timedelta(hours=3)
	