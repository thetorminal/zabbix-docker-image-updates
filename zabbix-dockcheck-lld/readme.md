### Installing
This is the installation readme for zabbix-dockcheck-lld, which shows the updates in Zabbix like this: "Docker update available for container-xyz".

#### On all hosts you want to monitor:  
Manual:  
* Install and configure package zabbix-agent2 (if not installed):  
     ```sh
     apt-get install zabbix-agent2
     ```
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
* add "dockcheck-lld.sh to /etc/zabbix/scripts:
     ```sh
     curl -L https://raw.githubusercontent.com/thetorminal/zabbix-docker-image-updates/refs/heads/main/zabbix-dockcheck-lld/dockcheck-lld.sh -o /etc/zabbix/scripts/dockcheck-lld.sh
     ```
* add "dockcheck.conf" to /etc/zabbix/zabbix_agent2.d/:  
     ```sh
     curl -L https://raw.githubusercontent.com/thetorminal/zabbix-docker-image-updates/refs/heads/main/zabbix-dockcheck-lld/dockcheck-lld.conf -o /etc/zabbix/zabbix_agent2.d/dockcheck.conf
     ```
* restart zabbix-agent2
     ```sh
     systemctl restart zabbix-agent2
     ```

#### On Zabbix frontend server:  
- Download and import the template `docker-image-update-lld.yaml`  
- Assign the `Template Docker Images Updates with LLD'` to the docker host(s) you want to monitor  
