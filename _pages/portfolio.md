---
title: Portfolio
layout: single
permalink: /portfolio/
collection: portfolio
excerpt: "Portfolio de projetos que desenvolvi ou participei"
header:
  overlay_image: /assets/images/dna.png
  overlay_filter: 0.60 # same as adding an opacity of 0.5 to a black background
---

Abaixo estão meus principais projetos e uma breve descrição. Clicando nos links
você será redirecionado para mais detalhes de cada um.

{% assign ordered_pages = site.portfolio | sort: "position" %}
{% for project in ordered_pages %}
  <h2 style="clear:both ">
    <a href="{{ project.url }}">
      {{ project.name }}
    </a>
  </h2>
  
  <p style="padding:0; margin:0; color:gray; font-style:italic">
    {{ project.period }}
  </p>

  ![image-left]({{ project.image }}){: .align-left} {{ project.description }}

  <div class="full" style="clear:both">
  </div>

{% endfor %}
