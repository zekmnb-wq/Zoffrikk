<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zoffrikk — реклама в центре</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: #0a0a0a;
            color: #d0d0d0;
            font-family: 'Courier New', Courier, monospace;
            min-height: 100vh;
            overflow-x: hidden;
            padding-bottom: 20px;
        }

        .top-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 30px;
            border-bottom: 1px solid #1f1f1f;
            font-size: 14px;
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .status { display: flex; align-items: center; gap: 8px; color: #4ade80; }

        .dot {
            width: 8px; height: 8px;
            background-color: #4ade80;
            border-radius: 50%;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; box-shadow: 0 0 8px #4ade80; }
            50% { opacity: 0.3; box-shadow: 0 0 2px #4ade80; }
        }

        .clock { color: #888; }

        .stage {
            position: relative;
            width: 100%;
            padding: 30px 70px 20px;
        }

        .viewport { width: 100%; max-width: 1400px; margin: 0 auto; }
        .panel { display: none; }
        .panel.active { display: block; }

        .nav-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: transparent;
            color: #4ade80;
            border: 1px solid #333;
            width: 50px; height: 50px;
            font-size: 22px;
            font-family: 'Courier New', monospace;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 10;
            display: flex; align-items: center; justify-content: center;
        }

        .nav-btn:hover {
            background: #4ade80;
            color: #0a0a0a;
            border-color: #4ade80;
            box-shadow: 0 0 25px rgba(74, 222, 128, 0.5);
        }

        .nav-btn.left { left: 10px; }
        .nav-btn.right { right: 10px; }

        .dot-row {
            display: flex;
            justify-content: center;
            gap: 12px;
            padding: 10px 0 30px;
        }

        .dot-btn {
            width: 10px; height: 10px;
            border-radius: 50%;
            border: 1px solid #4ade80;
            background: transparent;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 0;
        }

        .dot-btn.active {
            background: #4ade80;
            box-shadow: 0 0 12px rgba(74, 222, 128, 0.6);
        }

        .section-title {
            text-align: center;
            font-size: 28px;
            letter-spacing: 8px;
            text-transform: uppercase;
            color: #fff;
            margin: 10px auto 25px;
            padding-bottom: 15px;
            border-bottom: 1px solid #1f1f1f;
            max-width: 1400px;
            width: 90%;
        }

        .section-title span {
            color: #4ade80;
            text-shadow: 0 0 20px rgba(74, 222, 128, 0.4);
        }

        .subsection-title {
            text-align: center;
            font-size: 16px;
            letter-spacing: 4px;
            color: #666;
            margin: 30px auto 15px;
            text-transform: uppercase;
        }

        .grid-box {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            width: 90%;
            max-width: 1400px;
            margin: 0 auto 20px;
        }

        .cell {
            position: relative;
            width: 100%;
            height: 140px;
            border: 2px dashed #4ade80;
            background-color: #111;
            cursor: pointer;
            overflow: hidden;
            transition: box-shadow 0.3s ease;
        }

        .cell:hover { box-shadow: 0 0 30px rgba(74, 222, 128, 0.35); }

        .pic, .vid {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            object-fit: cover;
            z-index: 0;
        }

        .tag {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            z-index: 2;
            color: #4ade80;
            font-size: 13px;
            letter-spacing: 2px;
            text-transform: uppercase;
            padding: 8px 14px;
            border: 1px solid #4ade80;
            background: rgba(10, 10, 10, 0.75);
            white-space: nowrap;
            transition: all 0.3s ease;
        }

        .cell:hover .tag { background: #4ade80; color: #0a0a0a; }

        .tg-slot-btn {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 6;
            color: #4ade80;
            font-size: 11px;
            letter-spacing: 2px;
            text-transform: uppercase;
            padding: 8px 16px;
            border: 1px solid #4ade80;
            background: rgba(10, 10, 10, 0.9);
            text-decoration: none;
            white-space: nowrap;
            transition: all 0.3s ease;
        }

        .tg-slot-btn:hover { background: #4ade80; color: #0a0a0a; }

        .premium-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            width: 90%;
            max-width: 1400px;
            margin: 0 auto 20px;
        }

        .premium {
            position: relative;
            width: 100%;
            height: 260px;
            border: 3px solid #ef4444;
            background-color: #111;
            cursor: pointer;
            overflow: hidden;
            animation: premiumGlow 2.5s ease-in-out infinite;
        }

        @keyframes premiumGlow {
            0%, 100% { box-shadow: 0 0 15px rgba(239, 68, 68, 0.4), 0 0 30px rgba(239, 68, 68, 0.2); }
            50% { box-shadow: 0 0 25px rgba(239, 68, 68, 0.7), 0 0 50px rgba(239, 68, 68, 0.4); }
        }

        .crown-small {
            position: absolute;
            top: 12px; right: 12px;
            font-size: 28px;
            z-index: 5;
            filter: drop-shadow(0 0 8px rgba(239, 68, 68, 0.9));
            pointer-events: none;
        }

        .premium-badge {
            position: absolute;
            top: 12px; left: 12px;
            z-index: 5;
            background: linear-gradient(90deg, #ef4444, #dc2626, #ef4444);
            background-size: 200% auto;
            color: #fff;
            font-size: 11px;
            font-weight: bold;
            letter-spacing: 2px;
            text-transform: uppercase;
            padding: 6px 12px;
            border-radius: 3px;
            animation: badgeShine 3s linear infinite;
            pointer-events: none;
        }

        @keyframes badgeShine { to { background-position: 200% center; } }

        .premium-title {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            z-index: 3;
            font-size: 32px;
            letter-spacing: 6px;
            color: #ef4444;
            text-transform: uppercase;
            text-shadow: 0 0 15px rgba(239, 68, 68, 0.8);
            font-weight: bold;
            pointer-events: none;
        }

        .premium-price {
            position: absolute;
            bottom: 65px; left: 50%;
            transform: translateX(-50%);
            z-index: 5;
            color: #ef4444;
            font-size: 13px;
            letter-spacing: 2px;
            background: rgba(10, 10, 10, 0.85);
            border: 1px solid #ef4444;
            padding: 5px 12px;
            pointer-events: none;
            white-space: nowrap;
        }

        .premium .tg-slot-btn { border-color: #ef4444; color: #ef4444; }
        .premium .tg-slot-btn:hover { background: #ef4444; color: #fff; }

        .king-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
            width: 90%;
            max-width: 1400px;
            margin: 0 auto 20px;
        }

        .royal {
            position: relative;
            width: 100%;
            height: 500px;
            border: 3px solid #facc15;
            background-color: #111;
            cursor: pointer;
            overflow: hidden;
            animation: royalGlow 2.5s ease-in-out infinite;
        }

        @keyframes royalGlow {
            0%, 100% { box-shadow: 0 0 20px rgba(250, 204, 21, 0.4), 0 0 40px rgba(250, 204, 21, 0.2); }
            50% { box-shadow: 0 0 35px rgba(250, 204, 21, 0.8), 0 0 70px rgba(250, 204, 21, 0.4); }
        }

        .crown {
            position: absolute;
            top: 20px; right: 20px;
            font-size: 50px;
            z-index: 5;
            filter: drop-shadow(0 0 15px rgba(250, 204, 21, 0.9));
            animation: crownFloat 2s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes crownFloat {
            0%, 100% { transform: translateY(0) rotate(-5deg); }
            50% { transform: translateY(-8px) rotate(5deg); }
        }

        .royal-badge {
            position: absolute;
            top: 20px; left: 20px;
            z-index: 5;
            background: linear-gradient(90deg, #facc15, #f59e0b, #facc15);
            background-size: 200% auto;
            color: #0a0a0a;
            font-size: 14px;
            font-weight: bold;
            letter-spacing: 3px;
            text-transform: uppercase;
            padding: 10px 18px;
            border-radius: 3px;
            animation: badgeShine 3s linear infinite;
            pointer-events: none;
        }

        .royal-title {
            position: absolute;
            top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            z-index: 3;
            font-size: 64px;
            letter-spacing: 10px;
            color: #facc15;
            text-transform: uppercase;
            text-shadow: 0 0 30px rgba(250, 204, 21, 0.9), 0 0 60px rgba(250, 204, 21, 0.5);
            font-weight: bold;
            pointer-events: none;
            white-space: nowrap;
        }

        .royal-price {
            position: absolute;
            bottom: 80px; left: 50%;
            transform: translateX(-50%);
            z-index: 5;
            color: #facc15;
            font-size: 16px;
            letter-spacing: 3px;
            background: rgba(10, 10, 10, 0.85);
            border: 1px solid #facc15;
            padding: 8px 18px;
            pointer-events: none;
            white-space: nowrap;
        }

        .royal .tg-slot-btn { border-color: #facc15; color: #facc15; font-size: 11px; padding: 8px 16px; }
        .royal .tg-slot-btn:hover { background: #facc15; color: #0a0a0a; }

        .lightbox {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            background: rgba(0, 0, 0, 0.95);
            z-index: 9999;
            align-items: center;
            justify-content: center;
            cursor: zoom-out;
            padding: 20px;
        }

        .lightbox.active { display: flex; }

        .lightbox img, .lightbox video {
            max-width: 95%; max-height: 95%;
            object-fit: contain;
            border: 2px solid #4ade80;
        }

        .lightbox.royal-box img, .lightbox.royal-box video { border-color: #facc15; }
        .lightbox.premium-box img, .lightbox.premium-box video { border-color: #ef4444; }

        .lightbox-close {
            position: absolute;
            top: 20px; right: 30px;
            color: #4ade80;
            font-size: 24px;
            letter-spacing: 2px;
            cursor: pointer;
        }

        footer {
            border-top: 1px solid #1f1f1f;
            padding: 20px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
            font-size: 14px;
        }

        .contact a {
            color: #4ade80;
            text-decoration: none;
            border: 1px solid #4ade80;
            padding: 10px 20px;
            letter-spacing: 2px;
            text-transform: uppercase;
            display: inline-block;
            transition: all 0.3s ease;
        }

        .contact a:hover { background-color: #4ade80; color: #0a0a0a; }

        .footer-note { color: #444; letter-spacing: 1px; }

        @media (max-width: 800px) {
            .stage { padding: 20px 50px; }
            .grid-box { grid-template-columns: repeat(4, 1fr); gap: 8px; }
            .cell { height: 90px; }
            .premium { height: 160px; }
            .royal { height: 280px; }
            .premium-title { font-size: 20px; }
            .royal-title { font-size: 32px; letter-spacing: 4px; }
            .crown { font-size: 30px; }
            .crown-small { font-size: 18px; }
            .tag { font-size: 9px; padding: 4px 8px; }
        }

        @media (max-width: 600px) {
            .top-bar { font-size: 11px; padding: 12px 15px; }
            .section-title { font-size: 16px; letter-spacing: 4px; margin: 10px auto 15px; }
            .subsection-title { font-size: 11px; letter-spacing: 2px; margin: 15px auto 10px; }
            .stage { padding: 15px 38px; }
            .nav-btn { width: 28px; height: 28px; font-size: 14px; }
            .nav-btn.left { left: 3px; }
            .nav-btn.right { right: 3px; }
            .grid-box { grid-template-columns: repeat(4, 1fr); gap: 4px; }
            .cell { height: 90px; border-width: 1px; }
            .tag { font-size: 6px; padding: 2px 4px; letter-spacing: 1px; }
            .tg-slot-btn { font-size: 6px; padding: 2px 4px; bottom: 4px; }
            .premium { height: 180px; }
            .premium-title { font-size: 14px; }
            .premium-price { font-size: 8px; padding: 3px 6px; bottom: 30px; }
            .royal { height: 280px; }
            .royal-title { font-size: 22px; }
            .royal-price { font-size: 10px; padding: 4px 10px; bottom: 40px; }
            footer { flex-direction: column; text-align: center; }
        }
    </style>
</head>
<body>

    <div class="top-bar">
        <div class="status"><span class="dot"></span>SYSTEM LIVE</div>
        <div class="clock" id="clock">00:00:00</div>
    </div>

    <div class="stage">
        <button class="nav-btn left" onclick="goPrev()">‹</button>

        <div class="viewport">

            <!-- ============ ФОТО ============ -->
            <div class="panel active">
                <h2 class="section-title">[ <span>ФОТО</span> ]</h2>

                <!-- ЦАРЬ -->
                <div class="king-grid">
                    <div class="royal" onclick="location.href='#upload'">
                        <span class="crown">👑</span>
                        <span class="royal-badge">ЦАРЬ ФОТО #1</span>
                        <span class="royal-title">ЦАРЬ ФОТО #1</span>
                        <span class="royal-price">70 ₽ / 7 дней</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-100" class="tg-slot-btn" onclick="event.stopPropagation()">📱 В бота</a>
                    </div>
                </div>

                <!-- PREMIUM -->
                <div class="subsection-title">💎 PREMIUM</div>
                <div class="premium-grid">
                    <div class="premium">
                        <span class="crown-small">💎</span>
                        <span class="premium-badge">PREMIUM #1</span>
                        <span class="premium-title">PREMIUM</span>
                        <span class="premium-price">30 ₽ / 3 дня</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-101" class="tg-slot-btn">📱 В бота</a>
                    </div>
                    <div class="premium">
                        <span class="crown-small">💎</span>
                        <span class="premium-badge">PREMIUM #2</span>
                        <span class="premium-title">PREMIUM</span>
                        <span class="premium-price">30 ₽ / 3 дня</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-102" class="tg-slot-btn">📱 В бота</a>
                    </div>
                </div>

                <!-- STANDARD -->
                <div class="subsection-title">⬛ STANDARD</div>
                <div class="grid-box">
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-1" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-2" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-3" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-4" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-5" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-6" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-7" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-8" class="tg-slot-btn">📱 В бота</a></div>
                </div>
            </div>

            <!-- ============ ВИДЕО ============ -->
            <div class="panel">
                <h2 class="section-title">[ <span>ВИДЕО</span> ]</h2>

                <div class="king-grid">
                    <div class="royal">
                        <span class="crown">👑</span>
                        <span class="royal-badge">ЦАРЬ ВИДЕО #1</span>
                        <span class="royal-title">ЦАРЬ ВИДЕО #1</span>
                        <span class="royal-price">70 ₽ / 7 дней</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-200" class="tg-slot-btn">📱 В бота</a>
                    </div>
                </div>

                <div class="subsection-title">💎 PREMIUM</div>
                <div class="premium-grid">
                    <div class="premium">
                        <span class="crown-small">💎</span>
                        <span class="premium-badge">PREMIUM #1</span>
                        <span class="premium-title">PREMIUM</span>
                        <span class="premium-price">30 ₽ / 3 дня</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-201" class="tg-slot-btn">📱 В бота</a>
                    </div>
                    <div class="premium">
                        <span class="crown-small">💎</span>
                        <span class="premium-badge">PREMIUM #2</span>
                        <span class="premium-title">PREMIUM</span>
                        <span class="premium-price">30 ₽ / 3 дня</span>
                        <a href="https://t.me/ZoffrikkBot?start=СЛОТ-202" class="tg-slot-btn">📱 В бота</a>
                    </div>
                </div>

                <div class="subsection-title">⬛ STANDARD</div>
                <div class="grid-box">
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-11" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-12" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-13" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-14" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-15" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-16" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-17" class="tg-slot-btn">📱 В бота</a></div>
                    <div class="cell"><span class="tag">10 ₽ / 1 день</span><a href="https://t.me/ZoffrikkBot?start=СЛОТ-18" class="tg-slot-btn">📱 В бота</a></div>
                </div>
            </div>

        </div>

        <button class="nav-btn right" onclick="goNext()">›</button>
    </div>

    <div class="dot-row">
        <button class="dot-btn active" onclick="goTo(0)"></button>
        <button class="dot-btn" onclick="goTo(1)"></button>
    </div>

    <footer>
        <div class="footer-note">© ZOFFRIKK 2026</div>
        <div class="contact">
            <a href="https://t.me/anonaskbot?start=CgcCzK0SmqALCVS" target="_blank">Связаться → @Ivanee_tg</a>
        </div>
    </footer>

    <script>
        function updateClock() {
            var now = new Date();
            var h = String(now.getHours()).padStart(2, '0');
            var m = String(now.getMinutes()).padStart(2, '0');
            var s = String(now.getSeconds()).padStart(2, '0');
            document.getElementById('clock').textContent = h + ':' + m + ':' + s;
        }
        setInterval(updateClock, 1000);
        updateClock();

        var current = 0;
        var panels = document.querySelectorAll('.panel');
        var dots = document.querySelectorAll('.dot-btn');
        var total = panels.length;

        function showPanel(index) {
            if (total === 0) return;
            if (index < 0) index = total - 1;
            if (index >= total) index = 0;

            for (var i = 0; i < panels.length; i++) {
                if (i === index) panels[i].classList.add('active');
                else panels[i].classList.remove('active');
            }
            for (var j = 0; j < dots.length; j++) {
                if (j === index) dots[j].classList.add('active');
                else dots[j].classList.remove('active');
            }
            current = index;
        }

        function goNext() { showPanel(current + 1); }
        function goPrev() { showPanel(current - 1); }
        function goTo(i)  { showPanel(i); }

        document.addEventListener('keydown', function(e) {
            if (e.key === 'ArrowRight') goNext();
            if (e.key === 'ArrowLeft')  goPrev();
        });

        var touchStartX = 0;
        var stage = document.querySelector('.stage');
        if (stage) {
            stage.addEventListener('touchstart', function(e) {
                touchStartX = e.changedTouches[0].screenX;
            }, { passive: true });
            stage.addEventListener('touchend', function(e) {
                var diff = e.changedTouches[0].screenX - touchStartX;
                if (Math.abs(diff) > 50) {
                    if (diff < 0) goNext();
                    else goPrev();
                }
            }, { passive: true });
        }
    </script>

</body>
</html>
