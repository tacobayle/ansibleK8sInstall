# ansibleK8sInstall

## Goals
Install and bootstrap a K8S cluster including Docker, helm, K8S and CNI (flannel or calico).

## Prerequisites:
- Ansible available
- Ubuntu hosts available (controller and workers)

## Environment:
Playbook(s) has/have been tested against:
### Ansible host version
```
ansible 2.9.12
  config file = None
  configured module search path = ['/home/nic/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/nic/.local/lib/python3.8/site-packages/ansible
  executable location = /home/nic/.local/bin/ansible
  python version = 3.8.5 (default, Jul 28 2020, 12:59:40) [GCC 9.3.0]
  ```

### Ubuntu Bionic (as host to deploy Kubernetes)
```
NAME="Ubuntu"
VERSION="18.04.4 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.4 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

### docker and kubernetes version:
check var/param.yaml files

### Kubernetes Networking provider:
- flannel
- calico

## Input/Parameters:
- Make sure the Ansible hosts file is ready (with 'masters' and 'workers' in separate groups)
```
---
all:
  children:
    master:
      hosts:
        192.168.36.133:
    workers:
      hosts:
        192.168.36.134:
        192.168.36.136:
  vars:
    ansible_user: nic
    ansible_ssh_private_key_file: '~/.ssh/id_rsa.local'
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
```

- Make sure vars/params.yml is configured with appropriate variables

## Use the ansible playbook(s) to:
### On all Nodes (pbInstallK8s.yml)
- Install mandatory packages for Docker/K8s
- Add Docker/K8s/helm key to Apt
- Add Docker/K8s/helm repo to Apt
- Install Docker/K8s/helm pacakages
- Add Docker User to docker group
- Enable iptables
### On the master node (pbBootStrapMaster.yml)
- Initialize the cluster
- configure the kubernetes.dockerUser
- Install the Networking networking
- Generate the join join_command
- Save it to a local file locally
### On the workers (pbBootStrapWorkers.yml)
- Transfer the file to the workers
- Run the join_command on the workers
### On the master node (pbClusterStatus.yml)
- Wait 120 seconds
- Check the status of each node - if fails wait 180 seconds and redo

## Run the playbook:
ansible-playbook -i hosts main.yml

## Improvement:
- check status does not seem to work.

## Kubernetes command check:
```
nic@bionic1:~$
nic@bionic1:~$ kubectl get nodes
NAME      STATUS   ROLES    AGE   VERSION
bionic1   Ready    master   11m   v1.18.2
bionic2   Ready    <none>   11m   v1.18.2
bionic3   Ready    <none>   11m   v1.18.2
nic@bionic1:~$
nic@bionic1:~$
nic@bionic1:~$ kubectl get nodes -o wide
NAME      STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
bionic1   Ready    master   12m   v1.18.2   192.168.36.133   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.8
bionic2   Ready    <none>   11m   v1.18.2   192.168.36.134   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.8
bionic3   Ready    <none>   11m   v1.18.2   192.168.36.136   <none>        Ubuntu 18.04.4 LTS   4.15.0-99-generic   docker://19.3.8
nic@bionic1:~$
nic@bionic1:~$
nic@bionic1:~$
nic@bionic1:~$
nic@bionic1:~$ docker version
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b7f0
 Built:             Wed Mar 11 01:25:46 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b7f0
  Built:            Wed Mar 11 01:24:19 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
nic@bionic1:~$
```
