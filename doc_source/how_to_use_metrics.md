# How to use FSx for OpenZFS metrics<a name="how_to_use_metrics"></a>

The metrics reported by Amazon FSx provide information that you can analyze in different ways\. The list following shows some common uses for the metrics\. These are suggestions to get you started, not a comprehensive list\.


| How do I determine\.\.\. | Relevant metrics | 
| --- | --- | 
| My file system's IOPS? | SUM\(DataReadOperations \+ DataWriteOperations\)/Period \(in seconds\)  | 
| My file system's throughput? | SUM\(DataReadBytes\+DataWriteBytes\)/Period \(in seconds\)  | 

**Note**  
We recommend that you maintain an average throughput capacity utilization under 50% to ensure that you have enough spare throughput capacity for unexpected spikes in your workload, as well as for any background storage operations \(such as snapshots\)\.