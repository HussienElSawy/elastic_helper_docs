# How to Calculate Capacity for your Elastic Cluster:
### Calculate initial needed nodes for each index

REF: https://gitlab.com/groups/gitlab-org/-/uploads/e21f315177585c93ea6e748470367dca/elasticsearch-sizing-and-capacity-planning.pdf (Page 39)

- Retention: Number of days you will keep the index for 
- Replicas: Number of replicas of the data
- Size: Total size in GB per day
- Node Memory: Total RAM per node
- Low Watermark: Elastic Low Watermark, you can get it from the presistent settings (default = 85% or 0.85)
- Error Margin: 5-10% (0.05 - 0.1) as margin of error and background activities 
- Data Type:
  - Hot: if you will write/search on the index
  - Warm: likely infrequent updates for the index, and still searchable 
  - Cold: optimized for archiving (less frequent search and hardly will be updated)
  
 
 Memory:Data ratio can be calculated per data type, if hot 1:30, if warm 1:160 if cold 1:500
 
--- 
 - Total_Data_Size = Size * Retention * (Replicas + 1)
 - Total_Storage_Needed = Total_Data_Size * (1+ (1-Low_Watermark) + Error_Margin)
 - Total_Data_Nodes_Needed = Round up(total_storage_needed / node memory / Memory:data ratio) + 1 Data node for failover capacity
---

---
### Calculate the capacity of your expansions

Total Current Capacity (you cna get that from "Stack Monitoring" Kibana)  
Expected increase (the expected newly increased logs)   
Type of increase (hot, warm, cold)  
 - if will archive more data then cold  
 - if will get new index with more frequent write/search data  then hot  
 - if will keep the expected data for a while can be moved between hot, warm and cold  
low_watermark: 85%  (0.85)  
Error Margin: 10% (0.1)  

---
- Total_Storage_Can_be_used_current = Total_Cluster_Storage_current * Low_Watermark  
- Total_used_after_increase = Used_current + expected_increase  
- total_cluster_storage_after_increase = total_used_after_increase / low_watermark + ((total_used_after_increase / low_watermark) * error_margin)  
- increase_needed = total_cluster_storage_after_increase - Total_Cluster_Storage_current
---

Example:  
current utilization (60TB used out of 100TB)   
expected increase 30TB  


Total Storage can be used: 100TB * 0.85 = 85TB  
Used = 60TB (given)  
Free = 85-60 = 25TB  
expected increase = 30TB  
total used after increase = 60+30 = 90TB  
total storage needed = 90TB / 0.85 + ((90TB / 0.85)*0.1) =  116.46 ~ 117  
increase needed =  total storage needed - total current = 117 - 100 = 17TB  

if data is cold and current cold data can be expanded then you can expand storage only without adding new nodes  
else, if warm or hot most probably you will need new nodes so calculate the nodes needed



