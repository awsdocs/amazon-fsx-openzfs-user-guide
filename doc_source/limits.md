# Quotas<a name="limits"></a>

Following, you can find out about quotas when working with Amazon FSx for OpenZFS\.

**Topics**
+ [Quotas that you can increase](#soft-limits)
+ [Resource quotas for each file system](#limits-openzfs-resources-file-system)

## Quotas that you can increase<a name="soft-limits"></a>

Following are the quotas for Amazon FSx for OpenZFS for each AWS account, per AWS Region, that you can increase\.


****  

| Resource | Default | Description | 
| --- | --- | --- | 
|  OpenZFS file systems  |  100  |  The maximum number of Amazon FSx for OpenZFS file systems that you can create in this account\.  | 
|  OpenZFS SSD storage capacity  |  65,536 \(262,144 in US East \(N\. Virginia\), US East \(Ohio\), and US West \(Oregon\)\)  |  The maximum amount of SSD storage capacity \(in GiB\) that you can configure for all Amazon FSx for OpenZFS file systems in this account\.  | 
|  OpenZFS throughput capacity  |  10,240  |  The total amount of throughput capacity \(in MBps\) allowed for all Amazon FSx for OpenZFS file systems in this account\.  | 
|  OpenZFS disk IOPS  |  160,000  |  The total amount of disk IOPS allowed for all Amazon FSx for OpenZFS file systems in this account\.  | 
|  OpenZFS backups  |  10,000  |  The maximum number of user\-initiated backups for all Amazon FSx for OpenZFS file systems that you can have in this account\.  | 

**To request a quota increase**

1. Open the [AWS Support](https://console.aws.amazon.com/support/home#/) page, sign in if necessary, and then choose **Create case**\.

1. For **Create case**, choose **Account and billing support**\.

1. In the **Case details** panel make the following entries:
   + For **Type** choose **Account**\.
   + For **Category** choose **Other Account Issues**\.
   + For **Subject** enter **Amazon FSx for OpenZFS service limit increase request**\.
   + Provide a detailed **Description** of your request, including:
     + The FSx quota that you want increased, and the value you want it increased to, if known\.
     + The reason why you are seeking the quota increase\.
     + The file system ID and region for each file system you are requesting an increase for\.

1. Provide your preferred **Contact options** and choose **Submit**\.

## Resource quotas for each file system<a name="limits-openzfs-resources-file-system"></a>

Following are the quotas on Amazon FSx for OpenFS resources for each file system in an AWS Region\.


****  

| Resource | Limit per file system | 
| --- | --- | 
| Minimum storage capacity | 64 GiB | 
| Maximum storage capacity | 512 TiB | 
| Minimum throughput capacity | 64 MBps | 
| Maximum throughput capacity | 4,096 MBps | 
| Maximum number of volumes | 100 | 
| Maximum number of tags | 50 | 
| Maximum retention period for automated backups | 90 days | 
| Maximum retention period for user\-initiated backups | no retention limit | 