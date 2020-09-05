Kubernetes ansible
==================

This ansible role installs kubernetes using kubeadm. Note that Docker needs to be installed beforehand.
Flannel CNI will be installed alongside Kubernetes. You can change the configuration under `defaults/flannel-cni.yaml.j2`.

Role variables
--------------

All role variables are listed below with their default values.

```yaml
kube_apt_key_url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
kube_apt_repository: deb https://apt.kubernetes.io/ kubernetes-xenial main
kube_cluster_name: mycluster
kube_pod_cidr: 10.244.0.0/16
kube_version: '1.18.6'
kube_control_plane_endpoint: '{{ hostvars[groups.kube_master[0]].ansible_host }}'
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
      master01.example.com:
    kube_worker:
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

TODO
----

* Kubeadm upgrade mechanism
