---
title: Set Up Network and HTTP Load Balancers <hr> 設定Network 以及 HTTP 平衡負載器
date: 2019-02-19 15:57:05
tags:
    -   GCP
    -   QWIKLABS
    -   Network Load Balancer
    -   HTTP Load Balancer
category: Deployment
---
設定Network 以及 HTTP 平衡負載器
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
</p>

 ![](https://i.imgur.com/lLAZTuk.png)

## 本篇將會做什麼？
 - 設定一個網路平衡負載器
 - 設定一個HTTP(S)平衡負載器
 - 通過實作，學習兩者之間的不同之處
 
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

## 為你的資源設定預設的region以及zone
 - 在Cloud Shell，設定預設zone
     - `gcloud config set compute/zone us-central1-a`
 - 在Cloud Shell，設定預設region
     - `gcloud config set compute/region us-central1`
     
## 建立多個web server instance     
<p> 
為了模擬使用一個群組的機器來服務，我們可以利用Instance Template以及Managed Instance Groups來創建一個群集的Nginx  web servers用以服務靜態的內容。Instance Template定義了在這個群組中的virtual machine的規格（disk, CPUs, memory, 等等）。 Managed Instance Groups可以初始化多台使用Instance Template定義的virtual machine
</p>

要建立一個Nginx web server 服務群，建立以下事物:
 
 - 一份用來設定所有virtual machine上的Nginx server的script
 - 一份instance template來使用上述的script
 - 一個target pool
 - 一個使用instance template的managed instance group
 
以下開始建立上述事物:

 - 建立startup script
```bash
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```
 - 建立使用startup script的instance template
```bash
gcloud compute instance-templates create nginx-template \
         --metadata-from-file startup-script=startup.sh
```

 - 建立target pool。一個target pool將針對所有的群組內的instance提供一個存取點，且在之後的平衡負載步驟，這是必須的。
```bash
gcloud compute target-pools create nginx-pool
```

 - 建立一個使用instance template的managed instance group
 
```bash
gcloud compute instance-groups managed create nginx-group \
         --base-instance-name nginx \
         --size 2 \
         --template nginx-template \
         --target-pool nginx-pool
```
 
 
 - 這將會建立兩個擁有相同前綴名稱nginx-的vurtual machine，需要幾分鐘。
 - 檢視所有已建立的instances
     - `gcloud compute instances list`
 - 現在設定防火牆，所以我們可以經由port 80，EXTERNAL_IP位址來連接我們的機器    
     - `gcloud compute firewall-rules create www-firewall --allow tcp:80`
 - 現在我們應該要可以經由上述的external IP位址連接到任何一台的instance

## 建立Network Load Balancer
<p>
Network Load Balancing允許我們依據收到的IP協議資料，像是位址，port號，還有協議類型，來平衡負載我們的系統。
我們還可以取得一些HTTP(S) load balancing沒有提供的選項，例如說，基於TCP/UDP的協議-SMTP traffic，且如果你的應用對TCP連結相關的特性感興趣的話，Network Load Balacing也允許你的app去檢查封包，這是HTTP(S) Load Balancing沒有提供的。
</p>
 
 - 針對我們的instance，建立一個L3 network load balancer 
```bash
gcloud compute forwarding-rules create nginx-lb \
         --region us-central1 \
         --ports=80 \
         --target-pool nginx-pool
```
 
 - 列出所有Google Compute Engine轉發的規則
     - `gcloud compute forwarding-rules list`
 - 經由瀏覽器來拜訪load balancer    
     - `http://IP_ADDRESS/ where IP_ADDRESS is the address shown as the result of running the previous command.`
 
## 建立一個HTTP(S) Load Balancer
<p>
HTTP(S) Load Balancing 提供全球性的所有對我們的instance所做的請求。我們可以設定URL規則來將某些URL導向一些instance，而將另一些URL導向另外一些instance。正常下，請求將會被導向離使用者最近的instance，以確保該群組的instance有足後的資源可以提供給使用者。如果被導向的instance沒有足夠的資源，那請求將會被導向離使用者最近的並且有足夠資源的instance
</p>
 
 - 首先，建立一個health check。Health check可以核實instance有針對HTTP或HTTPS通道做回應
```bash
gcloud compute http-health-checks create http-basic-check
```
 - 定義一個HTTP服務，並且給予我們instance使用的port號
```bash
gcloud compute instance-groups managed \
       set-named-ports nginx-group \
       --named-ports http:80
```
 
 - 建立後端服務
```bash
gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global
```
 
 - 將我們的instance群組加到後端服務:
```bash
gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global
```
  
 - 建立一個預設的URL指定，他將會把所有收到的請求導向我們的instance
```bash
gcloud compute url-maps create web-map \
    --default-service nginx-backend
```
 
 - 建立一個target HTTP proxy來將請求導向我們的URL map
```bash
gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-map
```
 
 - 建立一個全球轉發規則來處理導向所有收到的請求。一個轉發規則將流量送到指定的HTTP或HTTPS代理根據請求的IP位址，IP協議，或特定的port號。全球轉發規則不支援多個prot
 
```bash
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
```

 - 全球轉發規則建立之後，需要幾分鐘時間生效
     - `gcloud compute forwarding-rules list` 
 
 - 我們現在應該要可以從瀏覽器經由`http://IP_ADDRESS/`來連接，這可能會需要幾分鐘生效。如果無法連接，多等一些時間，重新整理瀏覽器。
 
 
## 考考你
- Network Load Balancing is regional, non-proxied load balancer
    - [x] true
    - [ ] false
 

