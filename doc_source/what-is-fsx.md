# What is Amazon FSx for OpenZFS?<a name="what-is-fsx"></a>

Amazon FSx for OpenZFS is a fully managed file storage service that makes it easy to move data residing in on\-premises ZFS or other Linux\-based file servers to AWS without changing your application code or how you manage data\. It offers highly reliable, scalable, performant, and feature\-rich file storage built on the open\-source OpenZFS file system, providing the familiar features and capabilities of OpenZFS file systems with the agility, scalability, and simplicity of a fully managed AWS service\. For developers building cloud\-native applications, it offers simple, high\-performance storage with rich capabilities for working with data\.

Amazon FSx for OpenZFS file systems are broadly accessible from Linux, Windows, and macOS compute instances and containers using the industry\-standard NFS protocol \(v3, v4\.0, v4\.1, v4\.2\)\. Powered by AWS Graviton processors and the latest AWS disk and networking technologies \(including AWS Scalable Reliable Datagram networking and the AWS Nitro system\), Amazon FSx for OpenZFS delivers up to 1 million IOPS with latencies of hundreds of microseconds\. With complete support for OpenZFS features like instant point\-in\-time snapshots and data cloning, FSx for OpenZFS makes it easy for you to replace your on\-premises file servers with AWS storage that provides familiar file system capabilities and eliminates the need to perform lengthy qualifications and change or re\-architect existing applications or tools\. And, by combining the power of OpenZFS data management capabilities with the high performance and cost efficiency of the latest AWS technologies, FSx for OpenZFS enables you to build and run high\-performance, data\-intensive applications\.

As a fully managed service, FSx for OpenZFS makes it easy to launch, run, and scale fully managed file systems on AWS that replace the file servers you run on premises while helping to provide better agility and lower costs\. With Amazon FSx for OpenZFS, you no longer have to worry about setting up and provisioning file servers and storage volumes, replicating data, installing and patching file server software, detecting and addressing hardware failures, and manually performing backups\. It also provides rich integration with other AWS services, such as AWS Identity and Access Management IAM, AWS Key Management Service \(AWS KMS\), Amazon CloudWatch, and AWS CloudTrail\.

Amazon FSx for OpenZFS is available in the following AWS Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(Oregon\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Stockholm\)

## Features of Amazon FSx for OpenZFS<a name="fsx-openzfs-feature-overview"></a>

With FSx for OpenZFS, you get a fully managed file storage solution with:
+ Support for access from Linux, Windows, and macOS compute instances and containers \(running on AWS or on\-premises\) via the industry\-standard NFS protocol \(v3, v4\.0, v4\.1, and v4\.2\)\.
+ Up to 1 million IOPS with latencies of just a few hundred microseconds, and up to 12\.5 GBps of throughput for frequently accessed data \(from in\-memory cache\)\. Up to 160,000 IOPS and up to 4 GBps of uncompressed throughput and 8\-12 GBps of compressed throughput for data accessed from SSD disks\.
+ Powerful OpenZFS data management capabilities including Z\-Standard compression, near instant point\-in\-time snapshots, and data cloning, natively supported via the Amazon FSx API\.
+ Highly available and highly durable file systems\.
+ Support for multiple volumes per file system, thin provisioning, and user and group quotas for cost\-efficient shared file systems across multiple users and applications\.
+ Support for the following data protection and security features::
  + Built\-in, fully managed file system backups stored on S3, with support for cross\-region backup copies\.
  + Near\-instant point\-in time OpenZFS snapshots stored locally on each file system\.
  + Automatic encryption of file system data and backups at rest using KMS keys\.
  + Automatic encryption in\-transit when accessed from [supported EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/data-protection.html#encryption-transit)\.

## Security and data protection<a name="security-considerations"></a>

Amazon FSx provides multiple levels of security and compliance to help ensure that your data is protected\. It automatically encrypts data at rest in file systems and backups using keys that you manage in AWS Key Management Service \(AWS KMS\)\. Encryption of data in transit is automatically enabled when you access an Amazon FSx file system from [ Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/data-protection.html#encryption-transit) that support this feature\. For more information, see [Data protection in Amazon FSx for OpenZFS](data-protection.md)\.

Amazon FSx has been assessed to comply with International Organization for Standardization \(ISO\), Payment Card Industry Data Security Standard \(PCI DSS\), and System and Organization Controls \(SOC\) certifications, and is Health Insurance Portability and Accountability Act of 1996 \(HIPAA\) eligible\. For more information, see [Compliance validation for Amazon FSx for OpenZFS](fsx-openzfs-compliance.md)\.

Amazon FSx provides access control at the file system level using Amazon Virtual Private Cloud \(Amazon VPC\) security groups\. Amazon FSx provides access control at the API level using AWS Identity and Access Management \(IAM\) access policies\. To provide access control at the file and folder level, Amazon FSx supports Unix permissions\. Amazon FSx integrates with AWS CloudTrail to monitor and log your Amazon FSx API calls so that you can see actions taken by users on your Amazon FSx resources\. For more information, see [Logging FSx for OpenZFS API Calls with AWS CloudTrail](logging-using-cloudtrail-win.md)\.

Additionally, Amazon FSx protects your data with highly durable file system backups\. Amazon FSx performs automatic daily backups, and you can take additional backups at any point\. For more information, see [Protecting your Amazon FSx for OpenZFS data with backups and snapshots](protecting-data.md)\.

## Pricing for FSx for OpenZFS<a name="pricing-for-fsx-openzfs"></a>

You are billed for file systems based on the following categories:
+ SSD storage capacity \(per gigabtye\-month, or GB\-month\)
+ Throughput capacity \(per MBps\-month\)
+ Optional SSD IOPS that you provision above the default 3 IOPS/GB \(per IOPS\-month\)
+ Backup storage consumption \(per GB\-month\)

For more information about pricing and fees associated with the service, see [FSx for OpenZFS pricing](http://aws.amazon.com/fsx/openzfs/pricing/)\.

## Are you a first\-time Amazon FSx user?<a name="first-time-user"></a>

If you're a first\-time user of Amazon FSx, we recommend that you read the following sections in order:

1. If you're new to AWS, see [Setting up Amazon FSx for OpenZFS](setting-up.md) to set up an AWS account\.

1. If you're ready to create your first Amazon FSx file system, follow the instructions in [Getting started with Amazon FSx for OpenZFS](getting-started.md)\.

1. For information about performance, see [Amazon FSx for OpenZFS performancePerformance](performance.md)\.

1. For Amazon FSx security details, see [Security in Amazon FSx for OpenZFS](security.md)\.

1. For information about the Amazon FSx API, see [Amazon FSx API Reference](https://docs.aws.amazon.com/fsx/latest/APIReference/Welcome.html)\.

1. For information about the Amazon FSx AWS CLI, see [AWS Command Line Interface Command Reference for Amazon FSx](https://docs.aws.amazon.com/cli/latest/reference/fsx/index.html)\.

1. For more information about pricing and fees associated with the service, see [FSx for OpenZFS pricing](http://aws.amazon.com/fsx/openzfs/pricing/)\.