# Deleting a file system<a name="delete-file-system"></a>

You can delete an FSx for OpenZFS file system using the Amazon FSx console, the AWS CLI, and the Amazon FSx API and SDKs\.

**To delete a file system:**
+ **Using the console** – Follow the procedure described in [Step 3: Clean up resources](getting-started-step3.md)\.
+ **Using the CLI or API** – Use the [delete\-file\-system](https://docs.aws.amazon.com/cli/latest/reference/fsx/delete-file-system.html) CLI command or the [DeleteFileSystem](https://docs.aws.amazon.com/fsx/latest/APIReference/API_DeleteFileSystem.html) API operation\. The following is an example using the CLI to delete 
**Note**  
To delete a file system which still has child volumes present, you must include `DELETE_CHILD_VOLUMES_AND_SNAPSHOTS` in the `Options` property, otherwise the delete request will fail\.

  ```
  aws fsx delete-file-system \
      --file-system-id fs-1234567890abcdef0
      --open-zfs-configuration '{ 
        "FinalBackupTags": [ 
           { 
              "Key": "string",
              "Value": "string"
           }
        ],
        "Options": [ "DELETE_CHILD_VOLUMES_AND_SNAPSHOTS" ],
        "SkipFinalBackup": boolean
     }'
  ```