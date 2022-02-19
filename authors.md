---
layout: page
title: 수율
description: '안녕하세요. 수율입니다.'
permalink: /authors/
sitemap:
  priority: 0.7
---
{% for author in site.authors %}
* [{{ author.name }}]({{ site.baseurl }}/authors/{{ author.name }})
{% endfor %}
