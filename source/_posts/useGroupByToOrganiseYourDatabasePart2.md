---
title: Organise database with MySQL group by 2 <hr> 使用MySQL- group by 來整理資料庫 2
date: 2019-01-27 13:16:01
tags: group by
category: MySQL
---
##### <span style="color:blue">English</span>
<br>

Use MySQL-group by to organise your database part 2
==
<hr>

Hello everyone, it’s Ray!

Today I’m going to share with you the further usage about how to manipulate database with group by, and inserting refined data into a new table.

First, let’s start from the final image got on yesterday. What if we need the total rainfall of months, or years?
￼![](https://i.imgur.com/eFUaYkg.png)



Please take a look on code below:

~~~
<?php
// The year(date) and month(date) after select mean what data we want,
// and the second year and month after braces means the name of the
// column on shown data. Sum means to total all the of values of the
// column within braces followed by, and the second rainfall means 
// the name of the column on shown data. Use "group by" to make shown
// data grouped by month and year, and "order by" makes the data
// arranged in ascending order.


$selectQuery = 'SELECT year(date) year, month(date) month,
 sum(rainfall) rainfall from rainfall_by_date group by month(date),
  year(date) order by year(date) asc, month(date) asc;';

// Make a select request to the database
$selectResult = mysqli_query($dbc, $selectQuery);

// Use while loop to repeat select request until all of the arrays
// in $selectResult object are taken
while ($selectRow = mysqli_fetch_array($selectResult))
{
    // Insert the data taken from table rainfall_by_date
    // into the table rainfall_by_month
    $insertQuery = 'INSERT INTO rainfall_by_month (year, month, rainfall) VALUES("' . $selectRow['year'] . '", "' . $selectRow['month'] . '", "' . $selectRow['rainfall'] . '")';

    // Make an insert request.
    $insertResult = mysqli_query($dbc, $insertQuery);
}
~~~

After executing the script above, you will be able to get the new table with refined data, as table below:
￼![](https://i.imgur.com/bsu3U7Q.png)

It’s my sharing today, see you guys tomorrow!


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

使用MySQL- group by 來整理資料庫 part 2
==
<hr>

哈囉大家好，我是Ray！

今天想要跟大家分享group by 的更進一步的操作，如何使用group by 配合select 相對應的選項，新建一個表格，並在新表格內將資料重新整理爲我們需要的row and column。

首先，延續昨天的進度，如下圖所示，我們將降雨量根據天來做分類，那如果說今天我們需要月的降雨量，或者年雨量總和呢？
￼![](https://i.imgur.com/eFUaYkg.png)


請參考以下的code:

~~~
<?php
// SELECT後面的year(date)以及month(date)表示SELECT這兩項資料，
// 括號後的year以及month表示顯示出來的欄位名稱，sum表示加總括號內欄位資料的總和,
// 括號內的rainfall爲欄位名稱，括號後的表示顯示出來的欄位名稱，一樣使用group by，
// 使資料以月份以及年分來做顯示，order by 表示依照先後順序由先到後作排列。


$selectQuery = 'SELECT year(date) year, month(date) month,
 sum(rainfall) rainfall from rainfall_by_date group by month(date),
  year(date) order by year(date) asc, month(date) asc;';

// 向資料庫作select 請求
$selectResult = mysqli_query($dbc, $selectQuery);

// 使用迴圈來重複請求，直到拿出所有位於$selectResult物件中的所有array
while ($selectRow = mysqli_fetch_array($selectResult))
{
    // 將我們從rainfall_by_date取得的資料insert進新表格rainfall_by_month
    $insertQuery = 'INSERT INTO rainfall_by_month (year, month, rainfall) VALUES("' . $selectRow['year'] . '", "' . $selectRow['month'] . '", "' . $selectRow['rainfall'] . '")';

    // 作insert請求
    $insertResult = mysqli_query($dbc, $insertQuery);
}
~~~

執行以上的script之後，可以得到新的表格，如下：
￼![](https://i.imgur.com/bsu3U7Q.png)



