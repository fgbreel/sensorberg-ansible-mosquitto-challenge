```
$ vagrant up
$ ansible-playbook -i "192.168.56.10," playbook.yaml --user vagrant --private-key ./.vagrant/machines/default/virtualbox/private_key
$ openssl s_client -host 192.168.56.10 -port 8883
```


## from client1 (debian)
```
$ cp ca.crt /usr/local/share/ca-certificates/mosquitto-ca.crt && sudo update-ca-certificates
$ hostess add mosquitto.example.com 192.168.56.10
$ (client1) mosquitto_pub -d --tls-version tlsv1.3  -t "hey" -m "hello" -h mosquitto.example.com -p 8883 --cert client-1.example.com.crt --key client-1.example.com.key --cafile /usr/local/share/ca-certificates/mosquitto-ca.crt
```

## mosquitto vm log output when a new client connects successfully:
```
Jun 20 11:44:32 localhost.localdomain mosquitto[25910]: 1687261472: New connection from 192.168.56.1:55370 on port 8883.
Jun 20 11:44:32 localhost.localdomain mosquitto[25910]: 1687261472: New client connected from 192.168.56.1:55370 as auto-73FBC32B-0FB3-6EFC-CABD-A6804FE835C0 (p2, c1, k60, u'client1').
Jun 20 11:44:32 localhost.localdomain mosquitto[25910]: 1687261472: Client auto-73FBC32B-0FB3-6EFC-CABD-A6804FE835C0 disconnected.
```

```
$ vagrant destroy -f && ssh-keygen -f /home/gabriel/.ssh/known_hosts -R 192.168.56.10
```
