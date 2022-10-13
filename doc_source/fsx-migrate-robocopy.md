# How to migrate existing files to Amazon FSx using Robocopy<a name="fsx-migrate-robocopy"></a>

Robocopy is designed to replicate data between two locations that are locally accessible on the same host\. To use Robocopy to migrate data to your FSx for OpenZFS file system, you need to mount the source file system and the destination OpenZFS volume on the same Windows\-based EC2 client instance\. The following procedure outlines the necessary steps to perform this migration using a new EC2 instance\.

**To migrate existing files to Amazon FSx**

1. Launch a Windows Server 2016 Amazon EC2 instance in the same Amazon VPC as that of your Amazon FSx file system\.

1. Connect to your Amazon EC2 instance\. For more information, see [Connect to Your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Open **Command Prompt** and map the source file share on your existing file server \(on\-premises or in AWS\) to a drive letter \(for example, *Y*:\) as follows\. As part of this, you provide credentials for a member of your on\-premises Active Directory's **Domain Administrators** group\.

   ```
   C:\>net use Y: \\fileserver1.mydata.com\localdata /user:mydata.com\Administrator
   Enter the password for ‘fileserver1.mydata.com’: _
   
   Drive Y: is now connected to \\fileserver1.mydata.com\localdata.
   
   The command completed successfully.
   ```

1. Map the target file share on your Amazon FSx file system to a different drive letter \(for example, *Z*:\) on your Amazon EC2 instance using the [Mounting on a Windows EC2 client](attach-windows-client.md) instructions\.

1. Choose **Run as Administrator** from the context menu\. Open **Command Prompt** or **Windows PowerShell** as an administrator, and run the following Robocopy command to copy all the files on the source share to the target share\. The example command uses the following elements and options:
   + Y – Refers to the source share located in the on\-premises Active Directory forest mydata\.com\.
   + Z – Refers to the target share \\\\amznfsxabcdef1\.mydata\.com\\share on Amazon FSx\.
   + /copy – Specifies the following file properties to be copied: 
     + D – data
     + A – attributes
     + T – timestamps
   + /e – Copies subdirectories, including empty ones\.
   + /b – Uses the backup and restore privilege in Windows to copy files even if their NTFS ACLs deny permissions to the current user\.
   + /MT:8 – Specifies how many threads to use for performing multithreaded copies\.

   ```
   robocopy Y:\ Z:\ /copy:DAT /e /b /MT:8
   ```

**Note**  
If you are copying large files over a slow or unreliable connection, you can enable restartable mode by using the /zb option in place of the /b option\. With restartable mode, if the transfer of a large file is interrupted, a subsequent Robocopy operation can pick up in the middle of the transfer instead of having to re\-copy the entire file from the beginning\. Using the restartable mode can reduce data transfer speeds\.