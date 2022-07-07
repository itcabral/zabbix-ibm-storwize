# zabbix-ibm-storwize
Python script for monitoring IBM Storwize storages

Tested on:
- Python 3
- Paramiko 2.11.0
- IBM Storwize v5100

How it works:
- In resume, the script works by connecting to ibm storage, collecting info with svcinfo command and then formating a file for zabbix_sender. 
- A user with monitor access in storage is needed
- More on how zabbix_sender works: https://man.archlinux.org/man/zabbix_sender.1.en

In template "Template EMC Unity REST-API" in section "Macros" need set these macros:
- {$STORWIZE_USER}
- {$STORWIZE_PASSWORD}
- {$STORWIZE_PORT}

In agent configuration file, **/etc/zabbix/zabbix_agentd.conf** must be set parameter **ServerActive=xxx.xxx.xxx.xxx**

- In Linux-console need run this command to make discovery. Script must return value 0 in case of success.
```bash
./storwize_get_state.py  --storwize_ip=xxx.xx.xx.xxx --storwize_port=22 --storwize_user=user_name_of_storagedevice --storwize_password='password' --storage_name="storage_name_in_zabbix-web-interface" --discovery
```

Value of  --storage_name  is macros value {HOST.NAME}


- On zabbix proxy or on zabbix servers need run **zabbix_proxy -R config_cache_reload** (zabbix_server -R config_cache_reload)

- In Linux-console need run this command to get value of metrics. Scripts must return value 0 in case of success.
```bash
./storwize_get_state.py  --storwize_ip=xxx.xx.xx.xxx --storwize_port=22 --storwize_user=user_name_of_storagedevice --storwize_password='password' --storage_name="storage_name_in_zabbix-web-interface" --status
```
If you have executed this script from console from user root or from another user, please check access permission on file **/tmp/storwize_state.log**. It must be allow read, write to user zabbix.

Debug

Error codes

zabbix_sender:
The exit status is 0 if the values were sent and all of them were successfully processed by server.  If data was sent, but processing of at least one of the values failed, the exit status is 2 (check for hostname, who monitor the host or any inactive item).  If data sending failed, the exit status is 1 (check network connection related issues).

