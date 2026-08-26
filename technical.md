---
layout: single
title: Technical
permalink: /technical/
---

Posts about DevOps, Linux, AI/LLM tooling, and other engineering topics.

{% assign technical_categories = "DevOps,Magento,Linux,linux,macOS,Ansible,Automation,Configuration-Management,graphql,apachebench,Monitoring,monitoring,Observability,Kibana,Elasticsearch,ELK,MongoDB,Databases,AI,ai,ChatGPT,Gemini,LLM,llm,Homelab,Git,DNS,Terraform,open-source,security" | split: "," %}

<div class="entries-list">
  {%- for post in site.posts -%}
    {%- assign matched = false -%}
    {%- for cat in post.categories -%}
      {%- if technical_categories contains cat -%}
        {%- assign matched = true -%}
      {%- endif -%}
    {%- endfor -%}
    {%- if matched -%}
      {% include archive-single.html type="list" %}
    {%- endif -%}
  {%- endfor -%}
</div>
