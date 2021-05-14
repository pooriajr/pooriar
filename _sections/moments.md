---
heading: A Random Moment
subtitle: Fragments of the past; evidence of life and love.
---

{% assign moment = site.moments | sample %}
<div class="moment">
{{ moment.content }}
</div>

[See all Moments]({% link moments.html %})
