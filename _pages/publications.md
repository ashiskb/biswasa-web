---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if site.author.googlescholar %}
  You can also find my articles on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.
{% endif %}


{% include base_path %}

## Journals
{% for post in site.publications reversed %}
{% include archive-single-journal.html %}
<p></p>
{% endfor %}

## Conferences
{% for post in site.publications reversed %}
{% include archive-single-conference.html %}
<p></p>
{% endfor %}

