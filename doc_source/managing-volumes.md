# Managing FSx for OpenZFS volumes<a name="managing-volumes"></a>

An FSx for OpenZFS file system can contain one or more *volumes*, which are isolated data containers for files and directories\. Every FSx for OpenZFS file system has one \(and only one\) *root volume*, which is created at file system creation time\. All other volumes created on a file system are children of the root volume\.

After the file system is created, you can create volumes as needed\. Each customer\-created volume is a child of another volume\. This means that all volumes are children or descendants of the root volume\. For example, you could create 10 volumes with each one being a child of the root volume \(that is, all 10 volumes are siblings\) or you could create a hierarchy of 10 volumes in which each volume is a child of the previous volume\.

You use volumes to logically separate an individual file system into multiple namespaces\. This allows you to independently manage the storage capacity, compression, NFS exports, record size, and user and group quotas at the volume level\.

You access volumes from Linux, Windows, or macOS clients over the Network File System \(NFS\) protocol \(v3, v4\.0, v4\.1, or v4\.2\)\. FSx for OpenZFS presents data to your users and applications as a local directory or drive\.

## Volume properties<a name="volume-properties"></a>

When you create a volume, you can set the following configuration properties to customize and control the storage aspects of the volume\.
+ **Volume name** provides a name for the volume\. Please ensure that the specified name does not conflict with an existing file or directory on the parent volume\.
+ **Data compression type** reduces the storage capacity that your data consumes and can also help increase your effective throughput\. You can choose either **Z\-Standard**, **LZ4**, or **No compression**\. Z\-Standard compression provides a higher level of data compression and higher read throughput performance than LZ4 compression\. LZ4 compression provides a lower level of compression and higher write throughput performance than Z\-Standard compression\. For more information about the storage and performance benefits of the volume data compression options, see [Data compression](performance.md#perf-data-compression)\.
+ **Storage capacity quota** sets a volume quota, which is the maximum storage size for the volume\. A volume quota limits the amount of storage space that the volume can consume to the configured amount, but does not guarantee the space will be available\. To guarantee quota space, you must also set **Storage capacity reservation**\.

  By setting quotas without setting a **Storage capacity reservation**, you can create space\-efficient *thin\-provisioned* volumes where capacity is allocated only as storage is being consumed\. With thin\-provisioned volumes, you can assign quotas that are collectively larger than the existing capacity of the file system or quota of a parent volume\. If your file system is nearing capacity or a parent volume is nearing its quota, note that your users or applications may not be able to write in a child volume even though the volume has not reached its quota limit\.
+ **Storage capacity reservation** guarantees a specified amount of storage space to always be available for the volume\. The reservation reserves a configured amount of storage space from the parent volume\. Only the volume with the reservation can use that storage space, regardless of the volume quotas that other volumes may have\. Note that unlike volume quotas, you can't reserve an amount of storage space that doesn't exist in its immediate parent\. Set a reservation if an application must have a certain amount of storage space or it will fail\. 
+ **Record size** sets the suggested block size for a volume in a ZFS dataset\. Choose whether to use the default record size of 128 KiB, or to set a custom record size for the volume\. We recommend using the default setting for the majority of use cases\. For more information about the record size setting, see [ZFS Record size](performance.md#record-size-performance)\. Generally, workloads that write in fixed small or large record sizes may benefit from setting a custom record size, like database workloads \(small record size\) or media streaming workloads \(large record size\)\. See the OpenZFS documentation for more information about [ Dataset record size](https://openzfs.github.io/openzfs-docs/Performance%20and%20Tuning/Workload%20Tuning.html#dataset-recordsize) and ZFS datasets\.
+ **NFS exports** use NFS\-level export policies to define how the file system should be exported over the NFS protocol\. The NFS exports setting defines which clients can access the volume and what permissions they have\. For more information, see [NFS exports](#nfs-exports)\.
+ **User and group quotas** configures individual user and/or group quotas for volumes, which sets a limit on the amount of storage space they can consume on the volume\.
+ **Source snapshot ID** specifies a snapshot from which to create a volume\. Then use **Source snapshot copy strategy** to specify the type of volume to create:
  + **Clone** creates a clone volume\. A clone volume is a writable copy that is initialized with the same data as the snapshot from which it was created\. Because clone volumes reference the data from the snapshot, clone volumes are created almost instantly, and initially consume no additional capacity\. They only consume the capacity required for the incremental changes made to the source snapshot, providing an easy way to support multiple users or applications in parallel from a shared dataset\. However, a clone volume maintains a dependency on its source snapshot, so you cannot delete this source snapshot while the clone volume is in use\.
  + **Full copy** creates a full\-copy volume\. A full\-copy volume is a writable copy that is initialized with the same data as the snapshot from which it was created\. Unlike a clone volume, it does not maintain any dependency on its source snapshot\. Because a full\-copy volume requires copying all of the source snapshot data to a new volume, creation time will depend on the size of the source snapshot\. While this data is being copied, your full\-copy volume will be read only\. Once a full\-copy volume is created, it is identical to a standard FSx for OpenZFS volume\.

  For more information on snapshots, see [Working with FSx for OpenZFS snapshots](snapshots-openzfs.md)\.

### NFS exports<a name="nfs-exports"></a>

NFS exports are NFS\-level export policies that configure which clients can access the volume and the options that are available\. Each volume has its own NFS exports setting, so a client may be able to mount one volume on the file system but not a different volume\.

When creating a volume from the console, you provide the NFS exports information in an array of client configurations, each of which has **Client addresses** and **NFS options** fields\.

The **Client addresses** field specifies which hosts can mount over the NFS protocol and contains one of these settings:
+ `*` is a wildcard that means anyone who can route to the file server can mount it\.
+ The IP address of a client's computer \(such as `10.0.0.1`\) that means a client from that specific IP address can mount the file system\.
+ A CIDR block range \(such as `192.0.2.0/24`\) that means any client from that address range can mount the file system\.

**Note**  
If an IP address is permitted to mount a parent volume, it is also automatically permitted to mount any of the child volumes\.

The **NFS options** field lists a set of exports options available on the volume\. Following are descriptions of the most common NFS options:
+ `rw` allows both read and write requests on this NFS volume from the specified Client addresses\.
+ `ro` allows only read requests on this NFS volume\. The specified Client addresses can't write to the volume\.
+ `crossmnt` allows clients to inherit access to any child volumes within this volume \(if configured along with the `no_sub_tree_check` option, which is included by default\)\. This option is required to provide file\-level access to your snapshots in the `.zfs/snapshot` directory of each volume\.
+ `root_squash` maps requests from UID/GID 0 to the anonymous UID/GID\. It prevents remote root users from having superuser \(root\) privileges on remote NFS\-mounted volumes\. `root_squash` is the default unless overridden by `all_squash` or `no_root_squash`\.
+ `all_squash` maps all User IDs \(UIDs\) and group IDs \(GIDs\) to the anonymous user\.
+ `no_root_squash` turns off root squashing\.
+ `sync` replies to client requests only after the changes have been committed to stable storage \(that is, disk drives\)\. `sync` is the default unless overridden by `async` 
+ `async` replies to client requests \(such as write requests\) after the changes have been committed to memory, but before any changes made by that request have been committed to stable storage \(that is, disk drives\)\. This setting can improve performance for latency\-intensive or IOPS\-intensive workloads\. For more information, see [Amazon FSx for OpenZFS performancePerformance](performance.md)\.
**Warning**  
Use of the `async` option can cause data to be lost or corrupted if a write request is acknowledged but the server crashes before the write request is fully written to disk\.

  For a more comprehensive list of exports options, see the [exports\(5\) \- Linux man page](https://linux.die.net/man/5/exports) on the `die.net` web site\.

When you create a volume, the default for **Client addresses** is an asterisk \(`*`\) and the default for **NFS options** is `rw,crossmnt`\.

## Creating a volume<a name="creating-volumes"></a>

You can create an FSx for OpenZFS volume using the Amazon FSx console, the AWS CLI, and the Amazon FSx API\. You can create new volumes, or create a volume from an existing volume snapshot\.

### To create a volume \(console\)<a name="create-volume-console"></a>

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. In the left navigation pane, choose **File systems**, and then choose the FSx for OpenZFS file system that you want to create a volume for\.

1. Choose the **Volumes** tab\.

1. Choose **Create volume**\.

   The **Create volume** dialog box appears\.

1. In the **File system** field, choose the file system to create the volume on\.

1. In the **Parent volume ID** field, choose the ID of the parent volume, which can be the root volume or another volume\.

1. In the **Volume name** field, provide a name for the volume\. You can use a maximum of 203 alphanumeric characters, plus the underscore \(\_\) special character\. The name must be unique among all the volume names on the same parent volume on the same file system\.

1. For **Storage capacity quota \- optional**, you can set a quota that will be the maximum storage size for the volume\. This quota cannot be larger than the parent volume quota\. To set no quota and allow this volume to consume any available capacity in your file system, set this property to `-1`\.

1. For **Storage capacity reservation \- optional**, you can enter the reservation for the volume\. This reserves dedicated space on the file system storage pool, meaning the space available to all other volumes is reduced by the amount specified\. It cannot be set to a value that's greater than the parent volume quota or the remaining reservation space on the file system storage\. To set no reservation and allow this volume to consume any available capacity in your file system, set this property to `0` or `-1`\.

1. For **Data compression type**, choose the type of compression to use for your volume, either **Z\-Standard**, **LZ4**, or **No compression**\. Z\-Standard compression provides more data compression and higher read throughput than LZ4 compression\. LZ4 compression provides less compression and higher write throughput performance than Z\-Standard compression\. For more information about the storage and performance benefits of the volume data compression options, see [Data compression](performance.md#perf-data-compression)\.

1. For **Copy tags to snapshots**, enable or disable the option to copy tags on the root volume to snapshots\.

1. For **NFS exports**, there is a default client configuration setting which you can modify or remove\. Client configurations define which clients can access the volume and their permissions\.

   To provide additional client configurations:

   1. In the **Client addresses** field, specify which clients can access the volume\. Enter an asterisk \(`*`\) for any client, a specific IP address, or a CIDR range of IP addresses\.

   1. In the **NFS options** field, enter a comma\-delimited set of exports options\. For example, enter `rw` to allow read and write permissions to the volume\.

   1. Choose **Add client configuration**\.

   1. Repeat the procedure to add another client configuration\.

   For more information, see [NFS exports](#nfs-exports)\.

1. For **Record size**, choose whether to use the default suggested record size of 128 KiB, or to set a **User\-configured** suggested record size for the volume\. Generally, workloads that write in fixed small or large record sizes may benefit from setting a custom record size, like database workloads \(small record size\) or media streaming workloads \(large record size\)\. We recommend using the default setting for the majority of use cases\. For more information about the record size setting, see [Volume properties](#volume-properties)\. 

1. For **User and group quotas**, you can set a storage quota for a user or group:

   1. For **Quota type**, choose `USER` or `GROUP`\.

   1. For **User or group ID**, choose a number that is the ID of the user or group\.

   1. For **Usage quota**, choose a number that is the storage quota of the user or group\.

   1. Choose **Add quota**\.

   1. Repeat the procedure to add a quota for another user or group\.

1. To create a volume from an existing volume snapshot, use **Source snapshot ID \- optional**, to specify the ID of a snapshot from which to create a volume\. Then choose a **Source snapshot copy strategy** option for the type of volume you're creating:
   + **Clone** creates a clone volume\. The snapshot will provide the seed content for the volume\.
   + **Full copy** creates a volume that will contain data copied from the snapshot\.

1. Choose **Confirm** to create the volume\.

You can monitor the progress on the **File systems** detail page, in the **Status** column of the **Volumes** pane\. The volume is ready for use when its status is **Created**\.

### To create a volume \(CLI\)<a name="create-volume-cli"></a>
+ To create an FSx for OpenZFS volume, use the [create\-volume](https://docs.aws.amazon.com/cli/latest/reference/fsx/create-volume.html) CLI command \(or the equivalent [ CreateVolume](https://docs.aws.amazon.com/fsx/latest/APIReference/API_CreateVolume.html) API operation\)\. The following example creates a new volume by cloning an existing snapshot\.

  ```
  aws fsx create-volume \	
  	--name vol2 \
      --volume-type OPENZFS \
      --tags Key=creator,Value=Liu \
      --open-zfs-configuration '{ 
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
        "OriginSnapshot": { 
           "CopyStrategy": "CLONE",
           "SnapshotARN": "arn:aws:fsx:us-east-2:111122223333:snapshot/fsvol-0123456789abcdef0/fsvolsnap-1234567890abcdef0"
        },
        "ParentVolumeId": "fsvol-abcdef01234567890",
        "ReadOnly": false,
        "RecordSizeKiB": 128,
        "StorageCapacityQuotaGiB": 10000,
        "StorageCapacityReservationGiB": -1,
        "UserAndGroupQuotas": [ 
           { 
              "Id": 1004,
              "StorageCapacityQuotaGiB": 2000,
              "Type": "GROUP"
           }
        ]
     }'
  ```

After successfully creating the volume, Amazon FSx returns its description in JSON format\.

## Updating a volume configuration<a name="updating-volumes"></a>

You can update the following FSx for OpenZFS [volume properties](#volume-properties):
+ *Storage capacity quota* – you can change an existing quota setting, set a new quota, or unset the capacity quota\.
+ *Storage capacity reservation* – you can change an existing storage capacity setting, enable storage capacity reservation, or unset the capacity reservation\.
+ *Data compression type* – you can enable compression, change the compression type, and turn compression off\. Changing this property affects only newly\-written data\.
+ *NFS exports* – you can manage access to the volume by modifying **Client addresses** and **NFS options**\. For more information, see [NFS exports](#nfs-exports)\. 
+ *User and group quotas* – you can change or remove existing quotas and add new quotas\.
+ *Volume record size* – you can change the record size for the volume\. Changing the volume record size affects only files created afterward; existing files are unaffected\. For more information, see [ZFS Record size](performance.md#record-size-performance)\.



You can update the volume using the Amazon FSx console, the AWS CLI, and the Amazon FSx API\.

### To update a volume configuration \(console\)<a name="update-volume-console"></a>

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. Navigate to **File systems** and choose the FSx for OpenZFS file system that you want to update a volume for\.

1. Choose the **Volumes** tab\.

1. Choose the volume that you want to update\.

1. For **Actions**, choose **Update volume**\.

   The **Update volume** dialog box displays with the volume's current settings\.

1. For **Storage capacity quota \- optional**, you can set a quota that will be the maximum storage size for the volume\. This quota cannot be larger than the parent volume quota\. To set no quota and allow this volume to consume any available capacity in your file system, set this property to `-1`\.

1. For **Storage capacity reservation \- optional**, you can enter the reservation for the volume\. This reserves dedicated space on the file system storage pool, meaning the space available to all other volumes is reduced by the amount specified\. It cannot be set to a value that's greater than the parent volume quota or the remaining reservation space on the file system storage\. To set no reservation and allow this volume to consume any available capacity in your file system, set this property to `0` or `-1`\.

1. For **Data compression type**, choose the type of compression to use for your volume, either **Z\-Standard**, **LZ4**, or **No compression**\. Z\-Standard compression provides more data compression and higher read throughput than LZ4 compression\. LZ4 compression provides less compression and higher write throughput performance than Z\-Standard compression\. Changing this property affects only newly\-written data\. For more information about the storage and performance benefits of the volume data compression options, see [Data compression](performance.md#perf-data-compression)\.

1. For **NFS exports**, you can modify or remove the existing client configurations that define which clients can access the volume and what permissions they have\.

   You can provide additional client configurations for the volume:

   1. In the **Client addresses** field, specify which clients can access the volume\. Enter an asterisk \(`*`\) for any client, a specific IP address, or a CIDR range of IP addresses\.

   1. In the **NFS options** field, enter a comma\-delimited set of exports options\. For example, enter `rw` to allow read and write permissions to the volume\.

   1. Choose **Add client configuration**\.

   1. Repeat the procedure to add another client configuration\.

   For more information, see [NFS exports](#nfs-exports)\.

1. For **Record size**, choose whether to use the default suggested record size of 128 KiB, or to set a **User\-configured** suggested record size for the volume\. Generally, workloads that write in fixed small or large record sizes may benefit from setting a custom record size, like database workloads \(small record size\) or media streaming workloads \(large record size\)\. We recommend the default setting for the majority of use cases\. For more information about the record size setting, see [Volume properties](#volume-properties)\. 

1. For **User and group quotas**, you can change or set a storage quota for a user or group:

   1. For **Quota type**, choose `USER` or `GROUP`\.

   1. For **User or group ID**, enter a number that is the ID of the user or group\.

   1. For **Usage quota**, enter a number that is the storage quota in GiB of the user or group\.

   1. To create additional user or group quotas, choose **Add quota** and repeat the procedure to change or add a quota for another user or group\.

1. For **Record size**, enter the desired maximum size of a logical block in a ZFS dataset\. Valid values are 4, 8, 16, 32, 64, 128, 256, 512, or 1024 KiB\. The default is 128 KiB\. Most file systems will use the default value\. Database workflows can benefit from a smaller record size, while streaming workflows can benefit from a larger record size\. For more information about Record size and file system performance, see [ZFS Record size](performance.md#record-size-performance)\.

1. Choose **Update** to update the volume\.

### To update a volume configuration \(CLI\)<a name="update-volume-cli"></a>
+ To update the configuration of an FSx for OpenZFS volume, use the [update\-volume](https://docs.aws.amazon.com/cli/latest/reference/fsx/update-volume.html) CLI command \(or the equivalent [UpdateVolume](https://docs.aws.amazon.com/fsx/latest/APIReference/API_UpdateVolume.html) API operation\), as shown in the following example\. In this example, the `StorageCapacityQuota` and `StorageCapacityReservationGiB` properties are unset, turning off the quota capacity reservation for the volume\.

  ```
  aws fsx update-volume \
      --volume-id fsxvol-0123456789abcdef0 \
      --open-zfs-configuration '{ 
        "DataCompressionType": "ZSTD",
        "NfsExports": [ 
           { 
              "ClientConfigurations": [ 
                 { 
                    "Clients": "192.0.2.0/24",
                    "Options": [ "crossmnt" ]
                 }
              ]
           }
        ],
        "RecordSizeKiB": 128,
        "StorageCapacityQuotaGiB": -1,
        "StorageCapacityReservationGiB": -1,
        "UserAndGroupQuotas": [ 
           { 
              "Id": 1102,
              "StorageCapacityQuotaGiB": 2000,
              "Type": "GROUP"
           }
        ]
     }'
  ```

## Deleting a volume<a name="deleting-volumes"></a>

You can delete an FSx for OpenZFS volume using the Amazon FSx console, the AWS CLI, and the Amazon FSx API\.

When you delete a volume, the operation has the following results:
+ Deletes all data in the volume
+ Deletes all snapshots
+ Deletes all child volumes and their snapshots

**Note**  
You cannot delete a root volume\. You also cannot delete any volumes from a client mount \(for example, with a `rm -r /fsx/vol1` command\)\.

Before you delete a volume, make sure that no applications are accessing the data in the volume that you want to delete\.

### To delete a volume \(console\)<a name="delete-volume-console"></a>

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. In the left navigation pane, choose **File systems**, and then choose the FSx for OpenZFS file system that you want to delete a volume from\.

1. Choose the **Volumes** tab\.

1. Choose the volume that you want to delete\.

1. For **Actions**, choose **Delete volume**\.

1. In the delete dialog, do the following:

   1. Acknowledge that you understand the results of deleting the volume\.

   1. Confirm the volume deletion by entering **delete** in the **Confirm delete** field\.

1. Choose **Delete volume\(s\)**\.

### To delete a volume \(CLI\)<a name="delete-volume-cli"></a>
+ To delete an FSx for OpenZFS volume, use the [delete\-volume](https://docs.aws.amazon.com/cli/latest/reference/fsx/delete-volume.html) CLI command \(or the equivalent [DeleteVolume](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DeleteVolume.html) API operation\), as shown in the following example\.

  ```
  aws fsx delete-volume --volume-id fsxvol-0123456789abcdef1
  ```

## Viewing a volume<a name="viewing-volumes"></a>

You can see the FSx for OpenZFS volumes that are currently on your file system using the Amazon FSx console, the AWS CLI, and the Amazon FSx API and SDKs\.

**To view the volumes on your file system:**
+ **Using the console** – Choose a file system to view the **File systems** detail page\. Choose the **Volumes** tab to list all the volumes on the file system, and then choose the volume you want to view\.
+ **Using the CLI or API** – Use the [describe\-volumes](https://docs.aws.amazon.com/cli/latest/reference/fsx/describe-volumes.html) CLI command or the [DescribeVolumes](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeVolumes.html) API operation\.