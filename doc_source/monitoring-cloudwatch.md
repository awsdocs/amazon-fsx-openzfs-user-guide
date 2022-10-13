# Monitoring with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor file systems using Amazon CloudWatch, which collects and processes raw data from FSx for OpenZFS into readable, near real\-time metrics\. These statistics are retained for a period of 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. By default, FSx for OpenZFS metric data is automatically sent to CloudWatch at 1\-minute periods\. For more information about CloudWatch, see [What Is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) in the *Amazon CloudWatch User Guide*\.

Amazon FSx CloudWatch metrics are reported as raw *Bytes*\. Bytes are not rounded to either a decimal or binary multiple of the unit\.

FSx for OpenZFS publishes the following metrics into the `AWS/FSx` namespace in CloudWatch\. For each metric, FSx for OpenZFS emits a data point per resource \(that is, file system or volume\) per minute\.


| Metric | Description | 
| --- | --- | 
| DataReadBytes |  Number of bytes for read operations  Units: Bytes Valid statistics: `Sum`  | 
| DataWriteBytes |  Number of bytes for write operations  Units: Bytes Valid statistics: `Sum`  | 
| DataReadOperations |  Number of data read operations  Units: Count Valid statistics: `Sum`  | 
| DataWriteOperations |  Number of data write operations  Units: Count Valid statistics: `Sum`  | 
| UsedStorageCapacity |  Amount of storage used  Units: Bytes Valid statistics: `Average`, `Minimum`  | 
| StorageCapacity |  Total storage capacity, equal to the sum of used and available storage capacity  Units: Bytes Valid statistics: `Average`, `Minimum`  | 
| CompressionRatio |  Ratio of compressed storage usage to uncompressed storage usage  Valid statistics: `Average`, `Minimum`  | 
| NfsBadCalls |  Number of calls rejected by NFS server Remote Procedure Call \(RPC\) mechanism Units: Count Valid statistics: `Average`, `Minimum`  | 
| CPUUtilization |  Percentage utilization of your file server’s CPU resources Units: Percent Valid statistics: `Average`  | 
| MemoryUtilization |  Percentage utilization of your file server’s memory resources Units: Percent Valid statistics: `Average`  | 

## FSx for OpenZFS dimensions<a name="fsx-dimensions"></a>

FSx for OpenZFS metrics can be represented at both the file system or volume granularity by using the `FileSystemId` or `VolumeId`\. FSx for OpenZFS provides additional dimensions to further refine metrics listed in the previous table\.


| Metric | Description | 
| --- | --- | 
| `FileSystemId` | This dimension filters the metrics you request to an individual file system\. | 
| `VolumeId` | This dimension filters the metrics you request to an individual volume within a file system\. This dimension must be used in combination with `FileSystemId`\. | 
| `StorageTier` | The storage tier being reported\. Currently, the only valid value for FSx for OpenZFS metrics is `All`\. | 
| `DataType` | The type of data being reported\. For example, providing a value of `Snapshot` will report snapshot\-specific metrics for a file system or volume\. | 

FSx for OpenZFS metrics use the `FSx` namespace and provide metrics for the dimensions listed in the table above\.