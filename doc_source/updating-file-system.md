# Updating a file system<a name="updating-file-system"></a>

You can modify the following FSx for OpenZFS file system properties using the AWS Management Console, CLI, and API\.
+ **Copy tags to backups** turns on or off copying file system tags to file system backups\.
+ **Copy tags to volumes** turns on or off copying file system tags to the volumes attached to the file system\.
+ **Throughput capacity** [increases or decreases](managing-throughput-capacity.md) your file system's throughput capacity\.
+ **Automatic daily backups** turns automatic daily backups on or off, modifies the backup window and the backup retention period\. For more information about backups, see [Working with automatic daily backups ](using-backups.md#automatic-backups)\.
+ **Weekly maintenance window** sets the day of the week and time that Amazon FSx performs file system maintenance and updates\.

You can update an FSx for OpenZFS file system's configuration using the Amazon FSx console, the AWS CLI, and the Amazon FSx API\.

## To update a file system \(console\)<a name="update-file-system-console"></a>

**To update how file system tags are copied**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. In the left navigation pane, choose **File systems**, and then choose the FSx for OpenZFS file system that you want to update\.

1. For **Actions**, choose **Update file system**\.

   The **Update file system** dialog box displays\.
   + For **Copy tags to backups**, enable or disable the option to copy tags on the file system to any backup that's taken\.
   + For **Copy tags to volumes**, enable or disable the option to copy tags on the file system to any volume that you create\.

1. Choose **Update** to update the file system with your changes\.

**To update the weekly maintenance window**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. To display the file system details page, in the left navigation pane, choose **File systems**, and then choose the FSx for OpenZFS file system that you want to update\.

1. Choose the **Administration** tab in the second panel on the page\.

1. Choose **Update**\.

1. Modify when the weekly maintenance window occurs for this file system\.

1. Choose **Save** to save your changes\.

**To update automatic daily backups**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. To display the file system details page, in the left navigation pane, choose **File systems**, and then choose the FSx for OpenZFS file system that you want to update\.

1. Choose the **Backups** tab in the second panel on the page\.

1. Choose **Update**\.

1. Modify the automatic daily backup settings for this file system\.

1. Choose **Save** to save your changes\.

## To update a file system \(CLI\)<a name="update-file-system-cli"></a>
+ To update the configuration of an FSx for OpenZFS file system, use the [update\-file\-system](https://docs.aws.amazon.com/cli/latest/reference/fsx/update-file-system.html) CLI command \(or the equivalent [UpdateFileSystem](https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateFileSystem.html) API operation\), as shown in the following example\.

  ```
  aws fsx update-file-system \
      --file-system-id fs-0123456789abcdef0 \
      --open-zfs-configuration AutomaticBackupRetentionDays=30,DailyAutomaticBackupStartTime=01:00, \
        WeeklyMaintenanceStartTime=1:01:30
  ```