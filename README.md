# ansible_haproxy_keepalived
Auto deploy HAproxy and keepalived by ansible offline

## Requirements
- Centos7
- [Ansible](http://docs.ansible.com/intro_installation.html). Tested on 2.6.0 

## Matching Version
- HAproxy		1.8.12
- Keepalived	2.0.5

## Prepare

- Preparing tree(or five ...) hosts,and make sure the Ansible installed in the hosts(at least one).
  If you could not install Ansible on line,you can click [me](https://pan.baidu.com/s/1ebVT7E0i672zGP2CoL4bUw) to download all the rpm packages to your hosts.
```bash
tar -xvf ansible.tar
```
```bash
cd ansible && yum install *.rpm -y
```

- All of the hosts could install openssl-devel,pcre-devel,make,gcc,socat through yum.
- The host what run the program can ssh to te target hosts without password.

## Usage

- Edit ansible_hosts file
- Edit ha_vars/main.yml
- Run playbook.
```bash
ansible-playbook main.yml -i ansible_hosts
```

## Vars
```vars
keepalived_bind_interface: eth0
keepalived_vip: 172.25.144.88
keepalived_major_version: 2.0
keepalived_minor_version: 5
keepalived_src_dir: /tmp/kube_deploy
keepalived_prefix_dir: /opt/ha/keepalived
keepalived_conf_dir: /etc/keepalived

haproxy_bind_port: 443
haproxy_user: haproxy
haproxy_group: haproxy
haproxy_auth_name: admin
haproxy_auth_pass: cloudos
haproxy_major_version: 1.8
haproxy_minor_version: 12
haproxy_src_dir: /tmp/kube_deploy
haproxy_prefix_dir: /opt/ha/haproxy
haproxy_conf_dir: /etc/haproxy
haproxy_make_option: TARGET=linux2628 CPU=x86_64 USE_OPENSSL=1 USE_ZLIB=1 USE_PCRE=1
haproxy_backend_servers:
  - { name: 'k8s-api-1', ip: "172.25.144.85", port: "6443"}
  - { name: 'k8s-api-2', ip: "172.25.144.86", port: "6443"}
  - { name: 'k8s-api-3', ip: "172.25.144.87", port: "6443"}
