# Ansible Kubernetes Playbooks

This project is a collection of ansible playbooks for kubernetes setup and management.

## First steps

Before proceeding, you should have ansible installed. Usually ansible is available in 
package managers and may be installed with `apt`, `pacman` or `yum`, for example. 
A minimum ansible.cfg is included in this repo. A simple inventory model was also 
included. To try out your setup, run the following command:

```bash
$ ansible kubernetes_servers -a "echo {{ inventory_hostname }}"
ansible2 | CHANGED | rc=0 >>
ansible2
ansible1 | CHANGED | rc=0 >>
ansible1

```

You should see the list of servers, like the sample output.

## Usage of playbooks

### startKubernetesCluster.yml

This playbook is a complete configuration suite for a kubernetes cluster with the master 
running in the node that is a part of the kubernetes_master group in the inventory and the 
worker nodes specified in kubernetes_nodes groups. All the nodes should be part of the 
kubernetes_servers.

The CRI used is containerd and the CNI is calico. 

If you would prefer to use other CRI you may rewrite the task [installAndConfigureContainerd](playbooks/tasks/installAndConfigureContainerd.yml) if you would like another CNI, take a look at [setupKubernetesMaster](playbooks/setupKubernetesMaster.yml)

#### installKubernetes.yml

This playbook installs all the kubernetes components necessary to run a cluster in all the
servers specified in the inventory group kubernetes_servers.

#### setupKubernetesMaster.yml

This playbook runs kubeadm init on the kubernetes_master and sets up the Calico plugin. The pod CIDR 
may be customized with the variable `pod_cidr` and the default is 10.30.0.0/16


#### joinKubernetesCluster.yml

This playbook runs on the kubernetes_nodes and joins the cluster as a worker node. If you would like it
to join as a control plane node, take a look at the task [joinCluster](playbooks/tasks/joinCluster.yml)
and include `--control-plane` to the join command.

### uninstallKubernetes.yml

This playbook was made as an undo action for the startKubernetesCluster playbook. It uninstalls the 
kubernetes packages from all nodes, removes containerd and removes configuration files created by the
configuration of the cluster.



## Contribuindo

Por favor leia [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) para detalhes do nosso código de conduta e do processo de submissão de PR's para nós.

## Autores

* **Mateus Berardo** - *Trabalho Inicial* - [MatTerra](https://github.com/MatTerra)
Check out the [contributors](contributors) that participated in this project.

## License

Esse projeto está licenciado sob uma licença do MIT - veja o arquivo [LICENSE.md](LICENSE.md) para detalhes

