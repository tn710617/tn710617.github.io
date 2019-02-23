---
title: Send email via AWS SES in Laravel <hr> 怎麼在Laravel中，利用AWS SES發郵件?

date: 2019-02-06 15:24:19
tags: 
    -   AWS SES   
    -   Ngrok
    -   Laravel Mail
category:
    -   Laravel
---
##### <span style="color:blue">English</span>
<br>

How to send email via AWS SES in Laravel?
==
<hr>

1. Apply for AWS SES (simple email service) 
2. Create an user, and SES full accessible policy, along with the access key and secret key.
3. Go to AWS console, click email address option on left side, and verify your email.
![](https://i.imgur.com/3OGCOLF.png)

4. Go to AWS support center to submit a service limit increase case, and choose SES Sending Limits, called 'Remove it out of SandBox' Otherwise the capacity of the mail you are allowed to send and its rate will be limited to a large extend, besides that, all of the recipients will have to be verified by SES.

![](https://i.imgur.com/n4EI4kF.png)

5. Create a Laravel Project
6. Install mail package `composer require guzzlehttp/guzzle`
7. Install AWS SDK `composer require aws/aws-sdk-php`
8. Go to `config/mail.php`, and revise the driver to`ses`
9. Go to `config/services.php`, and config it as follows:
```
'ses' => [
    'key' => 'your-ses-key',
    'secret' => 'your-ses-secret',
    'region' => 'ses-region',  // e.g. us-east-1
],
```
10. You should config all above mentioned in your .env file as follows:
```
MAIL_DRIVER=ses
MAIL_FROM_ADDRESS=your-mail-address
MAIL_FROM_NAME=BuyBuyGo
SES_KEY=your-ses-key
SES_SECRET=your-ses-secret
SES_REGION=us-west-2
```
11. Create class，`php artisan make:mail OrderCreated --markdown=emails.orders.created`
12. Go to `OrderCreated.php`, config your build as follows:
```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;

class OrderShipped extends Mailable
{
    use Queueable, SerializesModels;

    protected $order;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($order)
    {
        $this->order = $order;
        //
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->markdown('emails.orders.created')
            ->with([
                'buyer'            => $this->order->user->name,
                'order'            => $this->order->name,
                'item_name'        => $this->order->item_name,
                'item_description' => $this->order->item_description,
                'quantity'         => $this->order->quantity,
                'total_amount'     => $this->order->total_amount,
                'unit_price'       => $this->order->unit_price,
                'expiry_time'      => $this->order->expiry_time,
            ]);
    }
}
```

13. Go to `created.blade.php` to customize the view as follows:
```
@component('mail::message')
# Dear {{ $buyer }}
Thanks for your patronage!

- Order: {{$order}}
- Item: {{$item_name}}
- Item description: {{$item_description}}
- Quantity: {{$quantity}}
- Unit price: {{$unit_price}}
- Amount: {{$total_amount}}

## Kindly make this payment before <span style="color: red">{{$expiry_time}}</span>

<hr>

<br>
## If you have any question, feel free to contact us

@component('mail::button', ['url' => 'https://tn710617.github.io/'])
Contact Us
@endcomponent

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```
14. Now you could use `mail` to send email wherever you want.
```
Mail::to($buyer->email)->send(new OrderCreated($order));
```

15. Now it should work

16. Do you think "that's it?", yeah almost. However, there is one more thing. 
I spent one day figuring out all above mentioned, and then I came across something tricky, and it took me one another day.
As a backend programmer, whenever I need to connect a third party payment service, I use ngrok to help me develop.
It's so weird this time. I made my script to do some things in my controller after I received the response from payment service. However, it did every function I wrote except for the mail function... and here is the error message:
```
    "message": "Expected response code 250 but got code \"530\", with message \"530 5.7.1 Authentication required\r\n\"",
    "exception": "Swift_TransportException",
    "file": "/Users/ray/code/FacebookOptimizedSellingSystem/vendor/swiftmailer/swiftmailer/lib/classes/Swift/Transport/AbstractSmtpTransport.php",
    "line": 457,
    "trace": [
```
1000 lines omitted.

After tormenting debugging, I found out if I used `valet share`, this error would not occur, and if I used `php artisan serve --port=yourPort`, and then `ngrok http yourPort`, and the error came out.

although I solved the error eventually, to be honest I still didn't know why..

If you know why the error occurred, kindly drop me a message or a mail, it will really help!


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

怎麼在Laravel中，利用AWS SES發郵件?
==
<hr>

1. 申請AWS SES(simple Email Service)服務
2. 建立一個使用者，並建立政策(SES full access)，取得Access key 跟Secret key
3. 到AWS SES 主控台，左方Email Addresses，然後進去點選verify a new email address 進行驗證
![](https://i.imgur.com/3OGCOLF.png)

4. Google AWS support center，提交‘移出沙盒’申請，約24小時內會解封。否則寄信數量跟頻率都會被很大程度上限制住，且任何收件人都必須要經過AWS驗證。
![](https://i.imgur.com/n4EI4kF.png)

5. 開立一個Laravel專案
6. 輸入`composer require guzzlehttp/guzzle`，安裝套件
7. 安裝AWS SDK `composer require aws/aws-sdk-php`
8. 到`config/mail.php`中，將driver選項相對的env參數改成`ses`
9. 到`config/services.php`中，進行以下配置
```
'ses' => [
    'key' => 'your-ses-key',
    'secret' => 'your-ses-secret',
    'region' => 'ses-region',  // e.g. us-east-1
],
```
10. 以上參數在env的配置，大概如下：
```
MAIL_DRIVER=ses
MAIL_FROM_ADDRESS=your-mail-address
MAIL_FROM_NAME=BuyBuyGo
SES_KEY=your-ses-key
SES_SECRET=your-ses-secret
SES_REGION=us-west-2
```
11. 建立Maiiables class，`php artisan make:mail OrderCreated --markdown=emails.orders.created`
12. 到`OrderCreated`中，建立build檔案，大略如下:
```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Contracts\Queue\ShouldQueue;

class OrderShipped extends Mailable
{
    use Queueable, SerializesModels;

    protected $order;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($order)
    {
        $this->order = $order;
        //
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->markdown('emails.orders.created')
            ->with([
                'buyer'            => $this->order->user->name,
                'order'            => $this->order->name,
                'item_name'        => $this->order->item_name,
                'item_description' => $this->order->item_description,
                'quantity'         => $this->order->quantity,
                'total_amount'     => $this->order->total_amount,
                'unit_price'       => $this->order->unit_price,
                'expiry_time'      => $this->order->expiry_time,
            ]);
    }
}
```

13. 到`created.blade`當中做版面客制，大略如下：
```
@component('mail::message')
# Dear {{ $buyer }}
Thanks for your patronage!

- Order: {{$order}}
- Item: {{$item_name}}
- Item description: {{$item_description}}
- Quantity: {{$quantity}}
- Unit price: {{$unit_price}}
- Amount: {{$total_amount}}

## Kindly make this payment before <span style="color: red">{{$expiry_time}}</span>

<hr>

<br>
## If you have any question, feel free to contact us

@component('mail::button', ['url' => 'https://tn710617.github.io/'])
Contact Us
@endcomponent

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```
14. 在任何你想要發這封mail的地方，使用`mail`來寄信，大略如下：
```
Mail::to($buyer->email)->send(new OrderCreated($order));
```

15. 至此，應該可以成功寄信了！

16. 你以為結束了嗎？ 呵呵，是快結束了啦！ 不過呢，還有一件事情非常重要！
上面的部分大概花了我一天，然後我遇到一個未解的謎題，又被搞了一天。
身為一個backend programmer，如果遇到需要接金流的話，我都是用ngrok來測試。
這次遇到的問題很奇怪，當我收到金流服務商的回饋時，我必須要去做一些事，自controller收到request之後所做的任何function都沒有問題，資料庫的CRUD也都正常，可偏偏只要執行到這一行寄mail的，就給我報錯！！ 錯誤訊息如下：
```
    "message": "Expected response code 250 but got code \"530\", with message \"530 5.7.1 Authentication required\r\n\"",
    "exception": "Swift_TransportException",
    "file": "/Users/ray/code/FacebookOptimizedSellingSystem/vendor/swiftmailer/swiftmailer/lib/classes/Swift/Transport/AbstractSmtpTransport.php",
    "line": 457,
    "trace": [
```
以下省略一千行...

在經過超級無敵疲勞的Debug之後，終於發現問題...

只要使用`valet share`，就不會有這個問題
只要是用`php artisan serve --port=yourPort`，然後`ngrok http yourPort`，這樣就會遇到我說的這個問題。

雖然最後問題解決了，但說實在的我還是不知道為什麼...

如果有大大知道這是什麼原因，還請麻煩來信幫我解惑一下！感激不盡！
