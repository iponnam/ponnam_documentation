# Kubernetes Binary Package Installations
## On machine WITH access to Internet
**Commands:***
```
wget -q -O - https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo sh -c 'echo deb http://apt.kubernetes.io/ kubernetes-xenial main > /etc/apt/sources.list.d/kubernetes.list'
sudo apt-get update
sudo apt-get install -y kubernetes-cni=0.5.1-00
sudo apt-get install -y kubelet=1.8.4-00
sudo apt-get install -y kubectl=1.8.4-00
sudo apt-get install -y kubeadm=1.8.4-00
```

## On machine WITHOUT access to Internet

1. Download all 4 Kubernetes debian packages "Version 1.8.4" on any linux server that have access to Internet
```
wget -q -O - https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo sh -c 'echo deb http://apt.kubernetes.io/ kubernetes-xenial main > /etc/apt/sources.list.d/kubernetes.list'
sudo apt-get update
sudo apt-get download kubernetes-cni=0.5.1-00
sudo apt-get download kubelet=1.8.4-00
sudo apt-get download kubectl=1.8.4-00
sudo apt-get download kubeadm=1.8.4-00
```
2. Copy the debian packages downloaded in step 1 to server when kubernetes needs to be installed
```
sudo dpkg -i <path-to-kubernetes-cni=0.5.1-00-file>
sudo dpkg -i <path-to-kubelet=1.8.4-00-file>
sudo dpkg -i <path-to-kubectl=1.8.4-00-file>
sudo dpkg -i <path-to-kubeadm=1.8.4-00-file>
```
