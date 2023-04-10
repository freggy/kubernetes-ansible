Requirements
============

* molecule-hetznercloud
* [kubelogin kubectl plugin](https://github.com/int128/kubelogin)

Install
=======

```
$ pip3 install molecule-hetznercloud
```

Notes
=====

This role has to be tested manually. For this to work do the following steps:

* `echo "{dex_host_ip} dex.example.com" >> /etc/hosts`
* `cp files/ssl/ca.pem /etc/ssl/cert/dex.example.com.pem`

Then simply run kubectl commands and login using the configured users.
