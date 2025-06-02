---
layout  : wikiindex
title   : Wiki
toc     : true
public  : true
comment : false
updated : 2023-12-25 13:36:35 +0900
regenerate: true
---

## Wiki items

* [[Project]]
    * [[Vimwiki-Project]]
* [[Processing]]
    * [[Winter]]
* [[Diary]]
    * [[2023-memo]]
* [[Study]]
    * [[C-lang]]
    * [[Git]]
    * [[Vim]]
    * [[Ubuntu]]
    * [[incubating]]

---

## Blog posts
<div>
    <ul>
{% for post in site.posts %}
    {% if post.public != false %}
        <li>
            <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">
                {{ post.title }}
            </a>
        </li>
    {% endif %}
{% endfor %}
    </ul>
</div>

