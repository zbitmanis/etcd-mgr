# ETCD manager 

## Description

Deploy a production-ready ETCD cluster and present your solution to the team. By production-ready we mean secure, highly available, stable, deployed in a repeatable and predictable way using Ansible/Puppet/Chef and monitored.
During the implementation you should focus on:
1.
Architecture.
1.
You should make sure that the ETCD cluster is highly available and resilient to failures.
2.
Security.
1.
A cluster should be secure to have multi-tenant usage.
2.
The cluster should also isolate its members’ communication from the public interface.
3.
Optionally PKI is set up to have secure communication with peers and clients



## Requirements

1. By production-ready we mean secure, highly available, stable, deployed in a repeatable and predictable way
2. A cluster should be secure to have multi-tenant usage
3. The cluster should also isolate its members’ communication from the public interface.
4. Optionally PKI is set up to have secure communication with peers and clients.

Requirments based on the setup descrition: 

 - There is a second interface that is connected to internal network for the cluster.
 - ---*There is a second disk attached with more io performance* ---

## Decisions 
- Deployment Approaches for etcd

   - Use Binary Packages Provided by the etcd Vendor
This approach, recommended by the vendor, involves using precompiled binary packages. While vendor-endorsed, it introduces certain maintainability risks due to limited visibility into the installed binaries and lack of comprehensive documentation. Additionally, there is a potential dependency risk if the binaries rely on dynamically linked libraries.

 - Build and Maintain Linux Distribution Packages
Building custom Linux distribution packages can mitigate risks associated with binary maintenance and feature updates. However, this approach demands extra effort to maintain these packages and introduces a dependency on artifact storage. Given the limited timeframe for this task, custom-built packages may only be considered if future demand increases.

 - Use Container-Based Deployment — **chosen method**
The vendor offers the option to deploy the etcd cluster using containers, aligning with industry standards. etcd is commonly used as a key-value store for Kubernetes (K8s) clusters, and container-based deployment is a proven method, even for managing complex solutions. Industry leaders like [RedHat](https://www.redhat.com) use this approach to maintain on-prem cloud solutions and distributed storage systems.

- Use a 3-Node Cluster:
 - To ensure a highly available and stable cluster, deploying an odd-numbered cluster (such as 3 nodes) reduces the risk of a 'split-brain' scenario. Although a 5-node setup is available, it's preferable to have dedicated nodes, especially since the potential cluster load is not yet clear. Based on these factors, the decision was made to use a 3-node cluster, reserving one dedicated node for monitoring. Furthermore, adhering to industry standards, the PKI infrastructure will operate within a limited-access environment to minimize the risk of compromising its private components.


## Environment 

### Description of the environment within the task


We provide you with access to Virtual Machines where you can test your solution, deploy, etc. There is a second disk attached with more io performance. There is a second interface that is connected to internal network for the cluster.
To describe the nodes the folwing a

There is a second disk attached with more io performance.

### Environment inspection 

To provide environment details the following ansible role was developed [role_node_info](https://github.com/zbitmanis/role_node_info/README.md). The purpose of the Ansible role is to collect infromation essential for the task  infrastructure components  and store information for the feature use. 



The following information was collected:

- [Node 01](doc/host-info-ca-a.md)
- [Node 02](doc/host-info-mon-a.md)
- [Node 03](doc/host-info-etcd-node-a.md)
- [Node 04](doc/host-info-etcd-node-b.md)
- [Node 05](doc/host-info-etcd-node-c.md)


Based on collected infromaton additional disk drive was not identified for any of the nodes


  

## Node roles

The follwoing roles was assigned to the provided infrastructure 

| node                                       | node role                                            | Notes                         |
|--------------------------------------------|--------------------------------------------------------|----------------------------------------|
| [Node 01](doc/host-info-ca-a.md)| PKI CA node|      Node to manage PKI infrastructure      |
| [Node 02](doc/host-info-mon-a.md) | Monitoring node| Node will run prometheus/grafana/alertmanager services                             |
| [Node 03](doc/host-info-etcd-node-a.md)| etcd cluster role|      Node to run etcd workloads      |
| [Node 04](doc/host-info-etcd-node-b.md)| etcd cluster role|      Node to run etcd workloads      |
| [Node 05](doc/host-info-etcd-node-c.md)| etcd cluster role|      Node to run etcd workloads      |
  
Using collected information and assigned roles the information was reflected within the main deployment ansible  [etcd-mgr](https://github.com/zbitmanis/etcd-mgr/tree/main) [group variables] (https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/group_vars/sw/main.yaml)


## How to manage etcd cluster 
The chapter descrives how ansible code can be used to manage etcd cluster

### Prerequisites 

   - The configuration managment is based on the [Ansible](https://docs.ansible.com) , hence Python can be concidered as major prerequisite, you an use dedicated python venv for the ansible managment
   - To use Python virtual environments could be recomended approach 
   -  ```bash
     $ python -m venv ansible
 
   - All configuration and Ansible code is stored within the [etcd-manager](https://github.com/zbitmanis/etcd-mgr/tree/main/ansible) git  repository . To run localy or via CI/CD pipelines the code have to be fetched using git client
   - All nodes have to be accessible via SSH
   		*Note:  to maintain specific to the project  ssh settings the [ansible/etcd-mgr-ssh.cfg](https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/etcd-mgr-ssh.cfg) can be updated*

### How to run and configure the etcd-cluster 

#### Iniitialise local environment to be ready for the feature maangment
  The main ansible plays have to be fetched from git repository [https://github.com/zbitmanis/etcd-mgr](https://github.com/zbitmanis/etcd-mgr) localy or to on CI/CD infrastructure. 
  To resolve dependencies for ansible the follwing actions have to be provided
  
  ```bash 
$git clone https://github.com/zbitmanis/etcd-mgr.git
$cd etcd-manager/ansible 
  	  #Install python requirments -(install ansible)
$pip install -r requirements.txt 
  	  #Install the external roles
  	  #roles will be downloaded to the common_roles folder
$ansible-galaxy install -r requirements.yaml
  ```  

#### Prepare to use encrypted data
Ansible vault is used to store some sensitive data store ansbie vault . The vault key is shared separatly . 

```bash
$cd etcd-mgr 
$echo ${ETCD_MGR_VAULT_KEY} > ansible/.vault_pass
```
#### Prepare the ansible managed infrastructure 

The  file is used to manage infrastructure : [inventory/hosts-sw](https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/inventory/hosts-sw)

	
#### Inspect the nodes 	

We have to collect the information to be used for the cluster managment, based on the information the internal variables can be updated. The node information colecting role is fetched during the local environment preparation

The nodes-info.yaml playbook can be used to collect the data, the data will be stored within on the host running the ansible output folder
 
```bash
$ansible-playbook -i inventory/hosts-sw nodes-info.yaml
```
based on the collected data [group variables] (https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/group_vars/sw/main.yaml) have to be updated. 


### Configure etcd cluster

To install etcd cluster prerequisites, prepare PKI infrastructure, requests ans sign certificates, collect CA public bundle configure 3 nodes etcd cluster, prepare monitoring environment prometheus, grafana plase use [nodes.yaml](https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/nodes.yaml) playbook 

```bash
$ansible-playbook -i inventory/hosts-sw nodes.yaml
```

Please be patioen execution coud take at least 15-20 minutes. 


### Configure etcd multi tenancy 


To manage etcd authentifications the role [etcd-users](https://github.com/zbitmanis/etcd-mgr/blob/main/ansible/roles/etcd-users/README.md) can be used


User and roles are managed using the folwing structure: 

``` yaml
etcd_roles_list:
  - role: atenant
    prefix: atenant
  - role: btenant
    prefix: btenant

etcd_users_list:
  - user: auser
    role: atenant
    password: '{{ vault_etcd_user_auser_password }}'
  - user: buser
    role: btenant
    password: '{{ vault_etcd_user_buser_password }}'

```
To execute user managment please use etcd-users.yaml playbook 

```bash
$ansible-playbook -i inventory/hosts-sw etcd-users.yaml
```

##How to access monitoring data

The monitoring data using grafana dashboard please use the flollowing urls: 

- grafana: [https://grafana.sw.fogc.space/](https://grafana.sw.fogc.space/)
- prometheus: [https://prometheus.sw.fogc.space/](https://grafana.sw.fogc.space/)
- alertmanager: [https://alertmanager.sw.fogc.space/](https://grafana.sw.fogc.space/)

Please use credentials shared separately 

To review etcd collected metrics feel free to use Etcd Cluster Overview dashboard. 


Note: *The services uses TLS from the local project PKI*

