---
layout: default
title: Highlights about technologies
permalink: /
search_exclude: true
---

<style>
ul.posts {
   list-style-type: none;
   margin-bottom: 2em;
}

ul.posts li {
   line-height: 1.75em;
}

ul.posts span {
   color: #aaa;
   font-family: Monaco, "Courier New", monospace;
   font-size: 80%;
}
</style>


# Highlights about technologies 

[Technology Radar]({{ site.baseurl }}{% link tech/tech.md %}){: .btn .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Tags Cloud]({{ site.baseurl }}{% link tags.md %}){: .btn .fs-5 .mb-4 .mb-md-0 .mr-2 }

<ul class="posts">
   {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
   {% endfor %}
</ul>
