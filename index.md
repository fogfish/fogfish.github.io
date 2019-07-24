---
layout: default
---

# Posts

<ul class="posts">
   {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
   {% endfor %}
</ul>


[Technology Radar]({{ site.baseurl }}{% link tech/tech.md %}){: .btn .fs-5 .mb-4 .mb-md-0 .mr-2 }
