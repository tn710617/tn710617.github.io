---
title: Create a persistent disk in GCP <hr> 在GCP建立一個persistent disk
date: 2019-02-16 09:54:55
tags: 
    -   GCP 
    -   GCP persistent disk
    -   QWIKLABS
category: Deployment
---
建立一個Google的Persistent Disk
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
原文請參閱

[Refer to official link](https://www.qwiklabs.com/focuses/1753?parent=catalog)

</p>

## Persistent分為兩種
 - 一般persistent disk
 - SSD persistent disk
 
## 本篇將會做什麼？
 - 建立一個新的VM instance，然後在其新增persistent disk
 - 掛載並格式化persistent disk
 
## 利用Google提供的臨時學習帳密登入
![](https://i.imgur.com/xjtczqE.png)

## 登入主控台
 - 左方為選單列
![](https://i.imgur.com/pJNHqjV.png)

 - 右上此按鈕可以打開Cloud Shell
![](https://i.imgur.com/aYuYmfB.png)
 
 - 點擊'START CLOUD SHELL'
![](https://i.imgur.com/5ZHGNvN.png)

## 建立VM instance
 - 建立一個名為'gcelab'的新虛擬機instance 
 `gcloud compute instances create gcelab --zone us-central1-c`
     - 新建的VM instance將有內建10GB的初始化disk

## 建立新的persistent disk
 - 在Cloud Shell中輸入以下指令，注意zone參數需與VM instance一致
 `gcloud compute disks create mydisk --size=200GB --zone us-centrall-c`

## 在運轉中的VM instance上新增剛建立的persistent disk
 - `gcloud compute instances attach-disk gcelab --disk mydisk --zone us-central1-c`

## 在VM instance 上找到剛剛新增的persistent disk
 - SSH到virtual machine
 `gcloud compute ssh gcelab --zone us-central1-c`
 
 - 輸入y繼續
 - 如果需要設定密碼，可以輸入密碼
 - 在/dev/disk/by-id/下找到disk裝置
 `ls -l /dev/disk/by-id/`

 - 找到預設裝置名稱如下:
 `scsi-0Google_PersistentDisk_persistent-disk-1.`

 - 如果你想要一個不一樣的裝置名稱，當你在新增disk時，你可以加入裝置名稱參數 
 `gcloud compute instances attach-disk gcelab --disk mydisk --device-name yourDeviceName --zone us-central1-c`
 
## 格式化，並且掛載persistent disk
 
 - 在找到裝置後，我們可以將disk分區，格式化，並且掛載
 - 建立一個掛載點
 `sudo mkdir /mnt/mydisk`
 
 - 使用`mkfs`工具，格式化disk為ext4格式，這個指令將會刪除指定disk下的所有資料
 `sudo mkfs.ext4 -F -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1`
 - 利用`mount`工具，掛載disk
 `sudo mount -o discard,defaults /dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk`
## 設定自動掛載
 - 預設值中，在VM instance重新啟動之後，persistent disk並不會自動掛載，我們需要在/etc/fstab檔案中增加一些輸入
 `sudo vim /etc/fstab`
 - 在開頭是`UUID`那段程式碼之後，加入：
 `/dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk ext4 defaults 1 1`
 
 - 此時，你的/etc/fstab應該看起來要像這樣:
 `UUID=e084c728-36b5-4806-bb9f-1dfb6a34b396 / ext4 defaults 1 1
/dev/disk/by-id/scsi-0Google_PersistentDisk_persistent-disk-1 /mnt/mydisk ext4 defaults 1 1`

 - `:wq`存檔離開
  
## 小習題： 
 - Can you prevent the destruction of an attached persistent disk when the instance is deleted?
     - [x] No, attached persistent disks are always associated with the lifetime of the instance.
     - [x] Yes, deselect the option `Delete boot disk when instance is deleted` when creating an instance
     - [ ] Yes, use the `-keep-disks` option with the `gcloud compute instances delete` command
     
 - For migrating data from a persistent disk to another region, reorder the following steps in which they should be performed:

1. Attach disk
2. Create disk
3. Create snapshot
4. Create instance
5. Unmount file system(s)

    - [ ] (4, 1, 2, 3, 5)
    - [ ] (2, 3, 1, 4, 5)
    - [ ] (1, 3, 2, 4, 5)
    - [x] (5, 3, 2, 4, 1)
 
 
## 非必要指令
 - 顯示活躍中帳戶
 `gcloud auth list`
 
 - 顯示project id
 `gcloud config list project`
