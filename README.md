# litecoin node k8s yml

### 0. 
install minikube
install virtualbox

### 1. start k8s
```
$ kubectl create -f litecoind-k8s.yml
```

### 2. rpc request
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
