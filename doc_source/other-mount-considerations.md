# Additional mounting considerations<a name="other-mount-considerations"></a>

To improve the performance of your file system, you can also include the following options when mounting your file system\. 
+ `rsize=1048576` – Sets the maximum number of bytes of data that the NFS client can receive for each network READ request\. This value applies when reading data from a file on an FSx for OpenZFS volume\. We recommend that you use the largest size possible, 1048576\. Due to lower memory capacity on file systems with 64 MB/s and 128 MB/s of provisioned throughput, these file systems will only accept a maximum rsize of 262144 and 524288 bytes, respectively\.
+ `wsize=1048576` – Sets the maximum number of bytes of data that the NFS client can send for each network WRITE request\. This value applies when writing data to a file on an FSx for OpenZFS volume\. We recommend that you use the largest size possible, 1048576\. Due to lower memory capacity on file systems with 64 MB/s and 128 MB/s of provisioned throughput, these file systems will only accept a maximum wsize of 262144 and 524288 bytes, respectively\.
+ `timeo=600` – Sets the timeout value that the NFS client uses to wait for a response before it retries an NFS request to 600 deciseconds \(60 seconds\)\.
+ `_netdev` – When present in `/etc/fstab`, prevents the client from attempting to mount the FSx for OpenZFS volume until the network has been enabled\.

The following example uses sample values\.

```
sudo mount -t nfs -o rsize=1048576,wsize=1048576,timeo=600 fs-01234567890abcdef1.fsx.us-east-1.amazonaws.com:/fsx/vol1 /fsx
```