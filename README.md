## Server configuration


### Create non-root user with sudo priveleges

```sh
ssh root@[SERVER_IP]
```

(on server) Create new user `admin`:
```sh
adduser admin
# create password for "admin" user
```

(on server) Grant sudo priveleges to `admin`:
```sh
adduser admin sudo
```

(on host) Add ssh keys:
```sh
ssh-copy-id -i $HOME/.ssh/[SSH_KEY_NAME].pub admin@[SERVER_IP]
```

---

### Install Ansible

(on host)
```sh
python3 -m venv venv
source venv/bin/activate
pip install ansible
```

---

### Configure Ansible

(on host)

```sh
cp ./hosts.example ./hosts
vim hosts
# add your SERVER_IP and SSH_KEY_NAME
```

```sh
vim sudo_pass
# write sudo password for "admin" user here
```
```sh
chmod go-rw sudo_pass
```

check that ansible is ready
```sh
ansible ubuntu -m ping
```

---

### Run playbooks

(on host)

Install packages, disable root login, restart sshd:
```sh
ansible-playbook basic_setup_playbook.yaml --become-password-file sudo_pass
```

Install docker:
```sh
ansible-playbook docker_playbook.yaml --become-password-file sudo_pass
```

Install zsh, ohmyzsh, plugins:
```sh
ansible-playbook zsh_playbook.yaml --become-password-file sudo_pass
```



---

### Additional features

**Configure ssh alias**

(on host)
```sh
cat << EOF >> $HOME/.ssh/config 
Host SERVER_ALIAS
Hostname SERVER_IP
User admin
Port 22
IdentityFile ~/.ssh/SSH_KEY_NAME
ServerAliveInterval 20
TCPKeepAlive no
EOF
```

connect to server with alias:
```sh
ssh SERVER_ALIAS
```

---
