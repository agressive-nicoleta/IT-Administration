# SLA - Service Level Agreement

### BACKUP COVERAGE

Three services are backed up: database servers MySQL, InfluxDB data, and Ansible repository, which contains all configurations for the IT Infrastructure services.

### RPO - RECOVERY POINT OBJECTIVE

Backups are created every day around midnight. Only 24h of data loss can be accepted since the last backup.

### VERSIONING AND RETENTION

Backups are held for a maximum of 30 days.
Incremental backuping is used, which means that every day a different version is stored (from Monday to Saturday)
Total versions: 30
Full backups are created once a week, on Sunday.
Backups that are older than 37 days are deleted.

### USABILITY CHECKS

On the first day of the week, the data that was backed up the week before is checked. The infrastructure is rebuilt using Ansible repository.

### RESTORATION CRITERIA

If within 4h the infrastructure does not respond to any of the fixing approaches and remains unavailable, everything is restored from the latest backup.

### RECOVERY TIME OBJECTIVE

To restore the services it will take from 3 to 5 hours.
