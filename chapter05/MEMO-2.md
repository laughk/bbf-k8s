# MEMO-3

chapter05 の 5.6 の内容に関するメモ

## 準備

書籍通りに準備をする

```console
$ kubectl apply --filename ~/ghq/github.com/aoi1/bbf-kubernetes/chapter-05/myapp.yaml -n default
pod/myapp created

$ k get pod myapp --namespace default 
NAME    READY   STATUS    RESTARTS   AGE
myapp   1/1     Running   0          110s
```

## アプリケーションを壊す

これで壊れるっぽい

```console
$ k apply --filename ~/ghq/github.com/aoi1/bbf-kubernetes/chapter-05/pod-destruction.yaml --namespace default 
pod/myapp configured
```

状況をみてみる

```console
$ k get pod myapp 
NAME    READY   STATUS             RESTARTS   AGE
myapp   0/1     ImagePullBackOff   0          3m23s
```

ImagePullBackOff, pull する際に何か問題になっている？ describe して Pod の詳細をみる

```console
$ k describe pod myapp 
Name:             myapp
Namespace:        default
Priority:         0
Service Account:  default
Node:             kind-control-plane/172.19.0.2
Start Time:       Sun, 08 Nov 2026 21:24:37 +0900
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.10
IPs:
  IP:  10.244.0.10
Containers:
  hello-server:
    Container ID:   containerd://66008f2d7024a69aec26908bfe33fc58b461281ddfd796b0a1e5dc76e0eb0a8d
    Image:          blux2/hello-server:1.1
    Image ID:       docker.io/blux2/hello-server@sha256:35ab584cbe96a15ad1fb6212824b3220935d6ac9d25b3703ba259973fac5697d
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    2
      Started:      Sun, 08 Nov 2026 21:24:37 +0900
      Finished:     Sun, 08 Nov 2026 21:27:15 +0900
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-w474k (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-w474k:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  4m9s               default-scheduler  Successfully assigned default/myapp to kind-control-plane
  Normal   Pulled     4m9s               kubelet            spec.containers{hello-server}: Container image "blux2/hello-server:1.0" already present on machine
  Normal   Created    4m9s               kubelet            spec.containers{hello-server}: Created container hello-server
  Normal   Started    4m9s               kubelet            spec.containers{hello-server}: Started container hello-server
  Normal   Killing    91s                kubelet            spec.containers{hello-server}: Container hello-server definition changed, will be restarted
  Normal   Pulling    50s (x3 over 91s)  kubelet            spec.containers{hello-server}: Pulling image "blux2/hello-server:1.1"
  Warning  Failed     49s (x3 over 89s)  kubelet            spec.containers{hello-server}: Failed to pull image "blux2/hello-server:1.1": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/blux2/hello-server:1.1": failed to resolve reference "docker.io/blux2/hello-server:1.1": docker.io/blux2/hello-server:1.1: not found
  Warning  Failed     49s (x3 over 89s)  kubelet            spec.containers{hello-server}: Error: ErrImagePull
  Warning  BackOff    24s (x2 over 38s)  kubelet            spec.containers{hello-server}: Back-off restarting failed container hello-server in pod myapp_default(0ec38a07-3456-411a-8236-760489a55e0c)
  Normal   BackOff    9s (x3 over 89s)   kubelet            spec.containers{hello-server}: Back-off pulling image "blux2/hello-server:1.1"
  Warning  Failed     9s (x3 over 89s)   kubelet            spec.containers{hello-server}: Error: ImagePullBackOff
```

以下のあたりっぽい

```
  Warning  Failed     49s (x3 over 89s)  kubelet            spec.containers{hello-server}: Failed to pull image "blux2/hello-server:1.1": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/blux2/hello-server:1.1": failed to resolve reference "docker.io/blux2/hello-server:1.1": docker.io/blux2/hello-server:1.1: not found
  Warning  Failed     49s (x3 over 89s)  kubelet            spec.containers{hello-server}: Error: ErrImagePull
  Warning  BackOff    24s (x2 over 38s)  kubelet            spec.containers{hello-server}: Back-off restarting failed container hello-server in pod myapp_default(0ec38a07-3456-411a-8236-760489a55e0c)
```

> docker.io/blux2/hello-server:1.1: not found

と出ているので、指定したコンテナイメージが dockerhub に無いっぽい。

実際、 https://hub.docker.com/r/blux2/hello-server/tags をみてみると `1.1` が無い。  
ここでどのタグに修正するべきかの判断が必要だが、ここは書籍に従うことにする。書籍だと `1.0` にする方針で修正するっぽい

```console
$ k edit pod myapp
pod/myapp edited
```

```console
$ k describe pod myapp  | rg "Image:"    
    Image:          blux2/hello-server:1.0
```

修正したところ、すぐに Running になった

```
$ k get pod
NAME    READY   STATUS    RESTARTS        AGE
myapp   1/1     Running   1 (7m33s ago)   10m
```

最後に掃除をする

```console
$ k delete pod myapp 
pod "myapp" deleted from default namespace

$ k get pod
No resources found in default namespace.
```


