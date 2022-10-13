# Step 1: Create an Amazon FSx for OpenZFS file system<a name="getting-started-step1"></a>

The Amazon FSx console has two options for creating a file system – a **Quick create** option and a **Standard create** option\. To rapidly and easily create an Amazon FSx for OpenZFS file system with the service recommended configuration, use the **Quick create** option\.

The **Quick create** option creates a file system with a default root volume configuration\. This configuration specifies an `NFS exports` setting in which **Client addresses** is an asterisk \(`*`\) and **NFS options** is `rw,crossmnt`\. With these settings, any clients permitted by your VPC and security group settings can access the volume with read and write permissions\. After your file system is created, you can create additional volumes as needed\.

For information about using the **Standard create** option to create a file system with a customized configuration, see [Creating FSx for OpenZFS file systems](creating-file-systems.md)\.

**To create your file system**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. On the dashboard, choose **Create file system** to start the file system creation wizard\.

1. On the **Select file system type** page, choose **Amazon FSx for OpenZFS**, and then choose **Next**\. The **Create OpenZFS file system** page appears\. For **Creation method**, choose **Quick create**\. To create a file system using the **Standard create** method, see [Creating FSx for OpenZFS file systems](creating-file-systems.md)\.  
![\[Quick create file system details screen showing options for the file system name and storage capacity.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/create-fs-quick.png)

1. In the **Quick configuration** section, for **File system name \- optional**, enter a name for your file system\. It's easier to find and manage your file systems when you name them\. You can use a maximum of 256 Unicode letters, white space, and numbers, plus these special characters: **\+** **\-** \(hyphen\) **=** **\.** **\_** \(underscore\) **:** **/**

1. For **SSD storage capacity**, specify the storage capacity of your file system, in gibibytes \(GiBs\)\. Enter any whole number in the range of 64–524288\.

1. For **Virtual Private Cloud \(VPC\)**, choose the Amazon VPC that you want to associate with your file system\.

1. For **Subnet**, choose the subnet in which your file system's elastic network interface resides\.

1. Choose **Next**\.

1. <a name="step_default_settings"></a>Review the file system configuration shown on the **Create OpenZFS file system** page\. For your reference, note which file system settings you can modify after the file system is created\.

1. Choose **Create file system**\.

**Quick create** creates a file system with one root volume named `fsx`\. The volume has a path of `/fsx`, and a record size of 128 KiB\. The file system data is encrypted at rest using your default service managed AWS KMS key, named `aws/fsx/ (default)`\. For more information about file system settings, see [File system properties](managing-file-systems.md#fsx-openzfs-file-system-properties)\. For more information about volume and root volume properties, see [Volume properties](managing-volumes.md#volume-properties)\.