# Using AWS Backup with Amazon FSx<a name="aws-backup-and-fsx"></a>

AWS Backup is a simple and cost\-effective way to protect your data by backing up your Amazon FSx for OpenZFS file systems\. AWS Backup is a unified backup service designed to simplify the creation, restoration, and deletion of backups, while providing improved reporting and auditing\. AWS Backup makes it easier to develop a centralized backup strategy for legal, regulatory, and professional compliance\. AWS Backup also makes protecting your AWS storage file systems, databases, and file systems simpler by providing a central place where you can do the following:
+ Configure and audit the AWS resources that you want to back up\.
+ Automate backup scheduling\.
+ Set retention policies\.
+ Copy backups across AWS Regions and AWS accounts
+ Monitor all recent backup, copy, and restore activity\.

AWS Backup uses the built\-in backup functionality of Amazon FSx\. Backups taken from the AWS Backup console have the same level of file system consistency and performance, are incremental relative to any other Amazon FSx backups you take of your file system \(user\-initiated or automatic\), and offer the same restore options as backups taken through the Amazon FSx console\. If you use AWS Backup to manage these backups, you gain additional functionality, such as unlimited retention options and the ability to create scheduled backups as frequently as every hour\. In addition, AWS Backup and Amazon FSx retain your immutable backups even after the source file system is deleted\. This protects against accidental or malicious deletion\.

Backups taken by AWS Backup are considered user\-initiated backups, and they count toward the user\-initiated backup quota for Amazon FSx\. You can view and restore backups taken by AWS Backup in the Amazon FSx console, CLI, and API\. However, you can't delete backups taken by AWS Backup in the Amazon FSx console, CLI, or API\. For more information about how to use AWS Backup to back up your Amazon FSx file systems and how to delete backups, see [Working with Amazon FSx File Systems](https://docs.aws.amazon.com/aws-backup/latest/devguide/working-with-other-services.html#working-with-fsx) in the *AWS Backup Developer Guide*\.

## Restoring backups in AWS Backup<a name="restoring-backups-bkp"></a>

You can use an available backup to create a new file system, effectively restoring a point\-in\-time snapshot of a file system\. You can restore a backup using the AWS Management Console, AWS CLI, or one of the AWS SDKs\. When you restore a backup, Amazon FSx must download the data in your backup onto the file system before the file system can be brought online\. This process utilizes the unused portion of your file system's throughput capacity, and can take from a few minutes to a few hours depending on the size of your backup and level of unused throughput capacity on your file system\.

The following procedure guides you through how to restore a backup using the console to create a new file system\. You can also restore a backup using the CLI [create\-file\-system\-from\-backup](https://docs.aws.amazon.com/cli/latest/reference/fsx/create-file-system-from-backup.html) command or the equivalent API action [CreateFileSystemFromBackup](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CreateFileSystemFromBackup.html)\.

**To restore an FSx for OpenZFS file system \(AWS Backup console\)**

1. Open the AWS Backup console at [https://console\.aws\.amazon\.com/backup](https://console.aws.amazon.com/backup)\.

1. In the navigation pane, choose **Protected resources**, and then choose the Amazon FSx resource ID that you want to restore\.

1. On the **Resource details** page, a list of recovery points for the selected resource ID is shown\. Choose the recovery point ID of the resource\. 

1.  In the upper\-right corner of the pane, choose **Restore** to open the **Restore backup to new file system** page\.

1. In the **Settings** section, the ID of your backup is shown under **Backup ID**, and the file system type is shown under **File system type**\. File system type should be **FSx for OpenZFS**\.

1. Under **Restore options**, you may select **Quick restore** or **Standard restore**\. Quick restore uses the settings of the source file system\. If you choose Standard restore, specify the additional following configurations:

   1. **Provisioned SSD IOPS**: You can choose the **Automatic radio button** or you can choose the **User\-provisioned option**\.

   1. **Throughput capacity**: You can choose the **Recommended throughput capacity** of 64 MB/sec or you can choose to **Specify throughput capacity**\.

   1. \(*Optional*\) **VPC security groups**: You can specify VPC security groups to associate with your file system’s network interface\.

   1. **Encryption key**: Specify the AWS Key Management Service key to protect the restored file system data at rest\.

   1. \(*Optional*\) **Root Volume configuration**: This configuration is collapsed by default\. You may expand it by clicking the down\-pointing carat \(arrow\)\. Creating a file system from a backup will create a new file system; the volumes and snapshots will retain their source configurations\.

   1. \(*Optional*\) **Backup and maintenance**: To set a scheduled backup, click the down\-pointing carat \(arrow\) to expand the section\. You may choose the backup window, hour and minute, retention period, and weekly maintenance window\.

1. \(*Optional*\) You may enter a name for your volume\.

1. The **SSD Storage capacity** will display the file system’s storage capacity\.

1. Choose the **Virtual Private Cloud** \(VPC\) from which your file system can be accessed\.

1. In the **Subnet** dropdown menu, choose the subnet in which your file system’s network interface resides\.

1. In the **Restore role** section, choose the IAM role that AWS Backup will use to create and manage your backups on your behalf\. We recommend that you choose the **Default role**\. If there is no default role, one is created for you with the correct permissions\. You can also choose an IAM role\.

1. Verify all your entries, and choose **Restore Backup**\.

## Deleting backups<a name="delete-aws-backups"></a>

Deleting a backup is a permanent, unrecoverable action\. Any data in a deleted backup is also deleted\. Do not delete a backup unless you're sure you won't need that backup again in the future\. You can't delete backups taken by Amazon FSx in the Amazon FSx console, CLI, or API\. For information on deleting backups taken with AWS Backup see [Deleting Backups](https://docs.aws.amazon.com/aws-backup/latest/devguide/deleting-backups.html) in the *AWS Backup Developer Guide*\.

**To delete a backup**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. From the console dashboard, choose **Backups** from the left side navigation\.

1. Choose the backup that you want to delete from the **Backups** table, and then choose **Delete backup**\.

1. In the **Delete backups** dialog box that opens, confirm that the ID of the backup identifies the backup that you want to delete\.

1. Confirm that the check box is checked for the backup that you want to delete\.

1. Choose **Delete backups**\.

Your backup and all included data are now permanently and unrecoverably deleted\.