# mosquitto

This role install the latest version `2.0.15` of `mosquitto` from epel.

# easyrsa

The `easyrsa` package available at epel is currently at version `3.0.8`,
which is incompatible with `openssl` version `3.x`.

Compatibility with `openssl` version `3.x` has been added in `easyrsa`
version `3.0.9`.

See more at:
- https://github.com/OpenVPN/easy-rsa/issues/648
- https://github.com/OpenVPN/easy-rsa/issues/554
- https://github.com/OpenVPN/easy-rsa/issues/454
