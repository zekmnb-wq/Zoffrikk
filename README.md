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

        .ad-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0; left: 0;
            z-index: 0;
        }

        .ad-video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            position: absolute;
            top: 0; left: 0;
            z-index: 0;
        }

        .ad-content {
            position: relative;
            z-index: 2;
            padding: 20px;
            pointer-events: none;
        }

        .ad-label {
            font-size: 13px;
            letter-spacing: 4px;
            color: #4ade80;
            text-transform: uppercase;
            text-shadow: 0 0 10px rgba(0,0,0,0.9);
            background: rgba(0,0,0,0.5);
            padding: 6px 14px;
            display: inline-block;
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

        /* Чат */
        #chat-container {
            max-width: 700px;
            margin: 40px auto;
            border: 1px solid #1f1f1f;
            background: #111;
            padding: 20px;
            width: calc(100% - 40px);
        }

        .chat-title {
            color: #4ade80;
            letter-spacing: 2px;
            margin-bottom: 15px;
            font-size: 14px;
            text-transform: uppercase;
        }

        #chat-messages {
            height: 300px;
            overflow-y: auto;
            border: 1px solid #1f1f1f;
            padding: 10px;
            margin-bottom: 15px;
            color: #d0d0d0;
            font-size: 14px;
        }

        #chat-messages div { margin-bottom: 6px; line-height: 1.4; word-wrap: break-word; }

        .chat-inputs { display: flex; gap: 10px; }

        .chat-inputs input {
            background: #0a0a0a;
            border: 1px solid #333;
            color: #fff;
            padding: 10px;
            font-family: 'Courier New', monospace;
        }

        #chat-name { width: 30%; }

        #chat-input { flex: 1; }

        .chat-inputs button {
            background: #4ade80;
            border: none;
            color: #0a0a0a;
            padding: 10px 20px;
            cursor: pointer;
            font-weight: bold;
            font-family: 'Courier New', monospace;
        }

        .chat-inputs button:hover { background: #38c46a; }

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
            .chat-inputs { flex-direction: column; }
            #chat-name { width: 100%; }
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

    <div class="ad-container">
        <div class="ad-slot" onclick="openLightbox()">
            <img src="money.jpg" alt="Реклама" class="ad-image">
            <div class="ad-content">
                <div class="ad-label">// слот 01 — занято</div>
            </div>
        </div>
    </div>

    <!-- Чат -->
    <div id="chat-container">
        <div class="chat-title">// ЧАТ</div>
        <div id="chat-messages"></div>
        <div class="chat-inputs">
            <input type="text" id="chat-name" placeholder="Ваше имя">
            <input type="text" id="chat-input" placeholder="Сообщение...">
            <button onclick="sendMessage()">→</button>
        </div>
    </div>

    <div class="lightbox" id="lightbox" onclick="closeLightbox()">
        <div class="lightbox-close" onclick="closeLightbox()">[ ЗАКРЫТЬ ✕ ]</div>
        <img src="money.jpg" alt="Реклама" onclick="event.stopPropagation()">
    </div>

    <footer>
        <div class="footer-note">© ZOFFRIKK 2026</div>

        <div class="hits-counter">
            <a href="https://hits.sh/zekmnb-wq.github.io/Zoffrikk/">
                <img alt="Хиты" src="https://hits.sh/zekmnb-wq.github.io/Zoffrikk.svg"/>
            </a>
        </div>

        <div class="contact">
            <a href="https://t.me/Ivanee_tg" target="_blank">Связаться → @Ivanee_tg</a>
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

        function openLightbox() {
            document.getElementById('lightbox').classList.add('active');
            document.body.style.overflow = 'hidden';
        }

        function closeLightbox() {
            document.getElementById('lightbox').classList.remove('active');
            document.body.style.overflow = '';
        }

        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') closeLightbox();
        });
    </script>

    <!-- Firebase SDK + чат -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp, getDocs } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

        // ⚠️ ВСТАВЬТЕ СЮДА ВАШ firebaseConfig ИЗ КОНСОЛИ FIREBASE
        const firebaseConfig = {
            apiKey: "ВАШ_API_KEY",
            authDomain: "ВАШ_AUTH_DOMAIN",
            projectId: "ВАШ_PROJECT_ID",
            storageBucket: "ВАШ_STORAGE_BUCKET",
            messagingSenderId: "ВАШ_MESSAGING_SENDER_ID",
            appId: "ВАШ_APP_ID"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const messagesCollection = collection(db, "chat_messages");
        const messagesDiv = document.getElementById('chat-messages');

        // Отправка сообщения
        window.sendMessage = async function() {
            const nameInput = document.getElementById('chat-name');
            const messageInput = document.getElementById('chat-input');
            const name = nameInput.value.trim();
            const text = messageInput.value.trim();

            if (name === '' || text === '') return;

            try {
                await addDoc(messagesCollection, {
                    name: name,
                    text: text,
                    timestamp: serverTimestamp()
                });
                messageInput.value = '';
            } catch (e) {
                console.error("Ошибка при отправке: ", e);
                alert("Не удалось отправить сообщение.");
            }
        };

        // Отправка по Enter
        document.getElementById('chat-input').addEventListener('keydown', function(e) {
            if (e.key === 'Enter') sendMessage();
        });

        // 1. Загружаем все старые сообщения один раз
        async function loadExistingMessages() {
            const q = query(messagesCollection, orderBy("timestamp", "asc"));
            const querySnapshot = await getDocs(q);
            messagesDiv.innerHTML = '';
            querySnapshot.forEach((doc) => {
                const data = doc.data();
                const msgElement = document.createElement('div');
                msgElement.innerHTML = `<span style="color:#888;">[${escapeHtml(data.name)}]</span> <span>${escapeHtml(data.text)}</span>`;
                messagesDiv.appendChild(msgElement);
            });
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        // 2. Слушаем только новые сообщения
        const q = query(messagesCollection, orderBy("timestamp", "asc"));
        let firstLoad = true;

        onSnapshot(q, (snapshot) => {
            if (firstLoad) {
                firstLoad = false;
                return; // пропускаем первую выдачу — её отрисует loadExistingMessages
            }
            snapshot.docChanges().forEach((change) => {
                if (change.type === "added") {
                    const data = change.doc.data();
                    const msgElement = document.createElement('div');
                    msgElement.innerHTML = `<span style="color:#888;">[${escapeHtml(data.name)}]</span> <span>${escapeHtml(data.text)}</span>`;
                    messagesDiv.appendChild(msgElement);
                    messagesDiv.scrollTop = messagesDiv.scrollHeight;
                }
            });
        });

        // Защита от XSS
        function escapeHtml(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        loadExistingMessages();
    </script>

</body>
</html>
