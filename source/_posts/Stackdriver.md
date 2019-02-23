---
title: Stackdriver-quick start <hr> Stackdriver 快速開始
date: 2019-02-18 09:47:01
tags:
    -   QUIKLABS
    -   GCP 
    -   Stackdriver
category: Deployment
---
Stackdriver: 快速開始
==

## 前言
<p>
本篇主要是利用Google的Qwiklab平台學習的同時，做的一份學習筆記
原文連結如下：

[Refer to QWIKLABS official website](https://www.qwiklabs.com/focuses/858?parent=catalog)
</p>

 ![](https://i.imgur.com/lLAZTuk.png)

## 本篇將會做什麼？
<p>
本篇實作將告訴你如何利用Stackdriver來監看Google Compute Engine virtual machine instance，你也將安裝監看以及紀錄的服務在你的VM上，他們可以從你的instance上收集更多的資訊
</p>
 
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
 

### 建立一個Compute Engline Instance
-  在GCP主控台的控制面板， Navigation menu > Compute Engine > VM instance，然後點擊 Create
￼
![](https://i.imgur.com/mvBTBMR.png)

- 照下面的資訊填入相對應的空格，無提到的空格內請保持預設值
    - Name: `lamp-1-vm`
    - Region: `us-central1 (Iowa)``
    - Zone: `us-central1-a`
    - Machine type: `small (1 shared vCPU)`
    - Firewall: `Select Allow HTTP traffic`
    ![](https://i.imgur.com/qxughsD.png)

- 點擊Create

## 安裝Apache2 Server
- 在主控台，點擊SSH來開啟一個連接此instance的terminal
![](https://i.imgur.com/2dwL5CK.png)

- 在SSH視窗中，執行下面的指令來設定Apache2
    - `sudo apt-get update`
    - `sudo apt-get install apache2 php7.0`
    - 當詢問是否繼續`y`
    - 如果你無法安裝php7.0，裝php5
    - `sudo service apache2 restart`
- 回到主控台，在VM instance的頁面，點選External IP處以連接到Apache2預設頁面
![](https://i.imgur.com/WtTHffk.png)

![](https://i.imgur.com/mMgX8SJ.png)

## 建立Stackdriver帳號
- 要使用Stackdriver，你的專案必須要在一個Stackdriver 帳號內，接下的步驟將建立一個有試用期的Stackdriver帳號

    1. 在GCP主控台，點擊Navigation menu > Monitoring
        - 這將在一個新視窗開啟Stackdriver，並顯示你的Qwiklabs專案。 點擊Create workspace
        ![](https://i.imgur.com/0wwazU5.png)
    2. 在接下來的頁面:
        - 加入GCP專案到monitor，你將看到你的專案顯示已勾選
        ![](https://i.imgur.com/97vi3ch.png)
        - 點擊Continue
        - 監看AWS帳號 - 略過設定
        - 安裝Stackdriver監看代理
            - `curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh` 
            - `sudo bash install-monitoring-agent.sh`
        - 安裝Stachdriver記錄代理
            - `curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh`
            - `sudo bash install-logging-agent.sh`

        - 點擊Continue
    3. 點擊Launch monitoring
    
## 建立運行時間確認
<p>
Uptime check用以確認資源總是可以被存取，在此範例中，我們將建立一個uptime check來確認Google網頁正常運行中
</p>
1. 在Stackdriver console主控台，在控制面板上，點擊Create an Uptime Check按鈕。你也可以從左邊到menu中，找到Uptime Checks > Uptime Checks Overview，然後點擊 Create an Uptime Check

2. 編輯New Uptime Check，加入以下資訊
    - Title: `Lamp Uptime Check`
    - Check type: `HTTP`
    - Resource Type: `Instacne`
    - Applies to: `Single, lamp-1-vm`
    - Path: `leave at default`
    - Check every: `1 min`
![](https://i.imgur.com/uHraYcN.png)
<p>
如果instance沒有自動載入在我們寫則"single"之後，取消這次的uptime check，重新整理Stackdriver頁面，然後重新試一遍
</p>

3. 點擊Test來確認我們的uptime check可以連結到資源

4. 點擊save，當顯示所有的資源都已經可以連接
5. 點擊No thanks來為這個uptime check 建立一個警告政策

<p>
Uptime Check的設置將會需要一些時間生效，我們繼續我們的進度，等等我們再來確認結果。讓我們先來建立一個警告政策。
</p>

## 建立一個警告政策
<p>
利用Stackdriver來建立一個或多個的alerting policies.
從左邊的選單，點擊Alerting > Create a Policy，然後設置Conditions, Notifications, and Documentation
</p>

1. 條件： 點擊Add Condition
 - 依照下面的資訊來設置空格處，如果沒有提到，請保留為默認值
 - Target:

```
     Resource Type: GCE VM Instance (gce_instance)

    Metric: Type "network" then select Network traffic
```
![](https://i.imgur.com/Lkjz74o.png)
 - Configuration
```
Condition: is above

Threshold: 500 bytes

For: 1 minute
```

![](https://i.imgur.com/ps4d7Zr.png)
 - 點擊Save
2. Notifications: 
 - 選擇Email Address，然後輸入你的個人信箱地址
 ![](https://i.imgur.com/wNbfKuk.png)
3. Documentation: 
 - 點擊Add Documentation然後新增一個訊息，這個訊息將會被包含在郵件警告中
4. Name this policy:
 - `Inbound Traffic Alert`
 - 點擊save

<p>
我們已經建立一個警告了！
在等待系統觸發警告的同時，建立一個控制面板和圖表，然後看一下紀錄
</p>

## 建立控制面板和圖表
1. 左邊選單，Dashboards > Create Dashboard
2. 螢幕右上方，點擊Add Chart
3. 在`Find resource type and metric`區域，輸入`CPU`，然後選擇 CPU Load(1m).
![](https://i.imgur.com/gKbyxR7.png)
<p>
GCE VM instance根據資源類型自動被選擇，圖表名稱自動命名，但如果你想要的話，你可以自訂命名
</p>

4. 點擊save

<p>
現在建立第二個圖表
</p>

1. 在新的控制面板右上方的選單，選擇Add Chart

`Find resource type and metric`欄位內輸入`Network`，選擇`Received Packets`，其餘欄位保持預設值，你可以在預覽區域看到圖表資料

2. 點擊save
3. 重新命名新的控制面板，從`Untitled Dashboard`改成`Stackdriver LAMP Qwik Start Dashboard`

## 檢視紀錄
<p>
Stackdriver Monitoring 和 Stackdriver Logging緊密地互相整合著
</p>

<p>
在Stackdriver 左手邊選單，點擊Logging來檢視紀錄
</p>

 - 選擇GCE VM instance > lamp-1-vm在第一個下拉選單
 - 從第二個下拉選單選擇syslog，然後點擊OK
 - 其餘欄位保留預設
 - 選擇`Start streaming logs` 圖案
 ![](https://i.imgur.com/I5Lc6uy.png)

 - 可以看到這個VM instance的logs
 ![](https://i.imgur.com/G1uXXyG.png)

<p>
現在來看看，當我們開始跟結束時，會發生什麼事
</p>

1. 點擊並拖曳Logs Viewer brower視窗，所以Compute Engine console 和 Stackdriver Logging console會並排

![](https://i.imgur.com/H388NeT.png)

2. 在主控台內，VM instance視窗，點擊lamp-1-vm instance
3. 在VM instance details 視窗，於螢幕上方點擊Stop，然後確認停止instance，這會需要幾分鐘，我們來看log messages

4. 我們可以看著Logs View 視窗，然後看VM什麼時候被結束
5. 在VM instance detail 視窗，在螢幕最上方點擊Start，然後確認。這會需要幾分鐘的時間，我們可以檢視log訊息

## 確認uptime check結果以及警告觸發

1. 在Stackdriver左邊區域，點擊`Uptime Checks > Lamp Uptime Check`。這將顯示uptime check的細節，包含等待時間，uptime 百分比，區域結果以及設定檢查。如果你看到的Location result 是 "No checks have run yet"，那請等待幾分鐘，然後重新整理頁面

2. 左方區域點擊Uptime Checks > Uptime Checks Overview，這將提供所有運行中的uptime checks，包含網站在不同區域的狀態

## 確認警報是否有觸發
1. 在StackDriver 主控台，左方頁面點擊Alerting > Incidents，如果你沒有看到開啟的事件，請確定一下你在看的是RESOLVED頁面
2. 依然在Stackdriver 主控台，點擊Alerting > Events，你應該會看到一個事件列表
3. 確認一下我們的email帳號，應該會收到警報

## 習題測試
- Stackdriver supports which of the following cloud platforms
    - [x] Google Cloud Platform
    - [ ] Azure
    - [x] Amazon Web Services
