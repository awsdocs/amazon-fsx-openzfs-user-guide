# Accessing data on FSx for OpenZFS volumes<a name="access-environments"></a>

Following, you can find information about how to access your FSx for OpenZFS file systems\.

Amazon VPC enables you to launch AWS resources into a virtual network that you've defined\. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS\. For more information, see [What is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon Virtual Private Cloud User Guide*\.

**Topics**
+ [Accessing data within the AWS Cloud](#access-within-aws)
+ [Access from on\-premises](#access-fsxopenzfs-onprem)

## Accessing data within the AWS Cloud<a name="access-within-aws"></a>

Each Amazon FSx file system is associated with a Virtual Private Cloud \(VPC\)\. You can access your FSx for OpenZFS file system from anywhere in the same VPC within which it is deployed regardless of the Availability Zone \(AZ\)\. You can also access your file system from other VPCs\. These VPCs can be in different accounts or regions\. In addition to any requirements listed in the following sections for accessing FSx for OpenZFS resources, you also need to ensure that your file system's VPC security group has the correct settings\. It needs to allow data to flow between your file system and any clients that connect to it\. For more information, see [Amazon VPC security groups](limit-access-security-groups.md#fsx-vpc-security-groups)\.

### Access from within the same VPC<a name="same-vpc-access"></a>

When you create your Amazon FSx for OpenZFS file system, you select the Amazon VPC in which it is located\. All volumes associated with the FSx for OpenZFS file system are also located in the same VPC\. When the file system and the client mounting the volume are located in the same VPC and AWS account, you can mount a volume using the file system's DNS name over the NFS protocol\. For more information, see [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)\.

You can achieve better performance and avoid data transfer charges between Availability Zones by accessing an FSx for OpenZFS volume using a client that is located in the same Availability Zone as the file system's subnet\. To identify a file system's subnet, in the Amazon FSx console, choose **File systems**, then choose the FSx for OpenZFS file system whose volume you are mounting, and the subnet is displayed in the **Subnet** panel\.

### Access from a different VPC<a name="vpc-peering"></a>

You can access your FSx for OpenZFS file system from compute instances in a different VPC, AWS account, or AWS Region from that associated with your file system\. To do so, you can use VPC peering or transit gateways\. When you use a VPC peering connection or transit gateway to connect VPCs, compute instances that are in one VPC can access Amazon FSx file systems in another VPC\. This access is possible even if the VPCs belong to different AWS accounts, and even if the VPCs reside in different AWS Regions\. 

A *VPC peering connection* is a networking connection between two VPCs that you can use to route traffic between them using private IPv4 or IP version 6 \(IPv6\) addresses\. You can use VPC peering to connect VPCs within the same AWS Region or between AWS Regions\. For more information on VPC peering, see [What is VPC peering?](vpc/latest/peering/what-is-vpc-peering.html) in the *Amazon Virtual Private Cloud VPC Peering Guide*\.

A *transit gateway* is a network transit hub that you can use to interconnect your VPCs and on\-premises networks\. For more information about using VPC transit gateways, see [ How transit gateways work](vpc/latest/tgw/how-transit-gateways-work.html) in the *Amazon Virtual Private Cloud Transit Gateway Guide*\.

After you set up a VPC peering or transit gateway connection, you can access your file system using its DNS name\. You do so just as you do from compute instances within the associated VPC\.

## Access from on\-premises<a name="access-fsxopenzfs-onprem"></a>

FSx for OpenZFS supports the use of AWS Direct Connect or AWS VPN to access your file systems from your on\-premises compute instances\. Using AWS Direct Connect, you to access your file system over a dedicated network connection from your on\-premises environment\. Using AWS VPN, you to access your file system from your on\-premises devices over a secure and private tunnel\.

After you connect your on\-premises environment to the VPC associated with your Amazon FSx file system, you can access your file system using its DNS name or a DNS alias\. You do so just as you do from compute instances within the VPC\. For more information about AWS Direct Connect, see [What is AWS Direct Connect?](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) in the *AWS Direct Connect User Guide*\. For more information on setting up AWS VPN connections, see [VPN connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html) in the *Amazon VPC User Guide*\.