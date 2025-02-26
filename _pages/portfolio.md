---
title: Portfolio
layout: single
permalink: /portfolio/
collection: portfolio
excerpt: "Projects I have worked on"
header:
  overlay_image: /assets/images/dna.png
  overlay_filter: 0.60 # same as adding an opacity of 0.5 to a black background
---

Here you can find projects I have worked on with a brief description.
Follow the links to more details about each one.

{% assign ordered_pages = site.portfolio | sort: "position" %}
{% for project in ordered_pages %}
  <h2 style="clear:both ">
    <a href="{{ project.url }}">
      {{ project.name }}
    </a>
  </h2>
  
  <!-- <p style="padding:0; margin:0; color:gray; font-style:italic">
    {{ project.period }}
  </p> -->

  ![image-left]({{ project.image }}){: .align-left width="250"} {{ project.description }}

  <div class="full" style="clear:both">
  </div>

{% endfor %}
