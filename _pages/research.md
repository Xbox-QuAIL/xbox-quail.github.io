---
title: "QuAIL - Research"
layout: gridlay
excerpt: "QuAIL -- Research"
sitemap: false
permalink: /research/
---

# Research Blog

---

{% assign number_printed = 0 %}
{% for blog in site.data.blogs %}

{% assign even_odd = number_printed | modulo: 2 %}
{% if blog.highlight == 1 %}

{% if even_odd == 0 %}
<div class="row">
{% endif %}

<div class="col-sm-6 clearfix">
 <div class="row">
 	<img src="{{ site.url }}{{ site.baseurl }}/images/respic/{{ blog.image }}" class="img-responsive" width="60%" style="float: right" />
  <p><a class="pub1" href="{{ blog.link.url }}">{{ blog.title }}</a></p>
  <p> {{ blog.description }} </p>
 </div>
</div>

{% assign number_printed = number_printed | plus: 1 %}

{% if even_odd == 1 %}
</div>
{% endif %}

{% endif %}
{% endfor %}

{% assign even_odd = number_printed | modulo: 2 %}
{% if even_odd == 1 %}
</div>
{% endif %}

<p> &nbsp; </p>



