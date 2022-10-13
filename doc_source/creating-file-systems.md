# Creating FSx for OpenZFS file systems<a name="creating-file-systems"></a>

You can create an FSx for OpenZFS file system using the Amazon FSx console, AWS CLI, or the Amazon FSx API\.

## To create a file system \(console\)<a name="create-file-system-console"></a>

This procedure uses the **Standard create** creation option to create an FSx for OpenZFS file system with a configuration that you customize for your needs\. For information about using the **Quick create** creation option to rapidly create a file system with a default set of configuration parameters, see [Step 1: Create an Amazon FSx for OpenZFS file system](getting-started-step1.md)\.

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. On the dashboard, choose **Create file system** to start the file system creation wizard\.

1. On the **Select file system type** page, choose **FSx for OpenZFS **, and then choose **Next**\. The **Create file system** page appears\.

1. For **Creation method**, choose **Standard create**\.

   Begin your configuration with the **File system details** section\.  
![\[Standard create File system basics section, showing options for file system parameters such as the file system name, SSD storage capacity, and throughput capacity.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/create-fs-standard.png)

1. For **File system name \- optional**, enter a name for your file system\. It's easier to find and manage your file systems when you name them\. You can use a maximum of 256 Unicode letters, white space, and numbers, plus these special characters: \+ \- = \. \_ : /

1. For **Storage capacity**, enter the storage capacity of your file system, in GiB\. Enter any whole number in the range of 64–524288\.

1. For **Provisioned SSD IOPS**, you have two options to provision the number of IOPS for your file system:
   + Choose **Automatic** \(the default\) if you want Amazon FSx to automatically provision 3 IOPS per GB of SSD storage\.
   + Choose **User\-provisioned** if you want to specify the number of IOPS\. You can provision a maximum of 160,000 SSD IOPS per file system\. You pay for SSD IOPS that you provision above the 3 IOPS per GB of SSD storage\.

1. For **Throughput capacity**, you have two options to provide your desired throughput capacity in MB per second \(MBps\)\. **Throughput capacity** is the sustained speed at which the file server that hosts your file system can serve data\.
   + Choose **Recommended throughput capacity** \(the default\) if you want Amazon FSx to automatically choose the throughput capacity\. The recommended value is based on the amount of storage capacity that you chose\.
   + Choose **Specify throughput capacity** if you want to specify the throughput capacity value\. You can choose a value of 64, 128, 256, 512, 1024, 2048, 3072, or 4096 MB/s\. You pay for additional throughput capacity that you provision above the recommended amount\.

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

## To create a file system \(CLI\)<a name="create-file-system-cli"></a>
+ To create an FSx for OpenZFS file system, use the [create\-file\-system](https://docs.aws.amazon.com/cli/latest/reference/fsx/create-file-system.html) CLI command \(or the equivalent [CreateFileSystem](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CreateFileSystem.html) API operation\), as shown in the following example\.

  ```
  aws fsx create-file-system\
    --region us-east-1 \
     --file-system-type OPENZFS \
     --storage-capacity 10000 \
     --storage-type SSD \
     --security-group-ids sg-0123456789abcdef3,sg-0123abcd4567ef89a \
     --subnet-ids subnet-1234567890abcdef4 \
     --tags Key=creator,Value=allison \
     --open-zfs-configuration '{
        "AutomaticBackupRetentionDays": 30,
        "CopyTagsToBackups": true,
        "DailyAutomaticBackupStartTime": "02:00",
        "DeploymentType": "SINGLE_AZ_1",
        "DiskIopsConfiguration": { 
           "Iops": 250,
           "Mode": "USER_PROVISIONED"
        },
        "RootVolumeConfiguration": { 
           "CopyTagsToSnapshots": true,
           "DataCompressionType": "LZ4",
           "NfsExports": [ 
              { 
                 "ClientConfigurations": [ 
                    { 
                       "Clients": "*",
                       "Options": [ "rw","root_squash","crossmnt" ]
                    }
                 ]
              }
           ],
           "ReadOnly": false,
           "RecordSizeKiB": 128,
           "UserAndGroupQuotas": [ 
              { 
                 "Id": 1001,
                 "StorageCapacityQuotaGiB": 2000,
                 "Type": "GROUP"
              }
           ]
        },
        "ThroughputCapacity": 128
     }'
  ```

After successfully creating the file system, Amazon FSx returns the file system's description in JSON format\.