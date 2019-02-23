---
title: Implement CI with Jenkins on AWS <hr> 利用Jenkins在AWS上達到CI
date: 2019-02-11 09:39:04
tags: 
    -   AWS
    -   Jenkins
    -   CI/CD
category: 
    -   Deployment
---
##### <span style="color:blue">English</span>
<br>

Implement CI with Jenkins on AWS
==
<hr>

### Launch an EC2 instance.
### Connect to EC2 with SSH
### Install and config Jenkins on AWS
- `sudo yum install java-1.8.0`
- `sudo yum update –y`
- `sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo`
- `sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key`
- `sudo yum install jenkins -y`
- `sudo service jenkins start`
- `sudo systemctl enable jenkins.service`
- `sudo vim /etc/sysconfig/jenkins` and revise setting as JENKINS_USER="root"

### Config Jenkins and install required plugin
- type `http://yourPublicDNS:8080` on your browser
- `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
- copy the password and paste it in order to login
- install suggested plugins
- Sign up your account
- Save and go ahead
- Go to 'Jenkins management'
![](https://i.imgur.com/yu8D5DQ.png)

- Install GitHub integration plugin
![](https://i.imgur.com/1PWndFU.png)

- Start a free style project
- Go to configuration
- Enter your project url
![](https://i.imgur.com/QPkhPVL.png)

- Check 'git', and enter your Git Repository url
![](https://i.imgur.com/R7mzIHH.png)

- check 'GitHub hook trigger for GITScm polling'
![](https://i.imgur.com/4jKFUWo.png)

- Enter the shell script you like
![](https://i.imgur.com/3ZkH6ij.png)

### Config GitHub
- Go to GitHub->setting
- build the webhook as follows:

![](https://i.imgur.com/CCLKHnK.png)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<hr>

##### <span style="color:red">繁體中文</span>
<br>

利用Jenkins在AWS上達到CI
==
<hr>

### 開立一個EC2 instance 
### 利用SSH連結到EC2
### 安裝以及設定AWS上的Jenkins
- `sudo yum install java-1.8.0`
- `sudo yum update –y`
- `sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat/jenkins.repo`
- `sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key`
- `sudo yum install jenkins -y`
- `sudo service jenkins start`
- `sudo systemctl enable jenkins.service`
- `sudo vim /etc/sysconfig/jenkins` 並更改如右邊的參數 `JENKINS_USER="root"`

### 設定Jenkins並安裝必要的插件
- 於瀏覽器輸入 `http://yourPublicDNS:8080`
- `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
- 複製密碼以登入
- 安裝建議的插件
- 創立帳號
- 存檔並登入
- 到Jenkins管理頁面
![](https://i.imgur.com/yu8D5DQ.png)

- 安裝GitHub插件
![](https://i.imgur.com/1PWndFU.png)

- 開始一個自由專案
- 到設定的地方
- 輸入專案url
![](https://i.imgur.com/QPkhPVL.png)

- 選取 'git', 並填入git資料夾的url
![](https://i.imgur.com/R7mzIHH.png)

- 勾選'GitHub hook trigger for GITScm polling'
![](https://i.imgur.com/4jKFUWo.png)

- 輸入客製化的shell script
![](https://i.imgur.com/3ZkH6ij.png)

### 設定GitHub
- 到GitHub的設定頁面
- 建立一個webhook，如下:

![](https://i.imgur.com/CCLKHnK.png)

