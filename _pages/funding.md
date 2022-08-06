---
layout: archive
title: "Sponsors"
permalink: /funding/
author_profile: true
---

## Acknowledgments and Disclaimer
Our research has been made possible by funds from the CEDC, ORS, EUReCA at University of Colorado Denver, US Department of Education, Google, and the National Science Foundation.

Any opinions, findings, and conclusions or recommendations expressed in our research results are those of the author(s) and do not necessarily reflect the views of the funding agencies.

## Funding Activities 
{% for post in site.funding reversed %}
  {% include archive-single-funding.html %}
{% endfor %}
