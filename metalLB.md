# Edit kube-proxy config 

kubectl edit configmap -n kube-system kube-proxy

apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: "ipvs"
ipvs:
  strictARP: true      <-----------

# -------------------------------------------------------------------

# To install MetalLB, apply the manifest

kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.12/config/manifests/metallb-native.yaml


# -------------------------------------------------------------------

# create ml.yaml


apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: production
  namespace: metallb-system
spec:
  addresses:
  - 192.168.1.150-192.168.1.160                        <--- (Paste your ip pool)


# control result

kubectl apply -f ml.yaml

kubectl -n metallb-system get IPAddressPool

# create l2.yaml

apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: example
  namespace: metallb-system
spec:
  ipAddressPools:
    - production

# -------------------------------------------------------------------
kubectl apply -f l2.yaml


