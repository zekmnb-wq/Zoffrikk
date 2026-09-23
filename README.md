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

        header { text-align: center; padding: 40px 20px 10px; }

        header h1 {
            font-size: 32px;
            letter-spacing: 4px;
            color: #fff;
            text-transform: uppercase;
            margin-bottom: 8px;
        }

        header p { color: #666; font-size: 14px; letter-spacing: 2px; }

        /* Контейнер для двух слотов рядом */
        .ad-container {
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 25px;
            padding: 40px 20px;
            flex-wrap: wrap;
        }

        .ad-slot {
            width: 100%;
            max-width: 500px;
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
            background: rgba(0, 0, 0, 0.35);
            z-index: 1;
            pointer-events: none;
        }

        /* Картинка внутри слота */
        .ad-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0; left: 0;
            z-index: 0;
        }

        /* Видео внутри слота */
        .ad-video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0; left: 0;
            z-index: 0;
            border: none;
        }

        /* Модальное окно для увеличения */
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

        .lightbox img {
            max-width: 95%;
            max-height: 95%;
            object-fit: contain;
            border: 2px solid #4ade80;
            box-shadow: 0 0 50px rgba(74, 222, 128, 0.3);
            animation: zoomIn 0.25s ease;
        }

        .lightbox video {
            max-width: 95%;
            max-height: 95%;
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
        }

        .hits-counter {
            display: flex;
            align-items: center;
            opacity: 0.8;
            transition: opacity 0.3s ease;
        }

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

        @media (max-width: 600px) {
            header h1 { font-size: 22px; letter-spacing: 2px; }
            .ad-slot { max-width: 100%; aspect-ratio: 4 / 3; }
            .top-bar { font-size: 11px; padding: 12px 15px; }
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

    <header>
        <h1>Zoffrikk</h1>
        <p>рекламное пространство</p>
    </header>

 <!---------------------------------------------------------------------------------------------------------------->

    <!-- Два слота рядом -->
    <div class="ad-container">

        <!-- Слот 1: фото -->
        <div class="ad-slot" onclick="openLightbox('image')">
            <img src="4.png" alt="Реклама" class="ad-image">
        </div>

        <!-- Слот 2: локальное видео mp4 -->
        <!-- Загрузите файл video.mp4 в корень репозитория рядом с index.html -->
        <div class="ad-slot" onclick="openLightbox('video')">
            <video class="ad-video" autoplay muted loop playsinline>
                <source src="5.mp4" type="video/mp4">
                Ваш браузер не поддерживает видео.
            </video>
        </div>

    </div>

    <!-- Модальное окно для фото -->
    <div class="lightbox" id="lightbox-image" onclick="closeLightbox()">
        <div class="lightbox-close" onclick="closeLightbox()">[ ЗАКРЫТЬ ✕ ]</div>
        <img src="4.png" alt="Реклама" onclick="event.stopPropagation()">
    </div>

    <!-- Модальное окно для видео -->
    <div class="lightbox" id="lightbox-video" onclick="closeLightbox()">
        <div class="lightbox-close" onclick="closeLightbox()">[ ЗАКРЫТЬ ✕ ]</div>
        <video controls onclick="event.stopPropagation()">
            <source src="5.mp4" type="video/mp4">
            Ваш браузер не поддерживает видео.
        </video>
    </div>

     <!-------------------------------------------------------------------------------------------------------------->

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

        function openLightbox(type) {
            document.getElementById('lightbox-' + type).classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closeLightbox() {
            document.querySelectorAll('.lightbox').forEach(function(el) {
                el.classList.remove('active');
            });
            document.body.style.overflow = '';
        }

        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') closeLightbox();
        });
    </script>

</body>
</html>
