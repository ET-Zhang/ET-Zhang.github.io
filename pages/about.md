---
layout: page
title: About
description: 有梦想，也有面包。知道自己是谁，要去何方。我希望是生活着，而非只是生存。
keywords: ET-Zhang
comments: true
menu: 关于
permalink: /about/
---

有梦想，也有面包。知道自己是谁，要去何方。我希望是生活着，而非只是生存。

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
