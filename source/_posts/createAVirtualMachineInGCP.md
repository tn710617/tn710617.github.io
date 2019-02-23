---
title: Create a virtual machine <hr> 開立一台虛擬機
date: 2019-02-19 08:28:38
tags: 
    -   GCP
    -   GCP virtual machine
    -   QWIKLABS
category: Deployment
---
建立一台虛擬機
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
原文可參閱
[Refer to official link](https://www.qwiklabs.com/focuses/3563?parent=catalog)
</p>
 
## 本篇將會做什麼？

- 利用GCP主控台建立一個virtual machine
- 利用gcloud command line建立一個virtual machine
- 在virtual machine上部署一個web server
 
## 利用Google提供的臨時學習帳密登入
![](https://i.imgur.com/xjtczqE.png)

## 登入主控台
 - 左方為選單列
![](https://i.imgur.com/pJNHqjV.png)

 - 右上此按鈕可以打開Cloud Shell
![](https://i.imgur.com/aYuYmfB.png)
 
 - 點擊'START CLOUD SHELL'
![](https://i.imgur.com/5ZHGNvN.png)

## 理解Regions 和 Zones
 - 特定的Compute Engine 資源位於特定的regions或zones.
 - Region表示一個特定的地理位置，而你的資源就跑在那邊。
 - 每個region都有一個或多個zones，舉例來說，us-central1 region位於Central United States，並且下面有us-central1-a, us-central1-b, us-central1-c, us-central1-f這些zones
 ![](https://i.imgur.com/rBWlNZy.png)
 - 位於zone的資源算是zonal資源。
 - Virtual machine instance還有persistent disk都位於zone, 如果要在一個virtual machine上加一個persistent disk，那兩者必須位於同一個zone
 - 相同的，如果你要分配一個static IP位址到一個instance，這個instance必須要跟這個static IP同一個region
 
## 從Cloud Console建立一個新的instance
 - Navigation menu > Compute Engine > VM instance
 ![](https://i.imgur.com/xDhMJgB.png)
 - 按create
 ![](https://i.imgur.com/d0Bw6AI.png)

| 欄位 | 值 | 額外資訊 |
|:------:|:-----------:| :----------: |
|  name  | `gcelab` |  |
|  region |`us-central1`  | [更多regions的資訊](https://cloud.google.com/compute/docs/regions-zones/) |
|  zone | `us-central1-c(Iowa)`<br>注意：記住你選擇的zone，待會會用到 | [更多zone的資訊](https://cloud.google.com/compute/docs/regions-zones/) |
|  Machine Type | `1 vCPU` <br> 這是一個(n1-standard-1), 1-CPU, 3.75GB RAM instance <br> 有很多種類型可以選擇，從基礎型的到32-core/208GB RAM的都有，更多資訊可以參考[機型種類文件](https://cloud.google.com/compute/docs/machine-types)| 一個新專案有所謂的[resource quota](https://cloud.google.com/compute/quotas), 他會限制可以開立的機型規格。我們可以要求更高規格的機型在此lab之外 |
| Boot Disk | `New 10 GB standard persistent disk` <br> `OS Image: Debian GNU/Linux 9 (Stretch)` | 有很多種類的images可以選擇，包含Debian, Ubuntu, CoreOS，以及一些高級的iamges，像是RedHat Enterprise, Linux，和Windows Server，更多資訊可以參考作業系統文件 |
| Firewall | 勾選 `Allow HTTP traffic` 勾選這個選項，所以我們等等才能存取安裝好的server | 注意：這會自動建立防火牆規則，容許HTTP 80 port通道 |

 - 點擊Create 
 - 點擊SSH，經由瀏覽器連到virtual machine

![](https://i.imgur.com/4mGYCPt.png)

注意：更多資訊可以參考[文件](https://cloud.google.com/compute/docs/instances/connecting-to-instance)

## 安裝NGINX web server

 - 經由SSH連接virtual machine之後，先取得root權限
     - `sudo su -`
 - 更新OS
     - `apt-get update`
 - 安裝NGINX
     - `apt-get install nginx -y`
 - 確認NGINX正常運行中
     - `ps auwx | grep nginx`
 - 現在我們可以經由點擊Cloud Console上的External IP連結按鈕，或者直接在瀏覽器上輸入`http://EXTERNAL_IP/` IP位址來連結到Server的網頁

## 使用gcloud來建立一個instance
<p>
除了使用GCP主控台之外，我們也可以使用gcloud的command line 工具來建立一個virtual machine instance，這個工具已經事先被安裝在Google Cloud Shell中了。
Cloud Shell 是一台以Debian為基礎的virtual machine，預載有所有你需要的開發工具(gcloud, git, 還有其他的等等), 並且提供5GB persistent disk的home目錄
</P>

<p>
如果你之後想要在自己的機器上嘗試看看，可以參考[gcloud command line tool guide](https://cloud.google.com/sdk/gcloud/)
</p>

 - 在Cloud Shell，利用command line gloud工具建立一台新的virtual machine instance
     - `gcloud compute instances create gcelab2 --zone us-central1-c`
 - 建立的instance將會有以下的預設值
     - 最新的Debian 9 image
     - `n1-standard-1` machine type, 在這個lab中，你可以選擇其他的machine type，像是`n1-highmen-4`或`n1-highcpu-4`，如果你在做這個lab之外的專案，你可以選擇客製化的machinee type
     - 預設的persistent disk名稱將與此instance一樣，並自動加到此instance
 - 使用`gcloud compute instances create --help` 檢視所有預設值
 - 如果你總是使用同一個區域，你可以將指定的地區設為預設，這樣就不需要每次都要使用`--zone`參數
     - `gcloud config set compute/zone`
     - `gcloud config set compute/region`

 - 檢視你的instance, `Navigation menu > Compute Engine > VM instances`
 - 最後，你可以使用gcloud經由SSH連線到你的instance，當你在連接時，確定一下後面的zone是跟你當初建的時候指定的一樣，或者如果你已經使用的上述的指定默認指令，那就不需要在指定一次。
     - `gcloud compute ssh gcelab2 --zone us-central1-c`
 - 選y繼續


## 考考你！

- Through which of the following ways you can create a VM instance in Google Compute Engine(GCE)?
    - [x] Through web console
    - [x] The gcloud command line tool.
