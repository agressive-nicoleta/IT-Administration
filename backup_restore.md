# The process of backup restoration: MySQL and InfluxDB

### Install and configure Ansible infrastructure on the local machine:

    ansible-playbook infra.yaml

## Restore MySQL data

### Log into agama host machine (agressive-nicoleta-1)

    ssh -p <port_number> ubuntu@<ip_address>

Example: ssh -p 6922 ubuntu@193.40.156.67

### Execute following commands on this machine to restore mysql data

1.  As root clean directory /home/backup/restore

        sudo su
        rm /home/backup/restore/\*
        exit

2.  Login in as backup user

        sudo su backup

3.  Download the backup to the managed host

        duplicity --no-encryption restore rsync://agressive-nicoleta@backup.nicole.ptr//home/agressive-nicoleta/mysql /home/backup/restore/mysql

4.  Exit backup user and login as root user

        exit
        sudo su

5.  Restore the backup for the mysql service

        mysql agama < /home/backup/restore/mysql/agama.sql

6.  Open agama app and look if data is restored

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

        sudo su backup

4.  Download the backup to the managed host

        duplicity --no-encryption restore rsync://agressive-nicoleta@backup.nicole.ptr//home/agressive-nicoleta/influxdb /home/backup/restore/influxdb

5.  Put restored dump into the telegraf database

        influxd restore -portable -database telegraf /home/backup/restore/influxdb

6.  Restart telegraf service my running Ansible playbook on the local machine:

        ansible-playbook infra.yaml

    **Or** restart manually on the host machine by running:

        sudo systemctl start influxdb

7.  To check if data was restored, access Syslog dashboard in Grafana
