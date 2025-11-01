---
layout: post
title:  "Simple LLM Security Platform Tool"
date:   2025-10-31 06:39:32 -0400

categories: llm ai security
---

I've been thinking more about security in regards to LLMs. Did you know that OWASP publishes a [top 10 for LLM applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) now?

While attempting to learn more about operating LLM Applications, I became curious about the security implications of operating these types of services in a production environment. So I built a simple security tool to protect LLMs from known attack vectors (assisted by Claude)

The tool protects against adversarial prompts using simple pattern matching and implements rate limiting

It can be found [here](https://github.com/MahmoudDolah/llm-security-platform)