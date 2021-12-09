Role Name
=========

Simple Ansible role to install nginx on centos7, create self signed ssl cert and create reverse proxy to server https with http backend

Requirements
------------
Only CentOS7 is supported at the moment.


Role Variables
--------------
* check `defaults/main.yml` for complete list

Some variables that require review:
* `nginx_ssl_rp_path:` / # default do offload entire namespace
* `nginx_proxy_pass_url:` 'http://127.0.0.1:80' # default, to localhost http offloading

Dependencies
------------
none.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: joe_speedboat.nginx-rp }

License
-------

GPLv3

Author Information
------------------

Here I am.
