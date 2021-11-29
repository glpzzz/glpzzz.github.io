---
title:  "How to add Open Graph and Twitter Card tags to Yii2 website"
date:   2020-09-10 00:00:00 -0500
description: "Small tutorial on how to use TaggedView Yii2 extension made by Daxslab to add OpenGraph and Twitter Card tags to any Yii2 application or website. Efficient and time saver!"
image: https://glpzzz.is-a.dev/assets/opengraph-twitter-card-yii2-websites.png
---

[OpenGraph](https://ogp.me/) and [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards) are two metadata sets that allow to describe web pages and make it more understandable for Facebook and Twitter respectively.

There a lot of meta tags to add to a simple webpage, so let's use [TaggedView](https://github.com/daxslab/yii2-taggedview)

This component overrides the `yii\web\View` adding more attributes to it, allowing to set the values on every view. Usually we setup page title with

```php
$this->title = $model->title;
```

Now, with **TaggedView** we are able to set:

```php
$this->title = $model->title;
$this->description = $model->abstract;
$this->image = $model->image;
$this->keywords = ['foo', 'bar'];
```

And this will generate the proper OpenGraph, Twitter Card and HTML meta description tags for this page. 

Also, we can define default values for every tag in the component configuration that will be available for every page and just will be overriden if redefined as in previous example.

```php
'components' => [
    //...
    'view' => [
        'class' => 'daxslab\taggedview\View',
        'site_name' => '',
        'author' => '',
        'locale' => '',
        'generator' => '',
        'updated_time' => '',
    ],
    //...
]
```

Some of this properties have default values assigned, like `site_name` that gets `Yii::$app->name` by default.

Result of usage on a website:

```html
<title>¿Deseas comprar o vender una casa en Cuba? | HogarEnCuba, para comprar y vender casas en Cuba</title>
<meta name="author" content="Daxslab (https://www.daxslab.com)">
<meta name="description" content="Hay 580 casas...">
<meta name="generator" content="Yii2 PHP Framework (http://www.yiiframework.com)">
<meta name="keywords" content="HogarEnCuba, ...">
<meta name="robots" content="follow">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:description" content="Hay 580 casas...">
<meta name="twitter:image" content="https://www.hogarencuba.com/images/main-identifier_es.png">
<meta name="twitter:site" content="HogarEnCuba">
<meta name="twitter:title" content="¿Deseas comprar o vender una casa en Cuba?">
<meta name="twitter:type" content="website">
<meta name="twitter:url" content="https://www.hogarencuba.com/">
<meta property="og:description" content="Hay 580 casas...">
<meta property="og:image" content="https://www.hogarencuba.com/images/main-identifier_es.png">
<meta property="og:locale" content="es">
<meta property="og:site_name" content="HogarEnCuba">
<meta property="og:title" content="¿Deseas comprar o vender una casa en Cuba?">
<meta property="og:type" content="website">
<meta property="og:updated_time" content="10 sept. 2020 9:43:00">
```

