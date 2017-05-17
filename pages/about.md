---
layout: page
title: About
description: 只有坚持的人活了下去
keywords: ET-Zhang
comments: true
menu: 关于
permalink: /about/
---

只有坚持的人活了下去

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}