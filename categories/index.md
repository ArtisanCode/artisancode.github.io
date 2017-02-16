---
layout: page
---
<h1>Post categories</h1>
<p>&nbsp;</p>
{% capture categories %}
  {% for category in site.categories %}
    {{ category[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedcategories = categories | split:' ' | sort %}
{% for category in sortedcategories %}
  <h3 id="{{ category }}">{{ category }}</h3>
  <ul>
  {% for post in site.categories[category] %}
    <li>
      {% include postlistitem.html %}
    </li>
  {% endfor %}
  </ul>
{% endfor %}
