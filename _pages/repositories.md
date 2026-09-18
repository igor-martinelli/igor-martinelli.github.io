---
layout: page
permalink: /repositories/
title: Repositories
description: Open-source repositories and codebase archives for academic and personal projects.
nav: true
nav_order: 3
---

{% if site.data.repositories.github_repos %}

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>

{% endif %}