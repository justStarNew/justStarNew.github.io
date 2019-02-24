---
layout: post
title: 'Jekyll Liquid'
date: 2019-02-24
author: Jekyll
color: rgb(255,210,32)
cover: '../assets/imgs/An-overview-of-Liquid-2016.jpg'
tags: jekyll liquid
---
# liquid
  [Liquid 文档](https://shopify.github.io/liquid/)  
  [cloudcannon 源代码](https://github.com/CloudCannon/bakery-store/tree/intro-to-liquid)
  
  ## 介绍
  Liquid是Jekyll用来处理站点的动态语言。你可以用Liquid来输出或修改变量。或者在你的页面中包含逻辑语句和循环内容。
  
  在Liquid中有两种标记:
  
  - 你可以用两个大括号包裹住他们来输出变量。例如 {{ 变量名 }}
  - 你也可以用一个大括号{}和百分号%包裹他们来执行逻辑语句。例如 {% if statement %}
  
  ## 输出
  
  让我们从一个简单的例子开始。我们在front matter中加入一些内容。然后使用front matter来输出它。在front matter中设置的变量在page.variable_name中可用。
  
  ~~~html
  ---
  heading: I like cupcakes
  ---
  <!doctype html>
  
  <html lang="en">
    <head>
      <meta charset="utf-8">
      <title></title>
    </head>
    <body>
      <h1>{{ page.heading }}</h1>
    </body>
  </html>
  ~~~
  
  ![上述结果](https://d1qmdf3vop2l07.cloudfront.net/quiet-lily.cloudvent.net/compressed/ac87fa365df599ec654dccafe24e70e4.png)
  
  ## Filter (过滤器)
  我们能将heading变成大写。我们可以对一个变量使用filter来改变输出。使用filter，我们将在变量名称后增加一个 | 然后将它传递给一个过滤器。在这里是uppcase
  
  ~~~ html
  ...
  <h1>{{ page.heading | upcase }}</h1>
  ...
  ~~~
  ![上述结果](https://d1qmdf3vop2l07.cloudfront.net/quiet-lily.cloudvent.net/compressed/bafba3dfe0ef790293beab50754d0635.png)
  
  我们可以对内容使用多个过滤器。下面我们增加一个过滤器来使得输出最多有八个字符。
  ~~~ html
  ...
  <h1>{{ page.heading | upcase | truncate: 8 }}</h1>
  ...
  ~~~
  ![上述结果](https://d1qmdf3vop2l07.cloudfront.net/quiet-lily.cloudvent.net/compressed/4e04789528dab50dec45c268cc4f8a9c.png)
  
  ## 逻辑语句
  让我们控制h1是否输出在页面上。我们将在front matter里增加一个叫做show_heading的变量，然后初始化为true.然后我们将h1包裹在一个if判断语句中。用来判断show_heading是否为true
  
  ~~~ html
  ---
  heading: I like cupcakes
  show_heading: true
  ---
  ...
  {% if page.show_heading %}
    <h1>{{ page.heading | upcase | truncate: 8 }}</h1>
  {% endif %}
  ...
  ~~~
  ![上述结果](https://d1qmdf3vop2l07.cloudfront.net/quiet-lily.cloudvent.net/compressed/4e04789528dab50dec45c268cc4f8a9c.png)
  
  当我们将show_heading修改为false时，它将展示为空。
  我们也可以添加else if来判断其他条件。如果page.show_heading为false,我们将检查page.heading是否包含cupcake字段，如果包含则输出I want cupcake.如果不包含则输出I don't want cupcakes.
  
  ~~~ html
  ---
  heading: I like cupcakes
  show_heading: false
  ---
  ...
  {% if page.show_heading %}
    <h1>{{ page.heading | upcase | truncate: 8 }}</h1>
  {% elsif page.heading contains "cupcake" %}
    <h1>I want cupcakes</h1>
  {% else %}
    <h1>I don't want cupcakes</h1>
  {% endif %}
  ...
  ~~~
  
  ## 循环 Loops
  Let’s create a cupcakes array in front matter then loop over it in Liquid and output it in an unordered list. The syntax to loop in Liquid is for variable in array, variable can named whatever you’d like and holds the item in the current iteration of the loop:
  ~~~ html
  ---
  heading: I like cupcakes
  show_heading: false
  cupcakes:
    - chocolate
    - lemon
    - strawberry
  ---
  ...
  <ul>
  {% for cupcake in page.cupcakes %}
    <li>{{ cupcake }}</li>
  {% endfor %}
  </ul>
  ...
  ~~~