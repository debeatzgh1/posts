





<div id="smart-float-container" class="float-wrapper">
    <div id="float-nudge" class="float-nudge">
        <p>Claim your <strong>.wordpress.com</strong> site!</p>
        <button onclick="dismissNudge()" class="nudge-close">×</button>
    </div>

    <div class="float-main-btn" onclick="launchWPSignup()">
        <i class="fab fa-wordpress"></i>
        <span class="btn-label">Create Site</span>
    </div>
</div>

<style>
    .float-wrapper {
        position: fixed;
        bottom: 20px;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 12px;
        z-index: 10005;
        font-family: 'Plus Jakarta Sans', sans-serif;
    }

    /* Floating Nudge Bubble */
    .float-nudge {
        background: rgba(10, 10, 12, 0.9);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(0, 242, 255, 0.3);
        padding: 8px 15px;
        border-radius: 12px;
        color: #f0f6fc;
        font-size: 11px;
        white-space: nowrap;
        box-shadow: 0 10px 25px rgba(0,0,0,0.5);
        display: none; /* Controlled by JS */
        animation: slideUpFade 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        align-items: center;
        gap: 10px;
    }

    .nudge-close {
        background: none;
        border: none;
        color: #64748b;
        font-size: 16px;
        cursor: pointer;
        padding: 0 2px;
        line-height: 1;
    }

    .nudge-close:hover { color: #00f2ff; }

    /* Main Button */
    .float-main-btn {
        background: #00f2ff;
        color: #000;
        padding: 10px 20px;
        border-radius: 99px;
        display: flex;
        align-items: center;
        gap: 8px;
        cursor: pointer;
        font-weight: 800;
        text-transform: uppercase;
        font-size: 10px;
        letter-spacing: 1px;
        box-shadow: 0 0 20px rgba(0, 242, 255, 0.3);
        transition: all 0.3s ease;
    }

    .float-main-btn:hover {
        transform: translateY(-3px) scale(1.05);
        background: #fff;
        box-shadow: 0 0 30px rgba(255, 255, 255, 0.4);
    }

    .float-main-btn i { font-size: 14px; }

    @keyframes slideUpFade {
        from { opacity: 0; transform: translateY(15px); }
        to { opacity: 1; transform: translateY(0); }
    }

    @media (max-width: 480px) {
        .float-main-btn { padding: 10px 16px; }
        .btn-label { display: none; } /* On mobile, only show icon to save space */
    }
</style>

<script>
    const NUDGE_KEY = 'wp_nudge_dismissed';

    function showNudge() {
        const isDismissed = localStorage.getItem(NUDGE_KEY);
        if (!isDismissed) {
            document.getElementById('float-nudge').style.display = 'flex';
        }
    }

    function dismissNudge() {
        document.getElementById('float-nudge').style.display = 'none';
        // Remember dismissal for 24h
        localStorage.setItem(NUDGE_KEY, 'true');
    }

    function launchWPSignup() {
        const targetUrl = "https://debeatzgh1.github.io/Blogger-sign-up-button-/";
        
        // Use existing overlay logic if available
        if (typeof openLink === "function") {
            openLink(targetUrl);
        } else if (typeof openFrame === "function") {
            openFrame(targetUrl);
        } else {
            window.open(targetUrl, '_blank');
        }
    }

    // Auto-popup nudge after 8 seconds
    window.addEventListener('load', () => {
        setTimeout(showNudge, 8000);
    });
</script>



<!DOCTYPE html>
<html lang="en-GB">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Glass UI Generator | Pro Dashboard</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --glass-bg: rgba(255, 255, 255, 0.1);
            --glass-border: rgba(255, 255, 255, 0.2);
            --accent-glow: linear-gradient(135.2deg, #e11d48 1.1%, #3b82f6 98.9%);
        }

        body {
            margin: 0;
            padding: 0;
            background-color: var(--bg-color);
            color: #ffffff;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        /* Ambient Glow */
        .orb {
            position: absolute;
            width: 500px;
            height: 500px;
            background: var(--accent-glow);
            filter: blur(100px);
            border-radius: 50%;
            z-index: -1;
            opacity: 0.4;
            animation: pulse 8s infinite alternate;
        }

        @keyframes pulse {
            from { transform: scale(1); opacity: 0.4; }
            to { transform: scale(1.2); opacity: 0.6; }
        }

        /* Main Dashboard Card */
        .glass-container {
            width: 90%;
            max-width: 500px;
            padding: 40px;
            background: var(--glass-bg);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid var(--glass-border);
            border-radius: 28px;
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5);
            text-align: center;
        }

        .controls { margin-top: 30px; text-align: left; display: grid; gap: 15px; }
        input[type=range] { width: 100%; cursor: pointer; }
        
        #code-output {
            margin-top: 25px;
            padding: 15px;
            background: rgba(0,0,0,0.4);
            border-radius: 12px;
            font-family: 'Courier New', monospace;
            font-size: 0.8rem;
            color: #60a5fa;
            border: 1px solid #1e293b;
        }

        /* Full Page Overlay */
        #url-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 9999;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }

        #close-btn {
            position: absolute;
            top: 20px; right: 20px;
            z-index: 10000;
            padding: 12px 24px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: white;
            border-radius: 50px;
            cursor: not-allowed;
            opacity: 0.5;
            transition: all 0.3s ease;
        }

        #close-btn.active {
            cursor: pointer;
            opacity: 1;
            background: #e11d48;
        }
    </style>
</head>
<body>

<div class="orb"></div>

<div class="glass-container">
    <h2 style="margin-top:0;">Glass UI Configurator 💎</h2>
    <div class="controls">
        <label>Transparency: <span id="op-val">0.1</span></label>
        <input type="range" id="op-in" min="0" max="1" step="0.01" value="0.1">
        
        <label>Blur: <span id="bl-val">15</span>px</label>
        <input type="range" id="bl-in" min="0" max="40" step="1" value="15">
    </div>
    <div id="code-output">Loading config...</div>
</div>

<div id="url-overlay">
    <button id="close-btn" onclick="closeOverlay()">Wait (3s)</button>
    <iframe id="overlay-frame" src="https://debeatzgh1.github.io/firebase-front-end-components/" 
            style="width:100%; height:100%; border:none;"></iframe>
</div>

<script>
    // --- GLASS GENERATOR LOGIC ---
    const opIn = document.getElementById('op-in');
    const blIn = document.getElementById('bl-in');
    const glass = document.querySelector('.glass-container');
    const code = document.getElementById('code-output');

    function syncUI() {
        const o = opIn.value; const b = blIn.value;
        document.getElementById('op-val').innerText = o;
        document.getElementById('bl-val').innerText = b;
        
        glass.style.background = `rgba(255, 255, 255, ${o})`;
        glass.style.backdropFilter = `blur(${b}px)`;
        code.innerHTML = `background: rgba(255, 255, 255, ${o});<br>backdrop-filter: blur(${b}px);`;
    }
    [opIn, blIn].forEach(i => i.addEventListener('input', syncUI));
    syncUI();

    // --- OVERLAY LOGIC ---
    const overlay = document.getElementById('url-overlay');
    const closeBtn = document.getElementById('close-btn');
    let countdown;

    function showOverlay() {
        // Referral check: Don't show if user arrives from the same URL
        if (document.referrer.includes("debeatzgh1.github.io/firebase-front-end-components")) return;

        overlay.style.display = 'block';
        startCountdown();
    }

    function startCountdown() {
        let timer = 3;
        closeBtn.classList.remove('active');
        closeBtn.disabled = true;
        closeBtn.innerText = `Wait (${timer}s)`;

        clearInterval(countdown);
        countdown = setInterval(() => {
            timer--;
            if (timer > 0) {
                closeBtn.innerText = `Wait (${timer}s)`;
            } else {
                clearInterval(countdown);
                closeBtn.innerText = "Close [X]";
                closeBtn.classList.add('active');
                closeBtn.disabled = false;
            }
        }, 1000);
    }

    function closeOverlay() {
        overlay.style.display = 'none';
    }

    // Initial Trigger
    window.onload = () => setTimeout(showOverlay, 1000);

    // Loop every 6 seconds
    setInterval(() => {
        if (overlay.style.display === 'none') showOverlay();
    }, 6000);
</script>

</body>
</html>


<!doctype html>


    
    
    <style>
        :root {
            --notify-color: #ff3e3e;
            --gist-accent: #58a6ff;
            --glass-bg: rgba(13, 17, 23, 0.98);
        }

        body { margin: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }

        /* 1. POSITIONED RIGHT-MIDDLE + HEARTBEAT */
        #gist-notifier {
            position: fixed;
            top: 50%;
            right: 15px;
            transform: translateY(-50%);
            width: 40px;
            height: 40px;
            background: var(--glass-bg);
            border: 2px solid var(--gist-accent);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 9999;
            box-shadow: 0 0 20px rgba(88, 166, 255, 0.4);
            animation: heartbeat 1.2s ease-in-out infinite;
        }

        @keyframes heartbeat {
            0% { transform: translateY(-50%) scale(1); }
            15% { transform: translateY(-50%) scale(1.15); }
            30% { transform: translateY(-50%) scale(1); }
            45% { transform: translateY(-50%) scale(1.15); }
            100% { transform: translateY(-50%) scale(1); }
        }

        .badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background: var(--notify-color);
            color: white;
            font-size: 8px;
            font-weight: 900;
            padding: 3px 6px;
            border-radius: 4px;
        }

        /* 2. OVERLAY & MODAL */
        #gist-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.9);
            backdrop-filter: blur(15px);
            display: none;
            z-index: 10000;
            justify-content: center;
            align-items: center;
        }

        .modal-container {
            width: 100%;
            max-width: 850px;
            height: 85vh;
            background: #0d1117;
            border-radius: 20px;
            border: 1px solid var(--gist-accent);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            position: relative;
            transition: 0.3s;
        }

        /* MOBILE FULL SCREEN OVERRIDE */
        @media (max-width: 768px) {
            .modal-container {
                height: 100vh;
                max-width: 100%;
                border-radius: 0;
                border: none;
            }
        }

        .modal-header {
            padding: 12px 20px;
            background: #161b22;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        /* 3. TIMER BUTTON */
        #close-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: #8b949e;
            width: 36px;
            height: 36px;
            border-radius: 8px;
            cursor: not-allowed;
            font-weight: bold;
            font-size: 14px;
        }

        #close-btn.ready {
            background: var(--notify-color);
            color: white;
            cursor: pointer;
        }

        .alert-banner {
            background: #ffd600;
            color: #000;
            text-align: center;
            padding: 8px;
            font-size: 10px;
            font-weight: 900;
            letter-spacing: 0.5px;
            text-transform: uppercase;
        }

        #gist-frame { width: 100%; flex-grow: 1; border: none; }

        #confirm-strip {
            padding: 12px;
            background: #161b22;
            text-align: center;
            display: none;
            border-top: 1px solid rgba(255,255,255,0.1);
        }
    </style>



    <div id="gist-notifier" onclick="openGist()">
        <span class="badge">SIGN IN</span>
        <svg width="24" height="24" viewbox="0 0 24 24" fill="none" stroke="#58a6ff" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
        </svg>
    </div>

    <div id="gist-overlay">
        <div class="modal-container">
            <div id="status-banner" class="alert-banner">⚠️ LOGIN REQUIRED TO DISABLE AUTO-POPUPS</div>
            <div class="modal-header">
                <span style="color:white; font-size:11px; font-weight:bold; letter-spacing:1px;">TECHSHOP SECURE PORTAL</span>
                <button id="close-btn">3</button>
            </div>
            
            <iframe id="gist-frame" src=""></iframe>
            
            <div id="confirm-strip">
                <button onclick="confirmAuth()" style="background:var(--gist-accent); border:none; color:black; padding:10px 25px; border-radius:8px; font-weight:800; font-size:12px; cursor:pointer; width: 100%; max-width: 300px;">I've Logged In - Stop Popups</button>
            </div>
        </div>
    </div>

    <script>
        const overlay = document.getElementById('gist-overlay');
        const frame = document.getElementById('gist-frame');
        const closeBtn = document.getElementById('close-btn');
        const banner = document.getElementById('status-banner');
        const confirmStrip = document.getElementById('confirm-strip');
        
        let countdown;
        let autoPopupInterval;
        let isAuth = localStorage.getItem('techshop_auth_v2') === 'true';

        function openGist() {
            if (frame.src === "" || frame.src.includes("about:blank")) {
                frame.src = "https://www.socialcreator.com/techshop/";
            }
            overlay.style.display = 'flex';
            document.body.style.overflow = 'hidden';
            
            // Start the 3-second lock
            startCloseTimer();
            
            // Wait 5 seconds to show the manual "Stop Popups" button
            setTimeout(() => { confirmStrip.style.display = 'block'; }, 5000);
        }

        function startCloseTimer() {
            let seconds = 3;
            closeBtn.classList.remove('ready');
            closeBtn.innerText = seconds;
            closeBtn.onclick = null;
            
            clearInterval(countdown);
            countdown = setInterval(() => {
                seconds--;
                if (seconds > 0) {
                    closeBtn.innerText = seconds;
                } else {
                    clearInterval(countdown);
                    closeBtn.innerText = "✖";
                    closeBtn.classList.add('ready');
                    closeBtn.onclick = closeGist;
                }
            }, 1000);
        }

        function closeGist() {
            overlay.style.display = 'none';
            document.body.style.overflow = 'auto';
        }

        function confirmAuth() {
            isAuth = true;
            localStorage.setItem('techshop_auth_v2', 'true');
            clearInterval(autoPopupInterval);
            banner.style.background = "#4caf50";
            banner.innerText = "✅ AUTHENTICATED. POPUPS REMOVED.";
            document.getElementById('gist-notifier').style.display = 'none';
            setTimeout(closeGist, 1000);
        }

        function initAutoPopup() {
            if (isAuth) return;
            autoPopupInterval = setInterval(() => {
                if (overlay.style.display !== 'flex' && !isAuth) {
                    openGist();
                }
            }, 6000); 
        }

        window.onload = () => {
            if (!isAuth) {
                initAutoPopup();
            } else {
                document.getElementById('gist-notifier').style.display = 'none';
            }
        };

        // Extra layer: Close on background click only if timer is finished
        overlay.onclick = (e) => {
            if (e.target === overlay && closeBtn.classList.contains('ready')) closeGist();
        };
    </script>

</!doctype>



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --ui-accent: #00f2ff; /* Cyber Cyan */
            --ui-bg: rgba(10, 10, 12, 0.9);
            --ui-glow: rgba(0, 242, 255, 0.4);
            --text-light: #f0f6fc;
        }

        /* --- TOPMOST FLOATING BANNER --- */
        .ui-browser-banner {
            position: fixed;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            width: 310px;
            height: 48px;
            background: var(--ui-bg);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid rgba(0, 242, 255, 0.2);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 8px 0 15px;
            z-index: 10001; /* Higher than other banners */
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.8), 0 0 10px var(--ui-glow);
            overflow: hidden;
        }

        /* Auto-Slide Text Container */
        .ui-slider-box {
            flex: 1;
            overflow: hidden;
            position: relative;
            height: 20px;
            margin-right: 10px;
        }

        .ui-slider-content {
            display: flex;
            flex-direction: column;
            animation: slideText 6s cubic-bezier(0.645, 0.045, 0.355, 1) infinite;
        }

        .ui-slider-content span {
            height: 20px;
            font-family: 'Monaco', 'Consolas', monospace;
            font-size: 0.75rem;
            color: var(--ui-accent);
            display: flex;
            align-items: center;
            gap: 6px;
            letter-spacing: 1px;
            font-weight: bold;
        }

        /* Action Button */
        .ui-open-btn {
            background: var(--ui-accent);
            color: #000;
            border: none;
            padding: 6px 14px;
            border-radius: 6px;
            font-size: 0.7rem;
            font-weight: 900;
            cursor: pointer;
            text-transform: uppercase;
            transition: 0.3s ease;
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .ui-open-btn:hover {
            background: #fff;
            box-shadow: 0 0 15px #fff;
            transform: scale(1.05);
        }

        /* --- OVERLAY IFRAME --- */
        #ui-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.95);
            display: none;
            flex-direction: column;
            z-index: 20000;
            animation: overlayFade 0.4s ease;
        }

        .ui-overlay-nav {
            height: 50px;
            background: #111;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 20px;
            border-bottom: 2px solid var(--ui-accent);
        }

        .ui-frame {
            width: 100%;
            flex-grow: 1;
            border: none;
            background: #fff;
        }

        /* --- ANIMATIONS --- */
        @keyframes slideText {
            0%, 20% { transform: translateY(0); }
            33%, 53% { transform: translateY(-20px); }
            66%, 86% { transform: translateY(-40px); }
            100% { transform: translateY(0); }
        }

        @keyframes overlayFade {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .pulse-icon {
            width: 6px;
            height: 6px;
            background: var(--ui-accent);
            border-radius: 50%;
            box-shadow: 0 0 8px var(--ui-accent);
        }
    </style>
</head>
<body>

    <div class="ui-browser-banner">
        <div class="ui-slider-box">
            <div class="ui-slider-content">
                <span><div class="pulse-icon"></div> Browse UI & Interfaces</span>
                <span><div class="pulse-icon"></div> Discover Experience</span>
                <span><div class="pulse-icon"></div> Portfolio Access</span>
            </div>
        </div>
        <button class="ui-open-btn" onclick="openUIHub()">
            Explore <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </button>
    </div>

    <div id="ui-overlay">
        <div class="ui-overlay-nav">
            <span style="color: var(--ui-accent); font-family: monospace; font-weight: bold; font-size: 0.8rem;">DEBEATZGH // UI_INTERFACES</span>
            <button onclick="closeUIHub()" style="background:none; border: 1px solid #444; color: #888; padding: 4px 12px; cursor: pointer; border-radius: 4px; font-size: 0.7rem;">CLOSE [ESC]</button>
        </div>
        <iframe class="ui-frame" id="ui-iframe"></iframe>
    </div>

    <script>
        const uiOverlay = document.getElementById('ui-overlay');
        const uiIframe = document.getElementById('ui-iframe');

        function openUIHub() {
            uiIframe.src = "https://debeatzgh1.github.io/blogs/";
            uiOverlay.style.display = 'flex';
            document.body.style.overflow = 'hidden';
        }

        function closeUIHub() {
            uiOverlay.style.display = 'none';
            uiIframe.src = "";
            document.body.style.overflow = 'auto';
        }

        // Close on Escape key
        document.addEventListener('keydown', (e) => {
            if (e.key === "Escape") closeUIHub();
        });
    </script>
</body>
</html



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --banner-bg: rgba(13, 17, 23, 0.85);
            --accent-pink: #FF1493;
            --accent-glow: rgba(255, 20, 147, 0.4);
            --text-white: #ffffff;
            --border-glass: rgba(255, 255, 255, 0.1);
        }

        /* Floating Banner Container */
        .top-floating-banner {
            position: fixed;
            top: 15px;
            left: 50%;
            transform: translateX(-50%);
            width: 90%;
            max-width: 700px;
            height: 50px;
            background: var(--banner-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--border-glass);
            border-radius: 100px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 10px 0 25px;
            z-index: 10000;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            animation: slideDown 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        /* Carousel Wrapper */
        .carousel-wrapper {
            flex: 1;
            overflow: hidden;
            position: relative;
            margin-right: 15px;
        }

        .carousel-content {
            display: flex;
            white-space: nowrap;
            animation: scrollText 15s linear infinite;
        }

        .carousel-content span {
            font-family: 'Segoe UI', Roboto, sans-serif;
            font-size: 0.85rem;
            color: var(--text-white);
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Launch Button */
        .banner-btn {
            background: var(--accent-pink);
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 50px;
            font-size: 0.8rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
            box-shadow: 0 0 15px var(--accent-glow);
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .banner-btn:hover {
            transform: scale(1.05);
            background: #ff69b4;
            box-shadow: 0 0 25px var(--accent-glow);
        }

        /* Modal Overlay for Milkshake */
        #milkshake-modal {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.9);
            display: none;
            z-index: 10001;
            flex-direction: column;
            animation: fadeIn 0.3s ease;
        }

        .modal-header {
            padding: 15px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #161b22;
        }

        .close-btn {
            background: #f85149;
            color: white;
            border: none;
            padding: 5px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
        }

        #msha-iframe {
            width: 100%;
            flex-grow: 1;
            border: none;
        }

        /* Animations */
        @keyframes scrollText {
            0% { transform: translateX(100%); }
            100% { transform: translateX(-100%); }
        }

        @keyframes slideDown {
            from { transform: translate(-50%, -100px); opacity: 0; }
            to { transform: translate(-50%, 0); opacity: 1; }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* Mobile Adjustments */
        @media (max-width: 600px) {
            .carousel-content span { font-size: 0.75rem; }
            .banner-btn span { display: none; }
            .banner-btn { padding: 8px 12px; }
        }
    </style>
</head>
<body>

    <div class="top-floating-banner">
        <div class="carousel-wrapper">
            <div class="carousel-content">
                <span>
                    💻 Access your lifestyle, productivity tools and ideas All in one place! 🚀 💡 📈
                </span>
            </div>
        </div>
        
        <button class="banner-btn" onclick="openMilkshake()">
            <span>Launch Hub</span> ⭐️
        </button>
    </div>

    <div id="milkshake-modal">
        <div class="modal-header">
            <span style="color:white; font-family: sans-serif; font-weight: bold;">Debeatzgh Hub</span>
            <button class="close-btn" onclick="closeMilkshake()">✕ Close</button>
        </div>
        <iframe id="msha-iframe" src=""></iframe>
    </div>

    <script>
        function openMilkshake() {
            const modal = document.getElementById('milkshake-modal');
            const iframe = document.getElementById('msha-iframe');
            
            // Set source only when clicked to save performance
            iframe.src = "https://msha.ke/debeatzgh";
            modal.style.display = "flex";
            document.body.style.overflow = "hidden"; // Disable scroll
        }

        function closeMilkshake() {
            const modal = document.getElementById('milkshake-modal');
            const iframe = document.getElementById('msha-iframe');
            
            modal.style.display = "none";
            iframe.src = ""; // Stop the iframe content
            document.body.style.overflow = "auto"; // Re-enable scroll
        }
    </script>

</body>
</html>


<html lang="en">
<head>
    <style>
        :root {
            --nav-bg: rgba(13, 17, 23, 0.9);
            --nav-border: #30363d;
            --nav-accent: #58a6ff;
            --nav-hover: #1f6feb;
            --glow-color: rgba(88, 166, 255, 0.5);
        }

        /* Dock Container */
        .nav-dock {
            position: fixed;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            z-index: 10000;
        }

        /* Launcher (>) */
        #nav-launcher {
            width: 38px;
            height: 38px;
            background: var(--nav-bg);
            border: 1px solid var(--nav-border);
            color: var(--nav-accent);
            border-radius: 10px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.4rem;
            backdrop-filter: blur(8px);
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.4);
        }

        #nav-launcher.open {
            color: white;
            background: var(--nav-hover);
            border-color: var(--nav-accent);
        }

        /* Button Group */
        .nav-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            pointer-events: none;
        }

        .nav-group.active {
            pointer-events: auto;
        }

        .nav-btn {
            width: 34px;
            height: 34px;
            background: var(--nav-bg);
            border: 1px solid var(--nav-border);
            color: #c9d1d9;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transform: scale(0.5) translateX(30px);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-decoration: none;
            position: relative;
        }

        /* Active/Open State for Buttons */
        .nav-group.active .nav-btn {
            opacity: 1;
            transform: scale(1) translateX(0);
        }

        /* Heartbeat Glow Animation */
        @keyframes heartbeatGlow {
            0% { box-shadow: 0 0 0 0 var(--glow-color); transform: scale(1); }
            50% { box-shadow: 0 0 15px 5px var(--glow-color); transform: scale(1.1); }
            100% { box-shadow: 0 0 0 0 var(--glow-color); transform: scale(1); }
        }

        .heartbeat-active {
            animation: heartbeatGlow 1.2s ease-in-out 2; /* Runs twice on open */
        }

        .nav-btn:hover {
            background: var(--nav-hover);
            color: white;
            border-color: var(--nav-accent);
        }

        /* Staggered transition delays for a smooth "pop-in" effect */
        .nav-group.active .nav-btn:nth-child(1) { transition-delay: 0.1s; }
        .nav-group.active .nav-btn:nth-child(2) { transition-delay: 0.2s; }
        .nav-group.active .nav-btn:nth-child(3) { transition-delay: 0.3s; }

        .nav-btn svg { width: 18px; height: 18px; }
    </style>
</head>
<body>

    <div class="nav-dock">
        <button id="nav-launcher" onclick="toggleNav()">›</button>

        <div class="nav-group" id="navGroup">
            <button class="nav-btn" onclick="window.scrollTo({top: 0, behavior: 'smooth'})">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="18 15 12 9 6 15"></polyline></svg>
            </button>

            <a href="https://debeatzgh1.github.io/Digital-Creator-s-Essential-Guides-Tools/" class="nav-btn">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
            </a>

            <button class="nav-btn" onclick="window.scrollTo({top: document.body.scrollHeight, behavior: 'smooth'})">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"></polyline></svg>
            </button>
        </div>
    </div>

    <script>
        function toggleNav() {
            const group = document.getElementById('navGroup');
            const launcher = document.getElementById('nav-launcher');
            const buttons = document.querySelectorAll('.nav-btn');
            
            const isOpen = group.classList.toggle('active');
            launcher.classList.toggle('open');
            launcher.innerText = isOpen ? '‹' : '›';

            if (isOpen) {
                // Trigger heartbeat animation on each button when opened
                buttons.forEach((btn, index) => {
                    // Slight delay before heartbeat starts to match the pop-in
                    setTimeout(() => {
                        btn.classList.add('heartbeat-active');
                    }, (index + 1) * 200);

                    // Remove class after animation ends so it can re-trigger next time
                    setTimeout(() => {
                        btn.classList.remove('heartbeat-active');
                    }, 3000);
                });
            }
        }
    </script>

</body>
</html>





<!-- Elfsight Social Feed | Tech tools and ideas for startups  -->
<script src="https://elfsightcdn.com/platform.js" async></script>
<div class="elfsight-app-a4b45e92-12e1-42d5-8780-cdadee65af5b" data-elfsight-app-lazy></div>

<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Debeatzgh AI Blog Hub</title>

<style>
:root{
  --bg:#0b1220;
  --card:#111827;
  --muted:#9ca3af;
  --accent:#38bdf8;
  --radius:16px;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family:system-ui,-apple-system,Segoe UI,Roboto;
  background:var(--bg);
  color:#fff;
}

/* Header */
.header{
  position:sticky;top:0;z-index:1000;
  background:#020617;
  padding:12px;
}
.search{
  width:100%;
  padding:10px 14px;
  border-radius:999px;
  border:none;
  background:var(--card);
  color:#fff;
  font-size:14px;
}

/* Tabs */
.tabs{
  display:flex;
  gap:8px;
  margin-top:10px;
  overflow-x:auto;
}
.tabs button{
  background:var(--card);
  color:#fff;
  border:0;
  padding:8px 14px;
  border-radius:999px;
  cursor:pointer;
}
.tabs button.active{
  background:var(--accent);
  color:#000;
}

/* Sections */
.section{padding:14px}
.section h2{
  font-size:16px;
  display:flex;
  gap:8px;
  align-items:center;
}
.badge{
  font-size:11px;
  background:var(--accent);
  color:#000;
  padding:3px 8px;
  border-radius:999px;
}

/* Cards */
.carousel{
  display:flex;
  gap:12px;
  overflow-x:auto;
}
.card{
  min-width:260px;
  background:var(--card);
  border-radius:var(--radius);
  padding:12px;
  cursor:pointer;
}
.card img{
  width:100%;
  height:140px;
  object-fit:cover;
  border-radius:12px;
}
.card h3{font-size:14px}
.meta{font-size:11px;color:var(--muted)}
.tags{display:flex;gap:6px;flex-wrap:wrap}
.tag{
  font-size:10px;
  background:#020617;
  padding:3px 7px;
  border-radius:999px;
  color:var(--muted);
}

/* Lists */
.list .item{
  background:var(--card);
  padding:12px;
  border-radius:12px;
  margin-bottom:10px;
  cursor:pointer;
}

/* Viewer */
#viewer{
  position:fixed;inset:0;
  background:#000;
  display:none;
  z-index:99999;
}
.viewer-bar{
  position:absolute;
  top:8px;left:8px;right:8px;
  display:flex;justify-content:space-between;
}
.viewer-bar button{
  background:#020617;
  color:#fff;
  border:0;
  padding:8px 12px;
  border-radius:10px;
}
#viewer iframe{width:100%;height:100%;border:0}

/* AI Summary */
#summary{
  position:absolute;
  bottom:0;
  left:0;right:0;
  background:rgba(2,6,23,.95);
  padding:14px;
  font-size:13px;
  display:none;
}
#summary strong{color:var(--accent)}
</style>
</head>

<body>

<div class="header">
  <input id="search" class="search" placeholder="🔍 Search across all Debeatzgh blogs…">
  <div class="tabs">
    <button class="active" onclick="showTab('all')">All Blogs</button>
    <button onclick="showTab('trending')">🔥 Trending</button>
    <button onclick="showTab('recent')">🆕 Most Recent</button>
  </div>
</div>

<div id="all"></div>
<div id="trending" style="display:none" class="section list"></div>
<div id="recent" style="display:none" class="section list"></div>

<!-- Viewer -->
<div id="viewer">
  <div class="viewer-bar">
    <button onclick="toggleFull()">⛶ Full</button>
    <button onclick="toggleSummary()">🧠 AI Summary</button>
    <button onclick="closeViewer()">✖ Close</button>
  </div>
  <iframe id="frame"></iframe>
  <div id="summary"></div>
</div>

<script>
const blogs=[
 {name:"Debeatzgh WP",url:"https://debeatzgh.wordpress.com/feed/",badge:"WordPress"},
 {name:"Debeatzgh Blogspot",url:"https://debeatzgh1.blogspot.com/feeds/posts/default?alt=rss",badge:"Blogspot"},
 {name:"Beatzde4",url:"https://beatzde4.blogspot.com/feeds/posts/default?alt=rss",badge:"AI Hustle"},
 {name:"DigiMart GH",url:"https://digimartgh.blogspot.com/feeds/posts/default?alt=rss",badge:"E-Commerce"},
 {name:"Debeatzgh 2",url:"https://debeatzgh2.blogspot.com/feeds/posts/default?alt=rss",badge:"Updates"},
 {name:"My Brands Online",url:"https://mybrandsonline.blogspot.com/feeds/posts/default?alt=rss",badge:"Branding"}
];

const allPosts=[];
const allContainer=document.getElementById("all");
const trending=document.getElementById("trending");
const recent=document.getElementById("recent");

/* Load blogs */
blogs.forEach(async(b)=>{
  const sec=document.createElement("div");
  sec.className="section";
  sec.innerHTML=`<h2>${b.name}<span class="badge">${b.badge}</span></h2><div class="carousel"></div>`;
  allContainer.appendChild(sec);
  const car=sec.querySelector(".carousel");

  const res=await fetch(`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(b.url)}`);
  const data=await res.json();

  data.items.slice(0,10).forEach(p=>{
    const post={...p,source:b.name};
    allPosts.push(post);

    const c=document.createElement("div");
    c.className="card";
    c.innerHTML=`
      <img src="${p.thumbnail||'https://via.placeholder.com/400x200'}">
      <h3>${p.title}</h3>
      <div class="meta">${b.name} · ${p.pubDate.split(" ")[0]}</div>
      <div class="tags">${(p.categories||[]).slice(0,4).map(t=>`<span class="tag">${t}</span>`).join("")}</div>
    `;
    c.onclick=()=>openViewer(p.link,p.description);
    car.appendChild(c);
  });

  setInterval(()=>car.scrollBy({left:280,behavior:"smooth"}),5000);
});

/* Trending & Recent */
setTimeout(()=>{
  const sorted=[...allPosts].sort((a,b)=>new Date(b.pubDate)-new Date(a.pubDate));
  sorted.slice(0,15).forEach(p=>{
    const el=document.createElement("div");
    el.className="item";
    el.innerHTML=`<strong>${p.title}</strong><br><small>${p.source}</small>`;
    el.onclick=()=>openViewer(p.link,p.description);
    recent.appendChild(el);
  });
  sorted.slice(0,15).sort(()=>Math.random()-0.5).forEach(p=>{
    const el=document.createElement("div");
    el.className="item";
    el.innerHTML=`🔥 <strong>${p.title}</strong><br><small>${p.source}</small>`;
    el.onclick=()=>openViewer(p.link,p.description);
    trending.appendChild(el);
  });
},3000);

/* Tabs */
function showTab(id){
  ["all","trending","recent"].forEach(t=>{
    document.getElementById(t).style.display=t===id?"block":"none";
  });
  document.querySelectorAll(".tabs button").forEach(b=>b.classList.remove("active"));
  event.target.classList.add("active");
}

/* Viewer + AI Summary */
const viewer=document.getElementById("viewer");
const frame=document.getElementById("frame");
const summaryBox=document.getElementById("summary");
let currentDesc="";

function openViewer(url,desc){
  frame.src=url;
  currentDesc=desc||"";
  summaryBox.style.display="none";
  viewer.style.display="block";
}
function closeViewer(){
  frame.src="";
  viewer.style.display="none";
}
function toggleFull(){
  !document.fullscreenElement?viewer.requestFullscreen():document.exitFullscreen();
}

/* Simple AI-style summarizer */
function summarize(text){
  const clean=text.replace(/<[^>]+>/g,"");
  const sentences=clean.split(".").filter(s=>s.length>40);
  return sentences.slice(0,3).join(". ")+"…";
}
function toggleSummary(){
  if(!summaryBox.style.display || summaryBox.style.display==="none"){
    summaryBox.innerHTML="<strong>AI Summary:</strong><br>"+summarize(currentDesc);
    summaryBox.style.display="block";
  }else summaryBox.style.display="none";
}

/* Search */
document.getElementById("search").addEventListener("input",e=>{
  const q=e.target.value.toLowerCase();
  recent.innerHTML="";
  if(!q){return;}
  showTab("recent");
  allPosts.filter(p=>p.title.toLowerCase().includes(q))
    .slice(0,20)
    .forEach(p=>{
      const el=document.createElement("div");
      el.className="item";
      el.innerHTML=`<strong>${p.title}</strong><br><small>${p.source}</small>`;
      el.onclick=()=>openViewer(p.link,p.description);
      recent.appendChild(el);
    });
});
</script>

</body>
</html>





# 📚 **AI DIGITAL LIBRARY — 20 Premium Blog Posts + Tools**

Welcome to your **AI-powered Digital Library**, a clean and interactive GitHub-style UI featuring 20 curated guides, toolkits, and AI content resources.

Use the buttons to navigate each post instantly.

---

# 🎨 **📌 Global Styles (GitHub-Safe)**

> These inline CSS styles are fully allowed inside README files.

```html
<style>
.library-card {
  border: 2px solid #e5e7eb;
  border-radius: 15px;
  padding: 20px;
  margin-bottom: 30px;
  background: #fafafa;
}

.card-title {
  font-size: 22px;
  font-weight: bold;
  color: #111827;
  margin-bottom: 10px;
}

.card-desc {
  font-size: 16px;
  color: #374151;
  margin-bottom: 15px;
}

.btn {
  display: inline-block;
  padding: 12px 18px;
  border-radius: 10px;
  margin-right: 10px;
  text-decoration: none;
  font-weight: 600;
}

.btn-view {
  background: #2563eb;
  color: white;
}

.btn-alt {
  background: #10b981;
  color: white;
}
</style>
```

---

# 📘 **20-Post Digital Library (With Thumbnails + Buttons)**

Below are the full 20 posts displayed using GitHub-friendly HTML.

---

## **1. AI Prompts for Startups**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Prompts+for+Startups" width="100%">
<p class="card-desc">A complete beginner-friendly guide helping startup founders use AI to automate tasks, build workflows, and scale faster.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Guide</a>
</div>

---

## **2. AI Prompts for Bloggers**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Prompts+for+Bloggers" width="100%">
<p class="card-desc">Master AI content generation — write posts, headlines, SEO content, and niche ideas effortlessly.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Article</a>
</div>

---

## **3. Side Hustle Starter Kit**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Side+Hustle+Starter+Kit" width="100%">
<p class="card-desc">A complete starter kit for beginners who want to launch profitable online side hustles.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Side-hustle-starter-kit-/">Open Kit</a>
</div>

---

## **4. Online Business Tools & Ideas**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Online+Business+Tools" width="100%">
<p class="card-desc">A full-stack guide with automation tools, AI workflow builders, and business scaling strategies.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Curated-Guides-for-Online-Business-AI-Product-Creation/">Read Guide</a>
</div>

---

## **5. Tech Business Tools & Ideas**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Tech+Business+Tools" width="100%">
<p class="card-desc">Top digital tools & strategies curated for creators, devs, and tech entrepreneurs.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/Home-/">Explore</a>
</div>

---

## **6. AI Affiliate Marketing Guide**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Affiliate+Marketing" width="100%">
<p class="card-desc">A full roadmap on building AI-powered systems to automate affiliate marketing.</p>
<a class="btn btn-view" href="https://beatzde4.blogspot.com/2025/08/ai-affiliate-marketing.html">Open Article</a>
</div>

---

## **7. AI Photography Prompts**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Photography+Prompts" width="100%">
<p class="card-desc">Perfect prompts for AI photography, concept art, and creative projects.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Guide</a>
</div>

---

## **8. Blogging: From Passion to Profits**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Blogging+Profit+Guide" width="100%">
<p class="card-desc">Your ultimate guide to turning blogging passion into long-term income streams.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Resource</a>
</div>

---

## **9. Free AI Web App Promo Post**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Free+AI+Web+App" width="100%">
<p class="card-desc">A full social campaign template: Followers comment “Free App” to claim a custom AI tool.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Details</a>
</div>

---

## **10. Digital Products Affiliate Shop**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Digital+Products+Affiliate+Shop" width="100%">
<p class="card-desc">Your curated store of digital tools, templates, and online business assets.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/">Visit Shop</a>
</div>

---

# 🔟 **Additional 10 Posts (11–20)**

*(All with thumbnails + buttons)*

---

## **11. AI Tools for Content Creators**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Content+Creator+Tools" width="100%">
<p class="card-desc">A curated list of creator-friendly AI tools for video, blogs, graphics, and automation.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read More</a>
</div>

---

## **12. Ultimate AI Automation Tools List**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Automation+Tools" width="100%">
<p class="card-desc">A powerful collection of automation apps for workflows, tasks, and productivity.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open List</a>
</div>

---

## **13. Beginner’s Guide to AI Productivity Hacks**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Productivity+Hacks" width="100%">
<p class="card-desc">Use AI to save time, simplify workflows, and improve daily operations.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Read Article</a>
</div>

---

## **14. Top AI Tools for Students**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+for+Students" width="100%">
<p class="card-desc">From study tools to research engines—AI that boosts academic performance.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Access Guide</a>
</div>

---

## **15. Zero-Budget Business Tools**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Zero+Budget+Tools" width="100%">
<p class="card-desc">Free digital tools to build, launch, and run an online business without spending.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Guide</a>
</div>

---

## **16. AI Branding & Design Toolkit**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Branding+Toolkit" width="100%">
<p class="card-desc">Logos, banner prompts, color palettes, brand kits—all powered by AI.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Explore Toolkit</a>
</div>

---

## **17. AI Video Creation Tools**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Video+Tools" width="100%">
<p class="card-desc">Tools for auto-generating videos, animations, and social media visuals.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Article</a>
</div>

---

## **18. Passive Income with AI**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=Passive+Income+with+AI" width="100%">
<p class="card-desc">Learn how to create digital assets that generate money on autopilot.</p>
<a class="btn btn-view" href="https://beatzde4.blogspot.com/">Read Article</a>
</div>

---

## **19. AI Tools for Social Media Marketing**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Social+Media+Marketing" width="100%">
<p class="card-desc">Automate posts, create viral content, and analyze engagement using AI.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Open Guide</a>
</div>

---

## **20. How to Build an AI Portfolio**

<div class="library-card">
<img src="https://via.placeholder.com/1000x450?text=AI+Portfolio" width="100%">
<p class="card-desc">A step-by-step guide for creating a professional AI portfolio on GitHub.</p>
<a class="btn btn-view" href="https://debeatzgh1.github.io/">Start Now</a>
</div>

---
<!-- Start of OpenWidget (www.openwidget.com) code -->
<script>
  window.__ow = window.__ow || {};
  window.__ow.organizationId = "8dbd48aa-17f7-49aa-b323-23eb6ad358fa";
  window.__ow.integration_name = "manual_settings";
  window.__ow.product_name = "openwidget";   
  ;(function(n,t,c){function i(n){return e._h?e._h.apply(null,n):e._q.push(n)}var e={_q:[],_h:null,_v:"2.0",on:function(){i(["on",c.call(arguments)])},once:function(){i(["once",c.call(arguments)])},off:function(){i(["off",c.call(arguments)])},get:function(){if(!e._h)throw new Error("[OpenWidget] You can't use getters before load.");return i(["get",c.call(arguments)])},call:function(){i(["call",c.call(arguments)])},init:function(){var n=t.createElement("script");n.async=!0,n.type="text/javascript",n.src="https://cdn.openwidget.com/openwidget.js",t.head.appendChild(n)}};!n.__ow.asyncInit&&e.init(),n.OpenWidget=n.OpenWidget||e}(window,document,[].slice))
</script>
<noscript>You need to <a href="https://www.openwidget.com/enable-javascript" rel="noopener nofollow">enable JavaScript</a> to use the communication tool powered by <a href="https://www.openwidget.com/" rel="noopener nofollow" target="_blank">OpenWidget</a></noscript>
<!-- End of OpenWidget code -->
