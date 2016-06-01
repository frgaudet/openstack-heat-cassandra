# Introduction

This heat template allows you to easily deploy a Cassandra cluster on OpenStack.

# Quick start
1. Clone this repository :

`git clone https://github.com/frgaudet/openstack-heat-cassandra.git`

2. Source your OpenStack environment file :

`source openrc.sh`

3. Prepare your parameters :

	* key_name: Name of key-pair to be used
	* image_id: Server image
	* net_id: Private network id
	* name: Prefix for all your instances's name

4. Launch a Cluster :

```
cd openstack-heat-cassandra
heat stack-create -f cassandra.yaml \
	-e lib/env.yaml \
	-P "key_name=fgaudet-key;image_id=ca70466d-5043-4cb6-a779-8a2b1cf303a4;net_id=dev-net;name=fgaudet_cassandra" Cassandra-stack
```

Default node count is 3 (+ 1 seeder).

# Parameters

You can change the default parameters to suit your own environment : 

```
heat stack-create -f cassandra.yaml \
	-e lib/env.yaml \
	-P "count=5;key_name=fgaudet-key;image_id=ca70466d-5043-4cb6-a779-8a2b1cf303a4;net_id=dev-net;name=fgaudet_cassandra" Cassandra-stack
```
