---
title: Good JavaScript practices
description: as defined by the community
---

Feel free to join the SpeakJS [discord server](<https://discord.gg/DMcFnN6>) to discuss the matter.

# Preface
This is a collection updated to fit the latest spec and practices in the community.  
Relevant ESLint rules are linked within each section.  
If you use a proper ESLint configuration file, you should already be notified for a lot of them.

{% assign sorted_practices = site.good_practices | sort: "position" %}
{% for practice in sorted_practices %}
- [{{ practice.title }}](#{{ practice.title | slugify: "ascii" }})
{% endfor %}

{% for practice in sorted_practices %}
# {{ practice.title }}

{{ practice.content }}

{% endfor %}
