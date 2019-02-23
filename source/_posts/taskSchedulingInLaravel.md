---
title: Task Scheduling of Laravel <hr> 使用Laravel任務排程
date: 2019-02-07 11:43:04
tags:
    -   Laravel Task Scheduling
    -   Linux crontab
category: Laravel
---
##### <span style="color:blue">English</span>
<br>

Task Scheduling of Laravel
==
<hr>


### Open Task Scheduling file
Open `yourProjectName/app/Console/Kernel.php`

### Config your schedule 
Here is a schedule example
```
    protected function schedule(Schedule $schedule)
    {
        $schedule->call(function () {
            Token::where('expiry_time', '<', time())->delete();
            PaymentServiceOrders::deleteExpiredOrders();
            Order::where('expiry_time', '<', Carbon::now())->delete();
        })->daily();
    }
```
In my case, it's to daily delete the expired orders

### Add the Task Scheduling into crontab
1. `sudo vim /etc/crontab`
2. 
``` 
* * * * * apache cd /var/www/html/yourProjectName && php artisan schedule:run >> /dev/null 2>&1
```

- Here are the meaning of * * * * * in orders
    1. Minute(0-59) 
    2. Hour(0-23) 
    3. What date in a month(1-31) 
    4. Month(1-12)
    5. What day in a week(0-6)
- apache 
it represents the user. It's important here, because if you don't set it properly, when error occurs as you execute the schedule, the log owner will be the user you set here, which might have authority problem. If you also log other information in other place, and the whole project might not be able to work properly when the log file reject to be written.
- `cd ray cd /var/www/html/yourProjectName` 
    To where your project is 
- `php artisan schedule:run >> /dev/null 2>&1`
    Run the Task Scheduling command in your Laravel project

3. Here you go!

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

使用Laravel任務排程
==
<hr>

### 打開排程檔案
打開yourProjectName/app/Console/Kernel.php

### 輸入你的排程
排程範例如下：
```
    protected function schedule(Schedule $schedule)
    {
        $schedule->call(function () {
            Token::where('expiry_time', '<', time())->delete();
            PaymentServiceOrders::deleteExpiredOrders();
            Order::where('expiry_time', '<', Carbon::now())->delete();
        })->daily();
    }
```

我設定的任務排程，是每天固定刪除資料庫裡過期的訂單。

### 將Laravel排程加入到Linux的crontab中
1. `sudo vim /etc/crontab`
2. 
``` 
* * * * * apache cd /var/www/html/yourProjectName && php artisan schedule:run >> /dev/null 2>&1
```

- 前面的 * * * * * 依序分別代表
    1. 分(0-59) 
    2. 時(0-23) 
    3. 每月的第幾天(1-31) 
    4. 月份(1-12)
    5. 每週的第幾天(0-6)
- apache 
表示使用者，這關乎權限問題，當執行的schedule中出現錯誤，log會由此使用者而建立，若權限沒有設好，之後的使用者都將無法讀取log，會造成，若我們本身有額外記log的話，會因為此log檔無法被開啟而造成錯誤
- `cd ray cd /var/www/html/yourProjectName` 
到該目錄底下
- `php artisan schedule:run >> /dev/null 2>&1`
執行Laravel的排程指令

3. 以上，這樣應該就可以順利地跑起來了！

