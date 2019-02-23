---
title: How to skip git add? <hr> 如何省略 git add?
date: 2019-02-02 11:18:11
tags: 
    -   git add
    -   git commit
    -   git commit -a
category: Git
---
##### <span style="color:blue">English</span>
<br>

How to skip `git add`
==
<hr>

Do you think it’s extremely troublesome to use `git add` every time before we want to make a commit? Let’s try `git commit -am`

Hello everyone. It’s Ray!

Today I am going to share how to simplify `git add` and `git commit`, let’s welcome `git commit -am`

As shared previously, every time before making a commit, we need to use `git add` to update the progress to be committed, and then leave the specific message for this commit before completing this commit.

Some think it’s a good design, after all, it’s best to err on the side of caution. However, some don’t, thinking that it’s a bit troublesome.

No matter which one you are belong to. today I’m going to share with you how to combine and turn them into one move.

Firstly, let’s add a new code in example1.html file as follows:
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
<p>This is the example commit for git commit -am</p>
</body>
</html>
```
enter `git status`

As image below, the example1.html has been modified, and it requires `git add` to update what will be committed before a commit. 

![](https://i.imgur.com/STwntg9.jpg)

According to articles that I posted previously, we need to `git add`, and then `git commit`, and finally leave the specific message before a completed commit.

Now let’s try something simpler.

Enter `Git commit -am "example for git commit -am"`

Enter `git status` to check the condition.

enter `git log`

As image below, we’ve successfully committed.

![](https://i.imgur.com/y3TzK2F.jpg)
Here I’m going to further explain how `git add` works.

When we create a new file, we need to add this file into the list of tracked files, and to do so, we use `git add`

When one of the tracked file is modified, and we would like to make a commit, we would need to update what will be committed, and we also use `git add` to do so.

That said, `git commit -am` would not work on a newly added file which hasn’t been added into tracked file list.

Caution! `-a` here means automatic, which would automatically add ALL those modified file from tracked file list.

After reading through this article, are you more familiar with Git?

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

`git add` 可以省略嗎？
==
<hr>


大家好，我是Ray!

今天要跟大家分享，`git commit -am`

如之前的文章跟大家分享的，每次在commit 之前，我們需要使用`git add`來明確要commit 的進度，然後commit的同時我們需要留下屬於該commit的訊息。

有些人覺得這樣的設計很好，然而有些則不然，他們覺得這樣有點麻煩。

不管您是屬於哪一派，今天我要跟大家分享，如何將這兩個步驟化為一個動作。

首先，讓我們新增一行code在我們現有的檔案example1.html，如下：
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
<p>This is the example commit for git commit -am</p>
</body>
</html>
```
現在輸入 `git status`

如下圖，example1.html已經被修改了，必且如果要commit，我們需要先`git add`來明確要commit的進度。

![](https://i.imgur.com/STwntg9.jpg)

依照之前的文章分享，我們需要先`git add`，然後`git commit`，並留下屬於此次commit的訊息來完成這次的commit。

現在讓我們來試試看比較簡單一點的方法吧！

輸入`git commit -am "example for git commit -am"`

輸入`git status` 確認狀況

輸入`Git log`

如下圖，我們已經成功的commit了！ 

![](https://i.imgur.com/y3TzK2F.jpg)
這邊要跟大家更進一步解釋一下`git add`的功能。

當我們今天新增一個新的檔案時，我們需要將該檔案加入“追蹤”的檔案清單中，我們使用`git add` 來達到這個功能。

當“已經入追蹤”的檔案有更改，且我們要做commit時，我們需要更新該檔案將被commit記錄下來的進度！簡單來說，就是訂出將被commit的資料範圍，而這時我們也是使用`git add`來更新這個進度。

所以說啦，如果今天我們新增一個檔案，且該檔案從未被加入“追蹤”清單中，那這個時候`git commit -am` 是不會對這個檔案起作用的！

有一點請大家注意，`-a` 在這裡代表automatic，它會自動的更新”所有已經加入追蹤清單且有更改”的檔案！

看完今天的分享，大家是不是對git有更進一步地瞭解了呢？

我們明天見！

