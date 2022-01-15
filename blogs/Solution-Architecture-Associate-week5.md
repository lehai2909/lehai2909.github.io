# Nhật ký tự học Solution Architecture Associate AWS - Week 1 (1/11/2021 - 7/11/2021)

![SAA-badge](../images/SAA/AWS-Certified_Solutions-Architect_Associate_badge.png)

## Những thứ mình đã học được trong một tuần qua:


### Resilient Storage: Data Backup, Recovery and Encryption

### S3

Resiliency:
+ multi AZ / Cross-region replication
+ S3 versioning
+ S3 ObjectLock (WORM)
+ Backup encryption


### Elastic File System (EFS)
Multi AZ file system
use hybrid mode - non-cloud native app
resilience:
+ Backup to S3
+ Save to another EFS

### Resilient Database 

#### RDS
+ using Multi AZ deployment (primary and standby instance - !not a scaling option)
+ read replicas (off load read request from primary instance)
+ Automated backup

 *Aurora*: using DB cluster with many replicas instance (up to 15 + primary instance)

 ### DynamoDB

+ multi AZ deployment
+ Point in time recovery provide automatic backup : up to 35 until current point


