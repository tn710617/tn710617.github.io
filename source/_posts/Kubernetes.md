---
title: Hello Kubernetes <hr> 你好！Kubernetes
date: 2019-02-17 10:46:40
tags:
    -   QUIKLABS
    -   GCP 
    -   Kubernetes
    -   Docker
category: Deployment
---
你好！ Node Kubernetes!
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
原文連結如下：

[Refer to QWIKLABS official website](https://www.qwiklabs.com/focuses/564?parent=catalog)
</p>

![](https://i.imgur.com/lLAZTuk.png)

## 本篇將會做什麼？
 - 建立一個 Node.js server
 - 建立一個 Docker container image
 - 建立一個 container cluster
 - 建立一個 Kubernetes pod
 - 擴大服務
 
## 基本需求
 - 需熟悉Linux內建的編輯器，像是vim, emacs, 或是nano
 
## 利用Google提供的臨時學習帳密登入
![](https://i.imgur.com/xjtczqE.png)

## 登入主控台
 - 左方為選單列
![](https://i.imgur.com/pJNHqjV.png)

 - 右上此按鈕可以打開Cloud Shell
![](https://i.imgur.com/aYuYmfB.png)
 
 - 點擊'START CLOUD SHELL'
![](https://i.imgur.com/5ZHGNvN.png)
 
## 建立Node.js 應用
 - 建立server.js檔案
     - vim server.js
 - 在檔案內新增以下代碼
 ```
 var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello World!");
}
var www = http.createServer(handleRequest);
www.listen(8080);
 ```
 - Cloud Shell以內建node，直接執行node server
     - `node server.js`
 - 使用Cloud Shell內建的Web預覽功能，開一個新的視窗並發請求到port 8080，如下圖：
![](https://i.imgur.com/QHMCIt2.png)

 - 結果如下：
 
![](https://i.imgur.com/BGXAP5h.png)

 - 在更進一步之前，讓我們先到Cloud Shell按下Ctrl+c停止正在運行中的node server，我們將打包這個運用，並置於Docker container內
 
## 建立一個Docker container image

 - 接下來，建立一個Dockerfile來敘述我們想要建立的image，Docker container images可以是已經存在image的延伸，所以我們將從已經存在的Node image來延伸
     - `vim Dockerfile`
     - 增加以下內容
     ```
     FROM node:6.9.2
     EXPOSE 8080
     COPY server.js .
     CMD node server.js
     ```
 - 上面的內容將：
     - 從Docker hub中開始一個找到的node image
     - 開啟port 8080
     - 複製server.js檔案到此docker image
     - 如之前操作般，手動開始node server
 
 - 輸入以下的指令以建立image，將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到
     - `docker build -t gcr.io/PROJECT_ID/hello-node:v1 .`
     - 接下來會花一些時間來下載以及擷取需要的東西，但你可以從進度條看到image建立的進度
     
 - 完成之後，於本地端使用下面的指令測試一下這個image，這個指令會從剛新建立的container image中，將Docker container以常駐的方式跑在port 8080，(將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到)

     - `docker run -d -p 8080:8080 gcr.io/PROJECT_ID/hello-node:v1`
 - 結果大概如下
     - `325301e6b2bffd1d0049c621866831316d653c0b25a496d04ce0ec6854cb7998`
 - 可使用Web預覽功能    
     - ![](https://i.imgur.com/AYrauID.png)
 - 或在Cloud Shell中使用curl
     - `curl http://localhost:8080`
 
## 停止Docker container 
 - 尋找Docker container ID
     - `docker ps`
 - 結果大概如下
 ```
CONTAINER ID        IMAGE                              COMMAND
2c66d0efcbd4        gcr.io/PROJECT_ID/hello-node:v1    "/bin/sh -c 'node 
 ```
 - 關閉docker container 
     - `docker stop containerID`
     
 - 結果會輸出你的container ID，如下：
     - `2c66d0efcbd4`
     
 - 現在，image如我們預期般的運作著，接下來我們將它推到`Google Container Registry`，一個可倍Google Cloud Projects存取的Docker image私人資料夾, 執行下面的指令(將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到)
     - `gcloud docker -- push gcr.io/PROJECT_ID/hello-node:v1`
     
 - Container image將會被列在主控台中，可從Navigation menu > Container Registry找到
 ![](https://i.imgur.com/WZacVJK.png)

 - 現在我們擁有project-wide的Docker image，可供Kubernetes存取以及編排
 ![](https://i.imgur.com/XA3xjVF.png)

## 建立cluster
 - 現在我們已經準備好可以建立Kubernetes Engine cluster。一個cluster內，有由Google的Kubernetes master API server，以及一組worker nodes。Woker nodes是Compute Engine virtual machines.
 - 確保我們已經使用gcould來設定我們的專案(將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到)
    - `gcloud config set project PROJECT_ID`
 - 使用兩個n1-standard-1 nodes來建立cluster(將會耗費幾分鐘) 
```
    gcloud container clusters create hello-world \
                --num-nodes 2 \
                --machine-type n1-standard-1 \
                --zone us-central1-a
```
 - 你也可以透過主控台來建立cluster:
    - `Kubernetes Engine > Kubernetes cluster > Create cluster`
    
 - cluster建立的區域，建議跟container registry使用的儲存區的所在區域一樣
 - `Vavigation menu > Kubernetes Engine` 可以看到，現在有一個由Kubernetes Engine驅動的完全運作的Kubernetes clsuter
 ![](https://i.imgur.com/kY7lfe6.png)
 - 接下來，是時候將我們容器化的application部署到Kubernetes cluster，從現在開始，我們將使用`kubectl`命令行（在Cloud Shell環境中，這已經被設定完畢）
 
## 建立pod 
 - Kubernetes pod由多個container組成，用於管理以及連結。它可以容納單一或多個containers。這邊我們將會使用儲存於私人的container registry，由Node.js image 建立的container。內容將會用在8080 port
 
 - 使用`kubectl run` 指令來建立一個pod(將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到)
     ```
     kubectl run hello-node \
    --image=gcr.io/PROJECT_ID/hello-node:v1 \
    --port=8080
     ```
 - 可以看到，我們已經建立一個deployment物件。Deployments是建立跟擴大pods推薦的方法。這邊，一個新的deployment管理一個執行`hello-node:v1` image的pod
     
     
## 允許外部連結
 - 在預設中，pod只可被cluster內部的ip存取。為了要讓hello-node container可倍Kubernetes virtual network之外的來源存取，我們必須設定pod成可被存取的Kubernates的服務
 - 在Cloud Shell，透過使用kubectl expose指令，並結合`--type="LoadBalancer" flag`, 我們可以讓pod可被公用網路存取。要建立一個外部存取IP，這個flag是必須的。
     - `kubectl expose deployment hello-node --type="LoadBalancer"`
     
 - 這個flag指定我們將使用underlying infrostructure提供的load-balancer (在此範例中，為Compute Engine load balancer)。需注意我們是使deployment可視化，並非直接暴露pod。這代表，產生的服務將會讀取所有由此depolyment管理的pod(於此範例中，為一個pod，但我們之後可以增加)
 
 - 此Kubernetes master建立了load balancer，相關的Compute Engline 轉發規格，target pools，以及防火牆規則， 所以服務可被Google Cloud Platform之外的來源所存取
 
 - 若要找公開可存取IP，可要求kubectl列出所有的cluster服務
     - `kubectl get services`
 - 結果如下：

```
NAME         CLUSTER-IP     EXTERNAL-IP      PORT(S)    AGE
hello-node   10.3.250.149   104.154.90.147   8080/TCP   1m
kubernetes   10.3.240.1     <none>           443/TCP    5m
```

 - 上圖可以看到，hello-node service有兩組IP，兩組都使用port 8080，CLUSTER-IP 是內部IP，只可被內部cloud vertial network所見，EXTERNAL-IP為外部load-balanced IP
 - 外部IP可能需要幾分鐘生效
 - 通過輸入IP來存取
     - http://<EXTERNAL_IP>:8080

## 擴充服務
 - 通過指令來擴充服務
     - `kubectl scale deployment hello-node --replicas=4`
 
 - 查看deployment
     - `kubectl get deployment`
 - 查看所有pod
     - `kubectl get pods`
 
 - 以下圖示顯示Kubernetes的大概運作方式: 
![](https://i.imgur.com/9NzaVco.png)

## 升級服務 
 - 某些時候，已經被部署的應用需要debug或增加新的功能。Kubernetes幫我們部署新的版本，並且不影響使用者
 - 首先，修改應用，編輯server.js
     - `vim server.js`
 - 更新回覆訊息
     - `response.end("Hello Kubernetes World!");`
 - 現在，我們可以透過往上增加的版本號，建立以及發布一個新的container image到registry。
 
 - 使用以下指令(將下面的PROJECT_ID替換成你的GCP Project ID，可以從主控台以及Connection Details區找到)
     - `docker build -t gcr.io/PROJECT_ID/hello-node:v2 .`
     - `gcloud docker -- push gcr.io/PROJECT_ID/hello-node:v2`
 - 編輯已經存在的hello-node deployment以及將image由`gcr.io/PROJECT_ID/hello-node:v1`變更為`gcr.io/PROJECT_ID/hello-node:v2`
     - `kubectl edit deployment hello-node`
 - 修改如下：
```bash
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: 2016-03-24T17:55:28Z
  generation: 3
  labels:
    run: hello-node
  name: hello-node
  namespace: default
  resourceVersion: "151017"
  selfLink: /apis/extensions/v1beta1/namespaces/default/deployments/hello-node
  uid: 981fe302-f1e9-11e5-9a78-42010af00005
spec:
  replicas: 4
  selector:
    matchLabels:
      run: hello-node
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: hello-node
    spec:
      containers:
      - image: gcr.io/PROJECT_ID/hello-node:v1 ## Update this line ##
        imagePullPolicy: IfNotPresent
        name: hello-node
        ports:
        - containerPort: 8080
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      securityContext: {}
      terminationGracePeriodSeconds: 30
```
     
 - 更新新的image到deployment，新的pods將被建立，舊的將被刪除
     - `kubectl get deployments`
 
## Kubernetes圖形化面板 (optional)
 - 取得cluster層級權限
     - `kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=$(gcloud config get-value account)`
 - 權限已經取得，建立新的面板服務
     - `kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml`
 
 - 編輯面板服務的表面形式
     - `kubectl -n kube-system edit service kubernetes-dashboard`
     - 將`type: ClusterIP`改成`type: NodePort`
 - 要登入Kubernetes面板，需要經過token驗證，產生token如下：
```
kubectl -n kube-system describe $(kubectl -n kube-system \
get secret -n kube-system -o name | grep namespace) | grep token:
```
 - 打開連線
     - `kubectl proxy --port 8081`
     
 - 使用Cloud Shell Web 預覽功能來改變port到8081
 ![](https://i.imgur.com/omwnEtM.png)
 - 我們將收到一串API endpoint，要連結到面板，將`/?authuser=0`移除，然後加上下面的url:
```
/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=default
```
 - 然後你會被帶到一個網頁預覽
 ![](https://i.imgur.com/2Mmxecr.png)
 - 點選Token radio，然後貼上剛剛拿到的token，然後Sign in
 ![](https://i.imgur.com/74uzP4O.png)
 - 你可以從主控台存取面板服務，Navigation menu > Kubernetes Engine，然後選擇Connect按鈕連接到我們想要連接的cluster
 ![](https://i.imgur.com/jmNioqz.png)


![](https://i.imgur.com/luJw5q4.png)

## 習題測驗
- which of the following are features of the Kubernetes Engine?
    - [x] Identity and Access Management
    - [x] Integrated Logging and Monitoring
    - [ ] None of these
    - [x] Stateful Application Support
 
 
 
 
## 非必要指令
 - 如果想要檢視這個deployment，可以使用以下指令：
     - `kubectl get deployments`
 - 若想檢視由deployment建立的pod，可以使用以下指令:
     - `kubectl get pods`
 - 以下也是一些有趣的kubectl指令：
     - kubectl cluster-info
     - kubectl config view
     - kubectl get events
     - kubectl logs podName
