<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Démo du Mini-Jeu</title>

  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: #1e6bff; /* fond bleu */
      font-family: Arial;
      color: white;
    }

    #menu { text-align: center; }

    #playBtn {
      background: #1dd11d;
      border: none;
      padding: 20px 40px;
      font-size: 24px;
      border-radius: 12px;
      cursor: pointer;
    }

    #game { text-align: center; }

    #arena {
      margin: 20px auto;
      width: 500px;
      height: 300px;
      background: #0f2fa8;
      border: 3px solid white;
      position: relative;
      overflow: hidden;
    }

    .target {
      width: 40px;
      height: 40px;
      background: red;
      position: absolute;
      cursor: pointer;
    }

    .hidden { display: none; }

    #screamer {
      position: fixed;
      inset: 0;
      background: black;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    #screamer img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  </style>
</head>

<body>

  <div id="menu">
    <h1>Démonstration</h1>
    <button id="playBtn">Jouer</button>
  </div>

  <div id="game" class="hidden">
    <h2>Attrape le carré rouge avant qu'il disparaisse !</h2>
    <p>Niveaux restants : <span id="levels">5</span></p>
    <div id="arena"></div>
  </div>

  <div id="screamer" class="hidden">
    <img src="https://i.imgur.com/3g7nmJC.png">
    <audio id="screamSound" src="https://www.myinstants.com/media/sounds/wilhelm_scream.mp3"></audio>
  </div>

  <script>
    const menu = document.getElementById("menu");
    const game = document.getElementById("game");
    const playBtn = document.getElementById("playBtn");
    const arena = document.getElementById("arena");
    const levelSpan = document.getElementById("levels");

    const screamer = document.getElementById("screamer");
    const screamSound = document.getElementById("screamSound");

    let levels = 5;
    let timer = null;

    playBtn.onclick = () => {
      menu.classList.add("hidden");
      game.classList.remove("hidden");
      startLevel();
    };

    function startLevel() {
      levelSpan.textContent = levels;
      arena.innerHTML = "";

      const t = document.createElement("div");
      t.className = "target";

      const x = Math.random() * (arena.clientWidth - 40);
      const y = Math.random() * (arena.clientHeight - 40);
      t.style.left = x + "px";
      t.style.top = y + "px";

      t.onclick = () => {
        clearTimeout(timer);
        levels--;

        if (levels === 0) {
          launchScreamer();
          return;
        }
        startLevel();
      };

      arena.appendChild(t);

      timer = setTimeout(() => {
        launchScreamer();
      }, 900);
    }

    function launchScreamer() {
      game.classList.add("hidden");
      screamer.classList.remove("hidden");
      screamSound.play();
    }
  </script>

</body>
</html>
