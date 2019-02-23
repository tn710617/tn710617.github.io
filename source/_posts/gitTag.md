---
title: Let’s specify a reversion number <hr> 標注一個版本號碼
date: 2019-02-10 11:55:03
tags: 
    -   git tag -a 
    -   git checkout
    -   git log --oneline
category:
    -   Git
---
##### <span style="color:blue">English</span>
<br>

Let’s specify a reversion number
==
<hr>

Hello everyone, It’s Ray!

Today I am going to share with you how to use `git tag` to specify a reversion number

After we complete a series of small functions, it could mean that we are going to release a new reversion. For example, think about the online games that you’ve played.

Every time when it released a new reversion, it came with some new function. This reversion always denotes that all those functions have been completed, also works functionally after internal testing.

This reversion number is quite convenient and important to developers. For example, a series of small functions build a big function, and the completion of this big function means that a new reversion is going to be released.

Every time when we complete a small function, we commit it, and when we finish a series of small functions and make them a big one, we use git tag to specify, denoting a reversion’s release. It’s very essential and important to developers when wanting to do some testing some time after the release.

Let’s begin with `git log --oneline`
輸入 `git log --oneline`

![](https://i.imgur.com/Cy9Sf0N.png)

Above image shows what log looks like before adding a tag.

Type 
`git tag -a v1.0 -m “The stable version of example”`

![](https://i.imgur.com/kgS4M83.jpg)

As image above shown, you could see the reversion number we just added.

enter

`git tag`

![](https://i.imgur.com/UWZeXPO.jpg)

You could see the reversions that we’ve made so far.

Now let’s create a new file, example2.html, and add some code on it.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p>This is the experimental file created after reversion v1.0</p>
</body>
</html>
```

Then type
```git
Git add example2.html
git commit -am “An example2 file after v1.0”
Git log --oneline
```

![](https://i.imgur.com/3wrPc0h.jpg)

As image above shown, we are now at sixth commit, and our reversion only covers to fifth commit.

When we want to go back to v1.0 reversion to check, we don’t need to checkout the name of fifth commit, instead, we type the reversion name as follows:

```git
Git checkout v1.0
Git log --oneline
```

![](https://i.imgur.com/4m99fLm.jpg)

As image above, we’ve gone back to v1.0.

Hope that today’s article will be helpful and useful to you. See you tomorrow.


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

標注一個版本號碼
==
<hr>

哈囉大家好，我是Ray！

今天我將跟大家分享`git tag`，如何建立一個版本號。

當我們接二連三地完成了專案裡的一些功能，一系列的功能可能代表著一個版本的產生，比方來說，大家都玩過線上遊戲吧？

每次改版時，常常都是釋出一些新的功能，這個版本號常常意味著，除了這些功能開發完成之外，並且都正常的運作著。

這個版本號對於開發者來說非常方便與重要，這樣來解釋，一系列的小功能構成一個大功能，而這個大功能的完成也代表著新版本的推出。

當每一次我們完成了一個小功能，我們使用`git commit`把它記錄下來，而當我們陸陸續續完成一系列的小功能而構成一個大功能時，我們使用git tag來標注，代表著一個版本的釋出。若日後我們需要回到這一版來做相關的一些測試的話，非常的方便！


輸入 `git log --oneline`

![](https://i.imgur.com/Cy9Sf0N.png)

以上是我們還沒有tag之前的樣子。

輸入
`git tag -a v1.0 -m “The stable version of example”`

![](https://i.imgur.com/kgS4M83.jpg)

如上圖你可以看到我們剛剛加入的版本號 v1.0

輸入 

`git tag`

![](https://i.imgur.com/UWZeXPO.jpg)

可以看到我們至今tag的任何版本號

現在讓我們新增一個檔案，example2.html，並且新增以下的code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p>This is the experimental file created after reversion v1.0</p>
</body>
</html>
```

然後輸入
```git
Git add example2.html
git commit -am “An example2 file after v1.0”
Git log --oneline
```

![](https://i.imgur.com/3wrPc0h.jpg)

如上圖，我們目前在第六個commit，而我們的版本是在第五個commit

當我們想要回到v1.0的版本時，我們不需要checkout第五個commit的名稱，我們只需要輸入版本編號即可，如下輸入：

```git
Git checkout v1.0
Git log --oneline
```

![](https://i.imgur.com/4m99fLm.jpg)

如上圖，我們已經回到版本v1.0的紀錄了

希望我今天的文章對你有所幫助！
