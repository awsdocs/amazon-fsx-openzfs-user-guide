# Working with backups<a name="using-backups"></a>

With FSx for OpenZFS, backups are file\-system\-consistent, highly durable, and incremental\. To ensure high durability, Amazon FSx stores backups in Amazon Simple Storage Service \(Amazon S3\)\.

Amazon FSx backups are incremental, whether they are generated using the automatic daily backup or the user\-initiated backup feature\. This means that only the data on the file system that has changed after your most recent backup is saved\. This minimizes the time required to create the backup and saves on storage costs by not duplicating data\. When you delete a backup, only the data unique to that backup is removed\. Each FSx for OpenZFS backup contains all of the information that is needed to create a new file system from the backup, effectively restoring a point\-in\-time snapshot of the file system\.

Creating regular backups for your file system is a best practice that complements the replication that FSx for OpenZFS performs for your file system\. Amazon FSx backups help support your backup retention and compliance needs\. Working with Amazon FSx backups is easy, whether it's creating backups, copying a backup, restoring a file system from a backup, or deleting a backup\.

**Topics**
+ [Working with automatic daily backups](#automatic-backups)
+ [Working with user\-initiated backups](#user-initiated-backups)
+ [Copying backups](#copy-backups)
+ [Restoring backups](#restoring-backups)
+ [Deleting backups](#delete-backups)
+ [Using AWS Backup with Amazon FSx](aws-backup-and-fsx.md)

## Working with automatic daily backups<a name="automatic-backups"></a>

By default, Amazon FSx takes an automatic daily backup of your file system\. These automatic daily backups occur during the daily backup window that was established when you created the file system\. At some point during the daily backup window, storage I/O might be suspended briefly while the backup process initializes \(typically for less than a few seconds\)\. When you choose your daily backup window, we recommend that you choose a convenient time of the day\. This time ideally is outside of the normal operating hours for the applications that use the file system\.

Automatic daily backups are kept for a certain period of time, known as a retention period\. The default retention period for automatic daily backups is 7 days\. You can set the retention period to be between 1–90 days\. Automatic daily backups are deleted when the file system is deleted\.

**Note**  
While automatic daily backups have a maximum retention period of 90 days, user\-initiated backups are kept forever, unless you delete them\. For more information about user\-initiated backups, see [Working with user\-initiated backups](#user-initiated-backups)\.\.

You can use the AWS CLI or one of the AWS SDKs to change the backup window and backup retention period for your file systems\. Use the [https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateFileSystem.html](https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateFileSystem.html) API operation or the [https://docs.aws.amazon.com/cli/latest/reference/fsx/update-file-system.html](https://docs.aws.amazon.com/cli/latest/reference/fsx/update-file-system.html) CLI command\.

## Working with user\-initiated backups<a name="user-initiated-backups"></a>

With Amazon FSx, you can manually take backups of your file systems at any time\. You can do so using the Amazon FSx console, API, or the AWS Command Line Interface \(AWS CLI\)\. Your user\-initiated backups of Amazon FSx file systems never expire, and they are available for as long as you want to keep them\. User\-initiated backups are retained even after you delete the file system that was backed up\. You can delete user\-initiated backups only by using the Amazon FSx console, API, or CLI\. They are never automatically deleted by Amazon FSx\. For more information, see [Deleting backups](#delete-backups)\.

### Creating user\-initiated backups<a name="creating-backups"></a>

The following procedure guides you through how to create a user\-initiated backup in the Amazon FSx console for an existing file system\.

**To create a user\-initiated file system backup**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. From the console dashboard, choose the name of the file system that you want to back up\.

1. From **Actions**, choose **Create backup**\.

1. In the **Create backup** dialog box that opens, provide a name for your backup\. Backup names can be a maximum of 256 Unicode characters, including letters, white space, numbers, and the special characters \. \+ \- = \_ : /

1. Choose **Create backup**\.

You have now created your file system backup\. You can find a table of all your backups in the Amazon FSx console by choosing **Backups** in the left side navigation\. You can search for the name you gave your backup, and the table filters to only show matching results\.

When you create a user\-initiated backup as this procedure described, it has the type `User-Initiated`, and it has the `Creating` status until it is fully available\.

## Copying backups<a name="copy-backups"></a>

You can use Amazon FSx to manually copy backups within the same AWS account to another AWS Region \(cross\-Region copies\) or within the same AWS Region \(in\-Region copies\)\. You can make cross\-Region copies only within the same AWS partition\. You can create user\-initiated backup copies using the Amazon FSx console, AWS CLI, or API\. When you create a user\-initiated backup copy, it has the type `USER_INITIATED`\.

*Cross\-Region backup copies* are particularly valuable for cross\-Region disaster recovery\. You take backups and copy them to another AWS Region so that in the event of a disaster in the primary AWS Region, you can restore from backup and recover availability quickly in the other AWS Region\. You can also use backup copies to clone your file dataset to another AWS Region or within the same AWS Region\. You make backup copies within the same AWS account \(cross\-Region or in\-Region\) by using the Amazon FSx console, AWS CLI, or Amazon FSx API\.

### Backup copy limitations<a name="copy-limitations"></a>

The following are some limitations when you copy backups:
+ Cross\-Region backup copies are supported only between any two commercial AWS Regions, between the China \(Beijing\) and China \(Ningxia\) Regions, and between the AWS GovCloud \(US\-East\) and AWS GovCloud \(US\-West\) Regions, but not across those sets of Regions\.
+ Cross\-Region backup copies are not supported in opt\-in Regions\.
+ You can make in\-Region backup copies within any AWS Region\.
+ Cross\-account backup copies are not supported\.
+ The source backup must have a status of `AVAILABLE` before you can copy it\.
+ You cannot delete a source backup if it is being copied\. There might be a short delay between when the destination backup becomes available and when you are allowed to delete the source backup\. You should keep this delay in mind if you retry deleting a source backup\.
+ You can have up to five backup copy requests in progress to a single destination AWS Region per account\.

### Permissions for cross\-Region backup copies<a name="copy-permissions"></a>

You use an IAM policy statement to grant permissions to perform a backup copy operation\. To communicate with the source AWS Region to request a cross\-Region backup copy, the requester \(IAM role or IAM user\) must have access to the source backup and the source AWS Region\.

You use the policy to grant permissions to the `CopyBackup` action for the backup copy operation\. You specify the action in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field, as in the following example\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "fsx:CopyBackup",
            "Resource": "arn:aws:fsx:*:111111111111:backup/*"
        }
    ]
}
```

For more information on IAM policies, see [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

### Full and incremental copies<a name="copy-incrementals"></a>

When you copy a backup to a different AWS Region from the source backup, the first copy is a full backup copy\. After the first backup copy, all subsequent backup copies to the same destination Region within the same AWS account are incremental, provided that you haven't deleted all previously\-copied backups in that Region and have been using the same AWS KMS key\. If both conditions aren't met, the copy operation results in a full \(not incremental\) backup copy\.

### To copy a backup within the same account \(cross\-Region or in\-Region\) using the console<a name="collapsible-section-1"></a>

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. In the navigation pane, choose **Backups**\.

1. In the **Backups** table, choose the backup that you want to copy, and then choose **Copy backup**\.

1. In the **Settings** section, do the following:
   + In the **Destination Region** list, choose a destination AWS Region to copy the backup to\. The destination can be in another AWS Region \(cross\-Region copy\) or within the same AWS Region \(in\-Region copy\)\.
   + \(Optional\) Select **Copy Tags** to copy tags from the source backup to the destination backup\. If you select **Copy Tags** and also add tags at step 6, all the tags are merged\.

1. For **Encryption**, choose the AWS KMS encryption key to encrypt the copied backup\.

1. For **Tags \- optional**, enter a key and value to add tags for your copied backup\. If you add tags here and also selected **Copy Tags** at step 4, all the tags are merged\.

1. Choose **Copy backup**\.

Your backup is copied within the same AWS account to the selected AWS Region\.

### To copy a backup within the same account \(cross\-Region or in\-Region\) using the CLI<a name="collapsible-section-2"></a>
+ Use the `copy-backup` CLI command or the [CopyBackup](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CopyBackup.html) API operation to copy a backup within the same AWS account, either across an AWS Region or within an AWS Region\.

  The following command copies a backup with an ID of `backup-0abc123456789cba7` from the `us-east-1` Region\.

  ```
  aws fsx copy-backup \
    --source-backup-id backup-0abc123456789cba7 \
    --source-region us-east-1
  ```

  The response shows the description of the copied backup\.

   You can view your backups on the Amazon FSx console or programmatically using the `describe-backups` CLI command or the [DescribeBackups](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeBackups.html) API operation\.

## Restoring backups<a name="restoring-backups"></a>

You can use an available backup to create a new file system, effectively restoring a point\-in\-time snapshot of another file system\. You can restore a backup using the AWS Backup or Amazon FSx consoles, AWS CLI, or one of the AWS SDKs\. For more information on using the AWS Backup console, see [Restoring backups in AWS Backup](aws-backup-and-fsx.md#restoring-backups-bkp)

Restoring a backup to a new file system takes the same amount of time as creating a new file system\. The data restored from the backup is lazy\-loaded onto the file system, during which time you will experience slightly higher latency\.

The following procedure guides you through how to restore a backup to a new file system using the Amazon FSx console\.

**To restore a file system from backup \(Amazon FSx console\)**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. From the console dashboard, choose **Backups** from the left side navigation\.

1. Choose the backup that you want to restore from the **Backups** table, and then choose **Restore backup**\. 

   Doing so opens the file system creation wizard\.  
![\[Standard create File system basics section, showing options for file system parameters such as the file system name, SSD storage capacity, and throughput capacity.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/create-fs-standard.png)

1. For **File system name \- optional**, you can enter a name using a maximum of 256 Unicode letters, white space, and numbers, plus these special characters: \+ \- = \. \_ : /

1. For **Storage capacity**, enter a value that is equal to or greater than the storage capacity of the original file of which the backup was taken, in GiB\. The range of valid values is 64–524288 GiB\.

1. For **Provisioned SSD IOPS**, you have two options to provision the number of IOPS for your file system:
   + Choose **Automatic** \(the default\) if you want Amazon FSx to automatically provision 3 IOPS per GiB of SSD storage\.
   + Choose **User\-provisioned** if you want to specify the number of IOPS\. You can provision a maximum of 160,000 SSD IOPS per file system\. You pay for SSD IOPS that you provision above the 3 IOPS per GiB of SSD storage\.

1. For **Throughput capacity**, you have two options to provide your desired throughput capacity in Megabytes per second \(MBps\)\. **Throughput capacity** is the sustained speed at which the file server that hosts your file system can serve data\.
   + Choose **Recommended throughput capacity** \(the default\) if you want Amazon FSx to automatically choose the throughput capacity\. The recommended value is based on the amount of storage capacity that you chose\.
   + Choose **Specify throughput capacity** if you want to specify the throughput capacity value, and choose a value of 64, 128, 256, 512, 1024, 2048, 3072, or 4096 MB/s\. You pay for additional throughput capacity that you provision above the recommended amount\.

   You can increase the amount of throughput capacity as needed at any time after you create the file system\. For more information, see [Managing throughput capacity](managing-throughput-capacity.md)\.

1. In the **Network & security** section, provide networking and security group information:
   + For **Virtual Private Cloud \(VPC\)**, choose the Amazon VPC that you want to associate with your file system\.
   + For **VPC Security Groups**, the ID for the default security group for your VPC should be already added\.
   + For **Subnet**, choose any value from the list of available subnets\.

1. In the **Encryption** section, for **Encryption key**, choose the AWS Key Management Service \(AWS KMS\) encryption key that protects your file system's data at rest\.

1. In **Root volume configuration**, you can set the following options for the file system's root volume:
   + For **Data compression type**, choose the type of compression to use for your volume, either **Z\-Standard**, **LZ4**, or **No compression**\. Z\-Standard compression provides more data compression and higher read throughput than LZ4 compression\. LZ4 compression provides less compression and higher write throughput performance than Z\-Standard compression\. For more information about the storage and performance benefits of the volume data compression options, see [Data compression](performance.md#perf-data-compression)\.
   + For **Copy tags to snapshots**, enable or disable the option to copy tags to the volume's snapshot\.
   + For **NFS exports**, there is a default client configuration setting which you can modify or remove\. Client configurations define which clients can access the volume and their permissions\.

     To provide additional client configurations:

     1. In the **Client addresses** field, specify which clients can access the volume\. Enter an asterisk \(`*`\) for any client, a specific IP address, or a CIDR range of IP addresses\.

     1. In the **NFS options** field, enter a comma\-delimited set of exports options\. For example, enter `rw` to allow read and write permissions to the volume for the specified **Client addresses**\.

     1. Choose **Add client configuration**\.

     1. Repeat the procedure to add another client configuration\.

     For more information, see [NFS exports](managing-volumes.md#nfs-exports)\.
   + For **Record size**, choose whether to use the default suggested record size of 128 KiB, or to set a custom suggested record size for the volume\. Generally, workloads that write in fixed small or large record sizes may benefit from setting a custom record size, like database workloads \(small record size\) or media streaming workloads \(large record size\)\. We recommend using the default setting for the majority of use cases\. For more information about the record size setting, see [Volume properties](managing-volumes.md#volume-properties)\. 
   + For **User and group quotas**, you can set a storage quota for a user or group:

     1. For **Quota type**, choose `USER` or `GROUP`\.

     1. For **User or group ID**, choose a number that is the ID of the user or group\.

     1. For **Usage quota**, choose a number that is the storage quota of the user or group\.

     1. Choose **Add quota**\.

     1. Repeat the procedure to add a quota for another user or group\.

1. In **Backup and maintenance \- *optional***, you can set the following options:
   + For **Daily automatic backup**, choose **Enabled** for automatic daily backups\. This option is enabled by default\.
   + For **Daily automatic backup window**, set the time of the day in Coordinated Universal Time \(UTC\) that you want the daily automatic backup window to start\. The window is 30 minutes starting from this specified time\. This window can't overlap with the weekly maintenance backup window\.
   + For **Automatic backup retention period**, set a period from 1–90 days that you want to retain automatic backups\.
   + For **Weekly maintenance window**, you can set the time of the week that you want the maintenance window to start\. Day 1 is Monday, 2 is Tuesday, and so on\. The window is 30 minutes starting from this specified time\. This window can't overlap with the daily automatic backup window\.

1. For **Tags \- *optional***, you can enter a key and value to add tags to your file system\. A tag is a case\-sensitive key\-value pair that helps you manage, filter, and search for your file system\.

   Choose **Next**\.

1. Review the file system configuration shown on the **Create file system** page\. For your reference, note which file system settings you can modify after the file system is created\.

1. Choose **Create file system**\.

## Deleting backups<a name="delete-backups"></a>

Deleting a backup is a permanent, unrecoverable action\. Any data in a deleted backup is also deleted\. Do not delete a backup unless you're sure you won't need that backup again in the future\. 

**To delete a backup**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. From the console dashboard, choose **Backups** from the left side navigation\.

1. From the **Backups** table, choose the backup that you want to delete\.

1. For **Actions**, choose **Delete backup**\.

1. In the **Delete backups** dialog box that opens, confirm that the ID of the backup identifies the backup that you want to delete\.

1. Confirm that the check box is checked for the backup that you want to delete\.

1. Choose **Delete backups**\.

Your backup and all included data are now permanently and irrecoverably deleted\.