# Resilience in Amazon FSx for OpenZFS<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS global infrastructure, Amazon FSx offers several features to help support your data resiliency and backup needs\.

## Backup and restore<a name="fsx-resilience-backups"></a>

Amazon FSx creates and saves automated backups of the volumes in your Amazon FSx for OpenZFS file system\. Amazon FSx creates automated backups of your volumes during the backup window of your Amazon FSx for OpenZFS file system\. Amazon FSx saves the automated backups of your volumes according to the backup retention period that you specify\. You can also back up your volumes manually, by creating a user\-initiated backup\. You restore a volume backup at any time by creating a new volume with the backup specified as the source\.

For more information, see TBD [Working with backups](using-backups.md)\.

## Snapshots<a name="resiliency-snapshots"></a>

 Amazon FSx provides the ability to take snapshots of volumes within your file systems\. A snapshot is a read\-only image of your OpenZFS volume at a point in time, offering protection against accidental deletion or modification of files in your volumes by end users\. For more information, see [Working with FSx for OpenZFS snapshots](snapshots-openzfs.md)\.