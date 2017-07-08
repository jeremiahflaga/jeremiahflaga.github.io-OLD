---
layout: page
title: Archive
permalink: /archive/
---

<!-- archive page code from http://chris.house/blog/building-a-simple-archive-page-with-jekyll -->

<section class="archive-post-list">

   {% for post in site.posts %}
       {% assign currentDate = post.date | date: "%B %Y" %}
       {% if currentDate != myDate %}
           {% unless forloop.first %}</ul>{% endunless %}
           <h2>{{ currentDate }}</h2>
           <ul>
           {% assign myDate = currentDate %}
       {% endif %}
       <li>
            <span>{{ post.date | date: "%B %-d, %Y" }}</span> - 
            <a href="{{ post.url }}">{{ post.title }}</a>
       </li>
       {% if forloop.last %}</ul>{% endif %}
   {% endfor %}

</section>
