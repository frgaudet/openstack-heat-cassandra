# Introduction

This heat template allows you to easily deploy a Cassandra cluster on OpenStack.

# Quick start
1) Clone this repository :

`git clone https://github.com/frgaudet/openstack-heat-cassandra.git`

2) Source your OpenStack environment file :

`source openrc.sh`

3) Prepare your parameters :

	* key_name: Name of key-pair to be used
	* image_id: Server image
	* net_id: Private network id
	* name: Prefix for all your instances's name

4) Launch a Cluster :

```
cd openstack-heat-cassandra
heat stack-create -f cassandra.yaml \
	-e lib/env.yaml \
	-P "key_name=fgaudet-key;image_id=3170a757-0c5b-48a8-899c-1c5a1a264907;net_id=dev-net;name=fgaudet-cassandra" Cassandra-stack
```

Default node count is 3 (+ 1 seeder).

# Parameters

You can change the default parameters to suit your own environment. For example, create a 5 nodes cluster, with a m1.xlarge flavor :

```
heat stack-create -f cassandra.yaml \
	-e lib/env.yaml \
	-P "count=5;flavor=m1.xlarge;key_name=fgaudet-key;image_id=1b115984-32b9-4485-a664-26229db439fa;net_id=dev-net;name=fgaudet-cassandra" Cassandra-stack
```

# Check it !

Connect to your seeder, using the floating IP which has been automatically set up.

`ssh <username>@IP`

Then login with the cassandra user (via root):

```
ubuntu@fgaudet-cassandra:~$ sudo su -
root@fgaudet-cassandra:~# su - cassandra
```

and check your cluster :

```
cassandra@fgaudet-cassandra:~$ nodetool status
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load       Tokens       Owns (effective)  Host ID                               Rack
UN  10.0.0.73  83.19 KB   256          49.3%             eb05607c-ee98-4173-9cfc-51735c72b692  rack1
UN  10.0.0.74  106.9 KB   256          49.0%             d8c96c25-abfb-4f29-aee4-874f54ce0247  rack1
UN  10.0.0.75  15.42 KB   256          50.9%             e68a3426-fc01-4e08-b304-ea8ebc91526d  rack1
UN  10.0.0.76  107.37 KB  256          50.7%             cc043259-c900-42ea-961f-ab5119670cfa  rack1
```
