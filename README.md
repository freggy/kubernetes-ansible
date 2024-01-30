Kubernetes ansible
==================

This ansible role installs kubernetes using kubeadm. CRI-O will be installed beforehand. 
Cilium CNI will be installed alongside Kubernetes.

**Note**: all pre `3.x.x` versions of this role use the deprecated apt package repositories of CRI-O and Kubernetes.

Role variables
--------------

All role variables and their defaults can be found at `defaults/main.yml`

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

