<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Учебник: 25 языков + Викторина + Чат-бот -- Apple Style</title>
<style>
  :root{
    --bg:#f5f5f7; --surface:#ffffff; --text:#1d1d1f; --muted:#6e6e73;
    --line:#e5e5ea; --accent:#0a84ff; --accent-2:#30d158; --ring:rgba(10,132,255,.35);
    --radius:18px; --shadow:0 10px 30px rgba(0,0,0,.06);
    --code-bg:#0b0b0d; --code-line:#2a2a2e; --code-text:#e9e9ef;
  }
  [data-theme="dark"]{
    --bg:#0b0b0d; --surface:#121216; --text:#e9e9ef; --muted:#9aa0a6;
    --line:rgba(255,255,255,.12); --shadow:0 12px 36px rgba(0,0,0,.45);
    --code-bg:#0b0b0d; --code-line:rgba(255,255,255,.12); --code-text:#e9e9ef;
  }
  *{box-sizing:border-box}
  body{
    margin:0; background:var(--bg); color:var(--text);
    font:16px/1.55 -apple-system,BlinkMacSystemFont,"SF Pro Display","Segoe UI",Roboto,Ubuntu,"Helvetica Neue",Arial,system-ui;
    -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
  }
  .container{max-width:1200px;margin:0 auto;padding:18px}
  /* Nav */
  .nav{display:flex;align-items:center;gap:12px;justify-content:space-between;padding:10px 0}
  .brand{display:flex;align-items:center;gap:10px;font-weight:800;letter-spacing:.2px}
  .logo{width:28px;height:28px;border-radius:9px;background:linear-gradient(135deg,#0a84ff,#30d158);box-shadow:0 6px 16px rgba(10,132,255,.35)}
  .nav-actions{display:flex;gap:8px;flex-wrap:wrap}
  .tabs-top{display:flex;gap:6px;flex-wrap:wrap}
  .chip{padding:8px 12px;border-radius:12px;border:1px solid var(--line);background:var(--surface);cursor:pointer;transition:.18s}
  .chip:hover{transform:translateY(-1px)}
  .chip.active{background:linear-gradient(180deg,rgba(10,132,255,.12),rgba(10,132,255,.06)); border-color:rgba(10,132,255,.35)}
  /* Buttons */
  .btn{appearance:none;border:none;border-radius:14px;padding:10px 14px;font-weight:600;cursor:pointer;transition:.2s ease;backdrop-filter:saturate(180%) blur(12px)}
  .btn-primary{background:linear-gradient(180deg,rgba(10,132,255,.95),rgba(10,132,255,.85));color:white;box-shadow:0 6px 18px var(--ring)}
  .btn-ghost{background:var(--surface);color:var(--text);border:1px solid var(--line)}
  .btn-ghost:hover{transform:translateY(-1px)}
  .btn-disabled{opacity:.5;pointer-events:none}
  .switch{display:inline-flex;align-items:center;gap:8px}
  /* Hero */
  .hero{display:grid;grid-template-columns:1.1fr .9fr;gap:28px;align-items:center;padding:6px 0 14px}
  .hero h1{font-size:clamp(28px,4.5vw,46px);margin:0 0 8px 0;letter-spacing:-.3px}
  .hero p{color:var(--muted);margin:0 0 12px 0}
  .grid3{display:grid;grid-template-columns:repeat(3,1fr);gap:18px}
  .stat{background:var(--surface);border:1px solid var(--line);padding:16px;border-radius:14px;text-align:center;box-shadow:var(--shadow)}
  .stat b{font-size:20px}
  .hero-card{background:var(--surface);border:1px solid var(--line);border-radius:var(--radius);padding:16px;box-shadow:var(--shadow)}
  /* Cards + code */
  .card{background:var(--surface);border:1px solid var(--line);border-radius:var(--radius);padding:16px;box-shadow:var(--shadow)}
  pre{margin:0;background:var(--code-bg);border:1px solid var(--code-line);border-radius:12px;padding:14px;overflow:auto;color:var(--code-text)}
  code{font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace}
  .row{display:flex;gap:12px;flex-wrap:wrap}
  .grow{flex:1 1 auto}
  .muted{color:var(--muted)}
  .sep{height:1px;background:var(--line);margin:12px 0}
  /* Lessons */
  .lang-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}
  .lang{padding:12px;border:1px solid var(--line);border-radius:14px;background:var(--surface);cursor:pointer;transition:.18s;display:flex;justify-content:space-between;align-items:center}
  .lang:hover{transform:translateY(-1px)}
  .lang small{color:var(--muted)}
  .lang .tag{font-size:12px;padding:4px 8px;border:1px solid var(--line);border-radius:999px}
  .search{width:100%;padding:12px;border-radius:12px;border:1px solid var(--line);background:var(--surface);color:var(--text);outline:none}
  .codebar{display:flex;gap:8px;align-items:center;margin-top:8px}
  .pill{padding:6px 10px;border-radius:999px;border:1px solid var(--line);background:var(--surface);cursor:pointer}
  .pill.active{border-color:#0a84ff;background:rgba(10,132,255,.08)}
  .copy{margin-left:auto}
  /* Quiz */
  .option{display:block;margin:10px 0;width:100%;padding:10px 12px;border-radius:12px;border:1px solid var(--line);background:var(--surface);color:var(--text);text-align:left;cursor:pointer}
  .correct{background:rgba(48,209,88,.2);border-color:rgba(48,209,88,.5)}
  .wrong{background:rgba(255,69,58,.15);border-color:rgba(255,69,58,.5)}
  .quiz-nav{display:flex;gap:8px;align-items:center;justify-content:space-between;margin-top:10px}
  .progress{height:10px;background:rgba(127,127,127,.15);border-radius:999px;overflow:hidden}
  .progress > span{display:block;height:100%;background:linear-gradient(90deg,#0a84ff,#30d158)}
  .lvl{font-weight:700;letter-spacing:.2px}
  /* Chat */
  #chat{border:1px solid var(--line);background:var(--surface);height:260px;overflow:auto;border-radius:14px;padding:12px}
  .user{color:#30d158}
  .bot{color:#0a84ff}
  #msg{width:100%;padding:12px;border-radius:12px;border:1px solid var(--line);background:var(--surface);color:var(--text);outline:none}
  #msg:focus{box-shadow:0 0 0 4px var(--ring)}
  /* Footer */
  .footer{color:var(--muted);text-align:center;padding:16px 0}
  @media (max-width:1000px){.hero{grid-template-columns:1fr}.grid3,.lang-grid{grid-template-columns:1fr 1fr}}
  @media (max-width:640px){.lang-grid{grid-template-columns:1fr}}
</style>
</head>
<body>
  <div class="container" id="app">
    <!-- NAV -->
    <div class="nav">
      <div class="brand"><div class="logo"></div> Учебник • Pro</div>
      <div class="nav-actions">
        <div class="tabs-top">
          <div class="chip active" data-view="home">Главная</div>
          <div class="chip" data-view="lessons">Учебник</div>
          <div class="chip" data-view="quiz">Викторина</div>
          <div class="chip" data-view="chat">Чат-бот</div>
          <div class="chip" data-view="settings">Настройки</div>
        </div>
        <div class="switch">
          <label class="muted">Тема</label>
          <button id="themeBtn" class="btn btn-ghost">🌞 Светлая</button>
        </div>
      </div>
    </div>

    <!-- HERO -->
    <section class="hero" data-screen="home">
      <div>
        <h1>Современный учебник в стиле Apple</h1>
        <p>Чистый минимализм, мягкие тени, стеклянные панели и плавные анимации. 25+ языков, 45 уровней викторины и умный чат-бот.</p>
        <div class="grid3">
          <div class="stat"><b>📚 25+ языков</b><div class="muted">Примеры с пояснениями</div></div>
          <div class="stat"><b>🧪 45 уровней</b><div class="muted">От базовых до продвинутых</div></div>
          <div class="stat"><b>🤖 Чат-бот</b><div class="muted">Подскажет синтаксис и команды</div></div>
        </div>
      </div>
      <div class="hero-card">
        <div class="row">
          <button class="btn btn-primary" data-go="lessons">Начать с учебника</button>
          <button class="btn btn-ghost" data-go="quiz">Пройти викторину</button>
          <button class="btn btn-ghost" data-go="chat">Спросить у бота</button>
        </div>
        <div class="sep"></div>
        <div class="muted">Подсказка: в чате попробуй «python print», «js fetch», «sql join», «go hello»…</div>
      </div>
    </section>

    <!-- LESSONS -->
    <section class="card" data-screen="lessons" style="display:none">
      <div class="row" style="align-items:center">
        <h2 class="grow" style="margin:0">📚 Учебник</h2>
        <input id="search" class="search" placeholder="Поиск по языкам: python, rust, sql..."/>
      </div>
      <div class="sep"></div>
      <div id="langGrid" class="lang-grid"></div>
      <div class="sep"></div>
      <div id="lessonPane" class="card" style="display:none"></div>
    </section>

    <!-- QUIZ -->
    <section class="card" data-screen="quiz" style="display:none">
      <h2 style="margin:0 0 8px 0">🧪 Викторина</h2>
      <div class="lvl" id="lvlLabel">Уровень 1 из 45</div>
      <div class="progress" style="margin:8px 0 12px 0"><span id="bar" style="width:2%"></span></div>
      <div id="quizBody"></div>
      <div id="why" class="muted" style="margin-top:8px"></div>
      <div class="quiz-nav">
        <div style="display:flex;gap:8px">
          <button id="prevBtn" class="btn btn-ghost">Назад</button>
          <button id="nextBtn" class="btn btn-primary btn-disabled">Далее</button>
        </div>
        <button id="resetBtn" class="btn btn-ghost">Начать заново</button>
      </div>
    </section>

    <!-- CHAT -->
    <section class="card" data-screen="chat" style="display:none">
      <h2 style="margin:0 0 8px 0">🤖 Чат-бот помощник</h2>
      <div id="chat"></div>
      <div style="height:10px"></div>
      <input type="text" id="msg" placeholder="Спроси: python print, js loop, sql join, go hello ..." onkeydown="if(event.key==='Enter') sendMessage()"/>
      <div style="height:10px"></div>
      <div class="row">
        <button class="btn btn-primary" onclick="sendMessage()">Отправить</button>
        <button class="btn btn-ghost" onclick="clearChat()">Очистить</button>
      </div>
    </section>

    <!-- SETTINGS -->
    <section class="card" data-screen="settings" style="display:none">
      <h2 style="margin:0 0 8px 0">⚙️ Настройки</h2>
      <div class="row">
        <button class="btn btn-ghost" onclick="toggleSound()">🔊 Звук клика: <span id="soundState">вкл</span></button>
        <button class="btn btn-ghost" onclick="copyLink()">🔗 Скопировать ссылку</button>
      </div>
      <div class="sep"></div>
      <div class="muted">Тема, звук и прогресс сохраняются локально (localStorage).</div>
    </section>

    <footer class="footer">Сделано в современном стиле ✨</footer>
  </div>

  <!-- AUDIO -->
  <audio id="звук"><source src="click.mp3" type="audio/mpeg"></audio>

<script>
/* ========== UTIL ========== */
const qs = s=>document.querySelector(s);
const qsa = s=>Array.from(document.querySelectorAll(s));
const store = {
  get(k,def){ try{ const v=localStorage.getItem(k); return v==null?def:JSON.parse(v);}catch{ return def }},
  set(k,v){ try{ localStorage.setItem(k,JSON.stringify(v)); }catch{} }
};
function escapeHtml(s){ return s.replace(/[&<>"']/g, m=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;' }[m])) }
function playClick(){ if(store.get('sound',true)){ const a=qs('#звук'); if(a){ try{ a.currentTime=0; a.play(); }catch(e){} } } }

/* ========== ROUTER / THEME ========== */
const screens = ['home','lessons','quiz','chat','settings'];
qsa('.chip').forEach(el=>{
  el.addEventListener('click',()=>{
    const view = el.dataset.view;
    if(!view) return;
    activateTab(view);
    playClick();
  });
});
qsa('[data-go]').forEach(b=>b.addEventListener('click',()=>{ activateTab(b.dataset.go); playClick(); }));

function activateTab(view){
  qsa('.chip').forEach(c=>c.classList.toggle('active', c.dataset.view===view));
  screens.forEach(s=>qs(`[data-screen="${s}"]`).style.display = (s===view?'':'none'));
  if(view==='lessons' && !lessonsInited) initLessons();
  if(view==='quiz' && !quizInited) initQuiz();
  if(view==='chat' && !chatInited) initChat();
  store.set('lastView',view);
}
document.addEventListener('DOMContentLoaded',()=>{
  // theme
  const savedTheme = store.get('theme','light');
  document.documentElement.setAttribute('data-theme', savedTheme==='dark'?'dark':'light');
  updateThemeBtn();
  // sound
  const s = store.get('sound',true); qs('#soundState').textContent = s?'вкл':'выкл';
  // route
  activateTab(store.get('lastView','home'));
});

qs('#themeBtn').addEventListener('click',()=>{
  const cur = document.documentElement.getAttribute('data-theme')==='dark'?'dark':'light';
  const next = cur==='dark'?'light':'dark';
  document.documentElement.setAttribute('data-theme',next);
  store.set('theme',next);
  updateThemeBtn(); playClick();
});
function updateThemeBtn(){
  const cur = document.documentElement.getAttribute('data-theme')==='dark'?'dark':'light';
  qs('#themeBtn').textContent = cur==='dark' ? '🌙 Тёмная' : '🌞 Светлая';
}

/* ========== SETTINGS HELPERS ========== */
function toggleSound(){
  const now = !store.get('sound',true);
  store.set('sound',now);
  qs('#soundState').textContent = now?'вкл':'выкл';
  playClick();
}
function copyLink(){
  navigator.clipboard.writeText(location.href).then(()=>alert('Ссылка скопирована!'));
}

/* ========== LESSONS (25+ языков) ========== */
const LANGS = [
  {id:'python',name:'Python',tag:'backend', hello:"print('Hello, world!')", loop:"for i in range(3): print(i)", ifs:"x=5\nif x>0:\n    print('plus')"},
  {id:'javascript',name:'JavaScript',tag:'web', hello:"console.log('Hello, world!')", loop:"for(let i=0;i<3;i++){ console.log(i) }", ifs:"const x=5; if(x>0){ console.log('plus') }"},
  {id:'typescript',name:'TypeScript',tag:'web', hello:"const msg: string = 'Hello';\nconsole.log(msg)", loop:"for (let i=0;i<3;i++){ console.log(i) }", ifs:"let x:number=5; if(x>0){ console.log('plus') }"},
  {id:'java',name:'Java',tag:'backend', hello:"class Main{ public static void main(String[] a){ System.out.println(\"Hello\"); }}", loop:"for(int i=0;i<3;i++){ System.out.println(i); }", ifs:"int x=5; if(x>0){ System.out.println(\"plus\"); }"},
  {id:'c',name:'C',tag:'system', hello:"#include <stdio.h>\nint main(){ printf(\"Hello\\n\"); }", loop:"for(int i=0;i<3;i++){ printf(\"%d\\n\",i); }", ifs:"int x=5; if(x>0){ printf(\"plus\\n\"); }"},
  {id:'cpp',name:'C++',tag:'system', hello:"#include <iostream>\nint main(){ std::cout<<\"Hello\"; }", loop:"for(int i=0;i<3;i++){ std::cout<<i; }", ifs:"int x=5; if(x>0){ std::cout<<\"plus\"; }"},
  {id:'csharp',name:'C#',tag:'backend', hello:"Console.WriteLine(\"Hello\");", loop:"for(int i=0;i<3;i++){ Console.WriteLine(i); }", ifs:"var x=5; if(x>0){ Console.WriteLine(\"plus\"); }"},
  {id:'go',name:'Go',tag:'backend', hello:"package main\nimport \"fmt\"\nfunc main(){ fmt.Println(\"Hello\") }", loop:"for i:=0;i<3;i++{ fmt.Println(i) }", ifs:"x:=5; if x>0 { fmt.Println(\"plus\") }"},
  {id:'rust',name:'Rust',tag:'system', hello:"fn main(){ println!(\"Hello\"); }", loop:"for i in 0..3 { println!(\"{}\", i); }", ifs:"let x=5; if x>0 { println!(\"plus\"); }"},
  {id:'kotlin',name:'Kotlin',tag:'mobile', hello:"fun main(){ println(\"Hello\") }", loop:"for(i in 0..2) println(i)", ifs:"val x=5; if(x>0) println(\"plus\")"},
  {id:'swift',name:'Swift',tag:'mobile', hello:"print(\"Hello\")", loop:"for i in 0..<3 { print(i) }", ifs:"let x=5; if x>0 { print(\"plus\") }"},
  {id:'php',name:'PHP',tag:'web', hello:"<?php echo 'Hello'; ?>", loop:"<?php for($i=0;$i<3;$i++){ echo $i; } ?>", ifs:"<?php $x=5; if($x>0){ echo 'plus'; } ?>"},
  {id:'ruby',name:'Ruby',tag:'backend', hello:"puts 'Hello'", loop:"3.times{|i| puts i }", ifs:"x=5; puts 'plus' if x>0"},
  {id:'dart',name:'Dart',tag:'mobile', hello:"void main(){ print('Hello'); }", loop:"for(var i=0;i<3;i++){ print(i); }", ifs:"var x=5; if(x>0){ print('plus'); }"},
  {id:'r',name:'R',tag:'data', hello:"print('Hello')", loop:"for(i in 0:2) print(i)", ifs:"x<-5; if(x>0) print('plus')"},
  {id:'matlab',name:'MATLAB',tag:'data', hello:"disp('Hello')", loop:"for i=0:2, disp(i); end", ifs:"x=5; if x>0, disp('plus'); end"},
  {id:'sql',name:'SQL',tag:'data', hello:"SELECT 'Hello' AS msg;", loop:"-- нет классического цикла", ifs:"SELECT CASE WHEN 5>0 THEN 'plus' END;"},
  {id:'bash',name:'Bash',tag:'devops', hello:"echo 'Hello'", loop:"for i in 0 1 2; do echo $i; done", ifs:"x=5; if [ $x -gt 0 ]; then echo plus; fi"},
  {id:'powershell',name:'PowerShell',tag:'devops', hello:"Write-Host 'Hello'", loop:"for($i=0;$i -lt 3;$i++){ Write-Host $i }", ifs:"$x=5; if($x -gt 0){ Write-Host 'plus' }"},
  {id:'html',name:'HTML',tag:'web', hello:"<h1>Hello</h1>", loop:"<!-- нет -->", ifs:"<!-- нет -->"},
  {id:'css',name:'CSS',tag:'web', hello:"h1 { color: royalblue; }", loop:"/* нет */", ifs:"/* нет */"},
  {id:'lua',name:'Lua',tag:'game', hello:"print('Hello')", loop:"for i=0,2 do print(i) end", ifs:"x=5; if x>0 then print('plus') end"},
  {id:'haskell',name:'Haskell',tag:'fp', hello:"main = putStrLn \"Hello\"", loop:"mapM_ print [0..2]", ifs:"let x=5 in if x>0 then putStrLn \"plus\" else return ()"},
  {id:'scala',name:'Scala',tag:'fp', hello:"@main def m()=println(\"Hello\")", loop:"for(i<-0 to 2) println(i)", ifs:"val x=5; if(x>0) println(\"plus\")"},
  {id:'julia',name:'Julia',tag:'data', hello:"println(\"Hello\")", loop:"for i in 0:2; println(i); end", ifs:"x=5; if x>0; println(\"plus\"); end"}
];

let lessonsInited=false;
function initLessons(){
  lessonsInited=true;
  renderLangGrid(LANGS);
  qs('#search').addEventListener('input', e=>{
    const t = e.target.value.toLowerCase().trim();
    const filtered = LANGS.filter(l => l.name.toLowerCase().includes(t) || l.id.includes(t) || l.tag.includes(t));
    renderLangGrid(filtered);
  });
}
function renderLangGrid(list){
  const grid = qs('#langGrid');
  grid.innerHTML = list.map(l=>`
    <div class="lang" data-id="${l.id}">
      <div>
        <div><b>${l.name}</b></div>
        <small class="muted">${l.tag}</small>
      </div>
      <span class="tag">Открыть</span>
    </div>
  `).join('');
  qsa('.lang').forEach(el=>{
    el.onclick = ()=>openLesson(el.dataset.id);
  });
}
function openLesson(id){
  const l = LANGS.find(x=>x.id===id);
  if(!l) return;
  const pane = qs('#lessonPane'); pane.style.display='block';
  const pills = [
    {k:'hello',t:'Hello World'},
    {k:'loop',t:'Цикл'},
    {k:'ifs',t:'Условие'}
  ];
  pane.innerHTML = `
    <div class="row" style="align-items:center">
      <h3 class="grow" style="margin:0">${l.name} -- базовые примеры</h3>
      <button class="btn btn-ghost" onclick="paneClose()">Закрыть</button>
    </div>
    <div class="muted">Нажимай на таблетки снизу, чтобы переключать пример. Нажми «Копировать», чтобы забрать код.</div>
    <div class="codebar" id="codebar">
      ${pills.map(p=>`<span class="pill" data-k="${p.k}">${p.t}</span>`).join('')}
      <span class="pill copy" onclick="copyCode()">📋 Копировать</span>
    </div>
    <div style="height:8px"></div>
    <pre id="codeblock"><code>${escapeHtml(l.hello)}</code></pre>
    <div class="sep"></div>
    <div class="muted">Пояснение: <br>
    <ul>
      <li><b>Hello World</b> -- минимальная программа для проверки окружения.</li>
      <li><b>Цикл</b> -- повторяет действие несколько раз (покажет 0,1,2).</li>
      <li><b>Условие</b> -- ветка if выполняется, если выражение истинно.</li>
    </ul></div>
  `;
  // pills events
  qsa('#codebar .pill').forEach(el=>{
    if(el.classList.contains('copy')) return;
    el.onclick = ()=>{
      qsa('#codebar .pill').forEach(p=>p.classList.toggle('active', p===el));
      const k = el.dataset.k;
      const code = (l[k]||l.hello);
      qs('#codeblock').innerHTML = `<code>${escapeHtml(code)}</code>`;
      playClick();
    };
  });
  // set first pill active
  qsa('#codebar .pill')[0].classList.add('active');
}
function paneClose(){ qs('#lessonPane').style.display='none'; }
function copyCode(){
  const code = qs('#codeblock').innerText;
  navigator.clipboard.writeText(code).then(()=>{ alert('Код скопирован'); });
}

/* ========== QUIZ (45 уровней) ========== */
let quizInited=false;
let QUIZ = { levels: [], current: 0 };

const baseLevels = [
  { q:"Что выведет print(type(42))?", options:["<class 'str'>","<class 'int'>","<class 'float'>","<class 'bool'>"], correct:1, why:"42 -- целое число (int)."},
  { q:"Как объявить список в Python?", options:["(1,2,3)","{1,2,3}","[1,2,3]","<1,2,3>"], correct:2, why:"Список -- квадратные скобки: [ ... ]."},
  { q:"Что выведет?\nfor i in range(3): print(i)", options:["1 2 3","0 1 
