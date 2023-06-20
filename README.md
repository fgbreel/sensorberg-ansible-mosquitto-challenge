```
$ vagrant up
$ ansible-playbook -i "192.168.56.10," playbook.yaml --user vagrant --private-key ./.vagrant/machines/default/virtualbox/private_key
$ openssl s_client -host 192.168.56.10 -port 8893
$ vagrant destroy -f && ssh-keygen -f /home/gabriel/.ssh/known_hosts -R 192.168.56.10
```
