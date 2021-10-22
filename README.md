````
#!/bin/sh

mv /etc/kubernetes/pki/apiserver.key /etc/kubernetes/pki/apiserver.key.old
mv /etc/kubernetes/pki/apiserver.crt /etc/kubernetes/pki/apiserver.crt.old
mv /etc/kubernetes/pki/apiserver-kubelet-client.crt /etc/kubernetes/pki/apiserver-kubelet-client.crt.old
mv /etc/kubernetes/pki/apiserver-kubelet-client.key /etc/kubernetes/pki/apiserver-kubelet-client.key.old
mv /etc/kubernetes/pki/front-proxy-client.crt /etc/kubernetes/pki/front-proxy-client.crt.old
mv /etc/kubernetes/pki/front-proxy-client.key /etc/kubernetes/pki/front-proxy-client.key.old


kubeadm alpha phase certs apiserver  --config /root/kubeadm-kubetest.yaml
kubeadm alpha phase certs apiserver-kubelet-client
kubeadm alpha phase certs front-proxy-client
 
mv /etc/kubernetes/pki/apiserver-etcd-client.crt /etc/kubernetes/pki/apiserver-etcd-client.crt.old
mv /etc/kubernetes/pki/apiserver-etcd-client.key /etc/kubernetes/pki/apiserver-etcd-client.key.old
kubeadm alpha phase certs  apiserver-etcd-client


mv /etc/kubernetes/pki/etcd/server.crt /etc/kubernetes/pki/etcd/server.crt.old
mv /etc/kubernetes/pki/etcd/server.key /etc/kubernetes/pki/etcd/server.key.old
kubeadm alpha phase certs  etcd-server --config /root/kubeadm-kubetest.yaml

mv /etc/kubernetes/pki/etcd/healthcheck-client.crt /etc/kubernetes/pki/etcd/healthcheck-client.crt.old
mv /etc/kubernetes/pki/etcd/healthcheck-client.key /etc/kubernetes/pki/etcd/healthcheck-client.key.old
kubeadm alpha phase certs  etcd-healthcheck-client --config /root/kubeadm-kubetest.yaml


mv /etc/kubernetes/pki/etcd/peer.crt /etc/kubernetes/pki/etcd/peer.crt.old
mv /etc/kubernetes/pki/etcd/peer.key /etc/kubernetes/pki/etcd/peer.key.old
kubeadm alpha phase certs  etcd-peer --config /root/kubeadm-kubetest.yaml

*)  Backup old configuration files
mv /etc/kubernetes/admin.conf /etc/kubernetes/admin.conf.old
mv /etc/kubernetes/kubelet.conf /etc/kubernetes/kubelet.conf.old
mv /etc/kubernetes/controller-manager.conf /etc/kubernetes/controller-manager.conf.old
mv /etc/kubernetes/scheduler.conf /etc/kubernetes/scheduler.conf.old

kubeadm alpha phase kubeconfig all  --config /root/kubeadm-kubetest.yaml

mv $HOME/.kube/config .$HOMEkube/config.old
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
chmod 777 $HOME/.kube/config
export KUBECONFIG=.kube/config

````
