### Add Below Entry To Your Deployer (Updated By Joe)
```sh
[shclustering]
pass4SymmKey = yoursecuritykey
shcluster_label = shcluster1
```
```sh 
# In all the SH cluster memebers ###(Run On All Search Head With The Exception Of Deployer)
./splunk init shcluster-config -auth <username>:<password> -mgmt_uri <URI>:<management_port> -replication_port <replication_port> -replication_factor <n> -conf_deploy_fetch_url <URL>:<management_port> -secret <security_key> -shcluster_label <label>
```
```sh 
# setting up the captain (any cluster member can be choosen to this command, that perticular instance will be the first captain)
./splunk bootstrap shcluster-captain -servers_list "<URI>:<management_port>,<URI>:<management_port>,..." -auth <username>:<password>
```
```sh 
# In all the SH cluster memebers ###(Run On All Search Head That Need To Receive Data On Indexer  )
./splunk edit cluster-config -mode  searchhead -master_uri https://indexerNodeMaster:8089(mgtport) -secret password 
```

```sh 
# remove a memeber 
./splunk remove shcluster-member
```
```sh 
# see the cluster status
./splunk show shcluster-status -auth <username>:<password>
```
```sh 
# see the member status
./splunk list shcluster-config -auth <username>:<password>
```

### Indexer clustering setup 

# In Cluster Master $SPLUNK_HOME/etc/system/local/server.conf
```sh 
[clustering]
mode = master
replication_factor = 4
search_factor = 3
pass4SymmKey = whatever
cluster_label = idxcluster1
```
# In each Indexer $SPLUNK_HOME/etc/system/local/server.conf
```sh 
[clustering]
master_uri = https://<Cluster Master URI>:<management_port>
mode = slave
pass4SymmKey = <pass4SymmKey configured in master>

[replication_port://<replication_port>]
```
# Configure each member of the SH cluster as search head in Indexer cluster
```sh 
./splunk edit cluster-config 
    -mode searchhead 
    -master_uri <Indexer Cluster Master URI>
    -secret <Indexer pass4SymmKey> 
```
./splunk restart
