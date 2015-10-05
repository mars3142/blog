---
title: Blog
layout: default
---

{% for post in site.posts %}
  <div class="mdl-card mdl-cell mdl-cell--12-col">
    <div class="mdl-card__media mdl-color-text--grey-50" style="background-image: url('/images/{{ post.header }}')">
      <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>
    </div>
    <div class="mdl-color-text--grey-600 mdl-card__supporting-text">
      {{ post.description }}
    </div>
    <div class="mdl-card__supporting-text meta mdl-color-text--grey-600">
      <div class="minilogo"></div>
      <div>
        <strong>Date created</strong>
        <span>{{ post.date | date_to_string }}</span>
      </div>
    </div>
  </div>
{% endfor %}
