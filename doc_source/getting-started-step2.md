# Step 2: Mounting your file system from an Amazon EC2 Linux instance<a name="getting-started-step2"></a>

You can mount your file system from an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. This procedure uses an instance running Amazon Linux 2\.

**To mount your file system from Amazon EC2**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Create or select an Amazon EC2 instance running Amazon Linux 2 that is in the same virtual private cloud \(VPC\) as your file system\. For more information about launching an instance, see [ Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your Amazon EC2 Linux instance\. For more information, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Open a terminal on your Amazon EC2 instance using secure shell \(SSH\), and log in with the appropriate credentials\.

1. Make a directory on your Amazon EC2 instance for the volume's local mount path with the following command\. In the following example, replace *fsx* with your desired location\.

   ```
   $ sudo mkdir /fsx
   ```

1. Use the following `mount` command to mount your Amazon FSx for OpenZFS file system to the directory that you created\. Replace the following:
   + Replace `nfs-version` with an NFS protocol version, such as `4.2`\.
   + Replace `fs-dns-name` with the DNS name or the IP address of the file system\.
   + Replace `volume-path` with the path of the volume to mount\. For example, use `/fsx` to mount the root volume or a path such as `/fsx/sales` to mount the top\-level `fsx/sales` directory\.
   + Replace `local-mount-path` with the directory path of your local mount path, such as `/fsx` for the directory you created in step 5\.

   ```
   sudo mount -t nfs -o nfsvers=nfs-version fs-dns-name:volume-path local-mount-path
   ```

   The following example uses sample values\.

   ```
   sudo mount -t nfs -o nfsvers=4.2 fs01234567.fsx.us-east-1.amazonaws.com:/fsx /fsx
   ```

   You can also use the IP address of the file system instead of its DNS name\.

   ```
   sudo mount -t nfs -o nfsvers=4.2 198.51.100.5:/fsx /fsx
   ```

If you have issues with your Amazon EC2 instance \(such as connections timing out\), see [Troubleshoot EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html) in the *Amazon EC2 User Guide for Linux Instances*\.

For more information on mounting FSx for OpenZFS file systems from Linux, macOS, or Windows instances, see [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)\.