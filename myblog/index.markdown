---
layout: home
title: "Mayra Madejón Vila"
description: "Explore challenges and practices related to mobile robotics."
---

<div class="homepage-container">
    <div class="hero-section">
        <!-- Favicon image as a featured image -->
        <img src="{{ '/assets/images/favicon-192x192.png' | relative_url }}" alt="Mobile Robotics Logo" class="hero-image">
        <h1>{{ site.title }}</h1>
        <p>{{ site.description }}</p>
        <a href="#posts-section" class="button-primary">See Latest Posts</a>
    </div>

    <section id="posts-section">
        <h2>Recent Posts</h2>
        <ul class="posts-list">
            {% for post in site.posts %}
            <li class="post-item">
                <a href="{{ post.url }}">
                    <h3>{{ post.title }}</h3>
                    <p>{{ post.excerpt }}</p>
                    <span>{{ post.date | date: "%B %d, %Y" }}</span>
                </a>
            </li>
            {% endfor %}
        </ul>
    </section>
</div>

