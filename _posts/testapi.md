---
layout: null
permalink: /post.json
---
[{% for post in site.posts %}
    {
        "title": {{ post.title | jsonify }},
        "url": {{ post.url | jsonify }},
        "category": {{ post.category | jsonify }},
        "date": {{ post.date | jsonify }},
        "tags": {{ post.tags | jsonify }},
        "content": {{ post.content | jsonify }}
    }{% unless forloop.last %},{% endunless %}
{% endfor %}]
