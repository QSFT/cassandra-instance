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

Create the 1st cassandra server instance cass2

    rhc app create cass2 diy

Add this upstream repo

    cd cass2
    git remote add upstream https://github.com/dell-oss/cassandra-instance
    git pull -s recursive -X theirs upstream master


Then push the repo upstream

    git push


Test

    ssh
    

    

