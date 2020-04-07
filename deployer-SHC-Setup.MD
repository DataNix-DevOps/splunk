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
