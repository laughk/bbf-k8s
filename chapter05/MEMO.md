# MEMO

トラブルシューティングガイド の内容に関連するメモ

## 事前準備

どうもコラムで紹介されていた `myapp2` も必要だったっぽい。  
起動する

```console
$ kubectl run myapp02 --image=blux2/hello-server:1.0 --namespace default
pod/myapp02 created
```

## ハンズオン

リソースの取得

### Podの情報

```console
$ kubectl get pod --namespace default
NAME      READY   STATUS    RESTARTS   AGE
myapp     1/1     Running   0          19m
myapp02   1/1     Running   0          54s
```

特定の Pod を指定

```console
$ kubectl get pod myapp --namespace default
NAME    READY   STATUS    RESTARTS   AGE
myapp   1/1     Running   0          20m
```

`--output` オプションで出力内容を変更する

```console
$ kubectl get pod --output wide --namespace default
NAME      READY   STATUS    RESTARTS   AGE     IP           NODE                 NOMINATED NODE   READINESS GATES
myapp     1/1     Running   0          20m     10.244.0.5   kind-control-plane   <none>           <none>
myapp02   1/1     Running   0          2m41s   10.244.0.6   kind-control-plane   <none>           <none>

$ kubectl get pod --output yaml --namespace default
apiVersion: v1
items:
- apiVersion: v1
  kind: Pod
  metadata:
    annotations:
      kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"myapp"},"name":"myapp","namespace":"default"},"spec":{"containers":[{"image":"blux2/hello-server:1.0","name":"hello-server","ports":[{"containerPort":8080}]}]}}
    creationTimestamp: "2026-11-08T07:29:17Z"
    labels:
      app: myapp
    name: myapp
    namespace: default
    resourceVersion: "1111"
    uid: f7f24270-5e8f-413e-afe7-69e5d6f13cec
  spec:
    containers:
    - image: blux2/hello-server:1.0
      imagePullPolicy: IfNotPresent
      name: hello-server
      ports:
      - containerPort: 8080
        protocol: TCP
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      volumeMounts:
      - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
        name: kube-api-access-ht884
        readOnly: true
    dnsPolicy: ClusterFirst
    enableServiceLinks: true
    nodeName: kind-control-plane
    preemptionPolicy: PreemptLowerPriority
    priority: 0
    restartPolicy: Always
    schedulerName: default-scheduler
    securityContext: {}
    serviceAccount: default
    serviceAccountName: default
    terminationGracePeriodSeconds: 30
    tolerations:
    - effect: NoExecute
      key: node.kubernetes.io/not-ready
      operator: Exists
      tolerationSeconds: 300
    - effect: NoExecute
      key: node.kubernetes.io/unreachable
      operator: Exists
      tolerationSeconds: 300
    volumes:
    - name: kube-api-access-ht884
      projected:
        defaultMode: 420
        sources:
        - serviceAccountToken:
            expirationSeconds: 3607
            path: token
        - configMap:
            items:
            - key: ca.crt
              path: ca.crt
            name: kube-root-ca.crt
        - downwardAPI:
            items:
            - fieldRef:
                apiVersion: v1
                fieldPath: metadata.namespace
              path: namespace
  status:
    conditions:
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:29:22Z"
      status: "True"
      type: PodReadyToStartContainers
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:29:17Z"
      status: "True"
      type: Initialized
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:29:22Z"
      status: "True"
      type: Ready
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:29:22Z"
      status: "True"
      type: ContainersReady
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:29:17Z"
      status: "True"
      type: PodScheduled
    containerStatuses:
    - containerID: containerd://a8656001ec89ba6a4888960ef82b9a2bc33dad8de420df10368f1fbb47b1c193
      image: docker.io/blux2/hello-server:1.0
      imageID: docker.io/blux2/hello-server@sha256:35ab584cbe96a15ad1fb6212824b3220935d6ac9d25b3703ba259973fac5697d
      lastState: {}
      name: hello-server
      ready: true
      restartCount: 0
      started: true
      state:
        running:
          startedAt: "2026-11-08T07:29:22Z"
    hostIP: 172.19.0.2
    hostIPs:
    - ip: 172.19.0.2
    phase: Running
    podIP: 10.244.0.5
    podIPs:
    - ip: 10.244.0.5
    qosClass: BestEffort
    startTime: "2026-11-08T07:29:17Z"
- apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: "2026-11-08T07:47:35Z"
    labels:
      run: myapp02
    name: myapp02
    namespace: default
    resourceVersion: "2536"
    uid: d57cf3ef-2411-4e8e-aea1-8e950059d862
  spec:
    containers:
    - image: blux2/hello-server:1.0
      imagePullPolicy: IfNotPresent
      name: myapp02
      resources: {}
      terminationMessagePath: /dev/termination-log
      terminationMessagePolicy: File
      volumeMounts:
      - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
        name: kube-api-access-kx7dh
        readOnly: true
    dnsPolicy: ClusterFirst
    enableServiceLinks: true
    nodeName: kind-control-plane
    preemptionPolicy: PreemptLowerPriority
    priority: 0
    restartPolicy: Always
    schedulerName: default-scheduler
    securityContext: {}
    serviceAccount: default
    serviceAccountName: default
    terminationGracePeriodSeconds: 30
    tolerations:
    - effect: NoExecute
      key: node.kubernetes.io/not-ready
      operator: Exists
      tolerationSeconds: 300
    - effect: NoExecute
      key: node.kubernetes.io/unreachable
      operator: Exists
      tolerationSeconds: 300
    volumes:
    - name: kube-api-access-kx7dh
      projected:
        defaultMode: 420
        sources:
        - serviceAccountToken:
            expirationSeconds: 3607
            path: token
        - configMap:
            items:
            - key: ca.crt
              path: ca.crt
            name: kube-root-ca.crt
        - downwardAPI:
            items:
            - fieldRef:
                apiVersion: v1
                fieldPath: metadata.namespace
              path: namespace
  status:
    conditions:
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:47:36Z"
      status: "True"
      type: PodReadyToStartContainers
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:47:35Z"
      status: "True"
      type: Initialized
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:47:36Z"
      status: "True"
      type: Ready
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:47:36Z"
      status: "True"
      type: ContainersReady
    - lastProbeTime: null
      lastTransitionTime: "2026-11-08T07:47:35Z"
      status: "True"
      type: PodScheduled
    containerStatuses:
    - containerID: containerd://113b12bc33eb85986c8fcadec189bde31ff49d67e7db20b56299ad6ee21048eb
      image: docker.io/blux2/hello-server:1.0
      imageID: docker.io/blux2/hello-server@sha256:35ab584cbe96a15ad1fb6212824b3220935d6ac9d25b3703ba259973fac5697d
      lastState: {}
      name: myapp02
      ready: true
      restartCount: 0
      started: true
      state:
        running:
          startedAt: "2026-11-08T07:47:35Z"
    hostIP: 172.19.0.2
    hostIPs:
    - ip: 172.19.0.2
    phase: Running
    podIP: 10.244.0.6
    podIPs:
    - ip: 10.244.0.6
    qosClass: BestEffort
    startTime: "2026-11-08T07:47:35Z"
kind: List
metadata:
  resourceVersion: ""
```

`--output yaml` の活用

```console
$ kubectl get pod myapp --output yaml --namespace default > chapter04/pod.yaml
```

マニュフェストとの差分を見る（書籍だと diff はオプション無しで実行しているが、見づらいので `-u` をつけている）

```diff
$ diff -u chapter04/{pod,myapp}.yaml 
--- chapter04/pod.yaml	2026-11-08 16:53:05.549255397 +0900
+++ chapter04/myapp.yaml	2026-11-08 16:29:21.826236509 +0900
@@ -1,110 +1,12 @@
 apiVersion: v1
 kind: Pod
 metadata:
-  annotations:
-    kubectl.kubernetes.io/last-applied-configuration: |
-      {"apiVersion":"v1","kind":"Pod","metadata":{"annotations":{},"labels":{"app":"myapp"},"name":"myapp","namespace":"default"},"spec":{"containers":[{"image":"blux2/hello-server:1.0","name":"hello-server","ports":[{"containerPort":8080}]}]}}
-  creationTimestamp: "2026-11-08T07:29:17Z"
+  name: myapp
   labels:
     app: myapp
-  name: myapp
-  namespace: default
-  resourceVersion: "1111"
-  uid: f7f24270-5e8f-413e-afe7-69e5d6f13cec
 spec:
   containers:
-  - image: blux2/hello-server:1.0
-    imagePullPolicy: IfNotPresent
-    name: hello-server
-    ports:
-    - containerPort: 8080
-      protocol: TCP
-    resources: {}
-    terminationMessagePath: /dev/termination-log
-    terminationMessagePolicy: File
-    volumeMounts:
-    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
-      name: kube-api-access-ht884
-      readOnly: true
-  dnsPolicy: ClusterFirst
-  enableServiceLinks: true
-  nodeName: kind-control-plane
-  preemptionPolicy: PreemptLowerPriority
-  priority: 0
-  restartPolicy: Always
-  schedulerName: default-scheduler
-  securityContext: {}
-  serviceAccount: default
-  serviceAccountName: default
-  terminationGracePeriodSeconds: 30
-  tolerations:
-  - effect: NoExecute
-    key: node.kubernetes.io/not-ready
-    operator: Exists
-    tolerationSeconds: 300
-  - effect: NoExecute
-    key: node.kubernetes.io/unreachable
-    operator: Exists
-    tolerationSeconds: 300
-  volumes:
-  - name: kube-api-access-ht884
-    projected:
-      defaultMode: 420
-      sources:
-      - serviceAccountToken:
-          expirationSeconds: 3607
-          path: token
-      - configMap:
-          items:
-          - key: ca.crt
-            path: ca.crt
-          name: kube-root-ca.crt
-      - downwardAPI:
-          items:
-          - fieldRef:
-              apiVersion: v1
-              fieldPath: metadata.namespace
-            path: namespace
-status:
-  conditions:
-  - lastProbeTime: null
-    lastTransitionTime: "2026-11-08T07:29:22Z"
-    status: "True"
-    type: PodReadyToStartContainers
-  - lastProbeTime: null
-    lastTransitionTime: "2026-11-08T07:29:17Z"
-    status: "True"
-    type: Initialized
-  - lastProbeTime: null
-    lastTransitionTime: "2026-11-08T07:29:22Z"
-    status: "True"
-    type: Ready
-  - lastProbeTime: null
-    lastTransitionTime: "2026-11-08T07:29:22Z"
-    status: "True"
-    type: ContainersReady
-  - lastProbeTime: null
-    lastTransitionTime: "2026-11-08T07:29:17Z"
-    status: "True"
-    type: PodScheduled
-  containerStatuses:
-  - containerID: containerd://a8656001ec89ba6a4888960ef82b9a2bc33dad8de420df10368f1fbb47b1c193
-    image: docker.io/blux2/hello-server:1.0
-    imageID: docker.io/blux2/hello-server@sha256:35ab584cbe96a15ad1fb6212824b3220935d6ac9d25b3703ba259973fac5697d
-    lastState: {}
-    name: hello-server
-    ready: true
-    restartCount: 0
-    started: true
-    state:
-      running:
-        startedAt: "2026-11-08T07:29:22Z"
-  hostIP: 172.19.0.2
-  hostIPs:
-  - ip: 172.19.0.2
-  phase: Running
-  podIP: 10.244.0.5
-  podIPs:
-  - ip: 10.244.0.5
-  qosClass: BestEffort
-  startTime: "2026-11-08T07:29:17Z"
+    - name: hello-server
+      image: blux2/hello-server:1.0
+      ports:
+        - containerPort: 8080
```

`--output` と `jsonpath` でフィールドを指定する

```console
$ kubectl get pod myapp --output jsonpath='{.spec.containers[].image}' --namespace default 
blux2/hello-server:1.0
```

`--output json` と `jq` を組み合わせて同様の結果を得ることもできる

```console
$ kubectl get pod myapp --output json --namespace default | jq '.spec.containers[].image'
"blux2/hello-server:1.0"
```

`--v` オプション。ログレベルを指定できる。 `-vvv` みたいにvの数ではなく数値でレベルを指定する

```console
$ kubectl get pod myapp --namespace default --v=7 
I1108 17:00:51.519124   36308 loader.go:408] Config loaded from file:  /home/kei/.kube/config
I1108 17:00:51.519367   36308 envvar.go:195] "Feature gate default state" feature="InOrderInformers" enabled=true
I1108 17:00:51.519385   36308 envvar.go:195] "Feature gate default state" feature="InformerResourceVersion" enabled=true
I1108 17:00:51.519393   36308 envvar.go:195] "Feature gate default state" feature="ClientsAllowCARotation" enabled=true
I1108 17:00:51.519402   36308 envvar.go:195] "Feature gate default state" feature="ClientsAllowTLSCacheGC" enabled=true
I1108 17:00:51.519411   36308 envvar.go:195] "Feature gate default state" feature="WatchListClient" enabled=true
I1108 17:00:51.519417   36308 envvar.go:195] "Feature gate default state" feature="AtomicFIFO" enabled=true
I1108 17:00:51.519424   36308 envvar.go:195] "Feature gate default state" feature="InOrderInformersBatchProcess" enabled=true
I1108 17:00:51.519431   36308 envvar.go:195] "Feature gate default state" feature="UnlockWhileProcessingFIFO" enabled=true
I1108 17:00:51.519437   36308 envvar.go:195] "Feature gate default state" feature="ClientsPreferCBOR" enabled=false
I1108 17:00:51.519444   36308 envvar.go:195] "Feature gate default state" feature="ClientsAllowCBOR" enabled=false
I1108 17:00:51.524886   36308 round_trippers.go:527] "Request" verb="GET" url="https://127.0.0.1:44127/api/v1/namespaces/default/pods/myapp" headers=<
	Accept: application/json;as=Table;v=v1;g=meta.k8s.io,application/json;as=Table;v=v1beta1;g=meta.k8s.io,application/json
	User-Agent: kubectl/v1.37.2 (linux/amd64) kubernetes/c50e539
 >
I1108 17:00:51.532394   36308 round_trippers.go:632] "Response" status="200 OK" milliseconds=7
NAME    READY   STATUS    RESTARTS   AGE
myapp   1/1     Running   0          31m
```

Podの詳細情報を見る（ describe ）

```console
$ kubectl describe pod myapp --namespace default
Name:             myapp
Namespace:        default
Priority:         0
Service Account:  default
Node:             kind-control-plane/172.19.0.2
Start Time:       Sun, 08 Nov 2026 16:29:17 +0900
Labels:           app=myapp
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  hello-server:
    Container ID:   containerd://a8656001ec89ba6a4888960ef82b9a2bc33dad8de420df10368f1fbb47b1c193
    Image:          blux2/hello-server:1.0
    Image ID:       docker.io/blux2/hello-server@sha256:35ab584cbe96a15ad1fb6212824b3220935d6ac9d25b3703ba259973fac5697d
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 08 Nov 2026 16:29:22 +0900
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-ht884 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       True 
  ContainersReady             True 
  PodScheduled                True 
Volumes:
  kube-api-access-ht884:
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
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  32m   default-scheduler  Successfully assigned default/myapp to kind-control-plane
  Normal  Pulling    32m   kubelet            spec.containers{hello-server}: Pulling image "blux2/hello-server:1.0"
  Normal  Pulled     32m   kubelet            spec.containers{hello-server}: Successfully pulled image "blux2/hello-server:1.0" in 4.342s (4.342s including waiting)
  Normal  Created    32m   kubelet            spec.containers{hello-server}: Created container hello-server
  Normal  Started    32m   kubelet            spec.containers{hello-server}: Started container hello-server
```

### コンテナログを見る

```console
$ kubectl logs myapp
2026/11/08 07:29:22 Starting server on port 8080

```

複数コンテナがいる場合は `--container(-c)` オプションで指定する

```console
$ kubectl logs myapp --container hello-server --namespace default
2026/11/08 07:29:22 Starting server on port 8080
```

Deployment 単位での指定。Deploymentについては Chapter06 で解説があるっぽい

```console
$ kubectl logs deploy/hello-server 
error: error from server (NotFound): deployments.apps "hello-server" not found in namespace "default"
```

コマンドはエラーになる。どうも書籍をみると「まだ Deployment リソースを作成していないため、こおでは実行例を載せておきます。」とあるので
多分自分の環境で実行する分には上記のエラーで問題なさそうに見える。

ラベルを指定するを試すために追加の apply （chapte05の内容を使うのでクローンしているサンプルコードからマニュフェストを使う）

```console
$ kubectl apply --filename ~/ghq/github.com/aoi1/bbf-kubernetes/chapter-05/myapp-label.yaml 
pod/myapp3 created

$ kubectl get pod --namespace default
NAME      READY   STATUS    RESTARTS   AGE
myapp     1/1     Running   0          59m
myapp02   1/1     Running   0          41m
myapp3    1/1     Running   0          8m31s
```

ラベル指定によるPodの参照

```console
$ kubectl get pod --selector app=myapp
NAME     READY   STATUS    RESTARTS   AGE
myapp    1/1     Running   0          60m
myapp3   1/1     Running   0          9m33s
```

log も同様にしてやってみる

```console
$ kubectl logs --selector app=myapp   
2026/11/08 07:29:22 Starting server on port 8080
2026/11/08 08:20:08 Starting server on port 8080
```

### 5.3 詳細な情報取得をやってみる

debug用のサイドカーコンテナ

```console
$ kubectl debug --stdin --tty myapp --image=curlimages/curl:8.4.0 --target=hello-server --namespace default
Targeting container "hello-server". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
Defaulting debug container name to debugger-clrgm.
curl: try 'curl --help' or 'curl --manual' for more information

```

`-- sh` をつけ忘れて起動したら上記のようになった。 `ENTRYPOINT` が `curl` になってるような挙動に見えた。 
コマンドを修正して再実行。

```console
$ kubectl debug --stdin --tty myapp --image=curlimages/curl:8.4.0 --target=hello-server --namespace default -- sh
Targeting container "hello-server". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
Defaulting debug container name to debugger-kjr28.
All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
~ $ 
~ $ curl localhost:8080
Hello, world!~ $ 
~ $ 
```

shell session となり、 curl も実行できた。実際は k8s クラスタの状況次第な気もするけど、
これだけ気楽にデバッグ環境を立ち上げられるのは便利だなと思った。

```console
~ $ 
Session ended, the ephemeral container will not be restarted but may be reattached using 'kubectl attach myapp -c debugger-kjr28 -n default -i -t' if it is still running
```

セッションを終了したときのメッセージ。後からアタッチもできそう？

---

`kubectl debug` が登場する前は `kubectl run` で debug 用の Pod を起動する必要があった模様。
以下は busybox で nslookup してみる例

```console
$ kubectl --namespace default run busybox --image=busybox:1.36.1 --rm --stdin --tty --restart=Never --command -- nslookup google.com
Server:		10.96.0.10
Address:	10.96.0.10:53

Non-authoritative answer:
Name:	google.com
Address: 142.250.23.138
Name:	google.com
Address: 142.250.23.139
Name:	google.com
Address: 142.250.23.100
Name:	google.com
Address: 142.250.23.102
Name:	google.com
Address: 142.250.23.113
Name:	google.com
Address: 142.250.23.101

Non-authoritative answer:
Name:	google.com
Address: 2404:6800:400b:c006::66
Name:	google.com
Address: 2404:6800:400b:c006::71
Name:	google.com
Address: 2404:6800:400b:c006::64
Name:	google.com
Address: 2404:6800:400b:c006::65

All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
warning: couldn't attach to pod/busybox, falling back to streaming logs: unable to upgrade connection: container busybox not found in pod busybox_default
Server:		10.96.0.10
Address:	10.96.0.10:53

Non-authoritative answer:
Name:	google.com
Address: 142.250.23.138
Name:	google.com
Address: 142.250.23.139
Name:	google.com
Address: 142.250.23.100
Name:	google.com
Address: 142.250.23.102
Name:	google.com
Address: 142.250.23.113
Name:	google.com
Address: 142.250.23.101

Non-authoritative answer:
Name:	google.com
Address: 2404:6800:400b:c006::66
Name:	google.com
Address: 2404:6800:400b:c006::71
Name:	google.com
Address: 2404:6800:400b:c006::64
Name:	google.com
Address: 2404:6800:400b:c006::65

pod "busybox" deleted from default namespace
```

一応なるほどなと思ったオプションもメモ

- `--rm`: 実行が完了したらPodを削除
- `--restart=Never`: Pod の再起動ポリシーを `Never` に設定する。終了しても再起動しないように。
- `--command --`: `--` の後に渡される拡張引数の1つ目が引数じゃなくてコマンドとして扱われるようにする

---

コンテナに直接ログインする。まずはログイン用のコンテナを起動（ Docker が出た初期の頃にみたテクニック ）

```console
$ kubectl --namespace default run curlpod --image=curlimages/curl:8.4.0 --command -- /bin/sh -c "while true; do sleep initify; done;"
pod/curlpod created

$ kubectl get pod --namespace default
NAME      READY   STATUS    RESTARTS   AGE
curlpod   1/1     Running   0          77s
myapp     1/1     Running   0          127m
myapp02   1/1     Running   0          109m
myapp3    1/1     Running   0          77m
```

myapp のIPアドレスを確認（ curl でアクセスする対象なので、ログインには使わない）

```console
$ kubectl get pod myapp --output wide --namespace default
NAME    READY   STATUS    RESTARTS   AGE    IP           NODE                 NOMINATED NODE   READINESS GATES
myapp   1/1     Running   0          128m   10.244.0.5   kind-control-plane   <none>           <none>
```

kubectl exec -> curl を実行してみる

```console
$ kubectl --namespace default exec --stdin --tty  curlpod -- /bin/sh  
~ $ curl 10.244.0.5:8080
Hello, world!~ $ 
~ $ 
```

pod のIPに対して疎通が通った

---

次はport-forwardをやってみる

```console
$ kubectl port-forward myapp 5555:8080 --namespace default
Forwarding from 127.0.0.1:5555 -> 8080
Forwarding from [::1]:5555 -> 8080
```

この状態で別ターミナルで curl を `5555` に対して実行

```console
$ curl localhost:5555
Hello, world!
```

また、ポートフォワードしているターミナルにもフォワーディング（？）のログがでた

```console
$ kubectl port-forward myapp 5555:8080 --namespace default
Forwarding from 127.0.0.1:5555 -> 8080
Forwarding from [::1]:5555 -> 8080
Handling connection for 5555
```

ちなみに、ローカル側を省略するとランダムなポートがフォワードされる模様

```console
$ kubectl port-forward myapp :8080 --namespace default 
Forwarding from 127.0.0.1:37243 -> 8080
Forwarding from [::1]:37243 -> 8080
Handling connection for 37243
^C

$ kubectl port-forward myapp :8080 --namespace default
Forwarding from 127.0.0.1:37993 -> 8080
Forwarding from [::1]:37993 -> 8080
^C 
```

---

### 5.4 障害を直すための kubectl

マニュフェストをその場でいじってみる。テキストログだとわからないけど、 `kubectl edit` を実行するとエディタ（自分の環境だと `neovim` ） が起動してマニュフェストの編集画面になる。
編集が完了すると `edited` になる

```console
$ kubectl edit pod myapp
pod/myapp edited
```

なにも編集しないでおわると `no changes made.` となる

```console
$ kubectl edit pod myapp
Edit cancelled, no changes made.
```

---

リソースの削除

削除前に現状を確認

```console
$ kubectl get pod --namespace default                              
NAME      READY   STATUS    RESTARTS   AGE
curlpod   1/1     Running   0          107m
myapp     1/1     Running   0          3h54m
myapp02   1/1     Running   0          3h36m
myapp3    1/1     Running   0          3h3m
```

myapp を削除してみる

```console
$ kubectl delete pod myapp --namespace default
pod "myapp" deleted from default namespace

$ kubectl get pod --namespace default         
NAME      READY   STATUS    RESTARTS   AGE
curlpod   1/1     Running   0          108m
myapp02   1/1     Running   0          3h36m
myapp3    1/1     Running   0          3h4m
```

確かに削除された。
実際のケースだと Deployment を利用した pod の再起動の際に利用されるケースが多い模様

---

最後に掃除

```console
$ kubectl delete pod curlpod myapp02 myapp3 --namespace default
pod "curlpod" deleted from default namespace
pod "myapp02" deleted from default namespace
pod "myapp3" deleted from default namespace

$ kubectl get pod --namespace default                          
No resources found in default namespace.

```
