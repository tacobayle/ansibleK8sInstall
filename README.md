# ansibleK8sInstall
## Prerequisites:
1. Ansible available
2. Ubuntu VMs available

## Input/Parameters:
1. Make sure the Ansible hosts file is ready
2. Make sure vars/params.yml is ready

## Use the ansible playbook(s) to:
1. Install mandatory packages for Docker/K8s
2. Add Docker/K8s key to Apt
3. Add Docker/K8s repo to Apt
4. Install Docker/K8s pacakages
5. Add Docker Users to docker group

## Run the playbook:
ansible-playbook -i hosts pbInstallK8s.yml

## Tests:
Playbook(s) has/have been tested against:
- Ubuntu Bionic
- ansible 2.9.5

## Improvement:

## logs (installing dockers on 3 nodes):
```
avi@ansible:~/ansible/k8sInstall$ ansible-playbook -i hosts pbInstallK8s.yml

PLAY [all] **************************************************************************

TASK [Gathering Facts] **************************************************************
ok: [192.168.36.134]
ok: [192.168.36.136]
ok: [192.168.36.133]

TASK [Update repositories cache and install a list of package] **********************
changed: [192.168.36.134] => (item=apt-transport-https)
changed: [192.168.36.136] => (item=apt-transport-https)
changed: [192.168.36.133] => (item=apt-transport-https)
ok: [192.168.36.134] => (item=ca-certificates)
ok: [192.168.36.136] => (item=ca-certificates)
ok: [192.168.36.133] => (item=ca-certificates)
ok: [192.168.36.134] => (item=curl)
ok: [192.168.36.136] => (item=curl)
ok: [192.168.36.133] => (item=curl)
changed: [192.168.36.134] => (item=gnupg-agent)
changed: [192.168.36.136] => (item=gnupg-agent)
changed: [192.168.36.133] => (item=gnupg-agent)
ok: [192.168.36.134] => (item=software-properties-common)
ok: [192.168.36.136] => (item=software-properties-common)
ok: [192.168.36.133] => (item=software-properties-common)

TASK [Add docker key to apt] ********************************************************
changed: [192.168.36.134] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.133] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.136] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.136] => (item=https://packages.cloud.google.com/apt/doc/apt-key.                                                                                        gpg)
changed: [192.168.36.133] => (item=https://packages.cloud.google.com/apt/doc/apt-key.                                                                                        gpg)
changed: [192.168.36.134] => (item=https://packages.cloud.google.com/apt/doc/apt-key.                                                                                        gpg)

TASK [debug] ************************************************************************
ok: [192.168.36.136] => {
    "msg": "Ubuntu"
}
ok: [192.168.36.134] => {
    "msg": "Ubuntu"
}
ok: [192.168.36.133] => {
    "msg": "Ubuntu"
}

TASK [debug] ************************************************************************
ok: [192.168.36.136] => {
    "msg": "18"
}
ok: [192.168.36.134] => {
    "msg": "18"
}
ok: [192.168.36.133] => {
    "msg": "18"
}

TASK [debug] ************************************************************************
ok: [192.168.36.136] => {
    "msg": "18.04"
}
ok: [192.168.36.134] => {
    "msg": "18.04"
}
ok: [192.168.36.133] => {
    "msg": "18.04"
}

TASK [debug] ************************************************************************
ok: [192.168.36.136] => {
    "msg": "bionic"
}
ok: [192.168.36.134] => {
    "msg": "bionic"
}
ok: [192.168.36.133] => {
    "msg": "bionic"
}

TASK [Add repo to apt] **************************************************************
changed: [192.168.36.136] => (item=deb [arch=amd64] https://download.docker.com/linux                                                                                        /ubuntu bionic stable)
changed: [192.168.36.134] => (item=deb [arch=amd64] https://download.docker.com/linux                                                                                        /ubuntu bionic stable)
changed: [192.168.36.133] => (item=deb [arch=amd64] https://download.docker.com/linux                                                                                        /ubuntu bionic stable)
changed: [192.168.36.136] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial m                                                                                        ain)
changed: [192.168.36.133] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial m                                                                                        ain)
changed: [192.168.36.134] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial m                                                                                        ain)

TASK [Install docker and k8s packages] **********************************************
changed: [192.168.36.133] => (item=docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic)
changed: [192.168.36.136] => (item=docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic)
changed: [192.168.36.134] => (item=docker-ce-cli=5:18.09.5~3-0~ubuntu-bionic)
changed: [192.168.36.134] => (item=docker-ce=5:18.09.5~3-0~ubuntu-bionic)
ok: [192.168.36.134] => (item=containerd.io)
changed: [192.168.36.136] => (item=docker-ce=5:18.09.5~3-0~ubuntu-bionic)
changed: [192.168.36.133] => (item=docker-ce=5:18.09.5~3-0~ubuntu-bionic)
ok: [192.168.36.136] => (item=containerd.io)
ok: [192.168.36.133] => (item=containerd.io)
changed: [192.168.36.134] => (item=kubectl=1.12.7-00)
changed: [192.168.36.136] => (item=kubectl=1.12.7-00)
changed: [192.168.36.133] => (item=kubectl=1.12.7-00)
changed: [192.168.36.136] => (item=kubelet=1.12.7-00)
changed: [192.168.36.136] => (item=kubeadm=1.12.7-00)
changed: [192.168.36.133] => (item=kubelet=1.12.7-00)
changed: [192.168.36.134] => (item=kubelet=1.12.7-00)
changed: [192.168.36.133] => (item=kubeadm=1.12.7-00)
changed: [192.168.36.134] => (item=kubeadm=1.12.7-00)

TASK [Add the docker users to the docker group] ********************************                                                                                             *****
changed: [192.168.36.134] => (item=nic)
changed: [192.168.36.133] => (item=nic)
changed: [192.168.36.136] => (item=nic)

PLAY RECAP *********************************************************************                                                                                             *****
192.168.36.133             : ok=10   changed=5    unreachable=0    failed=0    s                                                                                             kipped=0    rescued=0    ignored=0
192.168.36.134             : ok=10   changed=5    unreachable=0    failed=0    s                                                                                             kipped=0    rescued=0    ignored=0
192.168.36.136             : ok=10   changed=5    unreachable=0    failed=0    s                                                                                             kipped=0    rescued=0    ignored=0

avi@ansible:~/ansible/k8sInstall$
```
