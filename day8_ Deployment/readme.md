# day8
Kubernetes 官方提供我們 Deployment 元件，不只能幫我們做到Pod scaling，對於 應用服務的rollout與rollback的支援也非常豐富。今天學習筆記內容如下：

* 認識 Replication Set
* 介紹 Deployment
* 實作：透過 kubectl 操作 Deployment 物件 & 常用指令

# Replica Set
* Replica Sets 可以說是進化版的 Replication Controller，與 Replication Controller最大的差異在於，Replica Sets 提供了更彈性的selector。
* 雖然Replica Set提供更彈性的selector，並不推薦開發者直接使用kubectl create等指令創建Replica Set 物件，而是透過 Deployment 來創建新的 Replica Set。

# Deployment
Deployment 可以幫我們達成以下幾件事情：


* 部署一個應用服務(application)
* 協助 applications 升級到某個特定版本
* 服務升級過程中做到無停機服務遷移(zero downtime deployment)
* 可以Rollback到先前版本

* 查詢version
`kubectl version`
* 接著我們可以使用kubectl create指令來新建一個 Deployment 物件
`kubectl create -f ./my-deployment.yaml`
* 查詢狀態
`kubectl get deployment`
`kubectl get pods --show-labels`
* 取得Deployment/Replication Set/Pod 基本資訊
* 如果想要看Kubernetes Cluster中所有目前已被建立的物件，可以使用kubectl get all一次取得所有資訊。
`kubectl get all`

* 我們使用kubectl create創建好一個Deployment物件之後，我們可以先創建一個 Service 來測試是否我們創建的web app有正常運作。
`kubectl expose deploy hello-deployment --type=NodePort --name=my-deployment-service`
* 創建好之後，我們可以透過minikube server取得目前my-deployment-service的網址
`minikube service my-deployment-service --url`
* 接著我們可以透過docker set image來rollout我們的 web app (升級pod中container的狀態)
`kubectl set image deploy/hello-deployment my-pod=kimlin20011/docker_node_test:v2.0.0`
`kubectl set image deploy/hello-deployment my-pod=kimlin20011/docker_node_test:v3.0.0`
* 這時我們也可以使用kubectl rollout status來查詢目前升級的狀態
`kubectl rollout status deploy hello-deployment`
* 如果再重新訪問一次 http://127.0.0.1:64662，可以看到瀏覽器的畫面變成Hello World! v2了！

* 除了透過kubectl set image更新Pod的image之外，我們也可以透過kubectl edit來進行更新，輸入以下指令
`kubectl edit deploy hello-deployment`

* 我們可以透過kubectl rollout history來查該Deployment看過去rollout的紀錄
`kubectl rollout history deploy hello-deployment`

* 而若是kubectl set image 指令結尾加上 --record，CHANG-CAUSE可以紀錄我們每次rollout的指令。例如，我們希望將Pod“升級”到latest版本。
`kubectl set image deploy/hello-deployment my-pod=kimlin20011/docker_node_test:v3.0.0 --record`
* 如果我們想要把目前版本rollback到上一版，我們可以使用kubectl rollout undo
`kubectl rollout undo deployment hello-deployment`

* 要介紹的是如何rollback回到特定版本，已回滾到REVISION 3為例，可以使用以下指令
kubectl rollout undo deploy hello-deployment --to-revision=3
