# Mounting OpenZFS volumes to Microsoft Windows clients<a name="attach-windows-client"></a>

Mounting OpenZFS volumes to Windows clients leverages the NFSv3 protocol\. The following instructions include the necessary steps to install the NFS client on your Windows\-based EC2 instance\.

**To mount an OpenZFS volume on an EC2 Windows client**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Create or select an Amazon EC2 instance running Microsoft Windows that is in the same VPC as the file system\.

   For more information on launching an instance, see [ Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-launch-instance) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Connect to your Amazon EC2 Windows instance\. For more information, see [ Connecting to your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Open PowerShell as an administrator, and install the NFS client\.

   ```
   Install-WindowsFeature -Name NFS-Client
   ```

   If prompted to do so, restart and reconnect to your Windows instance\.

1. Open a command prompt window with standard user privileges\. If you run the mount command as Administrator, the mounted drive will not appear in File Explorer\.
**Note**  
To ensure this mounted drive appears in File Explorer, please open the Command Prompt window with standard user privileges\. If you run this command as Administrator, it will not appear in File Explorer\.

1. You can mount the drive using a command prompt, or using a Powershell path

   1. Mount the volume to any available drive letter by running the following command, replacing Z: with any available drive letter:
      + Replace *filesystem\-dns\-name* with the DNS name or the IP address of the file system\.
      + Replace *vol\_path* with the path of the FSx for OpenZFS volume you are trying to mount\.
      + Replace *Z:* with any available drive letter\.

      ```
      mount \\filesystem-dns-name\vol_path Z:
      ```

      The following example uses sample values\.

      ```
      mount \\fs-01234567890abcdef1.fsx.us-east-1.amazonaws.com\fsx\vol1 Z:
      ```

   1. You can also view and copy the exact commands to mount any OpenZFS volume in the Amazon FSx console by choosing **Attach** on that volume’s details page\.

      You can also mount the file system using the following Powershell path:

      ```
      New-PSDrive -Name "Z" -PSProvider "FileSystem" -Root "\\filesystem-dns-name\" -Persist
      ```

      The following example uses a sample file system DNS name\.

      ```
      New-PSDrive -Name "Z" -PSProvider "FileSystem" -Root "\\fs-0239c0e31af65bff1.fsx.us-east-1.amazonaws.com\fsx\" -Persist
      ```