---
title: Get information via Facebook graph API <hr> 串接Facebook graph API  
date: 2019-02-09 10:18:38
tags: 
    -   Facebook Graph API
category:
    -   Laravel
---
##### <span style="color:blue">English</span>
<br>

Get information via Facebook graph API
==
<hr>

### Introduction

<p> 
In this article, I'm going to share how to get user information from Facebook by submitting a token with PHP SDK, which is got by providing user a login page with JaveScript code.
</p>

### Go to FB develop page, and sign up, and then go to console and add a new application
![](https://i.imgur.com/uBliBlD.png)

### In the user information, copy `App key` and `App secret`

### Create a Laravel project
    laravel new Facebook
    
### Initiate Git
    git init
    
### Install Facebook PHP SDK
 - Under the project
 `composer require facebook/graph-sdk`

### Create a controller which we could interact with FB later with
    php artisan make:controller FBController
    
### Create a function called `getFacebookResources`
 - Copy the SDK example code in its page, and paste it in the function
     ```
     require_once __DIR__ . '/vendor/autoload.php'; // change path as needed
     
     $fb = new \Facebook\Facebook([
       'app_id' => '{app-id}',
       'app_secret' => '{app-secret}',
       'default_graph_version' => 'v2.10',
       //'default_access_token' => '{access-token}', // optional
     ]);
     
     // Use one of the helper classes to get a Facebook\Authentication\AccessToken entity.
     //   $helper = $fb->getRedirectLoginHelper();
     //   $helper = $fb->getJavaScriptHelper();
     //   $helper = $fb->getCanvasHelper();
     //   $helper = $fb->getPageTabHelper();
     
     try {
       // Get the \Facebook\GraphNodes\GraphUser object for the current user.
       // If you provided a 'default_access_token', the '{access-token}' is optional.
       $response = $fb->get('/me', '{access-token}');
     } catch(\Facebook\Exceptions\FacebookResponseException $e) {
       // When Graph returns an error
       echo 'Graph returned an error: ' . $e->getMessage();
       exit;
     } catch(\Facebook\Exceptions\FacebookSDKException $e) {
       // When validation fails or other local issues
       echo 'Facebook SDK returned an error: ' . $e->getMessage();
       exit;
     }
     
     $me = $response->getGraphUser();
     echo 'Logged in as ' . $me->getName();
     ```
     
### Enter `App key` and `App secret`
 - In the example above, we need to fill in some area with some information got from FB developer account as follows:
  ```
     $fb = new \Facebook\Facebook([
       'app_id' => 'App Key',
       'app_secret' => 'App Secret',
       'default_graph_version' => 'Current version',
       //'default_access_token' => '{access-token}', // optional
     ]);
  ```
### Create a user login button
 - User need to login in and get the token, and then we could use the token for further processing.
 - In routes/web.php file, create a route for the login page
    ```
    Route::get('/FBToken', function(){
    return view('FBToken');
    });
    ```
 - Under resources/views folder, create a php file called `FBtoken.blade`, and paste the example code of JS as follows:
 ```
 <!DOCTYPE html>
 <html>
 <head>
 <title>Facebook Login JavaScript Example</title>
 <meta charset="UTF-8">
 </head>
 <body>
 <script>
   // This is called with the results from from FB.getLoginStatus().
   function statusChangeCallback(response) {
     console.log('statusChangeCallback');
     console.log(response);
     // The response object is returned with a status field that lets the
     // app know the current login status of the person.
     // Full docs on the response object can be found in the documentation
     // for FB.getLoginStatus().
     if (response.status === 'connected') {
       // Logged into your app and Facebook.
       testAPI();
     } else {
       // The person is not logged into your app or we are unable to tell.
       document.getElementById('status').innerHTML = 'Please log ' +
         'into this app.';
     }
   }
 
   // This function is called when someone finishes with the Login
   // Button.  See the onlogin handler attached to it in the sample
   // code below.
   function checkLoginState() {
     FB.getLoginStatus(function(response) {
       statusChangeCallback(response);
     });
   }
 
   window.fbAsyncInit = function() {
     FB.init({
       appId      : '{your-app-id}',
       cookie     : true,  // enable cookies to allow the server to access 
                           // the session
       xfbml      : true,  // parse social plugins on this page
       version    : '{api-version}' // The Graph API version to use for the call
     });
 
     // Now that we've initialized the JavaScript SDK, we call 
     // FB.getLoginStatus().  This function gets the state of the
     // person visiting this page and can return one of three states to
     // the callback you provide.  They can be:
     //
     // 1. Logged into your app ('connected')
     // 2. Logged into Facebook, but not your app ('not_authorized')
     // 3. Not logged into Facebook and can't tell if they are logged into
     //    your app or not.
     //
     // These three cases are handled in the callback function.
 
     FB.getLoginStatus(function(response) {
       statusChangeCallback(response);
     });
 
   };
 
   // Load the SDK asynchronously
   (function(d, s, id) {
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) return;
     js = d.createElement(s); js.id = id;
     js.src = "https://connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
 
   // Here we run a very simple test of the Graph API after login is
   // successful.  See statusChangeCallback() for when this call is made.
   function testAPI() {
     console.log('Welcome!  Fetching your information.... ');
     FB.api('/me', function(response) {
       console.log('Successful login for: ' + response.name);
       document.getElementById('status').innerHTML =
         'Thanks for logging in, ' + response.name + '!';
     });
   }
 </script>
 
 <!--
   Below we include the Login Button social plugin. This button uses
   the JavaScript SDK to present a graphical Login button that triggers
   the FB.login() function when clicked.
 -->
 
 <fb:login-button scope="public_profile,email" onlogin="checkLoginState();">
 </fb:login-button>
 
 <div id="status">
 </div>
 
 </body>
 </html>
 ```
 - 同樣，在上面的程式碼中需填入編號及版本號，如下
 - Same here, we need to fill in `App key` and `current version` there as follows:
  ```
     FB.init({
       appId      : 'App key',
       cookie     : true,  // enable cookies to allow the server to access 
                           // the session
       xfbml      : true,  // parse social plugins on this page
       version    : 'version' // The Graph API version to use for the call
     });
  ```

### Familiar yourself with FB Graph API tool
 - We could we the API we need with FB Graph API tool
 ![](https://i.imgur.com/Dt9a26B.png)
 
### Customize endpoint
 - Because we might specify our endpoint simply by copying and pasting as follows:
![](https://i.imgur.com/kh7oNEt.png) 

 - So we could move endpoint to `.env` as follows:
  ```
        $endpoint = env('FBEndpoint');
        try
        {
            // Get the \Facebook\GraphNodes\GraphUser object for the current user.
            // If you provided a 'default_access_token', the '{access-token}' is optional.
            $response = $fb->get($endpoint, $token);
  ```
 - And then in the `.env`
  ```
FBEndpoint=me?fields=id,name,email
  ```
 - So, in the future, we could simply copy the endpoint from FB, and paste it on `.env`

### 修改錯誤回傳值
### Revise the error return
 - In the default setting of PHP SDK, it would return error message according to the situation, which might be good. However, I only need to know true or false, and that's it. A ineffective token might be caused by several factors as follows:
    - It doesn't exist
    - Wrong token
    - It expired
 - It doesn't matter which one it is, the only thing we have to do is inform front end of this matter, and ask them to provide make a request to FB again and provide the valid token to us. Therefore, we need to determine what we are going to by a simple true or false feedback from FB PHP SDK, so we revise it as follows:
  ```
  catch (\Facebook\Exceptions\FacebookResponseException $e)
        {
            return false;
//            echo 'Graph returned an error: ' . $e->getMessage();
//            exit;
        }
  ``` 

### Get a public URL
 - Use ngrok to get a public URL for the HTML page that we will use to get token from users.
 - Login the application => Find login product => shortcut => website => paste the public URL
![](https://i.imgur.com/Wogb6vc.png)

### Login and get token

### Feed the token to getFacebookResources function in FBController.

### Insert the data into our database

### We are done here


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

串接Facebook graph API
==
<hr>

### 前言

<p> 
本篇將分享如何使用JavaScript SDK讓用戶登入並取得token，然後利用PHP SDK向Facebook發請求，進而取得使用者的資訊。
</p>

### 到FB的開發者頁面，申請一個帳號，並且在主控台的地方，新增一個應用程式
![](https://i.imgur.com/uBliBlD.png)

### 到應用程式內的基本資料裡頭，複製`應用程式編號`以及`應用程式密鑰`

### 創建Laravel專案
    laravel new Facebook
    
### 初始化Git    
    git init
    
### 安裝Facebook PHP SDK
 - 於專案目錄下
 `composer require facebook/graph-sdk`

### 建立一個稍後用來向FB拿資料的Controller
    php artisan make:controller FBController
    
### 建立一個getFacebookResources function    
 - 複製Facebook SDK 首頁的範例程式碼，並貼在這個function裏頭
     ```
     require_once __DIR__ . '/vendor/autoload.php'; // change path as needed
     
     $fb = new \Facebook\Facebook([
       'app_id' => '{app-id}',
       'app_secret' => '{app-secret}',
       'default_graph_version' => 'v2.10',
       //'default_access_token' => '{access-token}', // optional
     ]);
     
     // Use one of the helper classes to get a Facebook\Authentication\AccessToken entity.
     //   $helper = $fb->getRedirectLoginHelper();
     //   $helper = $fb->getJavaScriptHelper();
     //   $helper = $fb->getCanvasHelper();
     //   $helper = $fb->getPageTabHelper();
     
     try {
       // Get the \Facebook\GraphNodes\GraphUser object for the current user.
       // If you provided a 'default_access_token', the '{access-token}' is optional.
       $response = $fb->get('/me', '{access-token}');
     } catch(\Facebook\Exceptions\FacebookResponseException $e) {
       // When Graph returns an error
       echo 'Graph returned an error: ' . $e->getMessage();
       exit;
     } catch(\Facebook\Exceptions\FacebookSDKException $e) {
       // When validation fails or other local issues
       echo 'Facebook SDK returned an error: ' . $e->getMessage();
       exit;
     }
     
     $me = $response->getGraphUser();
     echo 'Logged in as ' . $me->getName();
     ```
     
### 填入`應用程式編號`以及`應用程式密鑰`    
 - 上頭的範例中，在以下地方填入我們從FB開發者帳號中得到的資訊
  ```
     $fb = new \Facebook\Facebook([
       'app_id' => '應用程式編號',
       'app_secret' => '應用程式密鑰',
       'default_graph_version' => '目前版本',
       //'default_access_token' => '{access-token}', // optional
     ]);
  ```
### 建立使用者登入按鈕     
 - 使用者要先登入進而拿到token，我們才可以使用token來做事 
 - 於routes/web.php檔案中，新建一個給登入頁面使用的route
    ```
    Route::get('/FBToken', function(){
    return view('FBToken');
    });
    ```
 - 於resources/views/資料夾底下，新增`FBToken.blade` PHP檔, 然後在裡頭貼上以下的JS code
 ```
 <!DOCTYPE html>
 <html>
 <head>
 <title>Facebook Login JavaScript Example</title>
 <meta charset="UTF-8">
 </head>
 <body>
 <script>
   // This is called with the results from from FB.getLoginStatus().
   function statusChangeCallback(response) {
     console.log('statusChangeCallback');
     console.log(response);
     // The response object is returned with a status field that lets the
     // app know the current login status of the person.
     // Full docs on the response object can be found in the documentation
     // for FB.getLoginStatus().
     if (response.status === 'connected') {
       // Logged into your app and Facebook.
       testAPI();
     } else {
       // The person is not logged into your app or we are unable to tell.
       document.getElementById('status').innerHTML = 'Please log ' +
         'into this app.';
     }
   }
 
   // This function is called when someone finishes with the Login
   // Button.  See the onlogin handler attached to it in the sample
   // code below.
   function checkLoginState() {
     FB.getLoginStatus(function(response) {
       statusChangeCallback(response);
     });
   }
 
   window.fbAsyncInit = function() {
     FB.init({
       appId      : '{your-app-id}',
       cookie     : true,  // enable cookies to allow the server to access 
                           // the session
       xfbml      : true,  // parse social plugins on this page
       version    : '{api-version}' // The Graph API version to use for the call
     });
 
     // Now that we've initialized the JavaScript SDK, we call 
     // FB.getLoginStatus().  This function gets the state of the
     // person visiting this page and can return one of three states to
     // the callback you provide.  They can be:
     //
     // 1. Logged into your app ('connected')
     // 2. Logged into Facebook, but not your app ('not_authorized')
     // 3. Not logged into Facebook and can't tell if they are logged into
     //    your app or not.
     //
     // These three cases are handled in the callback function.
 
     FB.getLoginStatus(function(response) {
       statusChangeCallback(response);
     });
 
   };
 
   // Load the SDK asynchronously
   (function(d, s, id) {
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) return;
     js = d.createElement(s); js.id = id;
     js.src = "https://connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
 
   // Here we run a very simple test of the Graph API after login is
   // successful.  See statusChangeCallback() for when this call is made.
   function testAPI() {
     console.log('Welcome!  Fetching your information.... ');
     FB.api('/me', function(response) {
       console.log('Successful login for: ' + response.name);
       document.getElementById('status').innerHTML =
         'Thanks for logging in, ' + response.name + '!';
     });
   }
 </script>
 
 <!--
   Below we include the Login Button social plugin. This button uses
   the JavaScript SDK to present a graphical Login button that triggers
   the FB.login() function when clicked.
 -->
 
 <fb:login-button scope="public_profile,email" onlogin="checkLoginState();">
 </fb:login-button>
 
 <div id="status">
 </div>
 
 </body>
 </html>
 ```
 - 同樣，在上面的程式碼中需填入編號及版本號，如下
  ```
     FB.init({
       appId      : '編號',
       cookie     : true,  // enable cookies to allow the server to access 
                           // the session
       xfbml      : true,  // parse social plugins on this page
       version    : '版本' // The Graph API version to use for the call
     });
  ```

### 熟悉FB graph API 工具
 - 利用FB graph API測試工具，我們可以找到我們需要的API
 ![](https://i.imgur.com/Dt9a26B.png)
 
### 客製endpoint
 - 因為之後我們可能會直接複製經由graph API 工具 取得的endpoint，如下：
![](https://i.imgur.com/kh7oNEt.png) 

 - 所以我們可以把endpoint這段移到.env中，如下：
  ```
        $endpoint = env('FBEndpoint');
        try
        {
            // Get the \Facebook\GraphNodes\GraphUser object for the current user.
            // If you provided a 'default_access_token', the '{access-token}' is optional.
            $response = $fb->get($endpoint, $token);
  ```
 - 然後.env裡頭
  ```
FBEndpoint=me?fields=id,name,email
  ```
 - 如此一來，之後我們只要直接複製graph API取得的值，貼到.env，打完收工！

### 修改錯誤回傳值
 - PHP SDK預設錯誤時，會依照錯誤狀況回傳錯誤訊息，可我只需知道true or false，就行了，token無效有可能是因為以下幾種狀況
    - 根本沒帶
    - 帶的是錯的
    - 過期了
 - 不管是哪一種，我都需要回傳錯誤訊息給前端，並要求前端再去跟FB要一次，拿對的來，所以說，我必須要判斷PHP SDK的輸出，有沒有錯誤，若錯做一件事，對也做一件事，因此我們需要修改原本錯誤訊息輸出的地方，改成簡單的true or false，如下：
  ```
  catch (\Facebook\Exceptions\FacebookResponseException $e)
        {
            return false;
//            echo 'Graph returned an error: ' . $e->getMessage();
//            exit;
        }
  ``` 

### 取得public url
 - 使用ngrok取得用來拿token，HTML頁面的public url
 - 登入開發者應用程式 => 找到產品Facebook登入 => 快速入門 => 網站 => 貼上public url
![](https://i.imgur.com/Wogb6vc.png)

### 登入取得token

### 將token打到Laravel裡的FBController裡頭的getFacebookResources

### 將取得的資料，存入資料庫，完成會員建檔

### 打完收工
