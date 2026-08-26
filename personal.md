---
layout: single
title: Personal
permalink: /personal/
---

Posts about hobbies, musings, and everything that isn't strictly technical.

{% assign personal_categories = "Basketball,Hobbies,Personal,Learning,Books,Musings,Homelab" | split: "," %}

<div class="entries-list">
  {%- for post in site.posts -%}
    {%- assign matched = false -%}
    {%- for cat in post.categories -%}
      {%- if personal_categories contains cat -%}
        {%- assign matched = true -%}
      {%- endif -%}
    {%- endfor -%}
    {%- if matched -%}
      {% include archive-single.html type="list" %}
    {%- endif -%}
  {%- endfor -%}
</div>
