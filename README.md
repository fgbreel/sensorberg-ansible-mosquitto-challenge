> ⚠️ The code present in this repository is still work-in-progress!

# Current Status and Characteristics

1. Latest `mosquitto` version `2.0.15` running on Rocky Linux 9.
2. Configured `MQTT` listener over `TLS1.3` at port `8883`.
3. Public Key Infrastructure for client authentication/identity via `easyrsa`.

# Provision
```
$ vagrant up
$ ansible-playbook -i "192.168.56.10," playbook.yaml --user vagrant --private-key ./.vagrant/machines/default/virtualbox/private_key

gabriel@computer:~/Documents/sensorberg$ vagrant up && ansible-playbook -i "192.168.56.10," playbook.yaml --user vagrant --private-key ./.vagrant/machines/default/virtualbox/private_key
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'rockylinux/9'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'rockylinux/9' version '2.0.0' is up to date...
==> default: Setting the name of the VM: sensorberg_default_1687273957424_18383
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 6.1.40
    default: VirtualBox Version: 7.0
==> default: Configuring and enabling network interfaces...

PLAY [install and configure mosquitto with mqtt listener over tls with pki] ***************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
The authenticity of host '192.168.56.10 (192.168.56.10)' can't be established.
ED25519 key fingerprint is SHA256:GWVzwBYS3JVvfxL6sBmGykrbFXzAtPaeKUf0cla0ceg.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
ok: [192.168.56.10]

TASK [mosquitto : ensure epel is enabled] *************************************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : install packages] *******************************************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : ensure public key infrastructure is present] ****************************************************************************************************************************************************
included: /home/gabriel/Documents/sensorberg/roles/mosquitto/tasks/pki.yaml for 192.168.56.10

TASK [mosquitto : ensure easyrsa is present] **********************************************************************************************************************************************************************
included: /home/gabriel/Documents/sensorberg/roles/mosquitto/tasks/easyrsa.yaml for 192.168.56.10

TASK [mosquitto : check if easyrsa is already present] ************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure easyrsa project gpg key is present] ******************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : ensure easyrsa source and signature are present] ************************************************************************************************************************************************
changed: [192.168.56.10] => (item=https://github.com/OpenVPN/easy-rsa/releases/download/v3.1.5/EasyRSA-3.1.5.tgz)
changed: [192.168.56.10] => (item=https://github.com/OpenVPN/easy-rsa/releases/download/v3.1.5/EasyRSA-3.1.5.tgz.sig)

TASK [mosquitto : verify tarball signature and install] ***********************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : extract easyrsa tarball] ************************************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : initialize public key infrastructure] ***********************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : initialize certificate authority] ***************************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : generate server certificate] ********************************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : generate client certificates] *******************************************************************************************************************************************************************
changed: [192.168.56.10] => (item=client-1.example.com)
changed: [192.168.56.10] => (item=client-2.example.com)
changed: [192.168.56.10] => (item=client-3.example.com)

TASK [mosquitto : ensure mosquito certs directory is present] *****************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : ensure mosquitto ca certificate is present] *****************************************************************************************************************************************************
changed: [192.168.56.10]

TASK [mosquitto : ensure mosquitto certificate is present] ********************************************************************************************************************************************************
changed: [192.168.56.10] => (item=mosquitto.example.com.crt)

TASK [mosquitto : ensure mosquitto certificate key is present] ****************************************************************************************************************************************************
changed: [192.168.56.10] => (item=mosquitto.example.com.key)

TASK [mosquitto : ensure mosquitto configuration] *****************************************************************************************************************************************************************
changed: [192.168.56.10] => (item=etc/mosquitto/mosquitto.conf)

RUNNING HANDLER [mosquitto : reload mosquitto] ********************************************************************************************************************************************************************
changed: [192.168.56.10]

PLAY RECAP ********************************************************************************************************************************************************************************************************
192.168.56.10              : ok=20   changed=16   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

# Idempotency
```
gabriel@computer:~/Documents/sensorberg$ ansible-playbook -i "192.168.56.10," playbook.yaml --user vagrant --private-key ./.vagrant/machines/default/virtualbox/private_key

PLAY [install and configure mosquitto with mqtt listener over tls with pki] ***************************************************************************************************************************************

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure epel is enabled] *************************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : install packages] *******************************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure public key infrastructure is present] ****************************************************************************************************************************************************
included: /home/gabriel/Documents/sensorberg/roles/mosquitto/tasks/pki.yaml for 192.168.56.10

TASK [mosquitto : ensure easyrsa is present] **********************************************************************************************************************************************************************
included: /home/gabriel/Documents/sensorberg/roles/mosquitto/tasks/easyrsa.yaml for 192.168.56.10

TASK [mosquitto : check if easyrsa is already present] ************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure easyrsa project gpg key is present] ******************************************************************************************************************************************************
skipping: [192.168.56.10]

TASK [mosquitto : ensure easyrsa source and signature are present] ************************************************************************************************************************************************
skipping: [192.168.56.10] => (item=https://github.com/OpenVPN/easy-rsa/releases/download/v3.1.5/EasyRSA-3.1.5.tgz)
skipping: [192.168.56.10] => (item=https://github.com/OpenVPN/easy-rsa/releases/download/v3.1.5/EasyRSA-3.1.5.tgz.sig)
skipping: [192.168.56.10]

TASK [mosquitto : verify tarball signature and install] ***********************************************************************************************************************************************************
skipping: [192.168.56.10]

TASK [mosquitto : extract easyrsa tarball] ************************************************************************************************************************************************************************
skipping: [192.168.56.10]

TASK [mosquitto : initialize public key infrastructure] ***********************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : initialize certificate authority] ***************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : generate server certificate] ********************************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : generate client certificates] *******************************************************************************************************************************************************************
ok: [192.168.56.10] => (item=client-1.example.com)
ok: [192.168.56.10] => (item=client-2.example.com)
ok: [192.168.56.10] => (item=client-3.example.com)

TASK [mosquitto : ensure mosquito certs directory is present] *****************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure mosquitto ca certificate is present] *****************************************************************************************************************************************************
ok: [192.168.56.10]

TASK [mosquitto : ensure mosquitto certificate is present] ********************************************************************************************************************************************************
ok: [192.168.56.10] => (item=mosquitto.example.com.crt)

TASK [mosquitto : ensure mosquitto certificate key is present] ****************************************************************************************************************************************************
ok: [192.168.56.10] => (item=mosquitto.example.com.key)

TASK [mosquitto : ensure mosquitto configuration] *****************************************************************************************************************************************************************
ok: [192.168.56.10] => (item=etc/mosquitto/mosquitto.conf)

PLAY RECAP ********************************************************************************************************************************************************************************************************
192.168.56.10              : ok=15   changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

```

# TLS from client
```
$ openssl s_client -host mosquitto.example.com -port 8883

CONNECTED(00000003)
depth=1 CN = Easy-RSA CA
verify return:1
depth=0 CN = mosquitto.example.com
verify return:1
---
Certificate chain
 0 s:CN = mosquitto.example.com
   i:CN = Easy-RSA CA
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 20 14:26:13 2023 GMT; NotAfter: Sep 22 14:26:13 2025 GMT
 1 s:CN = Easy-RSA CA
   i:CN = Easy-RSA CA
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 20 14:26:12 2023 GMT; NotAfter: Jun 17 14:26:12 2033 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIDhTCCAm2gAwIBAgIQdEQqVMtY/b8UP9cYY+sFDDANBgkqhkiG9w0BAQsFADAW
MRQwEgYDVQQDDAtFYXN5LVJTQSBDQTAeFw0yMzA2MjAxNDI2MTNaFw0yNTA5MjIx
NDI2MTNaMCAxHjAcBgNVBAMMFW1vc3F1aXR0by5leGFtcGxlLmNvbTCCASIwDQYJ
KoZIhvcNAQEBBQADggEPADCCAQoCggEBAPLBaIOTgB2hY7fQiNYwu8Ob9diaU7XK
v3lIvdVr/TJun5axL4imuxn9rSAMdhWBVUVJXUgZSApvrPIMIfPzGRNn+0hABWrS
YceJv/7dp+7di8SeSNye2uA2X/MkhOe1tMZkeLrVbPL2X678eIAvWXWTsfFHzgWF
vCzx3rVLctrXD8z9eNxHZk0uKzc7iSHo6VL4dFMuBsyLjAZM7e1yJiC5nJBTP8yn
sm6GtLvsugQCJlX9nsgJhfG8d+tYm90y0nJvwIRaT/bExQlAXCV7lvnQB3ddjUzO
TCAyCb37zFlTKYrKx2n8D3pcSlejoG4wZA3PoyHCmmeSpxgvZPikZ9kCAwEAAaOB
xDCBwTAJBgNVHRMEAjAAMB0GA1UdDgQWBBSFS3VN046ZFn0YP/ERHRO1pLmlEjBR
BgNVHSMESjBIgBQG2VDVHQtdQKvfiyZe6Hnnhi4+IqEapBgwFjEUMBIGA1UEAwwL
RWFzeS1SU0EgQ0GCFH7iqJ8c2azJOSDwGNPEpHm+nWNSMBMGA1UdJQQMMAoGCCsG
AQUFBwMBMAsGA1UdDwQEAwIFoDAgBgNVHREEGTAXghVtb3NxdWl0dG8uZXhhbXBs
ZS5jb20wDQYJKoZIhvcNAQELBQADggEBABzYVsllGkQ+lTZDLwk2p8vbzkYM6ZFg
lHnagg7pWFzb7Aw79zmb1Q+U+jvu169qLEfoBEbAf1bjrVtz1lz542rvhduaHb5f
KQmPhfTHyutHSfB6UPY71U65Qo0mN7Oq6VAgDBWwYDRq6zpMJO5jYA3YV8aMZtOf
rreXhNaxZLnDF4vLQWe3NB86ZONgpaDMGlA3O5X05v6Kq7xGGEpSHi3RAnWyrOW0
mUkkUzLOOMU/EtO7GDAMHq28MK/bUwx+Jh9O1r3mnEyRhPjudyZNPH00r9U7Rka8
n2ljxBjee8WwAOa4guNFErTXqSAdeWqrVJJ0XhtltWhwuOBkh+hZ+1w=
-----END CERTIFICATE-----
subject=CN = mosquitto.example.com
issuer=CN = Easy-RSA CA
---
No client certificate CA names sent
Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512:ECDSA+SHA224:RSA+SHA224
Shared Requested Signature Algorithms: ECDSA+SHA256:ECDSA+SHA384:ECDSA+SHA512:Ed25519:Ed448:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA-PSS+SHA256:RSA-PSS+SHA384:RSA-PSS+SHA512:RSA+SHA256:RSA+SHA384:RSA+SHA512
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 2380 bytes and written 437 bytes
Verification: OK
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
4067F0D1787F0000:error:0A00045C:SSL routines:ssl3_read_bytes:tlsv13 alert certificate required:../ssl/record/rec_layer_s3.c:1586:SSL alert number 116
```


## publish from client-1.example.com:
```
$ cp ca.crt /usr/local/share/ca-certificates/mosquitto-ca.crt && sudo update-ca-certificates
$ hostess add mosquitto.example.com 192.168.56.10

$ mosquitto_pub -d --tls-version tlsv1.3  -t "hey" -m "hello" -h mosquitto.example.com -p 8883 -i client-1.example.com --cert client-1.example.com.crt --key client-1.example.com.key --cafile /usr/local/share/ca-certificates/mosquitto.crt

Client client-1.example.com sending CONNECT
Client client-1.example.com received CONNACK (0)
Client client-1.example.com sending PUBLISH (d0, q0, r0, m1, 'hey', ... (5 bytes))
Client client-1.example.com sending DISCONNECT
```

## mosquitto vm log output when a new client connects successfully:
```
Jun 20 14:48:14 localhost.localdomain mosquitto[25500]: 1687272494: New connection from 192.168.56.1:43236 on port 8883.
Jun 20 14:48:14 localhost.localdomain mosquitto[25500]: 1687272494: New client connected from 192.168.56.1:43236 as auto-B32D6BFB-5296-648F-A6FB-C3165557C770 (p2, c1, k60, u'client-1.example.com').
Jun 20 14:48:14 localhost.localdomain mosquitto[25500]: 1687272494: Client auto-B32D6BFB-5296-648F-A6FB-C3165557C770 disconnected.
```


# Destroy
```
$ vagrant destroy -f && ssh-keygen -f /home/gabriel/.ssh/known_hosts -R 192.168.56.10
```
