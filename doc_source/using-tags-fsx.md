# Using tags with Amazon FSx<a name="using-tags-fsx"></a>

You can use tags to control access to Amazon FSx resources and to implement attribute\-based access control \(ABAC\)\. Users need to have permission to apply tags to Amazon FSx resources during creation\.

## Grant permission to tag resources during creation<a name="supported-iam-actions-tagging"></a>

Some resource\-creating Amazon FSx API actions enable you to specify tags when you create the resource\. You can use resource tags to implement attribute\-based access control \(ABAC\)\. For more information, see [ What is ABAC for AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html) in the *IAM User Guide*\.

To enable users to tag resources on creation, they must have permissions to use the action that creates the resource, such as `fsx:CreateFileSystem` or `fsx:CreateVolume`\. If tags are specified in the resource\-creating action, Amazon performs additional authorization on the `fsx:TagResource` action to verify if users have permissions to create tags\. Therefore, users must also have explicit permissions to use the `fsx:TagResource` action\.

The following example demonstrates a policy that allows users to create file systems and volumes and apply tags to them during creation in a specific AWS account\.

```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
         "fsx:CreateFileSystem",
         "fsx:CreateVolume",
         "fsx:TagResource"       
      ],
      "Resource": [
         "arn:aws:fsx:region:account-id:file-system/*",
         "arn:aws:fsx:region:account-id:file-system/*/volume/*"
      ]
    }
  ]
}
```

Similarly, the following policy allows users to create backups on a specific file system and apply any tags to the backup during backup creation\.

```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
         "fsx:CreateBackup"
      ],
      "Resource": "arn:aws:fsx:region:account-id:file-system/file-system-id*"
    },
    {
      "Effect": "Allow",
      "Action": [
         "fsx:TagResource"
      ],
      "Resource": "arn:aws:fsx:region:account-id:backup/*"
    }
  ]
}
```

The `fsx:TagResource` action is only evaluated if tags are applied during the resource\-creating action\. Therefore, a user that has permissions to create a resource \(assuming there are no tagging conditions\) does not require permissions to use the `fsx:TagResource` action if no tags are specified in the request\. However, if the user attempts to create a resource with tags, the request fails if the user does not have permissions to use the `fsx:TagResource` action\.

For more information about tagging Amazon FSx resources, see *Tagging resources*\. For more information about using tags to control access to FSx resources, see [Using tags to control access to your Amazon FSx resources](#restrict-fsx-access-tags)\.

## Using tags to control access to your Amazon FSx resources<a name="restrict-fsx-access-tags"></a>

To control access to Amazon FSx resources and actions, you can use AWS Identity and Access Management \(IAM\) policies based on tags\. You can provide the control in two ways:

1. Control access to Amazon FSx resources based on the tags on those resources\.

1. Control what tags can be passed in an IAM request condition\.

For information about how to use tags to control access to AWS resources, see [Controlling access using tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\. For more information about tagging Amazon FSx resources at creation, see [Grant permission to tag resources during creation](#supported-iam-actions-tagging)\. For more information about tagging resources, see [Tag your Amazon FSx resources](tag-resources.md)\.

### Controlling access based on tags on a resource<a name="resource-tag-control"></a>

To control what actions a user or role can perform on an Amazon FSx resource, you can use tags on the resource\. For example, you might want to allow or deny specific API operations on a file system resource based on the key\-value pair of the tag on the resource\.

**Example policy – Create a file system on when providing a specific tag**  
This policy allows the user to create a file system only when they tag it with a specific tag key value pair, in this example, `key=Department, value=Finance`\.  

```
{
    "Effect": "Allow",
    "Action": [
        "fsx:CreateFileSystem",
        "fsx:TagResource"
    ],
    "Resource": "arn:aws:fsx:region:account-id:file-system/*",
    "Condition": {
        "StringEquals": {
            "aws:RequestTag/Department": "Finance"
        }
    }
}
```

**Example policy – Create backups only of Amazon FSx file systems with a specific tag**  
This policy allows users to create backups only of file systems that are tagged with the key value pair `key=Department, value=Finance`, and the backup will be created with the tag `Deparment=Finance`\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "fsx:CreateBackup"
            ],
            "Resource": "arn:aws:fsx:region:account-id:/file-system/*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Department": "Finance"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "fsx:TagResource",
                "fsx:CreateBackup"
            ],
            "Resource": "arn:aws:fsx:region:account-id:backup/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/Department": "Finance"
                }
            }
        }
     
    ]
}
```

**Example policy – Create a file system with a specific tag from backups with a specific tag**  
This policy allows users to create file systems that are tagged with `Department=Finance` only from backups that are tagged with `Department=Finance`\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "fsx:CreateFileSystemFromBackup"
            ],
            "Resource": "arn:aws:fsx:region:account-id:backup/*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Department": "Finance"
                }
            }
        }
    ]
}
```

**Example policy – Delete file systems with specific tags**  
This policy allows a user to delete only file systems that are tagged with `Department=Finance`\. If they create a final backup, then it must be tagged with `Department=Finance`\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "fsx:DeleteFileSystem"
            ],
            "Resource": "arn:aws:fsx:region:account-id:file-system/*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/Department": "Finance"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "fsx:TagResource"
            ],
            "Resource": "arn:aws:fsx:region:account-id:backup/*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/Department": "Finance"
                }
            }
        }
    ]
}
```