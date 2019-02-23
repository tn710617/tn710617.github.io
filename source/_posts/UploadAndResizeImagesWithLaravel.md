---
title: Save and resize images in Laravel
date: 2019-02-22 16:39:39
tags: ['Laravel Image', 'Laravel package Intervention']
category:
---

##### <span style="color:blue">English</span>
<br>

使用Laravel儲存並重新縮放圖片大小
Save and resize images in Laravel
==
<hr>

## 前言
## Introduction
<p>
本篇為實際上使用Laravel，以及套件Intervention來儲存及重新修改圖片尺寸的學習筆記。
This article is my learning log of saving images and resize it afterwards with Laravel and its package Internention
</P>

## Install package Intervention
## 安裝套件Intervention
Please refer to the installing guide on its [official GitHub](https://github.com/Intervention/image)
安裝流程請參照Intervention[GitHub官網](https://github.com/Intervention/image)

 - `composer require intervention/image`
 - Find`config/app.php`, and add  `Intervention\Image\ImageServiceProvider::class` in array `$providers`, and add `'Image' => Intervention\Image\Facades\Image::class` in array `aliases`
 - 打開`config/app.php`, 在`$providers` array裡面加上`Intervention\Image\ImageServiceProvider::class`, 在`aliases` array 裡面加上`'Image' => Intervention\Image\Facades\Image::class`
 
## Build the link between external folder and internal folder where we are going to save images in.
## 建立上傳資料夾與storage資料夾的連結
- Following the [official website](https://laravel.com/docs/5.7/filesystem) and build the link
- 依照[官網](https://laravel.com/docs/5.7/filesystem)說明建立連結
    - 在terminal `php artisan storage:link`
    - In terminal `php artisan storage:link`
- 連結之後，`project/storage/app/public` 會跟 `project/public/storage`這兩個資料夾就回相連。
- After the command, the `project/storage/app/public` and `project/public/storage` will be linked and so shared with each other
    - When you save files, please save it to `project/storage/app/public/(anySubdirectoryYouWant)`
    - 如果你是要儲存檔案，請儲存到`project/storage/app/public/(anySubdirectoryYouWant)`
    - If you want to produce externally accessible URL, use `project/public/storage/(anySubdirectoryYouWant)/fileName`, because from external, the default accessible folder is `public`, so please use `asset('storage/(anySubdirectoryYouWant/fileName)')`
    - 如果你是要提供外部存取的URL，請使用`project/public/storage/(anySubdirectoryYouWant)/fileName`，因為對外部來說，預設可存取資料夾為`public`，所以直接使用`asset('storage/(anySubdirectoryYouWant/fileName)')`

## Validate if the image is brought 
## 驗證圖片是否有被帶進來

```
// Because we don't need a lot of stuff, so we could only take what's in $request array.
// 因為我們不需要太多的東西，只需要request array裡頭的東西
$parameters = request()->all();

if (request()->hasFile('image'))
{
    // The file exists, so we save it to project/storage/app/public, and get the URL. In case, we will get 'public/fileName'
    // 檔案存在，所以存到project/storage/app/public，並拿到url，此範例會拿到public/fileName
    $imageURL = request()->file('image')->store('public');
    // However, we only want to insert pure file name into the database, so we remove the 'public' and only leave 'fileName'
    // 因為我們只想要將純粹的檔名存到資料庫，所以特別做處理
    $parameters['image'] = substr($imageURL, 7);
}
```

## Re-organise the size of the image
## 重新縮放圖片大小

- We are going to do resizing, so we will need the package intervention.
- 要縮放大小，所以會需要使用到套件intervention
    - Below namespace, add `use Intervention\Image\ImageManagerStatic as Image;`
    - 在namespace下加上`use Intervention\Image\ImageManagerStatic as Image;`

```
// 拿到剛剛存進DB的item實例
// Get the instance of the item I just inserted
$item = Item::create($parameters);

// 設定driver
// Set the driver
Image::configure(array('driver' => 'gd'));

// 如果我們dd (storage_phth)，我們將會得到'project/storage/'，但這不是我們要的
// If we simply dd(storage_path), we will get 'project/store/', but it's not what we want.
// 所以我們在後面加上'app/public/，如上所敘，這是內部儲存的資料夾位址
// So we append 'app/public/', that's the internal file address
// 請注意，當我們重新縮放圖片大小，目標都是我們的內部資料夾
// Note that when resizing images, the target is in internal directory
// 並且，再重新縮放之後，也是要存到同樣的地方
// Also, after resizing, we save it with the same name in the same place.
Image::make(storage_path('app/public/' . $item->image))
->resize(300, 300)
->save(storage_path('app/public/' . $item->image));
```

## Produce publicly accessible URL
## 產出可存取資源的URL
```
// 當產出公開存取的URL，它必須要是外部存取位址
// When returning publicly accessible URL, it will have to be external address.
return asset('storage/' . $parameters['image']);
```






<br>
<br>
<br>
<br>
<br>
<hr>
<br>
<br>
<br>
<br>
<br>
<br>


##### <span style="color:red">繁體中文</span>
<br>

使用Laravel儲存並重新縮放圖片大小
==
<hr>

## 前言
<p>
本篇為實際上使用Laravel，以及套件Intervention來儲存及重新修改圖片尺寸的學習筆記。
</P>

## 安裝套件Intervention
安裝流程請參照Intervention[GitHub官網](https://github.com/Intervention/image)

 - `composer require intervention/image`
 - 打開`config/app.php`, 在`$providers` array裡面加上`Intervention\Image\ImageServiceProvider::class`, 在`aliases` array 裡面加上`'Image' => Intervention\Image\Facades\Image::class`
 
## 建立上傳資料夾與storage資料夾的連結
- 依照[官網](https://laravel.com/docs/5.7/filesystem)說明建立連結
    - 在terminal `php artisan storage:link`
- 連結之後，`project/storage/app/public` 會跟 `project/public/storage`這兩個資料夾就回相連。
    - 如果你是要儲存檔案，請儲存到`project/storage/app/public/(anySubdirectoryYouWant)`
    - 如果你是要提供外部存取的URL，請使用`project/public/storage/(anySubdirectoryYouWant)/fileName`，因為對外部來說，預設可存取資料夾為`public`，所以直接使用`asset('storage/(anySubdirectoryYouWant/fileName)')`

## 驗證圖片是否有被帶進來

```
// 因為我們不需要太多的東西，只需要request array裡頭的東西
$parameters = request()->all();

if (request()->hasFile('image'))
{
    // 檔案存在，所以存到project/storage/app/public，並拿到url，此範例會拿到public/fileName
    $imageURL = request()->file('image')->store('public');
    // 因為我們只想要將純粹的檔名存到資料庫，所以特別做處理
    $parameters['image'] = substr($imageURL, 7);
}
```

## 重新縮放圖片大小

- 要縮放大小，所以會需要使用到套件intervention
    - 在namespace下加上`use Intervention\Image\ImageManagerStatic as Image;`

```
// 拿到剛剛存進DB的item實例
$item = Item::create($parameters);

// 設定driver
Image::configure(array('driver' => 'gd'));

// 如果我們dd (storage_phth)，我們將會得到'project/storage/'，但這不是我們要的
// 所以我們在後面加上'app/public/，如上所敘，這是內部儲存的資料夾位址
// 請注意，當我們重新縮放圖片大小，目標都是我們的內部資料夾
// 並且，再重新縮放之後，也是要存到同樣的地方
Image::make(storage_path('app/public/' . $item->image))
->resize(300, 300)
->save(storage_path('app/public/' . $item->image));
```

## 產出可存取資源的URL
```
// 當產出公開存取的URL，它必須要是外部存取位址
return asset('storage/' . $parameters['image']);
```

