---
title: How to configure Git?<hr>如何設置Git的個人資訊？
date: 2019-02-05 09:40:59
tags: 
    -   git config
    -   git config --global user.name
    -   git config --global user.email
category: Git
---
##### <span style="color:blue">English</span>
<br>

How to configure Git?
==
<hr>

Hello It’s Ray!

Today I’m going to share with you how to configure basic information of Git.

Ｏkay now let’s add a new code on our example file as follows:
```html
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
<p>This is the example1 for git configuration</p>
</body>
</html>
```


`Git commit -am “Before configuration”`

`Git log`

![](https://i.imgur.com/fMeBJ2M.jpg)

Take a look on the image above, which shows the author information of this commit in current configuration.

Now we add another description in the example1.html file as follows:

```html
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
<p>This is the example1 for git configuration</p>
<p>This is the example after git configuration</p>
</body>
</html>
```

Let’s configure the user information.

enter the following code.
 
`Git config --global user.name Ray`
`Git config --global user.email example@email.com`

Please fill in yours in “Ray” and “example@email.com”

And then enter:

`Git commit -am “after configuration”`

`Git log`

![](https://i.imgur.com/MKOWFsG.png)

Through image above we could see that we’ve successfully configured the user information. 

Let me explain further. We use --global to configure, so it applies to ALL, simply speaking, all of the repositories in your computer are applied with this setting. We could also configure a local setting, which means the configuration only apply to local repository. We will talk about it further!

Let’s call it a day. See you guys! 

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

如何設置Git的個人資訊？
==
<hr>

大家好，我是Ray!

今天要來跟大家分享，如何配置Git的基本資訊。

讓我們新增一行敘述在現有的example1.html 的檔案裡，如下：
```html
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
<p>This is the example1 for git configuration</p>
</body>
</html>
```

`Git commit -am “Before configuration”`

`Git log`

![](https://i.imgur.com/fMeBJ2M.jpg)

各位可以看一下上面我們我們剛剛所commit的Author 資料。

現在加入另一段敘述在example1.html檔案內，如下：

```html
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
<p>This is the example1 for git configuration</p>
<p>This is the example after git configuration</p>
</body>
</html>
```

現在讓我們來定義git 配置的使用者資訊：

輸入
 
`Git config --global user.name Ray`
`Git config --global user.email example@email.com`

上面的Ray以及example@email欄位請填你們自己的！

接下來輸入

`Git commit -am “after configuration”`

`Git log`

![](https://i.imgur.com/MKOWFsG.png)

由上面的截圖可以看到，我們已經成功的配置的使用者的名稱還有信箱改掉了！

這邊補充說明，這裡使用--global進行配置，所以這裡的設定是全域通用的，簡單來說，你電腦內的所有資料夾都套用這個資料，之後有機會我們再介紹如何針對單一資料夾進行更改。

今天的分享就到這裡了，我們明天見！
