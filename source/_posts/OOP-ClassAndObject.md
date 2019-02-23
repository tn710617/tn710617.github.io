---
title: OOP-Class and Object
date: 2019-01-29 08:55:47
tags: 
    -   class
    -   object
category: OOP
---
##### <span style="color:blue">English</span>
<br>

OOP-Class and Object
==
<hr>

Hello everyone, it’s Ray!

Today I’m going to share with you what class is, and what object is, also the relationship between them.

It’s hard to skip object when it comes to class, and vice versa, which is often what keep people from understanding.

Simply speaking, class is the code template that generates objects.

Talking is easy, let’s have some hand-on experiment first!

We could name a class with arbitrary name, which could be a combination of letters and numbers, and it must not begin with a number. Please take a look on the example below:
~~~
class ＭyAccessories
{
    // class body
}
~~~
The stuff above doesn’t look like a useful thing though, it’s a totally legal class.

As above mentioned, class is a template that generates objects. Now let’s create some objects as code below:
~~~
$accessory1 = MyAccessories();
$accessory2 = MyAccessories();
~~~
We use class “MyAccessories” to create two objects, and due to the same class that they come from, they share with the same functionality and type.

And you might ask, are they the same?

The answer is NO!

Although they are the same in functionality or type, they are indeed different objects

I know that you might still be confusing. Okay, let’s print them out. Add the following code:
~~~
var_dump($accessory1);
var_dump($accessory2);
~~~
I assume that you will print the stuff below, and you could see the number that follows pound sign, which represent their uniqueness. “Maybe the script shows the number in orders” You might say.
~~~
object(MyAccessories)#1 (0) {
}
object(MyAccessories)#2 (0) {
}

~~~
Okay, let’s have another experiment to make it crystal clear.

We switch the name of objects within var_dump braces, and if the script just shows the number in order, theorectically the number that follows pound sign will not change, will it?
~~~
var_dump($accessory2);
var_dump($accessory1);
~~~
I suppose that you will print something as follows:
~~~
object(MyAccessories)#2 (0) {
}
object(MyAccessories)#1 (0) {
}
~~~
See! The number after pound sign changed, which means the fact that even though they are generated with the same class, they have their own number that represent their uniqueness. That said each object is unique.

If you still have some confusion, let’s make an analogy as follows:

You could think of class as the a mould that produce a lot of castings, and objects are the castings that produced by the mould. They could be keycaps, or earphones. Despite the fact that they look identical on appearance, they are different. You might spot some serial number on them, and it’s like the number we printed earlier after pound sign.

Do you have more understanding on class and object after reading through this article?


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

OOP-Class and Object
==
<hr>

大家好，我是Ray!

今天想跟大家分享，什麼是Ｃlass，以及什麼是Object，還有他們之間的關係！

講到class就不得不講到object，而要解釋object就離不開class，這也常常是讓許多人感到困惑與不解的地方。

簡單來說，class算是一用來創造object的code模板。

廢話不多說，讓我們來創一個class先：

我們可以自訂我們喜歡的class的名稱，class的名稱可以是數字與字母的組成，但開頭的第一個字不可以是數字，如下面的code：
~~~
class ＭyAccessories
{
    // class body
}
~~~

雖然上面的東西看起來沒什麼用，但是這已經是一個符合標準的class

如上所述，我們說class是產出object的一個模板，現在讓我們來產出幾個object，如以下的code:
~~~
$accessory1 = MyAccessories();
$accessory2 = MyAccessories();
~~~

以上我們使用MyAccessories class 造出了兩個object，由於這兩個object是由同樣的class造出來的，所以他們有著相同的功能與類型。

那你會問，他們一樣嗎？

答案是，不。

或許在功能以及類型上它們是一樣的，但他們的確是不同的object。

我知道你可能還有疑惑，讓我們把他們印出來看看！新增以下的code:
~~~
var_dump($accessory1);
var_dump($accessory2);
~~~

沒有意外的話，你應該會印出下面的東西。#後面的編號代表著他們的獨特性。或許你會說，啊～這會不會是照順序來顯示＃後面數字啊？
~~~
object(MyAccessories)#1 (0) {
}
object(MyAccessories)#2 (0) {
}

~~~

那我們再來做一個實驗

我們將var_dump內的object名稱互換，如果說＃後面的數字只是照順序來顯示的話，照理說印出來的東西應該不會變，是吧？
~~~
var_dump($accessory2);
var_dump($accessory1);
~~~

你應該會印出下面的東西：

~~~
object(MyAccessories)#2 (0) {
}
object(MyAccessories)#1 (0) {
}
~~~

＃後面的數字變了！ 這代表一個事實，那就是每個object，儘管他們是由同一個class所產出的，都會有屬於自己的一組編號，代表他們的獨特性，所以每一個object都會是不同的。

如果你還有些困惑，讓我再來舉個例子：

Class就像是生產鑄件的模具，而object就像是被壓出來的鑄件，可以是一個鍵帽，或是一個同型號的耳機。外觀看來他們都是一模一樣的，但是他們確實是不同的獨立個體。
你或許可以在上面看到生產流水編號，那就相當於上面印出來的#後面的數字。

看完以上的文章，各位是否對class以及object有更深一層的認識了呢？
