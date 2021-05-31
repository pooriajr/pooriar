---
heading: Art
subtitle: Necessary or Not?
---

<div class="image-grid">
{% assign arts = site.art | sample:4 %}
{% for art in arts %}
<a href="{{ art.url }}">
  <img class="thumbnail" src="assets//art/thumbnails/{{art.file}}" alt="">
</a>
{% endfor %}
</div>


[More Art >]({% link gallery.html %})
