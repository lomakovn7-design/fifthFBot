<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Отправка от @fifthFBot</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            margin: 0;
        }
        .card {
            background: #ffffff;
            padding: 25px;
            border-radius: 16px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 450px;
        }
        h2 {
            margin-top: 0;
            color: #1c1e21;
            font-size: 20px;
            text-align: center;
        }
        label {
            font-weight: bold;
            font-size: 13px;
            color: #606770;
            display: block;
            margin-top: 12px;
            margin-bottom: 5px;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccd0d5;
            border-radius: 8px;
            box-sizing: border-box;
            font-size: 14px;
        }
        textarea {
            height: 120px;
            resize: vertical;
        }
        button {
            width: 100%;
            background-color: #0088cc;
            color: white;
            border: none;
            padding: 12px;
            margin-top: 18px;
            border-radius: 8px;
            font-weight: bold;
            font-size: 15px;
            cursor: pointer;
            transition: background 0.2s;
        }
        button:hover {
            background-color: #006699;
        }
        #status {
            margin-top: 15px;
            text-align: center;
            font-weight: bold;
            font-size: 14px;
        }
    </style>
</head>
<body>

<div class="card">
    <h2>📢 Отправка от имени @fifthFBot</h2>
    
    <label for="token">Токен бота (из BotFather):</label>
    <input type="password" id="token" placeholder="123456789:ABCdef..." value="">

    <label for="chatId">ID классного чата:</label>
    <input type="text" id="chatId" placeholder="-100123456789" value="">

    <label for="message">Текст сообщения:</label>
    <textarea id="message" placeholder="Напиши объявление для класса..."></textarea>

    <button onclick="sendMessage()">Опубликовать в чат</button>

    <div id="status"></div>
</div>

<script>
    // Автоматическое сохранение токена и ID чата в браузере
    document.getElementById('token').value = localStorage.getItem('bot_token') || '';
    document.getElementById('chatId').value = localStorage.getItem('chat_id') || '';

    async function sendMessage() {
        const token = document.getElementById('token').value.trim();
        const chatId = document.getElementById('chatId').value.trim();
        const text = document.getElementById('message').value.trim();
        const statusDiv = document.getElementById('status');

        if (!token || !chatId || !text) {
            statusDiv.style.color = "red";
            statusDiv.innerText = "Заполни все поля!";
            return;
        }

        // Сохраняем токен и ID, чтобы не вводить их каждый раз
        localStorage.setItem('bot_token', token);
        localStorage.setItem('chat_id', chatId);

        statusDiv.style.color = "gray";
        statusDiv.innerText = "Отправка...";

        try {
            const response = await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    chat_id: chatId,
                    text: text
                })
            });

            const data = await response.json();

            if (data.ok) {
                statusDiv.style.color = "green";
                statusDiv.innerText = "✅ Сообщение успешно отправлено!";
                document.getElementById('message').value = '';
            } else {
                statusDiv.style.color = "red";
                statusDiv.innerText = "❌ Ошибка: " + data.description;
            }
        } catch (error) {
            statusDiv.style.color = "red";
            statusDiv.innerText = "❌ Ошибка сети!";
        }
    }
</script>

</body>
</html>
