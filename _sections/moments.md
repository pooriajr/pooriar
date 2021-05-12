---
heading: Moments
subtitle: Fragments of the past; evidence of life and love.
---

{% for moment in site.moments limit:3 %}
<div class="moment">
{{ moment.content }}
</div>
{% endfor %}

[See all Moments]({% link moments.html %})
