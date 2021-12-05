* Quick and dirty clean up for testing
ansible -mshell -a 'systemctl stop mongod ; dnf -y remove mongo* ; rm -fr /etc/mongod* /var/log/mongodb /var/lib/mongo/ /data/db' all
