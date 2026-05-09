--- html-editor.html (原始)


+++ html-editor.html (修改后)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML Редактор</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1a1a2e;
            color: #eee;
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        header {
            background: #16213e;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid #0f3460;
        }

        h1 {
            font-size: 1.5rem;
            color: #e94560;
        }

        .controls {
            display: flex;
            gap: 10px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }

        .btn-clear {
            background: #e94560;
            color: white;
        }

        .btn-clear:hover {
            background: #ff6b6b;
        }

        .btn-sample {
            background: #0f3460;
            color: white;
        }

        .btn-sample:hover {
            background: #1a4a7a;
        }

        .container {
            display: flex;
            flex: 1;
            overflow: hidden;
        }

        .editor-panel, .preview-panel {
            flex: 1;
            display: flex;
            flex-direction: column;
            padding: 10px;
        }

        .panel-title {
            background: #16213e;
            padding: 10px;
            border-radius: 5px 5px 0 0;
            font-weight: bold;
            color: #e94560;
        }

        #htmlEditor {
            flex: 1;
            background: #0f0f1a;
            color: #00ff88;
            border: 2px solid #0f3460;
            border-top: none;
            border-radius: 0 0 5px 5px;
            padding: 15px;
            font-family: 'Courier New', monospace;
            font-size: 14px;
            resize: none;
            outline: none;
            line-height: 1.5;
        }

        #htmlEditor:focus {
            border-color: #e94560;
        }

        #preview {
            flex: 1;
            background: white;
            border: 2px solid #0f3460;
            border-top: none;
            border-radius: 0 0 5px 5px;
            padding: 0;
        }

        iframe {
            width: 100%;
            height: 100%;
            border: none;
            border-radius: 0 0 5px 5px;
        }

        .status-bar {
            background: #16213e;
            padding: 8px 20px;
            font-size: 12px;
            color: #888;
            border-top: 1px solid #0f3460;
        }
    </style>
</head>
<body>
    <header>
        <h1>📝 HTML Редактор</h1>
        <div class="controls">
            <button class="btn-sample" onclick="loadSample()">Загрузить пример</button>
            <button class="btn-clear" onclick="clearEditor()">Очистить</button>
        </div>
    </header>

    <div class="container">
        <div class="editor-panel">
            <div class="panel-title">Редактор кода</div>
            <textarea id="htmlEditor" placeholder="Введите ваш HTML код здесь..."></textarea>
        </div>
        <div class="preview-panel">
            <div class="panel-title">Предпросмотр</div>
            <iframe id="preview"></iframe>
        </div>
    </div>

    <div class="status-bar">
        Символов: <span id="charCount">0</span> | Строк: <span id="lineCount">0</span>
    </div>

    <script>
        const editor = document.getElementById('htmlEditor');
        const preview = document.getElementById('preview');
        const charCount = document.getElementById('charCount');
        const lineCount = document.getElementById('lineCount');

        // Обновление предпросмотра
        function updatePreview() {
            const html = editor.value;
            const iframe = preview.contentDocument || preview.contentWindow.document;
            iframe.open();
            iframe.write(html);
            iframe.close();

            // Обновление статистики
            charCount.textContent = html.length;
            lineCount.textContent = html.split('\n').length;
        }

        // Обработчик ввода
        editor.addEventListener('input', updatePreview);

        // Поддержка Tab в textarea
        editor.addEventListener('keydown', function(e) {
            if (e.key === 'Tab') {
                e.preventDefault();
                const start = this.selectionStart;
                const end = this.selectionEnd;
                this.value = this.value.substring(0, start) + '    ' + this.value.substring(end);
                this.selectionStart = this.selectionEnd = start + 4;
                updatePreview();
            }
        });

        // Очистить редактор
        function clearEditor() {
            if (confirm('Вы уверены, что хотите очистить редактор?')) {
                editor.value = '';
                updatePreview();
            }
        }

        // Загрузить пример
        function loadSample() {
            const sampleCode = `<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Пример страницы</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 50px auto;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        .card {
            background: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .button {
            display: inline-block;
            background: #667eea;
            color: white;
            padding: 12px 30px;
            border-radius: 25px;
            text-decoration: none;
            margin-top: 20px;
            transition: transform 0.3s;
        }
        .button:hover {
            transform: scale(1.05);
        }
        ul {
            color: #555;
            line-height: 1.8;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>🎉 Добро пожаловать!</h1>
        <p>Это пример HTML страницы, созданной в редакторе.</p>
        <ul>
            <li>✅ Чистый HTML/CSS</li>
            <li>✅ Адаптивный дизайн</li>
            <li>✅ Современные стили</li>
        </ul>
        <center>
            <a href="#" class="button">Нажми меня</a>
        </center>
    </div>
</body>
</html>`;
            editor.value = sampleCode;
            updatePreview();
        }

        // Инициализация с пустым предпросмотром
        updatePreview();
    </script>
</body>
</html>
