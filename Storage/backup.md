# AWS Backup

## General

- Fully managed service.
- Centralizes backup automation and managements across different AWS Services such as:
  - EC2 / EBS / EFS / FSx
  - S3
  - RDS / DocumentDB / Amazon Neptune
  - Storage Gateway
- Supports on-demand and scheduled backups.
- Supports cross-account and cross-region backups.
- Supports point-in-time recovery for supported services such as Amazon Aurora.
- Backup automation can be tag-based.

## Backup Plans

Backup plans are backup policies and basically says what the backup automation will do:

- **Frequency**: cron expression of when the backup should be made.
- **Backup Window**: what time range is more appropriate to run the backup.
- **Cold Storage Transition**: sets when it becomes safe to transition the backup to a lower cost/lower resilience storage (ex. never, days, years, etc.)
- **Retention Period**: defines how long it will be required to keep the backup (ex. always, days, months, etc.)

AWS Backup -> Backup Plan for an AWS Service -> Backup automatically saved to an S3 Bucket.

## Backup Vault Lock

- Enforces *write once ready many* (WORM) for all the backups stored in the Vault and serves as an additional layer of protection.
- **Not even the root user can delete the backup** while it is in the Backup Vault Lock.