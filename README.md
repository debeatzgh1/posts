<!-- Elfsight FAQ | FAQ -->
<script src="https://elfsightcdn.com/platform.js" async></script>
<div class="elfsight-app-e78e03db-1434-4f96-aea0-fcbc6c92c429" data-elfsight-app-lazy></div>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DeBeatzGH | OS Interface</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Plus+Jakarta+Sans:wght@400;800&display=swap');

        :root {
            --accent: #00f2ff;
            --bg-dark: #050507;
            --glass: rgba(255, 255, 255, 0.03);
            --border: rgba(0, 242, 255, 0.2);
        }

        body {
            background-color: var(--bg-dark);
            color: #f0f6fc;
            font-family: 'Plus Jakarta Sans', sans-serif;
            margin: 0; overflow: hidden; height: 100vh;
        }

        /* --- AMBIENT BACKGROUND --- */
        .cyber-orb {
            position: fixed; top: 50%; left: 50%;
            width: 600px; height: 600px;
            background: radial-gradient(circle, rgba(0, 242, 255, 0.15) 0%, transparent 70%);
            transform: translate(-50%, -50%);
            z-index: -1; animation: pulseOrb 10s infinite alternate;
        }

        @keyframes pulseOrb {
            from { transform: translate(-50%, -50%) scale(1); opacity: 0.5; }
            to { transform: translate(-50%, -50%) scale(1.3); opacity: 0.8; }
        }

        /* --- INTEGRATED SMART BANNER --- */
        .smart-dock {
            position: fixed; bottom: 20px; left: 50%;
            transform: translateX(-50%);
            width: 90%; max-width: 450px;
            background: rgba(10, 10, 12, 0.9);
            backdrop-filter: blur(20px);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 12px 15px;
            display: flex; align-items: center; gap: 15px;
            z-index: 10000;
            box-shadow: 0 15px 50px rgba(0,0,0,0.8);
        }

        .feed-ticker {
            flex: 1; height: 24px; overflow: hidden;
            font-family: 'JetBrains Mono', monospace; font-size: 11px;
        }

        .ticker-track {
            display: flex; flex-direction: column;
            animation: tickerScroll 10s infinite;
        }

        @keyframes tickerScroll {
            0%, 20% { transform: translateY(0); }
            25%, 45% { transform: translateY(-24px); }
            50%, 70% { transform: translateY(-48px); }
            75%, 95% { transform: translateY(-72px); }
            100% { transform: translateY(0); }
        }

        /* --- GLASS CONFIGURATOR CARD --- */
        .glass-panel {
            background: var(--glass);
            backdrop-filter: blur(10px);
            border: 1px solid var(--border);
            border-radius: 30px;
            padding: 30px;
            max-width: 400px;
            width: 90%;
            margin: auto;
        }

        /* --- MASTER OVERLAY --- */
        #master-overlay {
            position: fixed; inset: 0;
            background: rgba(0,0,0,0.95);
            backdrop-filter: blur(15px);
            z-index: 20000;
            display: none; flex-direction: column;
            animation: zoomIn 0.3s ease;
        }

        @keyframes zoomIn {
            from { opacity: 0; transform: scale(0.98); }
            to { opacity: 1; transform: scale(1); }
        }

        .loader-bar {
            height: 2px; width: 0%;
            background: var(--accent);
            position: absolute; top: 0; left: 0;
            transition: width 0.5s ease;
        }
    </style>
</head>
<body class="flex items-center justify-center">

    <div class="cyber-orb"></div>

    <div class="glass-panel text-center">
        <h1 class="text-2xl font-black mb-6 tracking-tighter uppercase">Glass_OS <span class="text-cyan-400">v2.0</span></h1>
        
        <div class="space-y-4 text-left">
            <div class="bg-black/40 p-4 rounded-2xl border border-white/5">
                <label class="text-[10px] uppercase font-bold text-gray-500 mb-2 block">Transparency</label>
                <input type="range" id="op-in" min="0" max="1" step="0.01" value="0.1" class="w-full accent-cyan-400">
            </div>

            <div class="bg-black/40 p-4 rounded-2xl border border-white/5">
                <label class="text-[10px] uppercase font-bold text-gray-500 mb-2 block">Blur Strength</label>
                <input type="range" id="bl-in" min="0" max="40" step="1" value="15" class="w-full accent-cyan-400">
            </div>
        </div>

        <div id="code-output" class="mt-6 p-4 bg-black/60 rounded-xl font-mono text-[10px] text-cyan-500 border border-cyan-500/20 text-left">
            Initializing config...
        </div>
    </div>

    <div class="smart-dock">
        <div class="w-10 h-10 rounded-xl bg-cyan-500/10 flex items-center justify-center text-cyan-400">
            <i class="fas fa-rss animate-pulse"></i>
        </div>
        
        <div class="feed-ticker">
            <div class="ticker-track">
                <span class="h-6 flex items-center text-white">>> DEBEATZGH: New Portfolio UI Live</span>
                <span class="h-6 flex items-center text-cyan-400">>> WORDPRESS: Claim your .com site</span>
                <span class="h-6 flex items-center text-white">>> FIREBASE: Build v2.4 out now</span>
                <span class="h-6 flex items-center text-cyan-400">>> TECHSHOP: Access Restricted Portal</span>
            </div>
        </div>

        <button onclick="openGlobalOverlay('https://appdategh1.blogspot.com/')" 
            class="bg-cyan-500 text-black text-[10px] font-black px-4 py-2 rounded-lg uppercase hover:bg-white transition">
            Access
        </button>
    </div>

    <div id="master-overlay">
        <div class="loader-bar" id="load-progress"></div>
        <div class="h-14 border-b border-white/10 flex items-center justify-between px-6">
            <span class="text-[10px] font-mono text-gray-500 uppercase" id="frame-title">Secure_Port // Connecting...</span>
            <button id="master-close" disabled class="px-4 py-1.5 bg-white/5 border border-white/10 text-gray-500 rounded-lg text-[10px] font-bold uppercase cursor-not-allowed">
                Wait <span id="timer-val">3</span>s
            </button>
        </div>
        <iframe id="master-frame" class="w-full flex-grow bg-white" src=""></iframe>
    </div>

    <script>
        // --- 1. GLASS LOGIC ---
        const opIn = document.getElementById('op-in');
        const blIn = document.getElementById('bl-in');
        const glass = document.querySelector('.glass-panel');
        const code = document.getElementById('code-output');

        function updateGlass() {
            const o = opIn.value; const b = blIn.value;
            glass.style.background = `rgba(255, 255, 255, ${o})`;
            glass.style.backdropFilter = `blur(${b}px)`;
            code.innerHTML = `background: rgba(255, 255, 255, ${o});<br>backdrop-filter: blur(${b}px);<br>// Tailwind: bg-white/[${o}] backdrop-blur-[${b}px]`;
        }
        [opIn, blIn].forEach(i => i.addEventListener('input', updateGlass));
        updateGlass();

        // --- 2. MASTER OVERLAY ENGINE ---
        const overlay = document.getElementById('master-overlay');
        const frame = document.getElementById('master-frame');
        const closeBtn = document.getElementById('master-close');
        const progressBar = document.getElementById('load-progress');
        let timer;

        function openGlobalOverlay(url) {
            // Anti-spam check
            if(localStorage.getItem('techshop_auth_v2') === 'true' && url.includes('techshop')) {
                console.log("Access granted. Popup suppressed.");
                return;
            }

            frame.src = url;
            overlay.style.display = 'flex';
            progressBar.style.width = '100%';
            
            // Start Security Timer
            let sec = 3;
            closeBtn.disabled = true;
            closeBtn.style.cursor = 'not-allowed';
            closeBtn.innerHTML = `Wait ${sec}s`;
            
            clearInterval(timer);
            timer = setInterval(() => {
                sec--;
                if(sec > 0) {
                    closeBtn.innerHTML = `Wait ${sec}s`;
                } else {
                    clearInterval(timer);
                    closeBtn.disabled = false;
                    closeBtn.style.cursor = 'pointer';
                    closeBtn.classList.add('bg-cyan-500', 'text-black', 'border-none');
                    closeBtn.innerHTML = "Close [X]";
                    closeBtn.onclick = closeGlobalOverlay;
                }
            }, 1000);
        }

        function closeGlobalOverlay() {
            overlay.style.display = 'none';
            frame.src = "";
            progressBar.style.width = '0%';
        }

        // --- 3. AUTO-TRIGGER LOGIC ---
        window.onload = () => {
            // Trigger WordPress signup after 8 seconds if not authenticated
            if(!localStorage.getItem('techshop_auth_v2')) {
                setTimeout(() => {
                    openGlobalOverlay('https://appdategh1.blogspot.com/');
                }, 8000);
            }
        };

        // Close on ESC
        document.addEventListener('keydown', (e) => {
            if (e.key === "Escape" && !closeBtn.disabled) closeGlobalOverlay();
        });
    </script>
</body>
</html>
