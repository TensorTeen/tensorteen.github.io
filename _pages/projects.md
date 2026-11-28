---
layout: page
title: Projects
permalink: /projects/
description: Research and experiments in robotics, engineering simulation, and applied AI.
nav: true
nav_order: 3
_styles: |
  .project-index {
    --project-line: var(--global-divider-color);
    --project-surface: var(--global-card-bg-color);
  }

  .project-index a {
    color: var(--global-text-color);
  }

  .project-index a:hover {
    color: var(--global-theme-color);
    text-decoration: none;
  }

  .project-index a:focus-visible {
    outline: 2px solid var(--global-theme-color);
    outline-offset: 4px;
  }

  .project-index .project-feature {
    display: grid;
    grid-template-columns: 1fr;
    overflow: hidden;
    border: 1px solid var(--project-line);
    border-radius: 0.8rem;
    background: var(--project-surface);
  }

  .project-index .project-feature img {
    display: block;
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
    background: var(--global-code-bg-color);
  }

  .project-index .project-feature-copy {
    align-self: center;
    padding: clamp(1.25rem, 3vw, 2rem);
  }

  .project-index .project-feature h2 {
    margin: 0 0 0.5rem;
    font-size: clamp(1.7rem, 4vw, 2.25rem);
    font-weight: 600;
  }

  .project-index .project-feature p,
  .project-index .project-item p {
    margin: 0;
    color: var(--global-text-color);
    line-height: 1.5;
  }

  .project-index .project-feature-cta {
    display: inline-block;
    margin-top: 1rem;
    color: var(--global-theme-color);
    font-weight: 600;
  }

  .project-index section {
    margin-top: 2.5rem;
  }

  .project-index .project-section-title {
    margin: 0 0 0.9rem;
    font-size: 1.45rem;
    font-weight: 600;
  }

  .project-index .project-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 0.8rem;
    padding: 0;
    margin: 0;
    list-style: none;
  }

  .project-index .project-item {
    display: flex;
    height: 100%;
    gap: 0.9rem;
    align-items: flex-start;
    padding: 0.9rem;
    border: 1px solid var(--project-line);
    border-radius: 0.6rem;
    background: var(--project-surface);
  }

  .project-index :is(.project-feature, .project-item):hover {
    border-color: var(--global-theme-color);
  }

  .project-index .project-item img {
    display: block;
    flex: 0 0 auto;
    width: clamp(4.5rem, 14vw, 6rem);
    aspect-ratio: 1;
    padding: 0.2rem;
    border-radius: 0.35rem;
    object-fit: contain;
    background: var(--global-code-bg-color);
  }

  .project-index .project-item h3 {
    margin: 0 0 0.3rem;
    font-size: 1.05rem;
    font-weight: 600;
    line-height: 1.25;
  }

  .project-index .project-item p {
    font-size: 0.92rem;
  }

  @media (min-width: 720px) {
    .project-index .project-feature {
      grid-template-columns: 1.2fr 1fr;
    }

    .project-index .project-feature img {
      height: 100%;
    }

    .project-index .project-grid {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }
  }
---

{% assign featured = site.projects | where: 'featured', true | first %}
{% assign category_ids = 'work,fun' | split: ',' %}
{% assign category_titles = 'Research projects,Other projects' | split: ',' %}

<div class="project-index">
  {% if featured %}
    <a class="project-feature" href="{{ featured.url | relative_url }}">
      <img src="{{ featured.img | escape }}" alt="" width="640" height="360" fetchpriority="high">
      <div class="project-feature-copy">
        <h2>{{ featured.title | escape }}</h2>
        <p>{{ featured.description | escape }}</p>
        <span class="project-feature-cta">View project <span aria-hidden="true">→</span></span>
      </div>
    </a>
  {% endif %}

{% for category in category_ids %}
    {% assign projects = site.projects | where: 'category', category | sort: 'importance' %}
    <section id="{{ category }}" aria-labelledby="projects-{{ category }}-title">
      <h2 class="project-section-title" id="projects-{{ category }}-title">{{ category_titles[forloop.index0] }}</h2>
      <ul class="project-grid">
        {% for project in projects %}
          {% if project.featured %}
            {% continue %}
          {% endif %}
          {% if project.redirect %}
            {% assign project_href = project.redirect %}
          {% else %}
            {% assign project_href = project.url | relative_url %}
          {% endif %}
          <li>
            <a class="project-item" href="{{ project_href | escape }}">
              {% if project.img %}
                <img src="{{ project.img | relative_url | escape }}" alt="" width="96" height="96" loading="lazy" decoding="async">
              {% endif %}
              <div>
                <h3>{{ project.title | escape }}</h3>
                <p>{{ project.description | escape }}</p>
              </div>
            </a>
          </li>
        {% endfor %}
      </ul>
    </section>
{% endfor %}
</div>
