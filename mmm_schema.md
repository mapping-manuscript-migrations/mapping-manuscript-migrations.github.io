---
layout: default
title: MMM Data Model schema
description: July 7 2017 conversion
permalink: '/schema/'
---

<ul>
{% for mapping in site.data.mmm_schema %}
  <li>

      {{ mapping.class_or_property }} {{mapping.cardinality}} {{mapping.range}} {{mapping.note}} {{mapping.user_interface_tooltip_text}} {{mapping.number_of_instances}}

  </li>
{% endfor %}
</ul>
