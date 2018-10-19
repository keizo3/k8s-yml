# litecoin node k8s yml

### 0. 
install minikube
https://kubernetes.io/docs/tasks/tools/install-minikube/

install virtualbox
https://www.virtualbox.org/wiki/Downloads

### 1. start minikube
```
$ minikube --vm-driver=virtualbox start
```

### 2. start k8s
```
$ kubectl create -f litecoind-k8s.yml
```

### 3. rpc request
```
$ curl --user test --data-binary '{"id":"t0", "method": "getinfo", "params": [] }' http://192.168.99.100:30003
Enter host password for user 'test': //enter "test"
{"result":{"version":140200,"protocolversion":70015,"walletversion":130000,"balance":0.00000000,"blocks":2256,"timeoffset":-1,"connections":8,"proxy":"","difficulty":0.0009000699454675865,"testnet":true,"keypoololdest":1539853668,"keypoolsize":100,"paytxfee":0.00000000,"relayfee":0.00100000,"errors":""},"error":null,"id":"t0"}
```

### delete k8s service and deployment
```
$ kubectl delete service litecoind-nodeport
$ kubectl delete deployments litecoind
```

##### auto scaling
https://qiita.com/sheepland/items/37ea0b77df9a4b4c9d80

### generate rpcauth
```
$ curl -sSL https://raw.githubusercontent.com/litecoin-project/litecoin/master/share/rpcauth/rpcauth.py | python - test
String to be appended to litecoin.conf:
rpcauth=test:295f2e1930a0e1bba3914e433690c3e3$dd81d68225a60b7cc9ac10b940c0a74bea59395ea75ccec8a18ecc48282fedc4
Your password:
xDOdimwdOde2Gy-NfsV4RcKNQxB98GR7q7nbi9Ul8Fs=
```

### use local docker image
```
eval $(minikube docker-env)
```

### minikube mount mac volume
```
echo "/Users -network 192.168.99.0 -mask 255.255.255.0 -alldirs -maproot=root:wheel" | sudo tee -a /etc/exports
sudo nfsd restart

minikube --vm-driver=virtualbox start
minikube ssh -- sudo umount /Users
minikube ssh -- sudo /usr/local/etc/init.d/nfs-client start
minikube ssh -- sudo mount 192.168.99.1:/Users /Users -o rw,async,noatime,rsize=32768,wsize=32768,proto=tcp
```