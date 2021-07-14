# day6 - 實際環境運行的 Kubernetes - Node & Architecture Overview
>介紹 Node 是什麼
>概觀 Kubernetes 的內部運作

## Node 是什麼
* 在 Kubernetes 中，Node 通常是指實體機、虛擬機。
* 只要他們上面裝有 Docker Engine ，足以跑起 Pod，就可以被加入 Kubernetes Cluster。
* 若還是記得 第二天在本機端架好的 minikube，輸入 `kubectl get nodes` 指令，就可以發現 minikube 已在其中
* 在 Kubernetes 上，當我們將 Node 都加到 Kubernetes Cluster 後，系統會根據目前 Pod 的設定檔去決定要部署在哪個 Node 上。
* 在實際場景應用上，Kubernetes 是會由多台 Node 組成


![](https://i.imgur.com/LBaMhV0.png)
* 一個橘色框分別代表一個 Pod ，而 Pod 中綠色的空心方型則代表一個container。以 Pod 3 與 Pod 5 為例，這兩個 Pod 分別只運行一個 container；而在 Pod 1 中則運行三個 containers。
* 圖中每個 Node 都有屬於它自己的 iptables，iptables 是 Linux 上的防火牆(firewall)，不只限制哪些連線可以連進來，也會管理網路連線，決定收到的 request 要交給哪個 Pod。
* 該圖中的 Kubernetes Cluster 有N 個 Nodes，Node 1, Node 2, ... Node N
* 同一個 Pod 中的 containers，可以透過 Pod 內部的網路直接溝通
* Kubelet & kube-proxy & Load balancing
    * kube-prox y則是會將目前該 Node 上所有 Pods 的資訊傳給 iptables，讓 iptables 即時獲得在該 Node 上所有 Pod 的最新狀態。好比當一個 Pod物件 被建立時，kube-proxy 會通知 iptables，以確保該 Pod 可以被 Kubernetes Cluster 中的其他物件存取。
* 所以，當我們創建一個新的 Pod 後，收到使用者發送的 request 時，ks8 內部會發生什麼事呢
    * 首先，kubelet 會先收到 master node 指令，創建一個 Pod。創建好後，kube-proxy 會去告知 iptables，目前該 Pod 可用。\
    * 當使用者透過 網路(internet) 發送 requests 時，requests 會先送到 Load Balancer，由 Load Balancer 決定要把request交給哪個 Node，這時收到 requests的 Node 會經由 iptables 決定要送給哪個 Pod 。
    * 但如果收到request的 Node 恰好沒有相對應可以處理 request Pod 的話，原本收到 request 的 Node 會透過 iptables 把 request 轉給其他有可以處理這個 request 的 Node 。