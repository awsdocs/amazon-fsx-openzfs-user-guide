# Monitoring Amazon FSx for OpenZFS<a name="monitoring_overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon FSx and your AWS solutions\. You should collect monitoring data from all parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. However, before you start monitoring Amazon FSx, you should create a monitoring plan that includes answers to the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

The next step is to establish a baseline for normal Amazon FSx performance in your environment, by measuring performance at various times and under different load conditions\. As you monitor Amazon FSx, you should consider storing historical monitoring data\. This stored data gives you a baseline to compare against with current performance data, identify normal performance patterns and performance anomalies, and devise methods to address issues\.

For example, with Amazon FSx, you can monitor network throughput, I/O for file system and volume read and write operations, and the amount of available storage capacity for your file system and volumes\. When performance falls outside your established baseline, you might need to change the size of your file system to optimize the file system for your workload\.