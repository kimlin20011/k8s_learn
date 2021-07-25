*  在建立Replication Controller之前，我們先檢查一下minikube狀態是否Ready
`kubectl get nodes`
* 確認沒問題之後，可以透過kubectl create的指令，創建一個新的Replication Controller物件，
`kubectl create -f my-replication-controller.yaml `
* 當新增replication controller成功之後，我們可以使用kubectl get rc查看目前狀態
`kubectl get rc`
`kubectl get pods`
* 如果這時我們手動刪除其中一個Pod，可以看到replication controller偵測到一個Pod終止服務時，產生另外一個新的Pod，來確保Pod的數量。
`kubectl delete pod my-replication-controller-7bcdx`
* 我們也可以透過kubectl scale來scaling Pod的數量，指令如下
`kubectl scale --replicas=4 -f ./my-replication-controller.yaml`
* 可以發現Pod的數量多一個了
`kubectl get rc`
* 也可以透過kubectl describe去看目前replication controller的詳細資料
`kubectl describe rc my-replication-controller`
* 當我們刪除replication controller時，要特別注意，由replication controller產生的pod也會因此而終止服務。
`kubectl delete rc my-replication-controller`
* 如果你希望刪掉replication controller之後，這些Pod仍然運行，可以指定 --cascade=false
`kubectl delete rc my-replication-controller --cascade=false`
* 如果Pod的數量很大，將會花一些時間來完成整個刪除動作，可以使用指令強迫pod立即結束。
`kubectl delete pods <pod> --grace-period=0 --force`
## note
* 終止pods
`kubectl delete pod my-pod`
