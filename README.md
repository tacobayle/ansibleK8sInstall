# ansibleK8sInstall
## Prerequisites:
1. Ansible available
2. Ubuntu VMs available

## Input/Parameters:
1. Make sure the Ansible hosts file is ready (with 'masters' and 'workers' in separate groups)
2. Make sure vars/params.yml is ready

## Use the ansible playbook(s) to:
# On all Nodes (pbInstallK8s.yml)
1. Install mandatory packages for Docker/K8s
2. Add Docker/K8s key to Apt
3. Add Docker/K8s repo to Apt
4. Install Docker/K8s pacakages
5. Add Docker User to docker group
6. Enable iptables
# On the master node (pbBootStrapMaster.yml)
1. Initialize the cluster
2. configure the dockerUser
3. Install the Networking networking
4. Generate the join join_command
5. Save it to a local file


## Run the playbook:
ansible-playbook -i hosts main.yml

## Tests:
Playbook(s) has/have been tested against:
- Ubuntu Bionic
- ansible 2.9.5

## Improvement:
Make sure disabling swap works (not tested)

## logs (installing dockers on 3 nodes):
```
```
