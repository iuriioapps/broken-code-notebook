# Disaster recovery

* **RTO - Recovery Time Objective** - is the time it takes after the disruption to restore a business process to its service level.
* **RPO - Recovery Point Objective** - is the acceptable amount of data loss measured in time before the disaster occurs.

### Disaster recovery strategies

* **Backup and restore** - storing backup data on S3 and recover data quickly and reliably.
* **Pilot light** - for quick recovery into AWS - quicker recovery times than backup and restore, because core pieces of the system are already running and are continually kept up-to-date
* **Warm standby** - a scaled-down version of a fully functional environment is always running in the cloud.
* **Multi-site** - run your infrastructure on another site, in an active-active configuration.



