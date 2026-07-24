<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tap Bola</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #6dd5ed, #2193b0);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .card {
      width: min(100%, 430px);
      background: #fff;
      border-radius: 20px;
      padding: 18px;
      box-shadow: 0 12px 30px rgba(0,0,0,0.18);
      text-align: center;
    }

    h1 {
      margin: 0 0 8px;
      font-size: 28px;
    }

    p {
      color: #555;
    }

    .topbar {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      margin: 14px 0;
    }

    .box {
      flex: 1;
      background: #f4f7fb;
      padding: 12px;
      border-radius: 14px;
      font-weight: bold;
    }

    .label {
      display: block;
      font-size: 12px;
      color: #666;
      margin-bottom: 4px;
    }

    button {
      width: 100%;
      padding: 14px;
      border: none;
      border-radius: 14px;
      background: #2193b0;
      color: white;
      font-size: 16px;
      font-weight: bold;
      margin-bottom: 14px;
    }

    button:disabled {
      background: #8bbccc;
    }

    #gameArea {
      position: relative;
      width: 100%;
      height: 380px;
      background: linear-gradient(#eaf7ff, #dff3ff);
      border: 3px solid #c8e9f7;
      border-radius: 18px;
      overflow: hidden;
      touch-action: manipulation;
    }

    #target {
      position: absolute;
      width: 72px;
      height: 72px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 30%, #ffffff, #ff5e62 60%, #d63031);
      box-shadow: 0 10px 20px rgba(214,48,49,0.35);
      display: none;
    }

    #message {
      min-height: 24px;
      margin-top: 12px;
      font-weight: bold;
      color: #333;
    }

    .small {
      font-size: 13px;
      color: #777;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>Tap Bola</h1>
    <p>Sentuh bola sebanyak mungkin dalam 30 detik.</p>

    <div class="topbar">
      <div class="box">
        <span class="label">Skor</span>
        <span id="score">0</span>
      </div>
      <div class="box">
        <span class="label">Waktu</span>
        <span id="time">30</span>
      </div>
      <div class="box">
        <span class="label">Best</span>
        <span id="best">0</span>
      </div>
    </div>

    <button id="startBtn">Mulai Game</button>

    <div id="gameArea">
      <div id="target"></div>
    </div>

    <div id="message">Tekan "Mulai Game" untuk bermain.</div>
    <div class="small">Game ini dibuat dengan HTML, CSS, dan JavaScript di GitHub Pages.</div>
  </div>

  <script>
    const scoreEl = document.getElementById('score');
    const timeEl = document.getElementById('time');
    const bestEl = document.getElementById('best');
    const startBtn = document.getElementById('startBtn');
    const gameArea = document.getElementById('gameArea');
    const target = document.getElementById('target');
    const messageEl = document.getElementById('message');

    let score = 0;
    let timeLeft = 30;
    let timer = null;
    let playing = false;

    let best = Number(localStorage.getItem('tapBolaBest') || 0);
    bestEl.textContent = best;

    function moveTarget() {
      const size = target.offsetWidth || 72;
      const maxX = gameArea.clientWidth - size;
      const maxY = gameArea.clientHeight - size;

      const x = Math.random() * maxX;
      const y = Math.random() * maxY;

      target.style.left = x + 'px';
      target.style.top = y + 'px';
    }

    function startGame() {
      if (playing) return;

      score = 0;
      timeLeft = 30;
      playing = true;

      scoreEl.textContent = score;
      timeEl.textContent = timeLeft;
      messageEl.textContent = 'Ayo! Kejar bolanya!';
      startBtn.textContent = 'Sedang Bermain...';
      startBtn.disabled = true;

      target.style.display = 'block';
      moveTarget();

      timer = setInterval(() => {
        timeLeft--;
        timeEl.textContent = timeLeft;

        if (timeLeft <= 0) {
          endGame();
        }
      }, 1000);
    }

    function endGame() {
      clearInterval(timer);
      playing = false;
      target.style.display = 'none';

      startBtn.disabled = false;
      startBtn.textContent = 'Main Lagi';

      if (score > best) {
        best = score;
        localStorage.setItem('tapBolaBest', best);
        bestEl.textContent = best;
        messageEl.textContent = 'Waktu habis! Skor: ' + score + '. Rekor baru!';
      } else {
        messageEl.textContent = 'Waktu habis! Skor akhir: ' + score;
      }
    }

    target.addEventListener('pointerdown', () => {
      if (!playing) return;

      score++;
      scoreEl.textContent = score;
      moveTarget();
    });

    startBtn.addEventListener('click', startGame);

    window.addEventListener('resize', () => {
      if (playing) moveTarget();
    });
  </script>
</body>
</html>
