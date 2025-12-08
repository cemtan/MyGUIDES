#### INFLUXDB
---
---



INSTALL GUIDE: https://www.devopsroles.com/install-influxdb-on-rhel-centos-7/
INFLUXDB DOWNLOAD: https://www.influxdata.com/downloads/
INFLUXDB STUDIO DOWNLOAD: https://github.com/CymaticLabs/InfluxDBStudio/releases

```bash
sudo systemctl start influxdb
sudo systemctl status influxdb

sudo firewall-cmd --add-port=8086/tcp --permanent
sudo firewall-cmd --reload

sudo vi /etc/influxdb/influxdb.conf
```

Add the content below:

	[http]
	auth-enabled = true

```bash
sudo systemctl restart influxdb

curl -XPOST "http://localhost:8086/query" --data-urlencode "q=CREATE USER user WITH PASSWORD 'password' WITH ALL PRIVILEGES"
influx -username 'user' -password 'password'
curl -G http://localhost:8086/query -u username:password --data-urlencode "q=SHOW DATABASES"
curl -XPOST "http://localhost:8086/query" -u username:password --data-urlencode "q=CREATE DATABASE hitachi"
curl -XPOST "http://localhost:8086/query" -u username:password --data-urlencode "q=GRANT ALL PRIVILEGES ON hitachi TO user"
sudo netstat -nplt | grep 8086
```
