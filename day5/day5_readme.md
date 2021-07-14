# DAY5  在 Minikube 上跑起你的 Docker Containers - Pod & kubectl 常用指令

## 認識 Kubernetes 最小運行單位 - Pod
* 每個 Pod 都有屬於自己的 yaml 檔
* 一個 Pod 裡面可以包含一個或多個 Docker Container
* 在同一個 Pod 裡面的 containers，可以用 local port numbers 來互相溝通

## 建立一個 Pod
* 先建立一個yaml檔案
```yaml
# my-first-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: webserver
spec:
  containers:
  - name: pod-demo
    image: kimlin20011/docker_node_test
    ports:
    - containerPort: 3000
```
* apiVersion 是代表目前 Kubernetes 中該元件的版本號
* 在 metadata 中，有三個重要的 Key，分別是 name, labels, annotations
    * metadata.labels 是 Kubernetes 的是核心的元件之一，Kubernetes 會透過 Label Selector 將Pod分群管理
    * annotations 通常是使用者任意自定義的附加資訊
* spec
    * 最後 spec 的部分則是定義 container，在這個範例中，一個 Pod 只運行一個 container
    * port: container 有哪些 port number 是允許外部資源存取

## 在 Kubernetes Cluster 中建立 Pod 物件
* `kubectl create` 指令在 Kubernetes Cluster 中建立 Pod 物件
`kubectl create -f my-first-pod.yaml`
* 查看目前 pods 的狀態
`kubectl get pods`
* kubectl describe 指令可以看到更多關於 my-pod 這個物件的資訊
`kubectl describe pods my-pod`

## 如何與 Pod 中的 container 互動
* 法一：kubectl 提供一個指令 port-forward 能將pod中的某個port number，與本機端的port做mapping
    * 打開  http://127.0.0.1:8000  確認是否成功
`kubectl port-forward my-pod 8000:3000`
* 方法二：建立一個service `kubectl expose `
    * kubectl port-forward 是將 pod 的 port mapping 到本機端上，而 kubectl expose 則是將 pod 的 port 與 Kubernete Cluster 的 port number 做 mapping。
    * 我們先可以使用 `minikube status`查看目前 minikube-vm 是使用本機端哪個內部網址
* 可以查看pot底下的資料夾
`kubectl exec my-pod -- ls /app`

## 使用 alpine 查看 cluster 狀況
* 安裝
`kubectl run -i --tty alpine --image=alpine --restart=Never -- sh`
* 這時我們可以用 curl 指令去訪問Pod中container跑在port number 3000的API service 。不過我們需要先在 alpine 中安裝 curl 套件。
`apk add --no-cache curl`
* 查看 Pod 目前 Cluster IP
`kubectl describe pod my-pod`
* 查看玩IP，輸入
`curl http://172.17.0.4:3000`