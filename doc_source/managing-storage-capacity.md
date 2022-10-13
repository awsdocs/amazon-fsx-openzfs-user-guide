# Managing SSD storage capacity and provisioned IOPS<a name="managing-storage-capacity"></a>

When you need additional storage for your dataset, you can increase the solid state drive \(SSD\) storage capacity of your Amazon FSx for OpenZFS file system without any disruption to your end users or applications\. You can do so by using the Amazon FSx console, Amazon FSx API, or AWS Command Line Interface \(AWS CLI\)\.

You can also change the provisioned SSD IOPS for your file system, either when you increase SSD storage capacity or as an independent action\. To specify the amount of provisioned SSD IOPS for your file system, you use one of two IOPS modes:
+ **Automatic** mode if you want Amazon FSx to automatically scale your SSD IOPS\.
+ **User\-provisioned** mode if you want to provision a specific amount of SSD IOPS\.

 For more information about these modes, see [Considerations when updating storage and IOPS](#scaling-considerations)\.

When you increase the SSD storage capacity of your Amazon FSx file system, the new capacity is available for use within minutes\. You can update the SSD storage capacity or SSD IOPS at anytime, as long as storage capacity increases are at least 6 hours apart\. These updates do not impact the availability of your file system in any way\. You will be billed for the new SSD storage capacity after it becomes available to you\. For more information, see [Amazon FSx for OpenZFS Pricing](http://aws.amazon.com/fsx/openzfs/pricing/)\.

 You can track the progress of an SSD storage capacity increase or SSD IOPS update at any time by using the Amazon FSx console, CLI, and API\. For more information, see [Monitoring storage capacity and IOPS updates](#monitoring-storage-capacity-increase)\.

Once the increased SSD capacity is available, if the file system's root volume storage capacity quota is set to the same size as the file system, FSx will automatically update the volume's storage capacity quota to match the newly\-increased file system capacity\. Otherwise, you need to manually increase the `storage capacity quota` of the root volume, and any other volumes in your file system\. For more information, see [Updating a volume configuration](managing-volumes.md#updating-volumes)\.

**Topics**
+ [Considerations when updating storage and IOPS](#scaling-considerations)
+ [When to increase storage capacity](#when-to-modify-storage-capacity)
+ [How to update SSD storage capacity and provisioned IOPS](#increase-storage-capacity)
+ [Monitoring storage capacity and IOPS updates](#monitoring-storage-capacity-increase)

## Considerations when updating storage and IOPS<a name="scaling-considerations"></a>

Here are a few important considerations when modifying your SSD storage capacity and provisioned IOPS:
+ **Storage capacity increase only** – You can only *increase* the amount of SSD storage capacity for a file system; you cannot decrease the storage capacity\.
+ **Storage capacity minimum increase** – Each SSD storage capacity increase must be a minimum of 10 percent of the file system's current SSD storage capacity, up to the maximum allowed value of 512 Tebibytes \(TiB\)\.
+ **Time between increases** – You can't make further SSD storage capacity increases on a file system until 6 hours after the last increase was requested\.
+ **Allocating increased storage capacity** – If the file system's root volume storage capacity quota is set to the same size as the file system, FSx will automatically update the volume's storage capacity quota to match the newly\-increased file system capacity\. Otherwise, you will need to manually [increase storage](managing-volumes.md#updating-volumes) on the root volume and any other volumes in your file system\.
+ **Provisioned IOPS modes** – For a provisioned IOPS change, you must specify a mode\. The two IOPS modes are the following:
  + **Automatic** mode – Amazon FSx automatically scales your SSD IOPS to maintain 3 SSD IOPS per GiB of storage capacity, up to 160,000 SSD IOPS per file system\.
  + **User\-provisioned** mode – You specify the number of SSD IOPS, which must be greater than or equal to 3 IOPS per GiB of storage capacity\. If the amount of SSD IOPS is not at least 3 IOPS per GiB, the request will fail\. You can optionally provision a higher level of IOPS\. If you do so, you pay for the average IOPS provisioned above 3 IOPS per GiB per file system\.
**Note**  
For file systems that are already configured with **User\-provisioned** SSD IOPS, you must specify a value for **User\-provisioned** SSD IOPS when you are updating your file system, or the update request will fail\.

## When to increase storage capacity<a name="when-to-modify-storage-capacity"></a>

If you are running out of SSD storage, we recommend that you increase the storage capacity of your file system\. You can monitor SSD storage capacity on the file system using these file system\-level Amazon CloudWatch metrics\.
+ `StorageCapacity` measure the total amount of file system SSD storage capacity\.
+ `UsedStorageCapacity` measures the amount of used SSD storage capacity\.

You can use these metrics to measure storage capacity and create alarms\. The following are some examples:
+ Per cent storage capacity used = `UsedStorageCapacity` ÷ `StorageCapacity`
+ Per cent storage capacity available = `StorageCapacity` ÷ `UsedStorageCapacity`
+ Amount of free storage capacity = `StorageCapacity` − `UsedStorageCapacity`

You can create a CloudWatch alarm on a metric and get notified when it drops below a specific threshold\. For more information, see [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)\.

## How to update SSD storage capacity and provisioned IOPS<a name="increase-storage-capacity"></a>

You can increase a file system's SSD storage capacity and modify your provisioned SSD IOPS by using the Amazon FSx console, the AWS CLI, or the Amazon FSx API\.

### To update SSD storage capacity and provisioned IOPS for a file system \(console\)<a name="increase-storage-console"></a>

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. In the left navigation pane, choose **File systems**\. In the **File systems** list, choose the FSx for OpenZFS file system that you want to update SSD storage capacity and SSD IOPS for\.

1. On the **Summary** panel, choose **Update** next to the file system's **SSD storage capacity** value\.

   The **Update SSD storage capacity and IOPS** dialog box appears\.

1. To increase SSD storage capacity, select **Modify storage capacity**\.

1. For **Input type**, choose one of the following: 
   + To enter the new SSD storage capacity as a percentage change from the current value, choose **Percentage**\.

     For **Desired % increase**, enter the percentage by which you want to increase storage capacity\. This value must be at least 10 percent\.
   + To enter the new value in GiB, choose **Absolute**\.

     For **Desired storage capacity**, enter the new value for SSD storage capacity value in GiB, up to the maximum allowed value of 512 TiB\.

1. For **Provisioned SSD IOPS**, you have two options to modify the number of provisioned SSD IOPS for your file system:
   + If you want Amazon FSx to automatically scale your SSD IOPS to maintain 3 provisioned SSD IOPS per GiB of primary storage capacity \(up to a maximum of 160,000\), choose **Automatic**\.
   + If you want to specify the number of SSD IOPS, choose **User\-provisioned**\. Enter an absolute number of IOPS that is at least 3 times the amount of GiB of your primary storage tier, and less than or equal to 160,000\.

1. Choose **Update**\.

### To update SSD storage capacity and provisioned IOPS for a file system \(CLI\)<a name="increase-storage-cli"></a>

To update the SSD storage capacity and provisioned IOPS for an FSx for OpenZFS file system, use the AWS CLI command [update\-file\-system](https://docs.aws.amazon.com/cli/latest/reference/fsx/update-file-system.html) \(UpdateFileSystem is the equivalent API action\)\. Set the following parameters:
+ Set `--file-system-id` to the ID of the file system that you are updating\.
+ To increase your SSD primary storage capacity, set `--storage-capacity` to a value that is at least 10 percent greater than the current value\.
+ To modify your provisioned SSD IOPS, use the `--open-zfs-configuration DiskIopsConfiguration` property\. This property has two parameters, `Iops` and `Mode`:
  + If you want to specify the number of provisioned SSD IOPS, use `Iops=number_of_IOPS` \(up to a maximum of 160,000\) and `Mode=USER_PROVISIONED`\. The SSD IOPS value must be greater than or equal to 3 times the requested SSD storage capacity\. If you're not increasing the storage capacity, the IOPs value must be greater than or equal to 3 times the current SSD storage capacity\.
  + If you want Amazon FSx to automatically increase your SSD IOPS, use `Mode=AUTOMATIC` and don't use the `Iops` parameter\. Amazon FSx will automatically maintain 3 provisioned SSD IOPS per GiB of your primary storage capacity \(up to a maximum of 160,000\)\.

The following example requests an increase of 2000 GiB to the file system's SSD storage capacity\. It also requests 7000 provisioned SSD IOPS\.

```
aws fsx update-file-system \
    --file-system-id fs-0123456789abcdef0 \
    --storage-capacity 2000 \
    --open-zfs-configuration 'DiskIopsConfiguration={Iops=7000,Mode=USER_PROVISIONED}'
```

To monitor the progress of the update, use the [describe\-file\-systems](https://docs.aws.amazon.com/cli/latest/reference/fsx/describe-file-systems.html) AWS CLI command\. Look for the `AdministrativeActions` section in the output\.

For more information, see [AdministrativeAction](https://docs.aws.amazon.com/fsx/latest/APIReference/API_AdministrativeAction.html) in the *Amazon FSx for OpenZFS API Reference*\.

## Monitoring storage capacity and IOPS updates<a name="monitoring-storage-capacity-increase"></a>

You can monitor the progress of an SSD storage capacity and IOPS update by using the Amazon FSx console, the API, or the AWS CLI\.

### Monitoring updates in the console<a name="monitor-storage-action-console"></a>

You can monitor file system updates in the **Updates** tab on the **File system details** page\.

![\[Image of the Amazon FSx Console showing the list of recent updates for a file system.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/fs-updates-panel.png)

For SSD storage capacity and IOPS updates, you can view the following information:

****Update type****  
Supported types are **Storage capacity**, **IOPS Mode**, and **SSD IOPS**\. The **IOPS Mode** and **SSD IOPS** values are listed for all storage capacity and IOPS scaling requests\. 

****Target value****  
The updated value for the file system's SSD storage capacity or IOPs\.

****Status****  
The current status of the update\. The possible values are as follows:  
+ **Pending** – Amazon FSx has received the update request, but has not started processing it\.
+ **In progress** – Amazon FSx is processing the update request\.
+ **Completed** – The update finished successfully\.
+ **Failed** – The update request failed\. Choose the question mark \(**?**\) to see details on why the request failed\.

****Request time****  
The time that Amazon FSx received the update action request\.

### Monitoring increases with the AWS CLI and API<a name="monitor-storage-action-cli-api"></a>

You can view and monitor file system SSD storage capacity increase requests using the [describe\-file\-systems](https://docs.aws.amazon.com/cli/latest/reference/fsx/describe-file-systems.html) AWS CLI command and the [DescribeFileSystems](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeFileSystems.html) API operation\. The `AdministrativeActions` array lists the 10 most recent update actions for each administrative action type\. When you increase a file system's SSD storage capacity, a `FILE_SYSTEM_UPDATE` `AdministrativeActions` is generated\.

The following example shows an excerpt of the response of a `describe-file-systems` CLI command\. The file system has a pending administrative action to increase the SSD storage capacity to 2000 GiB and the provisioned SSD IOPS to 7000\.

```
"AdministrativeActions": [
    {
        "AdministrativeActionType": "FILE_SYSTEM_UPDATE",
        "RequestTime": 1586797629.095,
        "Status": "PENDING",
        "TargetFileSystemValues": {
            "StorageCapacity": 2000,
            "OpenZFSConfiguration": {
                "DiskIopsConfiguration": {
                    "Mode": "USER_PROVISIONED",
                    "Iops": 7000
                }
            }
        }
    }
]
```

Amazon FSx processes the `FILE_SYSTEM_UPDATE` action, increasing the file system's storage capacity\. When the new storage is available to the file system, the `FILE_SYSTEM_UPDATE` status changes to `COMPLETED`\. The storage capacity shows the new larger value\. This behavior is shown in the following excerpt of the response of a `describe-file-systems` CLI command\. 

```
"AdministrativeActions": [
    {
        "AdministrativeActionType": "FILE_SYSTEM_UPDATE",
        "RequestTime": 1586799169.445,
        "Status": "UPDATED_OPTIMIZING",
        "TargetFileSystemValues": {
            "StorageCapacity": 2000,
            "OpenZFSConfiguration": {
                "DiskIopsConfiguration": {
                    "Mode": "USER_PROVISIONED",
                    "Iops": 7000
                }
            }
        }
    }
]
```

 If the storage capacity or IOPS update request fails, the status of the `FILE_SYSTEM_UPDATE` action changes to `FAILED`, as shown in the following example\. The `FailureDetails` property provides information about the failure\.

```
"AdministrativeActions": [
    {
        "AdministrativeActionType": "FILE_SYSTEM_UPDATE",
        "RequestTime": 1586373915.697,
        "Status": "FAILED",
        "TargetFileSystemValues": {
            "StorageCapacity": 2000,
            "OpenZFSConfiguration": {
                "DiskIopsConfiguration": {
                    "Mode": "USER_PROVISIONED",
                    "Iops": 7000
                }
            }
        },
        "FailureDetails": {
            "Message": "failure-message"
        }
    }
]
```