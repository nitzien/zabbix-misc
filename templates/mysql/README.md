# Introduction

Most of available mysql monitoring template uses one user parameter per item. Therefore to collect values for 10 items, they will run 10 mysql command (or same command 10 times and get extract desired information from result). This monitoring template instead get complete result in one item as json content and then uses dependent items to create multiple items on zabbix server. 

# Installation

1. Zabbix client (mysql server) host must have jq installed. If not, please install using 
``` 
sudo apt install jq 
```
2. Add following line in zabbix client configuration. 
```
UserParameter=Mysql.Server-Status, mysql --defaults-file=/etc/zabbix/.my.cnf --defaults-group-suffix=_monitoring -N -e  "show global status" |   jq  -c '.  | split("\n")[:-1]  | map (split("\t") | {(.[0]) : .[1]}  ) | add  ' -R -s
``` 
Mysql credentials are stored in /etc/zabbix/.my.cnf for me. Make sure zabbix user has read access to mysql password file
3. Import template into zabbix
4. Apply template to any host

# Template Contents

#### Items

![Mysql Items List](../../readme_images_branch/readme-images/mysql-items.png)


#### Triggers

![Mysql Triggers List](../../readme_images_branch/readme-images/mysql-trigger.png)

#### Graph

![Mysql Graph](../../readme_images_branch/readme-images/mysql-graph.png)
