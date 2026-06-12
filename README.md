<!-- Elfsight Announcement Bar | Ads -->
<script src="https://elfsightcdn.com/platform.js" async></script>
<div class="elfsight-app-da4c4e26-f1fe-4865-98e5-07ab2384d659" data-elfsight-app-lazy></div>




<!-- Elfsight Popup | Untitled Popup -->
<script src="https://elfsightcdn.com/platform.js" async></script>
<div class="elfsight-app-91604bc2-39c7-4e66-8173-ead1fe86bfbc" data-elfsight-app-lazy></div>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DeBeatzGH | Smart Overlay</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --accent: #00f2ff;
            --glass: rgba(15, 15, 20, 0.8);
            --border: rgba(255, 255, 255, 0.1);
        }

        body { background: #050507; font-family: 'Plus Jakarta Sans', sans-serif; height: 200vh; }

        /* --- 1. TRANSPARENT FLOATING BUTTON --- */
        #home-trigger {
            position: fixed; bottom: 30px; left: 30px;
            width: 55px; height: 55px;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid var(--border);
            border-radius: 50%;
            display: flex; align-items: center; justify-content: center;
            cursor: pointer; z-index: 10000;
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        #home-trigger:hover { border-color: var(--accent); background: rgba(0, 242, 255, 0.1); transform: scale(1.1); }

        /* --- 2. SCROLL INDICATOR ICONS --- */
        .scroll-icon {
            position: fixed; bottom: 95px; left: 47px;
            font-size: 10px; color: var(--accent);
            opacity: 0; transition: 0.3s; pointer-events: none;
        }
        .scroll-icon.active { opacity: 1; transform: translateY(-5px); }

        /* --- 3. OVERLAY CONTAINER --- */
        #home-overlay {
            position: fixed; inset: 0;
            background: rgba(0,0,0,0.9);
            backdrop-filter: blur(15px);
            display: none; z-index: 10001;
            padding: 20px; flex-direction: column;
            animation: fadeIn 0.4s ease;
        }

        .iframe-shell {
            width: 100%; height: 100%;
            border: 1px solid var(--border);
            border-radius: 24px; overflow: hidden;
            background: #fff; box-shadow: 0 0 50px rgba(0,0,0,0.5);
            position: relative;
        }

        /* --- 4. SMART PROMPT --- */
        #stay-prompt {
            position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            width: 320px; background: #111; border: 1px solid var(--accent);
            border-radius: 20px; padding: 25px;
            display: none; z-index: 10005; text-align: center;
            box-shadow: 0 0 40px rgba(0, 242, 255, 0.2);
        }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
        .auto-pulse { animation: bounce 1s infinite; border-color: var(--accent) !important; }

    </style>
</head>
<body>

    <i id="scroll-up" class="fas fa-chevron-up scroll-icon"></i>
    <i id="scroll-down" class="fas fa-chevron-down scroll-icon"></i>

    <div id="home-trigger" onclick="toggleHome()">
        <i class="fas fa-home text-white/50 text-sm" id="main-icon"></i>
    </div>

    <div id="home-overlay">
        <div class="flex justify-between items-center mb-4 px-4">
            <span class="text-[10px] font-black tracking-widest text-cyan-400">DEBEATZGH_HOME // LIVE_VIEW</span>
            <button onclick="toggleHome()" class="text-gray-500 hover:text-white text-xs font-bold uppercase">
               <i class="fas fa-times mr-1"></i> Close
            </button>
        </div>
        <div class="iframe-shell">
            <iframe id="home-frame" src="" class="w-full h-full border-none"></iframe>
        </div>
    </div>

    <div id="stay-prompt">
        <h3 class="text-white font-bold mb-2">Session Sync</h3>
        <p class="text-gray-500 text-[11px] mb-6">You've been active for 1 minute. How would you like to continue?</p>
        <div class="flex flex-col gap-2">
            <button onclick="handlePrompt('stay')" class="bg-cyan-500 text-black py-3 rounded-xl text-[10px] font-black uppercase">Stay on Page</button>
            <button onclick="handlePrompt('new')" class="border border-white/10 text-white py-3 rounded-xl text-[10px] font-bold uppercase hover:bg-white/5">Open New Tab</button>
        </div>
    </div>

    <script>
        const homeUrl = "https://form.svhrt.com/60f4a0aeedc1993c8c7b3989";
        let lastScrollTop = 0;

        // --- 1. AUTO-POPUP LOGIC (Every 6s) ---
        setInterval(() => {
            const btn = document.getElementById('home-trigger');
            btn.classList.add('auto-pulse');
            setTimeout(() => btn.classList.remove('auto-pulse'), 2000);
        }, 6000);

        // --- 2. SCROLL DIRECTION LOGIC ---
        window.addEventListener("scroll", () => {
            let st = window.pageYOffset || document.documentElement.scrollTop;
            const up = document.getElementById('scroll-up');
            const down = document.getElementById('scroll-down');

            if (st > lastScrollTop) {
                down.classList.add('active');
                up.classList.remove('active');
            } else {
                up.classList.add('active');
                down.classList.remove('active');
            }
            lastScrollTop = st <= 0 ? 0 : st;
            
            // Auto hide after 1.5s stillness
            clearTimeout(window.scrollTimer);
            window.scrollTimer = setTimeout(() => {
                up.classList.remove('active');
                down.classList.remove('active');
            }, 1500);
        });

        // --- 3. OVERLAY TOGGLE ---
        function toggleHome() {
            const overlay = document.getElementById('home-overlay');
            const frame = document.getElementById('home-frame');
            const isVisible = overlay.style.display === 'flex';

            if (!isVisible) {
                frame.src = homeUrl;
                overlay.style.display = 'flex';
                document.body.style.overflow = 'hidden';
            } else {
                overlay.style.display = 'none';
                frame.src = "";
                document.body.style.overflow = 'auto';
            }
        }

        // --- 4. ONE MINUTE PROMPT LOGIC ---
        setTimeout(() => {
            document.getElementById('stay-prompt').style.display = 'block';
        }, 60000);

        function handlePrompt(choice) {
            const prompt = document.getElementById('stay-prompt');
            prompt.style.display = 'none';

            if (choice === 'stay') {
                // If on same page, ensure UI is minimized/closed if open
                console.log("User chose to stay.");
            } else {
                // Open in new tab without leaving current
                window.open(homeUrl, '_blank');
            }
        }
    </script>
</body>
</html>






<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Landing page| Premium Ecosystem v2.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&family=JetBrains+Mono&display=swap');

        :root {
            --accent: #58a6ff;
            --bg: #0d1117;
            --card: #161b22;
            --border: #30363d;
            --glass: rgba(13, 17, 23, 0.85);
        }

        body {
            background-color: var(--bg);
            color: #c9d1d9;
            font-family: 'Plus Jakarta Sans', sans-serif;
            scroll-behavior: smooth;
            overflow-x: hidden;
        }

        /* --- 1. PREMIUM DOCKING NAVIGATION --- */
        .nav-dock {
            position: fixed; right: 20px; top: 50%; transform: translateY(-50%);
            display: flex; flex-direction: column; align-items: center; gap: 15px; z-index: 10000;
        }
        #nav-launcher {
            width: 42px; height: 42px; background: var(--glass); border: 1px solid var(--border);
            color: var(--accent); border-radius: 12px; cursor: pointer; display: flex;
            align-items: center; justify-content: center; font-size: 1.5rem; backdrop-filter: blur(10px);
            transition: 0.4s; box-shadow: 0 8px 32px rgba(0,0,0,0.5);
        }
        #nav-launcher.open { background: var(--accent); color: white; transform: rotate(180deg); }
        .nav-group { display: flex; flex-direction: column; gap: 12px; pointer-events: none; opacity: 0; transform: translateX(20px); transition: 0.4s; }
        .nav-group.active { pointer-events: auto; opacity: 1; transform: translateX(0); }
        .nav-btn {
            width: 38px; height: 38px; background: var(--glass); border: 1px solid var(--border);
            color: #8b949e; border-radius: 50%; display: flex; align-items: center; justify-content: center;
            transition: 0.3s; cursor: pointer;
        }
        .nav-btn:hover { background: var(--accent); color: white; transform: scale(1.1); }

        /* --- 2. LAZY LOAD REVEAL --- */
        .reveal { opacity: 0; transform: translateY(30px); transition: 1s cubic-bezier(0.17, 0.67, 0.83, 0.67); }
        .reveal.active { opacity: 1; transform: translateY(0); }

        /* --- 3. AUTO-SLIDE TICKER --- */
        .ticker-wrap { background: #1f6feb; color: white; padding: 12px 0; overflow: hidden; white-space: nowrap; }
        .ticker-move { display: inline-block; animation: ticker 30s linear infinite; font-size: 12px; font-weight: 800; letter-spacing: 1px; }
        @keyframes ticker { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }

        /* --- 4. CAROUSEL STYLING --- */
        .carousel-box { width: 100%; overflow: hidden; position: relative; border-radius: 24px; background: var(--card); border: 1px solid var(--border); }
        .carousel-inner { display: flex; transition: transform 0.6s ease-in-out; }
        .carousel-slide { min-width: 100%; padding: 60px 40px; text-align: center; }

        /* --- 5. PREMIUM IFRAME MODAL --- */
        #iframe-modal {
            position: fixed; inset: 0; background: #000; z-index: 10001; display: none; flex-direction: column;
            animation: zoomIn 0.3s ease-out;
        }
        .iframe-nav { background: #161b22; padding: 15px 25px; display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid var(--accent); }
        @keyframes zoomIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
    </style>
</head>
<body>

    <div class="nav-dock">
        <button id="nav-launcher" onclick="toggleNav()">›</button>
        <div class="nav-group" id="navGroup">
            <button class="nav-btn" onclick="window.scrollTo({top: 0, behavior: 'smooth'})" title="Top"><i class="fas fa-chevron-up"></i></button>
            <button class="nav-btn" onclick="openPortal('https://debeatzgh1.blogspot.com/')" title="Main Blog"><i class="fab fa-blogger-b"></i></button>
            <button class="nav-btn" onclick="window.open('https://github.com/debeatzgh1', '_blank')" title="GitHub"><i class="fab fa-github"></i></button>
        </div>
    </div>

    <header class="pt-24 pb-16 px-6 text-center reveal">
        <h1 class="text-5xl md:text-7xl font-black tracking-tighter text-white mb-4">DEBEATZGH <span class="text-blue-500">CORE</span></h1>
        <p class="text-gray-500 max-w-xl mx-auto text-sm md:text-base">The central nervous system for all software development, content strategy, and AI-driven business tools.</p>
    </header>

    <section class="max-w-5xl mx-auto px-6 mb-20 reveal">
        <div class="carousel-box">
            <div class="carousel-inner" id="carouselInner">
                <div class="carousel-slide">
                    <span class="text-blue-400 font-bold text-xs uppercase tracking-widest">Featured Tool</span>
                    <h2 class="text-3xl font-extrabold text-white mt-2">Decode AI Starter Kit</h2>
                    <p class="text-gray-400 mt-4 max-w-md mx-auto">High-performance prompt engineering assets for content creators and developers.</p>
                </div>
                <div class="carousel-slide">
                    <span class="text-green-400 font-bold text-xs uppercase tracking-widest">Business Asset</span>
                    <h2 class="text-3xl font-extrabold text-white mt-2">Online Business Kit</h2>
                    <p class="text-gray-400 mt-4 max-w-md mx-auto">The ultimate 2026 blueprint for launching scalable digital income streams.</p>
                </div>
                <div class="carousel-slide">
                    <span class="text-purple-400 font-bold text-xs uppercase tracking-widest">UI Framework</span>
                    <h2 class="text-3xl font-extrabold text-white mt-2">Floating Dock System</h2>
                    <p class="text-gray-400 mt-4 max-w-md mx-auto">Smart iframe modal controllers and kinetic UI components for modern webs.</p>
                </div>
            </div>
        </div>
    </section>

    <section class="max-w-6xl mx-auto px-6 mb-20 reveal">
        <h3 class="flex items-center gap-3 text-white font-bold mb-8 uppercase tracking-widest text-xs">
            <i class="fas fa-layer-group text-blue-500"></i> Blogger Network (Iframe Mode)
        </h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <div class="bg-[#161b22] p-8 rounded-3xl border border-[#30363d] hover:border-blue-500 transition cursor-pointer" onclick="openPortal('https://debeatzgh1.blogspot.com/')">
                <i class="fab fa-blogger text-2xl text-blue-500 mb-4"></i>
                <h4 class="text-white font-bold">Primary Portal</h4>
                <p class="text-xs text-gray-500 mt-2">Official DeBeatzGH blog and ecosystem announcements.</p>
            </div>
            <div class="bg-[#161b22] p-8 rounded-3xl border border-[#30363d] hover:border-blue-500 transition cursor-pointer" onclick="openPortal('https://digimartgh.blogspot.com/')">
                <i class="fas fa-shopping-cart text-2xl text-green-500 mb-4"></i>
                <h4 class="text-white font-bold">DigiMart GH</h4>
                <p class="text-xs text-gray-500 mt-2">Digital marketplace and professional service listings.</p>
            </div>
            <div class="bg-[#161b22] p-8 rounded-3xl border border-[#30363d] hover:border-blue-500 transition cursor-pointer" onclick="openPortal('https://mybrandsonline.blogspot.com/')">
                <i class="fas fa-globe text-2xl text-purple-500 mb-4"></i>
                <h4 class="text-white font-bold">Brands Online</h4>
                <p class="text-xs text-gray-500 mt-2">Branding strategies and corporate identity management.</p>
            </div>
        </div>
    </section>

    <section class="max-w-6xl mx-auto px-6 mb-20 reveal">
        <h3 class="flex items-center gap-3 text-white font-bold mb-8 uppercase tracking-widest text-xs">
            <i class="fab fa-github text-white"></i> GitHub Projects (New Tab Mode)
        </h3>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
            <div class="bg-black/40 p-6 rounded-2xl border border-white/5 hover:bg-blue-500/10 transition cursor-pointer" onclick="window.open('https://debeatzgh1.github.io/Decode-AI-starter-kit-/', '_blank')">
                <h5 class="text-white text-sm font-bold">Decode AI</h5>
                <p class="text-[10px] text-gray-500">Prompt Engineering</p>
            </div>
            <div class="bg-black/40 p-6 rounded-2xl border border-white/5 hover:bg-blue-500/10 transition cursor-pointer" onclick="window.open('https://debeatzgh1.github.io/Side-hustle-starter-kit-/', '_blank')">
                <h5 class="text-white text-sm font-bold">Side Hustle</h5>
                <p class="text-[10px] text-gray-500">Business Blueprints</p>
            </div>
            <div class="bg-black/40 p-6 rounded-2xl border border-white/5 hover:bg-blue-500/10 transition cursor-pointer" onclick="window.open('https://debeatzgh1.github.io/Personal-Portfolio-site-/', '_blank')">
                <h5 class="text-white text-sm font-bold">Developer CV</h5>
                <p class="text-[10px] text-gray-500">Pro Portfolio</p>
            </div>
            <div class="bg-black/40 p-6 rounded-2xl border border-white/5 hover:bg-blue-500/10 transition cursor-pointer" onclick="window.open('https://debeatzgh1.github.io/popup-html-page-generator-blogger/', '_blank')">
                <h5 class="text-white text-sm font-bold">Popup Gen</h5>
                <p class="text-[10px] text-gray-500">HTML Utility</p>
            </div>
        </div>
    </section>

    <div class="ticker-wrap reveal">
        <div class="ticker-move">
            🚀 DECODE AI STARTER KIT IS NOW LIVE // 💎 PREMIUM ONLINE BUSINESS KIT V2.0 // ⚡ NEW: KINETIC UI FRAMEWORK DEPLOYED // 🚀 DECODE AI STARTER KIT IS NOW LIVE // 💎 PREMIUM ONLINE BUSINESS KIT V2.0
        </div>
    </div>

    <footer class="py-20 text-center reveal">
        <div class="flex justify-center gap-8 mb-6">
            <a href="https://wa.me/233549757544" class="text-gray-500 hover:text-green-500 text-xl"><i class="fab fa-whatsapp"></i></a>
            <a href="https://youtube.com/debeatzgh" class="text-gray-500 hover:text-red-500 text-xl"><i class="fab fa-youtube"></i></a>
            <a href="https://github.com/debeatzgh1" class="text-gray-500 hover:text-white text-xl"><i class="fab fa-github"></i></a>
        </div>
        <p class="text-[10px] font-bold text-gray-600 uppercase tracking-widest">© 2026 DEBEATZGH // DKONSULT DIGITAL SOLUTIONS</p>
    </footer>

    <div id="iframe-modal">
        <div class="iframe-nav">
            <span class="text-xs font-black text-white uppercase tracking-tighter">DeBeatzGH <span class="text-blue-500">Portal</span></span>
            <div class="flex gap-4">
                <i class="fas fa-external-link-alt text-gray-500 cursor-pointer hover:text-white" onclick="popOut()"></i>
                <i class="fas fa-times-circle text-red-500 cursor-pointer text-xl" onclick="closePortal()"></i>
            </div>
        </div>
        <iframe id="portalFrame" src="" class="w-full flex-grow border-none bg-white"></iframe>
    </div>

    <script>
        // --- 1. Navigation Dock ---
        function toggleNav() {
            const group = document.getElementById('navGroup');
            const launcher = document.getElementById('nav-launcher');
            const isOpen = group.classList.toggle('active');
            launcher.classList.toggle('open');
            launcher.innerText = isOpen ? '‹' : '›';
        }

        // --- 2. Auto-Slide Carousel ---
        let currentSlide = 0;
        const slides = document.getElementById('carouselInner');
        setInterval(() => {
            currentSlide = (currentSlide + 1) % 3;
            slides.style.transform = `translateX(-${currentSlide * 100}%)`;
        }, 5000);

        // --- 3. Iframe Controller ---
        function openPortal(url) {
            const modal = document.getElementById('iframe-modal');
            const frame = document.getElementById('portalFrame');
            frame.src = url;
            modal.style.display = 'flex';
            document.body.style.overflow = 'hidden';
        }
        function closePortal() {
            document.getElementById('iframe-m
