# Managing FSx for OpenZFS file system resources<a name="managing-file-systems"></a>

A *file system* is the primary FSx for OpenZFS resource\. You can create, list, update, and delete FSx for OpenZFS file systems using the AWS Management Console, AWS CLI, and API\.

## Configurable FSx for OpenZFS file system properties<a name="fsx-openzfs-file-system-properties"></a>

When you create a file system, you specify the following file system properties:
+ **Storage capacity** – specifies the storage capacity of your file system, from range of 64 to 524288 GiB\.
+ **Provisioned SSD IOPS** – specifies the maximum number of read and write operations for your file system\. You can use the default setting of 3 IOPS per GB of SSD storage, or you can provision the SSD IOPS, to a maximum of 160,000 SSD IOPS per file system\. You pay for additional SSD IOPS that you provision above the default 3 IOPS per GB of SSD storage\.
+ **Throughput capacity** – the sustained speed at which the file server that hosts your file system can serve data, in MB per second \(MBps\)\. You can use the default Amazon FSx\-provisioned value or you can specify a different value\. You pay for additional throughput capacity that you provision above the Amazon FSx default value\.

  You can increase the amount of throughput capacity as needed at any time after you create the file system\. For more information, see [Managing throughput capacity](managing-throughput-capacity.md)\.
+ **Network and security** – You can specify the Amazon VPC \(VPC\), VPC security group, and subnet to use\.
+ **Encryption** – Amazon FSx automatically encrypts the data in your file system at rest using the Amazon FSx service AWS Key Management Service key for your AWS account by default\. You can choose to use a different KMS key\.

When you create a new file system, Amazon FSx automatically configures a root volume if you use the **Quick create** option\. You can customize the root volume configuration when creating a file system by using the **Standard create** option\. All new volumes that you create on a file system are children of the root volume\. For more information about the properties you'll use to configure the root volume, see [Volume properties](managing-volumes.md#volume-properties)\. After your file system is created, you can create additional volumes as needed to further organize your data\. For more information, see [Creating a volume](managing-volumes.md#creating-volumes)\. 