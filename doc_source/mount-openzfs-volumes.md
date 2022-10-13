# Mounting FSx for OpenZFS volumes<a name="mount-openzfs-volumes"></a>

You access the data in FSx for OpenZFS by mounting individual volumes on your client\. The commands in this section use the DNS name of the file system in which the volume is created to mount it\. To identify a file system's DNS name in the Amazon FSx console, choose **File systems**, then choose the FSx for OpenZFS file system whose volume you are mounting, and the **DNS name** is displayed in the **Network & security** panel\. Or, you can find them in the response of the [DescribeVolumes](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DescribeVolumes.html) API operation\.

**Topics**
+ [Mounting FSx for OpenZFS volumes to Linux clients](attach-linux-client.md)
+ [Mounting OpenZFS volumes to Microsoft Windows clients](attach-windows-client.md)
+ [Mounting FSx for OpenZFS volumes to macOS clients](attach-mac-client.md)
+ [Additional mounting considerations](other-mount-considerations.md)