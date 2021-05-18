---
heading: Art
subtitle: Necessary or Not?
---

<div class="image-grid">
{% assign arts = site.art | sample:3 %}
{% for art in arts %}
<a class="thumbnail" href="{{ art.url }}">
  <div style="background-image: url('/assets/images/art/{{art.image_path}}');"></div>
</a>
{% endfor %}
</div>


[More Art >]({% link art.html %})
