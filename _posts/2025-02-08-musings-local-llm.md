---
layout: post
title:  "Musings on Running LLMs Locally"
date:   2025-02-08 06:39:32 -0400

categories: Musings AI LLM
---

In an effort to find something interesting to do on my home server, I've been experimenting with running LLMs locally after being inspired by some others on social media

This process is pretty straightforward and has been documented in many other posts (see the references section below), but it merits repeating here (if nothing else then for my own sanity)

# Instructions

1. Install [ollama](https://ollama.com/download)

![screenshot of ollama download page](/assets/ollama-download-page.png)

*Who doesn't love a greate `curl` piped into `sh` :p*

2. Run `ollama run <model>`. A list of models you can download and run out of the box can be found on their [model library](https://github.com/ollama/ollama/blob/main/README.md#model-library) doc. Since it's all the rage, I decided to play around with `deepseek-r1`

3. From there you can start playing around with the model. Since I had this running on my home server, I made API requests to play around with some queries

# Concluding Thoughts
To the surprise of probably no one, my HP Thin Client performed pretty abysmally, but it was a fun little project. I'm planning to upgrade the RAM (from 8GB to 16GB) and SSD (from 16GB to 64GB) and will revisit this after installation


# References:
- https://github.com/ollama/ollama/blob/main/README.md#model-library
- https://python.langchain.com/docs/how_to/local_llms/
- https://debuggercafe.com/adding-models-to-ollama/
- https://weisser-zwerg.dev/posts/local-llm-getting-started/
- https://github.com/jasonacox/TinyLLM