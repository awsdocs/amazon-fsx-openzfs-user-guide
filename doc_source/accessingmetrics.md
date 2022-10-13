# Accessing CloudWatch metrics<a name="accessingmetrics"></a>

You can see Amazon FSx metrics for CloudWatch in the following ways\. 
+ The Amazon FSx console\.
+ The CloudWatch console\.
+ The CloudWatch CLI \(command line interface\)\.
+ The CloudWatch API\.

The following procedures show you how to access the metrics using these various tools\.

**To view metrics using the Amazon FSx console**

1. Open the Amazon FSx console at [https://console\.aws\.amazon\.com/fsx/](https://console.aws.amazon.com/fsx/)\.

1. From the navigation pane, choose **File systems**, then choose the file system whose metrics you want to view\.

1. Choose **Actions** and choose **View details**\. 

1. On the **Summary** page, choose **Monitoring** to see the metrics for your file system\. 

**To view metrics using the CloudWatch console**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch)\.

1. In the navigation pane, choose **Metrics**\. 

1. Select the **FSx** namespace\.

1. \(Optional\) To view a metric, enter its name in the search field\.

1. \(Optional\) To explore metrics, select the category that best matches your question\. *File system metrics* and *Volume metrics* report summary\-level metrics for individual file systems or volumes\. *File system detailed metrics* and *Volume detailed metrics* report more granular metrics within a file system or volume \(for example, storage capacity used by snapshots\)\.

**To access metrics from the AWS CLI**
+ Use the [https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command with the `--namespace "AWS/FSx"` namespace\. For more information, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

**Using the CloudWatch API**

**To access metrics from the CloudWatch API**
+ Call `[GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html)`\. For more information, see [Amazon CloudWatch API Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/)\. 