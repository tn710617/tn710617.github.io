---
title: Git-Where to start? <hr> Git-從哪裡開始？
date: 2019-01-25 16:30:33
tags:
    -   git init
category: Git
---
##### <span style="color:blue">English</span>
<br>

Git-where to start?
==
<hr>

Hello everyone! It’s Ray.

As mentioned in my the article I posted last time, Git is kind of an integral tool to coders.
Today I’m going to share the basics of Git.

At the first, let’s create an example folder, it could be my-git-repository.

Le’t go to command line

cd the idea patch you like for this folder

let’s type 
~~~
mkdir my-git-repository
~~~


Now type
~~~
cd code/my-git-repository
~~~
to get into the folder in command line. Code is the name of my own folder, which is different from yours. Please type your own.

Now we are in the folder as follows:
￼![](https://i.imgur.com/P98a8p1.jpg)


Let’s make a file here
~~~
touch example1.html
~~~

And then we add the content below in the file:
~~~
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First example</title>
</head>
<body>
<p>This is the first example</p>
</body>
</html>
~~~


Now type 
~~~
git init
~~~
￼![](https://i.imgur.com/OLQfSow.jpg)


And then let’s type and take a look

~~~
git status
~~~
￼![](https://i.imgur.com/AWKRzKf.jpg)


As image shown above, now we could start to use git’s feature.
Currently the example1 is still untracked. Remember that before we make any commit, sounds like that we have to follow those celebrities before receiving what’s new of them.

Let’s add the file and start to track it
~~~
git add example1.html
~~~

As image below:
￼
![](https://i.imgur.com/snC4CvF.jpg)


Okay. After tracking, now we are going to make our first commit.
Enter 
~~~
git commit
~~~

as image below:
￼![](https://i.imgur.com/2X8BOAH.jpg)


Supposedly, the new window would pop up as image below:
￼![](https://i.imgur.com/IPU8qLH.jpg)


Now we enter the message for this commit for further better recognising.

Finally enter 
~~~
:wq
~~~
it means save and exit this window


Now let’s type 
~~~
git status
~~~
![](https://i.imgur.com/uvvORen.jpg)

, and it should look like image above.

Enter 
~~~
git log
~~~
, and you could see a series of number which represents the identify number of this commit.
￼![](https://i.imgur.com/Lqsr55k.jpg)


Please note  that the number varies out of the content of commit, so each commit is unique. If your commit number is different from mine, rest-assured that that’s pretty normal.

Now we’ve completed our very first commit, and we will be able to go back to this commit whenever we need in the future. 

Okay. Let’s call it a day! I will bring more to you in the following days


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

Git-從哪裡開始？
==
<hr>

嗨大家好，我是Ray!

如上一篇提到，Git對於一個coder來說可以說是不可或缺的，今天我就來分享一下Git的基本操作

首先呢，讓我們先來創一個範例資料夾，名稱就叫做my-git-repository吧！

到Command Line 

cd 你想要這個資料夾在哪的路徑/

輸入
~~~
mkdir my-git-repository
~~~


然後
~~~
cd code/my-git-repository
~~~
進到資料夾內，code是我自己的母資料夾，各位請輸入你們自己的路徑

進到資料夾的位置，如下圖：
￼![](https://i.imgur.com/P98a8p1.jpg)


讓我們在裡面建立一個檔案
~~~
touch example1.html
~~~

然後在該檔案裡面添加以下內容：
~~~
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First example</title>
</head>
<body>
<p>This is the first example</p>
</body>
</html>
~~~


輸入
~~~
git init
~~~
￼![](https://i.imgur.com/OLQfSow.jpg)


然後輸入 

~~~
git status
~~~
￼![](https://i.imgur.com/AWKRzKf.jpg)


如上圖所示，我們已經可以開始使用git的相關功能，目前example1的檔案還處於untracked狀態，在建立任何存擋點之前，我們必須要先將檔案加入追蹤，就好像我們在FB上追蹤那些名人一樣！

輸入 
~~~
git add example1.html
~~~

如下圖：
￼
![](https://i.imgur.com/snC4CvF.jpg)


再來輸入 
~~~
git commit
~~~

看起來如下：
￼![](https://i.imgur.com/2X8BOAH.jpg)


應該會出現以下的畫面
￼![](https://i.imgur.com/IPU8qLH.jpg)


接著我們隨便輸入first example當作這個存擋點的訊息記錄

接著我們按
~~~
:wq
~~~
儲存並離開視窗


輸入
~~~
git status
~~~
![](https://i.imgur.com/uvvORen.jpg)

看起來如上圖：

輸入
~~~
git log
~~~
然後你應該可以看到一串代表著此次commit，獨一無二的號碼。
￼![](https://i.imgur.com/Lqsr55k.jpg)


你應該會看到以下的commit，commit 的號碼每個人都不同，所以如果你的號碼跟我的不同是正常的，不用覺得奇怪！

這樣一來就算是完成了一次的存擋啦！之後我們可以隨時回到這個狀況只要我們想要的話！

今天的分享就先到這啦，之後若有機會會再做進一步的介紹！
