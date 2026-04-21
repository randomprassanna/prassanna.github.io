---
layout: single
title: "About"
permalink: /
---

<style>
  .section {
    margin-bottom: 2.5em;
  }
  .section h2 {
    margin-bottom: 0.6em;
    font-size: 1.4em;
    font-weight: 600;
    color: #2c3e50;
    border-bottom: 2px solid #ecf0f1;
    padding-bottom: 0.3em;
  }
  .experience-chart p {
    margin-bottom: 1.2em;
    color: #555;
  }
  .experience-chart .chart-row {
    margin-bottom: 16px;
    display: flex;
    align-items: center;
  }
  .experience-chart .chart-label {
    width: 160px;
    font-weight: 600;
    color: #34495e;
    flex-shrink: 0;
    font-size: 0.95em;
  }
  .experience-chart .chart-bar-wrap {
    flex-grow: 1;
    background: #ecf0f1;
    border-radius: 6px;
    height: 28px;
    overflow: hidden;
  }
  .experience-chart .chart-bar {
    height: 100%;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding-right: 10px;
    color: #fff;
    font-weight: 600;
    font-size: 0.85em;
    letter-spacing: 0.3px;
    box-shadow: inset 0 -2px 4px rgba(0,0,0,0.08);
    transition: width 0.8s ease-in-out;
  }
  .bar-endoscopy { background: linear-gradient(90deg, #2980b9, #3498db); width: 100%; }
  .bar-multimodal { background: linear-gradient(90deg, #8e44ad, #9b59b6); width: 66.66%; }
  .bar-radiology { background: linear-gradient(90deg, #27ae60, #2ecc71); width: 33.33%; }
  .bar-pathology { background: linear-gradient(90deg, #d35400, #e67e22); width: 33.33%; }
  .bar-protein { background: linear-gradient(90deg, #16a085, #1abc9c); width: 16.66%; }
  
  .research-interests ul {
    list-style: none;
    padding-left: 0;
  }
  .research-interests li {
    padding: 0.4em 0;
    color: #444;
    line-height: 1.5;
  }
  .research-interests li strong {
    color: #2c3e50;
  }
  
  .recent-publications ul,
  .recent-posts ul,
  .current-directions ul {
    padding-left: 1.2em;
    margin: 0;
  }
  .recent-publications li,
  .recent-posts li,
  .current-directions li {
    margin-bottom: 0.5em;
    line-height: 1.5;
  }
  .recent-publications a,
  .recent-posts a {
    text-decoration: none;
    color: #2980b9;
    font-weight: 500;
  }
  .recent-publications a:hover,
  .recent-posts a:hover {
    text-decoration: underline;
  }
  .post-date {
    color: #7f8c8d;
    font-size: 0.9em;
    margin-left: 6px;
  }
  .view-all {
    display: inline-block;
    margin-top: 0.6em;
    font-size: 0.9em;
    color: #555;
    text-decoration: none;
    font-weight: 500;
  }
  .view-all:hover {
    color: #2980b9;
    text-decoration: underline;
  }
</style>

<div class="section experience-chart">
  <h2>Clinical AI Experience</h2>
  <p>Hands-on research across medical imaging and multimodal AI domains.</p>
  
  <div class="chart-row">
    <div class="chart-label">Endoscopy</div>
    <div class="chart-bar-wrap">
      <div class="chart-bar bar-endoscopy">3 yrs</div>
    </div>
  </div>
  
  <div class="chart-row">
    <div class="chart-label">Multimodal AI</div>
    <div class="chart-bar-wrap">
      <div class="chart-bar bar-multimodal">2 yrs</div>
    </div>
  </div>
  
  <div class="chart-row">
    <div class="chart-label">Radiology</div>
    <div class="chart-bar-wrap">
      <div class="chart-bar bar-radiology">1 yr</div>
    </div>
  </div>
  
  <div class="chart-row">
    <div class="chart-label">Pathology</div>
    <div class="chart-bar-wrap">
      <div class="chart-bar bar-pathology">1 yr</div>
    </div>
  </div>
  
  <div class="chart-row">
    <div class="chart-label">Protein / BioML</div>
    <div class="chart-bar-wrap">
      <div class="chart-bar bar-protein">&lt;1 yr</div>
    </div>
  </div>
</div>

<div class="section research-interests">
  <h2>Research Interests</h2>
  <ul>
    <li><strong>Medical Image Understanding</strong> — Endoscopy, radiology, and digital pathology</li>
    <li><strong>Multimodal Learning</strong> — Integration of vision, language, and biological data</li>
    <li><strong>Representation Learning</strong> — Self-supervised and transfer learning for clinical data</li>
    <li><strong>Protein & Structural Biology</strong> — Emerging work at the intersection of AI and bioinformatics</li>
  </ul>
</div>

<div class="section recent-publications">
  <h2>Recent Publications</h2>
  <ul>
    {% assign sorted_pubs = site.publications | sort: 'date' | reverse %}
    {% for publication in sorted_pubs limit:5 %}
      <li><a href="{{ publication.url }}">{{ publication.title }}</a></li>
    {% endfor %}
    {% if site.publications.size == 0 %}
      <li><em>No publications found.</em></li>
    {% endif %}
  </ul>
  <a href="/publications/" class="view-all">View All Publications →</a>
</div>

<div class="section recent-posts">
  <h2>Recent Blog Posts</h2>
  <ul>
    {% for post in site.posts limit:5 %}
      <li><a href="{{ post.url }}">{{ post.title }}</a><span class="post-date">({{ post.date | date: "%B %d, %Y" }})</span></li>
    {% endfor %}
    {% if site.posts.size == 0 %}
      <li><em>No blog posts found.</em></li>
    {% endif %}
  </ul>
  <a href="/blog/" class="view-all">View All Posts →</a>
</div>

<div class="section current-directions">
  <h2>Current Research Directions</h2>
  <ul>
    <li>Vision-language models for medical imaging</li>
    <li>Learning from limited clinical datasets</li>
    <li>Cross-modal learning between imaging and biological data</li>
  </ul>
</div>
