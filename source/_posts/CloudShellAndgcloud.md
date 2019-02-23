---
title: Getting Started with Cloud Shell & gcloud <hr> 讓我們開始Cloud Shell及gcloud吧！
date: 2019-02-19 15:27:06
tags:
    -   GCP
    -   QWIKLABS
    -   Cloud Shell
    -   gcloud
category: Deployment
---

Getting Started with Cloud Shell & gcloud
===

### 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
原文可參閱:

[官方連結](https://www.qwiklabs.com/focuses/563?parent=catalog)
</p>
 
## 本篇將會做什麼？
- 練習使用gcloud指令
- 連結到Google Cloud Platform的儲存裝置
 
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
 
 
## 使用終端機

 - 點擊位於GCP主控台右上角的圖案來開始一個新的Cloud Shell視窗，如下圖：
 ![](https://i.imgur.com/h1Gjh8W.png)

 - 在Cloud Shell成功開啟後，我們可以使用終端機來下達Cloud SDK gcloud，或任何其他vurtual machine instance有提供的指令。
 - 我們也可以在不同的專案，或著Cloud Shell，把檔案儲存在persistent disk的HOME資料夾。
 - HOME資料夾只屬於你個人，任何其他USER將無法存取。
 - gcloud提供使用指南，只要在指令的後面加上`-h`，試試下面的指令:
     - `gcloud -h`
 - 或者，你也可以打長一點
     - `gcloud config --help`
     - `gcloud help config`
     
## 使用你的Home資料夾

<p>
現在，讓我們來試試Home資料夾。就算你結束或者重開你的virtual machine，Cloud Shell Home 資料夾內的內容也會繼續存在，不同的專案或者Cloud Shell都可以存取。
</p>

 - 改變目前的工作資料夾
     `cd $HOME`
 - 使用vim打開.bashrc設定檔
     `vim .bashrc`
     
## 使用gcloud指令

 - 讓我們來檢視一下我們環境內的設定列表
     - `gcloud config list`
 - 檢視其他的property是怎麼被設定的
     - `gcloud config list --all`
     
## 管理Cloud儲存資料

 - 建立一個Cloud Storage bucket, bucket 的名字必須獨一無二，所以請給一個名稱來取代下面的unique-name
     - `gsutil mb gs://unique-name`
     
 - 現在，我們可以建立一些資料，並上傳的我們的bucket
 - 建立一個test檔案
     - `vim test.dat`
 - 加一些資料進去
     - `welcome to gcloud!`
 - 存檔
     - `:wq`
 - 現在，上傳一些檔案到我們建立的bucket，請使用我們之前給的名字來取代下面的unique-name
     - `gsutil cp test.dat gs://unique-name`
 - 如果想看一下我們建立的bucket，以及我們上傳的檔案，可以打開Navigation menu > Storage > Browser，然後點擊bucket，應該可以看到test.dat檔案，如下圖：
![](https://i.imgur.com/cFwjdjs.png)
 

## 考考你！

 - Three basic ways to interact with the GCP services and resources:
     - [x] Command-line interface
     - [x] Client libraries
     - [ ] GLib
     - [ ] GStreamer
     - [x] GCP Console
     
     
 - Which tool in cloud shell helps to manage Cloud Storage resources?
     - [ ] gcloud
     - [x] gsutil
     - [ ] compute
     - [ ] bq
     
