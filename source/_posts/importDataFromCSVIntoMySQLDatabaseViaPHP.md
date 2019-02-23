---
title: Import data into MySQL from CSV via PHP script <hr> 如何將CSV檔，經由PHP導入MYSQL？
date: 2019-01-24 20:43:40
tags: 
    -   CSV
    -   MySQL
category: PHP
---

##### <span style="color:blue">English</span>
<br>

How to import data from CSV into MySQL database through PHP script
==
<hr>

Hello everyone. My name is Ray. I would like to share how to import data from CSV file into MySQL database via PHP script
Firstly, take a look on the screenshot of CSV file below:
![](https://i.imgur.com/NTsjE9d.jpg)

Take a look on the PHP script below. Please put them in the same repository.
```php
<?php

// Connect to the database
$dbc = mysqli_connect('Your location', 'Your MySQL user_name',
 'Your MySQL password', 'Your Database Name');
 
// Set the charset to utf8
mysqli_set_charset($dbc,"utf8");

// Read the data
$handle = fopen("The file name.csv", "r");

// Set $i = 0 for further usage
$i=0;

// Use fgetcsv function along with while loop to get all of the rows in the file
while (($data = fgetcsv($handle, 1000, ',')))
{

     // Since the first line in the file is column name,
     // so we are going to skip the first line.
     // When $i = 0, it should be in the first line,
     // it gets into the if function,
     // and the continue skip all the codes afterwards 
     // and get back to the top of the loop,
     // and in this round the $i = 1. So it will not get into if function,
     // only skip the first line that we don't want.


    if($i == 0)
    {
        $i++;
        continue;
    }

// As shown on the image of csv, there is a string "NaN" on the rainfall column.
// If we want to set the data type of this column as float or decimal,
// we should take care of this string before inserting into the database.
// So we use condition sentence to replace 'NaN' with 0,
// and then it will not cause any problem when inserting data into this
// column with data type as either float or decimal.
    if($data[2] == 'NaN')
    {
        $data[2] = 0;
    }

    // Finally, we insert the data into our database.
    $query = 'INSERT INTO rainfall (district, date, rainfall)
VALUES ("'.$data[0] . '", "' . $data[1] . '", "' . $data[2].'")';

    echo $query;
    $result = mysqli_query($dbc, $query);

    if ($result == false)
    {
        echo 'Error description <br/>' . mysqli_error($dbc);
    }
}
?>
```
Finally, execute the script in terminal, php -f scriptName, and here you go!
￼
![](https://i.imgur.com/jiQ5zuW.jpg)


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

CSV檔案經由PHP導入MySQL資料庫
==
<hr>


大家好，我是Ray！我將跟大家分享如何使用PHP來將CSV檔案的內容導入MySQL資料庫。
首先，一個CSV檔如下：
![](https://i.imgur.com/NTsjE9d.jpg)

 以下爲PHP腳本，請將csv的檔案跟腳本放在同一個資料夾內
```php
<?php

// 連接資料庫
$dbc = mysqli_connect('Your location', 'Your MySQL user_name',
 'Your MySQL password', 'Your Database Name');
// 設定編碼爲utf8
mysqli_set_charset($dbc,"utf8");

// 利用fopen功能讀取檔案
$handle = fopen("The file name.csv", "r");
// 設定變數i，之後會用到
$i=0;

// 使用fgetcsv功能，配合while迴圈，可以拿到檔案內的每一行資料
while (($data = fgetcsv($handle, 1000, ',')))
{

    //如圖片所示，第一行是行的名稱，我們不想要將這行導入資料庫，所以我們設定條件句，
    當變數i爲0正是跑到第一行，進入條件句內，變數i變爲1，並且continue使迴圈將之後的code都跳掉，
    直接回到迴圈的最上面在開始跑，此時變數i已經是1，所以將不會在進到條件句中。如此一來我們就完成我們的目標，
    只跳掉第一行。


    if($i == 0)
    {
        $i++;
        continue;
    }

    // 如csv的圖片所示，降雨量那一行中有出現非數字的NaN字串，
    // 但我們又想要將這一行的屬性設爲float或decimal方便之後若有需要用到計算。
    // 要避免資料匯入出錯，我們必須將非數字的字串轉換爲數字，
    // 因此利用條件句，當$data array裏面的當三項爲NaN時，替換爲0
    if($data[2] == 'NaN')
    {
        $data[2] = 0;
    }

    // 最後，將資料導入資料庫
    $query = 'INSERT INTO rainfall (district, date, rainfall)
VALUES ("'.$data[0] . '", "' . $data[1] . '", "' . $data[2].'")';

    echo $query;
    $result = mysqli_query($dbc, $query);

    if ($result == false)
    {
        echo 'Error description <br/>' . mysqli_error($dbc);
    }
}
?>
```
最後，在終端機中執行該腳本，php -f 腳本名稱，完成！
![](https://i.imgur.com/jiQ5zuW.jpg)

 

