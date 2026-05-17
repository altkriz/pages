<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minimal — A Markdown-Style Website</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://code.iconify.design/iconify-icon/1.0.7/iconify-icon.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&family=Playfair+Display:ital,wght@0,400;0,500;0,600;1,400&family=Bricolage+Grotesque:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            serif: ['Playfair Display', 'serif'],
            bricolage: ['Bricolage Grotesque', 'sans-serif'],
            mono: ['ui-monospace', 'SFMono-Regular', 'Menlo', 'Monaco', 'Consolas', 'monospace'],
          }
        }
      }
    }
  </script>
  <style>
    body { font-family: 'Inter', sans-serif; background: #0a0a0a; color: #fafafa; }

    /* Noise Texture */
    .noise-overlay {
      position: fixed; inset: 0; z-index: 9999; pointer-events: none; opacity: 0.04;
    }

    /* Scrollbar */
    ::-webkit-scrollbar { width: 6px; }
    ::-webkit-scrollbar-track { background: #0a0a0a; }
    ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.15); border-radius: 3px; }
    ::-webkit-scrollbar-thumb:hover { background: rgba(255,255,255,0.25); }

    /* Prose Styles */
    .md-content h1 { font-family: 'Bricolage Grotesque', sans-serif; font-size: 3rem; font-weight: 600; line-height: 1.05; letter-spacing: -0.03em; margin-bottom: 1.5rem; }
    .md-content h2 { font-family: 'Bricolage Grotesque', sans-serif; font-size: 1.875rem; font-weight: 600; line-height: 1.1; letter-spacing: -0.02em; margin-top: 3rem; margin-bottom: 1rem; padding-bottom: 0.75rem; border-bottom: 1px solid rgba(255,255,255,0.1); }
    .md-content h3 { font-family: 'Bricolage Grotesque', sans-serif; font-size: 1.35rem; font-weight: 500; margin-top: 2rem; margin-bottom: 0.75rem; }
    .md-content p { font-size: 1.125rem; font-weight: 300; line-height: 1.7; margin-bottom: 1.25rem; color: #d4d4d4; }
    .md-content strong { color: #fafafa; font-weight: 500; }
    .md-content em { font-family: 'Playfair Display', serif; font-style: italic; color: #e7e5e4; }
    .md-content a { color: #10b981; text-decoration: underline; text-underline-offset: 3px; text-decoration-color: rgba(16,185,129,0.3); transition: all 0.3s; }
    .md-content a:hover { text-decoration-color: #10b981; }

    .md-content ul { list-style: none; padding-left: 0; margin-bottom: 1.25rem; }
    .md-content ul li { position: relative; padding-left: 1.5rem; margin-bottom: 0.5rem; font-size: 1.125rem; font-weight: 300; line-height: 1.7; color: #d4d4d4; }
    .md-content ul li::before { content: '→'; position: absolute; left: 0; color: #10b981; font-size: 0.875rem; top: 0.15em; }

    .md-content ol { list-style: none; padding-left: 0; margin-bottom: 1.25rem; counter-reset: ol-counter; }
    .md-content ol li { position: relative; padding-left: 2.5rem; margin-bottom: 0.5rem; font-size: 1.125rem; font-weight: 300; line-height: 1.7; color: #d4d4d4; counter-increment: ol-counter; }
    .md-content ol li::before { content: counter(ol-counter, decimal-leading-zero); position: absolute; left: 0; color: #10b981; font-family: ui-monospace, monospace; font-size: 0.875rem; top: 0.2em; font-weight: 500; }

    .md-content blockquote { position: relative; padding: 1.25rem 1.5rem; margin: 1.5rem 0; background: rgba(16,185,129,0.05); border-left: 3px solid #10b981; border-radius: 0 1rem 1rem 0; }
    .md-content blockquote p { color: #a3a3a3; font-style: italic; margin-bottom: 0; }
    .md-content blockquote p strong { color: #10b981; font-style: normal; }

    .md-content pre { background: #0d0d0d; border: 1px solid rgba(255,255,255,0.08); border-radius: 1rem; padding: 1.5rem; margin: 1.5rem 0; overflow-x: auto; position: relative; }
    .md-content pre code { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace; font-size: 0.875rem; line-height: 1.8; color: #d4d4d4; }
    .md-content pre .lang-tag { position: absolute; top: 0.75rem; right: 1rem; font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: rgba(255,255,255,0.25); font-family: ui-monospace, monospace; }
    .md-content code:not(pre code) { background: rgba(16,185,129,0.1); color: #10b981; padding: 0.15em 0.4em; border-radius: 0.375rem; font-family: ui-monospace, monospace; font-size: 0.85em; }

    .md-content table { width: 100%; margin: 1.5rem 0; border-collapse: separate; border-spacing: 0; border-radius: 1rem; overflow: hidden; border: 1px solid rgba(255,255,255,0.08); }
    .md-content thead th { background: rgba(255,255,255,0.03); text-align: left; padding: 0.875rem 1.25rem; font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.1em; color: #a3a3a3; font-weight: 500; border-bottom: 1px solid rgba(255,255,255,0.08); }
    .md-content tbody td { padding: 0.875rem 1.25rem; font-size: 1rem; font-weight: 300; color: #d4d4d4; border-bottom: 1px solid rgba(255,255,255,0.04); }
    .md-content tbody tr:last-child td { border-bottom: none; }
    .md-content tbody tr:hover { background: rgba(255,255,255,0.02); }

    .md-content hr { border: none; height: 1px; background: rgba(255,255,255,0.08); margin: 3rem 0; position: relative; }
    .md-content hr::after { content: '§'; position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%); background: #0a0a0a; padding: 0 1rem; color: rgba(255,255,255,0.15); font-size: 1.25rem; }

    .md-content img { width: 100%; border-radius: 1rem; margin: 1.5rem 0; border: 1px solid rgba(255,255,255,0.08); }

    /* Sidebar */
    .sidebar-link { display: flex; align-items: center; gap: 0.5rem; padding: 0.5rem 0.75rem; border-radius: 0.5rem; font-size: 0.875rem; color: #737373; transition: all 0.3s; text-decoration: none; }
    .sidebar-link:hover { color: #fafafa; background: rgba(255,255,255,0.05); }
    .sidebar-link.active { color: #10b981; background: rgba(16,185,129,0.08); }
    .sidebar-link iconify-icon { font-size: 1rem; opacity: 0.6; }

    /* Toast */
    .toast { position: fixed; bottom: 2rem; right: 2rem; background: #10b981; color: #fff; padding: 0.75rem 1.25rem; border-radius: 0.75rem; font-size: 0.875rem; font-weight: 500; transform: translateY(120%); opacity: 0; transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1); z-index: 100; pointer-events: none; }
    .toast.show { transform: translateY(0); opacity: 1; pointer-events: auto; }

    /* Animations */
    @keyframes cinematicEntrance {
        0% { transform: translateY(40px); opacity: 0; }
        100% { transform: translateY(0); opacity: 1; }
    }
    .animate-entry { opacity: 0; animation: cinematicEntrance 1s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
    .delay-1 { animation-delay: 0.15s; }
    .delay-2 { animation-delay: 0.3s; }
    .delay-3 { animation-delay: 0.45s; }
    .delay-4 { animation-delay: 0.6s; }
    .delay-5 { animation-delay: 0.75s; }

    @keyframes pulse-glow {
        0%, 100% { opacity: 0.4; }
        50% { opacity: 0.8; }
    }

    .copy-btn { position: absolute; top: 0.75rem; right: 3.5rem; background: rgba(255,255,255,0.06); border: 1px solid rgba(255,255,255,0.1); color: #737373; border-radius: 0.5rem; padding: 0.35rem 0.6rem; font-size: 0.7rem; cursor: pointer; transition: all 0.3s; font-family: ui-monospace, monospace; text-transform: uppercase; letter-spacing: 0.05em; }
    .copy-btn:hover { background: rgba(255,255,255,0.1); color: #fafafa; }

    /* Mobile sidebar */
    @media (max-width: 1023px) {
      .sidebar-wrapper { display: none; }
      .sidebar-wrapper.open { display: block; position: fixed; inset: 0; z-index: 50; background: rgba(0,0,0,0.8); backdrop-filter: blur(24px); }
      .sidebar-wrapper.open .sidebar-inner { margin: 2rem; }
    }
  </style>
</head>
<body class="antialiased">

  <!-- Noise Overlay -->
  <svg class="noise-overlay" width="100%" height="100%">
    <filter id="noise"><feTurbulence type="fractalNoise" baseFrequency="0.8" numOctaves="3" stitchTiles="stitch"/></filter>
    <rect width="100%" height="100%" filter="url(#noise)"/>
  </svg>

  <!-- Navigation -->
  <nav class="fixed top-6 left-1/2 -translate-x-1/2 z-50 animate-entry">
    <div class="flex items-center gap-1 bg-neutral-900/80 backdrop-blur-2xl border border-white/10 rounded-full pl-2 pr-4 py-2 shadow-2xl">
      <a href="#" class="flex items-center gap-2 bg-white rounded-full px-3 py-1.5">
        <iconify-icon icon="mdi:language-markdown" style="color:#10b981; font-size:1.1rem;"></iconify-icon>
        <span class="font-bricolage font-medium text-sm text-black tracking-tight">minimal.md</span>
      </a>
      <div class="hidden md:flex items-center gap-0.5 ml-2">
        <a href="#intro" class="px-3 py-1.5 text-sm text-white/60 hover:text-white transition-colors rounded-full hover:bg-white/5">Intro</a>
        <a href="#features" class="px-3 py-1.5 text-sm text-white/60 hover:text-white transition-colors rounded-full hover:bg-white/5">Features</a>
        <a href="#code" class="px-3 py-1.5 text-sm text-white/60 hover:text-white transition-colors rounded-full hover:bg-white/5">Code</a>
        <a href="#data" class="px-3 py-1.5 text-sm text-white/60 hover:text-white transition-colors rounded-full hover:bg-white/5">Data</a>
      </div>
      <button id="mobileMenuBtn" class="lg:hidden ml-2 p-1.5 text-white/60 hover:text-white transition-colors">
        <iconify-icon icon="mdi:menu" style="font-size:1.2rem;"></iconify-icon>
      </button>
    </div>
  </nav>

  <!-- Mobile Sidebar Overlay -->
  <div id="mobileSidebar" class="sidebar-wrapper lg:hidden">
    <div class="sidebar-inner">
      <div class="flex justify-between items-center mb-6">
        <span class="font-bricolage font-medium text-lg">Navigation</span>
        <button id="closeSidebar" class="text-white/60 hover:text-white p-2">
          <iconify-icon icon="mdi:close" style="font-size:1.25rem;"></iconify-icon>
        </button>
      </div>
      <div class="flex flex-col gap-1" id="mobileNav"></div>
    </div>
  </div>

  <!-- Main Layout -->
  <div class="max-w-[90rem] mx-auto px-6 lg:px-8 flex gap-8">

    <!-- Sidebar -->
    <aside class="sidebar-wrapper hidden lg:block w-64 shrink-0">
      <div class="sticky top-32">
        <div class="mb-6">
          <p class="text-xs uppercase tracking-widest text-white/25 font-mono mb-3 px-3">On this page</p>
          <div class="flex flex-col gap-0.5" id="sidebarNav">
            <a href="#intro" class="sidebar-link active" data-section="intro">
              <iconify-icon icon="mdi:text"></iconify-icon> Introduction
            </a>
            <a href="#features" class="sidebar-link" data-section="features">
              <iconify-icon icon="mdi:star-four-points"></iconify-icon> Features
            </a>
            <a href="#code" class="sidebar-link" data-section="code">
              <iconify-icon icon="mdi:code-braces"></iconify-icon> Code Blocks
            </a>
            <a href="#data" class="sidebar-link" data-section="data">
              <iconify-icon icon="mdi:table"></iconify-icon> Data Tables
            </a>
            <a href="#media" class="sidebar-link" data-section="media">
              <iconify-icon icon="mdi:image"></iconify-icon> Media
            </a>
            <a href="#quotes" class="sidebar-link" data-section="quotes">
              <iconify-icon icon="mdi:format-quote-close"></iconify-icon> Blockquotes
            </a>
          </div>
        </div>
        <div class="border-t border-white/5 pt-4 mt-4">
          <p class="text-xs uppercase tracking-widest text-white/25 font-mono mb-3 px-3">Resources</p>
          <div class="flex flex-col gap-0.5">
            <a href="#" class="sidebar-link"><iconify-icon icon="mdi:github"></iconify-icon> GitHub</a>
            <a href="#" class="sidebar-link"><iconify-icon icon="mdi:file-document"></iconify-icon> Docs</a>
            <a href="#" class="sidebar-link"><iconify-icon icon="mdi:palette-swatch"></iconify-icon> Theme</a>
          </div>
        </div>
        <!-- Reading progress -->
        <div class="mt-8 px-3">
          <div class="flex justify-between items-center mb-2">
            <span class="text-xs text-white/25 font-mono uppercase tracking-widest">Progress</span>
            <span class="text-xs text-white/40 font-mono" id="progressPercent">0%</span>
          </div>
          <div class="h-1 bg-white/5 rounded-full overflow-hidden">
            <div id="progressBar" class="h-full bg-emerald-500 rounded-full transition-all duration-300" style="width: 0%"></div>
          </div>
        </div>
      </div>
    </aside>

    <!-- Content -->
    <main class="flex-1 min-w-0 pt-32 pb-24">
      <div class="max-w-3xl">

        <!-- Hero Badge -->
        <div class="animate-entry mb-8">
          <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full border border-white/10 bg-white/5">
            <span class="w-2 h-2 rounded-full bg-emerald-500" style="animation: pulse-glow 2s ease-in-out infinite;"></span>
            <span class="text-xs font-mono uppercase tracking-widest text-white/60">Last updated — June 2025</span>
          </div>
        </div>

        <!-- Markdown Content -->
        <article class="md-content">

          <h1 class="animate-entry delay-1">Build something<br><em class="font-serif text-white/70">beautiful</em> with markdown</h1>

          <p class="animate-entry delay-2">This entire page is styled like a rendered <code>.md</code> file — but elevated. Every Markdown element you know and love, reimagined with purposeful typography, careful spacing, and a dark aesthetic that makes content <strong>feel different</strong>.</p>

          <p class="animate-entry delay-3">Below you'll find every common Markdown element, styled to match our design system. Use this as a reference, a template, or simply as inspiration for your own projects.</p>

          <!-- Section: Introduction -->
          <h2 id="intro" class="animate-entry delay-4">Introduction</h2>

          <p>Markdown was created by <a href="#">John Gruber</a> in 2004 as a lightweight markup language. Its philosophy is simple: a document should be readable as plain text <em>before</em> it's rendered. That elegance carries through when the rendering is done right.</p>

          <p>The key principles are straightforward:</p>

          <ul>
            <li><strong>Readability first</strong> — the source should be as readable as the output</li>
            <li><strong>Minimal syntax</strong> — formatting should be intuitive, not verbose</li>
            <li><strong>Platform agnostic</strong> — works everywhere, from GitHub to VS Code</li>
            <li><strong>Extensible</strong> — flavors like CommonMark and GHFM add power without complexity</li>
          </ul>

          <hr>

          <!-- Section: Features -->
          <h2 id="features">Features</h2>

          <p>This template supports the full range of Markdown elements, each carefully styled:</p>

          <h3>Typography</h3>

          <p>You can write in <strong>bold</strong>, <em>italic</em>, or even <strong><em>bold italic</em></strong>. Inline <code>code snippets</code> stand out with a subtle emerald tint. Links like <a href="#">this one</a> use a gentle underline that intensifies on hover.</p>

          <h3>Ordered Lists</h3>

          <p>When sequence matters:</p>

          <ol>
            <li>Clone the repository to your local machine</li>
            <li>Install dependencies with <code>npm install</code></li>
            <li>Configure your environment variables</li>
            <li>Run the development server</li>
            <li>Open your browser to <code>localhost:3000</code></li>
          </ol>

          <h3>Task Lists</h3>

          <div class="space-y-2 my-5">
            <label class="flex items-center gap-3 cursor-pointer group">
              <input type="checkbox" checked class="sr-only peer">
              <div class="w-5 h-5 rounded border border-white/15 flex items-center justify-center peer-checked:bg-emerald-500 peer-checked:border-emerald-500 transition-all">
                <iconify-icon icon="mdi:check" class="text-black text-xs peer-checked:opacity-100 opacity-0"></iconify-icon>
              </div>
              <span class="text-lg font-light text-neutral-400 line-through">Design the layout system</span>
            </label>
            <label class="flex items-center gap-3 cursor-pointer group">
              <input type="checkbox" checked class="sr-only peer">
              <div class="w-5 h-5 rounded border border-white/15 flex items-center justify-center peer-checked:bg-emerald-500 peer-checked:border-emerald-500 transition-all">
                <iconify-icon icon="mdi:check" class="text-black text-xs"></iconify-icon>
              </div>
              <span class="text-lg font-light text-neutral-400 line-through">Implement dark mode</span>
            </label>
            <label class="flex items-center gap-3 cursor-pointer group">
              <input type="checkbox" class="sr-only peer">
              <div class="w-5 h-5 rounded border border-white/15 flex items-center justify-center peer-checked:bg-emerald-500 peer-checked:border-emerald-500 transition-all">
                <iconify-icon icon="mdi:check" class="text-black text-xs opacity-0"></iconify-icon>
              </div>
              <span class="text-lg font-light text-neutral-300">Add animation library</span>
            </label>
            <label class="flex items-center gap-3 cursor-pointer group">
              <input type="checkbox" class="sr-only peer">
              <div class="w-5 h-5 rounded border border-white/15 flex items-center justify-center peer-checked:bg-emerald-500 peer-checked:border-emerald-500 transition-all">
                <iconify-icon icon="mdi:check" class="text-black text-xs opacity-0"></iconify-icon>
              </div>
              <span class="text-lg font-light text-neutral-300">Deploy to production</span>
            </label>
          </div>

          <hr>

          <!-- Section: Code -->
          <h2 id="code">Code Blocks</h2>

          <p>Code is a first-class citizen. Inline code like <code>const x = 42</code> blends into paragraphs, while blocks get their own stage:</p>

          <pre><span class="lang-tag">javascript</span><button class="copy-btn" onclick="copyCode(this)">Copy</button><code><span style="color:#c792ea">const</span> <span style="color:#82aaff">createApp</span> <span style="color:#89ddff">=</span> <span style="color:#c792ea">async</span> (<span style="color:#f78c6c">config</span>) <span style="color:#89ddff">=></span> {
  <span style="color:#c792ea">const</span> app <span style="color:#89ddff">=</span> <span style="color:#82aaff">initialize</span>(config);
  
  <span style="color:#676e95;font-style:italic">// Register middleware</span>
  app.<span style="color:#82aaff">use</span>(logger());
  app.<span style="color:#82aaff">use</span>(compress());
  app.<span style="color:#82aaff">use</span>(cors({ <span style="color:#f78c6c">origin</span>: <span style="color:#c3e88d">'*'</span> }));

  <span style="color:#c792ea">await</span> app.<span style="color:#82aaff">listen</span>(<span style="color:#f78c6c">3000</span>);
  <span style="color:#82aaff">console</span>.<span style="color:#82aaff">log</span>(<span style="color:#c3e88d">'🚀 Ready at localhost:3000'</span>);
};</code></pre>

          <p>And here's a CSS example — notice the syntax highlighting:</p>

          <pre><span class="lang-tag">css</span><button class="copy-btn" onclick="copyCode(this)">Copy</button><code><span style="color:#c792ea">.glow-effect</span> {
  <span style="color:#f78c6c">position</span>: <span style="color:#c3e88d">relative</span>;
  <span style="color:#f78c6c">background</span>: <span style="color:#82aaff">radial-gradient</span>(
    <span style="color:#f78c6c">ellipse</span> at <span style="color:#f78c6c">top</span>,
    <span style="color:#c3e88d">rgba(16, 185, 129, 0.15)</span>,
    <span style="color:#f78c6c">transparent</span> <span style="color:#f78c6c">70%</span>
  );
  <span style="color:#f78c6c">filter</span>: <span style="color:#82aaff">blur</span>(<span style="color:#f78c6c">40px</span>);
}</code></pre>

          <p>Shell commands feel right at home too:</p>

          <pre><span class="lang-tag">bash</span><button class="copy-btn" onclick="copyCode(this)">Copy</button><code><span style="color:#676e95">$</span> <span style="color:#82aaff">npx</span> create-minimal-app my-project
<span style="color:#676e95">$</span> <span style="color:#82aaff">cd</span> my-project
<span style="color:#676e95">$</span> <span style="color:#82aaff">npm</span> run dev

  <span style="color:#c3e88d">✓</span> Ready on http://localhost:3000</code></pre>

          <hr>

          <!-- Section: Data Tables -->
          <h2 id="data">Data Tables</h2>

          <p>Structured data deserves structured presentation:</p>

          <table>
            <thead>
              <tr>
                <th>Feature</th>
                <th>Status</th>
                <th>Version</th>
                <th>Priority</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Dark mode</td>
                <td><span class="inline-flex items-center gap-1.5 text-emerald-400"><span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span>Shipped</span></td>
                <td><code>v2.1.0</code></td>
                <td>—</td>
              </tr>
              <tr>
                <td>Animations</td>
                <td><span class="inline-flex items-center gap-1.5 text-emerald-400"><span class="w-1.5 h-1.5 rounded-full bg-emerald-400"></span>Shipped</span></td>
                <td><code>v2.2.0</code></td>
                <td>—</td>
              </tr>
              <tr>
                <td>i18n support</td>
                <td><span class="inline-flex items-center gap-1.5 text-amber-400"><span class="w-1.5 h-1.5 rounded-full bg-amber-400"></span>In progress</span></td>
                <td><code>v2.3.0</code></td>
                <td>High</td>
              </tr>
              <tr>
                <td>Plugin API</td>
                <td><span class="inline-flex items-center gap-1.5 text-neutral-500"><span class="w-1.5 h-1.5 rounded-full bg-neutral-500"></span>Planned</span></td>
                <td><code>v3.0.0</code></td>
                <td>Medium</td>
              </tr>
              <tr>
                <td>Edge runtime</td>
                <td><span class="inline-flex items-center gap-1.5 text-neutral-500"><span class="w-1.5 h-1.5 rounded-full bg-neutral-500"></span>Planned</span></td>
                <td><code>v3.0.0</code></td>
                <td>Low</td>
              </tr>
            </tbody>
          </table>

          <p>Tables automatically adapt with hover states and clean dividers. No extra classes needed.</p>

          <hr>

          <!-- Section: Media -->
          <h2 id="media">Media</h2>

          <p>Images render at full width with rounded corners and a subtle border:</p>

          <img src="https://picsum.photos/seed/minimal-dark/1200/600.jpg" alt="A moody architectural photograph" loading="lazy">

          <p><em>Architecture as metaphor — structure reveals intention.</em></p>

          <p>You can also use side-by-side layouts with a bit of HTML:</p>

          <div class="grid grid-cols-2 gap-4 my-6">
            <img src="https://picsum.photos/seed/dark-abstract/600/400.jpg" alt="Abstract dark image 1" class="rounded-2xl border border-white/8" loading="lazy">
            <img src="https://picsum.photos/seed/emerald-glow/600/400.jpg" alt="Abstract dark image 2" class="rounded-2xl border border-white/8" loading="lazy">
          </div>

          <hr>

          <!-- Section: Blockquotes -->
          <h2 id="quotes">Blockquotes</h2>

          <p>For emphasis, for wisdom, for calls to action:</p>

          <blockquote>
            <p><strong>Tip:</strong> The best documentation is the one you actually read. Write for humans, not for parsers.</p>
          </blockquote>

          <blockquote>
            <p><strong>Warning:</strong> Never deploy on a Friday. This is not a suggestion — it's a survival strategy.</p>
          </blockquote>

          <p>Blockquotes can also nest other elements:</p>

          <blockquote>
            <p>As <em>Donald Knuth</em> once said:</p>
            <p>"Premature optimization is the root of all evil." — or at least, that's how we justify our O(n²) algorithms.</p>
          </blockquote>

          <hr>

          <!-- Collapsible Section -->
          <h2>Details & Summary</h2>

          <details class="group my-6 border border-white/8 rounded-2xl overflow-hidden">
            <summary class="flex items-center justify-between px-6 py-4 cursor-pointer hover:bg-white/[0.02] transition-colors">
              <span class="text-lg font-bricolage font-medium">What technologies are used?</span>
              <iconify-icon icon="mdi:chevron-down" class="text-white/40 transition-transform duration-300 group-open:rotate-180" style="font-size:1.25rem;"></iconify-icon>
            </summary>
            <div class="px-6 pb-5 border-t border-white/5 pt-4">
              <p class="mb-0">This template uses <strong>Tailwind CSS</strong> via CDN, <strong>Iconify</strong> for icons, and custom CSS for the Markdown prose styles. No build step required — just open the HTML file.</p>
            </div>
          </details>

          <details class="group my-6 border border-white/8 rounded-2xl overflow-hidden">
            <summary class="flex items-center justify-between px-6 py-4 cursor-pointer hover:bg-white/[0.02] transition-colors">
              <span class="text-lg font-bricolage font-medium">Can I customize the accent color?</span>
              <iconify-icon icon="mdi:chevron-down" class="text-white/40 transition-transform duration-300 group-open:rotate-180" style="font-size:1.25rem;"></iconify-icon>
            </summary>
            <div class="px-6 pb-5 border-t border-white/5 pt-4">
              <p class="mb-0">Absolutely. Replace <code>#10b981</code> (emerald-500) with any color you like throughout the CSS. The entire design token system is defined at the top of the <code>&lt;style&gt;</code> block.</p>
            </div>
          </details>

          <details class="group my-6 border border-white/8 rounded-2xl overflow-hidden">
            <summary class="flex items-center justify-between px-6 py-4 cursor-pointer hover:bg-white/[0.02] transition-colors">
              <span class="text-lg font-bricolage font-medium">Is this production-ready?</span>
              <iconify-icon icon="mdi:chevron-down" class="text-white/40 transition-transform duration-300 group-open:rotate-180" style="font-size:1.25rem;"></iconify-icon>
            </summary>
            <div class="px-6 pb-5 border-t border-white/5 pt-4">
              <p class="mb-0">This is a design template. For production, you'd want to minify the CSS, optimize images, add proper meta tags, and integrate a real Markdown parser like <a href="#">marked.js</a> or <a href="#">remark</a>.</p>
            </div>
          </details>

          <hr>

          <!-- Final Section -->
          <h2>Getting Started</h2>

          <p>Ready to build your own? It takes just three steps:</p>

          <ol>
            <li>Copy the HTML source of this page</li>
            <li>Replace the content with your own Markdown-inspired text</li>
            <li>Customize the design tokens to match your brand</li>
          </ol>

          <p>That's it. No frameworks, no build tools, no dependencies beyond a CDN. Just <em>clean, readable, beautiful</em> content.</p>

          <blockquote>
            <p><strong>Remember:</strong> Good design is invisible. Great design makes you <em>feel</em> something before you read a single word.</p>
          </blockquote>

          <!-- Footer Card -->
          <div class="mt-16 p-8 rounded-3xl border border-white/8 bg-gradient-to-br from-white/[0.03] to-transparent relative overflow-hidden">
            <div class="absolute top-0 right-0 w-64 h-64 bg-emerald-500/5 rounded-full blur-3xl"></div>
            <div class="relative">
              <p class="text-xs font-mono uppercase tracking-widest text-white/30 mb-4">End of document</p>
              <h3 class="font-bricolage text-2xl font-semibold mb-3">Start writing today</h3>
              <p class="text-neutral-400 font-light text-lg mb-6">Take this template and make it yours. The best way to learn is to build.</p>
              <div class="flex flex-wrap gap-3">
                <a href="#" class="inline-flex items-center gap-2 bg-white text-black px-6 py-3 rounded-full font-medium text-sm hover:bg-white/90 transition-colors">
                  <iconify-icon icon="mdi:download" style="font-size:1rem;"></iconify-icon>
                  Download Template
                </a>
                <a href="#" class="inline-flex items-center gap-2 bg-white/5 border border-white/10 text-white px-6 py-3 rounded-full font-medium text-sm hover:bg-white/10 transition-colors">
                  <iconify-icon icon="mdi:github" style="font-size:1rem;"></iconify-icon>
                  View Source
                </a>
              </div>
            </div>
          </div>

        </article>
      </div>
    </main>

    <!-- Right Side: Outline / Metadata -->
    <aside class="hidden xl:block w-56 shrink-0">
      <div class="sticky top-32">
        <p class="text-xs uppercase tracking-widest text-white/25 font-mono mb-3">Metadata</p>
        <div class="space-y-3 text-sm">
          <div class="flex justify-between">
            <span class="text-white/40">Words</span>
            <span class="text-white/70 font-mono">~1,240</span>
          </div>
          <div class="flex justify-between">
            <span class="text-white/40">Read time</span>
            <span class="text-white/70 font-mono">6 min</span>
          </div>
          <div class="flex justify-between">
            <span class="text-white/40">Sections</span>
            <span class="text-white/70 font-mono">7</span>
          </div>
          <div class="border-t border-white/5 my-3"></div>
          <div class="flex justify-between">
            <span class="text-white/40">Author</span>
            <span class="text-white/70">Minimal</span>
          </div>
          <div class="flex justify-between">
            <span class="text-white/40">License</span>
            <span class="text-emerald-400">MIT</span>
          </div>
        </div>

        <div class="mt-8">
          <p class="text-xs uppercase tracking-widest text-white/25 font-mono mb-3">Tags</p>
          <div class="flex flex-wrap gap-2">
            <span class="px-2.5 py-1 rounded-full bg-white/5 border border-white/8 text-xs text-white/50">markdown</span>
            <span class="px-2.5 py-1 rounded-full bg-white/5 border border-white/8 text-xs text-white/50">design</span>
            <span class="px-2.5 py-1 rounded-full bg-white/5 border border-white/8 text-xs text-white/50">dark-mode</span>
            <span class="px-2.5 py-1 rounded-full bg-white/5 border border-white/8 text-xs text-white/50">typography</span>
            <span class="px-2.5 py-1 rounded-full bg-white/5 border border-white/8 text-xs text-white/50">tailwind</span>
          </div>
        </div>

        <!-- Share -->
        <div class="mt-8">
          <p class="text-xs uppercase tracking-widest text-white/25 font-mono mb-3">Share</p>
          <div class="flex gap-2">
            <button onclick="showToast('Link copied to clipboard!')" class="w-9 h-9 rounded-full bg-white/5 border border-white/8 flex items-center justify-center text-white/40 hover:text-white hover:bg-white/10 transition-all">
              <iconify-icon icon="mdi:link-variant" style="font-size:1rem;"></iconify-icon>
            </button>
            <button onclick="showToast('Opening Twitter...')" class="w-9 h-9 rounded-full bg-white/5 border border-white/8 flex items-center justify-center text-white/40 hover:text-white hover:bg-white/10 transition-all">
              <iconify-icon icon="mdi:twitter" style="font-size:1rem;"></iconify-icon>
            </button>
            <button onclick="showToast('Opening GitHub...')" class="w-9 h-9 rounded-full bg-white/5 border border-white/8 flex items-center justify-center text-white/40 hover:text-white hover:bg-white/10 transition-all">
              <iconify-icon icon="mdi:github" style="font-size:1rem;"></iconify-icon>
            </button>
          </div>
        </div>
      </div>
    </aside>

  </div>

  <!-- Toast -->
  <div id="toast" class="toast"></div>

  <script>
    // Toast notification
    function showToast(message) {
      const toast = document.getElementById('toast');
      toast.textContent = message;
      toast.classList.add('show');
      setTimeout(() => toast.classList.remove('show'), 2500);
    }

    // Copy code
    function copyCode(btn) {
      const pre = btn.parentElement;
      const code = pre.querySelector('code');
      const text = code.textContent;
      navigator.clipboard.writeText(text).then(() => {
        btn.textContent = 'Copied!';
        setTimeout(() => btn.textContent = 'Copy', 2000);
      });
    }

    // Sidebar active state on scroll
    const sections = document.querySelectorAll('h2[id]');
    const sidebarLinks = document.querySelectorAll('#sidebarNav .sidebar-link');

    function updateActiveSection() {
      let current = '';
      sections.forEach(section => {
        const rect = section.getBoundingClientRect();
        if (rect.top <= 150) current = section.id;
      });
      sidebarLinks.forEach(link => {
        link.classList.toggle('active', link.dataset.section === current);
      });
    }

    // Reading progress
    function updateProgress() {
      const scrollTop = window.scrollY;
      const docHeight = document.documentElement.scrollHeight - window.innerHeight;
      const progress = Math.min((scrollTop / docHeight) * 100, 100);
      document.getElementById('progressBar').style.width = progress + '%';
      document.getElementById('progressPercent').textContent = Math.round(progress) + '%';
    }

    window.addEventListener('scroll', () => {
      updateActiveSection();
      updateProgress();
    });

    // Mobile menu
    document.getElementById('mobileMenuBtn').addEventListener('click', () => {
      const sidebar = document.getElementById('mobileSidebar');
      sidebar.classList.add('open');
      // Populate mobile nav
      const mobileNav = document.getElementById('mobileNav');
      mobileNav.innerHTML = '';
      sidebarLinks.forEach(link => {
        const clone = link.cloneNode(true);
        clone.addEventListener('click', () => sidebar.classList.remove('open'));
        mobileNav.appendChild(clone);
      });
    });

    document.getElementById('closeSidebar').addEventListener('click', () => {
      document.getElementById('mobileSidebar').classList.remove('open');
    });

    // Smooth scroll for anchor links
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
      anchor.addEventListener('click', function(e) {
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
          e.preventDefault();
          target.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
      });
    });

    // Initialize
    updateActiveSection();
    updateProgress();
  </script>

</body>
</html>
