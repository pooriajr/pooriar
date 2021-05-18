---
heading: A Random Moment
subtitle: Fragments of the past
---

{% assign moment = site.moments | sample %}
<div class="moment">
{{ moment.content }}
</div>

[More Moments >]({% link moments.html %})
