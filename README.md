<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Тайфун — голосовой помощник</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; padding: 20px; background: #eef; }
    #face { font-size: 80px; margin-bottom: 20px; }
    #status { font-size: 18px; color: #333; }
  </style>
</head>
<body>
  <div id="face">🎙️</div>
  <h1>Тайфун — голосовой помощник</h1>
  <p id="status">Нажми разрешить микрофон и говори</p>

  <script>
    const face = document.getElementById('face');
    const status = document.getElementById('status');

    let recognition;
    if ('webkitSpeechRecognition' in window) {
      recognition = new webkitSpeechRecognition();
    } else if ('SpeechRecognition' in window) {
      recognition = new SpeechRecognition();
    } else {
      status.textContent = 'Микрофон не поддерживается в этом браузере.';
    }

    if (recognition) {
      recognition.lang = 'ru-RU';
      recognition.continuous = false;
      recognition.interimResults = false;

      recognition.onstart = () => {
        face.textContent = '👂';
        status.textContent = 'Слушаю...';
      };

      recognition.onresult = (event) => {
        const text = event.results[0][0].transcript.toLowerCase();
        face.textContent = '🤔';
        status.textContent = 'Ты сказал: ' + text;
        handleCommand(text);
      };

      recognition.onerror = () => {
        face.textContent = '😕';
        status.textContent = 'Ошибка распознавания.';
      };

      recognition.onend = () => {
        setTimeout(() => recognition.start(), 1000);
      };

      recognition.start();
    }

    function handleCommand(text) {
      let reply = '';
      if (text.includes('привет')) reply = 'Привет! Я Тайфун.';
      else if (text.includes('как дела')) reply = 'Отлично! А у тебя?';
      else if (text.includes('время')) {
        const now = new Date();
        reply = `Сейчас ${now.getHours()} часов ${now.getMinutes()} минут.`;
      } else if (text.includes('пока')) reply = 'Пока! Увидимся!';
      else reply = 'Извини, я не понимаю эту команду.';

      say(reply);
    }

    function say(text) {
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = 'ru-RU';
      speechSynthesis.speak(utter);
      face.textContent = '🗣️';
      status.textContent = 'Говорю: ' + text;
    }
  </script>
</body>
</html>
