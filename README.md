<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<title>Desert Outlaw</title>
<style>
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

  html, body {
    margin: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #08090d;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    touch-action: none;
  }

  body {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #phone {
    position: relative;
    width: min(100vw, 520px);
    height: 100dvh;
    max-height: 950px;
    overflow: hidden;
    background: #17191f;
    box-shadow: 0 0 50px #000;
  }

  #game {
    display: block;
    width: 100%;
    height: 100%;
    image-rendering: pixelated;
    background: #c9924c;
  }

  #hud {
    position: absolute;
    top: env(safe-area-inset-top, 10px);
    left: 10px;
    right: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    pointer-events: none;
    z-index: 5;
  }

  .hudBox {
    color: white;
    background: rgba(8, 10, 15, .82);
    border: 1px solid rgba(255,255,255,.12);
    border-radius: 12px;
    padding: 8px 11px;
    font-size: 14px;
    font-weight: 800;
    text-shadow: 1px 1px #000;
  }

  #pauseBtn {
    pointer-events: auto;
    width: 46px;
    height: 42px;
    border: 0;
    border-radius: 12px;
    background: rgba(8,10,15,.9);
    color: white;
    font-size: 21px;
  }

  #controls {
    position: absolute;
    left: 0;
    right: 0;
    bottom: max(12px, env(safe-area-inset-bottom));
    height: 175px;
    z-index: 10;
    pointer-events: none;
  }

  .control {
    position: absolute;
    border: 2px solid rgba(255,255,255,.16);
    background: rgba(20,22,29,.88);
    color: white;
    border-radius: 18px;
    font-size: 25px;
    font-weight: 900;
    box-shadow: 0 6px 0 rgba(0,0,0,.3);
    pointer-events: auto;
    user-select: none;
    touch-action: none;
  }

  .control:active, .control.active {
    transform: translateY(4px);
    box-shadow: 0 2px 0 rgba(0,0,0,.3);
    background: #343947;
  }

  #up { left: 86px; bottom: 106px; width: 62px; height: 58px; }
  #down { left: 86px; bottom: 8px; width: 62px; height: 58px; }
  #left { left: 16px; bottom: 57px; width: 62px; height: 58px; }
  #right { left: 156px; bottom: 57px; width: 62px; height: 58px; }

  #shoot {
    right: 20px;
    bottom: 35px;
    width: 118px;
    height: 105px;
    border-radius: 50%;
    background: #8b2727;
    border-color: #d85b4e;
    font-size: 18px;
  }

  #shoot:active, #shoot.active { background: #c23a30; }

  .overlay {
    position: absolute;
    inset: 0;
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 30;
    background: rgba(4,5,8,.88);
    padding: 24px;
  }

  .panel {
    width: min(92%, 390px);
    background: #151820;
    border: 1px solid #3b404d;
    border-radius: 20px;
    padding: 22px;
    color: white;
    text-align: center;
    box-shadow: 0 20px 60px #000;
  }

  .panel h1 {
    margin: 0 0 8px;
    font-size: 27px;
  }

  .panel p {
    color: #aeb4c2;
    margin: 8px 0 18px;
  }

  .shopItem {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    background: #20232c;
    border-radius: 14px;
    padding: 14px;
    margin: 10px 0;
    text-align: left;
  }

  .shopItem strong { display: block; }
  .shopItem small { color: #aeb4c2; }

  button {
    font-family: inherit;
    cursor: pointer;
  }

  .buy, .primary, .secondary {
    border: 0;
    border-radius: 12px;
    padding: 11px 14px;
    color: white;
    font-weight: 900;
    white-space: nowrap;
  }

  .buy, .primary { background: #b66a2b; }
  .secondary { background: #343945; margin-top: 8px; width: 100%; }

  .buy:disabled {
    opacity: .4;
  }

  #adRevenue {
    margin: 18px 0;
    color: #62df8b;
    font-weight: 900;
    font-size: 17px;
  }

  #startScreen {
    position: absolute;
    inset: 0;
    z-index: 50;
    background:
      radial-gradient(circle at 50% 30%, #55402d, transparent 35%),
      linear-gradient(#11131a, #090a0d);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 25px;
  }

  #startScreen h1 {
    font-size: 44px;
    margin: 0 0 5px;
    letter-spacing: 2px;
  }

  #startScreen p {
    color: #aaa;
    margin: 0 0 25px;
  }

  #runBtn {
    border: 0;
    border-radius: 15px;
    padding: 16px 32px;
    font-size: 18px;
    font-weight: 900;
    color: white;
    background: #b66a2b;
    box-shadow: 0 7px 0 #713f1c;
  }

  #runBtn:active {
    transform: translateY(5px);
    box-shadow: 0 2px 0 #713f1c;
  }

  .hidden { display: none !important; }
</style>
</head>

<body>
<div id="phone">

  <canvas id="game"></canvas>

  <div id="hud">
    <div class="hudBox">❤️ <span id="health">100</span></div>
    <div class="hudBox">💰 $<span id="cash">0</span></div>
    <button id="pauseBtn">☰</button>
  </div>

  <div id="controls">
    <button class="control" id="up">▲</button>
    <button class="control" id="down">▼</button>
    <button class="control" id="left">◀</button>
    <button class="control" id="right">▶</button>
    <button class="control" id="shoot">🔫<br>SHOOT</button>
  </div>

  <div class="overlay" id="shop">
    <div class="panel">
      <h1>🤠 GUN SHOP</h1>
      <p>Cash: $<span id="shopCash">0</span></p>

      <div class="shopItem">
        <div>
          <strong>👢 Speed Boots</strong>
          <small>Move faster</small>
        </div>
        <button class="buy" id="buySpeed">$50</button>
      </div>

      <div class="shopItem">
        <div>
          <strong>🔫 Golden Revolver</strong>
          <small>Golden bullets • 2× damage</small>
        </div>
        <button class="buy" id="buyGold">$100</button>
      </div>

      <button class="secondary" id="closeShop">Back to Game</button>
    </div>
  </div>

  <div class="overlay" id="adScreen">
    <div class="panel">
      <h1>📺 Advertisement</h1>
      <p>Thanks for supporting this game!</p>
      <div id="adTimer">Loading Advertisement... 3</div>
      <div id="adRevenue">Ad Revenue Earned: +$0.05</div>
      <button class="primary hidden" id="respawn">Respawn</button>
    </div>
  </div>

  <div id="startScreen">
    <div>
      <h1>🤠 DESERT<br>OUTLAW</h1>
      <p>Survive the bandits. Collect cash. Become the fastest gunslinger.</p>
      <button id="runBtn">▶ RUN GAME</button>
    </div>
  </div>
</div>

<script>
(() => {
  "use strict";

  const canvas = document.getElementById("game");
  const ctx = canvas.getContext("2d");

  const W = 900, H = 1500;
  canvas.width = W;
  canvas.height = H;

  const player = {
    x: W / 2,
    y: H / 2,
    r: 34,
    speed: 5,
    health: 100,
    damage: 1,
    gold: false,
    boots: false
  };

  let cash = 0;
  let bullets = [];
  let bandits = [];
  let particles = [];
  let running = false;
  let paused = false;
  let spawnTimer = 0;
  let lastTime = 0;
  let shootCooldown = 0;

  const keys = {
    up: false,
    down: false,
    left: false,
    right: false
  };

  const $ = id => document.getElementById(id);

  function resizeCanvas() {
    // CSS handles display scaling; keeping a fixed internal resolution
    // provides consistent gameplay on different phone sizes.
  }

  function updateHUD() {
    $("health").textContent = Math.max(0, Math.ceil(player.health));
    $("cash").textContent = cash;
    $("shopCash").textContent = cash;

    $("buySpeed").disabled = player.boots || cash < 50;
    $("buyGold").disabled = player.gold || cash < 100;
  }

  function resetPlayer() {
    player.x = W / 2;
    player.y = H / 2;
    player.health = 100;
    bullets = [];
    bandits = [];
    particles = [];
    shootCooldown = 0;
    updateHUD();
  }

  function randomEdgePosition() {
    const side = Math.floor(Math.random() * 4);

    if (side === 0) return { x: Math.random() * W, y: -50 };
    if (side === 1) return { x: W + 50, y: Math.random() * H };
    if (side === 2) return { x: Math.random() * W, y: H + 50 };
    return { x: -50, y: Math.random() * H };
  }

  function spawnBandit() {
    const p = randomEdgePosition();

    bandits.push({
      x: p.x,
      y: p.y,
      r: 31,
      speed: 1.0 + Math.random() * 0.7,
      hp: 1,
      wobble: Math.random() * Math.PI * 2
    });
  }

  function shoot() {
    if (!running || paused || shootCooldown > 0 || player.health <= 0) return;

    shootCooldown = player.gold ? 13 : 18;

    let nearest = null;
    let nearestDist = Infinity;

    for (const b of bandits) {
      const dx = b.x - player.x;
      const dy = b.y - player.y;
      const d = Math.hypot(dx, dy);

      if (d < nearestDist) {
        nearest = b;
        nearestDist = d;
      }
    }

    let angle = Math.random() * Math.PI * 2;

    if (nearest) {
      angle = Math.atan2(nearest.y - player.y, nearest.x - player.x);
    }

    bullets.push({
      x: player.x,
      y: player.y,
      vx: Math.cos(angle) * 15,
      vy: Math.sin(angle) * 15,
      r: 8,
      damage: player.damage,
      life: 70
    });
  }

  function createParticles(x, y, amount = 8) {
    for (let i = 0; i < amount; i++) {
      const a = Math.random() * Math.PI * 2;
      const s = 1 + Math.random() * 4;

      particles.push({
        x, y,
        vx: Math.cos(a) * s,
        vy: Math.sin(a) * s,
        life: 25 + Math.random() * 20,
        size: 3 + Math.random() * 5
      });
    }
  }

  function update(dt) {
    if (!running || paused) return;

    const speed = player.boots ? 7.5 : 5;

    let dx = 0;
    let dy = 0;

    if (keys.left) dx--;
    if (keys.right) dx++;
    if (keys.up) dy--;
    if (keys.down) dy++;

    if (dx || dy) {
      const len = Math.hypot(dx, dy);
      player.x += dx / len * speed * dt;
      player.y += dy / len * speed * dt;
    }

    player.x = Math.max(45, Math.min(W - 45, player.x));
    player.y = Math.max(70, Math.min(H - 70, player.y));

    shootCooldown = Math.max(0, shootCooldown - dt);

    spawnTimer -= dt;

    if (spawnTimer <= 0) {
      spawnBandit();
      spawnTimer = Math.max(35, 85 - bandits.length * 1.5);
    }

    for (const b of bandits) {
      const dxp = player.x - b.x;
      const dyp = player.y - b.y;
      const d = Math.hypot(dxp, dyp) || 1;

      b.x += dxp / d * b.speed * dt;
      b.y += dyp / d * b.speed * dt;
      b.wobble += .08 * dt;

      if (d < player.r + b.r) {
        player.health -= 0.8 * dt;

        if (Math.random() < .12) {
          createParticles(player.x, player.y, 2);
        }
      }
    }

    for (let i = bullets.length - 1; i >= 0; i--) {
      const bullet = bullets[i];

      bullet.x += bullet.vx * dt;
      bullet.y += bullet.vy * dt;
      bullet.life -= dt;

      let hit = false;

      for (let j = bandits.length - 1; j >= 0; j--) {
        const b = bandits[j];
        const d = Math.hypot(b.x - bullet.x, b.y - bullet.y);

        if (d < b.r + bullet.r) {
          b.hp -= bullet.damage;
          hit = true;

          if (b.hp <= 0) {
            cash += 10;
            createParticles(b.x, b.y, 12);
            bandits.splice(j, 1);
            updateHUD();
          }

          break;
        }
      }

      if (
        hit ||
        bullet.life <= 0 ||
        bullet.x < -50 ||
        bullet.x > W + 50 ||
        bullet.y < -50 ||
        bullet.y > H + 50
      ) {
        bullets.splice(i, 1);
      }
    }

    for (let i = particles.length - 1; i >= 0; i--) {
      const p = particles[i];

      p.x += p.vx * dt;
      p.y += p.vy * dt;
      p.vx *= .96;
      p.vy *= .96;
      p.life -= dt;

      if (p.life <= 0) particles.splice(i, 1);
    }

    if (player.health <= 0) {
      player.health = 0;
      updateHUD();
      triggerAd();
    }
  }

  function drawDesert() {
    ctx.fillStyle = "#c9924c";
    ctx.fillRect(0, 0, W, H);

    // Sand bands
    for (let y = 0; y < H; y += 80) {
      ctx.fillStyle = y % 160 === 0 ? "rgba(255,220,150,.08)" : "rgba(80,45,20,.035)";
      ctx.fillRect(0, y, W, 40);
    }

    // Pixel-like rocks
    for (let i = 0; i < 55; i++) {
      const x = (i * 173) % W;
      const y = (i * 311) % H;

      ctx.fillStyle = i % 2 ? "#9c6638" : "#ad733c";
      ctx.fillRect(x, y, 9 + (i % 5) * 4, 5 + (i % 3) * 3);
    }

    // Cacti
    for (let i = 0; i < 9; i++) {
      const x = 70 + ((i * 277) % (W - 140));
      const y = 120 + ((i * 191) % (H - 280));

      if (Math.hypot(x - player.x, y - player.y) < 150) continue;

      ctx.fillStyle = "#42633c";
      ctx.fillRect(x, y, 18, 70);
      ctx.fillRect(x - 18, y + 25, 18, 12);
      ctx.fillRect(x - 18, y + 10, 10, 27);
      ctx.fillRect(x + 18, y + 40, 18, 12);
      ctx.fillRect(x + 26, y + 28, 10, 25);
    }
  }

  function drawCharacter() {
    ctx.save();
    ctx.translate(player.x, player.y);

    // Shadow
    ctx.fillStyle = "rgba(40,20,10,.3)";
    ctx.beginPath();
    ctx.ellipse(0, 35, 38, 12, 0, 0, Math.PI * 2);
    ctx.fill();

    // Body
    ctx.fillStyle = "#244a6b";
    ctx.fillRect(-20, -5, 40, 45);

    // Head
    ctx.fillStyle = "#d99b64";
    ctx.beginPath();
    ctx.arc(0, -30, 20, 0, Math.PI * 2);
    ctx.fill();

    // Hat
    ctx.fillStyle = "#55351f";
    ctx.fillRect(-28, -52, 56, 9);
    ctx.fillRect(-16, -68, 32, 18);
    ctx.fillStyle = "#c78b3f";
    ctx.fillRect(-16, -54, 32, 5);

    // Face
    ctx.fillStyle = "#111";
    ctx.fillRect(-10, -34, 6, 4);
    ctx.fillRect(5, -34, 6, 4);

    // Gun
    ctx.fillStyle = "#272727";
    ctx.fillRect(18, 5, 28, 7);
    ctx.fillStyle = "#5d351d";
    ctx.fillRect(12, 12, 9, 17);

    ctx.restore();
  }

  function drawBandit(b) {
    ctx.save();
    ctx.translate(b.x, b.y);

    ctx.fillStyle = "rgba(40,20,10,.3)";
    ctx.beginPath();
    ctx.ellipse(0, 34, 35, 11, 0, 0, Math.PI * 2);
    ctx.fill();

    ctx.fillStyle = "#693535";
    ctx.fillRect(-20, -4, 40, 44);

    ctx.fillStyle = "#b9784d";
    ctx.beginPath();
    ctx.arc(0, -28, 20, 0, Math.PI * 2);
    ctx.fill();

    ctx.fillStyle = "#24201c";
    ctx.fillRect(-28, -50, 56, 9);
    ctx.fillRect(-15, -65, 30, 17);

    ctx.fillStyle = "#111";
    ctx.fillRect(-10, -32, 6, 5);
    ctx.fillRect(5, -32, 6, 5);

    ctx.fillStyle = "#222";
    ctx.fillRect(19, 7, 26, 7);

    ctx.restore();
  }

  function drawBullet(b) {
    ctx.fillStyle = player.gold ? "#ffd700" : "#f3e3a3";
    ctx.shadowColor = player.gold ? "#ffd700" : "transparent";
    ctx.shadowBlur = player.gold ? 12 : 0;

    ctx.beginPath();
    ctx.arc(b.x, b.y, b.r, 0, Math.PI * 2);
    ctx.fill();

    ctx.shadowBlur = 0;
  }

  function drawParticles() {
    for (const p of particles) {
      ctx.globalAlpha = Math.max(0, p.life / 40);
      ctx.fillStyle = "#e9b45c";
      ctx.fillRect(p.x, p.y, p.size, p.size);
    }

    ctx.globalAlpha = 1;
  }

  function render() {
    drawDesert();

    for (const b of bandits) drawBandit(b);
    for (const b of bullets) drawBullet(b);

    drawCharacter();
    drawParticles();

    if (paused && $("shop").style.display !== "flex") {
      ctx.fillStyle = "rgba(0,0,0,.35)";
      ctx.fillRect(0, 0, W, H);
      ctx.fillStyle = "#fff";
      ctx.textAlign = "center";
      ctx.font = "bold 60px system-ui";
      ctx.fillText("PAUSED", W / 2, H / 2);
    }
  }

  function gameLoop(time) {
    const dt = Math.min(2, (time - lastTime) / 16.67 || 1);
    lastTime = time;

    update(dt);
    render();

    requestAnimationFrame(gameLoop);
  }

  function triggerAd() {
    if (!running) return;

    running = false;
    paused = true;
    $("adScreen").style.display = "flex";

    let seconds = 3;
    $("adTimer").textContent = "Loading Advertisement... " + seconds;
    $("respawn").classList.add("hidden");

    const timer = setInterval(() => {
      seconds--;

      if (seconds > 0) {
        $("adTimer").textContent = "Loading Advertisement... " + seconds;
      } else {
        clearInterval(timer);
        $("adTimer").textContent = "Advertisement Complete!";
        $("respawn").classList.remove("hidden");
      }
    }, 1000);
  }

  function openShop() {
    if (!running) return;

    paused = true;
    $("shop").style.display = "flex";
    updateHUD();
  }

  function closeShop() {
    $("shop").style.display = "none";
    paused = false;
  }

  function buySpeed() {
    if (!player.boots && cash >= 50) {
      cash -= 50;
      player.boots = true;
      updateHUD();
    }
  }

  function buyGold() {
    if (!player.gold && cash >= 100) {
      cash -= 100;
      player.gold = true;
      player.damage = 2;
      updateHUD();
    }
  }

  function startGame() {
    $("startScreen").classList.add("hidden");
    resetPlayer();
    running = true;
    paused = false;
    spawnTimer = 30;
  }

  $("runBtn").addEventListener("click", startGame);
  $("pauseBtn").addEventListener("click", openShop);
  $("closeShop").addEventListener("click", closeShop);
  $("buySpeed").addEventListener("click", buySpeed);
  $("buyGold").addEventListener("click", buyGold);

  $("respawn").addEventListener("click", () => {
    $("adScreen").style.display = "none";
    resetPlayer();
    running = true;
    paused = false;
  });

  function bindHold(button, property) {
    const start = e => {
      e.preventDefault();
      keys[property] = true;
      button.classList.add("active");
    };

    const end = e => {
      e.preventDefault();
      keys[property] = false;
      button.classList.remove("active");
    };

    button.addEventListener("pointerdown", start);
    button.addEventListener("pointerup", end);
    button.addEventListener("pointercancel", end);
    button.addEventListener("pointerleave", end);
  }

  bindHold($("up"), "up");
  bindHold($("down"), "down");
  bindHold($("left"), "left");
  bindHold($("right"), "right");

  $("shoot").addEventListener("pointerdown", e => {
    e.preventDefault();
    $("shoot").classList.add("active");
    shoot();
  });

  $("shoot").addEventListener("pointerup", () => {
    $("shoot").classList.remove("active");
  });

  window.addEventListener("keydown", e => {
    if (e.key === "ArrowUp" || e.key.toLowerCase() === "w") keys.up = true;
    if (e.key === "ArrowDown" || e.key.toLowerCase() === "s") keys.down = true;
    if (e.key === "ArrowLeft" || e.key.toLowerCase() === "a") keys.left = true;
    if (e.key === "ArrowRight" || e.key.toLowerCase() === "d") keys.right = true;
    if (e.code === "Space") shoot();
  });

  window.addEventListener("keyup", e => {
    if (e.key === "ArrowUp" || e.key.toLowerCase() === "w") keys.up = false;
    if (e.key === "ArrowDown" || e.key.toLowerCase() === "s") keys.down = false;
    if (e.key === "ArrowLeft" || e.key.toLowerCase() === "a") keys.left = false;
    if (e.key === "ArrowRight" || e.key.toLowerCase() === "d") keys.right = false;
  });

  window.addEventListener("resize", resizeCanvas);

  updateHUD();
  requestAnimationFrame(gameLoop);
})();
</script>
</body>
</html>
