Cassandra instance as a cluster node on OpenShift
=================================================

This is a simple Red Hat OpenShift example that demonstrates how to deploy a Cassandra node in a cluster as an OpenShift cartridge.  


Running on OpenShift
----------------------------

Create an account at https://www.openshift.com

Create the 1st cassandra instance cass1

    rhc app create cass1 diy

Add this upstream repo

    cd cass1
    git remote add upstream https://github.com/dell-oss/cassandra-instance
    git pull -s recursive -X theirs upstream master


Then push the repo upstream

    git push

Find the IP address of cass1 instance
   
    rhc app show
    ssh <to the gear of cass1>
    env | grep $OPENSHIFT_DIY_IP
    exit

Create the 2nd cassandra instance cass2 in a cluster with the 1st instance cass1

    cd ..
    rhc app create cass2 diy

Check if the private IP of 1st cassandra instance can be reached from cass2 
   
    ssh <to the gear of cass2>
    curl http://<OPENSHIFT_DIY_IP of cass1>:19042
    
    (expect to see the message other than “curl: (7) couldn't connect to host”)
    if you get that message, then you need to retry the step “Create the 2nd cassandra instance” until Openshift gives you the environment that can connect to the 1st instance.
    
Set the cass1 IP for cass2

    cd cass2

    rhc env set CASSANDRA_NODE_IP=<OPENSHIFT_DIY_IP of cass1>

Check the env is set

    rhc env-list


Add this upstream repo

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

  on cass2, verify cluster replication by

    cd app-root/data/cassandra/bin/
    ./cqlsh $OPENSHIFT_DIY_IP 19160

    use demo;
    select * from names;
    

	
    

    

