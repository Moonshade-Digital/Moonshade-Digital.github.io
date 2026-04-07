<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Moonshade Digital</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;1,300&family=Syne:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

  :root {
    --ink: #0a0a12;
    --paper: #f5f2ec;
    --moon: #c8bfa8;
    --silver: #9ca3b0;
    --glow: #7b9cbf;
    --accent: #b8a882;
    --deep: #1a1a2e;
    --mid: #2d2d4a;
  }

  html { scroll-behavior: smooth; }

  body {
    background: var(--ink);
    color: var(--paper);
    font-family: 'Syne', sans-serif;
    cursor: none;
    overflow-x: hidden;
  }

  /* ── Custom Cursor ── */
  #cursor-dot {
    position: fixed;
    width: 8px; height: 8px;
    background: var(--paper);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.08s ease, background 0.2s ease, width 0.2s ease, height 0.2s ease;
  }

  #cursor-ring {
    position: fixed;
    width: 36px; height: 36px;
    border: 1px solid rgba(200, 191, 168, 0.4);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%, -50%);
    transition: width 0.3s ease, height 0.3s ease, border-color 0.3s ease, transform 0.12s ease;
  }

  body.cursor-hover #cursor-dot {
    width: 14px; height: 14px;
    background: var(--glow);
  }

  body.cursor-hover #cursor-ring {
    width: 56px; height: 56px;
    border-color: rgba(123, 156, 191, 0.6);
  }

  /* ── Star Canvas ── */
  #star-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    pointer-events: none;
    opacity: 0.5;
  }

  /* ── Navigation ── */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem 4rem;
    background: linear-gradient(to bottom, rgba(10,10,18,0.95), transparent);
  }

  .nav-logo {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.4rem;
    font-weight: 300;
    letter-spacing: 0.12em;
    color: var(--moon);
    text-decoration: none;
  }

  .nav-logo span { font-style: italic; color: var(--paper); }

  .nav-links { display: flex; gap: 2.5rem; list-style: none; }

  .nav-links a {
    color: var(--silver);
    text-decoration: none;
    font-size: 0.78rem;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    position: relative;
    transition: color 0.3s ease;
  }

  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 0;
    width: 0; height: 1px;
    background: var(--glow);
    transition: width 0.3s ease;
  }

  .nav-links a:hover { color: var(--paper); }
  .nav-links a:hover::after { width: 100%; }

  /* ── Hero ── */
  #hero {
    position: relative;
    z-index: 1;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    padding: 0 4rem;
    overflow: hidden;
  }

  .hero-eyebrow {
    font-size: 0.72rem;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    color: var(--glow);
    margin-bottom: 1.5rem;
    opacity: 0;
    transform: translateY(16px);
    animation: fadeUp 0.8s 0.4s ease forwards;
  }

  .hero-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(4rem, 9vw, 8rem);
    font-weight: 300;
    line-height: 0.95;
    letter-spacing: -0.01em;
    margin-bottom: 2.5rem;
    opacity: 0;
    transform: translateY(20px);
    animation: fadeUp 0.9s 0.6s ease forwards;
  }

  .hero-title em {
    font-style: italic;
    color: var(--moon);
  }

  .hero-subtitle {
    font-size: 1rem;
    line-height: 1.8;
    color: var(--silver);
    max-width: 480px;
    margin-bottom: 3rem;
    opacity: 0;
    transform: translateY(16px);
    animation: fadeUp 0.8s 0.8s ease forwards;
  }

  .hero-cta {
    display: inline-flex;
    align-items: center;
    gap: 1rem;
    padding: 1rem 2.2rem;
    border: 1px solid rgba(200, 191, 168, 0.3);
    color: var(--paper);
    text-decoration: none;
    font-size: 0.78rem;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s ease, color 0.3s ease;
    opacity: 0;
    animation: fadeUp 0.8s 1s ease forwards;
  }

  .hero-cta::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--glow);
    transform: translateX(-100%);
    transition: transform 0.4s ease;
    z-index: -1;
  }

  .hero-cta:hover { border-color: var(--glow); }
  .hero-cta:hover::before { transform: translateX(0); }
  .hero-cta:hover .cta-arrow { transform: translateX(4px); }

  .cta-arrow { transition: transform 0.3s ease; }

  .hero-moon {
    position: absolute;
    right: -80px;
    top: 50%;
    transform: translateY(-50%);
    width: 480px; height: 480px;
    border-radius: 50%;
    background: radial-gradient(circle at 38% 40%, rgba(200,191,168,0.12) 0%, rgba(123,156,191,0.04) 60%, transparent 80%);
    border: 1px solid rgba(200,191,168,0.08);
    opacity: 0;
    animation: moonRise 2s 0.2s ease forwards;
  }

  .hero-moon::before {
    content: '';
    position: absolute;
    inset: 20px;
    border-radius: 50%;
    border: 1px solid rgba(200,191,168,0.06);
  }

  /* ── Section base ── */
  section {
    position: relative;
    z-index: 1;
  }

  .section-label {
    font-size: 0.68rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--glow);
    margin-bottom: 1rem;
  }

  .section-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: clamp(2.2rem, 4vw, 3.5rem);
    font-weight: 300;
    line-height: 1.1;
    color: var(--paper);
    margin-bottom: 1.5rem;
  }

  .section-title em { font-style: italic; color: var(--moon); }

  /* ── Work Section ── */
  #work {
    padding: 8rem 4rem;
  }

  .work-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    margin-bottom: 4rem;
  }

  .work-header p {
    color: var(--silver);
    font-size: 0.9rem;
    line-height: 1.8;
    max-width: 340px;
    text-align: right;
  }

  .project-grid {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    gap: 2px;
  }

  .project-card {
    position: relative;
    overflow: hidden;
    background: var(--mid);
  }

  .project-card:nth-child(1) { grid-column: 1 / 8; grid-row: 1; }
  .project-card:nth-child(2) { grid-column: 8 / 13; grid-row: 1; }
  .project-card:nth-child(3) { grid-column: 1 / 5; grid-row: 2; }
  .project-card:nth-child(4) { grid-column: 5 / 13; grid-row: 2; }

  .card-inner {
    aspect-ratio: 4/3;
    background: var(--deep);
    position: relative;
    overflow: hidden;
    display: flex;
    align-items: flex-end;
  }

  .card-visual {
    position: absolute;
    inset: 0;
    transition: transform 0.7s ease;
  }

  .project-card:hover .card-visual { transform: scale(1.04); }

  .v1 { background: radial-gradient(ellipse at 70% 30%, rgba(123,156,191,0.3) 0%, rgba(10,10,18,0) 65%), linear-gradient(135deg, #1a1a2e 0%, #0f0f1e 100%); }
  .v2 { background: radial-gradient(ellipse at 30% 70%, rgba(200,191,168,0.2) 0%, rgba(10,10,18,0) 65%), linear-gradient(225deg, #1e1a2e 0%, #0f0f1e 100%); }
  .v3 { background: radial-gradient(ellipse at 60% 40%, rgba(184,168,130,0.25) 0%, rgba(10,10,18,0) 65%), linear-gradient(160deg, #1a1e2e 0%, #0f0f1e 100%); }
  .v4 { background: radial-gradient(ellipse at 40% 60%, rgba(156,163,176,0.2) 0%, rgba(10,10,18,0) 65%), linear-gradient(200deg, #12151e 0%, #0a0a12 100%); }

  .card-glyph {
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    font-family: 'Cormorant Garamond', serif;
    font-size: 6rem;
    font-style: italic;
    color: rgba(200,191,168,0.08);
    letter-spacing: -0.02em;
    pointer-events: none;
    transition: color 0.4s ease, transform 0.4s ease;
  }

  .project-card:hover .card-glyph {
    color: rgba(200,191,168,0.15);
    transform: translate(-50%, -50%) scale(1.08);
  }

  .card-info {
    position: relative;
    z-index: 2;
    padding: 1.5rem;
    width: 100%;
    background: linear-gradient(to top, rgba(10,10,18,0.95) 0%, transparent 100%);
    transform: translateY(8px);
    transition: transform 0.4s ease;
  }

  .project-card:hover .card-info { transform: translateY(0); }

  .card-cat {
    font-size: 0.65rem;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--glow);
    margin-bottom: 0.3rem;
  }

  .card-title {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.4rem;
    font-weight: 300;
    color: var(--paper);
  }

  .card-arrow {
    position: absolute;
    top: 1.2rem; right: 1.2rem;
    width: 36px; height: 36px;
    border: 1px solid rgba(200,191,168,0.2);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    color: var(--moon);
    font-size: 0.9rem;
    opacity: 0;
    transform: scale(0.8);
    transition: opacity 0.3s ease, transform 0.3s ease, border-color 0.3s ease;
  }

  .project-card:hover .card-arrow {
    opacity: 1;
    transform: scale(1);
    border-color: rgba(123,156,191,0.5);
    color: var(--glow);
  }

  /* ── Services ── */
  #services {
    padding: 8rem 4rem;
    border-top: 1px solid rgba(200,191,168,0.06);
  }

  .services-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 1px;
    margin-top: 5rem;
    border: 1px solid rgba(200,191,168,0.06);
  }

  .service-item {
    padding: 3rem 2.5rem;
    border-right: 1px solid rgba(200,191,168,0.06);
    position: relative;
    overflow: hidden;
    transition: background 0.4s ease;
  }

  .service-item:last-child { border-right: none; }

  .service-item::before {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 2px;
    background: var(--glow);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.5s ease;
  }

  .service-item:hover { background: rgba(123,156,191,0.04); }
  .service-item:hover::before { transform: scaleX(1); }

  .service-num {
    font-family: 'Cormorant Garamond', serif;
    font-size: 3.5rem;
    font-weight: 300;
    color: rgba(200,191,168,0.08);
    line-height: 1;
    margin-bottom: 1.5rem;
    transition: color 0.4s ease;
  }

  .service-item:hover .service-num { color: rgba(123,156,191,0.2); }

  .service-name {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.6rem;
    font-weight: 300;
    color: var(--paper);
    margin-bottom: 1rem;
    line-height: 1.2;
  }

  .service-desc {
    font-size: 0.85rem;
    line-height: 1.9;
    color: var(--silver);
  }

  /* ── About ── */
  #about {
    padding: 8rem 4rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8rem;
    align-items: center;
    border-top: 1px solid rgba(200,191,168,0.06);
  }

  .about-visual {
    position: relative;
    height: 500px;
  }

  .about-orb {
    position: absolute;
    border-radius: 50%;
  }

  .orb-1 {
    width: 320px; height: 320px;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    border: 1px solid rgba(200,191,168,0.1);
    animation: orbitSpin 20s linear infinite;
  }

  .orb-2 {
    width: 200px; height: 200px;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    background: radial-gradient(circle, rgba(123,156,191,0.12) 0%, transparent 70%);
    border: 1px solid rgba(123,156,191,0.15);
    animation: orbitSpin 14s linear infinite reverse;
  }

  .orb-3 {
    width: 80px; height: 80px;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    background: radial-gradient(circle, rgba(200,191,168,0.25) 0%, transparent 70%);
  }

  .orb-dot {
    position: absolute;
    width: 6px; height: 6px;
    background: var(--glow);
    border-radius: 50%;
    top: -3px; left: 50%;
    margin-left: -3px;
    box-shadow: 0 0 10px var(--glow);
  }

  .about-text p {
    font-size: 0.9rem;
    line-height: 2;
    color: var(--silver);
    margin-bottom: 1.5rem;
  }

  .about-text p strong {
    color: var(--moon);
    font-weight: 400;
  }

  .stats-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
    margin-top: 3rem;
    padding-top: 2.5rem;
    border-top: 1px solid rgba(200,191,168,0.1);
  }

  .stat-num {
    font-family: 'Cormorant Garamond', serif;
    font-size: 2.8rem;
    font-weight: 300;
    color: var(--paper);
    line-height: 1;
  }

  .stat-label {
    font-size: 0.7rem;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--silver);
    margin-top: 0.4rem;
  }

  /* ── Contact ── */
  #contact {
    padding: 10rem 4rem;
    text-align: center;
    border-top: 1px solid rgba(200,191,168,0.06);
  }

  #contact .section-title { font-size: clamp(3rem, 6vw, 5.5rem); margin-bottom: 2rem; }

  #contact p {
    color: var(--silver);
    font-size: 0.95rem;
    line-height: 1.8;
    max-width: 440px;
    margin: 0 auto 3rem;
  }

  .contact-link {
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.4rem;
    font-style: italic;
    color: var(--moon);
    text-decoration: none;
    position: relative;
    transition: color 0.3s ease;
  }

  .contact-link::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 0; right: 0;
    height: 1px;
    background: var(--glow);
    transform: scaleX(0.4);
    transform-origin: left;
    transition: transform 0.4s ease;
  }

  .contact-link:hover { color: var(--paper); }
  .contact-link:hover::after { transform: scaleX(1); }

  /* ── Footer ── */
  footer {
    position: relative;
    z-index: 1;
    padding: 2rem 4rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-top: 1px solid rgba(200,191,168,0.06);
  }

  footer p {
    font-size: 0.7rem;
    letter-spacing: 0.1em;
    color: rgba(156,163,176,0.4);
  }

  .footer-links {
    display: flex;
    gap: 2rem;
    list-style: none;
  }

  .footer-links a {
    font-size: 0.7rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: rgba(156,163,176,0.4);
    text-decoration: none;
    transition: color 0.3s ease;
  }

  .footer-links a:hover { color: var(--silver); }

  /* ── Scroll reveal ── */
  .reveal {
    opacity: 0;
    transform: translateY(28px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }

  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* ── Animations ── */
  @keyframes fadeUp {
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes moonRise {
    to { opacity: 1; }
  }

  @keyframes orbitSpin {
    to { transform: translate(-50%, -50%) rotate(360deg); }
  }

  /* ── Responsive ── */
  @media (max-width: 900px) {
    nav { padding: 1.5rem 2rem; }
    .nav-links { display: none; }
    #hero { padding: 0 2rem; }
    #work, #services, #contact { padding: 5rem 2rem; }
    #about { grid-template-columns: 1fr; padding: 5rem 2rem; gap: 4rem; }
    .project-card:nth-child(1), .project-card:nth-child(2),
    .project-card:nth-child(3), .project-card:nth-child(4) {
      grid-column: 1 / -1; grid-row: auto;
    }
    .services-grid { grid-template-columns: 1fr; }
    .service-item { border-right: none; border-bottom: 1px solid rgba(200,191,168,0.06); }
    footer { flex-direction: column; gap: 1rem; text-align: center; }
  }
  .p {
    display: none !important;
}
</style>
</head>
<body>

<!-- Cursor -->
<div id="cursor-dot"></div>
<div id="cursor-ring"></div>

<!-- Star field -->
<canvas id="star-canvas"></canvas>

<!-- Navigation -->
<nav>
  <a href="#" class="nav-logo">Moon<span>shade</span> Digital</a>
  <ul class="nav-links">
    <li><a href="#work">Work</a></li>
    <li><a href="#services">Services</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
</nav>

<!-- Hero -->
<section id="hero">
  <p class="hero-eyebrow">Digital Media Studio — Est. 2024</p>
  <h1 class="hero-title">We craft<br><em>stories</em><br>in light.</h1>
  <p class="hero-subtitle">Moonshade Digital is a boutique creative studio shaping brands, campaigns, and digital experiences that resonate in the modern era.</p>
  <a href="#work" class="hero-cta">
    View Our Work
    <span class="cta-arrow">→</span>
  </a>
  <div class="hero-moon"></div>
</section>

<!-- Work -->
<section id="work">
  <div class="work-header reveal">
    <div>
      <p class="section-label">Selected Work</p>
      <h2 class="section-title">Projects that<br><em>define</em> brands.</h2>
    </div>
    <p>A curated selection of campaigns, identities, and digital experiences we've brought to life for our clients.</p>
  </div>

  <div class="project-grid reveal">
    <div class="project-card">
      <div class="card-inner">
        <div class="card-visual v1"></div>
        <div class="card-glyph">Æ</div>
        <div class="card-info">
          <p class="card-cat">Brand Identity</p>
          <p class="card-title">Aether Collective</p>
        </div>
        <div class="card-arrow">↗</div>
      </div>
    </div>
    <div class="project-card">
      <div class="card-inner">
        <div class="card-visual v2"></div>
        <div class="card-glyph">Lx</div>
        <div class="card-info">
          <p class="card-cat">Web Experience</p>
          <p class="card-title">Lumex Studios</p>
        </div>
        <div class="card-arrow">↗</div>
      </div>
    </div>
    <div class="project-card">
      <div class="card-inner">
        <div class="card-visual v3"></div>
        <div class="card-glyph">Nr</div>
        <div class="card-info">
          <p class="card-cat">Campaign</p>
          <p class="card-title">Noura Atelier</p>
        </div>
        <div class="card-arrow">↗</div>
      </div>
    </div>
    <div class="project-card">
      <div class="card-inner">
        <div class="card-visual v4"></div>
        <div class="card-glyph">Vs</div>
        <div class="card-info">
          <p class="card-cat">Motion + Identity</p>
          <p class="card-title">Voss & Serra</p>
        </div>
        <div class="card-arrow">↗</div>
      </div>
    </div>
  </div>
</section>

<!-- Services -->
<section id="services">
  <div class="reveal">
    <p class="section-label">What We Do</p>
    <h2 class="section-title">Capabilities built for<br><em>the digital age.</em></h2>
  </div>
  <div class="services-grid reveal">
    <div class="service-item">
      <p class="service-num">01</p>
      <p class="service-name">Brand Identity & Strategy</p>
      <p class="service-desc">From naming through visual systems, we create brand identities that hold weight in saturated markets.</p>
    </div>
    <div class="service-item">
      <p class="service-num">02</p>
      <p class="service-name">Digital Experience Design</p>
      <p class="service-desc">Websites and interactive platforms that balance aesthetic ambition with usability and performance.</p>
    </div>
    <div class="service-item">
      <p class="service-num">03</p>
      <p class="service-name">Motion & Content</p>
      <p class="service-desc">Campaign films, social content, and motion graphics engineered for emotional impact and reach.</p>
    </div>
  </div>
</section>

<!-- About -->
<section id="about">
  <div class="about-visual reveal">
    <div class="about-orb orb-1">
      <div class="orb-dot"></div>
    </div>
    <div class="about-orb orb-2"></div>
    <div class="about-orb orb-3"></div>
  </div>
  <div class="about-text reveal">
    <p class="section-label">About the Studio</p>
    <h2 class="section-title">Made in the<br><em>shadow of the moon.</em></h2>
    <p>Moonshade Digital was founded on the belief that the most enduring creative work lives in <strong>the space between light and shadow</strong> — where ideas become atmosphere and brands become feeling.</p>
    <p>Our team of strategists, designers, and storytellers works collaboratively with clients who value craft, intention, and long-term thinking over quick wins.</p>
    <div class="stats-row">
      <div>
        <p class="stat-num">48+</p>
        <p class="stat-label">Projects delivered</p>
      </div>
      <div>
        <p class="stat-num">12</p>
        <p class="stat-label">Brand awards</p>
      </div>
      <div>
        <p class="stat-num">6</p>
        <p class="stat-label">Countries</p>
      </div>
    </div>
  </div>
</section>

<!-- Contact -->
<section id="contact">
  <p class="section-label reveal">Get in Touch</p>
  <h2 class="section-title reveal">Let's make something<br><em>worth remembering.</em></h2>
  <p class="reveal">Whether you have a clear brief or just an early idea, we'd love to hear from you. Most great projects start with a conversation.</p>

  <!-- Email -->

  <a href="mailto:hello@moonshade-digital.github.io" class="contact-link reveal">moonshadedigital@gmail.com</a>
</section>

<!-- Footer -->
<footer>
  <p>© 2026 Moonshade Digital. All rights reserved.</p>
  <ul class="footer-links">
    <li><a href="https://www.instagram.com/moonshadedigital/">Instagram</a></li>
    <li><a href="#">LinkedIn</a></li>
    <li><a href="#">Twitter</a></li>
  </ul>
</footer>

<script>
  // ── Custom Cursor ──
  const dot = document.getElementById('cursor-dot');
  const ring = document.getElementById('cursor-ring');
  let mx = -100, my = -100, rx = -100, ry = -100;

  document.addEventListener('mousemove', e => {
    mx = e.clientX; my = e.clientY;
    dot.style.left = mx + 'px';
    dot.style.top = my + 'px';
  });

  function animateRing() {
    rx += (mx - rx) * 0.12;
    ry += (my - ry) * 0.12;
    ring.style.left = rx + 'px';
    ring.style.top = ry + 'px';
    requestAnimationFrame(animateRing);
  }
  animateRing();

  const hoverEls = document.querySelectorAll('a, button, .project-card, .service-item');
  hoverEls.forEach(el => {
    el.addEventListener('mouseenter', () => document.body.classList.add('cursor-hover'));
    el.addEventListener('mouseleave', () => document.body.classList.remove('cursor-hover'));
  });

  // ── Star field ──
  const canvas = document.getElementById('star-canvas');
  const ctx = canvas.getContext('2d');
  let stars = [];

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }

  function initStars() {
    stars = [];
    const count = Math.floor((canvas.width * canvas.height) / 8000);
    for (let i = 0; i < count; i++) {
      stars.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        r: Math.random() * 1.2 + 0.2,
        a: Math.random(),
        speed: Math.random() * 0.004 + 0.001,
        phase: Math.random() * Math.PI * 2
      });
    }
  }

  let frame = 0;
  function drawStars() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    frame += 0.008;
    stars.forEach(s => {
      const alpha = s.a * (0.5 + 0.5 * Math.sin(frame * s.speed * 60 + s.phase));
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2);
      ctx.fillStyle = `rgba(200,191,168,${alpha})`;
      ctx.fill();
    });
    requestAnimationFrame(drawStars);
  }

  resizeCanvas();
  initStars();
  drawStars();
  window.addEventListener('resize', () => { resizeCanvas(); initStars(); });

  // ── Scroll reveal ──
  const reveals = document.querySelectorAll('.reveal');
  const io = new IntersectionObserver(entries => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        setTimeout(() => entry.target.classList.add('visible'), i * 80);
        io.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });

  reveals.forEach(el => io.observe(el));

  // ── Parallax moon ──
  window.addEventListener('scroll', () => {
    const moon = document.querySelector('.hero-moon');
    if (moon) moon.style.transform = `translateY(calc(-50% + ${window.scrollY * 0.15}px))`;
  });
</script>
</body>
</html>
