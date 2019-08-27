---
title: Jekyll API endpoint
layout: layout-blog
category: Development
url: "https://trim-seahorse.cloudvent.net"
author: Pieter Roozen
source: ""
source_url: ""
---

One of the best so called **unusual use cases of Jekyll** is learning the basics of web development: HTML, CSS and JavaScript. But as the JavaScript part is growing in your application the need of a package manager like npm comes around and with that we reach the limits of using Jekyll. Although not impossible to integrate with npm, the production environment might become too complicated using two different compilers at the same time. Yet we don't have to say goodbye to Jekyll at this point at all. 

>It is very well possible to use Jekyll in creating API endpoints that can be consumed by other website generators. In this demo I will show the combination with React. That way you are completely free in your development, while enjoying the bennefits of a simple, static and blog-aware Jekyll application, this time as a real backend.

## MangiarBene

We start with a simple Jekyll website called **MangiarBene**, which in this case is the actual website you are visiting right now. It constraints of two collections of **15 cookbooks** and **48 recipes** as well as a **blog** from which this actual post is the 'latest'.

You can go to the [homepage](/) of **MangiarBene** now, if you want and see how the website is structured. Finally we can go to a different OPA website (in React), which I developed for this demo, that just uses the content from **MangiarBene** as JSON data. But let me explain first.

## Many to many relationships

In Jekyll it is possible to relate collections and posts from the content of the YAML front matter blocks. We can call the recipes of a book, using the condition:

**if recipe.book == page.title**

This way we can get all the recipes of the collection recipes along with all the data of that recipe and add it to a certain book of the collection books.

Likewise we can add the data of a book to a recipe:

**if book.title == page.book**

Both result in a data schema that is similar to the many-to-many-relationships used in non-relational databases like MongoDB. So what if in Jekyll we could use this technique to create a JSON file that contains the same structure and make it available to the internet? And we sure can! 

## Constructing the API endpoints

Create a folder called **api** with files for **books.json** and **recipes.json**. In the front matter block we just set the layout to null and that's it. Now we can construct a JSON file and establish the same relations as we did before in creating the pages for books, recipes and posts.

There is only one problem here (fortunatly with a simple solution). In order to achieve valid JSON, the last iteration cannot end with a comma. Since the if-statement works as a filter, and therefore not all iterations will become valid, a comma cannot be avoided here. Let me explain.

In Liquid you can eleminate the last comma, with the **unless forloop.last** statement. But this wil not work in case the last forloop is not valid to the condition. In other words the last forloop which is valid to the condition might not be the last forloop of the forloop as a whole. Resulting in the last comma.

The solution is to create a new empty array, while using the push method to gather the desired content. This way we get rid of the conditional statement. 

Finally check if the JSON is valid go to: 
<a href="https://jsonlint.com/" target="_blank" rel="noopener noreferrer">
https://jsonlint.com/
</a>.


## Jekyll as a blog API

More interesting maybe is the fact that we can use the same technique to create a blog API from the Jekyll data. Now in the **data** folder create a file called blog.json and follow the same instructions as before. Here we want to use the **content** of a post as well, which contains the Liquid templating language, resulting in HTML tags in your output. Therefore you need to use the **jsonify** filter here.

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

Consuming the api's in React makes it possible to develop an almost similar website:

<a href="https://api.roozen.nl" target="_blank" rel="noopener noreferrer">
https://api.roozen.nl
</a>

