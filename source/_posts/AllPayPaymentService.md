---
title: Handle AllPay third party payment service with Laravel <hr> Laravel串接歐付寶第三方金流支付
date: 2019-02-08 09:06:45
tags: ['AllPay / 歐付寶', 'Laravel Transaction' , 'Laravel Log', 'ngrok', 'Laravel Middleware']
category: Payment Service 
---
##### <span style="color:blue">English</span>
<br>

Handle AllPay third party payment service with Laravel
==
<hr>

### Create Laravel Project
    Laravel new AllPay
### Let's initialize git, it's a must be!
    git init
### Download AllPay SDK, in this article, we will use AllPay PHP SDK
    git clone https://github.com/o-pay/Payment_PHP
### Move SDK Laravel's app folder
    cp Payment_PHP/sdk/AllPay.Payment.Integration.php AllPay/app/
### Create a testing controller
    php artisan make:controller PaymentsController
### Move the example in the SDK package to our Controller as follows:
```
/**
*
*/
    
    //SDK address(You could manage it yourself)
    include('AllPay.Payment.Integration.php');
    try {
        
    	$obj = new AllInOne();

        //Service parameters
        $obj->ServiceURL  = "https://payment-stage.opay.tw/Cashier/AioCheckOut/V5";         
        //Service location
        $obj->HashKey     = '5294y06JbISpM5x9' ;                                            
        //Testing Hashkey, in real case, please use the one provided by AllPay
        $obj->HashIV      = 'v77hoKGq4kWxNNIS' ;                                            
        //Testing HashIV, in real case, please use the one provided by AllPay
        $obj->MerchantID  = '2000132';                                                      
        //Testing MerchantID, in real case, please use the one provided by AllPay
        $obj->EncryptType = EncryptType::ENC_SHA256;                                        
        //CheckMacValue encrypted type, please stay 1, using SHA256


        //Basic parameters(It depends on your need)
        $MerchantTradeNo = "Test".time();

        $obj->Send['ReturnURL']         = 'http://localhost/simple_ServerReplyPaymentStatus.php' ;    
        //The URL AllPay will return after the payment is paid
        $obj->Send['MerchantTradeNo']   = $MerchantTradeNo;                                 
        $obj->Send['MerchantTradeDate'] = date('Y/m/d H:i:s');                              
        $obj->Send['TotalAmount']       = 2000;                                             
        $obj->Send['TradeDesc']         = "good to drink";                                  
        $obj->Send['ChoosePayment']     = PaymentMethod::ALL;                               

        //Items information
        array_push($obj->Send['Items'], array('Name' => "歐付寶黑芝麻豆漿", 'Price' => (int)"2000",
                   'Currency' => "元", 'Quantity' => (int) "1", 'URL' => "dedwed"));

        # E-Invoice parameters
        /*
        $obj->Send['InvoiceMark'] = InvoiceState::Yes;
        $obj->SendExtend['RelateNumber'] = $MerchantTradeNo;
        $obj->SendExtend['CustomerEmail'] = 'test@opay.tw';
        $obj->SendExtend['CustomerPhone'] = '0911222333';
        $obj->SendExtend['TaxType'] = TaxType::Dutiable;
        $obj->SendExtend['CustomerAddr'] = '台北市南港區三重路19-2號5樓D棟';
        $obj->SendExtend['InvoiceItems'] = array();
        // Add item into list of e-invoice
        foreach ($obj->Send['Items'] as $info)
        {
            array_push($obj->SendExtend['InvoiceItems'],array('Name' => $info['Name'],'Count' =>
                $info['Quantity'],'Word' => '個','Price' => $info['Price'],'TaxType' => TaxType::Dutiable));
        }
        $obj->SendExtend['InvoiceRemark'] = '測試發票備註';
        $obj->SendExtend['DelayDay'] = '0';
        $obj->SendExtend['InvType'] = InvType::General;
        */


        //Create order
        $obj->CheckOut();
      
    } catch (Exception $e) {
    	echo $e->getMessage();
    } 
```
    
### Let's move some sensitive information into .env file
```
//服務位置
$obj->HashKey = env('HASHKEY');                                            
$obj->HashIV = env('HASHIV');                                            
$obj->MerchantID = env('MERCHANTID');                                                      

$obj->Send['ReturnURL'] = env('ALLPAYRETURNURL');
$obj->Send['ClientBackURL'] = $request->ClintBackURL;

```
- In .env:
```
ALLPAYRETURNURL=https://163be100.ngrok.io/api/paymentsResponse

HASHKEY=5294y06JbISpM5x9
HASHIV=v77hoKGq4kWxNNIS
MERCHANTID=2000132
```

### Use `use` to replace `include`
 - Delete
```
include('AllPay.Payment.Integration.php');
```
 - Add it in AllPay.Payment.Integration.php到composer.json file
```
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        },
        "files": [
            "app/Helpers.php",
            "app/AllPay.Payment.Integration.php"
        ]
```
 - In terminal, under AllPay project
    - `composer dump-autoload`
 - In the PaymentsController, use all those required classes
```
namespace App\Http\Controllers;

use AllInOne;
use EncryptType;
use Exception;
use Illuminate\Http\Request;
use PaymentMethod;
```

### Create payment order (It depends on your need.)
```
$totalAmount = Order::getTotalAmountForPayments($orders);
$ordersName = Order::getOrdersNameForPayments($orders);
$MerchantTradeNo = time() . Helpers::createAUniqueNumber();
$MerchantTradeDate = date('Y/m/d H:i:s');
$TradeDesc = 'BuyBuyGo';
$quantity = 1;

//Because I am going to insert data into two tables at a time, so I use 
//Transaction of Laravel to prevent the possible inconsistency of two tables

//start transaction
DB::beginTransaction();

//All those scripts below should be executed without errors, otherwise the whole action rollback
try
{
    $payment_service_order = new PaymentServiceOrders();

    $payment_service_order->user_id = User::getUserID($request);
    //Payment service ID
    $payment_service_order->payment_service_id = $thirdPartyPaymentService->id;
    $payment_service_order->expiry_time = (new Carbon())->now()->addDay(1)->toDateTimeString();
    $payment_service_order->MerchantID = env('MERCHANTID');
    $payment_service_order->MerchantTradeNo = $MerchantTradeNo;
    $payment_service_order->MerchantTradeDate = $MerchantTradeDate;
    $payment_service_order->TotalAmount = $totalAmount;
    $payment_service_order->TradeDesc = $TradeDesc;
    //Item order number
    $payment_service_order->ItemName = $ordersName;
    $payment_service_order->save();

    foreach ($orders as $order)
    {
        $order_relations = new OrderRelations();
        $order_relations->payment_service_id = $thirdPartyPaymentService->id;
        $order_relations->payment_service_order_id = $payment_service_order->id;
        $order_relations->order_id = $order->id;
        $order_relations->save();
    }
    //Once any errors occur, the whole action stops and rollback, and provide customized error message
} catch (Exception $e)
{
    DB::rollBack();

    return Helpers::result('false', 'Something went wrong with DB', 400);
}
//If no errors occur, the whole action commit
DB::commit();

```
### Create an API for PaymentsController
 - Add new route in api.php under routes folder
```
Route::post('pay', 'PaymentsController@pay');
```

### Create a simplest HTML
 - You could simply revise the content of default welcome.blade page as follows:
```
<!DOCTYPE html>
<html>
<head>
    <title>Facebook Login JavaScript Example</title>
    <meta charset="UTF-8">
</head>
<body>
// 這邊需輸入PaymentsController的API
<form action="/api/pay" method="POST">
    @csrf()
    <input type="checkbox" value="1" name="order_id[]">
    <input type="checkbox" value="2" name="order_id[]">
    <input type="checkbox" value="3" name="order_id[]">
    <input type="hidden" value="https://64b30ea0.ngrok.io/" name="ClintBackURL">
    <button type="submit">Submit</button>
</form>
</body>
</html>
```

### Simple test
 - Now we are on the default page of Laravel, it should change to the simple form we just created
 - Check nothing, go summit
 - Yes, we successfully arrived payment page
![](https://i.imgur.com/v26RmZj.png)
 
### Create a log
 - In order to get what AllPay will send to us after the payment is made, we need to use Log to see what we will receive.
 - Is there some place that all requests and responses will have to go through where we could have full accessibility? It seems to be a perfect one for logging
 - We could make a middleware, and use Log function of Laravel therein to log whatever it goes through
 - Make a middleware, in the terminal under the AllPay project
    - `php artisan make:middleware TestLog`
 - 註冊middleware
    - In`/app/Http/Kernel.php`，register the middleware we just made
    ```
    protected $middleware = [
        \App\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
        \App\Http\Middleware\TrustProxies::class,
        \App\Http\Middleware\TestLog::class,
    ];
    ```
 - Create Log
    - In the TestLog file that we just made, and added:
    ```
        $response = $next($request);
        log::info([
        $request->header(),
        $request->getMethod(),
        $request->getRequestUri(),
        $request->all(),
        $response->getStatusCode(),
        $response->getContent()
        ]);
        return $response;
    ```
    - Actually, what you might log varies upon different cases
 
### Create a public url
 - We need to get the response from third party, so we need a public url to get it
 - We could use ngrok to create the public url
 - Install ngrok, please refer to its [official website](https://ngrok.com/)
 - Change ngrok to be globally executable
    - mv ngrok /usr/local/bin
 - Get public url
    - `ngrok http 8000`
 - In terminal under AllPay project
    - `php artisan serve 8000`
 - Copy the public url produced by ngrok
 
### Create receive function
 - We are going to catch the response from AllPay, firstly we need to create a function, and after catching it, we could do things there.
    - In PaymentsController, we create a function `receive` as follows:
    ```
    public function receive()
    {
        
    }
    ```
    - Make an API for it
 ```
    Route::post('pay', 'PaymentsController@pay');
    Route::post('receive', 'PaymentsController@receive');
 ```

### Set up client return url
 - In .env, if you've been following what I taught, there should be this parameter, just simply paste the public url there
    - `ALLPAYRETURNURL=https://163be100.ngrok.io/api/recevie`
    - Your link will be different from mine, don't paste mine.

### Test payment
 - Let's connect AllPay payment page again
 - Login the testing buyer account provided by AllPay
    - Account：stageuser001
    - Password：test1234
 - Use the testing credit card provided by AllPay
    - Number：4311-9522-2222-2222
    - Expiry Date：12 / 20 or later than current date
    - Security number：222
 - After payment, let's check log to see if there is a response from AllPay, in terminal under AllPay project
    - `cat storage/logs/laravel-2019-02-08.log`
 - On, we've got the response.
```
'MerchantID' => '2000132',
    'MerchantTradeNo' => 'Test1549597724',
    'PayAmt' => '2000',
    'PaymentDate' => '2019/02/08 11:49:03',
    'PaymentType' => 'Credit_CreditCard',
    'PaymentTypeChargeFee' => '20',
    'RedeemAmt' => '0',
    'RtnCode' => '1',
    'RtnMsg' => '交易成功',
    'SimulatePaid' => '0',
    'TradeAmt' => '2000',
    'TradeDate' => '2019/02/08 11:48:44',
    'TradeNo' => '1902081148440800',
    'CheckMacValue' => '5B1EE24B0E9D600C65578DD82D3168E2ED56799453577E17E1EBEFC536BD7EAF',
```
### Validation
 - Think about it, if your API was accidentally leaked, and some developer knew it. He bought some items from your service, and called your API, how could you tell?
 - So, there is a specific mechanism that only applicable between you and third party payment service
 - The validation mechanism is like a formula every column goes in and out will be calculated with which is only applicable between you and third party, if you've been paying attention, you should notice that in the last column of the response we've received from AllPay.
 - If you are interested in the formula in detail, you could check it on AllPay's website.
 - Since in this article, we use AllPay SDK, so we are going to share how to use official SDK to validate the information.
    - Firstly, let's check a class called `CheckMacValue` in `app/AllPay.Payment.Integration.php`
    - Secondly, you could find a function called `generate`, and you could check it, which it the formula of the CheckMacValue
    - So, we could calculate the received information with this formula, and it should exactly the same as what we've got from third party
    - Let's get all those information except for `CheckMacValue` from AllPay, we could use the following code.
    ```
      $parameters = $paymentResponse->except('CheckMacValue');
    ```
    - And then, we assign a variable with the `CheckMacValue` we've got from AllPay
    ```
      $receivedCheckMacValue = $paymentResponse->CheckMacValue;
    ```
      Then, we calculate the $parameters with function generate to produce correct `CheckMacValue`
    ```
      $calculatedCheckMacValue = CheckMacValue::generate($parameters, env('HASHKEY'), env('HASHIV'), EncryptType::ENC_SHA256);
    ```
    - Finally, we compare if two values are identical, if so, it proves the validity of its source. If not, we shouldn't give any credibility to this information
    ```
      if($receivedCheckMacValue == $calculatedCheckMacValue)
          return true;
      return false;
    ```
### What's next?
 - After validating the source, we could do things accordingly
 - For example, if payment is successfully made, what we are going to do, and if not, what then?
 - In this article, we mark the orders as paid and notify the buyers via email after the payment is made.
 ```
        if (PaymentServiceOrders::checkIfCheckMacValueCorrect($request) && PaymentServiceOrders::checkIfPaymentPaid($request->RtnCode))
        {
            $paymentServiceOrder = (new PaymentServiceOrders)->where('MerchantTradeNo', $request->MerchantTradeNo)->first();
            $paymentServiceOrder->update(['status' => 1, 'expiry_time' => null]);

            $orderRelations = $paymentServiceOrder->where('MerchantTradeNo', $request->MerchantTradeNo)->first()->orderRelations;
            Order::updateStatus($orderRelations);

            $payerEmail = $paymentServiceOrder->user->email;

            if ($payerEmail !== null)
            Mail::to($payerEmail)->send(new PaymentReceived($paymentServiceOrder, $orderRelations));

            return '1|OK';
        }
 ```
 - At the end, don't forget to return '1|OK' to let AllPay knows that we've received the message.
    
### Those I didn't mentioned
 - With the testing buyer account provided by AllPay, except for credit card, is also supports multiple payment method
 - With convenient store pay or bank transferring method, you could login with a testing backed account provided by AllPay, which could simulate making the payment.
    - Account：StageTest
    - Password：test1234


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

Laravel串接歐付寶第三方金流支付
==
<hr>

### 建立Laravel專案
    Laravel new AllPay
### 一開始先Git，這幾乎是一定要的啊！
    git init
### 下載歐付寶SDK，本篇使用PHP SDK
    git clone https://github.com/o-pay/Payment_PHP
### 將SDK移到Laravel裡頭的app底下
    cp Payment_PHP/sdk/AllPay.Payment.Integration.php AllPay/app/
### 建立等等測試用的Controller    
    php artisan make:controller PaymentsController
### 從剛剛的SDK包裡面，複製example到我們的controller裡，本篇使用All的example，如下：
```
/**
*
*/
    
    //載入SDK(路徑可依系統規劃自行調整)
    include('AllPay.Payment.Integration.php');
    try {
        
    	$obj = new AllInOne();

        //服務參數
        $obj->ServiceURL  = "https://payment-stage.opay.tw/Cashier/AioCheckOut/V5";         
        //服務位置
        $obj->HashKey     = '5294y06JbISpM5x9' ;                                            
        //測試用Hashkey，請自行帶入AllPay提供的HashKey
        $obj->HashIV      = 'v77hoKGq4kWxNNIS' ;                                            
        //測試用HashIV，請自行帶入AllPay提供的HashIV
        $obj->MerchantID  = '2000132';                                                      
        //測試用MerchantID，請自行帶入AllPay提供的MerchantID
        $obj->EncryptType = EncryptType::ENC_SHA256;                                        
        //CheckMacValue加密類型，請固定填入1，使用SHA256加密


        //基本參數(請依系統規劃自行調整)
        $MerchantTradeNo = "Test".time();

        $obj->Send['ReturnURL']         = 'http://localhost/simple_ServerReplyPaymentStatus.php' ;    
        //付款完成通知回傳的網址
        $obj->Send['ReturnURL']         = 'http://gw.grazia.tw/sdk/op_sdk/op_payment/example/simple_ServerReplyPaymentStatus.php' ;    
        //付款完成通知回傳的網址
        $obj->Send['MerchantTradeNo']   = $MerchantTradeNo;                                 
        //訂單編號
        $obj->Send['MerchantTradeDate'] = date('Y/m/d H:i:s');                              
        //交易時間
        $obj->Send['TotalAmount']       = 2000;                                             
        //交易金額
        $obj->Send['TradeDesc']         = "good to drink";                                  
        //交易描述
        $obj->Send['ChoosePayment']     = PaymentMethod::ALL;                               
        //付款方式:全功能

        //訂單的商品資料
        array_push($obj->Send['Items'], array('Name' => "歐付寶黑芝麻豆漿", 'Price' => (int)"2000",
                   'Currency' => "元", 'Quantity' => (int) "1", 'URL' => "dedwed"));

        # 電子發票參數
        /*
        $obj->Send['InvoiceMark'] = InvoiceState::Yes;
        $obj->SendExtend['RelateNumber'] = $MerchantTradeNo;
        $obj->SendExtend['CustomerEmail'] = 'test@opay.tw';
        $obj->SendExtend['CustomerPhone'] = '0911222333';
        $obj->SendExtend['TaxType'] = TaxType::Dutiable;
        $obj->SendExtend['CustomerAddr'] = '台北市南港區三重路19-2號5樓D棟';
        $obj->SendExtend['InvoiceItems'] = array();
        // 將商品加入電子發票商品列表陣列
        foreach ($obj->Send['Items'] as $info)
        {
            array_push($obj->SendExtend['InvoiceItems'],array('Name' => $info['Name'],'Count' =>
                $info['Quantity'],'Word' => '個','Price' => $info['Price'],'TaxType' => TaxType::Dutiable));
        }
        $obj->SendExtend['InvoiceRemark'] = '測試發票備註';
        $obj->SendExtend['DelayDay'] = '0';
        $obj->SendExtend['InvType'] = InvType::General;
        */


        //產生訂單(auto submit至AllPay)
        $obj->CheckOut();
      
    } catch (Exception $e) {
    	echo $e->getMessage();
    } 
```
    
### 我們將上面一些機敏資訊，移到Laravel的.env檔裡面，如下:
```
//服務位置
$obj->HashKey = env('HASHKEY');                                            
//測試用Hashkey，請自行帶入AllPay提供的HashKey
$obj->HashIV = env('HASHIV');                                            
//測試用HashIV，請自行帶入AllPay提供的HashIV
$obj->MerchantID = env('MERCHANTID');                                                      
//測試用MerchantID，請自行帶入AllPay提供的MerchantID

$obj->Send['ReturnURL'] = env('ALLPAYRETURNURL');
//付款完成通知回傳的網址
$obj->Send['ClientBackURL'] = $request->ClintBackURL;
//付款完成後，於第三方頁面顯示回到我們服務的網址

```
- .env檔內如下：
```
ALLPAYRETURNURL=https://163be100.ngrok.io/api/paymentsResponse

HASHKEY=5294y06JbISpM5x9
HASHIV=v77hoKGq4kWxNNIS
MERCHANTID=2000132
```

### 使用`use`代替`include`
 - 刪除
```
//載入SDK(路徑可依系統規劃自行調整)
include('AllPay.Payment.Integration.php');
```
 - 新增AllPay.Payment.Integration.php到composer.json file
```
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        },
        "files": [
            "app/Helpers.php",
            "app/AllPay.Payment.Integration.php"
        ]
```
 - 在terminal下達`composer dump-autoload`
 - 在controller檔案裡，use新的class
```
namespace App\Http\Controllers;

use AllInOne;
use EncryptType;
use Exception;
use Illuminate\Http\Request;
use PaymentMethod;
```

### 建立金流訂單 (這部分屬個人表格設計，帶入參數每個人都不同)
```
//總金額
$totalAmount = Order::getTotalAmountForPayments($orders);
//商品訂單編號
$ordersName = Order::getOrdersNameForPayments($orders);
//金流訂單編號
$MerchantTradeNo = time() . Helpers::createAUniqueNumber();
//金流訂單建立時間
$MerchantTradeDate = date('Y/m/d H:i:s');
//金流訂單敘述
$TradeDesc = 'BuyBuyGo';
//數量
$quantity = 1;

//因為同時建立兩張表格，這邊使用Laravel的transaction功能來防止資料庫資料不一
//transaction開始
DB::beginTransaction();

//以下動作需全部完成無錯誤，否則終止並回朔
try
{
    $payment_service_order = new PaymentServiceOrders();

    $payment_service_order->user_id = User::getUserID($request);
    //金流服務商ID
    $payment_service_order->payment_service_id = $thirdPartyPaymentService->id;
    $payment_service_order->expiry_time = (new Carbon())->now()->addDay(1)->toDateTimeString();
    $payment_service_order->MerchantID = env('MERCHANTID');
    $payment_service_order->MerchantTradeNo = $MerchantTradeNo;
    $payment_service_order->MerchantTradeDate = $MerchantTradeDate;
    $payment_service_order->TotalAmount = $totalAmount;
    $payment_service_order->TradeDesc = $TradeDesc;
    //商品訂單的編號
    $payment_service_order->ItemName = $ordersName;
    $payment_service_order->save();

    foreach ($orders as $order)
    {
        $order_relations = new OrderRelations();
        $order_relations->payment_service_id = $thirdPartyPaymentService->id;
        $order_relations->payment_service_order_id = $payment_service_order->id;
        $order_relations->order_id = $order->id;
        $order_relations->save();
    }
    //若有錯誤，則停止並回朔，回報自訂錯誤訊息
} catch (Exception $e)
{
    DB::rollBack();

    return Helpers::result('false', 'Something went wrong with DB', 400);
}
//若無錯誤，則寫入資料庫
DB::commit();

```
### 為PaymentsController建立一支API
 - 到routes資料夾底下的api.php
 - 增加route
```
Route::post('pay', 'PaymentsController@pay');
```

### 建立一頁最簡單的的html
 - 直接更改Laravel內建welcome.blade的內容，如下：
```
<!DOCTYPE html>
<html>
<head>
    <title>Facebook Login JavaScript Example</title>
    <meta charset="UTF-8">
</head>
<body>
// 這邊需輸入PaymentsController的API
<form action="/api/pay" method="POST">
    @csrf()
    <input type="checkbox" value="1" name="order_id[]">
    <input type="checkbox" value="2" name="order_id[]">
    <input type="checkbox" value="3" name="order_id[]">
    <input type="hidden" value="https://64b30ea0.ngrok.io/" name="ClintBackURL">
    <button type="submit">Submit</button>
</form>
</body>
</html>
```

### 簡單傳送測試
 - 到Laravel首頁，此時此頁面應已經變更為我們剛剛建立的簡單form頁面
 - 什麼都不要勾選，點選submit
 - 成功到了歐付寶的付款頁面
![](https://i.imgur.com/v26RmZj.png)
 
### 建立Log
 - 為了要知道當我們成功付款之後，歐付寶會回傳什麼給我們，我們需要用Log來看看回傳的東西
 - 有沒有一個地方，是所有的請求跟回饋都一定會進出通過，而且可以讓我們控制的？ 這似乎是個完美記log的地方
 - 我們可以建立一個middleware，然後在middleware裏頭使用Laravel的Log功能，將所有進來的請求跟我們回饋的東西全都記下來
 - 建立middleware
    - 於terminal頁面，`php artisan make:middleware TestLog`
 - 註冊middleware
    - 到`/app/Http/Kernel.php`檔案裡頭，加上我們剛剛建立的middleware
    ```
    protected $middleware = [
        \App\Http\Middleware\CheckForMaintenanceMode::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
        \App\Http\Middleware\TrustProxies::class,
        \App\Http\Middleware\TestLog::class,
    ];
    ```
 - 建立Log
    - 到我們剛剛建立的TestLog檔案裡頭，新增：
    ```
        $response = $next($request);
        log::info([
        $request->header(),
        $request->getMethod(),
        $request->getRequestUri(),
        $request->all(),
        $response->getStatusCode(),
        $response->getContent()
        ]);
        return $response;
    ```
    - 實際上，Log要記些什麼視乎每個人的需求
 
### 建立public url
 - 我們要取得第三方回傳的資訊，所以我們必須要有一個public url來接收第三方的response
 - 我們可以使用ngrok來取得public url
 - 至[ngrok官網](https://ngrok.com/)安裝ngrok
 - 將ngrok變更為可全域執行
    - mv ngrok /usr/local/bin
 - 先取得public url，在terminal中
    - `ngrok http 8000`
 - 在terminal中，位於AllPay專案資料夾內，開啟本機通道
    - `php artisan serve 8000`
 - 複製ngrok產生的public https url
 
### 建立接收的function
 - 我們準備要來接歐付寶回傳的訊息了，我們需要建立一個function，並且接到之後，可以在裡面做我們想做的事
    - 在PaymentsController裡頭，我們先建一個function `receive`，如下：
    ```
    public function receive()
    {
        
    }
    ```
 - 針對此function，建立一個API
 ```
    Route::post('pay', 'PaymentsController@pay');
    Route::post('receive', 'PaymentsController@receive');
 ```

### 設定ReturnURL
 - 於.env檔內，如有照之前步驟，應有以下參數，將複製的public url 貼上 
    - `ALLPAYRETURNURL=https://163be100.ngrok.io/api/recevie`
    - 你的連結跟我的不一樣哦，別貼我的

### 付款測試
 - 再次連到歐付寶付款頁面
 - 登入歐付寶提供的買家測試帳號
    - 帳號：stageuser001
    - 密碼：test1234
 - 使用歐付寶提供的測試信用卡付款
    - 卡號：4311-9522-2222-2222
    - 有效期限：請大於目前月/年，例：12 / 20
    - 末三碼：222
 - 付款後，我們到log去看一下有沒有收到歐付寶的回饋，於terminal，位於AllPay的資料夾內
    - `cat storage/logs/laravel-2019-02-08.log`
 - 咦，有收到歐付寶的回饋了！
```
'MerchantID' => '2000132',
    'MerchantTradeNo' => 'Test1549597724',
    'PayAmt' => '2000',
    'PaymentDate' => '2019/02/08 11:49:03',
    'PaymentType' => 'Credit_CreditCard',
    'PaymentTypeChargeFee' => '20',
    'RedeemAmt' => '0',
    'RtnCode' => '1',
    'RtnMsg' => '交易成功',
    'SimulatePaid' => '0',
    'TradeAmt' => '2000',
    'TradeDate' => '2019/02/08 11:48:44',
    'TradeNo' => '1902081148440800',
    'CheckMacValue' => '5B1EE24B0E9D600C65578DD82D3168E2ED56799453577E17E1EBEFC536BD7EAF',
```
### 驗證
 - 試想，如果有人不小心知道了你的API，然後對方也是開發者，他如果跑到你的服務購買商品，然後呼叫你的API結帳，你如何辨別？
 - 所以說，第三方跟廠商會有一套只有雙方身份對了，驗證才能通過的機制
 - 驗證機制大概就是，所以你傳出去的資料，每一項欄位，會經過一套只有雙方適用的公式下去計算，最後會得出一串CheckMacValue，眼尖的朋友應該已經看到，在我們收到的訊息最下面一個欄位帶的就是這串資料。
 - 詳細的公式各位朋友可參照官方網站。
 - 由於本篇使用官方的SDK，所以本篇將教大家如何使用官方SDK來驗證收到的資訊
    - 首先，我們到官方SDK的檔案中`app/AllPay.Payment.Integration.php`，搜尋名為`CheckMacValue`的class
    - `CheckMacValue` class裡頭，有一個名為`generate`的function，大家可以看一下它是怎麼寫的，這就是產生這串CheckMacValue的公式。
    - 所以說，我們只要通過這套公式，驗證我們收到的訊息之中，除了`CheckMacValue`這個欄位的資訊，那理應得到跟回傳的`CheckMacValue`一模一樣的值
    - 先取得回傳資訊中，除了`CheckMacValue`之外的所有資訊，我們可以使用以下的code
    ```
      $parameters = $paymentResponse->except('CheckMacValue');
    ```
    - 再來，取得歐付寶回傳的`CheckMacValue`
    ```
      $receivedCheckMacValue = $paymentResponse->CheckMacValue;
    ```
    - 接下來，使用官方的generate function，帶入我們收到的資訊，來產出正確的`CheckMacValue`
    ```
      $calculatedCheckMacValue = CheckMacValue::generate($parameters, env('HASHKEY'), env('HASHIV'), EncryptType::ENC_SHA256);
    ```
    - 最後，比較兩者的值是否一樣？如果一樣，表示這則訊息的確來自歐付寶，如果不同，那此資訊沒有可信度
    ```
      if($receivedCheckMacValue == $calculatedCheckMacValue)
          return true;
      return false;
    ```
### 驗證之後呢？ 
 - 確認資訊來源正確之後，我們就可以依據收到的資訊下去做事了！ 
 - 比方說，付款了做些什麼，付款失敗又做些什麼
 - 本篇範例為收到付款之後，將資料庫內訂單標記付款，並發email通知買家，如下：
 ```
        if (PaymentServiceOrders::checkIfCheckMacValueCorrect($request) && PaymentServiceOrders::checkIfPaymentPaid($request->RtnCode))
        {
            $paymentServiceOrder = (new PaymentServiceOrders)->where('MerchantTradeNo', $request->MerchantTradeNo)->first();
            $paymentServiceOrder->update(['status' => 1, 'expiry_time' => null]);

            $orderRelations = $paymentServiceOrder->where('MerchantTradeNo', $request->MerchantTradeNo)->first()->orderRelations;
            Order::updateStatus($orderRelations);

            $payerEmail = $paymentServiceOrder->user->email;

            if ($payerEmail !== null)
            Mail::to($payerEmail)->send(new PaymentReceived($paymentServiceOrder, $orderRelations));

            return '1|OK';
        }
 ```
 - 最後記得別忘記return '1|OK', 通知歐付寶我們已經收到付款囉！
    
### 一些沒說到的事
 - 歐付寶測試帳號中，除了信用卡之外，也支援多種其他方式付款哦！可於登入買家測試帳號之後使用。
 - 超商或轉帳付款方式，需特別登入歐付寶提供的後台測試帳號，方可達到模擬付款！
    - 帳號：StageTest
    - 密碼：test1234
    
