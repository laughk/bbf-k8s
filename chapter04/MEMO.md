# MEMO

## 環境の確認から再構築

書籍だと特に再構築の指示はないけど、やらないと進めなさそう

### 起動状況の確認

node の確認。クラスタ自体はchapter02の最後で壊したままなので当然無い

```console
$ kubectl get nodes
E1108 16:18:22.803100   28776 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:18:22.803528   28776 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:18:22.804975   28776 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:18:22.805399   28776 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:18:22.806968   28776 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

podも同様

```console
$ kubectl get pod --namespace kube-system
E1108 16:19:25.039998   28931 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:19:25.040553   28931 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:19:25.042099   28931 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:19:25.042527   28931 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
E1108 16:19:25.043975   28931 memcache.go:381] "Couldn't get current server API group list" err="Get \"http://localhost:8080/api?timeout=32s\": dial tcp [::1]:8080: connect: connection refused"
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

### クラスタ作り直し

書籍には無いが、 kind で再び作成する

```console
$ kind create cluster --image=kindest/node:v1.29.0
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.29.0) 🖼️
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Thanks for using kind! 😊
```

これで node も pod も見えるようになった

```console
$ kubectl get nodes                               
NAME                 STATUS     ROLES           AGE   VERSION
kind-control-plane   NotReady   control-plane   18s   v1.29.0

$ kubectl get pod --namespace kube-system         
NAME                                         READY   STATUS              RESTARTS   AGE
coredns-76f75df574-24bsj                     0/1     ContainerCreating   0          4s
coredns-76f75df574-p6529                     0/1     ContainerCreating   0          4s
etcd-kind-control-plane                      1/1     Running             0          19s
kindnet-nhtbb                                1/1     Running             0          4s
kube-apiserver-kind-control-plane            1/1     Running             0          19s
kube-controller-manager-kind-control-plane   1/1     Running             0          18s
kube-proxy-5q9cx                             1/1     Running             0          4s
kube-scheduler-kind-control-plane            1/1     Running             0          18s
```

## ハンズオン

これで chapter04 の 4.2 にうつれる

### 準備

kind でデフォルトクラスタが作成されているか確認

```console
$ kind get clusters
kind
```

マニュフェストを作成

- [myapp.yaml](./myapp.yaml)


マニュフェストをk8sクラスタへ適用: Podが存在しないことを確認

```console
$ kubectl get pod --namespace default
No resources found in default namespace.
```

マニュフェストをk8sクラスタへ適用:

```console
$ kubectl apply --filename chapter04/myapp.yaml --namespace default                  
Error from server (BadRequest): error when creating "chapter04/myapp.yaml": Pod in version "v1" cannot be handled as a Pod: strict decoding error: unknown field "spec.containers[0].images"
```

マニュフェストで `image` とするべきところを `images` にタイポしていた。

修正して再チャレンジ

```console
$ kubectl apply --filename chapter04/myapp.yaml --namespace default
pod/myapp created
```

podが作成されたことも確認できた

```console
$ kubectl get pod --namespace default                              
NAME    READY   STATUS    RESTARTS   AGE
myapp   1/1     Running   0          95s
```

## kubectl run

コラムに kubectrl run じゃないの？ という補足がまとまっていた。
読んでみると以下のような感じらしい

- `kubectl run` というコマンドはあるが、一時的な Pod の利用にとどめて通常はマニュフェストを使ったほうがいい
- マニュフェストにまとまってないと扱えない設定もある

何となくだけど、 ecs-deploy 的なものを感じた。あれもECSタスク定義がコード化されないのでサイドカーを入れるとかしようとすると面倒なことになったしな

内容を読んだだけで、特に手は動かさず
