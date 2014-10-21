Cassandra instance on OpenShift
===============================

This is a simple Red Hat OpenShift example that demonstrates how to deploy a Cassandra instance as an OpenShift cartridge.  


Running on OpenShift
----------------------------

Create an account at https://www.openshift.com

Create the 1st cassandra server instance cass1

    rhc app create cass1 diy

Add this upstream repo

    cd cass1
    git remote add upstream https://github.com/dell-oss/cassandra-instance
    git pull -s recursive -X theirs upstream master


Then push the repo upstream

    git push

Find the IP address of cass1 instance
   
    rhc app show
    ssh <to the gear>
    env | grep $OPENSHIFT_DIY_IP

Create the 2nd cassandra server instance cass2

    rhc app create cass2 diy
    
Set the cass1 IP for cass2

    rhc env set CASSANDRA_NODE=<OPENSHIFT_DIY_IP of cass1> —app cass2

Add this upstream repo

    cd cass2
    git remote add upstream https://github.com/dell-oss/cassandra-instance
    git pull -s recursive -X theirs upstream master


Then push the repo upstream

    git push


Test

    ssh to each server
    
  on cass1

    cd app-root/data/cassandra/bin/
    ./cqlsh $OPENSHIFT_DIY_IP 19160

    create keyspace demo with replication = {'class':'SimpleStrategy', 'replication_factor':2};
    use demo;
    create table names ( id int primary key, name text ); insert into names (id,name) values (1,'trad');

  on cass2
    cd app-root/data/cassandra/bin/
   ./cqlsh $OPENSHIFT_DIY_IP 19160

    use demo;
    select * from names;
    

	
    

    

