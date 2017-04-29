1) HDFS Federation :
                    > In HDFS federation, there are multiple NameNodes, each storing metadata and block mapping of file and directories contained in particular sub-directories.
                    > The list of sub-directories managed by a NameNode is called a namespace volume.
                    > Blocks for files belonging to a Namespace is called a block pool.
                    > One DataNode can contain blocks of different namespace volumes.
                    > So namespace volumes are divided among NameNodes but not among the DataNodes.
                    
 High Availability :
                    > There is a pair of NameNodes in an active-standby configuration
                    > In the event of the failure of the active NameNode, the standby takes over its duties without a significant interruption
                    > The NameNodes must use highly-available shared storage to share the edit log.
                    > Edit logs are read by StandbyNameNode when it takes the responsibility of the ActiveNameNode
                    > DataNodes must send blocked reports to both the NameNodes because of the block mappings
                    > Checkpointing is done by Standby NameNode
                    > Each DataNode reports block stored by it to both NameNodes
                   
2) Writing data Failure Handling using HDFS :
                    > The pipeline is closed and any packets in the ack queue are added to the front of the data queue
                    > The current block on the good DataNodes is given a new identity, which is communicated to the NameNode
                    > The failed DataNode is removed from the pipeline, and a new pipeline is constructed from the two good DataNodes
                    > The remainder of the block’s data is written to the good DataNodes in the pipeline
                    > The NameNode notices that the block is under-replicated, and it arranges for a further replica to be created on another node
                    > As long as dfs.namenode.replication.min replicas (which defaults to 1) are written, the write will succeed
                    > The block will be asynchronously replicated across the cluster until its target replication factor is reached 
                    
