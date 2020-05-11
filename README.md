# ansibleK8sInstall
## Prerequisites:
1. Ansible available
2. Ubuntu VMs available

## Input/Parameters:
1. Make sure the Ansible hosts file is ready (with 'masters' and 'workers' in separate groups)
2. Make sure vars/params.yml is ready

## Use the ansible playbook(s) to:
### On all Nodes (pbInstallK8s.yml)
1. Install mandatory packages for Docker/K8s
2. Add Docker/K8s key to Apt
3. Add Docker/K8s repo to Apt
4. Install Docker/K8s pacakages
5. Add Docker User to docker group
6. Enable iptables
### On the master node (pbBootStrapMaster.yml)
1. Initialize the cluster
2. configure the dockerUser
3. Install the Networking networking
4. Generate the join join_command
5. Save it to a local file locally
### On the workers (pbBootStrapWorkers.yml)
1. Transfer the file to the workers
2. Run the join_command on the workers
### On the master node (pbClusterStatus.yml)
1. Wait 120 seconds
2. Check the status of each node - if fails wait 180 seconds and redo

## Run the playbook:
ansible-playbook -i hosts main.yml

## Tests:
Playbook(s) has/have been tested against:
- Ubuntu Bionic
- ansible 2.9.5
- docker 18.06.1~ce~3-0~ubuntu
- kubernetes 1.12.7-00

## Improvement:
Make sure disabling swap works (not tested).
Improve the cluster Status check.

## logs (installing dockers on 3 nodes - 1 master, 2 workers):

### Ansible outputs:
```
avi@ansible:~/ansible/k8sInstall$ ansible-playbook -i hosts main.yml

PLAY [all] ******************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************
ok: [192.168.36.136]
ok: [192.168.36.134]
ok: [192.168.36.133]

TASK [Update repositories cache and install a list of package] **************************************************************************************************************
changed: [192.168.36.133] => (item=apt-transport-https)
changed: [192.168.36.134] => (item=apt-transport-https)
changed: [192.168.36.136] => (item=apt-transport-https)
ok: [192.168.36.133] => (item=ca-certificates)
ok: [192.168.36.136] => (item=ca-certificates)
ok: [192.168.36.134] => (item=ca-certificates)
ok: [192.168.36.133] => (item=curl)
ok: [192.168.36.136] => (item=curl)
ok: [192.168.36.134] => (item=curl)
changed: [192.168.36.136] => (item=gnupg-agent)
changed: [192.168.36.133] => (item=gnupg-agent)
changed: [192.168.36.134] => (item=gnupg-agent)
ok: [192.168.36.136] => (item=software-properties-common)
ok: [192.168.36.133] => (item=software-properties-common)
ok: [192.168.36.134] => (item=software-properties-common)

TASK [Add docker key to apt] ************************************************************************************************************************************************
changed: [192.168.36.133] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.136] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.134] => (item=https://download.docker.com/linux/ubuntu/gpg)
changed: [192.168.36.134] => (item=https://packages.cloud.google.com/apt/doc/apt-key.gpg)
changed: [192.168.36.136] => (item=https://packages.cloud.google.com/apt/doc/apt-key.gpg)
changed: [192.168.36.133] => (item=https://packages.cloud.google.com/apt/doc/apt-key.gpg)

TASK [Add repo to apt] ******************************************************************************************************************************************************
changed: [192.168.36.136] => (item=deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable)
changed: [192.168.36.133] => (item=deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable)
changed: [192.168.36.134] => (item=deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable)
changed: [192.168.36.133] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial main)
changed: [192.168.36.134] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial main)
changed: [192.168.36.136] => (item=deb https://apt.kubernetes.io/ kubernetes-xenial main)

TASK [Install docker and k8s packages] **************************************************************************************************************************************
changed: [192.168.36.136] => (item=docker-ce=5:19.03.8~3-0~ubuntu-bionic)
changed: [192.168.36.136] => (item=kubectl=1.18.2-00)
changed: [192.168.36.134] => (item=docker-ce=5:19.03.8~3-0~ubuntu-bionic)
changed: [192.168.36.133] => (item=docker-ce=5:19.03.8~3-0~ubuntu-bionic)
changed: [192.168.36.134] => (item=kubectl=1.18.2-00)
changed: [192.168.36.133] => (item=kubectl=1.18.2-00)
changed: [192.168.36.136] => (item=kubelet=1.18.2-00)
changed: [192.168.36.136] => (item=kubeadm=1.18.2-00)
changed: [192.168.36.134] => (item=kubelet=1.18.2-00)
changed: [192.168.36.133] => (item=kubelet=1.18.2-00)
changed: [192.168.36.134] => (item=kubeadm=1.18.2-00)
changed: [192.168.36.133] => (item=kubeadm=1.18.2-00)

TASK [Add user nic to the docker group] *************************************************************************************************************************************
changed: [192.168.36.134]
changed: [192.168.36.133]
changed: [192.168.36.136]

TASK [Enable iptables] ******************************************************************************************************************************************************
changed: [192.168.36.133] => (item=echo "net.bridge.bridge-nf-call-iptables=1" | tee -a /etc/sysctl.conf)
changed: [192.168.36.134] => (item=echo "net.bridge.bridge-nf-call-iptables=1" | tee -a /etc/sysctl.conf)
changed: [192.168.36.136] => (item=echo "net.bridge.bridge-nf-call-iptables=1" | tee -a /etc/sysctl.conf)
changed: [192.168.36.136] => (item=sysctl -p)
changed: [192.168.36.133] => (item=sysctl -p)
changed: [192.168.36.134] => (item=sysctl -p)

PLAY [master] ***************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************
ok: [192.168.36.133]

TASK [Initialize the cluster] ***********************************************************************************************************************************************
changed: [192.168.36.133]

TASK [Setup the kube config file for nic] ***********************************************************************************************************************************
changed: [192.168.36.133] => (item=mkdir -p /home/nic/.kube)
changed: [192.168.36.133] => (item=cp -i /etc/kubernetes/admin.conf /home/nic/.kube/config)
changed: [192.168.36.133] => (item=chown nic:nic /home/nic/.kube/config)
[WARNING]: Consider using the file module with state=directory rather than running 'mkdir'.  If you need to use command because file is insufficient you can add 'warn:
false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.
[WARNING]: Consider using the file module with owner rather than running 'chown'.  If you need to use command because file is insufficient you can add 'warn: false' to this
command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.

TASK [Install Networking Provider] ******************************************************************************************************************************************
changed: [192.168.36.133] => (item=kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml)

TASK [Generate join command] ************************************************************************************************************************************************
changed: [192.168.36.133]

TASK [Copy join command to local file] **************************************************************************************************************************************
changed: [192.168.36.133 -> localhost]

PLAY [workers] **************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************
ok: [192.168.36.136]
ok: [192.168.36.134]

TASK [Copy the join command to server location] *****************************************************************************************************************************
changed: [192.168.36.136]
changed: [192.168.36.134]

TASK [Run the command to join the cluster] **********************************************************************************************************************************
changed: [192.168.36.134]
changed: [192.168.36.136]

PLAY [master] ***************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************
ok: [192.168.36.133]

TASK [Pausing for 120 seconds] **********************************************************************************************************************************************
Pausing for 120 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [192.168.36.133]

TASK [Kubectl command "kubectl get nodes --output=json"] ********************************************************************************************************************
changed: [192.168.36.133]

TASK [set_fact] *************************************************************************************************************************************************************
ok: [192.168.36.133]

TASK [debug] ****************************************************************************************************************************************************************
ok: [192.168.36.133] => (item=Checking the API item contains ready for each node) => {
    "msg": "Status for node bionic1 is Ready"
}
ok: [192.168.36.133] => (item=Checking the API item contains ready for each node) => {
    "msg": "Status for node bionic2 is Ready"
}
ok: [192.168.36.133] => (item=Checking the API item contains ready for each node) => {
    "msg": "Status for node bionic3 is Ready"
}

PLAY RECAP ******************************************************************************************************************************************************************
192.168.36.133             : ok=18   changed=12   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
192.168.36.134             : ok=10   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
192.168.36.136             : ok=10   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

avi@ansible:~/ansible/k8sInstall$
```

### Kubernetes command check:
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
