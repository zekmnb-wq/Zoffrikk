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

        /* ===== Сетка слотов 3×3 ===== */
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

        /* ============================================
           ЦАРСКИЙ СЛОТ 👑
           ============================================ */
        .ad-slot.king {
            border: 2px solid #facc15;
            animation: kingGlow 2.5s ease-in-out infinite;
        }

        @keyframes kingGlow {
            0%, 100% {
                box-shadow: 0 0 20px rgba(250, 204, 21, 0.4),
                            0 0 40px rgba(250, 204, 21, 0.2);
            }
            50% {
                box-shadow: 0 0 35px rgba(250, 204, 21, 0.8),
                            0 0 70px rgba(250, 204, 21, 0.4);
            }
        }

        .ad-slot.king:hover {
            transform: scale(1.02);
            box-shadow: 0 0 50px rgba(250, 204, 21, 1),
                        0 0 90px rgba(250, 204, 21, 0.5);
        }

        /* Царь занимает 2×2 ячейки */
        .ad-slot.king {
            grid-column: span 2;
            grid-row: span 2;
            aspect-ratio: auto;
            min-height: 100%;
        }

        /* Затемнение внутри царского слота — светлее, чтобы фото было видно */
        .ad-slot.king::before {
            background: rgba(0, 0, 0, 0.25);
        }

        .ad-slot.king:hover::before {
            background: rgba(0, 0, 0, 0.1);
        }

        /* Корона в углу */
        .king-crown {
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 32px;
            z-index: 5;
            filter: drop-shadow(0 0 10px rgba(250, 204, 21, 0.9));
            animation: crownFloat 2s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes crownFloat {
            0%, 100% { transform: translateY(0) rotate(-5deg); }
            50% { transform: translateY(-6px) rotate(5deg); }
        }

        /* Бейдж «ЦАРЬ ФОТО» */
        .king-badge {
            position: absolute;
            top: 15px;
            left: 15px;
            z-index: 5;
            background: linear-gradient(90deg, #facc15, #f59e0b, #facc15);
            background-size: 200% auto;
            color: #0a0a0a;
            font-size: 13px;
            font-weight: bold;
            letter-spacing: 3px;
            text-transform: uppercase;
            padding: 8px 16px;
            border-radius: 3px;
            animation: badgeShine 3s linear infinite;
            box-shadow: 0 0 20px rgba(250, 204, 21, 0.6);
            pointer-events: none;
        }

        @keyframes badgeShine {
            to { background-position: 200% center; }
        }

        /* Таймер до смены царя */
        .king-timer {
            position: absolute;
            bottom: 15px;
            left: 15px;
            z-index: 5;
            color: #facc15;
            font-size: 12px;
            letter-spacing: 2px;
            text-transform: uppercase;
            background: rgba(10, 10, 10, 0.85);
            border: 1px solid #facc15;
            padding: 6px 12px;
            pointer-events: none;
        }

        .king-timer b { color: #fff; }

        /* Счётчик просмотров */
        .king-views {
            position: absolute;
            bottom: 15px;
            right: 15px;
            z-index: 5;
            color: #facc15;
            font-size: 12px;
            letter-spacing: 2px;
            background: rgba(10, 10, 10, 0.85);
            border: 1px solid #facc15;
            padding: 6px 12px;
            pointer-events: none;
        }

        .king-views b { color: #fff; }

        /* Центральная надпись «ЦАРЬ ФОТО» */
        .king-title {
            position: relative;
            z-index: 3;
            font-size: clamp(20px, 3vw, 34px);
            letter-spacing: 8px;
            color: #facc15;
            text-transform: uppercase;
            text-shadow: 0 0 20px rgba(250, 204, 21, 0.8),
                         0 0 40px rgba(250, 204, 21, 0.4);
            font-weight: bold;
            pointer-events: none;
        }

        .king-subtitle {
            position: relative;
            z-index: 3;
            margin-top: 10px;
            font-size: clamp(10px, 1.2vw, 13px);
            letter-spacing: 4px;
            color: #fde68a;
            text-transform: uppercase;
            pointer-events: none;
        }

        /* Музыка — «скоро» */
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

        /* Lightbox */
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

        /* Царский лайтбокс — золотая рамка */
        .lightbox.king-box img,
        .lightbox.king-box video {
            border: 3px solid #facc15;
            box-shadow: 0 0 60px rgba(250, 204, 21, 0.6),
                        0 0 120px rgba(250, 204, 21, 0.3);
        }

        .lightbox.king-box .lightbox-close {
            color: #facc15;
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

        /* ===== Планшет ===== */
        @media (max-width: 900px) {
            .carousel { padding: 30px 55px; }
            .nav-arrow { width: 40px; height: 40px; font-size: 18px; }
            .slide-title { font-size: 24px; letter-spacing: 6px; margin-bottom: 18px; padding-bottom: 12px; }
            .slot-label { font-size: 13px; letter-spacing: 3px; padding: 8px 14px; }
            .king-crown { font-size: 24px; top: 10px; right: 10px; }
            .king-badge { font-size: 10px; padding: 6px 10px; letter-spacing: 2px; top: 10px; left: 10px; }
            .king-timer, .king-views { font-size: 10px; padding: 4px 8px; bottom: 10px; }
            .king-timer { left: 10px; }
            .king-views { right: 10px; }
        }

        /* ===== Телефон ===== */
        @media (max-width: 600px) {
            .top-bar { font-size: 11px; padding: 12px 15px; }
            .carousel { padding: 20px 40px 15px; }
            .nav-arrow { width: 30px; height: 30px; font-size: 14px; }
            .nav-arrow.prev { left: 3px; }
            .nav-arrow.next { right: 3px; }
            .slide-title { font-size: 16px; letter-spacing: 4px; margin-bottom: 12px; padding-bottom: 8px; }

            .ad-container { grid-template-columns: repeat(3, 1fr); gap: 6px; }

            .ad-slot { border-width: 1px; }

            .slot-label {
                font-size: 7px;
                letter-spacing: 1px;
                padding: 3px 6px;
            }

            .ad-slot:hover .slot-label { letter-spacing: 1px; }

            .ad-slot.king { border-width: 2px; }

            .king-crown { font-size: 16px; top: 5px; right: 5px; }
            .king-badge {
                font-size: 7px;
                padding: 3px 6px;
                letter-spacing: 1px;
                top: 5px;
                left: 5px;
            }
            .king-timer, .king-views {
                font-size: 7px;
                padding: 2px 5px;
                bottom: 5px;
                letter-spacing: 1px;
            }
            .king-timer { left: 5px; }
            .king-views { right: 5px; }
            .king-title { font-size: 14px; letter-spacing: 3px; }
            .king-subtitle { font-size: 8px; letter-spacing: 2px; margin-top: 5px; }

            .dots { padding: 0 0 15px; gap: 8px; }
            .dot-indicator { width: 8px; height: 8px; }
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

            <!-- ===== Слайд 1: ФОТО ===== -->
            <div class="slide active" data-index="0">
                <div class="slide-title">[ <span>ФОТО</span> ]</div>
                <div class="ad-container">

                    <!-- 👑 ЦАРСКИЙ СЛОТ (2×2) -->
                    <div class="ad-slot king" id="king-photo" onclick="openLightbox('photo-king')">
                        <img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'">
                        <span class="king-crown">👑</span>
                        <span class="king-badge">Царь фото</span>
                        <div class="king-title">ЦАРЬ ФОТО</div>
                        <div class="king-subtitle">// премиум место //</div>
                        <div class="king-timer">До смены: <b id="king-timer-photo">00д 00ч 00м</b></div>
                        <div class="king-views">Просмотров: <b id="king-views-photo">0</b></div>
                    </div>

                    <div class="ad-slot" onclick="openLightbox('photo-1')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-2')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-3')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-4')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-5')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-6')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-7')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('photo-8')"><img src="6.jpg" alt="" class="ad-image" onerror="this.style.display='none'"><span class="slot-label">Занять слот</span></div>

                </div>
            </div>

            <!-- ===== Слайд 2: ВИДЕО ===== -->
            <div class="slide" data-index="1">
                <div class="slide-title">[ <span>ВИДЕО</span> ]</div>
                <div class="ad-container">

                    <!-- 👑 ЦАРСКИЙ СЛОТ (2×2) -->
                    <div class="ad-slot king" id="king-video" onclick="openLightbox('video-king')">
                        <video class="ad-video" autoplay muted loop playsinline>
                            <source src="IMG_3985.MP4" type="video/mp4">
                        </video>
                        <span class="king-crown">👑</span>
                        <span class="king-badge">Царь видео</span>
                        <div class="king-title">ЦАРЬ ВИДЕО</div>
                        <div class="king-subtitle">// премиум место //</div>
                        <div class="king-timer">До смены: <b id="king-timer-video">00д 00ч 00м</b></div>
                        <div class="king-views">Просмотров: <b id="king-views-video">0</b></div>
                    </div>

                    <div class="ad-slot" onclick="openLightbox('video-1')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-2')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-3')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-4')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-5')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-6')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-7')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>
                    <div class="ad-slot" onclick="openLightbox('video-8')"><video class="ad-video" autoplay muted loop playsinline><source src="IMG_3985.MP4" type="video/mp4"></video><span class="slot-label">Занять слот</span></div>

                </div>
            </div>

            <!-- ===== Слайд 3: МУЗЫКА ===== -->
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

    <!-- ===== Lightbox'ы: фото ===== -->
    <div class="lightbox king-box" id="lightbox-photo-king" onclick="closeLightbox()">
        <div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div>
        <img src="6.jpg" onclick="event.stopPropagation()">
    </div>
    <div class="lightbox" id="lightbox-photo-1" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-2" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-3" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-4" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-5" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-6" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-7" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>
    <div class="lightbox" id="lightbox-photo-8" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><img src="6.jpg" onclick="event.stopPropagation()"></div>

    <!-- ===== Lightbox'ы: видео ===== -->
    <div class="lightbox king-box" id="lightbox-video-king" onclick="closeLightbox()">
        <div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div>
        <video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video>
    </div>
    <div class="lightbox" id="lightbox-video-1" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-2" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-3" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-4" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-5" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-6" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-7" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>
    <div class="lightbox" id="lightbox-video-8" onclick="closeLightbox()"><div class="lightbox-close">[ ЗАКРЫТЬ ✕ ]</div><video controls onclick="event.stopPropagation()"><source src="IMG_3985.MP4" type="video/mp4"></video></div>

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
        /* ===== Часы ===== */
        function updateClock() {
            const now = new Date();
            const h = String(now.getHours()).padStart(2, '0');
            const m = String(now.getMinutes()).padStart(2, '0');
            const s = String(now.getSeconds()).padStart(2, '0');
            document.getElementById('clock').textContent = `${h}:${m}:${s}`;
        }
        updateClock();
        setInterval(updateClock, 1000);

        /* ===== Карусель ===== */
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

        /* ===== Lightbox ===== */
        function openLightbox(id) {
            document.getElementById('lightbox-' + id).classList.add('active');
            document.body.style.overflow = 'hidden';

            /* Счётчик просмотров для царского слота */
            if (id === 'photo-king') {
                incrementViews('photo');
            } else if (id === 'video-king') {
                incrementViews('video');
            }
        }

        function closeLightbox() {
            document.querySelectorAll('.lightbox').forEach(function(el) {
                el.classList.remove('active');
            });
            document.body.style.overflow = '';
        }

        /* ============================================================
           ЦАРСКИЙ ФУНКЦИОНАЛ
           ============================================================ */

        /* --- 1. Счётчик просмотров (localStorage) --- */
        function incrementViews(type) {
            const key = 'king-views-' + type;
            let views = parseInt(localStorage.getItem(key) || '0', 10);
            views += 1;
            localStorage.setItem(key, views);
            updateViewsDisplay();
        }

        function updateViewsDisplay() {
            const photoViews = parseInt(localStorage.getItem('king-views-photo') || '0', 10);
            const videoViews = parseInt(localStorage.getItem('king-views-video') || '0', 10);
            const photoEl = document.getElementById('king-views-photo');
            const videoEl = document.getElementById('king-views-video');
            if (photoEl) photoEl.textContent = photoViews;
            if (videoEl) videoEl.textContent = videoViews;
        }

        /* --- 2. Таймер до смены царя (раз в неделю, в понедельник 00:00) --- */
        function getNextMonday() {
            const now = new Date();
            const day = now.getDay(); /* 0=вс, 1=пн, ... */
            const daysUntilMonday = (day === 0 ? 1 : 8 - day) % 7 || 7;
            const next = new Date(now);
            next.setDate(now.getDate() + daysUntilMonday);
            next.setHours(0, 0, 0, 0);
            return next;
        }

        function updateKingTimer() {
            const now = new Date();
            const next = getNextMonday();
            let diff = Math.max(0, next - now);

            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            diff -= days * (1000 * 60 * 60 * 24);
            const hours = Math.floor(diff / (1000 * 60 * 60));
            diff -= hours * (1000 * 60 * 60);
            const minutes = Math.floor(diff / (1000 * 60));

            const str = `${String(days).padStart(2, '0')}д ${String(hours).padStart(2, '0')}ч ${String(minutes).padStart(2, '0')}м`;
            const photoEl = document.getElementById('king-timer-photo');
            const videoEl = document.getElementById('king-timer-video');
            if (photoEl) photoEl.textContent = str;
            if (videoEl) videoEl.textContent = str;
        }

        updateKingTimer();
        setInterval(updateKingTimer, 30000); /* обновляем каждые 30 сек */

        updateViewsDisplay();

        /* --- 3. Определение, кто сейчас царь (по номеру недели) --- */
        /* Если хочешь, чтобы царь был фиксированный — просто не трогай.
           Если хочешь случайного царя каждую неделю — раскомментируй ниже. */

        /*
        const weekNumber = Math.floor(Date.now() / (7 * 24 * 60 * 60 * 1000));
        const kingSlot = weekNumber % 9;
        */

    </script>

</body>
</html>
