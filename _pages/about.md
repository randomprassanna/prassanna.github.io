---
layout: single
title: About Me
permalink: /
---

<div class="hero-section">
  <h1>Prassanna</h1>
  <p><strong>AI Researcher | Biomedical Vision & Multimodal Learning</strong></p>
  <p>A researcher focused on medical imaging (endoscopy, radiology, pathology), multimodal learning, and emerging interest in protein-related AI.</p>
  <a href="/portfolio/" class="btn btn-primary">Projects</a>
  <a href="/blog/" class="btn btn-secondary">Blog</a>
  <a href="mailto:your.email@example.com" class="btn btn-info">Contact</a>
</div>

<div class="section research-interests">
  <h2>Research Interests</h2>
  <ul>
    <li>Medical Image Understanding (Endoscopy, Radiology, Pathology)</li>
    <li>Multimodal Learning (Vision + Text + Biological Data)</li>
    <li>Representation Learning in Biomedical Data</li>
    <li>Protein / Structural Biology (emerging)</li>
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
  <a href="/publications/" class="btn btn-link">View All Publications</a>
</div>

<div class="section recent-posts">
  <h2>Recent Blog Posts</h2>
  <ul>
    {% for post in site.posts limit:5 %}
      <li><a href="{{ post.url }}">{{ post.title }}</a> <span class="post-date">({{ post.date | date: "%B %d, %Y" }})</span></li>
    {% endfor %}
    {% if site.posts.size == 0 %}
      <li><em>No blog posts found.</em></li>
    {% endif %}
  </ul>
  <a href="/blog/" class="btn btn-link">View All Posts</a>
</div>

<style>
  .experience-chart {
    margin-top: 20px;
  }
  .experience-chart .chart-row {
    margin-bottom: 18px;
    display: flex;
    align-items: center;
  }
  .experience-chart .chart-label {
    width: 180px;
    font-weight: 600;
    color: #2c3e50;
    flex-shrink: 0;
  }
  .experience-chart .chart-bar-wrap {
    flex-grow: 1;
    background: #ecf0f1;
    border-radius: 8px;
    height: 32px;
    overflow: hidden;
    position: relative;
  }
  .experience-chart .chart-bar {
    height: 100%;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding-right: 12px;
    color: #fff;
    font-weight: bold;
    font-size: 0.95em;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  .bar-endoscopy { background: linear-gradient(90deg, #3498db, #2980b9); width: 100%; }
  .bar-multimodal { background: linear-gradient(90deg, #9b59b6, #8e44ad); width: 66.66%; }
  .bar-radiology { background: linear-gradient(90deg, #2ecc71, #27ae60); width: 33.33%; }
  .bar-pathology { background: linear-gradient(90deg, #e67e22, #d35400); width: 33.33%; }
  .bar-protein { background: linear-gradient(90deg, #1abc9c, #16a085); width: 16.66%; }
  .post-date {
    color: #7f8c8d;
    font-size: 0.9em;
    margin-left: 8px;
  }
</style>

<div class="section experience-chart">
  <h2>Clinical AI Experience</h2>
  <p>Years of hands-on research across medical imaging domains:</p>
  
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

<div class="section current-exploration">
  <h2>Current Exploration</h2>
  <ul>
    <li>Vision-language models for medical imaging</li>
    <li>Learning from limited clinical datasets</li>
    <li>Cross-modal learning between imaging and biological data</li>
  </ul>
</div>

<div class="section projects">
  <h2>Projects</h2>
  <div class="project">
    <h3>Title: Project 1</h3>
    <p>Description: A brief description of the project.</p>
    <a href="#" class="btn btn-link">View Project</a>
  </div>
  <div class="project">
    <h3>Title: Project 2</h3>
    <p>Description: Another brief description of the project.</p>
    <a href="#" class="btn btn-link">View Project</a>
  </div>
  <div class="project">
    <h3>Title: Project 3</h3>
    <p>Description: A third brief description of the project.</p>
    <a href="#" class="btn btn-link">View Project</a>
  </div>
</div>

<div class="section blog-writing">
  <h2>Blog / Writing</h2>
  <ul>
    <li><a href="/blog/post1/">Post Title 1</a></li>
    <li><a href="/blog/post2/">Post Title 2</a></li>
    <li><a href="/blog/post3/">Post Title 3</a></li>
  </ul>
  <a href="/blog/" class="btn btn-link">View All Posts</a>
</div>

<div class="section journey">
  <h2>Journey</h2>
  <p>Transitioned into AI research with a focus on biomedical vision and multimodal learning. Current work involves developing advanced models for medical imaging and exploring new applications in protein-related AI.</p>
</div>

<div class="section contact">
  <h2>Contact</h2>
  <ul>
    <li><a href="https://github.com/yourusername">GitHub</a></li>
    <li><a href="mailto:your.email@example.com">Email</a></li>
    <li><a href="https://linkedin.com/in/yourprofile">LinkedIn</a></li>
  </ul>
</div>
