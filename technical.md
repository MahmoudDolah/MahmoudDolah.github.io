---
layout: page
title: Technical
permalink: /technical/
---

Posts about DevOps, Linux, AI/LLM tooling, and other engineering topics.

{% assign technical_categories = "DevOps,Magento,Linux,linux,macOS,Ansible,Automation,Configuration-Management,graphql,apachebench,Monitoring,monitoring,Observability,Kibana,Elasticsearch,ELK,MongoDB,Databases,AI,ai,ChatGPT,Gemini,LLM,llm,Homelab,Git,DNS,Terraform,open-source,security" | split: "," %}

<ul class="post-list">
  {%- for post in site.posts -%}
    {%- assign matched = false -%}
    {%- for cat in post.categories -%}
      {%- if technical_categories contains cat -%}
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
