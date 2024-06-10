# Auto-K8s

This project is focused to deploy Kubernetes clusters for development using Vagrant virtual machines

# Environment

Tested on:

- Fedora 34
- Fedora 39

## Requirements

- Vagrant
- Ansible

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
export AUTO_K8S_FLAVOUR=<FLAVOUR>
cd vagrant
vagrant up
vagrant status
cd ..
```

### Launch Ansible

```sh
# To install Kubernetes
ansible-playbook -i inventories/${AUTO_K8S_FLAVOUR} -b site.yaml

# To remove Kubernetes
ansible-playbook -i inventories/${AUTO_K8S_FLAVOUR} -b clean.yaml
```

### Flavours

1. Simple: Simple k8s cluster without storage, single master node and two
   workers.

   ```sh
   export AUTO_K8S_FLAVOUR=simple
   cd vagrant
   vagrant up
   vagrant status
   cd -
   ansible-playbook -i inventories/$AUTO_K8S_FLAVOUR -b site.yaml
   ```

2. NFS-Storage: Deploys one master and two workers and a fourth server for
   providing NFS Storage.

   ```sh
   export AUTO_K8S_FLAVOUR=nfs-storage
   cd vagrant
   vagrant up
   vagrant status
   cd -
   ansible-playbook -i inventories/$AUTO_K8S_FLAVOUR -b site.yaml

   # Test
   export KUBECONFIG=./build/kubeconfig
   kubectl apply -f inventories/nfs-storage/nfs-storage-test.yaml
   ```

### Accessing Kubernetes

```sh
export KUBECONFIG=./build/kubeconfig
kubectl get nodes
kubectl get pods -A
```
