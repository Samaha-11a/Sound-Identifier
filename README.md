<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>محول الكلام إلى نص</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f8ff;
            color: #333;
            margin: 0;
            padding: 0;
        }

        header {
            background-color: #4682b4;
            color: white;
            text-align: center;
            padding: 1rem;
        }

        main {
            max-width: 600px;
            margin: 2rem auto;
            text-align: center;
        }

        button {
            background-color: #4682b4;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
        }

        button:hover {
            background-color: #5a9bd4;
        }

        #result {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            background-color: #fff;
            border-radius: 5px;
            font-size: 18px;
        }

        .highlight {
            font-weight: bold;
            color: #ff4500;
        }

        .lang-switch {
            margin-top: 10px;
        }

        .lang-switch button {
            margin: 5px;
        }
    </style>
</head>
<body>
    <header>
        <h1>محول الكلام إلى نص</h1>
    </header>
    <main>
        <p>اضغط على الزر أدناه وابدأ بالتحدث.</p>
        <button id="start-button">ابدأ الاستماع</button>
        <div id="result">ستظهر النتيجة هنا.</div>
        <div class="lang-switch">
            <button id="lang-ar">العربية</button>
            <button id="lang-en">English</button>
        </div>
    </main>
    <script>
        const startButton = document.getElementById('start-button');
        const resultDiv = document.getElementById('result');
        const langArButton = document.getElementById('lang-ar');
        const langEnButton = document.getElementById('lang-en');

        const animalKeywords = {
            en: ['dog', 'cat', 'bird', 'fish', 'rabbit', 'snake'],
            ar: ['كلب', 'قطة', 'طائر', 'سمكة', 'أرنب', 'ثعبان']
        };

        const decorationKeywords = {
            en: ['lamp', 'vase', 'painting', 'curtain', 'rug', 'candle'],
            ar: ['مصباح', 'مزهرية', 'لوحة', 'ستارة', 'سجادة', 'شمعة']
        };

        let currentLang = 'ar';

        const highlightText = (text) => `<span class="highlight">${text}</span>`;

        const processText = (text) => {
            let result = text;
            animalKeywords[currentLang].forEach(keyword => {
                const regex = new RegExp(`\\b${keyword}\\b`, 'gi');
                result = result.replace(regex, highlightText(keyword));
            });

            decorationKeywords[currentLang].forEach(keyword => {
                const regex = new RegExp(`\\b${keyword}\\b`, 'gi');
                result = result.replace(regex, highlightText(keyword));
            });

            return result;
        };

        const displayResult = (text) => {
            const highlightedText = processText(text);
            resultDiv.innerHTML = highlightedText;
        };

        startButton.addEventListener('click', () => {
            const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            recognition.lang = currentLang === 'ar' ? 'ar-EG' : 'en-US';
            recognition.interimResults = false;

            recognition.start();

            recognition.onresult = (event) => {
                const transcript = event.results[0][0].transcript;
                displayResult(transcript);
            };

            recognition.onerror = (event) => {
                resultDiv.textContent = `حدث خطأ: ${event.error}`;
            };

            recognition.onspeechend = () => {
                recognition.stop();
            };
        });

        langArButton.addEventListener('click', () => {
            currentLang = 'ar';
            document.querySelector('header h1').textContent = 'محول الكلام إلى نص';
            document.querySelector('main p').textContent = 'اضغط على الزر أدناه وابدأ بالتحدث.';
            startButton.textContent = 'ابدأ الاستماع';
            resultDiv.textContent = 'ستظهر النتيجة هنا.';
        });

        langEnButton.addEventListener('click', () => {
            currentLang = 'en';
            document.querySelector('header h1').textContent = 'Speech to Text Classifier';
            document.querySelector('main p').textContent = 'Click the button below and start speaking.';
            startButton.textContent = 'Start Listening';
            resultDiv.textContent = 'Your result will appear here.';
        });
    </script>
</body>
</html>
