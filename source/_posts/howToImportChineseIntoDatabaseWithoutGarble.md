---
title: Import Chinese into Database without garbles <hr> 如何正確的導入中文而不會出現亂碼？
date: 2019-01-25 16:30:33
tags:
    -   garbles
    -   MySQL
category: PHP
---

##### <span style="color:blue">English</span>
<br>

How to import Chinese into Database without garbles?
==
<hr>

Some detail when importing Chinese from CSV file into database via PHP script

Hello everyone. It’s Ray!
Today I am going to share more details of importing Chinese characters into database from CSV file via PHP script

Firstly, let’s start from PHP script
~~~
<?php
mysqli_set_charset($dbc,"utf8");
~~~

After the code of connecting to database, bear in mind that the code above should be added in order to specify the default format of data from and to database.

As to CSV file:

Firstly, open it with Excel, open new
![](https://i.imgur.com/uzQZ9lQ.jpg)


Secondly, choose data, and click from text
![](https://i.imgur.com/046KNLb.jpg)


Use delimiter
![](https://i.imgur.com/RKj8vx9.jpg)


Use comma to delimit the data
![](https://i.imgur.com/fZwJ39S.jpg)


Finally, general is fine.
![](https://i.imgur.com/8u5DLDO.jpg)


As to database:

If you use GUI tool such as Sequel Pro, remember to choose UTT-8 when creating a new table
![](https://i.imgur.com/OFBttt3.jpg)



If you use terminal, as image below, remember to specify the utf8 format when creating a new table
![](https://i.imgur.com/Cjqw44Z.jpg)



If garble still occurs, check if the format for each column is utf-8
![](https://i.imgur.com/N42PZIe.jpg)


Basically, if all above mentioned is followed, you should be able to import Chinese and show it in database without any trouble as image below:
![](https://i.imgur.com/TB8l7Yx.jpg)

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

如何正確的導入中文而不會出現亂碼？
==
<hr>

如何正確的導入中文而不會出現亂碼？

大家好，我是Ray!
今天我想跟大家分享CSV檔案匯入MySQL的更多細節部分，像是如何正確的導入中文字而不會出現亂碼。

首先，先講PHP的部分：
~~~
<?php
mysqli_set_charset($dbc,"utf8");
~~~

在連接資料庫之後，請記得一定要加入上面的code，目的是明確來往資料庫的資料編碼格式。


檔案部分：

首先，打開Excel，然後開啓新檔案
![](https://i.imgur.com/uzQZ9lQ.jpg)


接下來，點選Data，並且選取From text
![](https://i.imgur.com/046KNLb.jpg)


這邊請選擇使用分界符號
![](https://i.imgur.com/RKj8vx9.jpg)



這裏選擇使用逗號來做分隔
![](https://i.imgur.com/fZwJ39S.jpg)


最後選擇一般即可
![](https://i.imgur.com/8u5DLDO.jpg)


接下來爲，資料庫部分：

如果你是使用Sequel Pro,那請務必在創建表格時點選UTF-8，如下圖
![](https://i.imgur.com/OFBttt3.jpg)



如果你是使用終端機部分，如下圖，請記得要在創立表格的同時賦予utf8的編碼。
![](https://i.imgur.com/Cjqw44Z.jpg)



如果依然在匯入之後顯示亂碼，請確認column的編碼是否爲utf-8
![](https://i.imgur.com/N42PZIe.jpg)


基本上如果以上的細節都有注意到，應該就可以順利的導入中文，並且成功的在資料庫內顯示中文，如下圖：
![](https://i.imgur.com/TB8l7Yx.jpg)


大家寫code愉快！

