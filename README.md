# vagrant-box

## VSCode Remote Dev

### Disable strict keys checking

In /etc/ssh/ssh_config change

```sh
StrictHostKeyChecking no
```

### Setup ssh config

Edit ssh config located in ~/.ssh/config and add vagrant based host

```sh

Host vm-19.10
    HostName 127.0.0.1
    ForwardAgent yes
    User vagrant
    Port 2222
    StrictHostKeyChecking no
    PasswordAuthentication no
    IdentityFile ~/Projects/ubuntu-box/.vagrant/machines/19.10/virtualbox/private_key

```
