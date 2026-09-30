<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- SECURITY: 内容安全策略（CSP），限制资源加载来源，防止 XSS -->
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self'; connect-src 'self'; object-src 'none'; frame-src 'none'; base-uri 'self'; form-action 'self'; upgrade-insecure-requests;" />
  <!-- SECURITY: 禁止 MIME 类型嗅探 -->
  <meta http-equiv="X-Content-Type-Options" content="nosniff" />
  <!-- SECURITY: 控制 Referrer 信息 -->
  <meta name="referrer" content="strict-origin-when-cross-origin" />
  <!-- SECURITY: 禁用自动填充敏感信息（可选） -->
  <meta name="format-detection" content="telephone=no" />

  <title>VOLTA — 专业电池制造</title>
  <style>
    /* ========== 全局重置 ========== */
    * { margin: 0; padding: 0; box-sizing: border-box; }

    html { scroll-behavior: smooth; }

    body {
      background: #f7f6f4;
      font-family: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      color: #1a1a1a;
      line-height: 1.6;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
    }

    /* SECURITY: 蜜罐字段隐藏（防机器人） */
    .honeypot {
      position: absolute !important;
      left: -9999px !important;
      top: -9999px !important;
      width: 1px;
      height: 1px;
      overflow: hidden;
      opacity: 0;
      pointer-events: none;
    }

    /* SECURITY: 表单状态提示 */
    .form-status {
      font-size: 0.85rem;
      margin-top: 0.5rem;
      min-height: 1.2em;
      color: #666;
    }
    .form-status.error { color: #c0392b; }
    .form-status.success { color: #27ae60; }

    /* ========== 导航栏 ========== */
    .navbar {
      position: fixed;
      top: 0; left: 0; width: 100%;
      padding: 1.2rem 3rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(247, 246, 244, 0.82);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
      z-index: 1000;
      border-bottom: 1px solid rgba(0, 0, 0, 0.04);
      transition: background 0.3s ease, border-color 0.3s ease;
    }

    .navbar.scrolled {
      background: rgba(247, 246, 244, 0.96);
      border-bottom-color: rgba(0, 0, 0, 0.07);
    }

    .logo {
      font-size: 1.35rem;
      font-weight: 600;
      letter-spacing: -0.03em;
      color: #1a1a1a;
      text-decoration: none;
    }

    .logo span { opacity: 0.35; font-weight: 400; }

    .nav-right {
      display: flex;
      align-items: center;
      gap: 2.8rem;
    }

    .nav-links {
      display: flex;
      gap: 2.4rem;
      font-size: 0.9rem;
      font-weight: 420;
      letter-spacing: -0.01em;
    }

    .nav-links a {
      text-decoration: none;
      color: #1a1a1a;
      opacity: 0.65;
      transition: opacity 0.25s ease;
    }

    .nav-links a:hover { opacity: 1; }

    /* ========== 语言切换 ========== */
    .lang-switcher { position: relative; }

    .lang-btn {
      display: flex;
      align-items: center;
      gap: 0.55rem;
      padding: 0.45rem 0.8rem;
      background: transparent;
      border: 1px solid rgba(0, 0, 0, 0.1);
      border-radius: 100px;
      cursor: pointer;
      font-size: 0.82rem;
      font-weight: 450;
      color: #1a1a1a;
      font-family: inherit;
      transition: border-color 0.25s ease, background 0.25s ease;
    }

    .lang-btn:hover {
      border-color: rgba(0, 0, 0, 0.25);
      background: rgba(0, 0, 0, 0.02);
    }

    .lang-btn .arrow {
      font-size: 0.6rem;
      opacity: 0.5;
      transition: transform 0.25s ease;
    }

    .lang-switcher.open .lang-btn .arrow { transform: rotate(180deg); }

    .lang-dropdown {
      position: absolute;
      top: calc(100% + 0.6rem);
      right: 0;
      background: #fff;
      border-radius: 14px;
      padding: 0.5rem;
      min-width: 190px;
      box-shadow: 0 12px 40px -8px rgba(0, 0, 0, 0.12), 0 0 0 1px rgba(0, 0, 0, 0.04);
      opacity: 0;
      visibility: hidden;
      transform: translateY(-6px);
      transition: opacity 0.25s ease, transform 0.25s ease, visibility 0.25s;
      z-index: 1001;
    }

    .lang-switcher.open .lang-dropdown {
      opacity: 1;
      visibility: visible;
      transform: translateY(0);
    }

    .lang-option {
      display: flex;
      align-items: center;
      gap: 0.7rem;
      padding: 0.6rem 0.8rem;
      border-radius: 9px;
      cursor: pointer;
      font-size: 0.85rem;
      font-weight: 420;
      color: #1a1a1a;
      transition: background 0.2s ease;
      border: none;
      background: transparent;
      width: 100%;
      font-family: inherit;
      text-align: left;
    }

    .lang-option:hover { background: #f2f1ef; }
    .lang-option.active { background: #1a1a1a; color: #fff; }
    .lang-option.active .flag { border-color: rgba(255,255,255,0.3); }

    /* ========== 国旗 ========== */
    .flag {
      width: 22px;
      height: 15px;
      border-radius: 3px;
      overflow: hidden;
      flex-shrink: 0;
      position: relative;
      border: 1px solid rgba(0, 0, 0, 0.08);
      box-shadow: 0 0 0 1px rgba(0,0,0,0.03);
    }

    .flag-cn { background: #de2910; }
    .flag-cn::before {
      content: '★';
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      color: #ffde00;
      font-size: 9px;
      line-height: 1;
    }

    .flag-us {
      background: repeating-linear-gradient(
        to bottom,
        #b22234 0%, #b22234 14.28%,
        #fff 14.28%, #fff 28.57%
      );
    }
    .flag-us::before {
      content: '';
      position: absolute;
      top: 0; left: 0;
      width: 42%; height: 54%;
      background: #3c3b6e;
    }

    .flag-es {
      background: linear-gradient(
        to bottom,
        #aa151b 0%, #aa151b 25%,
        #f1bf00 25%, #f1bf00 75%,
        #aa151b 75%, #aa151b 100%
      );
    }

    .flag-fr {
      background: linear-gradient(
        to right,
        #002395 0%, #002395 33.33%,
        #fff 33.33%, #fff 66.66%,
        #ed2939 66.66%, #ed2939 100%
      );
    }

    .flag-de {
      background: linear-gradient(
        to bottom,
        #000 0%, #000 33.33%,
        #dd0000 33.33%, #dd0000 66.66%,
        #ffce00 66.66%, #ffce00 100%
      );
    }

    .flag-jp { background: #fff; }
    .flag-jp::before {
      content: '';
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      width: 50%; height: 70%;
      background: #bc002d;
      border-radius: 50%;
    }

    /* ========== 容器 ========== */
    .container {
      max-width: 1280px;
      margin: 0 auto;
      padding: 0 2.5rem;
    }

    /* ========== Hero ========== */
    .hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      padding-top: 6rem;
      padding-bottom: 4rem;
    }

    .hero-inner {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      width: 100%;
    }

    .hero-eyebrow {
      font-size: 0.82rem;
      font-weight: 500;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: #999;
      margin-bottom: 1.8rem;
    }

    .hero h1 {
      font-size: clamp(2.6rem, 6vw, 4.6rem);
      font-weight: 550;
      letter-spacing: -0.045em;
      line-height: 1.08;
      color: #1a1a1a;
      max-width: 820px;
      margin-bottom: 1.4rem;
    }

    .hero p {
      font-size: 1.15rem;
      font-weight: 380;
      color: #6b6b6b;
      max-width: 560px;
      letter-spacing: -0.01em;
      margin-bottom: 3.5rem;
    }

    .hero-visual {
      width: 100%;
      max-width: 980px;
      border-radius: 28px;
      overflow: hidden;
      background: linear-gradient(145deg, #eeedea 0%, #e2e0dc 100%);
      box-shadow: 0 40px 80px -24px rgba(0, 0, 0, 0.12);
      transition: transform 0.7s cubic-bezier(0.2, 0.9, 0.3, 1), box-shadow 0.5s ease;
    }

    .hero-visual:hover {
      transform: scale(1.008);
      box-shadow: 0 50px 90px -28px rgba(0, 0, 0, 0.16);
    }

    .hero-visual svg { width: 100%; height: auto; display: block; }

    /* ========== 产品展示 ========== */
    .products { padding: 9rem 0 7rem; background: #fff; }

    .section-header { text-align: center; margin-bottom: 5rem; }

    .section-header h2 {
      font-size: clamp(1.9rem, 3.5vw, 2.8rem);
      font-weight: 550;
      letter-spacing: -0.04em;
      line-height: 1.15;
      margin-bottom: 0.9rem;
    }

    .section-header p {
      font-size: 1.05rem;
      color: #777;
      font-weight: 380;
      max-width: 480px;
      margin: 0 auto;
    }

    .product-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 2rem;
    }

    .product-card {
      background: #f7f6f4;
      border-radius: 24px;
      padding: 3rem 2.8rem 2.8rem;
      display: flex;
      flex-direction: column;
      transition: transform 0.5s cubic-bezier(0.2, 0.9, 0.3, 1), box-shadow 0.5s ease;
      cursor: pointer;
      position: relative;
      overflow: hidden;
    }

    .product-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 24px 48px -16px rgba(0, 0, 0, 0.1);
    }

    .product-card .product-tag {
      font-size: 0.72rem;
      font-weight: 550;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: #aaa;
      margin-bottom: 0.8rem;
    }

    .product-card h3 {
      font-size: 1.5rem;
      font-weight: 550;
      letter-spacing: -0.03em;
      margin-bottom: 0.7rem;
    }

    .product-card .product-desc {
      font-size: 0.95rem;
      color: #777;
      line-height: 1.65;
      font-weight: 380;
      margin-bottom: 2.2rem;
      max-width: 340px;
    }

    .product-card .product-image {
      margin-top: auto;
      border-radius: 16px;
      overflow: hidden;
      background: #eeedea;
    }

    .product-card .product-image svg {
      width: 100%;
      height: auto;
      display: block;
      transition: transform 0.6s cubic-bezier(0.2, 0.9, 0.3, 1);
    }

    .product-card:hover .product-image svg { transform: scale(1.03); }

    /* ========== 公司介绍 ========== */
    .about { padding: 8rem 0; background: #f7f6f4; }

    .about-inner {
      display: flex;
      gap: 6rem;
      align-items: center;
      max-width: 1100px;
      margin: 0 auto;
    }

    .about-text { flex: 1; }

    .about-text h2 {
      font-size: clamp(1.8rem, 3vw, 2.5rem);
      font-weight: 550;
      letter-spacing: -0.04em;
      line-height: 1.15;
      margin-bottom: 1.6rem;
    }

    .about-text p {
      font-size: 1.02rem;
      color: #666;
      line-height: 1.8;
      font-weight: 380;
      margin-bottom: 1.2rem;
    }

    .about-stats {
      flex: 1;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 2.5rem;
    }

    .stat-item h4 {
      font-size: 2.6rem;
      font-weight: 550;
      letter-spacing: -0.04em;
      line-height: 1;
      margin-bottom: 0.5rem;
    }

    .stat-item p {
      font-size: 0.9rem;
      color: #888;
      font-weight: 400;
      line-height: 1.5;
    }

    /* ========== 联系方式 ========== */
    .contact { padding: 8rem 0; background: #fff; }

    .contact-inner {
      max-width: 900px;
      margin: 0 auto;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 5rem;
      align-items: start;
    }

    .contact-info h2 {
      font-size: clamp(1.8rem, 3vw, 2.4rem);
      font-weight: 550;
      letter-spacing: -0.04em;
      line-height: 1.15;
      margin-bottom: 1.4rem;
    }

    .contact-info > p {
      font-size: 1rem;
      color: #777;
      line-height: 1.7;
      font-weight: 380;
      margin-bottom: 2.5rem;
    }

    .contact-item { margin-bottom: 1.6rem; }

    .contact-item .label {
      font-size: 0.72rem;
      font-weight: 550;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: #aaa;
      margin-bottom: 0.35rem;
    }

    .contact-item .value {
      font-size: 1rem;
      color: #1a1a1a;
      font-weight: 420;
    }

    .contact-item .value a {
      color: #1a1a1a;
      text-decoration: none;
      border-bottom: 1px solid rgba(0,0,0,0.15);
      transition: border-color 0.25s ease;
    }

    .contact-item .value a:hover { border-bottom-color: #1a1a1a; }

    .contact-form { display: flex; flex-direction: column; gap: 1.2rem; }

    .contact-form input,
    .contact-form textarea {
      width: 100%;
      padding: 0.95rem 1.2rem;
      border: 1px solid rgba(0, 0, 0, 0.1);
      border-radius: 12px;
      font-size: 0.95rem;
      font-family: inherit;
      color: #1a1a1a;
      background: #faf9f7;
      outline: none;
      transition: border-color 0.25s ease, background 0.25s ease;
      font-weight: 380;
    }

    .contact-form input:focus,
    .contact-form textarea:focus {
      border-color: rgba(0, 0, 0, 0.3);
      background: #fff;
    }

    .contact-form textarea { resize: vertical; min-height: 120px; }

    .contact-form button {
      padding: 0.95rem 2rem;
      background: #1a1a1a;
      color: #fff;
      border: none;
      border-radius: 12px;
      font-size: 0.95rem;
      font-weight: 500;
      font-family: inherit;
      cursor: pointer;
      transition: background 0.25s ease, transform 0.2s ease;
      letter-spacing: -0.01em;
    }

    .contact-form button:hover { background: #333; transform: translateY(-1px); }
    .contact-form button:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

    /* ========== 页脚 ========== */
    .footer {
      padding: 2.8rem 0;
      border-top: 1px solid rgba(0, 0, 0, 0.05);
      background: #f7f6f4;
    }

    .footer-inner {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.85rem;
      color: #999;
      font-weight: 380;
    }

    .footer-links { display: flex; gap: 2rem; }

    .footer-links a {
      color: #999;
      text-decoration: none;
      transition: color 0.2s ease;
    }

    .footer-links a:hover { color: #1a1a1a; }

    /* ========== 动画 ========== */
    .fade-up {
      opacity: 0;
      transform: translateY(24px);
      animation: fadeUp 1s cubic-bezier(0.2, 0.9, 0.3, 1) forwards;
    }

    @keyframes fadeUp { to { opacity: 1; transform: translateY(0); } }

    .delay-1 { animation-delay: 0.12s; }
    .delay-2 { animation-delay: 0.24s; }
    .delay-3 { animation-delay: 0.36s; }

    /* ========== 响应式 ========== */
    @media (max-width: 1024px) {
      .navbar { padding: 1rem 1.8rem; }
      .container { padding: 0 1.8rem; }
      .nav-links { gap: 1.6rem; font-size: 0.85rem; }
      .product-grid { gap: 1.5rem; }
      .about-inner { gap: 3.5rem; }
    }

    @media (max-width: 820px) {
      .nav-links { display: none; }
      .product-grid { grid-template-columns: 1fr; }
      .about-inner { flex-direction: column; gap: 3rem; }
      .contact-inner { grid-template-columns: 1fr; gap: 3rem; }
      .hero h1 { font-size: 2.4rem; }
      .hero p { font-size: 1rem; }
      .product-card { padding: 2.2rem 1.8rem; }
    }

    @media (max-width: 480px) {
      .navbar { padding: 0.9rem 1.2rem; }
      .container { padding: 0 1.2rem; }
      .logo { font-size: 1.1rem; }
      .lang-btn { padding: 0.35rem 0.6rem; font-size: 0.75rem; }
      .flag { width: 18px; height: 12px; }
      .footer-inner { flex-direction: column; gap: 1.2rem; text-align: center; }
    }
  </style>
</head>
<body>

  <!-- ========== 导航栏 ========== -->
  <nav class="navbar" id="navbar">
    <a href="#" class="logo">VOLTA<span>.</span></a>
    <div class="nav-right">
      <div class="nav-links">
        <a href="#products" data-i18n="nav.products">产品</a>
        <a href="#about" data-i18n="nav.about">公司介绍</a>
        <a href="#contact" data-i18n="nav.contact">联系我们</a>
      </div>

      <!-- 语言切换 -->
      <div class="lang-switcher" id="langSwitcher">
        <button class="lang-btn" id="langBtn" aria-haspopup="listbox" aria-expanded="false">
          <span class="flag flag-cn" id="currentFlag"></span>
          <span id="currentLang">中文</span>
          <span class="arrow">▼</span>
        </button>
        <div class="lang-dropdown" id="langDropdown" role="listbox">
          <button class="lang-option active" data-lang="zh" role="option">
            <span class="flag flag-cn"></span> 中文
          </button>
          <button class="lang-option" data-lang="en" role="option">
            <span class="flag flag-us"></span> English
          </button>
          <button class="lang-option" data-lang="es" role="option">
            <span class="flag flag-es"></span> Español
          </button>
          <button class="lang-option" data-lang="fr" role="option">
            <span class="flag flag-fr"></span> Français
          </button>
          <button class="lang-option" data-lang="de" role="option">
            <span class="flag flag-de"></span> Deutsch
          </button>
          <button class="lang-option" data-lang="ja" role="option">
            <span class="flag flag-jp"></span> 日本語
          </button>
        </div>
      </div>
    </div>
  </nav>

  <main>
    <!-- ========== Hero ========== -->
    <section class="hero">
      <div class="container hero-inner">
        <div class="hero-eyebrow fade-up" data-i18n="hero.eyebrow">专业电池制造 · 始于 2005</div>
        <h1 class="fade-up delay-1" data-i18n="hero.title">动力，源于专业。</h1>
        <p class="fade-up delay-2" data-i18n="hero.subtitle">为汽车、游艇、帆船及工业设备提供高性能蓄电池解决方案。全球信赖，持久可靠。</p>
        <div class="hero-visual fade-up delay-3">
          <svg viewBox="0 0 900 500" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="VOLTA 电池产品示意">
            <rect width="900" height="500" fill="url(#heroGrad)"/>
            <defs>
              <linearGradient id="heroGrad" x1="0" y1="0" x2="900" y2="500" gradientUnits="userSpaceOnUse">
                <stop offset="0%" stop-color="#eeedea"/>
                <stop offset="100%" stop-color="#e0ded9"/>
              </linearGradient>
              <linearGradient id="battGrad" x1="300" y1="150" x2="600" y2="380" gradientUnits="userSpaceOnUse">
                <stop offset="0%" stop-color="#2a2a2a"/>
                <stop offset="100%" stop-color="#111"/>
              </linearGradient>
            </defs>
            <rect x="300" y="140" width="300" height="220" rx="20" fill="url(#battGrad)" opacity="0.92"/>
            <rect x="340" y="120" width="40" height="24" rx="6" fill="#3a3a3a"/>
            <rect x="520" y="120" width="40" height="24" rx="6" fill="#3a3a3a"/>
            <circle cx="360" cy="132" r="6" fill="#e8e8e8" opacity="0.6"/>
            <circle cx="540" cy="132" r="6" fill="#e8e8e8" opacity="0.3"/>
            <rect x="320" y="160" width="260" height="2" rx="1" fill="#fff" opacity="0.08"/>
            <text x="450" y="265" text-anchor="middle" fill="#fff" opacity="0.85" font-family="Inter, sans-serif" font-size="28" font-weight="600" letter-spacing="4">VOLTA</text>
            <text x="450" y="295" text-anchor="middle" fill="#fff" opacity="0.35" font-family="Inter, sans-serif" font-size="12" letter-spacing="6">POWER SERIES</text>
            <rect x="350" y="320" width="200" height="4" rx="2" fill="#fff" opacity="0.12"/>
            <rect x="350" y="320" width="140" height="4" rx="2" fill="#fff" opacity="0.5"/>
            <circle cx="180" cy="100" r="120" stroke="#1a1a1a" stroke-width="0.6" opacity="0.06"/>
            <circle cx="720" cy="400" r="160" stroke="#1a1a1a" stroke-width="0.6" opacity="0.06"/>
            <line x1="80" y1="420" x2="280" y2="420" stroke="#1a1a1a" stroke-width="0.6" opacity="0.08"/>
            <line x1="620" y1="80" x2="820" y2="80" stroke="#1a1a1a" stroke-width="0.6" opacity="0.08"/>
          </svg>
        </div>
      </div>
    </section>

    <!-- ========== 产品展示 ========== -->
    <section class="products" id="products">
      <div class="container">
        <div class="section-header fade-up">
          <h2 data-i18n="products.title">产品系列</h2>
          <p data-i18n="products.subtitle">为每一种动力需求，提供专业可靠的电池解决方案。</p>
        </div>

        <div class="product-grid">
          <div class="product-card fade-up">
            <div class="product-tag" data-i18n="products.auto.tag">汽车电瓶</div>
            <h3 data-i18n="products.auto.title">启动电池</h3>
            <p class="product-desc" data-i18n="products.auto.desc">高冷启动电流，卓越低温性能，为每一次点火提供强劲动力。</p>
            <div class="product-image">
              <svg viewBox="0 0 500 280" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="汽车启动电池">
                <rect width="500" height="280" fill="#eeedea"/>
                <rect x="150" y="70" width="200" height="140" rx="14" fill="#2a2a2a" opacity="0.9"/>
                <rect x="175" y="55" width="30" height="20" rx="5" fill="#3a3a3a"/>
                <rect x="295" y="55" width="30" height="20" rx="5" fill="#3a3a3a"/>
                <circle cx="190" cy="65" r="4" fill="#e8e8e8" opacity="0.5"/>
                <circle cx="310" cy="65" r="4" fill="#e8e8e8" opacity="0.25"/>
                <text x="250" y="148" text-anchor="middle" fill="#fff" opacity="0.8" font-family="Inter, sans-serif" font-size="16" font-weight="600" letter-spacing="3">VOLTA</text>
                <text x="250" y="168" text-anchor="middle" fill="#fff" opacity="0.3" font-family="Inter, sans-serif" font-size="8" letter-spacing="4">AUTO</text>
                <rect x="185" y="190" width="130" height="3" rx="1.5" fill="#fff" opacity="0.1"/>
                <rect x="185" y="190" width="90" height="3" rx="1.5" fill="#fff" opacity="0.4"/>
              </svg>
            </div>
          </div>

          <div class="product-card fade-up delay-1">
            <div class="product-tag" data-i18n="products.marine.tag">游艇电瓶</div>
            <h3 data-i18n="products.marine.title">船用电池</h3>
            <p class="product-desc" data-i18n="products.marine.desc">抗腐蚀设计，深循环能力，从容应对海上严苛环境。</p>
            <div class="product-image">
              <svg viewBox="0 0 500 280" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="游艇船用电池">
                <rect width="500" height="280" fill="#eeedea"/>
                <rect x="140" y="60" width="220" height="150" rx="16" fill="#1e3a4a" opacity="0.92"/>
                <rect x="170" y="45" width="30" height="20" rx="5" fill="#2a4a5a"/>
                <rect x="300" y="45" width="30" height="20" rx="5" fill="#2a4a5a"/>
                <circle cx="185" cy="55" r="4" fill="#fff" opacity="0.5"/>
                <circle cx="315" cy="55" r="4" fill="#fff" opacity="0.2"/>
                <text x="250" y="140" text-anchor="middle" fill="#fff" opacity="0.8" font-family="Inter, sans-serif" font-size="16" font-weight="600" letter-spacing="3">VOLTA</text>
                <text x="250" y="160" text-anchor="middle" fill="#fff" opacity="0.3" font-family="Inter, sans-serif" font-size="8" letter-spacing="4">MARINE</text>
                <rect x="180" y="190" width="140" height="3" rx="1.5" fill="#fff" opacity="0.1"/>
                <rect x="180" y="190" width="100" height="3" rx="1.5" fill="#fff" opacity="0.4"/>
              </svg>
            </div>
          </div>

          <div class="product-card fade-up delay-2">
            <div class="product-tag" data-i18n="products.sail.tag">帆船电瓶</div>
            <h3 data-i18n="products.sail.title">帆船电池</h3>
            <p class="product-desc" data-i18n="products.sail.desc">轻量化设计，长续航表现，为远航提供稳定能量。</p>
            <div class="product-image">
              <svg viewBox="0 0 500 280" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="帆船电池">
                <rect width="500" height="280" fill="#eeedea"/>
                <rect x="155" y="65" width="190" height="145" rx="14" fill="#2a2a2a" opacity="0.88"/>
                <rect x="180" y="50" width="28" height="20" rx="5" fill="#3a3a3a"/>
                <rect x="292" y="50" width="28" height="20" rx="5" fill="#3a3a3a"/>
                <circle cx="194" cy="60" r="4" fill="#e8e8e8" opacity="0.5"/>
                <circle cx="306" cy="60" r="4" fill="#e8e8e8" opacity="0.25"/>
                <text x="250" y="142" text-anchor="middle" fill="#fff" opacity="0.8" font-family="Inter, sans-serif" font-size="15" font-weight="600" letter-spacing="3">VOLTA</text>
                <text x="250" y="162" text-anchor="middle" fill="#fff" opacity="0.3" font-family="Inter, sans-serif" font-size="8" letter-spacing="4">SAIL</text>
                <rect x="190" y="190" width="120" height="3" rx="1.5" fill="#fff" opacity="0.1"/>
                <rect x="190" y="190" width="80" height="3" rx="1.5" fill="#fff" opacity="0.4"/>
              </svg>
            </div>
          </div>

          <div class="product-card fade-up delay-3">
            <div class="product-tag" data-i18n="products.industrial.tag">蓄电池</div>
            <h3 data-i18n="products.industrial.title">工业蓄电池</h3>
            <p class="product-desc" data-i18n="products.industrial.desc">高循环寿命，稳定输出，适用于储能、UPS及工业设备。</p>
            <div class="product-image">
              <svg viewBox="0 0 500 280" fill="none" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="工业蓄电池">
                <rect width="500" height="280" fill="#eeedea"/>
                <rect x="130" y="55" width="240" height="160" rx="16" fill="#1a1a1a" opacity="0.9"/>
                <rect x="160" y="40" width="32" height="20" rx="5" fill="#2a2a2a"/>
                <rect x="308" y="40" width="32" height="20" rx="5" fill="#2a2a2a"/>
                <circle cx="176" cy="50" r="4" fill="#e8e8e8" opacity="0.5"/>
                <circle cx="324" cy="50" r="4" fill="#e8e8e8" opacity="0.25"/>
                <text x="250" y="140" text-anchor="middle" fill="#fff" opacity="0.8" font-family="Inter, sans-serif" font-size="15" font-weight="600" letter-spacing="3">VOLTA</text>
                <text x="250" y="160" text-anchor="middle" fill="#fff" opacity="0.3" font-family="Inter, sans-serif" font-size="8" letter-spacing="3">INDUSTRIAL</text>
                <rect x="170" y="195" width="160" height="3" rx="1.5" fill="#fff" opacity="0.1"/>
                <rect x="170" y="195" width="110" height="3" rx="1.5" fill="#fff" opacity="0.4"/>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ========== 公司介绍 ========== -->
    <section class="about" id="about">
      <div class="container about-inner">
        <div class="about-text fade-up">
          <h2 data-i18n="about.title">二十年专注，<br/>只为一块好电池。</h2>
          <p data-i18n="about.p1">VOLTA 成立于 2005 年，专注于蓄电池研发与制造。我们为全球客户提供汽车、游艇、帆船及工业领域的专业电池解决方案。</p>
          <p data-i18n="about.p2">从原材料到成品，每一道工序都经过严格品控。我们的产品通过多项国际认证，远销全球 60 多个国家和地区。</p>
        </div>
        <div class="about-stats">
          <div class="stat-item fade-up delay-1">
            <h4>20+</h4>
            <p data-i18n="about.stat1">年行业经验</p>
          </div>
          <div class="stat-item fade-up delay-2">
            <h4>60+</h4>
            <p data-i18n="about.stat2">出口国家</p>
          </div>
          <div class="stat-item fade-up delay-1">
            <h4>500万+</h4>
            <p data-i18n="about.stat3">年产量</p>
          </div>
          <div class="stat-item fade-up delay-2">
            <h4>99.8%</h4>
            <p data-i18n="about.stat4">客户满意度</p>
          </div>
        </div>
      </div>
    </section>

    <!-- ========== 联系方式 ========== -->
    <section class="contact" id="contact">
      <div class="container contact-inner">
        <div class="contact-info fade-up">
          <h2 data-i18n="contact.title">联系我们</h2>
          <p data-i18n="contact.subtitle">无论是产品咨询、合作洽谈还是售后服务，我们随时为您提供支持。</p>

          <div class="contact-item">
            <div class="label" data-i18n="contact.email">邮箱</div>
            <div class="value"><a href="mailto:info@volta.com">info@volta.com</a></div>
          </div>
          <div class="contact-item">
            <div class="label" data-i18n="contact.phone">电话</div>
            <div class="value">+86 400 888 6666</div>
          </div>
          <div class="contact-item">
            <div class="label" data-i18n="contact.address">地址</div>
            <div class="value" data-i18n="contact.addressValue">中国 · 广东省 · 深圳市 · 科技园</div>
          </div>
        </div>

        <!-- SECURITY: 表单加固（honeypot + 长度限制 + 自动完成 + 前端校验） -->
        <form class="contact-form fade-up delay-1" id="contactForm" novalidate>
          <!-- 蜜罐字段：正常用户不可见，机器人会填写 -->
          <div class="honeypot" aria-hidden="true">
            <label for="website">Website</label>
            <input type="text" id="website" name="website" tabindex="-1" autocomplete="off" />
          </div>

          <input
            type="text"
            id="name"
            name="name"
            data-i18n-placeholder="contact.formName"
            placeholder="您的姓名"
            required
            maxlength="100"
            autocomplete="name"
          />
          <input
            type="email"
            id="email"
            name="email"
            data-i18n-placeholder="contact.formEmail"
            placeholder="您的邮箱"
            required
            maxlength="254"
            autocomplete="email"
          />
          <textarea
            id="message"
            name="message"
            data-i18n-placeholder="contact.formMessage"
            placeholder="留言内容"
            required
            maxlength="2000"
            autocomplete="off"
          ></textarea>
          <button type="submit" data-i18n="contact.formSubmit" id="submitBtn">发送信息</button>
          <div class="form-status" id="formStatus" role="status" aria-live="polite"></div>
        </form>
      </div>
    </section>

    <!-- ========== 页脚 ========== -->
    <footer class="footer">
      <div class="container footer-inner">
        <span data-i18n="footer.copyright">© 2025 VOLTA. 保留所有权利。</span>
        <div class="footer-links">
          <a href="#" data-i18n="footer.privacy">隐私政策</a>
          <a href="#" data-i18n="footer.terms">使用条款</a>
        </div>
      </div>
    </footer>
  </main>

  <!-- ========== 多语言数据 ========== -->
  <script>
    const translations = {
      zh: {
        "nav.products": "产品",
        "nav.about": "公司介绍",
        "nav.contact": "联系我们",
        "hero.eyebrow": "专业电池制造 · 始于 2005",
        "hero.title": "动力，源于专业。",
        "hero.subtitle": "为汽车、游艇、帆船及工业设备提供高性能蓄电池解决方案。全球信赖，持久可靠。",
        "products.title": "产品系列",
        "products.subtitle": "为每一种动力需求，提供专业可靠的电池解决方案。",
        "products.auto.tag": "汽车电瓶",
        "products.auto.title": "启动电池",
        "products.auto.desc": "高冷启动电流，卓越低温性能，为每一次点火提供强劲动力。",
        "products.marine.tag": "游艇电瓶",
        "products.marine.title": "船用电池",
        "products.marine.desc": "抗腐蚀设计，深循环能力，从容应对海上严苛环境。",
        "products.sail.tag": "帆船电瓶",
        "products.sail.title": "帆船电池",
        "products.sail.desc": "轻量化设计，长续航表现，为远航提供稳定能量。",
        "products.industrial.tag": "蓄电池",
        "products.industrial.title": "工业蓄电池",
        "products.industrial.desc": "高循环寿命，稳定输出，适用于储能、UPS及工业设备。",
        "about.title": "二十年专注，<br/>只为一块好电池。",
        "about.p1": "VOLTA 成立于 2005 年，专注于蓄电池研发与制造。我们为全球客户提供汽车、游艇、帆船及工业领域的专业电池解决方案。",
        "about.p2": "从原材料到成品，每一道工序都经过严格品控。我们的产品通过多项国际认证，远销全球 60 多个国家和地区。",
        "about.stat1": "年行业经验",
        "about.stat2": "出口国家",
        "about.stat3": "年产量",
        "about.stat4": "客户满意度",
        "contact.title": "联系我们",
        "contact.subtitle": "无论是产品咨询、合作洽谈还是售后服务，我们随时为您提供支持。",
        "contact.email": "邮箱",
        "contact.phone": "电话",
        "contact.address": "地址",
        "contact.addressValue": "中国 · 广东省 · 深圳市 · 科技园",
        "contact.formName": "您的姓名",
        "contact.formEmail": "您的邮箱",
        "contact.formMessage": "留言内容",
        "contact.formSubmit": "发送信息",
        "footer.copyright": "© 2025 VOLTA. 保留所有权利。",
        "footer.privacy": "隐私政策",
        "footer.terms": "使用条款",
        "form.required": "请填写所有必填字段。",
        "form.emailInvalid": "请输入有效的邮箱地址。",
        "form.submitting": "正在发送…",
        "form.success": "发送成功，我们会尽快与您联系。",
        "form.error": "发送失败，请稍后重试。",
        "form.tooFast": "提交过于频繁，请稍后再试。"
      },
      en: {
        "nav.products": "Products",
        "nav.about": "About",
        "nav.contact": "Contact",
        "hero.eyebrow": "Professional Battery Manufacturing · Since 2005",
        "hero.title": "Power, born from expertise.",
        "hero.subtitle": "High-performance battery solutions for automotive, marine, sailboat and industrial applications. Trusted worldwide.",
        "products.title": "Product Series",
        "products.subtitle": "Professional and reliable battery solutions for every power need.",
        "products.auto.tag": "Automotive",
        "products.auto.title": "Starting Battery",
        "products.auto.desc": "High cold cranking amps and excellent low-temperature performance for every ignition.",
        "products.marine.tag": "Marine",
        "products.marine.title": "Marine Battery",
        "products.marine.desc": "Corrosion-resistant design with deep-cycle capability for harsh marine environments.",
        "products.sail.tag": "Sailboat",
        "products.sail.title": "Sailboat Battery",
        "products.sail.desc": "Lightweight design and long endurance for stable power on every voyage.",
        "products.industrial.tag": "Industrial",
        "products.industrial.title": "Industrial Battery",
        "products.industrial.desc": "Long cycle life and stable output for energy storage, UPS and industrial equipment.",
        "about.title": "Two decades of focus,<br/>for one perfect battery.",
        "about.p1": "Founded in 2005, VOLTA specializes in battery R&D and manufacturing. We provide professional battery solutions for automotive, marine, sailboat and industrial applications worldwide.",
        "about.p2": "From raw materials to finished products, every process undergoes strict quality control. Our products are certified internationally and exported to over 60 countries.",
        "about.stat1": "Years of Experience",
        "about.stat2": "Export Countries",
        "about.stat3": "Annual Output",
        "about.stat4": "Customer Satisfaction",
        "contact.title": "Contact Us",
        "contact.subtitle": "Whether for product inquiries, partnership or after-sales support, we are here to help.",
        "contact.email": "Email",
        "contact.phone": "Phone",
        "contact.address": "Address",
        "contact.addressValue": "Science Park, Shenzhen, Guangdong, China",
        "contact.formName": "Your Name",
        "contact.formEmail": "Your Email",
        "contact.formMessage": "Message",
        "contact.formSubmit": "Send Message",
        "footer.copyright": "© 2025 VOLTA. All rights reserved.",
        "footer.privacy": "Privacy Policy",
        "footer.terms": "Terms of Use",
        "form.required": "Please fill in all required fields.",
        "form.emailInvalid": "Please enter a valid email address.",
        "form.submitting": "Sending…",
        "form.success": "Sent successfully. We will contact you soon.",
        "form.error": "Failed to send. Please try again later.",
        "form.tooFast": "Too frequent. Please try again later."
      },
      es: {
        "nav.products": "Productos",
        "nav.about": "Empresa",
        "nav.contact": "Contacto",
        "hero.eyebrow": "Fabricación profesional de baterías · Desde 2005",
        "hero.title": "Potencia, nacida de la experiencia.",
        "hero.subtitle": "Soluciones de baterías de alto rendimiento para automoción, náutica, veleros e industria. Confianza global.",
        "products.title": "Serie de Productos",
        "products.subtitle": "Soluciones de baterías profesionales y fiables para cada necesidad de energía.",
        "products.auto.tag": "Automoción",
        "products.auto.title": "Batería de Arranque",
        "products.auto.desc": "Alta corriente de arranque en frío y excelente rendimiento a baja temperatura.",
        "products.marine.tag": "Náutica",
        "products.marine.title": "Batería Marina",
        "products.marine.desc": "Diseño resistente a la corrosión con capacidad de ciclo profundo para ambientes marinos.",
        "products.sail.tag": "Veleros",
        "products.sail.title": "Batería para Velero",
        "products.sail.desc": "Diseño ligero y larga autonomía para una energía estable en cada travesía.",
        "products.industrial.tag": "Industrial",
        "products.industrial.title": "Batería Industrial",
        "products.industrial.desc": "Larga vida de ciclo y salida estable para almacenamiento de energía, UPS y equipos industriales.",
        "about.title": "Dos décadas de enfoque,<br/>para una batería perfecta.",
        "about.p1": "Fundada en 2005, VOLTA se especializa en I+D y fabricación de baterías. Ofrecemos soluciones profesionales para automoción, náutica, veleros e industria.",
        "about.p2": "Desde las materias primas hasta el producto terminado, cada proceso pasa por un estricto control de calidad. Nuestros productos están certificados internacionalmente y se exportan a más de 60 países.",
        "about.stat1": "Años de Experiencia",
        "about.stat2": "Países de Exportación",
        "about.stat3": "Producción Anual",
        "about.stat4": "Satisfacción del Cliente",
        "contact.title": "Contáctenos",
        "contact.subtitle": "Ya sea para consultas de productos, colaboración o soporte postventa, estamos aquí para ayudarle.",
        "contact.email": "Correo",
        "contact.phone": "Teléfono",
        "contact.address": "Dirección",
        "contact.addressValue": "Parque Tecnológico, Shenzhen, Guangdong, China",
        "contact.formName": "Su Nombre",
        "contact.formEmail": "Su Correo",
        "contact.formMessage": "Mensaje",
        "contact.formSubmit": "Enviar Mensaje",
        "footer.copyright": "© 2025 VOLTA. Todos los derechos reservados.",
        "footer.privacy": "Política de Privacidad",
        "footer.terms": "Términos de Uso",
        "form.required": "Por favor complete todos los campos obligatorios.",
        "form.emailInvalid": "Por favor ingrese un correo válido.",
        "form.submitting": "Enviando…",
        "form.success": "Enviado correctamente. Le contactaremos pronto.",
        "form.error": "Error al enviar. Inténtelo más tarde.",
        "form.tooFast": "Demasiado frecuente. Inténtelo más tarde."
      },
      fr: {
        "nav.products": "Produits",
        "nav.about": "Entreprise",
        "nav.contact": "Contact",
        "hero.eyebrow": "Fabrication professionnelle de batteries · Depuis 2005",
        "hero.title": "La puissance, née de l'expertise.",
        "hero.subtitle": "Solutions de batteries hautes performances pour l'automobile, le nautisme, la voile et l'industrie. Confiance mondiale.",
        "products.title": "Gamme de Produits",
        "products.subtitle": "Des solutions de batteries professionnelles et fiables pour chaque besoin d'énergie.",
        "products.auto.tag": "Automobile",
        "products.auto.title": "Batterie de Démarrage",
        "products.auto.desc": "Courant de démarrage à froid élevé et excellentes performances à basse température.",
        "products.marine.tag": "Nautisme",
        "products.marine.title": "Batterie Marine",
        "products.marine.desc": "Conception résistante à la corrosion avec capacité de décharge profonde pour les environnements marins.",
        "products.sail.tag": "Voilier",
        "products.sail.title": "Batterie de Voilier",
        "products.sail.desc": "Conception légère et longue autonomie pour une énergie stable à chaque voyage.",
        "products.industrial.tag": "Industriel",
        "products.industrial.title": "Batterie Industrielle",
        "products.industrial.desc": "Longue durée de cycle et sortie stable pour le stockage d'énergie, les UPS et les équipements industriels.",
        "about.title": "Deux décennies de focus,<br/>pour une batterie parfaite.",
        "about.p1": "Fondée en 2005, VOLTA est spécialisée dans la R&D et la fabrication de batteries. Nous offrons des solutions professionnelles pour l'automobile, le nautisme, la voile et l'industrie.",
        "about.p2": "Des matières premières au produit fini, chaque processus fait l'objet d'un contrôle qualité strict. Nos produits sont certifiés internationalement et exportés dans plus de 60 pays.",
        "about.stat1": "Années d'Expérience",
        "about.stat2": "Pays d'Exportation",
        "about.stat3": "Production Annuelle",
        "about.stat4": "Satisfaction Client",
        "contact.title": "Contactez-Nous",
        "contact.subtitle": "Que ce soit pour des demandes de produits, un partenariat ou un support après-vente, nous sommes là pour vous aider.",
        "contact.email": "Email",
        "contact.phone": "Téléphone",
        "contact.address": "Adresse",
        "contact.addressValue": "Parc Technologique, Shenzhen, Guangdong, Chine",
        "contact.formName": "Votre Nom",
        "contact.formEmail": "Votre Email",
        "contact.formMessage": "Message",
        "contact.formSubmit": "Envoyer",
        "footer.copyright": "© 2025 VOLTA. Tous droits réservés.",
        "footer.privacy": "Politique de Confidentialité",
        "footer.terms": "Conditions d'Utilisation",
        "form.required": "Veuillez remplir tous les champs obligatoires.",
        "form.emailInvalid": "Veuillez saisir un email valide.",
        "form.submitting": "Envoi…",
        "form.success": "Envoyé avec succès. Nous vous contacterons bientôt.",
        "form.error": "Échec de l'envoi. Veuillez réessayer plus tard.",
        "form.tooFast": "Trop fréquent. Veuillez réessayer plus tard."
      },
      de: {
        "nav.products": "Produkte",
        "nav.about": "Unternehmen",
        "nav.contact": "Kontakt",
        "hero.eyebrow": "Professionelle Batterieherstellung · Seit 2005",
        "hero.title": "Kraft, geboren aus Expertise.",
        "hero.subtitle": "Hochleistungs-Batterielösungen für Automobil, Marine, Segelboote und Industrie. Weltweit vertraut.",
        "products.title": "Produktserie",
        "products.subtitle": "Professionelle und zuverlässige Batterielösungen für jeden Energiebedarf.",
        "products.auto.tag": "Automobil",
        "products.auto.title": "Starterbatterie",
        "products.auto.desc": "Hoher Kaltstartstrom und hervorragende Leistung bei niedrigen Temperaturen.",
        "products.marine.tag": "Marine",
        "products.marine.title": "Marinebatterie",
        "products.marine.desc": "Korrosionsbeständiges Design mit Deep-Cycle-Fähigkeit für raue Meeresumgebungen.",
        "products.sail.tag": "Segelboot",
        "products.sail.title": "Segelbootbatterie",
        "products.sail.desc": "Leichtes Design und lange Ausdauer für stabile Energie auf jeder Reise.",
        "products.industrial.tag": "Industrie",
        "products.industrial.title": "Industriebatterie",
        "products.industrial.desc": "Lange Zykluslebensdauer und stabile Leistung für Energiespeicher, USV und Industrieanlagen.",
        "about.title": "Zwei Jahrzehnte Fokus,<br/>für eine perfekte Batterie.",
        "about.p1": "VOLTA wurde 2005 gegründet und ist auf Batterie-F&E und -Herstellung spezialisiert. Wir bieten professionelle Lösungen für Automobil, Marine, Segelboote und Industrie.",
        "about.p2": "Von den Rohstoffen bis zum Fertigprodukt durchläuft jeder Prozess eine strenge Qualitätskontrolle. Unsere Produkte sind international zertifiziert und werden in über 60 Länder exportiert.",
        "about.stat1": "Jahre Erfahrung",
        "about.stat2": "Exportländer",
        "about.stat3": "Jahresproduktion",
        "about.stat4": "Kundenzufriedenheit",
        "contact.title": "Kontaktieren Sie Uns",
        "contact.subtitle": "Ob Produktanfragen, Partnerschaft oder After-Sales-Support – wir sind für Sie da.",
        "contact.email": "E-Mail",
        "contact.phone": "Telefon",
        "contact.address": "Adresse",
        "contact.addressValue": "Technologiepark, Shenzhen, Guangdong, China",
        "contact.formName": "Ihr Name",
        "contact.formEmail": "Ihre E-Mail",
        "contact.formMessage": "Nachricht",
        "contact.formSubmit": "Nachricht Senden",
        "footer.copyright": "© 2025 VOLTA. Alle Rechte vorbehalten.",
        "footer.privacy": "Datenschutz",
        "footer.terms": "Nutzungsbedingungen",
        "form.required": "Bitte füllen Sie alle Pflichtfelder aus.",
        "form.emailInvalid": "Bitte geben Sie eine gültige E-Mail-Adresse ein.",
        "form.submitting": "Wird gesendet…",
        "form.success": "Erfolgreich gesendet. Wir melden uns bald.",
        "form.error": "Senden fehlgeschlagen. Bitte später erneut versuchen.",
        "form.tooFast": "Zu häufig. Bitte später erneut versuchen."
      },
      ja: {
        "nav.products": "製品",
        "nav.about": "会社概要",
        "nav.contact": "お問い合わせ",
        "hero.eyebrow": "プロフェッショナル電池製造 · 2005年創業",
        "hero.title": "力は、専門性から生まれる。",
        "hero.subtitle": "自動車、マリン、セーリング、産業用の高性能バッテリーソリューション。世界中で信頼されています。",
        "products.title": "製品シリーズ",
        "products.subtitle": "あらゆる電力ニーズに応える、プロフェッショナルで信頼性の高いバッテリーソリューション。",
        "products.auto.tag": "自動車",
        "products.auto.title": "スターターバッテリー",
        "products.auto.desc": "高いコールドクランキングアンペアと優れた低温性能で、あらゆる始動を強力にサポート。",
        "products.marine.tag": "マリン",
        "products.marine.title": "マリンバッテリー",
        "products.marine.desc": "耐腐食設計とディープサイクル能力で、過酷な海洋環境に从容対応。",
        "products.sail.tag": "セーリング",
        "products.sail.title": "セーリングバッテリー",
        "products.sail.desc": "軽量設計と長い航続性能で、遠洋航海に安定したエネルギーを提供。",
        "products.industrial.tag": "産業用",
        "products.industrial.title": "産業用バッテリー",
        "products.industrial.desc": "長いサイクル寿命と安定した出力で、蓄電、UPS、産業機器に最適。",
        "about.title": "20年の集中、<br/>一つの完璧なバッテリーのために。",
        "about.p1": "VOLTAは2005年に設立され、バッテリーの研究開発と製造に専念しています。自動車、マリン、セーリング、産業分野のプロフェッショナルなバッテリーソリューションを世界のお客様に提供しています。",
        "about.p2": "原材料から完成品まで、すべての工程で厳格な品質管理を行っています。製品は国際認証を取得し、60カ国以上に輸出されています。",
        "about.stat1": "年の業界経験",
        "about.stat2": "輸出国",
        "about.stat3": "年間生産量",
        "about.stat4": "顧客満足度",
        "contact.title": "お問い合わせ",
        "contact.subtitle": "製品に関するご質問、パートナーシップ、アフターサポートなど、いつでもお気軽にご連絡ください。",
        "contact.email": "メール",
        "contact.phone": "電話",
        "contact.address": "住所",
        "contact.addressValue": "中国 · 広東省 · 深セン市 · 科技園",
        "contact.formName": "お名前",
        "contact.formEmail": "メールアドレス",
        "contact.formMessage": "メッセージ",
        "contact.formSubmit": "送信する",
        "footer.copyright": "© 2025 VOLTA. All rights reserved.",
        "footer.privacy": "プライバシーポリシー",
        "footer.terms": "利用規約",
        "form.required": "必須項目をすべて入力してください。",
        "form.emailInvalid": "有効なメールアドレスを入力してください。",
        "form.submitting": "送信中…",
        "form.success": "送信完了。折り返しご連絡いたします。",
        "form.error": "送信に失敗しました。後でもう一度お試しください。",
        "form.tooFast": "送信が頻繁すぎます。しばらくしてからお試しください。"
      }
    };

    /* ========== 语言切换逻辑 ========== */
    (function() {
      const switcher = document.getElementById('langSwitcher');
      const btn = document.getElementById('langBtn');
      const dropdown = document.getElementById('langDropdown');
      const options = dropdown.querySelectorAll('.lang-option');
      const currentFlag = document.getElementById('currentFlag');
      const currentLang = document.getElementById('currentLang');

      const flagMap = {
        zh: 'flag-cn', en: 'flag-us', es: 'flag-es',
        fr: 'flag-fr', de: 'flag-de', ja: 'flag-jp'
      };

      const langNameMap = {
        zh: '中文', en: 'English', es: 'Español',
        fr: 'Français', de: 'Deutsch', ja: '日本語'
      };

      // SECURITY: 语言白名单，防止通过 URL 或 localStorage 注入非法值
      const allowedLangs = Object.keys(translations);

      btn.addEventListener('click', function(e) {
        e.stopPropagation();
        const isOpen = switcher.classList.toggle('open');
        btn.setAttribute('aria-expanded', isOpen ? 'true' : 'false');
      });

      document.addEventListener('click', function(e) {
        if (!switcher.contains(e.target)) {
          switcher.classList.remove('open');
          btn.setAttribute('aria-expanded', 'false');
        }
      });

      options.forEach(function(option) {
        option.addEventListener('click', function() {
          const lang = this.dataset.lang;
          if (allowedLangs.indexOf(lang) === -1) return; // SECURITY: 白名单校验
          setLanguage(lang);
          switcher.classList.remove('open');
          btn.setAttribute('aria-expanded', 'false');
        });
      });

      function setLanguage(lang) {
        const t = translations[lang];
        if (!t) return;

        document.querySelectorAll('[data-i18n]').forEach(function(el) {
          const key = el.getAttribute('data-i18n');
          if (t[key] !== undefined) {
            // SECURITY: 使用 textContent 防止 XSS；仅对已知安全的 <br/> 使用 innerHTML
            if (t[key].indexOf('<br') !== -1) {
              el.innerHTML = t[key].replace(/<(?!br\s*\/?>)[^>]*>/gi, '');
            } else {
              el.textContent = t[key];
            }
          }
        });

        document.querySelectorAll('[data-i18n-placeholder]').forEach(function(el) {
          const key = el.getAttribute('data-i18n-placeholder');
          if (t[key] !== undefined) {
            el.placeholder = t[key];
          }
        });

        currentFlag.className = 'flag ' + flagMap[lang];
        currentLang.textContent = langNameMap[lang];

        options.forEach(function(opt) {
          opt.classList.toggle('active', opt.dataset.lang === lang);
        });

        document.documentElement.lang = lang;
        // SECURITY: 仅存储白名单内的语言值
        try { localStorage.setItem('preferredLang', lang); } catch (e) {}
      }

      // 恢复用户语言偏好（白名单校验）
      try {
        const saved = localStorage.getItem('preferredLang');
        if (saved && allowedLangs.indexOf(saved) !== -1) {
          setLanguage(saved);
        }
      } catch (e) {}

      window.setLanguage = setLanguage;
    })();

    /* ========== 导航栏滚动效果 ========== */
    (function() {
      const navbar = document.getElementById('navbar');
      window.addEventListener('scroll', function() {
        navbar.classList.toggle('scrolled', window.scrollY > 20);
      });
    })();

    /* ========== SECURITY: 表单安全处理 ========== */
    (function() {
      const form = document.getElementById('contactForm');
      const statusEl = document.getElementById('formStatus');
      const submitBtn = document.getElementById('submitBtn');
      let lastSubmitTime = 0;

      // 获取当前语言对应的提示文案
      function t(key) {
        const lang = document.documentElement.lang || 'zh';
        const dict = translations[lang] || translations.zh;
        return dict[key] || translations.zh[key] || key;
      }

      // SECURITY: 前端输入清理（后端必须再次校验）
      function sanitize(str) {
        return String(str)
          .replace(/[<>]/g, '')          // 去除尖括号
          .replace(/[\u0000-\u001F]/g, '') // 去除控制字符
          .trim()
          .slice(0, 2000);
      }

      function setStatus(msg, type) {
        statusEl.textContent = msg;
        statusEl.className = 'form-status' + (type ? ' ' + type : '');
      }

      form.addEventListener('submit', function(e) {
        e.preventDefault();

        // SECURITY: 速率限制（前端简易版，后端必须再做）
        const now = Date.now();
        if (now - lastSubmitTime < 5000) {
          setStatus(t('form.tooFast'), 'error');
          return;
        }

        // SECURITY: 蜜罐检测
        const honeypot = document.getElementById('website');
        if (honeypot && honeypot.value !== '') {
          // 机器人填写了隐藏字段，静默拒绝
          setStatus(t('form.success'), 'success');
          form.reset();
          return;
        }

        // 获取并清理输入
        const name = sanitize(document.getElementById('name').value);
        const email = sanitize(document.getElementById('email').value);
        const message = sanitize(document.getElementById('message').value);

        // SECURITY: 必填校验
        if (!name || !email || !message) {
          setStatus(t('form.required'), 'error');
          return;
        }

        // SECURITY: 邮箱格式校验
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;
        if (!emailRegex.test(email)) {
          setStatus(t('form.emailInvalid'), 'error');
          return;
        }

        // SECURITY: 长度校验（与 maxlength 双重保险）
        if (name.length > 100 || email.length > 254 || message.length > 2000) {
          setStatus(t('form.required'), 'error');
          return;
        }

        // 提交处理
        lastSubmitTime = now;
        submitBtn.disabled = true;
        setStatus(t('form.submitting'));

        // TODO: 替换为您的后端接口或表单服务
        // 示例：
        // fetch('/api/contact', {
        //   method: 'POST',
        //   headers: { 'Content-Type': 'application/json', 'X-CSRF-Token': getCsrfToken() },
        //   body: JSON.stringify({ name, email, message })
        // })
        // .then(res => { if (!res.ok) throw new Error(); return res.json(); })
        // .then(() => { setStatus(t('form.success'), 'success'); form.reset(); })
        // .catch(() => { setStatus(t('form.error'), 'error'); })
        // .finally(() => { submitBtn.disabled = false; });

        // 演示：模拟成功
        setTimeout(function() {
          setStatus(t('form.success'), 'success');
          form.reset();
          submitBtn.disabled = false;
        }, 800);
      });

      // 清除错误提示
      form.querySelectorAll('input, textarea').forEach(function(el) {
        el.addEventListener('input', function() {
          if (statusEl.classList.contains('error')) {
            setStatus('');
          }
        });
      });
    })();
  </script>
</body>
</html>
