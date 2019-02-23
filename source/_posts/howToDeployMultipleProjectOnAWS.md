---
title: Deploy multiple projects on AWS <hr> 如何在AWS上部署多個專案？
date: 2019-02-04 08:10:35
tags: 
    -   AWS
    -   Apache
category: Deployment
---
##### <span style="color:blue">English</span>
<br>

How to deploy multiple projects on AWS?
==
<hr>

1. Launch a AWS EC2 instance, the testing instance is `Amazon Linux 2 AMI (HVM), SSD Volume Type - ami-0d7ed3ddb85b521a6`

2. Connect to your EC2 instance, and enter 
`sudo vim /etc/httpd/conf.d/yourProjectName.conf`

3. paste the following code
```
<VirtualHost *:443>
    # port 443 for https
    ServerName letussleep.space
    # Your domin name
    DocumentRoot "/var/www/html/yourLaravelProjectName/public"
    # The absolute address of your project on EC2
    SSLEngine on
    SSLCertificateFile /whateverLocationYouWant/certificate.crt
    SSLCertificateKeyFile /whateverLocationYouWant/private.key
    SSLCertificateChainFile /whateverLocationYouWant/ca_bundle.crt
    # They are for SSL signing purpose, corresponding to those validation files you get from SSL signing service.
</VirtualHost>

<VirtualHost *:80>
    # port 80 is for http
    ServerName letussleep.space
    DocumentRoot "/var/www/html/yourLaravelProjectName/public"
    redirect / Https://letussleep.space
    # When user connect this project via http, redirect to https
</VirtualHost>

<VirtualHost *:80>
ServerName oldletussleep.space
    # So, basically in a conf file, we could complete multiple project deployment once we set the domain name properly.
DocumentRoot "/var/www/html/yourProjectName/public"

</VirtualHost>

```
4. Although we could complete multiple project deployment in a single config file once we set the domain name and project address properly, I personally prefer one config file to one project to prevent any possible confusion in the future.
5. So simply repeat all the steps mentioned above, creating a new config file, and config it properly, and then type `sudo service httpd restart`
6. Connect to your domain, it should work now.


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

如何在AWS上部署多個專案？
==
<hr>


1. 建立一個AWS EC2 instance, 本文章使用的instance型號為 `Amazon Linux 2 AMI (HVM), SSD Volume Type - ami-0d7ed3ddb85b521a6`

2. 連結到你的EC2 instance, 輸入：
`sudo vim /etc/httpd/conf.d/yourProjectName.conf`

3. 貼上下面的code

```
<VirtualHost *:443>
    # port 443，給https用的
    ServerName letussleep.space
    # 你的Domain名稱
    DocumentRoot "/var/www/html/yourLaravelProjectName/public"
    # 你在EC2上的專案絕對路徑
    SSLEngine on
    SSLCertificateFile /whateverLocationYouWant/certificate.crt
    SSLCertificateKeyFile /whateverLocationYouWant/private.key
    SSLCertificateChainFile /whateverLocationYouWant/ca_bundle.crt
    # 簽署SSL簽證，分別對應你從從簽證網站上面取得的簽證檔案
</VirtualHost>

<VirtualHost *:80>
    # port 80 給 http用的
    ServerName letussleep.space
    DocumentRoot "/var/www/html/yourLaravelProjectName/public"
    redirect / Https://letussleep.space
    # 當使用者使用http連接，重新導向到https
</VirtualHost>

<VirtualHost *:80>
ServerName oldletussleep.space
    # 在同一個conf檔案裡頭，其實就可以部署不同的專案，只要把Domain name區分好
DocumentRoot "/var/www/html/yourProjectName/public"

</VirtualHost>

```
4. 雖然在同一個config檔案裡頭，只要設好domain name 以及不同的專案路徑就可以完成多專案部署，但是這樣難免混亂，所以個人偏好一個專案一個conf檔案。

5. 所以只要重複上面的步驟，創一個新的config檔，並且輸入相對應的資訊，最後輸入
`sudo service httpd restart`

6. 連到你的Domain, 應該已經沒問題了！

