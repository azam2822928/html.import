<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Тайфун — Умный Ассистент</title>
</head>
<body>
  <h1>Привет, я Тайфун!</h1>
  <button onclick="startRecognition()">🎙 Слушать</button>
  <p id="result">Говори...</p>

  <script>
    const resultEl = document.getElementById("result");

    // Проверка поддержки распознавания речи
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

    if (!SpeechRecognition) {
      resultEl.innerText = "Твой браузер не поддерживает распознавание речи.";
    }

    const recognition = new SpeechRecognition();
    recognition.lang = 'ru-RU';
    recognition.interimResults = false;

    recognition.onresult = (event) => {
      const text = event.results[0][0].transcript;
      resultEl.innerText = "Ты сказал: " + text;
      speakAnswer(text);
    };

    recognition.onerror = (e) => {
      resultEl.innerText = "Ошибка: " + e.error;
    };

    function startRecognition() {
      recognition.start();
      resultEl.innerText = "Слушаю...";
    }

    function speakAnswer(text) {
      const synth = window.speechSynthesis;
      const utter = new SpeechSynthesisUtterance();

      utter.text = processCommand(text);
      utter.lang = 'ru-RU';
      synth.speak(utter);
    }

    function processCommand(text) {
      text = text.toLowerCase();
      if (text.includes("привет")) return "Привет, чем могу помочь?";
      if (text.includes("как дела")) return "У меня всё хорошо!";
      if (text.includes("погода")) return "Погода отличная, как у тебя?";
      return "Я тебя понял!";
    }
  </script>
</body>
</html>
