---
layout: page
title: Research Posts
hero_image: /assets/img/background.gif
---
{% assign posts = site.posts | where:"categories","publication" %}
<div class="columns is-multiline">
    {% for post in posts %}
    <div class="column is-3-desktop is-6-tablet">
        {% include post-card.html %}
    </div>
    {% endfor %}
</div>