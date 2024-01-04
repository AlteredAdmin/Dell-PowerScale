## Warn Alert - Resolved: Query to isi_stats_d failed

## Description:
I have been receiving numerous alerts stating "Resolved: Query to isi_stats_d failed." email inbox is flooded with these notifications. It seems these alerts are linked to a specific node in the power scale. However, this node is currently operational, and services like share and others are functioning normally. 
We had a week prior updated the powerscale / isilon to 9.5.0.3.

Knowledge base event ID 400270001 

## Example of alert:

| ID    | Started       | Sev | Message                                     |
|-------|---------------|-----|---------------------------------------------|
| 21750 | 12/31 16:52   | W   | Resolved: Query to isi_stats_d failed       |


## Dell Service Request Notes:
In this situation, it appears that following the OneFS upgrade to version 9.5, we're encountering a known problem where the memory allocation for the isi_stats_d service is insufficient for handling certain requests from the isi_pp_d (Partition Performance Daemon) service, leading to these events.

Dell needs to review the logs to verify if this is indeed the issue. Meanwhile, please initiate a new log collection on the cluster. 

### Asked for:
- `isi_gather_info`
- `isi_for_array -s 'limits -P $(pgrep isi_stats_d) -B | grep vmemoryuse'`
- `isi event list | grep SW_PP_STATS_QUERY_FAILED`

## Log Review:

### SW_PP_STATS_QUERY_FAILED
The isilon has been triggering "SW_PP_STATS_QUERY_FAILED" right after the cluster was upgraded from 9.2.1.12 to 9.5.0.3. And they are only happening for node 2, which are self-resolving right away.

This error alert SW_PP_STATS_QUERY_FAILED is generated when the isi_pp_d (partition performance daemon) service leader fails to query the isi_stats_d service. This means that the cluster’s health cannot be properly computed. 

The warning is caused by the isi_pp_d process that couldn't able to retrieve stats from the isi_stats_d process on all nodes giving false performance and state status.

These alerts are just informational and have no impact on the status of the cluster. The error means the performance data is not collected but it won't impact any running jobs/protocols. If the system has some other issue there will be other events to indicate.

From logs we see the warning "Query to isi_stats_d failed" is caused by the isi_pp_d process (partition performance daemon) when it is not able to retrieve stats from isi_stats_d process on another node, and the isi_stats_d logs are matching the timestamps of the "SW_PP_STATS_QUERY_FAILED" events: 

## Recommended workaround:
These are the symptoms of an identified issue which is expected to be fixed in future PowerScale releases but the version has not been confirmed yet.

The recommendation to workaround this issue is to increase the isi_stats_d virtual memory limit from 1GB (default) to 2GB by running the following command and monitoring further if still receiving the same alerts

Run this command to set the value to 2GB:

- `isi_for_array -s 'limits -P $(pgrep isi_stats_d) -v 2g'`

Then run this command to check if the change took place:

- `isi_for_array -s 'limits -P $(pgrep isi_stats_d) -B | grep vmemoryuse'`

This change is not impactful to the cluster operation.

