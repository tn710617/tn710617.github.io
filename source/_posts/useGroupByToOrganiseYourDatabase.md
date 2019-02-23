---
title: Organise database with MySQL group by <hr> 使用MySQL- group by 來整理資料庫
date: 2019-01-26 10:38:47
tags: group by
category: MySQL
---
##### <span style="color:blue">English</span>
<br>

Use MySQL-group by to organise your database. 
==
<hr>

Hello everyone. It's Ray! I shared how to import Chinese data into database yesterday, and today I’m going to share how to use MySQL group by to organise your database.
![](https://i.imgur.com/cxVkZMO.png)


Through the image above you could see that the data is divided with different district to each day. Let’s assume that the data of the rightest column is rainfall data, what if we want to access the average rainfall of each day from all districts?
We could use MySQL-group by to achieve it.


Take a look on the snippet of code below:
~~~
select date, avg(rainfall) rainfall from rainfall group by date;
~~~

The first date in the above code means name of the column, avg means to average the amount inside the braces, which means to average the amount of value on the rainfall column. 
The second rainfall is the name of this table, and the final date means that we reorganise the data with date as its unit. It would enable automatic calculation  of averaging on the data on the same date.


The result is as follows:
![](https://i.imgur.com/pDuCvr9.png)
￼
Here you go! I hope my sharing has been helpful.

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

使用MySQL- group by 來整理資料庫
==
<hr>

大家好，我是Ray! 昨天跟大家分享如何正確的導入中文的資訊到資料庫裡，今天呢，我將分享如何使用MySQL的group by 來整理資料庫！
![](https://i.imgur.com/cxVkZMO.png)


￼

在上圖我們可以看到，所有資料都以不同的地區來做劃分。假設右手邊的欄位資料爲降雨量好了，如果我們今天想要取得全地區的平均降雨量，該怎麼作呢？
我們可以使用MySQL的group by 來達到我們的目的。


輸入以下code:
~~~
select date, avg(rainfall) rainfall from rainfall group by date;
~~~

上面的date代表我日期欄位的名稱，avg代表平均值，rainfall代表降雨量欄位的名稱，而在括號後面又出現一次rainfall代表顯示在取得的資料表上的欄位的名稱，最後一個rainfall則是我這個表格的名稱。
由於我select的項目裡並沒有地區，而最後的group by 表示資料將以date下去做重新整理，如果有相同天數的欄位就會自
動重整，並使用我前面下的avg平均化處理。


得出的結果如下圖：
![](https://i.imgur.com/pDuCvr9.png)
￼

我們下次見。


