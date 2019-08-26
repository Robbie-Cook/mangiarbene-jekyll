---
layout: layout-blog
category: Development
url: "https://trim-seahorse.cloudvent.net"
---

One of the best so called 'unusual use cases of Jekyll' is learning the basics of web development: html, css and JavaScript. But as the JavaScript part is growing in your application the need of a package manager like npm comes around and with that we reach the limits of using Jekyll. Although not impossible the production environment might becomes too complicated using two different compilers at the same time. Yet it we don't have to say goodbye to Jekyll at this point at all. It is very well possible to use Jekyll in creating API endpoints that can be consumed by other website generators. In this demo I will show the combination with React.

We start with a simple Jekyll website that constraints of two collections ('cookbooks' and 'recipes') as well as a blog from which this post is the 'latest'.

.── _books
├── _config.yml
├── _includes
├── _layouts
├── _posts
├── _recipes
├── _sass
├── _site
├── blog.md
├── books.md
├── index.md
└── recipes.html


In Jekyll, we can call the recipes of a book, using a condition:


{% for recipe in site.data.recipes %}
	{%- if recipe.book == page.title %}
		{{ recipe.title }}
	{% endif %}
{% endfor %}

Likewise we can call the book of a recipe:

{% for book in site.books %}
	{% if book.title == page.book %}
		{{  book.title }}
	{% endif %}
{% endfor %} 

Both examples result in a data schema that is similar to the many-to-many-relationships used in non-relational databases like MongoDB. So what if in Jekyll we could use this technique to create a json file that contains the same structure. And we sure can! 

Create a folder 'api' with files for blog, books and recipes, like so:

├── api
|   ├── blog.json
|   ├── books.json
|   └── recipes.json

Now there is only one problem here, fortunatly with a simple solution. In order to achieve valid json, the last iteration cannot end with a comma. And because the if statement filters from an array, and therefore not all iterations will become valid, a comma cannot be avoided here.

The solution is to create a new array, using the push method, so we get rid of the conditional statement.

{% assign p = book.title %}
{% assign my_recipe = "" | split: "" %}
{% for recipetitle in site.recipes %}
	{% assign thetitle = recipetitle.book %}
	{% if p == thetitle %}            
		{% assign itemtitle = "" | split: "" %}            
		{% assign itemtitle = itemtitle | push: recipetitle %}         
		{% assign my_recipe = my_recipe | push: itemtitle %} 
	{% endif %}
{% endfor %}

Finally check if the json is valid go to https://jsonlint.com/.

I published the result on Github and Cloudcannon.

Now we can use the json file calling: https://trim-seahorse.cloudvent.net/


Jekyll api endpoints

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


consume in React

(mangiarbene.io)

