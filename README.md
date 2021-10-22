````
#!/bin/sh

kubeadm alpha certs check-expiration
kubeadm init phase kubelet-finalize all
kubeadm alpha certs renew all
cd /etc/kubernetes
kubeadm alpha kubeconfig user --org system:nodes --client-name system:node:$(hostname) > kubelet.conf
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
restart server
kubectl get nodes

````
