# Mounting FSx for OpenZFS volumes to Linux clients<a name="attach-linux-client"></a>

## To mount an FSx for OpenZFS volume on a Linux client<a name="one-time-mount-linux"></a>

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Create or select a Linux\-based Amazon EC2 instance that is in the same VPC as the file system\.

   For more information on launching an EC2 Linux instance, see [ Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Connect to your Amazon EC2 Linux instance\. For more information, see [Connect to your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Open a terminal on your EC2 instance using secure shell \(SSH\), and log in with the appropriate credentials\.

1. If you are using CentOS, RedHat, or Ubuntu, install the NFS client\. This step is not necessary if you are using the latest version of the Amazon Linux 2\.

   1. For CentOS and RedHat use the following command: sudo yum –y install nfs\-utils

   1. For Ubuntu use this command: sudo apt\-get \-y install nfs\-common

1. Create a directory on the EC2 instance for mounting the OpenZFS volume as follows:

   ```
   sudo mkdir /localpath
   ```

1. Mount the volume using the following command\.

   ```
   sudo mount -t nfs filesystem_dns_name:/volume_path /localpath
   ```

   The following example uses sample values\. Replace volume\-path with the path of the volume to mount\. For example, use /fsx to mount the root volume or a full volume path such as /fsx/vol1 to mount the vol1 volume within your root volume\.

   ```
   sudo mount -t nfs fs-01234567890abcdef1.fsx.us-east-1.amazonaws.com:/fsx/vol1 /fsx
   ```

   You can also view and copy the exact commands to mount any OpenZFS volume in the Amazon FSx console by choosing **Attach** on that volume’s details page\.

## To automatically mount FSx for OpenZFS file systems on instance reboot using /etc/fstab<a name="mount-fs-auto-mount-update-fstab"></a>

To automatically remount your FSx for OpenZFS volume that's mounted on an Amazon EC2 Linux instance when the instance reboots, use the `/etc/fstab` file\. The `/etc/fstab` file contains information about file systems\. The command `mount -a`, which runs during instance start\-up, mounts the file systems listed in `/etc/fstab`\.

**Note**  
FSx for OpenZFS file systems do not support automatic mounting using `/etc/fstab` on Amazon EC2 Mac instances\.

**Note**  
Before you can update the `/etc/fstab` file of your EC2 instance, make sure that you already created your FSx for OpenZFS file system\. For more information, see [Creating FSx for OpenZFS file systems](creating-file-systems.md)\.

1. Connect to your EC2 instance:
   + To connect to your instance from a computer running macOS or Linux, specify the \.pem file for your SSH command\. To do this, use the `-i` option and the path to your private key\.
   + To connect to your instance from a computer running Windows, you can use either MindTerm or PuTTY\. To use PuTTY, install it and convert the \.pem file to a \.ppk file\.

   For more information, see the following topics in the *Amazon EC2 User Guide for Linux Instances*:
   +  [Connecting to your Linux instance using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
   +  [Connecting to your Linux instance from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) 

1. Create a local directory that will be used to mount the FSx for OpenZFS volume\.

   ```
   sudo mkdir /fsx
   ```

1. Open the `/etc/fstab` file in an editor of your choice\.

1. Add the following line to the `/etc/fstab` file\. Insert a tab character between each parameter\. It should appear as one line with no line breaks\.

   ```
   filesystem-dns-name:volume-path /localpath nfs nfsver=version defaults 0 0
   ```

   The last three parameters indicate NFS options \(which we set to default\), dumping of file system and filesystem check \(these are typically not used so we set them to 0\)\.

1. Save the changes to the file\.

1. Test the fstab entry by using the mount command with the `fake` `all` `verbose` options\.

   ```
   sudo mount -fav
                           fs-dns-name:/vol_path      : successfully mounted
   ```

   Now mount the volume using the following command\. The next time the EC2 instance restarts, the volume will be mounted automatically\.

   ```
   sudo mount /localpath
   sudo mount filessystem-dns-name:/volume-path
   ```

Your EC2 instance is now configured to mount the OpenZFS volume whenever it restarts\.