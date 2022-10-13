# Amazon FSx for OpenZFS performance<a name="performance"></a>

Amazon FSx for OpenZFS provides simple, high\-performance file storage that delivers up to 12\.5 GBps of throughput and 1 million IOPS, with latencies of a few hundred microseconds\. In this section, we provide an overview of FSx for OpenZFS performance and describe how your file system configuration impacts key performance dimensions\. We also include some important tips and recommendations for maximizing the performance of your file system\.

**Topics**
+ [How FSx for OpenZFS file systems work](#perf-overview)
+ [File system performance](#zfs-fs-performance)
+ [Tips for maximizing performance](#performance-tips-zfs)
+ [Monitoring performance](#measure-performance-cw)

## How FSx for OpenZFS file systems work<a name="perf-overview"></a>

Each FSx for OpenZFS file system consists of the file server that clients communicate with, and a set of disks attached to that file server\. Each file server employs a fast, in\-memory cache to enhance performance for the most frequently accessed data\. FSx for OpenZFS utilizes an Adaptive Replacement Cache \(ARC\) that is built into the OpenZFS file system, which improves the portion of data access driven from the in\-memory cache\.

When a client accesses data that's stored in the in\-memory cache, the file server doesn't need to read it from disk and the data is served directly from file server memory to the requesting client as *network I/O*\. When a client accesses data that is not in file server memory, it is read from disk as *disk I/O* and then served to the client as network I/O; data read from disk is also subject to the IOPS and bandwidth limits of the underlying disks\.

FSx for OpenZFS file systems can serve network I/O about three times faster than disk I/O, which means that clients can drive greater throughput and IOPS with lower latencies for frequently accessed data in cache\. The following diagram illustrates how data is accessed from an FSx for OpenZFS file system\.

![\[Diagram showing how data is accessed in an FSx for OpenZFS file system.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/zfs-data-access.png)

File\-based workloads are typically spiky, characterized by short, intense periods of high I/O with plenty of idle time between bursts\. To support spiky workloads, in addition to the *baseline* speeds that a file system can sustain 24/7, Amazon FSx provides the capability to *burst* to higher speeds for periods of time for both network I/O and disk I/O operations\. Amazon FSx uses a network I/O credit mechanism to allocate throughput and IOPS based on average utilization — file systems accrue credits when their throughput and IOPS usage is below their baseline limits, and can use these credits when they perform I/O operations\.

## File system performance<a name="zfs-fs-performance"></a>

File system performance is typically measured in latency, throughput, and I/O operations per second \(IOPS\)\. In this section, we document the performance you can expect for data accessed from the in\-memory cache and data accessed from disk\. We also document the baseline performance you can always deliver, as well as the burst performance you can drive for short periods of time\.

The specific level of performance a file system can provide is defined by its *provisioned throughput* capacity, which determines the size of the file server hosting the file system and is equivalent to the baseline disk throughput supported by your file server\. For data access from disks, your file system’s performance is also dependent on the number of *provisioned SSD disk IOPS * configured for the file system’s underlying disks\.

### Data access from in\-memory cache<a name="data-access-memory-cache"></a>

The performance when accessing \(reads\) data from the in\-memory ARC cache primarily depends on two components: the performance supported by the client\-server network I/O connection, and the size of the in\-memory cache\. The following table shows the cached read performance of FSx for OpenZFS file systems, by provisioned throughput capacity:


| Provisioned throughput capacity \(MBps\) | In\-memory cache \(GB\) | Network throughput capacity \(MBps\) | Maximum network IOPS | 
| --- |--- |--- |--- |
| **** | **** | **Baseline** | **Burst** | **** | 
| --- |--- |--- |--- |--- |
| 64 | 1\.4 | 200 | 3,200 | Tens of thousands of IOPS | 
| --- |--- |--- |--- |--- |
| 128 | 2\.8 | 400 | 3,200 | 
| --- |--- |--- |--- |
| 256 | 5\.6 | 800 | 3,200 | 
| --- |--- |--- |--- |
| 512 | 11\.2 | 1,600 | 3,200 | Hundreds of thousands of IOPS | 
| --- |--- |--- |--- |--- |
| 1,024 | 22\.4 | 3,200 |  –  | 
| --- |--- |--- |--- |
| 2,048 | 44\.8 | 6,400 |  –  | 
| --- |--- |--- |--- |
| 3,072 | 67\.2 | 9,600 |  –  | 
| --- |--- |--- |--- |
| 4,096 | 89\.6 | 12,800 |  –  | 1 million IOPS | 
| --- |--- |--- |--- |--- |

### Data access from disk<a name="data-access-disk"></a>

The performance when accessing \(reads\) data from disk depends on the performance supported by the server’s disk I/O connection\. Similar to data accessed from in\-memory cache, the performance of this connection is determined by the provisioned throughput of the file system\. Provisioned throughput capacity is equivalent to the baseline throughput capacity of your file server, as shown in the following table:


| Provisioned throughput capacity \(MBps\) | Disk throughput capacity \(MBps\) | Maximum Disk IOPS | 
| --- |--- |--- |
| **** | **Baseline** | **Burst** | **Baseline** | **Burst** | 
| --- |--- |--- |--- |--- |
| 64 | 64 | 1,024 | 2,500 | 40,000 | 
| --- |--- |--- |--- |--- |
| 128 | 128 | 1,024 | 5,000 | 40,000 | 
| --- |--- |--- |--- |--- |
| 256 | 256 | 1,024 | 10,000 | 40,000 | 
| --- |--- |--- |--- |--- |
| 512 | 512 | 1,024 | 20,000 | 40,000 | 
| --- |--- |--- |--- |--- |
| 1,024 | 1,024 |  –  | 40,000 |  –  | 
| --- |--- |--- |--- |--- |
| 2,048 | 2,048 |  –  | 80,000 |  –  | 
| --- |--- |--- |--- |--- |
| 3,072 | 3,072 |  –  | 120,000 |  –  | 
| --- |--- |--- |--- |--- |
| 4,096 | 4,096 |  –  | 160,000 |  –  | 
| --- |--- |--- |--- |--- |

The previous table shows your file system’s throughput capacity for uncompressed data\. However, because data compression reduces the amount of data that needs to be transferred as disk I/O, you can often deliver higher levels of effective throughput for compressed data\. For example, if your data is compressed to be 50% smaller \(that is, a compression ratio of 2\), then you can drive up to 2x the throughput than you could if the data were uncompressed\. For more information, see [Data compression](#perf-data-compression)\.

#### SSD IOPS and performance<a name="ssd-iops-disk-performance"></a>

Data accessed from disk is also subject to the performance of those underlying disks, which is determined by the number of provisioned SSD IOPS configured on the file system\. The maximum IOPS levels you can achieve are defined by the lower of: the maximum IOPS supported by your file server’s disk I/O connection, and the maximum SSD disk IOPS supported by your disks\. In order to drive the maximum performance supported by the server\-disk connection, you should configure your file system’s provisioned SSD IOPS to match the maximum IOPS in the table above\.

If you select `Automatic` provisioned SSD IOPS, Amazon FSx will provision 3 IOPS per GB of storage capacity up to the maximum of 160,000, which is the highest IOPS level supported by the disk I/O connection documented above\. If you select `User-provisioned`, you can configure any level of SSD IOPS from the minimum of 3 IOPS per GB of storage, up to the maximum 160,000 IOPS \(as long as you don't exceed 1000 IOPS per GB\)\.

![\[Chart showing provisioned IOPS.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/iops-chart.png)

### Example: storage capacity and throughput capacity<a name="throughput-example-openzfs"></a>

The following example illustrates how throughput capacity and provisioned IOPS impact file system performance\.

A file system that is configured with 2 TiB of SSD storage capacity and 512 MBps of throughput capacity can support the following performance:
+ Data access from in\-memory cache – 1,600 MBps baseline throughput and 3,200 MBps burst throughput\.
+ Disk access from disks – 512 MBps baseline throughput and 1,024 MBps burst throughput\. 6,144 disk IOPS by default \(3 IOPS per GiB\), and up to 20,000 baseline IOPS and 40,000 burst IOPS if you provision additional SSD IOPS\.

### Single client performance<a name="single-client-performance"></a>

FSx for OpenZFS file systems are designed to deliver the maximum performance of your file system across your clients in aggregate, whether you are supporting data access from a single client, or thousands of clients\. The following sections provide some practical tips on how to maximize client performance\.

## Tips for maximizing performance<a name="performance-tips-zfs"></a>

### Client considerations<a name="client-considerations"></a>

#### Amazon EC2 instances<a name="ec2-instances"></a>

When launching the Amazon EC2 instances that will work with your FSx for OpenZFS file system, ensure that they can support the level of performance your OpenZFS file system to deliver\. Ensure they have the compute, memory, and network capacity sufficient to drive the throughput, IOPS, and latencies provided by your FSx for OpenZFS file system\.

To determine your EC2 instance’s compute and memory capacity, see [Instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide for Linux Instances*\. To determine its network capacity, see [Amazon EC2 instance network bandwidth](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-network-bandwidth.html) in the same guide\. The performance characteristics of FSx for OpenZFS file systems don't depend on the use of Amazon EC2–optimized instances\.

#### NFS nconnect<a name="nfs-nconnect"></a>

With FSx for OpenZFS, NFS clients can use the `nconnect` mount option to have multiple TCP connections \(up to 16\) associated with a single NFS mount\. Such an NFS client multiplexes file operations onto multiple TCP connections \(multi\-flow\) in a round\-robin fashion to obtain improved performance beyond single TCP connection \(single\-flow\) limits\. For more information on single\-flow limits, see [Amazon EC2 instance network bandwidth](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-network-bandwidth.html) in the *Amazon EC2 User Guide for Linux Instances*\.

The following command demonstrates how to use the `nconnect` mount option to mount an FSx for OpenZFS volume with a maximum of 16 simultaneous connections:

```
sudo mount -t nfs -o nconnect=16 filesystem_dns_name:/vol_path /localpath
```

See your NFS client documentation to confirm whether `nconnect` is supported in your client version\. NFS `nconnect` is included by default in Linux kernel versions 5\.3 and above, meaning it is supported on EC2 instances running Ubuntu 18\.04 or RHEL8\.3\.

#### NFS v3<a name="nfs-v3"></a>

FSx for OpenZFS file systems flexibly support multiple versions of the NFS protocol \(v3, v4\.0, v4\.1, v4\.2\)\. While more recent versions of NFS can better support simultaneous access from many clients \(due to a more robust file\-locking mechanism\) and client\-side caching, NFSv3 may still provide improved latency, throughput, and IOPS performance for performance\-sensitive workloads\. You can mount using NFSv3 from Linux, Windows, or macOS EC2 instances\. For more information, see [Mounting FSx for OpenZFS volumes](mount-openzfs-volumes.md)\.

The following example illustrates how to specify NFS v3 when mounting an FSx for OpenZFS volume:

```
sudo mount -t nfs -o nfsvers=3 fs-dns-name:/vol_path /local_path
```

#### NFS delegations<a name="nfs-delegations"></a>

To improve the ability of NFS clients to cache data locally, NFSv4 introduced NFS delegations, or the ability of the server to delegate certain responsibilities to the client\. If the client is granted a read delegation, it is assured that no other client has the ability to write to the file for the duration of the delegation, meaning that the client can read from its local copy instead of having to go back to the file server\.

FSx for OpenZFS file systems support NFSv4 file read delegations\. To take advantage of this capability, ensure your clients are mounting with NFS v4\.0 or higher\.

#### Request model<a name="request-model"></a>

When you mount your file system, asynchronous writes are enabled by default \(that is, `-o async`\)\. With asynchronous writes, pending write operations are buffered on the client before they are written to your Amazon FSx file system, enabling lower latencies for these operations\. A client that has enabled synchronous writes \(that is, `-o sync`\), or one that opens files using an option that bypasses the cache \(for example, `O_DIRECT`\), issues synchronous requests which mean that every operation incurs a round\-trip between your client and the file server\. We recommend using the default asynchronous write option to maximize client performance\.

#### Other recommended mount options<a name="other-mount-options"></a>

To improve the performance of your file system, you can also include the following options when mounting your file system\.
+ `rsize=1048576` – Sets the maximum number of bytes of data that the NFS client can receive for each network READ request\. to 1048576 bytes \(1 MB\)\. Due to lower memory capacity on file systems with 64 MB/s and 128 MB/s of provisioned throughput, these file systems will only accept a maximum rsize of 262144 and 524288 bytes, respectively\.
+ `wsize=1048576` – Sets the maximum number of bytes of data that the NFS client can send for each network WRITE request to 1048576 bytes \(1 MB\)\. Due to lower memory capacity on file systems with 64 MB/s and 128 MB/s of provisioned throughput, these file systems will only accept a maximum wsize of 262144 and 524288 bytes, respectively\.
+ `timeo=600` – Sets the timeout value that the NFS client uses to wait for a response before it retries an NFS request to 600 deciseconds \(60 seconds\)\.
+ `_netdev` – When present in **/etc/fstab**, prevents the client from attempting to mount the FSx for OpenZFS volume until the network has been enabled\.

The following example uses sample values\.

```
sudo mount -t nfs -o rsize=1048576,wsize=1048576,timeo=600 fs-01234567890abcdef1.fsx.us-east-1.amazonaws.com:/fsx/vol1 /fsx
```

### File system and volume configurations<a name="fs-vol-configuratios"></a>

#### Provisioned throughput capacity and in\-memory cache<a name="provisioned-throughput-in-mem-cache"></a>

In addition to defining the throughput and IOPS that a file system can deliver, a file system's provisioned throughput capacity also determines the amount of in\-memory cache on your file server\. Increasing your file system's throughput capacity improves workload performance in two ways\.

First, it increases the throughput and IOPS you can drive from disk \(disk I/O\) and from in\-memory cache\. Second, by increasing the amount of in\-memory cache, you can store more data in your file server's in\-memory cache, which drives higher cached performance for larger workloads\.

Some request\- or metadata\-intensive workloads will also benefit from a larger file server in\-memory cache\. These types of workloads can generate and store a large volume of metadata in the in\-memory cache\. To ensure the size of your file server's in\-memory cache is not a bottleneck for your file system performance, we recommend provisioning at least 128 MB/s of throughput capacity for these types of workloads\. 

#### NFS export options \(sync and async\)<a name="perf-nfs-export-options"></a>

On the file server side, the `sync` or `async` NFS export option can impact performance\. \(This is distinct from the similarly\-named option you use when mounting your FSx for OpenZFS volume on your client\.\) This option determines whether your file server will acknowledge client I/O requests as complete when they are written to the file server’s in\-memory cache \(`async`\), or only after they are committed to the file server’s persistent disks \(`sync`\)\. `sync` is the default option and is generally recommended for most workloads\.

If you have performance\-intensive workloads that can use an FSx for OpenZFS volume as temporary storage for shorter\-term data processing or workloads that are resilient to data loss, you can use the `async` option to achieve substantially higher performance\. Because an FSx for OpenZFS volume exported with the `async` option will acknowledge client writes before they are committed to durable disk storage, clients can write data to the file server at a significantly faster rate\. However, this performance comes at the cost that a file server crash or reboot will cause data loss of acknowledged writes that have not yet been committed to the server's disks\.

To help you take advantage of this option, you can also make your workloads more resilient to data loss by leveraging FSx for OpenZFS file system backups and volume snapshots\. For more information, see [Protecting your Amazon FSx for OpenZFS data with backups and snapshots](protecting-data.md)\.

#### Data compression<a name="perf-data-compression"></a>

For read\-heavy workloads, compression can significantly improve the overall throughput performance of your file system because it reduces the amount of data that needs to be sent between the underlying storage and the file server\. FSx for OpenZFS volumes support the following data compression algorithms\.
+ *Z\-Standard compression* delivers very high levels of on\-disk data compression, with higher read throughput and reduced write throughput performance than LZ4 compression\.
+ *LZ4 compression* delivers higher write throughput performance, but achieves lower levels of data compression than Z\-Standard compression\.

With data compression, you can improve your read throughput on data accessed from disk up to the same levels you deliver for frequently accessed cached data\. The specific improvement depends upon the amount by which compression can reduce the size of your dataset: your effective throughput will be roughly equivalent to the product of your provisioned disk throughput and your compression ratio \(defined as the ratio of the size of the compressed data to the size of the uncompressed data\)\. For the highest provisioned throughput level \(4096 MBps\), common Z\-Standard compression ratios of 2\-3x can increase your *effective read throughput by up to 8\-12 GBps*\.

You can change a volume's data compression to improve performance\. Changing this property affects only newly\-written data on the volume\.

#### ZFS Record size<a name="record-size-performance"></a>

Specifies a suggested block size for files in the volume\. This property is designed solely for use with databases and other workloads that access files in fixed\-size records\. ZFS automatically tunes block sizes according to internal algorithms optimized for typical access patterns\. When you create volume, the default record size is 128 KiB\. General purpose workflows perform well using the default record size, and we don't recommend changing it, as it may adversely affect performance\.

For database workflows that create very large files but access them in small random chunks, specifying a record size greater than or equal to the record size of the database can result in significant performance gains\. For databases that use a fixed disk block or record size for I/O, set the ZFS record size to match it\. See [Dataset record size](https://openzfs.github.io/openzfs-docs/Performance%20and%20Tuning/Workload%20Tuning.html#dataset-recordsize) in the OpenZFS documentation for more information\.

Streaming workflows such as multimedia and video can benefit from setting a larger record size than the default value\. For more information about setting the record size on a volume, see [Managing FSx for OpenZFS volumes](managing-volumes.md)\.

You can change a volume's record size to make performance improvements\. Changing the volume record size affects only files created afterward; existing files are unaffected\.

## Monitoring performance<a name="measure-performance-cw"></a>

Every minute, FSx for OpenZFS emits usage metrics to Amazon CloudWatch and you can use these metrics to help identify opportunities to improve the performance your clients can drive from your file system\.

You can investigate aggregate file system performance with the `Sum` statistic of each metric\. For example, the `Sum` of the `DataReadBytes` statistic reports the total read throughput by file system or volume, and the `Sum` of the `DataWriteBytes` statistic reports the total write throughput by file system or volume\.

For more information on monitoring your file system’s performance, see [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)\.