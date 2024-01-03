## Warn Alert - Resolved: Query to isi_stats_d failed

## Description:
I have been receiving numerous alerts stating "Resolved: Query to isi_stats_d failed." email inbox is flooded with these notifications. It seems these alerts are linked to a specific node in the power scale. However, this node is currently operational, and services like share and others are functioning normally. 
We had a week prior updated the powerscale / isilon to 9.5.0.3.

## Example of alert:

| ID    | Started       | Sev | Message                                     |
|-------|---------------|-----|---------------------------------------------|
| 21750 | 12/31 16:52   | W   | Resolved: Query to isi_stats_d failed       |


## Dell Service request notes:
In this situation, it appears that following the OneFS upgrade to version 9.5, we're encountering a known problem where the memory allocation for the isi_stats_d service is insufficient for handling certain requests from the isi_pp_d (Partition Performance Daemon) service, leading to these events.

I need to review the logs to verify if this is indeed the issue. Meanwhile, please initiate a new log collection on the cluster. 

### Asked for:
- `isi_gather_info`
- `isi_for_array -s 'limits -P $(pgrep isi_stats_d) -B | grep vmemoryuse'`
- `isi event list | grep SW_PP_STATS_QUERY_FAILED`
