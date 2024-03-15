

```
kind create cluster --config=small-cluster-config.yaml --name kind1

kind create cluster --config=small-cluster-config.yaml --name kind2



docker stop kind2-control-plane


docker ps --all
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS                        PORTS                       NAMES
8b600f2035cd   kindest/node:v1.29.2   "/usr/local/bin/entr…"   3 minutes ago   Exited (137) 41 seconds ago                               kind2-control-plane
20fb38caf7a8   kindest/node:v1.29.2   "/usr/local/bin/entr…"   4 minutes ago   Up 4 minutes                  127.0.0.1:51355->6443/tcp   kind1-control-plane

curl -k -v https://127.0.0.1:51355      #OK

```

Reboot Mac

```
docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS         PORTS                       NAMES
8b600f2035cd   kindest/node:v1.29.2   "/usr/local/bin/entr…"   10 minutes ago   Up 8 seconds   127.0.0.1:51359->6443/tcp   kind2-control-plane
20fb38caf7a8   kindest/node:v1.29.2   "/usr/local/bin/entr…"   11 minutes ago   Up 8 seconds   127.0.0.1:51355->6443/tcp   kind1-control-plane

curl -k https://127.0.0.1:51359 # OK
curl -k https://127.0.0.1:51355 # OK

kind create cluster --config=ha-cluster-config.yaml --name kind3

kind create cluster --config=ha-cluster-config.yaml --name kind4

docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED              STATUS              PORTS                       NAMES
13daa3db888b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:49631->6443/tcp   kind4-control-plane
7c284a66a38b   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   About a minute ago   Up About a minute   127.0.0.1:49632->6443/tcp   kind4-external-load-balancer
80b0def394f3   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:49634->6443/tcp   kind4-control-plane3
f166d4bc63cc   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute                               kind4-worker2
46ad6e1ca30b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute   127.0.0.1:49633->6443/tcp   kind4-control-plane2
86f6765302f6   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   About a minute ago   Up About a minute                               kind4-worker
d5e4b3331b14   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   4 minutes ago        Up 4 minutes        127.0.0.1:49591->6443/tcp   kind3-external-load-balancer
024ed48d2d1e   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   4 minutes ago        Up 4 minutes                                    kind3-worker2
4637fa03c299   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   4 minutes ago        Up 4 minutes        127.0.0.1:49594->6443/tcp   kind3-control-plane3
74a6164b317b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   4 minutes ago        Up 4 minutes                                    kind3-worker
f2310ffc36bd   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   4 minutes ago        Up 4 minutes        127.0.0.1:49593->6443/tcp   kind3-control-plane
18fb1e97e40b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   4 minutes ago        Up 4 minutes        127.0.0.1:49592->6443/tcp   kind3-control-plane2
8b600f2035cd   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   20 minutes ago       Up 10 minutes       127.0.0.1:51359->6443/tcp   kind2-control-plane
20fb38caf7a8   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   21 minutes ago       Up 10 minutes       127.0.0.1:51355->6443/tcp   kind1-control-plane



curl -k https://127.0.0.1:49591 # kind3 LB
curl -k https://127.0.0.1:49592 # kind3-control-plane2

curl -k https://127.0.0.1:49632 # kind4 LB
curl -k https://127.0.0.1:49633 # kind4-control-plane2


docker stop kind2-control-plane
docker stop kind4-external-load-balancer
docker stop kind4-worker
docker stop kind4-worker2
docker stop kind4-control-plane
docker stop kind4-control-plane2
docker stop kind4-control-plane3

```


Reboot Mac


```
docker ps -a
CONTAINER ID   IMAGE                                COMMAND                  CREATED          STATUS          PORTS                       NAMES
13daa3db888b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   12 minutes ago   Up 17 seconds   127.0.0.1:49631->6443/tcp   kind4-control-plane
7c284a66a38b   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   12 minutes ago   Up 16 seconds   127.0.0.1:49632->6443/tcp   kind4-external-load-balancer
80b0def394f3   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   12 minutes ago   Up 16 seconds   127.0.0.1:49634->6443/tcp   kind4-control-plane3
f166d4bc63cc   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   12 minutes ago   Up 17 seconds                               kind4-worker2
46ad6e1ca30b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   12 minutes ago   Up 16 seconds   127.0.0.1:49633->6443/tcp   kind4-control-plane2
86f6765302f6   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   12 minutes ago   Up 17 seconds                               kind4-worker
d5e4b3331b14   kindest/haproxy:v20230606-42a2262b   "haproxy -W -db -f /…"   14 minutes ago   Up 17 seconds   127.0.0.1:49591->6443/tcp   kind3-external-load-balancer
024ed48d2d1e   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   14 minutes ago   Up 17 seconds                               kind3-worker2
4637fa03c299   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   14 minutes ago   Up 17 seconds   127.0.0.1:49594->6443/tcp   kind3-control-plane3
74a6164b317b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   14 minutes ago   Up 17 seconds                               kind3-worker
f2310ffc36bd   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   14 minutes ago   Up 17 seconds   127.0.0.1:49593->6443/tcp   kind3-control-plane
18fb1e97e40b   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   14 minutes ago   Up 17 seconds   127.0.0.1:49592->6443/tcp   kind3-control-plane2
8b600f2035cd   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   30 minutes ago   Up 16 seconds   127.0.0.1:51359->6443/tcp   kind2-control-plane
20fb38caf7a8   kindest/node:v1.29.2                 "/usr/local/bin/entr…"   31 minutes ago   Up 17 seconds   127.0.0.1:51355->6443/tcp   kind1-control-plane


curl -k https://127.0.0.1:51359 # OK kind2-control-plane
curl -k https://127.0.0.1:51355 # OK kind1-control-plane

curl -k https://127.0.0.1:49591 # kind3 LB
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:49591

curl -k https://127.0.0.1:49592 # kind3-control-plane2
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:49592

curl -k https://127.0.0.1:49632 # kind4 LB
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:49632

curl -k https://127.0.0.1:49633 # kind4-control-plane2
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:49633


curl -k https://127.0.0.1:49593 # kind3-control-plane
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:49593

docker exec -it kind3-control-plane /bin/bash
root@kind3-control-plane:/# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 21:08 ?        00:00:00 /sbin/init
root         120       1  0 21:08 ?        00:00:00 /lib/systemd/systemd-journald
root         135       1  3 21:08 ?        00:00:13 /usr/local/bin/containerd
root         234       1  2 21:08 ?        00:00:09 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml --c
root         285       1  0 21:08 ?        00:00:00 /usr/local/bin/containerd-shim-runc-v2 -namespace k8s.io -id e8f92e5d73fc05ae8442e36629681ce7073140335ead865fdd1b855880d1841e -address /run/containerd/containerd.
root         291       1  0 21:08 ?        00:00:02 /usr/local/bin/containerd-shim-runc-v2 -namespace k8s.io -id fa9c83d3b6c80b8a87fe3809f405d34d0f17096fdbbbe4ef559b96108acbc4f5 -address /run/containerd/containerd.
root         318       1  0 21:08 ?        00:00:00 /usr/local/bin/containerd-shim-runc-v2 -namespace k8s.io -id b38f396ddb7a540a23dd2c072061bf7adcf7f91955fe9da458e3e9d1bcfd00d2 -address /run/containerd/containerd.
root         319       1  0 21:08 ?        00:00:00 /usr/local/bin/containerd-shim-runc-v2 -namespace k8s.io -id 91efad522113fc1591189e1f6a73224af6ef6b5e45c00ed3a500952c30557c07 -address /run/containerd/containerd.
65535        376     285  0 21:08 ?        00:00:00 /pause
65535        385     318  0 21:08 ?        00:00:00 /pause
65535        394     319  0 21:08 ?        00:00:00 /pause
65535        403     291  0 21:08 ?        00:00:00 /pause
root         491     318  1 21:08 ?        00:00:03 kube-scheduler --authentication-kubeconfig=/etc/kubernetes/scheduler.conf --authorization-kubeconfig=/etc/kubernetes/scheduler.conf --bind-address=127.0.0.1 --kub
root         515     319  1 21:08 ?        00:00:03 kube-controller-manager --allocate-node-cidrs=true --authentication-kubeconfig=/etc/kubernetes/controller-manager.conf --authorization-kubeconfig=/etc/kubernetes/
root        1011     291 22 21:12 ?        00:00:18 etcd --advertise-client-urls=https://172.18.0.7:2379 --cert-file=/etc/kubernetes/pki/etcd/server.crt --client-cert-auth=true --data-dir=/var/lib/etcd --experiment
root        1123       0  0 21:14 pts/1    00:00:00 /bin/bash
root        1129    1123  0 21:14 pts/1    00:00:00 ps -ef

# NO API SERVER

cd /var/log/containers

cat kube-apiserver-kind3-control-plane_kube-system_kube-apiserver-76bf020c7370cf735d456a208cf17c1dc0856e66cd227bbb237f1be170cc3341.log
....
2024-03-14T21:13:59.399795282Z stderr F W0314 21:13:59.399094       1 logging.go:59] [core] [Channel #5 SubChannel #6] grpc: addrConn.createTransport failed to connect to {Addr: "127.0.0.1:2379", ServerName: "127.0.0.1:2379", }. Err: connection error: desc = "transport: authentication handshake failed: context deadline exceeded"

cat etcd-kind3-control-plane_kube-system_etcd-8f8bb809b62ddaff4cc5af26da21ed588ef54d5f0c9e2ecbbaac944f9d518801.log
....
2024-03-14T21:16:57.04800154Z stderr F {"level":"warn","ts":"2024-03-14T21:16:57.047341Z","caller":"embed/config_logging.go:169","msg":"rejected connection","remote-addr":"172.18.0.7:33974","server-name":"","error":"remote error: tls: bad certificate"}

```


Cleanup:

```
docker stop $(docker ps -a -q)
docker container prune
docker volume prune --all --force
docker image prune --all --force
```

```

kind create cluster --config=ha-cluster-config.yaml --name kind5

curl -k https://127.0.0.1:49959
curl -k https://127.0.0.1:49960
```

https://github.com/kubernetes-sigs/kind/issues/148
https://github.com/kubernetes-sigs/kind/issues/2715

https://github.com/kubernetes-sigs/kind/issues/2045
https://github.com/kubernetes-sigs/kind/pull/2775