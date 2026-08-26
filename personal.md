---
layout: page
title: Personal
permalink: /personal/
---

Posts about hobbies, musings, and everything that isn't strictly technical.

{% assign personal_categories = "Basketball,Hobbies,Personal,Learning,Books,Musings,Homelab" | split: "," %}

<ul class="post-list">
  {%- for post in site.posts -%}
    {%- assign matched = false -%}
    {%- for cat in post.categories -%}
      {%- if personal_categories contains cat -%}
        {%- assign matched = true -%}
      {%- endif -%}
    {%- endfor -%}
    {%- if matched -%}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
      <h3>
        <a class="post-link" href="{{ post.url | relative_url }}">
          {{ post.title | escape }}
        </a>
      </h3>
    </li>
    {%- endif -%}
  {%- endfor -%}
</ul>
