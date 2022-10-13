# Accessing data: supported clients and environments<a name="supported-fsx-clients"></a>

You can access your Amazon FSx file systems using a variety of supported clients in the AWS Cloud and on premise environments\.

You can access the data on your Amazon FSx for OpenZFS file systems using a wide variety of compute instances and operating systems using the Network File System \(NFS\) protocol \(v3, v4\.0, v4\.1 and v4\.2\)\. including Amazon Elastic Compute Cloud \(Amazon EC2\) instances running Linux, \(including Amazon Linux, Amazon Linux 2\), Microsoft Windows, and MacOS;\. For more information, see [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)

You can also access data on your FSx for OpenZFS file systems using other AWS services\. including Amazon Elastic Container Service and Amazon Elastic Kubernetes Service\. For more information, see [Using FSx for OpenZFS with AWS container services](openzfs-integrations.md)\.

Once mounted, FSx for OpenZFS file systems appear as a local directory or drive letter over NFS, providing fully managed, shared network file storage that can be simultaneously accessed by up to thousands of clients\.

**Topics**
+ [Accessing data on FSx for OpenZFS volumes](access-environments.md)
+ [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)
+ [Using FSx for OpenZFS with AWS container services](openzfs-integrations.md)
+ [Using FSx for OpenZFS with Autodesk Flame on AWS](use-fsxZ-autodesk.md)