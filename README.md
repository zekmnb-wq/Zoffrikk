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
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
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

        .slide-title {
            text-align: center;
            font-size: 28px;
            letter-spacing: 8px;
            text-transform: uppercase;
            color: #fff;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 1px solid #1f1f1f;
        }

        .slide-title span {
            color: #4ade80;
            text-shadow: 0 0 20px rgba(74, 222, 128, 0.4);
        }

        .carousel {
            position: relative;
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 40px 70px 30px;
            overflow: hidden;
        }

        .slides-viewport {
            width: 100%;
            max-width: 1400px;
            position: relative;
        }

        .slide { display: none; animation: slideIn 0.45s ease; }
        .slide.active { display: block; }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(40px); }
            to   { opacity: 1; transform: translateX(0); }
        }

        .slide.back { animation: slideInBack 0.45s ease; }

        @keyframes slideInBack {
            from { opacity: 0; transform: translateX(-40px); }
            to   { opacity: 1; transform: translateX(0); }
        }

        .nav-arrow {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: transparent;
            color: #4ade80;
            border: 1px solid #333;
            width: 50px;
            height: 50px;
            font-size: 22px;
            font-family: 'Courier New', monospace;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 10;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .nav-arrow:hover {
            background: #4ade80;
            color: #0a0a0a;
            border-color: #4ade80;
            box-shadow: 0 0 25px rgba(74, 222, 128, 0.5);
        }

        .nav-arrow.prev { left: 10px; }
        .nav-arrow.next { right: 10px; }

        .dots {
            display: flex;
            justify-content: center;
            gap: 12px;
            padding: 0 0 30px;
        }

        .dot-indicator {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            border: 1px solid #4ade80;
            background: transparent;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 0;
        }

        .dot-indicator.active {
            background: #4ade80;
            box-shadow: 0 0 12px rgba(74, 222, 128, 0.6);
        }

        .dot-indicator:hover { background: rgba(74, 222, 128, 0.5); }

        .ad-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            width: 100%;
        }

        .ad-slot {
            width: 100%;
            aspect-ratio: 16 / 9;
            border: 2px dashed #4ade80;
            background-color: #111;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 0;
            transition: all 0.3s ease;
            cursor: zoom-in;
            position: relative;
            overflow: hidden;
        }

        .ad-slot:hover { box-shadow: 0 0 30px rgba(74, 222, 128, 0.25); }

        /* Тестовый слот — жёлтая рамка */
        .ad-slot.test {
            border-color: #facc15;
        }

        .ad-slot.test:hover {
            box-shadow: 0 0 30px rgba(250, 204, 21, 0.35);
        }

        .ad-slot::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.45);
            z-index: 1;
            pointer-events: none;
            transition: background 0.3s ease;
        }

        .ad-slot:hover::before { background: rgba(0, 0, 0, 0.25); }

        .ad-image, .ad-video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0; left: 0;
            z-index: 0;
        }

        .slot-label {
            position: relative;
            z-index: 2;
            color: #4ade80;
            font-size: 15px;
            letter-spacing: 4px;
            text-transform: uppercase;
            padding: 10px 20px;
            border: 1px solid #4ade80;
            background: rgba(10, 10, 10, 0.7);
            backdrop-filter: blur(2px);
            transition: all 0.3s ease;
            pointer-events: none;
        }

        .ad-slot:hover .slot-label {
            background: #4ade80;
            color: #0a0a0a;
            box-shadow: 0 0 25px rgba(74, 222, 128, 0.6);
            letter-spacing: 5px;
        }

        /* Тестовая надпись */
        .ad-slot.test .slot-label {
            color: #facc15;
            border-color: #facc15;
        }

        .ad-slot.test:hover .slot-label {
            background: #facc15;
            color: #0a0a0a;
            box-shadow: 0 0 25px rgba(250, 204, 21, 0.7);
        }

        .music-placeholder {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 60px 20px;
            min-height: 40vh;
        }

        .music-placeholder .soon {
            font-size: clamp(48px, 12vw, 120px);
            letter-spacing: 20px;
            color: #4ade80;
            text-transform: uppercase;
            text-shadow: 0 0 40px rgba(74, 222, 128, 0.4);
            animation: glow 2s ease-in-out infinite;
        }

        .music-placeholder .sub {
            margin-top: 30px;
            color: #555;
            letter-spacing: 6px;
            font-size: 14px;
            text-transform: uppercase;
        }

        @keyframes glow {
            0%, 100% { text-shadow: 0 0 40px rgba(74, 222, 128, 0.4); }
            50% { text-shadow: 0 0 80px rgba(74, 222, 128, 0.8); }
        }

        .lightbox {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(0, 0, 0, 0.95);
            z-index: 9999;
            align-items: center;
            justify-content: center;
            cursor: zoom-out;
            padding: 20px;
        }

        .lightbox.active { display: flex; }

        .lightbox img, .lightbox video {
            max-width: 95%;
            max-height: 95%;
            object-fit: contain;
            border: 2px solid #4ade80;
            box-shadow: 0 0 50px rgba(74, 222, 128, 0.3);
            animation: zoomIn 0.25s ease;
        }

        @keyframes zoomIn {
            from { transform: scale(0.85); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }

        .lightbox-close {
            position: absolute;
            top: 20px; right: 30px;
            color: #4ade80;
            font-size: 24px;
            letter-spacing: 2px;
            cursor: pointer;
            font-family: 'Courier New', monospace;
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
            margin-top: auto;
        }

        .hits-counter { display: flex; align-items: center; opacity: 0.8; transition: opacity 0.3s ease; }
        .hits-counter:hover { opacity: 1; }

        .contact a {
            color: #4ade80;
            text-decoration: none;
            border: 1px solid #4ade80;
            padding: 10px 20px;
            letter-spacing: 2px;
            text-transform: uppercase;
            transition: all 0.3s ease;
            display: inline-block;
        }

        .contact a:hover {
            background-color: #4ade80;
            color: #0a0a0a;
            box-shadow: 0 0 20px rgba(74, 222, 128, 0.4);
        }

        .footer-note { color: #444; letter-spacing: 1px; }

        @media (max-width: 800px) {
            .ad-container { grid-template-columns: repeat(2, 1fr); gap: 12px; }
            .carousel { padding: 25px 55px; }
            .nav-arrow { width: 40px; height: 40px; font-size: 18px; }
            .slot-label { font-size: 12px; letter-spacing: 2px; padding: 7px 12px; }
            .slide-title { font-size: 22px; letter-spacing: 5px; }
        }

        @media (max-width: 600px) {
            .ad-container { grid-template-columns: 1fr; }
            .top-bar { font-size: 11px; padding: 12px 15px; }
            .carousel { padding: 15px 45px; }
            .nav-arrow { width: 34px; height: 34px; font-size: 16px; }
            .nav-arrow.prev { left: 4px; }
            .nav-arrow.next { right: 4px; }
            .slot-label { font-size: 11px; letter-spacing: 2px; }
            .slide-title { font-size: 18px; letter-spacing: 3px; margin-bottom: 15px; padding-bottom: 10px; }
            footer { flex-direction: column; text-align: center; }
        }
    </style>
</head>
<body>

    <div class="top-bar">
        <div class="status">
            <span class="dot"></span>
            SYSTEM LIVE
        </div>
        <div class="clock" id="clock">00:00:00</div>
    </div>

    <div class="carousel">

        <button class="nav-arrow prev" onclick="goPrev()">‹</button>

        <div class="slides-viewport">

            <!-- Слайд 1: ФОТО (9 обычных + 1 тестовый = 10) -->
            <div class="slide active" data-index="0">
                <div class="slide-title">[ <span>ФОТО</span> ]</div>
                <div class="ad-container">
                    <div class="ad-slot" onclick="openLightbox('photo-1')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-2')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-3')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-4')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-5')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-6')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-7')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-8')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-9')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <!-- ТЕСТОВЫЙ СЛОТ -->
                    <div class="ad-slot test" onclick="openLightbox('photo-test')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Тест</span></div>
                </div>
            </div>

            <!-- Слайд 2: ВИДЕО (9 обычных + 1 тестовый = 10) -->
            <div class="slide" data-index="1">
                <div class="slide-title">[ <span>ВИДЕО</span> ]</div>
                <div class="ad-container">
                    <div class="ad-slot" onclick="openLightbox('video-1')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-2')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-3')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-4')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-5')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-6')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-7')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-8')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-9')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <!-- ТЕСТОВЫЙ СЛОТ -->
                    <div class="ad-slot test" onclick="openLightbox('video-test')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Тест</span></div>
                </div>
            </div>

            <!-- Слайд 3: МУЗЫКА -->
            <div class="slide" data-index="2">
                <div class="slide-title">[ <span>МУЗЫКА</span> ]</div>
                <div class="music-placeholder">
                    <div class="soon">СКОРО</div>
                    <div class="sub">// музыкальный раздел в разработке //</div>
                </div>
            </div>

        </div>

        <button class="nav-arrow next" onclick="goNext()">›</button>
    </div>

    <div class="dots">
        <button class="dot-indicator active" onclick="goTo(0)"></button>
        <button class="dot-indicator" onclick="goTo(1)"></button>
        <button class="dot-indicator" onclick="goTo(2)"></button>
    </div>

    <!-- Lightbox'ы: фото -->
    <div class="lightbox" id="lightbox-photo-1" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-2" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-3" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-4" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-5" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-6" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-7" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-8" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-9" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-test" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>

    <!-- Lightbox'ы: видео -->
    <div class="lightbox" id="lightbox-video-1" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-2" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-3" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-4" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-5" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-6" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-7" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-8" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-9" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-test" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>

    <footer>
        <div class="footer-note">© ZOFFRIKK 2026</div>

        <div class="hits-counter">
            <a href="https://hits.sh/zekmnb-wq.github.io/Zoffrikk/">
                <img alt="Хиты" src="https://hits.sh/zekmnb-wq.github.io/Zoffrikk.svg"/>
            </a>
        </div>

        <div class="contact">
            <a href="https://t.me/anonaskbot?start=CgcCzK0SmqALCVS" target="_blank">Связаться → @Ivanee_tg</a>
        </div>
    </footer>

    <script>
        function updateClock() {
            const now = new Date();
            const h = String(now.getHours()).padStart(2, '0');
            const m = String(now.getMinutes()).padStart(2, '0');
            const s = String(now.getSeconds()).padStart(2, '0');
            document.getElementById('clock').textContent = `${h}:${m}:${s}`;
        }
        updateClock();
        setInterval(updateClock, 1000);

        let current = 0;
        const slides = document.querySelectorAll('.slide');
        const dots = document.querySelectorAll('.dot-indicator');
        const total = slides.length;

        function showSlide(index, direction) {
            if (index < 0) index = total - 1;
            if (index >= total) index = 0;

            slides.forEach(function(s) { s.classList.remove('active', 'back'); });
            dots.forEach(function(d) { d.classList.remove('active'); });

            const slide = slides[index];
            slide.classList.add('active');
            if (direction === 'back') slide.classList.add('back');
            dots[index].classList.add('active');

            slides.forEach(function(s, i) {
                if (i !== index) {
                    s.querySelectorAll('video').forEach(function(v) { v.pause(); });
                }
            });

            current = index;
        }

        function goNext() { showSlide(current + 1, 'next'); }
        function goPrev() { showSlide(current - 1, 'back'); }
        function goTo(i)  { showSlide(i, i > current ? 'next' : 'back'); }

        document.addEventListener('keydown', function(e) {
            if (e.key === 'ArrowRight') goNext();
            if (e.key === 'ArrowLeft')  goPrev();
            if (e.key === 'Escape')     closeLightbox();
        });

        let touchStartX = 0;
        const carousel = document.querySelector('.carousel');
        carousel.addEventListener('touchstart', function(e) {
            touchStartX = e.changedTouches[0].screenX;
        }, { passive: true });
        carousel.addEventListener('touchend', function(e) {
            const diff = e.changedTouches[0].screenX - touchStartX;
            if (Math.abs(diff) > 50) {
                if (diff < 0) goNext();
                else goPrev();
            }
        }, { passive: true });

        function openLightbox(id) {
            document.getElementById('lightbox-' + id).classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closeLightbox() {
            document.querySelectorAll('.lightbox').forEach(function(el) {
                el.classList.remove('active');
            });
            document.body.style.overflow = '';
        }
    </script>

</body>
</html>
