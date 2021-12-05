joe-speedboat.mongo_easy
=========

Wrapper role for configuring mongodb nodes with the undergreen.mongodb role.
The basic idea is to have a "as simple as possible" role that achieves the following goals:

* compatibility with elastic.elasticsearch without customizing it
* inventory of multiple hosts will form a working cluster 
* single node inventory will form a secured single node
* add and/or reconfigure nodes within a cluster
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
* vars of joe-speedboat.mongo_easy role start with ```me_```

Dependencies
------------

Documented under Requirements.

Example Playbook
----------------

Documented in ```tests``` folder.

Example LAB Cluster Setup
----------------
* setup 6 VMs with the following settings: 
  * Name: test0[1:3]
  * Alma Linux 8 (Any RHEL8 based distro should work)
  * 2 CPU
  * 4 GB Memory
  * 1 NIC 
  * 50 GB Disk (or whatever you need)
  * Static IP
  * Forward/reverse DNS
  * NTP Configured
  * Disable any firewalling (or don't cry)

* Create a project folder on your ansible control node and prepare the setup
```
mkdir -p mongo-easy/{roles,collections}
cd mongo-easy

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
[mongo]
test01
test02
test03
EOF

# pull THE ROLE :-)
ansible-galaxy role install -p ./roles joe-speedboat.mongo_easy

# fetch the deps into your project folder
ansible-galaxy role install -r roles/joe-speedboat.mongo_easy/roles/requirements.yml -p ./roles

# test if all your target nodes are online
ansible -m ping all

# make the miracle happen :-)
ansible-playbook roles/joe-speedboat.mongo_easy/tests/test.yml

# check the status on the nodes as you need (with 100s of terminals, this looks pretty nerdy 🦖)
mongo
  use admin
    db.auth('siteRootAdmin','.Change.Me.123')
    rs.status()
    db.admin.insert({"name":"bitbull test data"})
    db.admin.find()
    quit()
```
# This is a great GUI
https://www.mongodb.com/try/download/compass

License
-------

https://opensource.org/licenses/LGPL-3.0    
Copyright (c) Chris Ruettimann <chris@bitbull.ch>

