# Mounting FSx for OpenZFS from Amazon Elastic Container Service<a name="mount-openzfs-ecs-containers"></a>

You can access your Amazon FSx for OpenZFS file systems from an Amazon Elastic Container Service \(Amazon ECS\) Docker container on an Amazon EC2 Linux  instance by using a bind mount\. For more information, see [ Bind mounts](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/bind-mounts.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Mounting FSx for OpenZFS volumes on Amazon ECS Linux containers<a name="mount-ecs-linux"></a>

1. Create an ECS cluster using the EC2 Linux \+ Networking cluster template for your Linux containers\. For more information, see [ Creating a cluster](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create_cluster.html) in the *Amazon ECS Developer Guide*\.

1. Create a directory on the EC2 instance for mounting the volume as follows:

   ```
   sudo mkdir /fsxopenzfs
   ```

1. Mount your FSx for OpenZFS volume on the Linux EC2 instance by either using a user\-data script during instance launch, or by running the following commands:

   ```
   sudo mount -t nfs -o nfsvers=NFS_version file-system-dns-name:/volume-path /localpath
   ```

   The following example uses sample values in the mount command\.

   ```
   sudo mount -t nfs -o nfsvers=4.1 fs-01234567890abcdef1.fsx.us-east-1.amazonaws.com:/fsx/vol1 /fsxopenzfs
   ```

   You can also use the file system's IP address instead of its DNS name\.

   ```
   sudo mount -t nfs -o nfsvers=4.1 198.51.100.1:/fsx/vol1 /fsxopenzfs
   ```

1. When creating your Amazon ECS task definitions, add the following `volumes` and `mountPoints` container properties in the JSON container definition\. Replace the `sourcePath` with the mount point and directory in your FSx for OpenZFS file system\.

   ```
   {
       "volumes": [
           {
               "name": "openzfs-volume",
               "host": {
                   "sourcePath": "mountpoint"
               }
           }
       ],
       "mountPoints": [
           {
               "containerPath": "container_local_path",
               "sourceVolume": "openzfs-volume"
           }
       ],
       .
       .
       .
   }
   ```