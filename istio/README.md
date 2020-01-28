 istio

## Kops configuration
```
kops get cluster --state=s3://kops-state-8485
kops edit cluster kubernetes.rohitnewtech.asia --state=s3://kops-state-8485


Add:
```
  kubeAPIServer:
    admissionControl:
    - NamespaceLifecycle
    - LimitRanger
    - ServiceAccount
    - PersistentVolumeLabel
    - DefaultStorageClass
    - DefaultTolerationSeconds
    - MutatingAdmissionWebhook
    - ValidatingAdmissionWebhook
    - ResourceQuota
    - NodeRestriction
    - Priority
```

## download (1.0.3):
```
cd ~
wget https://github.com/istio/istio/releases/download/1.4.3/istio-1.4.3-linux.tar.gz
tar -zxvf istio-1.4.3-linux.tar.gz 
chmod +x istio-1.4.3
cd istio-1.4.3

echo "export PATH=$PATH:$HOME/.local/bin:$HOME/bin:$HOME/istio-1.4.3/bin" >> ~/.bash_profile 
export PATH=$PATH:$HOME/.local/bin:$HOME/bin:$HOME/istio-1.4.3/bin

```

## Download (latest):
```
cd ~
curl -L https://git.io/getLatestIstio | sh -
echo 'export PATH="$PATH:/home/ubuntu/istio-1.0.3/bin"' >> ~/.profile # change 1.0.3 in your version
cd istio-1.0.3 # change 1.0.3 in your version
```

## Istio install

Apply CRDs:

```
kubectl apply -f ~/istio-1.0.3/install/kubernetes/helm/istio/templates/crds.yaml
```

Wait a few seconds.


Option 1: with no mutual TLS authentication
```
kubectl apply -f ~/istio-1.0.3/install/kubernetes/istio-demo.yaml
```

Option 2: or with mutual TLS authentication
```
kubectl apply -f ~/istio-1.0.3/install/kubernetes/istio-demo-auth.yaml
```

## Example app

### Example app (from istio)
```
export PATH="$PATH:/home/ubuntu/istio-1.0.3/bin"
kubectl apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)
```

### Hello world app 
```
export PATH="$PATH:/home/ubuntu/istio-1.0.3/bin"
kubectl apply -f <(istioctl kube-inject -f helloworld.yaml)
kubectl apply -f helloworld-gw.yaml
```

### Mutual TLS example
Create pods, services, destinationrules, virtualservices
```
kubectl create -f <(istioctl kube-inject -f helloworld-tls.yaml)
kubectl create -f helloworld-legacy.yaml
```

### End-user authentication
```
kubectl create -f <(istioctl kube-inject -f helloworld-jwt.yaml)
kubectl create -f helloworld-jwt-enable.yaml
```
