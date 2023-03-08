---
title: "QuAIL - Research"
layout: textlay
excerpt: "QuAIL -- Research"
sitemap: false
permalink: /research/
---

# Research Blog

---

<!-- ![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/banner.png){: style="width: 300px; float: left;margin-right: 30px; border: 10px"}

## Open-world game exploration and object inspection using AI agents
<div style="text-align: justify">
We designed and evlauated AI agents that can effectively explore and inspect open-world games following human preference in exploration and bug testing while only using raw RGB pixel input.
</div>
<h4><a href="ai_agents_shooter22" class="off">Read More</a></h4>
----  -->

{% assign number_printed = 0 %}
{% for blog in site.data.blogs %}

{% assign even_odd = number_printed | modulo: 2 %}
{% if blog.highlight == 1 %}

{% if even_odd == 0 %}
<div class="row">
{% endif %}

<div class="col-sm-6 clearfix">
 <div class="row">
 	<img src="{{ site.url }}{{ site.baseurl }}/images/respic/{{ publi.image }}" class="img-responsive" width="30%" style="float: right" />
  <p><a class="pub1" href="{{ blog.link.url }}">{{ blog.title }}</a></p>
  <a class="pub2"> {{ blog.link.display }} </a>
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



