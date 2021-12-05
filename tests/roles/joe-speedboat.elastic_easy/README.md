joe-speedboat.elastic_easy
=========

Wrapper role for configuring elasticsearch nodes with the official elastic.elasticsearch role.
The basic idea is to have a "as simple as possible" role that achieves the following goals:

* compatibility with elastic.elasticsearch without customizing it
* inventory of multiple hosts will form a working cluster 
* single node inventory will form a secured single node
* add and/or reconfigure nodes within a cluster
* scale out single node into real cluster
* automatic bootstrapping of a new cluster
* basic best practice implemented
* more to follow...

This role follows the principal of unix/linux that says one tool is doing one thing, 
but this single thing, it does in a simple and clean way.
Altought I tried to achieve the target, your feedback by pull requests and issue requests are highly welcome.
Since elasticsearch is still a complex thing, do not expect to get free consulting here ...
But for good money, we do good consulting as well :-)

Cheers
Chris

Requirements
------------

Roles in ```roles/requirements.yml```
```ansible-galaxy role install -r roles/requirements.yml -p ./roles```

Role Variables
--------------

All variables are documented in ```defaults/main.yml```

HINT
--------------
* vars of joe-speedboat.elastic_easy role start with ```ee_```
* vars of ```elastic.elasticsearch``` role start with ```es_```

```
  es_config <-- +ee_config <---- +ee_config_template_cluster [if node > 1] <--- ee_config_template_all
                             \-- +ee_config_template_single  [if node = 1] <-/

```
WARNING about es_config
-----------------------
* defining elements of ```es_config``` dict, is not allowed, since we configure that for you
* if you want to do so, use ```ee_config``` dict


Dependencies
------------

Documented under Requirements.

Example Playbook
----------------

Documented in ```tests``` folder.

Example LAB Cluster Setup
----------------
* setup 6 VMs with the following settings: 
  * Name: test0[1:6]
  * Alma Linux 8 (Any RHEL8 based distro should work)
  * 4 CPU
  * 8 GB Memory
  * 1 NIC 
  * 50 GB Disk (or whatever you need)
  * Static IP
  * Forward/reverse DNS
  * NTP Configured
  * Disable any firewalling (or don't cry if your masters do not join)

* Create a project folder on your ansible control node and prepare the setup
```
mkdir -p elastic-easy/{roles,collections}
cd elastic-easy

# ANSIBLE CONFIG
cat <<EOF >>ansible.cfg
[defaults]
inventory         = ./inventory
roles_path        = ./roles
collections_paths = collections
remote_user = root
log_path = /tmp/ansible.log
forks = 99
EOF

# ANSIBLE INVENTORY
cat <<EOF >>inventory
[elastic:children]
elastic_master
elastic_data
[elastic:vars]
ee_cluster_name=graylog
ee_action_auto_create_index=false

[elastic_master]
test01
test02
test03
[elastic_master:vars]
ee_is_data=false

[elastic_data]
test04
test05
test06
[elastic_data:vars]
ee_is_master=false
EOF

# pull THE ROLE :-)
ansible-galaxy role install -p ./roles joe-speedboat.elastic_easy

# fetch the deps into your project folder
ansible-galaxy role install -r roles/joe-speedboat.elastic_easy/roles/requirements.yml -p ./roles

# test if all your target nodes are online
ansible -m ping all

# make the miracle happen :-)
ansible-playbook roles/joe-speedboat.elastic_easy/tests/setup_elastic_cluster.yml

# check the status on the master nodes as you need (with 100s of terminals, this looks pretty nerdy 🦖)
watch "curl -s -X GET "localhost:9200/_cat/nodes?pretty""
watch head -n 20 /etc/elasticsearch/elasticsearch.yml
tail -F /var/log/elasticsearch/*.log
journalctl -af
```


License
-------

https://opensource.org/licenses/LGPL-3.0    
Copyright (c) Chris Ruettimann <chris@bitbull.ch>

