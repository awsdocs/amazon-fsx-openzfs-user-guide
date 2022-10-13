# Mounting FSx for OpenZFS volumes to macOS clients<a name="attach-mac-client"></a>

**To mount an OpenZFS volume on an EC2 macOS client**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Create or select an Amazon EC2 Mac instance running the macOS that is in the same VPC as the file system\.

   For more information on launching an instance, see [ Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html#mac-instance-launch) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your Amazon EC2 Mac instance\. For more information, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Open a terminal on your EC2 Mac instance using secure shell \(SSH\), and log in with the appropriate credentials\.

1. Create a directory on the EC2 instance for mounting the volume as follows:

   ```
   sudo mkdir /localpath
   ```

1. Mount the volume using the following command\.

   ```
   sudo mount -t nfs -o resvport file-system-dns-name:/vol_path mount-point
   ```

   The following example uses sample values\.

   ```
   sudo mount -t nfs -o resvport fs-01234567890abcde5.fsx.us-east-1.amazonaws.com:/fsx/vol1 /fsx
   ```

   You can also view and copy the exact commands to mount any OpenZFS volume in the Amazon FSx console by choosing **Attach** on that volume’s details page\.