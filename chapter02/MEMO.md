# MEMO

## 環境構築

kubectl のインストール

```console
$ sudo pacman -S kubectl 
resolving dependencies...
looking for conflicting packages...

Packages (1) kubectl-1.37.2-1

Total Download Size:   17.11 MiB
Total Installed Size:  87.90 MiB

:: Proceed with installation? [Y/n] 
:: Retrieving packages...
 kubectl-1.37.2-1-x86_64                                                                 17.1 MiB  54.1 MiB/s 00:00 [#####################################################################] 100%
(1/1) checking keys in keyring                                                                                      [#####################################################################] 100%
(1/1) checking package integrity                                                                                    [#####################################################################] 100%
(1/1) loading package files                                                                                         [#####################################################################] 100%
(1/1) checking for file conflicts                                                                                   [#####################################################################] 100%
(1/1) checking available disk space                                                                                 [#####################################################################] 100%
:: Processing package changes...
(1/1) installing kubectl                                                                                            [#####################################################################] 100%
:: Running post-transaction hooks...
(1/1) Arming ConditionNeedsUpdate...
```

kind のインストール。pacman経由でインストールできた

```console
$ sudo pacman -S kind 
resolving dependencies...
looking for conflicting packages...

Packages (1) kind-0.33.0-1

Total Download Size:    3.61 MiB
Total Installed Size:  10.63 MiB

:: Proceed with installation? [Y/n] 
:: Retrieving packages...
 kind-0.33.0-1-x86_64                                                                     3.6 MiB  9.99 MiB/s 00:00 [#####################################################################] 100%
(1/1) checking keys in keyring                                                                                      [#####################################################################] 100%
(1/1) checking package integrity                                                                                    [#####################################################################] 100%
(1/1) loading package files                                                                                         [#####################################################################] 100%
(1/1) checking for file conflicts                                                                                   [#####################################################################] 100%
(1/1) checking available disk space                                                                                 [#####################################################################] 100%
:: Processing package changes...
(1/1) installing kind                                                                                               [#####################################################################] 100%
Optional dependencies for kind
    docker: docker node provider [installed]
    podman: podman node provider
    nerdctl: nerdctl node provider
:: Running post-transaction hooks...
(1/1) Arming ConditionNeedsUpdate...
```

kind の確認

```console
$ kind version
kind v0.33.0 go1.27.0-X:nodwarf5 linux/amd64
```

クラスタ構築。古いかもしれないけど、バージョンは書籍にあわせておく

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

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```

kubectlで起動したか確認

```console
$ kubectl cluster-info --context kind-kind
Kubernetes control plane is running at https://127.0.0.1:45103
CoreDNS is running at https://127.0.0.1:45103/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

kubectl の設定確認

```console
$ ls -l ~/.kube 
total 12
drwxr-x--- 4 kei kei 4096 11月  8 16:00 cache/
-rw------- 1 kei kei 5602 11月  8 15:59 config
```

kindで作ったクラスタの削除

```console
$ kind delete cluster
Deleting cluster "kind" ...
Deleted nodes: ["kind-control-plane"]
```
