Kubernetes ansible
==================

This ansible role installs kubernetes using kubeadm. Note that Docker needs to be installed beforehand.
Cilium CNI will be installed alongside Kubernetes. You can change the configuration under `defaults/cilium-<version>.yaml.j2`.

Role variables
--------------

All role variables are listed below with their default values.

```yaml
kube_apt_key_url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
kube_apt_repository: deb https://apt.kubernetes.io/ kubernetes-xenial main
kube_cluster_name: mycluster
kube_pod_cidr: 10.244.0.0/16
# kube_version: '<version>'
kube_kubelet_extra_args: ''
kube_kubelet_extra_args_config_file: /etc/default/kubelet
kube_control_plane_endpoint: '{{ hostvars[groups.kube_master[0]].ansible_host }}'
kube_replace_kubeproxy: true
kube_cilium_version: 1.9.0
```

Example inventory
-----------------

Your inventory must contain the groups `kube_master` and `kube_worker`. As the group names might suggest worker nodes live in `kube_worker` and master nodes in `kube_master`.

```YAML
all:
  hosts:
    master01.example.com:
    worker01.example.com:
  children:
    kube_master:
      hosts:
        master01.example.com:
    kube_worker:
      hosts:
        worker01.example.com:
```

Example playbook
----------------

```yaml
---
- hosts: kube_worker kube_master
  vars:
    kube_control_plane_endpoint: 127.0.0.1:6443
  roles:
    - role: geerlingguy.docker
    - role: freggy.kubernetes
```

Upgrading the cluster
---------------------

To upgrade your cluster, you can import the `upgrade.yaml` tasks.

```
tasks:
  - name: "Include kubernetes-ansible"
    include_role:
      name: "kubernetes-ansible"
      tasks_from: upgrade
```

