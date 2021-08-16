# Auto-K8s

This project is focused to deploy Kubernetes clusters for development using Vagrant virtual machines

# Environment
Tested on Fedora 32

## Requirements
* Vagrant
* Ansible

```sh
# Install packages
sudo dnf install -y ansible vagrant-libvirt qemu

# Start and enable libvirtd
sudo systemctl enable libvirtd
sudo systemctl start libvirtd

# Add our user to libvirt group
sudo usermod --append --groups libvirt `whoami`
```


## Deployment
```sh
vagrant up

vagrant status
```
