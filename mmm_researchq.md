---
layout: default
title: MMM Research Questions
description: April 27, 2020
permalink: '/researchq/'
---

<ul>
{% for mapping in site.data.researchq %}
  <li>

      {{ mapping.OriginalMMMResearchQuestion }} {{mapping.RevisedMMMquestion}} {{mapping.Questionfunction}} {{mapping.note}} {{mapping.user_interface_tooltip_text}} {{mapping.number_of_instances}}

  </li>
{% endfor %}
</ul>
