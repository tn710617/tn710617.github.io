---
title: What is in git log? <hr> Git log 裡面的東西是什麼？
date: 2019-02-03 09:02:07
tags: 
    -   git log
    -   git log --oneline
category: Git
---
##### <span style="color:blue">English</span>
<br>

What is in git log?
==
<hr>

Hello everyone, It’s Ray!

Today I am going to share with you some detail in git log

Firstly, take a look on the image below:
![](https://i.imgur.com/sagi6Gk.jpg)

We could see that there is a long-random-looking string on every commit, and what the heck is that?

This is a checksum produced by Git with SHA1 according to the committed content, by the way, what’s SHA1?

SHA is the abbreviation of “security hash algorithm”

There are several algorithm like this, and, moreover, SHA is not reversible, which means that if you got a hashed string like the one above, you wouldn’t be able to decode and reverse it back to the one before hashed. 

If you are interested in that, you could google it. I think google serves as a better teacher than me, lol.

Now I am going to share a very very useful code! `Git log --oneline`

Type `git log --oneline`

You could see the difference between `git log` and `git log --oneline`

![](https://i.imgur.com/YcCOikE.jpg)

As image shown above, you could see that git takes author, date information off, also only keep 7 characters from the original long-random-looking checksum.

So could we use `git checkout` that we previously mentioned to switch between different commits? 

The answer is yes! Type `git checkout cc92d2f` (please note that yours will be different from mine, so just type the one shown on your computer)

![](https://i.imgur.com/KUSUwFA.jpg)

As photo shown above, we’ve successfully switched to the previous commit

Type `git checkout master`

Type `git log --oneline`

![](https://i.imgur.com/FtO4T3k.jpg)

Now we are back.

After reading through article today, do you have better understanding on Git?

See you guys!


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

Git log 裡面的東西是什麼？
==
<hr>

大家好，我是Ray!

今天想跟大家分享，git log 裡面的一些細節。

首先，我們先來看看下面的圖片：
![](https://i.imgur.com/sagi6Gk.jpg)

我們可以看到，每一個commit後面都有一段非常長的隨機字串，那這是什麼呢？

這是一串git 根據commit的內容，由SHA1生成的隨機驗證字串，也許你會問，什麼是SHA1?

SHA1全名為security hash algorithm, 中文意思大概就是“安全加密演算法”。

諸如此類的演算法有好幾種，SHA系列的演算法是不可逆的，簡單來說，如果你拿到一串加密過的字串，就像上面那些驗證字串，你是沒有辦法透過將它逆轉回加密前的樣子。

有興趣的朋友可以google一下，這邊我們就不針對SHA多做討論！

接下來介紹一個非常實用的指令，`git log --oneline`!

輸入`git log --online`

可以對照下圖，這是`git log` 與 `git log --oneline`的差別。

![](https://i.imgur.com/YcCOikE.jpg)

由上圖大家可以看到，`git --oneline` 拿掉了作者，日期相關資訊，並且只保留驗證字串的七碼！

那我們之前提到的`git checkout`也可以使用這七碼來作切換嗎？

輸入`git checkout out cc92d2f` (請輸入你電腦上的驗證字串，你的跟我的不一樣）

![](https://i.imgur.com/KUSUwFA.jpg)

如上圖，我們已經成功的切換到前一個commit

輸入 `git checkout master`

輸入 `git log --oneline`

![](https://i.imgur.com/FtO4T3k.jpg)

這樣就又切回來了！

看完今天的文章，是不是對於git 有更深一層的理解了呢？

我們明天見！
