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
openstack stack create -e lib/env.yaml \
	--parameter "key_name=fgaudet-key;image_id=Ubuntu Server 16.04 LTS (xenial);net_id=fgaudet-net2;name=fgaudet-cassandra" \
	-t cassandra.yaml \
	Cassandra-stack
```

Default node count is 3 (+ 1 seeder).

# Parameters

You can change the default parameters to suit your own environment. For example, create a 5 nodes cluster, with a m1.xlarge flavor :

```
openstack stack create -e lib/env.yaml \
	--parameter "count=5;flavor=m1.xlarge;key_name=fgaudet-key;image_id=Ubuntu Server 16.04 LTS (xenial);net_id=fgaudet-net2;name=fgaudet-cassandra" \
	-t cassandra.yaml \
	Cassandra-stack
```

Note the stack id :
```
+---------------------+--------------------------------------------------------+
| Field               | Value                                                  |
+---------------------+--------------------------------------------------------+
| id                  | 3372c082-a049-405b-99b5-ac439278ea5f                   |
| stack_name          | Cassandra-stack                                        |
| description         | Template that installs a cluster of Cassandra servers. |
| creation_time       | 2017-05-30T12:13:47                                    |
| updated_time        | None                                                   |
| stack_status        | CREATE_IN_PROGRESS                                     |
| stack_status_reason | Stack CREATE started                                   |
+---------------------+--------------------------------------------------------+
```

# Check the master

Get your cluster's public IP address using the stack id (see above):

`openstack stack output show 3372c082-a049-405b-99b5-ac439278ea5f public_ip`

This command should return you something like this, with of course a real IP :
```
+--------------+--------------------------------------------------+
| Property     | Value                                            |
+--------------+--------------------------------------------------+
| description  | The public IP address of this Cassandra cluster. |
| output_key   | public_ip                                        |
| output_value | "XXX.XXX.XXX.XXX"                                |
+--------------+--------------------------------------------------+
```

# Check it !

Connect to your seeder, using this IP you've just find out :

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
