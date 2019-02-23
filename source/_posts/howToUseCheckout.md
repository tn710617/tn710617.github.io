---
title: How to use checkout <hr>如何使用 git checkout
date: 2019-02-01 10:08:16
Category: Git
tags: git checkout
---
##### <span style="color:blue">English</span>
<br>

How to use `git checkout`?
==
<hr>

Hello everyone, it’s Ray!
![](https://i.imgur.com/l3sTlbj.jpg)

Remember where we were in my last article? I hope that the image above would do some help.

You are right, last time we introduced how to initialise Git, and create a file called example1.html, and then complete our first commit.

As mentioned previously, Git allows us to go back to whatever commit whenever we want. Today I’m going to share how to freely switch among different commits.

Now let’s add some experimental code `<p>We add a new paragraph on the first example</p>` in the example1.html file as follows:
現在，讓我們在檔案內加入下面highlight的一段

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First example</title>
</head>
<body>
<p>This is the first example</p>
<p>We add a new paragraph on the first example</p>
</body>
</html>
```

And then we go to command line, and type `git status`

You will see what looks like the following image, which indicating that the example1.html file has been modified. 

![](https://i.imgur.com/beVFqMH.jpg)

As mentioned in the last article, before making any commit, we need to use `git add` to specify what we want to commit.

So let’s type `git add example1.html`

and type `git status`

![](https://i.imgur.com/v7OMcQx.png)
As the image above, we’ve specified what we are going to commit

Now type `git commit`

And leave the message “New paragraph added in example1.html file” for this commit.

After that we type `git status` to check it

And then `git log`

We will see the second commit that we just made as follows:

![](https://i.imgur.com/Kg76E6M.jpg)

Okay, now let’s go back to our first commit.

The `git log` shows the history of all of the commits, and therefore we could use the information within to switch among different commits

Type `git checkout b45934852da471efbbbc52b5a119e8723fb01866`

This checksum is my version, and it varies upon different time, author, even the data. In other words, it’s unique, so yours will be different from mine.

![](https://i.imgur.com/onHT04j.jpg)

As image shown below, now we are already at our first commit

Now let’s open editor to check. The experimental code ``<p>We add a new paragraph on the first example</p>`` is gone. Now we go back to our first commit, and it doesn’t matter if we’ve saved it with our editor or IDE.

So now how could we go back to our latest commit?

Type `git checkout master`

![](https://i.imgur.com/Mpp9pns.jpg)
As image shown above, now we  are at our latest commit.

Now go to editor to check. See! The disappeared “new paragraph” appears again!

Isn’t it a magic? 

Hope that it will do some help on the understanding of git of yours. See you guys!


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

如何使用`git checkout`？
==
<hr>

大家好，我是Ray!
![](https://i.imgur.com/l3sTlbj.jpg)

還記得我們上次到了哪裡了嗎？看完上面的圖片有沒有讓你回想些什麼呢？

沒錯，上次的git介紹我們從初始化開始，並且建立一個名為example1.html的檔案，然後完成了我們第一個存擋！

如同之前提到的，我說git讓我們再存擋後，如果我們有需要的話，我們可以隨時地回到任何一個我們用git做的存擋點，今天我將跟大家分享如何回到存擋點，並且在存擋點之間自由的切換。

現在，讓我們在檔案內加入下面highlight的一段

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>First example</title>
</head>
<body>
<p>This is the first example</p>
<p>We add a new paragraph on the first example</p>
</body>
</html>
```

然後我們到command line，輸入`git status`

你應該會看到如下圖，如下圖所示，git 顯示example1.html已經被修改過了。

![](https://i.imgur.com/beVFqMH.jpg)

如上一篇提到的，再做存擋之前，我們必須要先使用`git add` 來指定我們想要存擋的進度，所以

輸入`git add example1.html`

輸入`git status`

![](https://i.imgur.com/v7OMcQx.png)
如上圖，我們已經指定了要存擋的進度

現在輸入`git commit`

並記錄訊息”New paragraph added in example1.html file”

完成後輸入`git status`確認一下狀態

然後`git log`

現在我們可以看到我們的第二個commit如下圖：

![](https://i.imgur.com/Kg76E6M.jpg)

好啦，接下來我們來切換回第一個記錄點

`git log` 的功能是顯示我們所有記錄點的歷史，我們可以經由log裡面提供的資料自由的切換於不同的紀錄點。

輸入 `git checkout b45934852da471efbbbc52b5a119e8723fb01866`

這是我的版本，你們的版本會是一串不同的數字

![](https://i.imgur.com/onHT04j.jpg)

如下圖所示，我們現在已經在一個第一個記錄點。

現在可以打開我editor查看，我們新增加的<p>We add a new paragraph on the first example</p> 已經不見了，此時版本恢復到我們第一個記錄點的狀態，不管我們是否有另外在editor做任何的紀錄。

那要如何回到我們的最新的紀錄點呢？

輸入`git checkout master`

![](https://i.imgur.com/Mpp9pns.jpg)
如上圖，我們現在已經回復到我們最新的紀錄點啦！

現在打開我們的editor做確認，登登！ 原本消失的new paragraph 又出現啦！

是不是很神奇呢？

以上是今天的分享，希望可以讓大家對Git有更深的了解，我們明天見！
