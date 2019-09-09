---
title: Jekyll API endpoint
layout: layout-blog
category: Development
url: "https://trim-seahorse.cloudvent.net"
author: Pieter Roozen
source: ""
source_url: ""
---

#### Talk at the Jekyll video conference 2019

> One of the best so called **unconventional use cases of Jekyll** is learning the basics of web development: HTML, CSS and JavaScript. But as the JavaScript part is growing in your application, the need of a package manager like npm comes around, and with that we reach the limits of using Jekyll. Although not impossible to integrate with npm, the production environment might become too complicated, using two different compilers at the same time. Yet we don't have to say goodbye to Jekyll at this point at all. 

>It is very well possible to use Jekyll in creating API endpoints, that can be consumed by other website generators. In this demo I will show the combination with React. That way you are completely free in your development, while enjoying the bennefits of a simple, static and blog-aware Jekyll application. This time as a real backend.

<br/>

# Hello,

<div class="home-img_pieter">
    <img src="/public/img/pieter0.jpg" alt="">   
</div>

**...my name is Pieter Roozen and I live in The Hague, the Netherlands. I am 65 years old, father of 4 and partner of Ruth. I love skitouring and sailing and I bake sourdough bread on a daily basis.**

<br/>

<div class="home-img_vangogh">
    <img src="/public/img/museums.jpg" alt="">   
</div>

## My former life as a designer

In my former life I was a graphic designer, specialised in designing books, art catalogues and museum concepts for the 'big' Dutch museums. Well, Dutch museums aren't that big, but the Van Gogh Museum is famous anough. Among other, our design agency was for more than 15 years in full service for the VGM. We contributed to all the major pioneer work in presenting a modern museum to a broad audience, including the automation of text content and color management of the museum's reproduction output.

<br/>
<div class="home-img_vangogh">
    <img src="/public/img/van-gogh-00.jpg" alt="">
</div>

## My next step

At my age I could retire and make bicycle tours along the Dutch coast, sail around the world, or take long holliday trips. But I decided to learn web development.

## Jekyll

At this point I found Jekyll very usefull in learning the basics of HTML, CSS and JavaScript. We all know why: Jekyll is simple, fast and secure and very well suited for presentational websites like portfolio's, blogs or even museum websites. In combination with the <a href="https://cloudcannon.com/" target="_blank" rel="noopener noreferrer">**CloudCannon**</a> CMS many would find it even a superior alternative for Wordpress, Drupal or other PHP frameworks like Laravel. I think this would apply to all situations where the user does not need to realtime interact with a  RESTfull backend.

Of course the portfolio website of my design agency <a href="https://roozen.nl" target="_blank" rel="noopener noreferrer">**Studio Roozen**</a> was also made in Jekyll.

## Jekyll separates content from logic

The reason why I love Jekyll is because it has a very elegant way of separating content from logic, including the HTML. You can use the Markdown language to write coded content without writing the code yourself. In fact I recognized that it is very similar to the way in 'the early days', where we used text content written in Word in combination with the QuarkXpress DTP software.

So it all feels very intuïtive for an old school graphic designer, like me.

## RESTfull api's

These days, API's to transfer data around the web are very popular. API's are RESTfull if you can do all the HTTP requests like GET, PUT, POST and DELETE, but very often you only need the GET request in order to present your content to the user. Drupal and even Wordpress, which I already mentioned before, recently came with plugins for RESTfull API solutions.

## How about Jekyll?

So how about Jekyll, as it is very well suited in seperating content from the logic? And yes, you definitly can do! 

This is where my talk is all about.

Basically, API's are used to make the same content available to different applications in a form that can be parsed in the desired language. But let me be clear: obviously you cannot make Jekyll RESTfull, because it is static. Nevertheless it is very well possible to create a so called API endpoint, and consume the data in an other application.

## MangiarBene

For this demo I designed a simple Jekyll website about Cookbooks (what else...? :-) called **MangiarBene**, dedicated to the famous Italian businessman and writer Pellegrino Artusi (1820–1911), and which in this case is the actual website you are visiting right now. It consists of two collections: one of **15 cookbooks** and another of **48 recipes** as well as a **blog** from which this actual post is the 'latest'.

<div class="home-img_websites">
    <img src="/public/img/jekyll.jpg" alt="">
    <img src="/public/img/react.jpg" alt="">
</div>

You can go to the [homepage](/) of **MangiarBene** now, if you want, and see how the website is structured. 

Subsequently I developed a second SPA website (in React), that just uses the content from **MangiarBene** as JSON data. You could also examin this at <a href="https://api.roozen.nl" target="_blank" rel="noopener noreferrer">
https://api.roozen.nl
</a>. You will see that it is identical, only the background color is different.

But let me explain first.

## Many to many relationships

In Jekyll it is possible to relate collections and posts from the content of the YAML front matter blocks. We can call the recipes of a book, using the condition:

**if recipe.book == page.title**

{% raw %}
```html
<div class="book-links">
    {%- for recipe in site.recipes -%}
    {%- if recipe.book == page.title -%}
    <a href="/recipes/{{ recipe.title | slugify }}">
    <h5>{{ recipe.title }} {{ recipe.index }}</h5>
    </a>
    {%- endif -%}
    {%- endfor -%}    
</div>
```
{% endraw %}

This way, we can get all the recipes of the collection recipes, along with all the data of that recipe and add it to a certain book of the collection books.

Likewise we can add the data of a book to a recipe:

**if book.title == page.book**

{% raw %}
```html
<div class="recipe-box_credits">
    {%- for book in site.books -%}
    {%- if book.title == page.book -%}
        <h6>{{  book.title }}</h6>    
        <p>{{  book.author }}</p>     
        <p>{{  book.content | truncatewords: 20 }} <span>Read more ></span></p>     
    {%- endif -%}
    {%- endfor -%}           
</div>
```
{% endraw %}

Both result in a data schema that is similar to the many-to-many-relationships used in non-relational databases, like MongoDB. So what if in Jekyll we could use this technique to create a JSON file that contains the same structure, and make it available to the internet? And we sure can! 

## Constructing the API endpoints

Let's create a folder called **api** with files for **books.json** and **recipes.json**. In the front matter block we just set the layout to null and that's it. Now we can construct a JSON file and establish the same relations as we did before in creating the pages for books, recipes and posts.

The form of the JSON is very mutch dependent of the language in which the data are consumed. In the case of React the best advise is to keep the JSON tree as flat as possible. This pattern is called **denormalisation** is well known in non-relational databases as MongoDB. In this case we would 'connect' the **recipes** data with the **books** data using a **bookId** instead of calling all the data of each book as nested in the **recipes** object.

## Nested structures

With Jekyll we could establish every structure of the JSON we want, also deeply nested ones.

There is only one problem here (fortunatly with a simple solution). In order to achieve valid JSON, the last iteration cannot end with a comma. Since the if-statement works as a filter, and therefore not all iterations will become valid, a comma cannot be avoided here. Let me explain.

In Liquid you can eleminate the last comma, with the **unless forloop.last** statement. But this wil not work in case the last forloop is not valid to the condition. In other words, the last forloop which is valid to the condition might not be the last forloop of the forloop as a whole. Resulting in the last comma.

The solution is to create a new empty array, while using the push method to gather the desired content. This way we get rid of the conditional statement. 

{% raw %}
```html
---
layout: null
---
[ 
    {% for book in site.books %}
        {
            "title"    : "{{ book.title }}",
            "author"   : "{{ book.author }}",
            "publisher": "{{ book.publisher }}",
            "year"     : "{{ book.year }}",
            "kitchen"  : "{{ book.kitchen }}",
            "link"     : "{{ book.link }}",
            "book_url" : "{{ book.book_url }}",
            {% assign p = book.title | slugify %}
            {% assign my_recipe = "" | split: "" %}
            {% for recipetitle in site.recipes %}
            {% assign detitel = recipetitle.book | slugify %}
            {% if p == detitel %}
            {% assign itemtitle = "" | split: "" %}
            {% assign itemtitle = itemtitle | push: recipetitle %}
            {% assign my_recipe = my_recipe | push: itemtitle %}
            {% endif %}
            {% endfor %}
            "recipes"  : [ 
                {% for therecipe in my_recipe %}  
                {
                    "index"   : "{{ therecipe[0].index }}",
                    "title"   : "{{ therecipe[0].title }}",
                    "page"    : "{{ therecipe[0].page }}",
                    "product" : "{{ therecipe[0].product }}",
                    "dish"    : "{{ therecipe[0].dish }}"
                }{% unless forloop.last %},{% endunless %}
                {% endfor %}
            ],
            "text"  : "{{ book.text }}",
            "content"  : {{ book.content | jsonify }}
        }{% unless forloop.last %},{% endunless %}
    {% endfor %}
]
```
{% endraw %}

Finally check if the JSON is valid go to: 
<a href="https://jsonlint.com/" target="_blank" rel="noopener noreferrer">
https://jsonlint.com/
</a>.

{% raw %}
```html
[ 
    {
        "index": "1",
        "title": "Candied citrus peel",
        "id": "candied-citrus-peel",
        "product": "lemon",
        "dish": "basics",
        "url": "/recipes/candied-citrus-peel/",
        "book"       : [   
            {
                "index"    : "1",
                "title"    : "30 ingredients",
                "author"   : "Sally Clarke",
                "content"  : "<p>Britain’s pioneer of seasonal cooking Sally Clarke is back with a new collection of seasonal recipes to mark the 30th birthday year of her legendary Notting Hill restaurant.</p>\n\n<p>Known for pioneering seasonal fine dining in British cuisine, the award winning chef, restaurateur and author has chosen a handful of recipes for each of her favourite 30 ingredients in her stunning new cookery book. The simple idea of cooking with the freshest and best market produce, Sally Clarke’s vision for thirty years, is at the heart of her new book of ninety-five recipes.</p>\n"
            }
        ],
        "page": "160"
    },
...

```
{% endraw %}

## Jekyll as a blog API

More interesting maybe is the fact that we can use the same technique to create a blog API from the Jekyll data. Now in the **data** folder create a file called blog.json, and follow the same instructions as before. Here we want to use the **content** of a post as well, while using Markdown text-to-HTML conversion, resulting in HTML tags in your output. Therefore you need to use the **jsonify** filter here.

{% raw %}
```html
---
layout: null
---
[ 
    {% for book in site.books %}
        {
            "title"    : "{{ book.title }}",
            <!-- ... -->
            "content"  : {{ book.content | jsonify }}
        }{% unless forloop.last %},{% endunless %}
    {% endfor %}
]
```
{% endraw %}



## Publishing the Jekyll site on CloudCannon

Publish the result on Github and Cloudcannon, which in this case I already did, obviously.

Now we can use the created Jekyll API endpoints. As you can see they are part of this actual website.

<a href="https://trim-seahorse.cloudvent.net/api/recipes.json" target="_blank" rel="noopener noreferrer">
/api/recipes.json
</a>
<br>
<a href="https://trim-seahorse.cloudvent.net/api/books.json" target="_blank" rel="noopener noreferrer">
/api/books.json
</a>
<br>
<a href="https://trim-seahorse.cloudvent.net/api/blog.json" target="_blank" rel="noopener noreferrer">
/api/blog.json
</a>

## Using the API in React

Hosting the Jekyll website – which includes the API endpoints – on either GitHub or CloudCannon makes it easy to change the content, and see how this is in sync with the SPA that consumes the content, in this demo developed in React:
<a href="https://api.roozen.nl" target="_blank" rel="noopener noreferrer">
https://api.roozen.nl
</a>

## Other use cases

You can even use Jekyll to load a MongoDB database, which can be done by using a script in JavaScript and load different kinds of JSON files and structures at the same time or by just loading one JSON file directly. Here is a link to the documentation of <a href="https://docs.mongodb.com/manual/reference/program/mongoimport/" target="_blank" rel="noopener noreferrer">
MongoDB</a>.

That's all there is to it.

So now just use your imagination. There is more you can do with Jekyll, than with Jekyll on its own!
