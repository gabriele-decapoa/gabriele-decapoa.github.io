---
layout: default
tag: callback
permalink: "/category/callback"
---
### {{ page.tag | capitalize }}
  {% assign category = site.categories %}
  {% for tag in category %}
    {{% if tag[0] == page.tag %}}
      {% assign pages_list = tag[1] %}
        {% for post in pages_list %}
          {% if post.title != null %}
            {% if group == null or group == post.group %}
            - [{{ post.title }}]({{ site.url }}{{ post.url }}) ({{% post.date | date: "%B %e, %Y" %}})
            {% endif %}
          {% endif %}
        {% endfor %}
        {% assign pages_list = nil %}
    {{% endif %}}
  {% endfor %}
