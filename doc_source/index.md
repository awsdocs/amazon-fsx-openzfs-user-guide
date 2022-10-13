# FSx for OpenZFS OpenZFS User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in
connection with any product or service that is not Amazon's,
in any manner that is likely to cause confusion among customers,
or in any manner that disparages or discredits Amazon. All other
trademarks not owned by Amazon are the property of their respective
owners, who may or may not be affiliated with, connected to, or
sponsored by Amazon.

-----
## Contents
+ [What is Amazon FSx for OpenZFS?](what-is-fsx.md)
+ [Setting up Amazon FSx for OpenZFS](setting-up.md)
+ [Getting started with Amazon FSx for OpenZFS](getting-started.md)
   + [Step 1: Create an Amazon FSx for OpenZFS file system](getting-started-step1.md)
   + [Step 2: Mounting your file system from an Amazon EC2 Linux instance](getting-started-step2.md)
   + [Step 3: Clean up resources](getting-started-step3.md)
+ [Accessing data: supported clients and environments](supported-fsx-clients.md)
   + [Accessing data on FSx for OpenZFS volumes](access-environments.md)
   + [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)
      + [Mounting FSx for OpenZFS volumes to Linux clients](attach-linux-client.md)
      + [Mounting OpenZFS volumes to Microsoft Windows clients](attach-windows-client.md)
      + [Mounting FSx for OpenZFS volumes to macOS clients](attach-mac-client.md)
      + [Additional mounting considerations](other-mount-considerations.md)
   + [Using FSx for OpenZFS with AWS container services](openzfs-integrations.md)
      + [Mounting FSx for OpenZFS from Amazon Elastic Container Service](mount-openzfs-ecs-containers.md)
   + [Using FSx for OpenZFS with Autodesk Flame on AWS](use-fsxZ-autodesk.md)
+ [Availability and durability](availability-durability.md)
+ [Managing Amazon FSx for OpenZFS resources](administering-file-systems.md)
   + [Managing FSx for OpenZFS file system resources](managing-file-systems.md)
      + [Creating FSx for OpenZFS file systems](creating-file-systems.md)
      + [Updating a file system](updating-file-system.md)
      + [Managing SSD storage capacity and provisioned IOPS](managing-storage-capacity.md)
      + [Managing throughput capacity](managing-throughput-capacity.md)
      + [Deleting a file system](delete-file-system.md)
      + [Viewing your file system](viewing-file-system.md)
   + [Managing FSx for OpenZFS volumes](managing-volumes.md)
   + [FSx for OpenZFS file system status](file-system-lifecycle-states.md)
   + [Tag your Amazon FSx resources](tag-resources.md)
   + [Working with Amazon FSx maintenance windows](maintenance-windows.md)
+ [Migrating to Amazon FSx for OpenZFS](migrating-fsx-openzfs.md)
   + [How to migrate existing files to FSx for OpenZFS using AWS DataSync](migrate-files-to-fsx-datasync.md)
   + [How to migrate existing files to Amazon FSx using rsync](fsx-migrate-rsync.md)
   + [How to migrate existing files to Amazon FSx using Robocopy](fsx-migrate-robocopy.md)
   + [Cutting over to Amazon FSx](cutover.md)
+ [Protecting your Amazon FSx for OpenZFS data with backups and snapshots](protecting-data.md)
   + [Working with backups](using-backups.md)
      + [Using AWS Backup with Amazon FSx](aws-backup-and-fsx.md)
   + [Working with FSx for OpenZFS snapshots](snapshots-openzfs.md)
      + [Setting up a custom snapshot schedule](custom-snapshot-schedule.md)
+ [Monitoring Amazon FSx for OpenZFS](monitoring_overview.md)
   + [Monitoring tools](monitoring_automated_manual.md)
   + [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)
      + [How to use FSx for OpenZFS metrics](how_to_use_metrics.md)
      + [Accessing CloudWatch metrics](accessingmetrics.md)
      + [Creating CloudWatch alarms to monitor Amazon FSx](creating_alarms.md)
   + [Logging FSx for OpenZFS API Calls with AWS CloudTrail](logging-using-cloudtrail-win.md)
+ [Amazon FSx for OpenZFS performance](performance.md)
+ [Security in Amazon FSx for OpenZFS](security.md)
   + [Data protection in Amazon FSx for OpenZFS](data-protection.md)
      + [Encryption at rest](encryption-rest.md)
      + [Encryption in transit](encryption-transit.md)
   + [File System Access Control with Amazon VPC](limit-access-security-groups.md)
   + [Identity and access management for Amazon FSx for OpenZFS](security-iam.md)
      + [How Amazon FSx for OpenZFS works with IAM](security_iam_service-with-iam.md)
      + [Identity-based policy examples for Amazon FSx for OpenZFS](security_iam_id-based-policy-examples.md)
      + [Troubleshooting Amazon FSx for OpenZFS identity and access](security_iam_troubleshoot.md)
      + [Using tags with Amazon FSx](using-tags-fsx.md)
      + [Using service-linked roles for Amazon FSx](using-service-linked-roles.md)
   + [Compliance validation for Amazon FSx for OpenZFS](fsx-openzfs-compliance.md)
   + [Amazon FSx for OpenZFS and interface VPC endpoints (AWS PrivateLink)](fsx-vpc-endpoints.md)
   + [Resilience in Amazon FSx for OpenZFS](disaster-recovery-resiliency.md)
   + [Infrastructure security in Amazon FSx for OpenZFS](infrastructure-security.md)
   + [AWS managed policies for Amazon FSx](security-iam-awsmanpol.md)
+ [Quotas](limits.md)
+ [Document history for Amazon FSx for OpenZFS](document-history.md)