
1.) Make sure pssh is installed on this driver node

2.) Make sure that 'node1' & 'node2' are defined in
    /etc/hosts, can communicate via psql between them, and
    are fully reachable from passwordless ssh

3.) refresh-bigsql.sh
     - Make sure each node is clean and ready for bigsql 'remove-bigsql.sh'
     - 'install-bigsql.sh' to install bigsql on each node
     - run 'setup.sh' to do following on each node using pssh;
         + use pgc to install/tune pg11 & install pglogical into the demo db
         + replace pghba.conf with a permissive version for logical rep
         + create the replication role

#### For simple T1 replication of one table between nodes #############
T1) Run 't1-setup.sh' to do the following on each node:
       - create empty T1 table in each node
       - create each node for pglogical 
       - subscribe each node to the other in pglogical

T2) Demo multimaster working across nodes by explaining and then
     running 't1-insert.sh'

#### For testing with the TPC-C like BenchmarkSQL ####################

B1) run bms-build-1.sh
        bms-dump-1.sh

B2) runBenchmark.sh from driver1 to get comfortable with calculating 45%
         sustained rate on node1 standalone.

B3) run bms-setup.sh to:
        bms-restore.sh node1
        bms-pglogical-create-node.sh node1
        bms-restore2.sh node2
        bms-pglogical-create-node.sh node2
        bms-subscribe-nodes.sh

B4) runBenchmark.sh on  to calibrate 40% utilization level on 
    node1 running with pglogical replicating to node2.

B5) run benchmarksql on node1 & node2 with level calculated from B4)

B6) rinse & repeat for different size nodes

