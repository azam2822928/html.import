<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<title>Тайфун — Голосовой Ассистент</title>
</head>
<body>
<h1>Привет! Я Тайфун 🤖</h1>
<button onclick="startRecognition()">🎤 Нажми и говори</button>
<p id="output">Жду твоих слов...</p>

<script>
const output = document.getElementById('output');
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

if (!SpeechRecognition) {
  output.textContent = 'Извини, твой браузер не поддерживает распознавание речи.';
} else {
  const recognition = new SpeechRecognition();
  recognition.lang = 'ru-RU';
  recognition.interimResults = false;

  recognition.onresult = e => {
    const text = e.results[0][0].transcript.toLowerCase();
    output.textContent = 'Ты сказал: ' + text;
    respond(text);
  };

  recognition.onerror = e => {
    output.textContent = 'Ошибка распознавания: ' + e.error;
  };

  function startRecognition() {
    recognition.start();
    output.textContent = 'Слушаю...';
  }

  function respond(text) {
    let response = 'Я не понял. Попробуй сказать "привет" или "как дела".';
    if (text.includes('привет')) response = 'Привет! Я Тайфун, твой голосовой помощник.';
    else if (text.includes('как дела')) response = 'У меня всё отлично! Как у тебя?';
    else if (text.includes('погода')) response = 'Погода отличная, солнечно и тепло!';
    speak(response);
  }

  function speak(text) {
    const utterance = new SpeechSynthesisUtterance(text);
    utterance.lang = 'ru-RU';
    window.speechSynthesis.speak(utterance);
  }

  window.startRecognition = startRecognition;
}
</script>
</body>
</html>
