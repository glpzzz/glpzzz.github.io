---
title: "Hello! I'm @glpzzz"
description: "I'm Gabriel Alejandro López López (glpzzz) and I'm a software engineer. Working at @daxslab and always looking for new business opportunities. Bootstrap, C++, C, CSS3, Docker, HTML5, Java, JavaScript, jQuery, MySQL, PHP, PostgreSQL, Python, Qt, Yii PHP Framework and more. Let me know if I can help you!"
---

# {{page.title}}

{{site.description}}

Here, I blog mostly about tech...

<ul id="post-list">
    {% for post in site.posts %}
    <li>
        <time datetime="{{ post.date }}">{{ post.date | date: '%B %d, %Y' }}</time>
        <h2>
            <a href="{{ post.url }}">{{ post.title }}:</a>
        </h2>
        <p class="description-post">
            {{ post.description | xml_escape }}
        </p>
    </li>
    {% endfor %}
</ul>