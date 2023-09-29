---
title: "Hello, I'm @glpzzz"
description: "I'm Gabriel Alejandro López López (glpzzz) and I'm a software engineer. Working at @daxslab and always looking for new business opportunities. Bootstrap, C++, C, CSS3, Docker, HTML5, Java, JavaScript, jQuery, MySQL, PHP, PostgreSQL, Python, Qt, Yii PHP Framework and more. Let me know if I can help you!"
---

<ul>
    {% for post in site.posts %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
