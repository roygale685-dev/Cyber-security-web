<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NexShield —  Roy Gale Cyber Security</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@300;400;600;700&family=Exo+2:wght@100;300;400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --cyan: #00f5ff;
    --green: #00ff88;
    --red: #ff2d55;
    --dark: #020b14;
    --darker: #010810;
    --panel: rgba(0,245,255,0.04);
    --border: rgba(0,245,255,0.18);
    --glow: 0 0 20px rgba(0,245,255,0.4);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--dark);
    color: #cce8f0;
    font-family: 'Exo 2', sans-serif;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* ── CANVAS BACKGROUND ── */
  #matrix-canvas {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    opacity: 0.07;
    pointer-events: none;
  }

  /* Scan-line overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1;
  }

  /* ── NAVBAR ── */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 3rem;
    background: rgba(2,11,20,0.85);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid var(--border);
    transition: all 0.4s ease;
  }

  .logo {
    font-family: 'Share Tech Mono', monospace;
    font-size: 1.4rem;
    color: var(--cyan);
    letter-spacing: 3px;
    text-shadow: var(--glow);
    text-decoration: none;
  }

  .logo span { color: var(--green); }

  .nav-links {
    display: flex;
    gap: 2.5rem;
    list-style: none;
  }

  .nav-links a {
    color: #7bb8c8;
    text-decoration: none;
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.9rem;
    font-weight: 600;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: color 0.3s, text-shadow 0.3s;
    position: relative;
  }

  .nav-links a::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 0;
    width: 0; height: 1px;
    background: var(--cyan);
    transition: width 0.3s ease;
    box-shadow: 0 0 8px var(--cyan);
  }

  .nav-links a:hover { color: var(--cyan); text-shadow: var(--glow); }
  .nav-links a:hover::after { width: 100%; }

  .nav-cta {
    background: transparent;
    border: 1px solid var(--cyan);
    color: var(--cyan);
    font-family: 'Rajdhani', sans-serif;
    font-weight: 700;
    letter-spacing: 2px;
    padding: 0.5rem 1.4rem;
    font-size: 0.85rem;
    text-transform: uppercase;
    cursor: crosshair;
    transition: background 0.3s, box-shadow 0.3s;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
  }

  .nav-cta:hover {
    background: rgba(0,245,255,0.12);
    box-shadow: var(--glow), inset 0 0 20px rgba(0,245,255,0.06);
  }

  /* ── SECTIONS ── */
  section { position: relative; z-index: 2; }

  /* ── HERO ── */
  #hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 6rem 2rem 4rem;
    overflow: hidden;
  }

  .hero-inner { max-width: 900px; }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    border: 1px solid var(--green);
    padding: 0.3rem 1rem;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.72rem;
    color: var(--green);
    letter-spacing: 2px;
    margin-bottom: 2rem;
    animation: fadeSlideDown 0.8s ease both;
    clip-path: polygon(6px 0%, 100% 0%, calc(100% - 6px) 100%, 0% 100%);
  }

  .badge-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: pulse 1.5s infinite;
  }

  .hero-title {
    font-family: 'Exo 2', sans-serif;
    font-size: clamp(2.8rem, 6vw, 5.5rem);
    font-weight: 700;
    line-height: 1.05;
    letter-spacing: -1px;
    margin-bottom: 1.5rem;
    animation: fadeSlideUp 0.9s 0.2s ease both;
  }

  .hero-title .line1 { color: #e0f4ff; display: block; }

  .hero-title .line2 {
    display: block;
    color: var(--cyan);
    text-shadow: var(--glow);
    position: relative;
  }

  .hero-title .line2::after {
    content: '';
    position: absolute;
    bottom: -4px; left: 50%; transform: translateX(-50%);
    width: 60%; height: 2px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
    box-shadow: 0 0 12px var(--cyan);
  }

  .hero-sub {
    font-weight: 300;
    font-size: 1.1rem;
    color: #7bb8c8;
    max-width: 600px;
    margin: 2rem auto;
    line-height: 1.8;
    letter-spacing: 0.5px;
    animation: fadeSlideUp 0.9s 0.4s ease both;
  }

  .hero-buttons {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
    margin-top: 2.5rem;
    animation: fadeSlideUp 0.9s 0.6s ease both;
  }

  .btn-primary {
    background: var(--cyan);
    color: var(--darker);
    font-family: 'Rajdhani', sans-serif;
    font-weight: 700;
    font-size: 0.9rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 0.85rem 2.2rem;
    border: none;
    cursor: crosshair;
    clip-path: polygon(10px 0%, 100% 0%, calc(100% - 10px) 100%, 0% 100%);
    transition: box-shadow 0.3s, transform 0.2s;
    text-decoration: none;
    display: inline-block;
  }

  .btn-primary:hover {
    box-shadow: 0 0 30px rgba(0,245,255,0.6);
    transform: translateY(-2px);
  }

  .btn-outline {
    background: transparent;
    color: #cce8f0;
    font-family: 'Rajdhani', sans-serif;
    font-weight: 600;
    font-size: 0.9rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    padding: 0.85rem 2.2rem;
    border: 1px solid rgba(255,255,255,0.2);
    cursor: crosshair;
    transition: border-color 0.3s, color 0.3s;
    text-decoration: none;
    display: inline-block;
    clip-path: polygon(10px 0%, 100% 0%, calc(100% - 10px) 100%, 0% 100%);
  }

  .btn-outline:hover { border-color: var(--cyan); color: var(--cyan); }

  /* Floating hexagons */
  .hex-grid {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
    z-index: -1;
  }

  .hex {
    position: absolute;
    width: 80px; height: 80px;
    border: 1px solid rgba(0,245,255,0.1);
    clip-path: polygon(50% 0%,100% 25%,100% 75%,50% 100%,0% 75%,0% 25%);
    animation: floatHex linear infinite;
  }

  .hex:nth-child(1) { left: 8%; top: 20%; animation-duration: 18s; animation-delay: 0s; opacity: 0.5; }
  .hex:nth-child(2) { left: 85%; top: 15%; animation-duration: 22s; animation-delay: -4s; width: 50px; height: 50px; opacity: 0.3; }
  .hex:nth-child(3) { left: 75%; top: 65%; animation-duration: 20s; animation-delay: -8s; width: 100px; height: 100px; opacity: 0.2; }
  .hex:nth-child(4) { left: 15%; top: 70%; animation-duration: 25s; animation-delay: -12s; width: 60px; height: 60px; opacity: 0.4; }
  .hex:nth-child(5) { left: 50%; top: 10%; animation-duration: 16s; animation-delay: -6s; width: 40px; height: 40px; opacity: 0.3; }

  /* ── STATS BAR ── */
  .stats-bar {
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    background: rgba(0,245,255,0.025);
    padding: 1.5rem 3rem;
    display: flex;
    justify-content: center;
    gap: 4rem;
    flex-wrap: wrap;
  }

  .stat-item {
    text-align: center;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }

  .stat-item.visible { opacity: 1; transform: translateY(0); }

  .stat-num {
    font-family: 'Share Tech Mono', monospace;
    font-size: 2rem;
    color: var(--cyan);
    text-shadow: var(--glow);
    display: block;
  }

  .stat-label {
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.8rem;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: #5a8a99;
  }

  /* ── SERVICES ── */
  #services {
    padding: 6rem 3rem;
    max-width: 1200px;
    margin: 0 auto;
  }

  .section-header {
    text-align: center;
    margin-bottom: 4rem;
  }

  .section-tag {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.72rem;
    color: var(--green);
    letter-spacing: 4px;
    text-transform: uppercase;
    display: block;
    margin-bottom: 1rem;
  }

  .section-title {
    font-family: 'Exo 2', sans-serif;
    font-size: clamp(1.8rem, 3.5vw, 2.8rem);
    font-weight: 700;
    color: #e0f4ff;
    line-height: 1.2;
  }

  .section-title em {
    font-style: normal;
    color: var(--cyan);
    text-shadow: var(--glow);
  }

  .services-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
  }

  .service-card {
    background: var(--panel);
    border: 1px solid var(--border);
    padding: 2rem;
    position: relative;
    overflow: hidden;
    cursor: default;
    transition: border-color 0.4s, transform 0.3s;
    clip-path: polygon(0 0, calc(100% - 20px) 0, 100% 20px, 100% 100%, 20px 100%, 0 calc(100% - 20px));
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s ease, transform 0.6s ease, border-color 0.4s;
  }

  .service-card.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .service-card::before {
    content: '';
    position: absolute;
    top: 0; left: -100%;
    width: 100%; height: 100%;
    background: linear-gradient(90deg, transparent, rgba(0,245,255,0.05), transparent);
    transition: left 0.6s ease;
  }

  .service-card:hover { border-color: var(--cyan); }
  .service-card:hover::before { left: 100%; }

  /* corner accent */
  .service-card::after {
    content: '';
    position: absolute;
    top: 0; right: 0;
    width: 20px; height: 20px;
    border-top: 1px solid var(--cyan);
    border-right: 1px solid var(--cyan);
  }

  .card-icon {
    width: 48px; height: 48px;
    margin-bottom: 1.2rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .card-icon svg { width: 100%; height: 100%; }

  .card-title {
    font-family: 'Rajdhani', sans-serif;
    font-size: 1.25rem;
    font-weight: 700;
    color: #e0f4ff;
    letter-spacing: 1px;
    margin-bottom: 0.8rem;
  }

  .card-desc {
    font-size: 0.88rem;
    color: #5a8a99;
    line-height: 1.75;
  }

  .card-tag {
    display: inline-block;
    margin-top: 1.2rem;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.68rem;
    color: var(--cyan);
    letter-spacing: 1px;
    border-left: 2px solid var(--cyan);
    padding-left: 0.5rem;
  }

  /* ── THREAT MONITOR ── */
  #monitor {
    padding: 6rem 3rem;
    background: rgba(0,0,0,0.3);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }

  .monitor-inner {
    max-width: 1100px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
  }

  @media(max-width:768px){
    .monitor-inner { grid-template-columns: 1fr; }
    nav { padding: 1rem 1.5rem; }
    .nav-links { display: none; }
    .stats-bar { gap: 2rem; padding: 1.5rem 1.5rem; }
    #services, #monitor, #pricing { padding: 4rem 1.5rem; }
  }

  .terminal {
    background: rgba(0,0,0,0.5);
    border: 1px solid var(--border);
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.78rem;
    color: var(--green);
    padding: 1.5rem;
    line-height: 2;
    position: relative;
    clip-path: polygon(0 0, calc(100% - 15px) 0, 100% 15px, 100% 100%, 0 100%);
    min-height: 280px;
  }

  .terminal-bar {
    display: flex;
    gap: 6px;
    margin-bottom: 1rem;
    padding-bottom: 0.8rem;
    border-bottom: 1px solid var(--border);
  }

  .t-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .t-dot:nth-child(1) { background: #ff2d55; }
  .t-dot:nth-child(2) { background: #ffd600; }
  .t-dot:nth-child(3) { background: #00ff88; }

  .terminal-line { display: block; }
  .terminal-line.warn { color: #ffcc00; }
  .terminal-line.danger { color: var(--red); }
  .terminal-line.muted { color: #2a5560; }
  .terminal-line.info { color: var(--cyan); }

  .cursor-blink {
    display: inline-block;
    width: 8px; height: 1em;
    background: var(--green);
    vertical-align: text-bottom;
    animation: blink 1s infinite;
  }

  .monitor-text .section-title { text-align: left; margin-bottom: 1.2rem; }
  .monitor-text .section-tag { text-align: left; }

  .feature-list {
    list-style: none;
    margin-top: 1.5rem;
    display: flex;
    flex-direction: column;
    gap: 0.8rem;
  }

  .feature-list li {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-size: 0.9rem;
    color: #7bb8c8;
    opacity: 0;
    transform: translateX(-20px);
    transition: opacity 0.5s ease, transform 0.5s ease;
  }

  .feature-list li.visible { opacity: 1; transform: translateX(0); }

  .feat-icon {
    width: 18px; height: 18px;
    flex-shrink: 0;
    color: var(--cyan);
  }

  /* ── PRICING ── */
  #pricing {
    padding: 6rem 3rem;
    max-width: 1100px;
    margin: 0 auto;
  }

  .pricing-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
  }

  .price-card {
    border: 1px solid var(--border);
    padding: 2.5rem 2rem;
    position: relative;
    clip-path: polygon(0 0, calc(100% - 18px) 0, 100% 18px, 100% 100%, 18px 100%, 0 calc(100% - 18px));
    opacity: 0;
    transform: scale(0.95);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }

  .price-card.visible { opacity: 1; transform: scale(1); }

  .price-card.featured {
    border-color: var(--cyan);
    background: rgba(0,245,255,0.04);
  }

  .price-card.featured::after {
    content: 'MOST POPULAR';
    position: absolute;
    top: -1px; left: 50%; transform: translateX(-50%);
    background: var(--cyan);
    color: var(--darker);
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.62rem;
    letter-spacing: 2px;
    padding: 3px 12px;
  }

  .plan-name {
    font-family: 'Rajdhani', sans-serif;
    font-size: 0.8rem;
    font-weight: 700;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: var(--cyan);
    margin-bottom: 1rem;
  }

  .plan-price {
    font-family: 'Exo 2', sans-serif;
    font-size: 3rem;
    font-weight: 700;
    color: #e0f4ff;
    line-height: 1;
    margin-bottom: 0.25rem;
  }

  .plan-price sup {
    font-size: 1.2rem;
    vertical-align: top;
    margin-top: 0.5rem;
    display: inline-block;
    color: var(--cyan);
  }

  .plan-period {
    font-size: 0.8rem;
    color: #5a8a99;
    margin-bottom: 2rem;
  }

  .plan-features {
    list-style: none;
    margin-bottom: 2rem;
    display: flex;
    flex-direction: column;
    gap: 0.7rem;
  }

  .plan-features li {
    font-size: 0.87rem;
    color: #7bb8c8;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .plan-features li::before {
    content: '▹';
    color: var(--cyan);
    font-size: 0.8rem;
  }

  .btn-plan {
    width: 100%;
    background: transparent;
    border: 1px solid var(--border);
    color: #cce8f0;
    font-family: 'Rajdhani', sans-serif;
    font-weight: 700;
    letter-spacing: 2px;
    font-size: 0.85rem;
    text-transform: uppercase;
    padding: 0.85rem;
    cursor: crosshair;
    transition: all 0.3s;
    clip-path: polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
  }

  .price-card.featured .btn-plan {
    background: var(--cyan);
    border-color: var(--cyan);
    color: var(--darker);
  }

  .btn-plan:hover {
    border-color: var(--cyan);
    color: var(--cyan);
    box-shadow: var(--glow);
  }

  .price-card.featured .btn-plan:hover {
    color: var(--darker);
    box-shadow: 0 0 30px rgba(0,245,255,0.5);
  }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 3rem;
    text-align: center;
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.75rem;
    color: #2a5560;
    letter-spacing: 1px;
  }

  footer .f-logo {
    font-size: 1.2rem;
    color: var(--cyan);
    text-shadow: var(--glow);
    letter-spacing: 4px;
    display: block;
    margin-bottom: 1rem;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeSlideDown {
    from { opacity: 0; transform: translateY(-20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  @keyframes fadeSlideUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  @keyframes floatHex {
    0%   { transform: translateY(0) rotate(0deg); }
    50%  { transform: translateY(-40px) rotate(180deg); }
    100% { transform: translateY(0) rotate(360deg); }
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; box-shadow: 0 0 4px var(--green); }
    50%       { opacity: 0.4; box-shadow: 0 0 12px var(--green); }
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0; }
  }

  @keyframes scanline {
    from { top: -4px; }
    to   { top: 100%; }
  }

  /* Moving scan highlight */
  .scan-highlight {
    position: fixed;
    left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(0,245,255,0.15), transparent);
    pointer-events: none;
    z-index: 3;
    animation: scanline 5s linear infinite;
  }

  /* Glitch effect on logo hover */
  .logo:hover {
    animation: glitch 0.4s steps(2) 1;
  }

  @keyframes glitch {
    0%   { text-shadow: 0 0 20px rgba(0,245,255,0.4); clip-path: none; }
    20%  { text-shadow: 3px 0 0 rgba(255,0,80,0.6), -3px 0 0 rgba(0,255,136,0.6); clip-path: inset(10% 0 80% 0); }
    40%  { text-shadow: -3px 0 0 rgba(255,0,80,0.6), 3px 0 0 rgba(0,255,136,0.6); clip-path: inset(60% 0 20% 0); }
    60%  { text-shadow: 2px 0 0 rgba(255,0,80,0.4); clip-path: inset(40% 0 50% 0); }
    80%  { clip-path: none; }
    100% { text-shadow: var(--glow); }
  }
</style>
</head>
<body>

<canvas id="matrix-canvas"></canvas>
<div class="scan-highlight"></div>

<!-- NAV -->
<nav>
  <a href="#" class="logo">NEX<span>SHIELD</span></a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#monitor">Monitoring</a></li>
    <li><a href="#pricing">Pricing</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
  <button class="nav-cta">Get Protected</button>
</nav>

<!-- HERO -->
<section id="hero">
  <div class="hex-grid">
    <div class="hex"></div>
    <div class="hex"></div>
    <div class="hex"></div>
    <div class="hex"></div>
    <div class="hex"></div>
  </div>
  <div class="hero-inner">
    <div class="badge">
      <div class="badge-dot"></div>
      SYSTEM STATUS: ALL DEFENSES ONLINE
    </div>
    <h1 class="hero-title">
      <span class="line1">Next-Generation</span>
      <span class="line2">Cyber Security</span>
    </h1>
    <p class="hero-sub">
      AI-powered threat detection and zero-trust architecture that adapts in real-time. 
      Protect your digital infrastructure before threats materialize.
    </p>
    <div class="hero-buttons">
      <a href="#services" class="btn-primary">Explore Arsenal</a>
      <a href="#monitor" class="btn-outline">Live Threat Feed</a>
    </div>
  </div>
</section>

<!-- STATS -->
<div class="stats-bar">
  <div class="stat-item">
    <span class="stat-num" data-target="99.9">0</span>
    <span class="stat-label">% Uptime SLA</span>
  </div>
  <div class="stat-item">
    <span class="stat-num" data-target="4800">0</span>
    <span class="stat-label">Threats Neutralized Daily</span>
  </div>
  <div class="stat-item">
    <span class="stat-num" data-target="2300">0</span>
    <span class="stat-label">Global Clients</span>
  </div>
  <div class="stat-item">
    <span class="stat-num" data-target="0.3">0</span>
    <span class="stat-label">ms Avg Response Time</span>
  </div>
</div>

<!-- SERVICES -->
<section 
