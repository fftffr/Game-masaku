<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Game Masak-masak 🍳</title>
  <style>
    body { background:#fffae6; font-family:sans-serif; text-align:center; margin:0; padding:20px; }
    #game { margin: auto; width:300px; }
    .order, .ingredients { font-size:2em; margin:10px; }
    button { font-size:1.5em; padding:10px; margin:5px; cursor:pointer; }
    #status { margin:15px; font-size:1.2em; }
    .animate { animation: pop 0.2s; }
    @keyframes pop { 0%{transform:scale(1);} 50%{transform:scale(1.2);}100%{transform:scale(1);} }
  </style>
</head>
<body>
<div id="game">
  <h1>🍽️ Game Masak-masak</h1>
  <div id="level">Level: 1</div>
  <div id="score">Skor: 0 | Nyawa: ❤️❤️❤️</div>
  <div class="order" id="order"></div>
  <div class="ingredients" id="ingredients"></div>
  <div id="status"></div>
</div>

<script>
// --- variabel game ---
let level = 1, score = 0, lives = 3;
const maxLevels = 3;
const allMenu = [
  ["🍔","🍟"], ["🍕","🥤"], ["🍣","🍙"], ["🌭","🥤"], ["🍝","🥗"]
];
// bahan pilihan
const options = ["🍔","🍟","🍕","🥤","🍣","🍙","🌭","🍝","🥗"];

// --- fungsi mulai level ---
function startLevel(){
  document.getElementById("status").textContent = "";
  document.getElementById("level").textContent = "Level: "+level;
  document.getElementById("score").textContent = "Skor: "+score+" | Nyawa: "+ "❤️".repeat(lives);
  order = allMenu[Math.floor(Math.random()*allMenu.length)];
  document.getElementById("order").textContent = "Pesanan: " + order.join(" + ");
  renderButtons();
  // timer
  clearTimeout(window.levelTimer);
  window.levelTimer = setTimeout(() => {
    lives--; fail("Waktu habis!");
  }, 5000 - (level-1)*1000);
}

// --- render tombol bahan ---
function renderButtons(){
  const div = document.getElementById("ingredients");
  div.innerHTML = "";
  options.forEach(b => {
    const btn = document.createElement("button");
    btn.textContent = b;
    btn.onclick = () => choose(b, btn);
    div.appendChild(btn);
  });
}

// --- ketika memilih bahan ---
let chosen = [];
function choose(b, btn){
  btn.classList.add("animate");
  setTimeout(()=>btn.classList.remove("animate"),200);
  chosen.push(b);
  if(chosen.length === order.length){
    clearTimeout(window.levelTimer);
    checkOrder();
  }
}

// --- cek urutan atas pilihan ---
function checkOrder(){
  const ok = order.every(o => chosen.includes(o)) && chosen.length===order.length;
  if(ok) {
    score += level * 10;
    success("Pesanan benar!");
  } else {
    lives--;
    fail("Salah bahan!");
  }
}

// --- sukses atau gagal ---
function success(msg){ nextStep(msg); }
function fail(msg){ nextStep(msg); }

function nextStep(msg){
  document.getElementById("status").textContent = msg;
  chosen = [];
  if(lives <= 0){
    alert("Game Over! Skor akhir: "+score);
    resetGame();
  } else if(score >= level*10 && level < maxLevels){
    level++;
    setTimeout(startLevel,1000);
  } else if(level >= maxLevels && score >= maxLevels*10){
    alert("🎉 Kamu menang! Skor: "+score);
    resetGame();
  } else {
    setTimeout(startLevel,1000);
  }
}

// --- reset game ---
function resetGame(){
  level = 1; score = 0; lives = 3;
  startLevel();
}

// mulai pertama kali
startLevel();
</script>

</body>
</html>
