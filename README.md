# Zabbix docker image update monitoring
Monitoring of docker image updates with Zabbix.  

## Description
Based on “dockcheck” (CLI query to check updates): [Github - dockcheck](https://github.com/mag37/dockcheck/tree/main)  

Shows number of available docker image updates on host (Example: "2 Docker Image updates on host-xyz").  
Important: The Docker image version must be specified with :latest (e.g. in a docker-compose file), a fixed version number such as mariadb:10.6 does not work.  

Example notification:  
![docker_image_zabbix](https://github.com/user-attachments/assets/be2d897b-7fc6-40dc-ac12-19c833a6f0ef)

Tested with:  
* Zabbix Server 7.0.5
* zabbix-agent2 (on Debian 12 server)

## Getting Started

### Dependencies
* [dockcheck.sh](https://github.com/mag37/dockcheck/blob/main/dockcheck.sh)
* [regclient/regctl](https://github.com/regclient/regclient) (Licensed under Apache-2.0 License)
* Zabbix-Server with timeouts set up to 30 seconds! (Timeout=30)
* Host with zabbix-agent2 and docker installed

### Installing

#### On Zabbix frontend server:  
- Download and import the template `docker-image-update.yaml`  
- Assign the `Template Docker Image Updates'` to the docker host(s) you want to monitor  

#### On all hosts you want to monitor:  
Manual:  
* Install and configure package zabbix-agent2 (if not installed):  
     ```sh
     apt-get install zabbix-agent2  
* download "dockcheck.sh" from dockcheck repository to new directory `/etc/zabbix/scripts/` and change permission:  
     ```sh
     mkdir /etc/zabbix/scripts
     curl -L https://raw.githubusercontent.com/mag37/dockcheck/main/dockcheck.sh -o /etc/zabbix/scripts/dockcheck.sh
     chown zabbix:zabbix /etc/zabbix/scripts/dockcheck.sh && chmod 0755 /etc/zabbix/scripts/dockcheck.sh
     ```
* run "dockcheck.sh" to install regctl:  
     ```sh
     bash /etc/zabbix/scripts/dockcheck.sh -n
     # Confirm with "y":   
     Required dependency 'regctl' missing, do you want it downloaded? y/[n] y  
     chown zabbix:zabbix dockcheck.sh && chmod 0755 /etc/zabbix/scripts/regctl
     ```
* add "dockcheck.conf" to /etc/zabbix/zabbix_agent2.d/:  
     ```sh
     curl -L https://raw.githubusercontent.com/thetorminal/zabbix-docker-image-updates/refs/heads/main/dockcheck.conf -o /etc/zabbix/zabbix_agent2.d/dockcheck.conf
     ```
* restart zabbix-agent2
     ```sh
     systemctl restart zabbix-agent2
     ```

via Ansible playbook:  
* run the playbook `zabbix-dockcheck.yml` on host(s) you want to monitor docker on.  

## Version History
* 0.1
    * Initial Release

## To-Do
* List name of image, which needs an update (using Zabbix LLD)

## Acknowledgments
* [Github - dockcheck](https://github.com/mag37/dockcheck/tree/main)
