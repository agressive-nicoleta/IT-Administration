# The process of MySQL backup restore

### Install and configure infrastructure with Ansible on the local machine:

    ```
    ansible-playbook infra.yaml
    ```

## Restore MySQL data from the backup:

### Log into agama host machine (agressive-nicoleta-1)

    ```
    ssh -p <port_number> ubuntu@<ip_address>
    ```
    Example: ssh -p 6922  ubuntu@193.40.156.67

### Execute following commands on this machine to restore mysql data

    1. As root clean directory /home/backup/restore
    ```
    sudo su
    rm /home/backup/restore/*
    exit
    ```

    2. Login in as backup user
    ```
    sudo su backup
    ```

    3. Download the backup to the managed host
    ```
    duplicity --no-encryption restore rsync://agressive-nicoleta@backup.nicole.ptr//home/agressive-nicoleta/mysql /home/backup/restore/mysql
    ```

    4. Exit backup user and login as root user
    ```
    exit
    sudo su
    ```

    5. Restore the backup for the mysql service
    ```
    mysql agama < /home/backup/restore/mysql/agama.sql
    ```

    6. Open agama app and check if data is restored

<add a few words here how the result of backup restore can be checked>
