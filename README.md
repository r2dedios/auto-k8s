# Auto-K8s

This project is focused to deploy Kubernetes clusters for development using Vagrant virtual machines

# Environment
Tested on Fedora 34

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
### Create VMs
```sh
cd vagrant
vagrant --cluster-flavour=./config/<FLAVOUR>.yaml up
vagrant --cluster-flavour=./config/<FLAVOUR>.yaml status
cd ..
```

### Launch Ansible
```sh
# To install Kubernetes
ansible-playbook -i inventories/<FLAVOUR> -b site.yaml

# To remove Kubernetes
ansible-playbook -i inventories/<FLAVOUR> -b clean.yaml
```

### Examples
```sh
export AUTO_K8S_FLAVOUR=simple
cd vagrant
vagrant --cluster-flavour=./config/$AUTO_K8S_FLAVOUR.yaml up
vagrant --cluster-flavour=./config/$AUTO_K8S_FLAVOUR.yaml status
cd -
ansible-playbook -i inventories/$AUTO_K8S_FLAVOUR -b site.yaml
```


### Access Kubernetes
```sh
kubectl --kubeconfig=./build/kubeconfig get nodes
kubectl --kubeconfig=./build/kubeconfig get pods -A
```

### Example
This section contains an example deployment using the "simple" infrastructure
flavour
```sh
cd vagrant
vagrant --cluster-flavour=./config/simple.yaml up
vagrant --cluster-flavour=./config/simple.yaml status
cd ..

ansible-playbook -i inventories/simple -b site.yaml

export KUBECONFIG=./build/kubeconfig
kubectl get nodes
```

