---
layout: blog
title: 
permalink: /blog/
---

<br>

## Blog Posts

### Research
<ul>
  {% for post in site.posts %}
    {% if post.category == 'Research' %}
    <li style="margin-bottom: 10px;">
      <span style="font-family: 'Ubuntu Mono', monospace; color: #828282; font-size: 0.9rem; margin-right: 10px;">
        {{ post.date | date: "%b %d, %Y" }}
      </span>
      <a href="{{ post.url | relative_url }}" style="font-weight: 500; margin-right: 10px;">{{ post.title }}</a>   
      {% if post.code %}
      <a href="{{ post.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:10px; padding: 2px 6px;">Code</a>
      {% endif %}
    </li>
    {% endif %}
  {% endfor %}
</ul>

<br>

### Software
<ul>
  {% for post in site.posts %}
    {% if post.category == 'Software' %}
    <li style="margin-bottom: 10px;">
      <span style="font-family: 'Ubuntu Mono', monospace; color: #828282; font-size: 0.9rem; margin-right: 10px;">
        {{ post.date | date: "%b %d, %Y" }}
      </span>
      <a href="{{ post.url | relative_url }}" style="font-weight: 500; margin-right: 10px;">{{ post.title }}</a>
      {% if post.code %}
      <a href="{{ post.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:10px; padding: 2px 6px;">Code</a>
      {% endif %}
    </li>
    {% endif %}
  {% endfor %}
</ul>