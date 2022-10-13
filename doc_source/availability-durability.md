# Availability and durability<a name="availability-durability"></a>

FSx for OpenZFS file systems are designed to be highly durable and have an availability SLA \(Service Level Agreement\) of 99\.99%\.

With Single\-AZ file systems, Amazon FSx automatically replicates your data within an Availability Zone \(AZ\) to protect it from component failure\. It continuously monitors for hardware failures and automatically replaces infrastructure components in the event of a failure\.

FSx for OpenZFS also offers a native backups feature, designed to support archival, data retention, and compliance needs\. A backup is a secondary, offline copy of your file system\. Amazon FSx backups are crash\-consistent and are also incremental, which means that only the changes after your most recent backup are saved, thus saving on backup storage costs by not duplicating data\. By default, Amazon FSx takes an automatic backup of your file system each day during a backup window that you specify\. You can create additional backups at any time using the AWS Management Console, AWS Command Line Interface, or Amazon FSx API\.

## Working with file system resources<a name="single-multi-az-resources"></a>

### Subnets<a name="fs-subnets"></a>

When you create a VPC, it spans all the Availability Zones \(AZs\) in the Region\. Availability Zones are distinct locations that are engineered to be isolated from failures in other Availability Zones\. After creating a VPC, you can add one or more subnets in each Availability Zone\. The default VPC has a subnet in each Availability Zone\. Each subnet must reside entirely within one Availability Zone and cannot span zones\. When you create a Single\-AZ Amazon FSx file system, you specify a single subnet for the file system\. The subnet you choose defines the Availability Zone in which the file system is created\.

### File system elastic network interfaces<a name="file-system-eni"></a>

 When you create an Amazon FSx file system, Amazon FSx provisions an [elastic network interface](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_ElasticNetworkInterfaces.html) in the [Amazon Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) that you associate with your file system\. The network interface allows your client to communicate with the FSx for OpenZFS file system\. The network interface is considered to be within the service scope of Amazon FSx, despite being part of your account's VPC\.

**Warning**  
You must not modify or delete the elastic network interfaces associated with your file system\. Modifying or deleting the network interface can cause a permanent loss of connection between your VPC and your file system\.

Following is a summary of the subnet, elastic network interface, and IP address resources for FSx for OpenZFS file systems:
+ Number of subnets: 1
+ Number of elastic network interfaces: 1
+ Number of IP addresses per ENI: 1

Once a file system is created, its IP addresses don't change until the file system is deleted\.

**Important**  
Amazon FSx doesn't support accessing file systems from, or exposing file system to the public Internet\. If an Elastic IP address, which is a public IP address reachable from the Internet, is attached to a file system's elastic network interface, Amazon FSx automatically detaches it\.