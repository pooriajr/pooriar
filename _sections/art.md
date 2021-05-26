---
heading: Art
subtitle: Necessary or Not?
---

<div class="image-grid">
{% assign arts = site.art | sample:4 %}
{% for art in arts %}
<a href="{{ art.url }}">
  <img class="thumbnail" src="assets/images/art/thumbnails/{{art.image_path}}" alt="">
</a>
{% endfor %}
</div>


[More Art >]({% link art.html %})
