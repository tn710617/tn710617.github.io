---
title: What if I push a wrong commit? <hr> 推錯了Commit該怎麼辦？
date: 2019-02-12 08:00:28
tags: git revert
category: Git
---

##### <span style="color:blue">English</span>
<br>

What if I push a wrong commit
==
<hr>

<p>
Sometimes after pushing a new commit, we realise that there seems to be something wrong.

Don’t panic. In this case we could use git revert to cancel the commit.

Now let me illustrate it as follows:
</p>

### Build a simulating remote folder locally
- Due to the inaccessibility of internet of some people, we are going to create a simulating remote folder locally
- Go to the folder where normally you put your projects
    - `mkdir git_demonstration git_demonstration_central`
    - `cd git_demonstration_central`
    - `git init --bare`
- `git_demonstration_central` will be the remote repository in this article.

### Build testing environment locally
- Go to testing folder
    - `cd ../git_demonstration`
- Intialize git
    - `git init`
- Create a file called test
    - `touch test`
- Add number 1 into the file test
    - `cat 1 > test`
- Add test file into git tracking list
    - `git add test`
- Make a commit towards current file and content
    - `git commit -m'1'`
- Add number 2 into file test, and make a commit named 2
    - `cat 2 >> test;git commit -am'2'`
- Add number 3 into file test, and make a commit named 3
    - `cat 3 >> test;git commit -am'3'`

### Build remote branch
- Add the simulating remote repository as the remote of our testing folder.
    - `git remote add origin /user/yourUserName/yourDirectory/git_demonstration_central`
- put current master branch to remote, and set the newly created remote branch as the local one's upstream branch
    - `git push -u origin master`
- Check the history of remote repository
    - `cd ../git_demonstration_central;git log`
    ![](https://i.imgur.com/CTs3Q41.png)


### Revert existing commit
- For example, we would like to remove the content recorded in commit 3
    - `git revert f06550f7`
    ![](https://i.imgur.com/RRSzhrW.png)


- Update the change to remote
    - `git push`
- Check if the content of file test has changed
    - `cat test`
- Got value `1 2`,  and the number 3 existed in commit 3 was already removed 
![](https://i.imgur.com/lvgF0Vd.png)

- Check the history of remote repository
    - `cd ../git_demonstration_central;git log`
    ![](https://i.imgur.com/SpH8ur7.png)
    
### Conclusion
<p>
Some git novices might have the same confusion as mine when I learnt this part. Why don’t we just eliminate the commit? instead, we would we add one more?

Here I would like to make a further explanation.
Normally, after pushing our commit to mutual repository, I strongly urge you not to revise the existing history. Because once you revise the existing history and push it to mutual repository, it could cause a huge impact to the history on everyone’s repository. After revising, every collaborator’s history will be different from yours, which would cause a lot of confusion and conflict.

What we want to take out is a code existing in the file, so realistically, we want to cancel the code, not history. In multi-collaboration, you could add new history, and not recommended revising old one. You could add a new commit specifying what you’ve done, but not to revise the history on your side, because only you know what you’ve done, and other people know nothing on your side.

In other words, before you push your part to mutual repository, you could do whatever you want (only to what you haven’t pushed. Don’t revise anything you’ve pushed), however, after pushing, don’t revise the history. If you want to do some revising on the file, just make a new commit explaining what you’ve done and push it, and then you could avoid possible confusion and conflict.

It’s my sharing today. See you guys.
</p>

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

推錯了Commit該怎麼辦？
==
<hr>

<p>
有時我們把功能做好並且推上公共資料夾之後才發現，靠...我commit好像推錯了...

別緊張，這時候我們可以使用git revert 來取消我們的commit。

現在讓我來為各位做個示範：
</p>

### 建立一個本地遠端資料夾
- 因為有些朋友可能沒有網路，所以本篇範例將創立一個在本地的遠端資料夾
- 到你一般存放專案的資料夾底下
    - `mkdir git_demonstration git_demonstration_central`
    - `cd git_demonstration_central`
    - `git init --bare`
- `git_demonstration_central`將會是本篇範例中的遠端資料夾

### 建造本地測試環境
- 進到本範例本地資料夾
    - `cd ../git_demonstration`
- 初始化git
    - `git init`
- 建立名為test的檔案 
    - `touch test`
- 在檔案內增加內容1 
    - `cat 1 > test`
- 將test檔案加到git追蹤清單
    - `git add test`
- 針對目前檔案以及內容做一個commit名為1
    - `git commit -m'1'`
- 在檔案test中增加數字2，並做一個新的commit名為2 
    - `cat 2 >> test;git commit -am'2'`
- 在檔案test中增加數字3，並做一個新的commit名為3 
    - `cat 3 >> test;git commit -am'3'`

### 建立遠端branch
### Build remote branch
- 將我們一開始建立的位於本地的模擬遠端資料夾加到當前測試環境的遠端
    - `git remote add origin /user/yourUserName/yourDirectory/git_demonstration_central`
- 將目前master branch 推到此遠端，並將遠端新增的分支設為本地的上游分支
    - `git push -u origin master`
- 到遠端資料夾看一下，目前狀況
    - `cd ../git_demonstration_central;git log`
    ![](https://i.imgur.com/CTs3Q41.png)


### Revert已存在的commit
- 假設今天我們要將commit 3的內容移除
    - `git revert f06550f7`
    ![](https://i.imgur.com/RRSzhrW.png)


- 更新到遠端    
    - `git push`
- 看一下檔案test的內容是否已變更    
    - `cat test`
- 得值`1 2`，原本數字3已經在revert之後被移除了    
![](https://i.imgur.com/lvgF0Vd.png)

- 確認遠端歷史狀況
    - `cd ../git_demonstration_central;git log`
    ![](https://i.imgur.com/SpH8ur7.png)
    
### 總結
<p>
有些剛接觸git的人可能會跟我當初有同樣的疑問，那為啥不要整個git的log紀錄都抹掉就好，為啥要多一個commit？

這邊跟大家解釋一下，如果今天我們已經把我們完成的進度推到共同資料夾了，我們就不建議去修改歷史了，因為你一但修改了歷史再往上推，整個共同資料夾的歷史就會改變，共同資料夾的紀錄相當於所有協作者的紀錄，所以如果你單方面變動了歷史，很可能會造成所有協作者的歷史都跟你的不一致，甚至在commit的過程中會有衝突，這在多人協作是相當不建議的。

我們要拿掉的，是我們檔案內的一段有commit紀錄的code，所以實際上我們要取消的是code，不是歷史，而在多人協作中，歷史是可以增加，不建議修改的。你可以新增一個commit明確說明我這段commit是新增或者拿掉了什麼東西，但是不建議單方面地把東西拿掉並且去修改你的歷史紀錄，因為你改的東西只有你自己知道，對於其他的協作者來說他們並不知道在你的電腦上發生了什麼事。

簡單來說，在你上傳到共同資料夾之前，你可以對你的歷史做任何事（這邊僅限於還未上傳的部分，已經上傳的不建議去修改），但是一但上傳之後，就不建議去修改歷史。如果你要對檔案內容做任何修改，請新增一個commit說明修改的內容，這樣才不會造成其他協作者的疑惑以及大家的git歷史有衝突。

以上就是今天的分享，我們明天見！
</p>
