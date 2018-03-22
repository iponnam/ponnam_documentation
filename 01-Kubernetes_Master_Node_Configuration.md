# Kubernetes Cluster Configuration


## Kubernetes Master Initiation

```
sudo kubeadm init --kubernetes-version=v1.8.4
```

By issuing the above kubeadm init command, it shall configure API Server, ETCD, Controller, & Scheduler components of Kubernetes along with certificates required for bootstrapping Worker Nodes. Once successfully initiated, it will spit out a token and discovery hash for the worker nodes to boot strap. The Token expires in 24 hours and the discovery-token-ca-cert-hash needs to be saved to be able to bootstrap the worker nodes.

**Example:**
 
*kubeadm join --token 845de7.d621e30286c92936 10.138.0.2:6443 --discovery-token-ca-cert-hash sha256:927cfea2e0e01aefaa7518ed5ead06734213df1afe68f2730f9761d7a0474ea4*

### Following Commands need to executed by the user with which the kubernetes management is to be done.

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Weave Network Over-Lay Configuration

*This will automatically monitors Kubernetes for any NetworkPolicy annotations on all namespaces and configures **iptables** rules to allow or block traffic as directed by the policies*

**Command:**

```
export kubever=$(kubectl version | base64 | tr -d '\n')
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
```


## Kubernetes Worker Node configuration
**__Repeat this step on all the worker nodes on which the pods/containers needs to be scheduled and managed by the master node__**

Command: 
```
sudo kubeadm join --token <token#> --discovery-token-ca-cert-hash <discovery certificate hash>
```
Example:
*kubeadm join --token 845de7.d621e30286c92936 10.138.0.2:6443 --discovery-token-ca-cert-hash sha256:927cfea2e0e01aefaa7518ed5ead06734213df1afe68f2730f9761d7a0474ea4*

