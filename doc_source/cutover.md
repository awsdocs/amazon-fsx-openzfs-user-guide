# Cutting over to Amazon FSx<a name="cutover"></a>

To cut over to your FSx for OpenZFS file system, do the following:
+ Disconnect all clients that write to the source file system\.
+ Perform an rsync or Robocopy final file sync to ensure there is no data loss when cutting over\.
+ Connect all clients to your FSx for OpenZFS file system\.

Now your FSx for OpenZFS file system is available with the data from the source file system and is available for clients to read and write to it\. To make this data accessible to clients and applications, see [Accessing data: supported clients and environmentsAccessing data](supported-fsx-clients.md)\.