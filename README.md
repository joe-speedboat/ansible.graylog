# graylog
This is my personal role to setup a Graylog single node for testing and debugging.
It is not a starting pint for production env, but for evaluation.

Installation with ansible-galaxy:

``` bash
ansible-galaxy install joe-speedboat.graylog
```

## Requirements
* Currently tested with Rocky Linux 9
* Ansible 2.18 or higher is required for this Ansible Role

* Operating System: Rocky Linux 9
* OS Disk: min 50GB
* CPU: min 6   
* Memory: min 16GB   


Role Variables
--------------
Variables are speaking or documented in defaults/main.yml   


License
-------
https://opensource.org/licenses/GPL-3.0    
Copyright (c) Chris Ruettimann <chris@bitbull.ch>
