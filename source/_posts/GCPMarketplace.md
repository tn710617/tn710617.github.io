---
title: Provision Services with GCP Marketplace <hr> 利用GCP Marketplace來提供服務
date: 2019-02-19 15:26:31
tags: 
    -   GCP 
    -   QWIKLABS
    -   GCP-Marketplace
category: Deployment
---

利用GCP Marketplace來提供服務
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
</p>
 
## 本篇將會做什麼？

- 使用Marketplace來建立一套網路工具包
- 核對部署
 
## 利用Google提供的臨時學習帳密登入

![](https://i.imgur.com/xjtczqE.png)

## 登入主控台
 - 左方為選單列
![](https://i.imgur.com/pJNHqjV.png)

 - 右上此按鈕可以打開Cloud Shell
![](https://i.imgur.com/aYuYmfB.png)
 
 - 點擊'START CLOUD SHELL'
![](https://i.imgur.com/5ZHGNvN.png)


## 導覽到Marketplace

 - 在Google Cloud Console，找到Marketplace如下：
 ![](https://i.imgur.com/an3nvtI.png)

 - 然後應該可以看到Marketplace首頁
 ![](https://i.imgur.com/sSx7xLh.png)

## 選擇Nginx


 - 在搜尋欄輸入`Nginx`, 然後選擇Nginx Certified by Bitnami的版本
 ![](https://i.imgur.com/NuPyFBS.png)
 
## 建立Nginx 工具組

### VM instance 設定

<p>
一旦專案建立了，我們將會被帶到位於Cloud主控台，新的Nginx部署頁面來設定我們的Nginx instance
</p>
 
 - 為instance取名，例如，nginxstack-1
 - 選擇zone
 
 以下保持預設值
 
 - Machine type: `micro(1-shared vCPU)0.6GB memory`
 - Boot Disk: `10 GB SSD`
 - "Allow HTTP Traffic" 以及 "Allow HTTPS Traffic" 需要被勾選
 - 請接受GCP Marketplace Terms of Service，在頁面的下方
 - 點擊Deploy來建立我們的Nginx 工具組
 ![](https://i.imgur.com/unOmnHm.png)

## 核對部署

- 當Cloud主控台回報，我們的Nginx套組已經部署完畢，我們可以核實一下，是否所有東西都正常運行，我們的畫面看起來應該如下圖：
![](https://i.imgur.com/IfJRwW7.png)

## 核對網頁

- 點擊上圖的藍色按鈕`Visit the site`，我們可以存取部署好的Nginx套組，看起來如下圖：
![](https://i.imgur.com/KeGcBUf.png)

## 核對SSH

- 我們也可以點擊SSH連結來打開一個新的VM instance視窗。我們可以使用Unix指令，像是ps來看看Nginx是否正常的運行在我們的instance
    - `ps aux | grep nginx`

## 考考你！！

- Does Google Cloud Platform Marketplace allow you to deply a software package now, and scale that deployment later when your application require additional capacity without updating the software that you have already deployed
    - [x] true
    - [ ] false
 
