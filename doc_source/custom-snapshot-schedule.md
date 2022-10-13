# Setting up a custom snapshot schedule<a name="custom-snapshot-schedule"></a>

You can set up a custom, automated snapshot schedule for FSx for OpenZFS volumes using the resources and configuration template provided in the following topic\. The custom snapshot scheduling solution performs user\-initiated snapshots of your Amazon FSx volumes on a custom schedule that you define\. For example, you can configure a custom schedule to take a snapshot every hour and automatically delete snapshots that are older than two days\.

This solution requires input for the following parameters:
+ The ID of the FSx for OpenZFS file system volume of which to take the snapshots\. If you provide a file system ID, the solution will take snapshots of all volumes on the file system\.
+ A CRON schedule pattern that defines when to take the snapshots\.
+ The snapshot retention period \(in days\)\.
+ The name tags to apply to the snapshots\.

For more information on CRON schedule patterns, see [Schedule expressions for rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

## Architecture overview<a name="fsx-custom-snapshot-overview"></a>

Deploying this solution builds the following resources in the AWS Cloud:

![\[Diagram showing the AWS resources the custom snapshot schedule AWS CloudFormation template builds in the AWS Cloud, and the workflow used to manage snapshots.\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/images/openzfs-custom-snapshot-architecture.png)

The diagram illustrates the following custom snapshot schedule workflow:

1. The solution AWS CloudFormation template deploys an CloudWatch Event, an AWS Lambda function, an Amazon Simple Notification Service \(Amazon SNS\) queue, and an IAM role\. The IAM role gives the Lambda function permission to invoke the necessary Amazon FSx API operations\.

1. The CloudWatch event runs on a schedule you define as a CRON pattern, during the initial deployment\. This event invokes the solution’s snapshot manager Lambda function that invokes the Amazon FSx `CreateSnapshot` API operation to initiate a snapshot\.

1. The snapshot manager retrieves a list of existing user\-initiated snapshots for the specified volume using `DescribeSnapshots`\. It then deletes snapshots older than the retention period, which you specify during the initial deployment\.

1. The snapshot manager sends a notification message to the Amazon SNS queue on a successful snapshot if you choose the option to be notified during the initial deployment\. A notification is always sent in the event of a failure\.

## AWS CloudFormation template<a name="fsx-custom-snapshot-template"></a>

This solution uses AWS CloudFormation to automate the deployment of the Amazon FSx custom snapshot scheduling solution\. To use this solution, download the [fsx\-openzfs\-scheduled\-snapshot\.template](https://solution-references.s3.amazonaws.com/fsx/snapshot/fsx-openzfs-scheduled-snapshot.yaml) AWS CloudFormation template\.

## Automated deployment<a name="fsx-custom-snapshot-deployment"></a>

The following procedure configures and deploys this custom snapshot scheduling solution\. It takes about five minutes to deploy\. Before you start, you must have the ID of a volume on an Amazon FSx file system running in an Amazon Virtual Private Cloud \(Amazon VPC\) in your AWS account\. For more information on creating these resources, see [Creating a volume](managing-volumes.md#creating-volumes)\.

**Note**  
Implementing this solution incurs billing for the associated AWS services\. For more information, see the pricing details pages for those services\.

**To launch the custom snapshot solution stack**

1. Download the [fsx\-openzfs\-scheduled\-snapshot\.template](https://solution-references.s3.amazonaws.com/fsx/snapshot/fsx-openzfs-scheduled-snapshot.yaml) AWS CloudFormation template\. For more information on creating an AWS CloudFormation stack, see [Creating a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) in the *AWS CloudFormation User Guide*\.
**Note**  
By default, this template launches in the US East \(N\. Virginia\) AWS Region\. Amazon FSx for OpenZFS is currently only available in specific AWS Regions\. You must launch this solution in an AWS Region where FSx for OpenZFS is available\. For more information, see [Amazon FSx endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/fsxn.html) in the *AWS General Reference*\.

1. For **Parameters**, review the parameters for the template and modify them for the needs of your file system volumes\. This solution uses the following default values\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/custom-snapshot-schedule.html)

1. Choose **Next**\.

1. For **Options**, choose **Next**\.

1. For **Review**, review and confirm the settings\. Select the check box acknowledging that the template creates IAM resources\.

1. Choose **Create** to deploy the stack\.

You can view the status of the stack in the AWS CloudFormation console in the **Status** column\. You should see a status of **CREATE\_COMPLETE** in about five minutes\.

## Additional options<a name="fsx-custom-snapshots-supplemental"></a>

You can use the Lambda function created by this solution to perform custom scheduled snapshots of more than one FSx for OpenZFS volume\. The volume ID is passed to the Amazon FSx function in the input JSON for the CloudWatch event\. The default JSON passed to the Lambda function is as follows, where the values for `VolumeId` and `SuccessNotification` are passed from the parameters specified when launching the AWS CloudFormation stack\.

```
{
	"start-snapshot": "true",
	"purge-snapshots": "true",
	"volume-id": "${VolumeId}",
	"notify_on_success": "${SuccessNotification}"
}
```

To schedule snapshots for an additional FSx for OpenZFS volume, create another CloudWatch event rule\. You do so using the Schedule event source, with the Lambda function created by this solution as the target\. Choose **Constant \(JSON text\)** under **Configure Input**\. For the JSON input, simply substitute the volume ID of the FSx for OpenZFS volume to back up in place of `${VolumeId}`\. Also, substitute either `Yes` or `No` in place of `${SuccessNotification}` in the JSON above\.

Any additional CloudWatch Event rules you create manually aren't part of the AWS CloudFormation stack for the Amazon FSx custom scheduled snapshot solution\. Thus, they aren't removed if you delete the stack\.