# The process of backup restoration: MySQL and InfluxDB

### Install and configure Ansible infrastructure on the local machine:

    ansible-playbook infra.yaml

## Restore MySQL data

### Log into mysql host machine (agressive-nicoleta-2)

    ssh -p <port_number> ubuntu@<ip_address>

Example: ssh -p 6922 ubuntu@193.40.156.67

### Execute following commands on this machine to restore mysql data

1.  Login in as backup user

        sudo su - backup

2.  Download the backup to the managed host

        duplicity --no-encryption restore rsync://agressive-nicoleta@backup.nicole.ptr//home/agressive-nicoleta/mysql /home/backup/restore/mysql

3.  Exit backup user and login as root user

        exit
        sudo su

4.  Restore the backup for the mysql service

        mysql agama < /home/backup/restore/mysql/agama.sql

5.  Open agama app and look if data is restored

## Restore InfluxDB data

### ### Log into the host machine with influxdb (agressive-nicoleta-1)

    ssh -p <port_number> ubuntu@<ip_address>

Example: ssh -p 6922 ubuntu@193.40.156.67

### Execute following commands on this machine to restore mysql data

1.  As root user stop the telegram service

        sudo su
        service telegraf stop

2.  Delete existing telegraf

        influx -execute 'DROP DATABASE telegraf'

3.  Login as backup user

        sudo su - backup

4.  Download the backup to the managed host

        duplicity --no-encryption restore rsync://agressive-nicoleta@backup.nicole.ptr//home/agressive-nicoleta/influxdb/ /home/backup/restore/influxdb/

5.  Put restored dump into the telegraf database

        influxd restore -portable -database telegraf /home/backup/restore/influxdb

6.  Restart telegraf service my running Ansible playbook on the local machine:

        ansible-playbook infra.yaml

7.  To check if data was restored, access Syslog dashboard in Grafana
