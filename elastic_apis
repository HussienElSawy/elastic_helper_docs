# Kibana DevTools APIs

### Add version if missing after upgrading to 7.17.5 [issue: https://github.com/elastic/elasticsearch/issues/84880]
#### add meta.version to .watches index
    PUT .watches/_mapping
    {
        "_meta": {
        "version" : "7.17.5"
    }
    }

#### add meta.version to .triggered_watches index
    PUT .triggered_watches/_mapping
    {
        "_meta": {
        "version" : "7.17.5"
    }
    }
### investigate watchers issues (not working, delayed ..etc)
#### to stop watchers on all nodes
    POST _watcher/_stop
#### to start watchers on all nodes
    POST _watcher/_start
#### get watcher status on each node
    GET _xpack/watcher/stats
#### get queue on each node
    GET _xpack/watcher/stats?filter_path=stats.execution_thread_pool.queue_size,stats.node_id,stats.watch_count
#### get details on pending watchers
    GET _xpack/watcher/stats?metric=pending_watches
#### get all metrics about watchers
    GET _xpack/watcher/stats?metric=_all
### Misc. helpful APIs
#### get shards (index, primary/replica, shard number, size) and sort them by primary & size, and show size in GB
    GET _cat/shards?v=true&h=index,prirep,shard,store&s=prirep,store&bytes=gb
#### get current heap for each ip 
    GET _cat/nodes?v=true&h=ip,heap.current
#### get default view of all shards
    GET _cat/shards?v=true
#### get total active shards
    GET _cluster/stats?filter_path=indices.shards.total
#### get transient settings in your cluster
    GET _cluster/settings?flat_settings=true&filter_path=transient
#### get persistent settings in your cluster
    GET _cluster/settings?flat_settings=true&filter_path=persistent
#### insert cluster settings to your cluster (this will overwrite settings) 
    PUT _cluster/settings
    {
    "persistent": {
        "cluster.max_shards_per_node": 4000,
        "cluster.routing.rebalance.enable" : "all",
        "cluster.routing.allocation.allow_rebalance" : "indices_primaries_active",
        "cluster.routing.allocation.cluster_concurrent_rebalance":"5",
        "cluster.routing.allocation.enable" : "all",
        "cluster.routing.allocation.node_concurrent_recoveries": "5",
        "cluster.routing.allocation.node_concurrent_incoming_recoveries" : "5",
        "cluster.routing.use_adaptive_replica_selection" : "true",
        "cluster.routing.allocation.total_shards_per_node": 4000,
        "indices.recovery.max_bytes_per_sec": "125mb",
        "indices.recovery.max_concurrent_file_chunks": 2,
        "indices.recovery.max_concurrent_operations": 1,
        "cluster.routing.allocation.disk.watermark.low": "85%",
        "cluster.routing.allocation.disk.watermark.high": "90%",
        "cluster.routing.allocation.disk.watermark.flood_stage" : "95%"
    },
    "transient": {
        "*": null
    }
    }

#### override current presistent settings with transient settings
    PUT _cluster/settings
    {
    "transient": {
        "cluster.routing.allocation.node_concurrent_recoveries": "250",
        "cluster.routing.allocation.node_concurrent_incoming_recoveries" : "250"
    }
    }
#### get your license info
    GET _license
#### get nodes info
    GET _cat/nodes?v=true&h=ip,cpu,rm,rc,d,du,r
#### get cluster health
    GET _cluster/health
#### get sements sorted by size.memory
    GET _cat/segments?v=true&s=size.memory
#### get hot threads and check if you have any threads issues
    GET _nodes/hot_threads
#### get nodes stats
    GET _nodes/stats/os,process
#### get node name and its ID
    GET _nodes?filter_path=nodes.*.name
#### get thread pool to see number of handled requests and number of queued/rejected requests for all type of requests
    GET _cat/thread_pool/*?v=true&h=nn,p,po,n,t,a,psz,q,qs,r,l,c,cr,mx,sz,ka
#### get thread pool to see number of handled requests and number of queued/rejected requests for write data only
    GET _cat/thread_pool/write?v=true&h=nn,p,po,n,t,a,psz,q,qs,r,l,c,cr,mx,sz,ka
#### assign "unassigned" shards to specific node
    POST _cluster/reroute
    {
    "commands": [
        {
        "allocate_replica": {
            "index": "shard_name", 
            "shard": 0,
            "node": "data06"
            }
        }
    ]
    }
#### move assigned shard from node to another node manually
    POST _cluster/reroute
    {
    "commands": [
        {
        "move": {
            "index": ".watches", 
            "shard": 0,
            "from_node": "data13", 
            "to_node": "data05"
        }
        }
    ]
    }
#### execlude specific node from allocation
    PUT _cluster/settings
    {
    "transient" : {
    "cluster.routing.allocation.exclude._ip" : ""
    }
    }
#### get sorted list of all nodes and the disk avail on each
    GET _cat/allocation?v=true&s=disk.avail&h=shards,disk.avail,node
#### if any unassigned shard use this to know the reason why its not assigned or stuck
    GET /_cluster/allocation/explain?pretty
#### get specific shard info
    GET _cat/shards?index=.watches
