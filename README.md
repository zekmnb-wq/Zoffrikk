<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zoffrikk — реклама в центре</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background-color: #0a0a0a;
            color: #d0d0d0;
            font-family: 'Courier New', Courier, monospace;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }

        /* Верхняя панель статуса */
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

        .status {
            display: flex;
            align-items: center;
            gap: 8px;
            color: #4ade80;
        }

        .dot {
            width: 8px;
            height: 8px;
            background-color: #4ade80;
            border-radius: 50%;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; box-shadow: 0 0 8px #4ade80; }
            50% { opacity: 0.3; box-shadow: 0 0 2px #4ade80; }
        }

        .clock {
            color: #888;
        }

        /* Заголовок */
        header {
            text-align: center;
            padding: 40px 20px 10px;
        }

        header h1 {
            font-size: 32px;
            letter-spacing: 4px;
            color: #fff;
            text-transform: uppercase;
            margin-bottom: 8px;
        }

        header p {
            color: #666;
            font-size: 14px;
            letter-spacing: 2px;
        }

        /* Центральный блок рекламы */
        .ad-container {
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 40px 20px;
        }

        .ad-slot {
            width: 100%;
            max-width: 700px;
            aspect-ratio: 16 / 9;
            border: 2px dashed #4ade80;
            background-color: #111;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 30px;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .ad-slot:hover {
            background-color: #151515;
            box-shadow: 0 0 30px rgba(74, 222, 128, 0.15);
        }

        .ad-slot::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: repeating-linear-gradient(
                0deg,
                transparent,
                transparent 2px,
                rgba(74, 222, 128, 0.03) 2px,
                rgba(74, 222, 128, 0.03) 4px
            );
            pointer-events: none;
        }

        .ad-label {
            font-size: 12px;
            letter-spacing: 3px;
            color: #4ade80;
            margin-bottom: 15px;
            text-transform: uppercase;
        }

        .ad-title {
            font-size: 28px;
            color: #fff;
            letter-spacing: 2px;
            margin-bottom: 15px;
        }

        .ad-subtitle {
            font-size: 14px;
            color: #888;
            letter-spacing: 1px;
        }

        /* Нижняя панель */
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
            transition: all 0.3s ease;
            display: inline-block;
        }

        .contact a:hover {
            background-color: #4ade80;
            color: #0a0a0a;
            box-shadow: 0 0 20px rgba(74, 222, 128, 0.4);
        }

        .footer-note {
            color: #444;
            letter-spacing: 1px;
        }

        @media (max-width: 600px) {
            header h1 { font-size: 22px; letter-spacing: 2px; }
            .ad-title { font-size: 18px; }
            .ad-slot { aspect-ratio: 4 / 3; }
            .top-bar { font-size: 11px; padding: 12px 15px; }
            footer { flex-direction: column; text-align: center; }
        }
    </style>
</head>
<body>

    <!-- Верхняя панель -->
    <div class="top-bar">
        <div class="status">
            <span class="dot"></span>
            SYSTEM LIVE
        </div>
        <div class="clock" id="clock">00:00:00</div>
    </div>

    <!-- Заголовок -->
    <header>
        <h1>Zoffrikk</h1>
        <p>рекламное пространство</p>
    </header>

    <!-- Центральное место под рекламу -->
    <div class="ad-container">
        <div class="ad-slot" onclick="window.location.href='https://t.me/ваш_телеграм'">
            <div class="ad-label">// слот 01</div>
            <div class="ad-title">МЕСТО СВОБОДНО</div>
            <div class="ad-subtitle">нажмите, чтобы забронировать</div>
        </div>
    </div>

    <!-- Подвал -->
    <footer>
        <div class="footer-note">© ZOFFRIKK 2026</div>
        <div class="contact">
            <a href="https://t.me/ваш_телеграм" target="_blank">Связаться →</a>
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
    </script>

</body>
</html>
