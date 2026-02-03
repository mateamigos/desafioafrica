<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DESAFÍO ÁFRICA</title>
    <link href="https://fonts.googleapis.com/css2?family=Bangers&display=swap" rel="stylesheet">
    
    <style>
        * { box-sizing: border-box; }

        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            text-align: center; 
            background: url('https://drive.google.com/thumbnail?id=1FWVC24WelRpy_Bx1XOcp47K_kAaYfdJh&sz=w1200') no-repeat center center fixed; 
            background-size: cover;
            color: #fff; 
            margin: 0;
            height: 100vh;
            width: 100vw;
            display: flex; align-items: center; justify-content: center;
            overflow: hidden;
        }

        #mute-control {
            position: absolute; top: 10px; right: 10px; z-index: 100;
            background: rgba(139, 69, 19, 0.8); border: 2px solid #FFD700;
            border-radius: 50%; width: 45px; height: 45px; cursor: pointer;
            font-size: 1.2rem; display: flex; align-items: center; justify-content: center;
        }

        .container { 
            width: 95%; max-width: 1000px; 
            height: 95vh;
            background: rgba(255, 245, 230, 0.7); 
            backdrop-filter: blur(5px);
            padding: 10px; border-radius: 20px; 
            box-shadow: 0 12px 40px rgba(0,0,0,0.5); 
            border: 4px solid #8B4513;
            color: #3E2723;
            display: flex; flex-direction: column; position: relative;
            overflow: hidden;
        }

        h1 { 
            margin: 5px 0; font-family: 'Bangers', cursive;
            color: #8B4513; text-transform: uppercase; 
            font-size: clamp(1.5rem, 5vw, 2.5rem);
            text-shadow: 1px 1px 0px rgba(255,255,255,0.8);
        }

        #start-screen, #final-screen {
            display: flex; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(255, 248, 240, 0.98); border-radius: 15px;
            flex-direction: column; align-items: center; justify-content: center;
            z-index: 20; padding: 20px;
        }

        #final-screen { display: none; }

        .timer-container {
            width: 100%; height: 10px; background: rgba(139, 69, 19, 0.2);
            border-radius: 50px; margin-bottom: 5px; overflow: hidden;
            border: 1px solid #8B4513; flex-shrink: 0;
        }
        #timer-bar { width: 100%; height: 100%; background: linear-gradient(90deg, #FFD700, #FF4500); }

        .stats-bar {
            display: flex; justify-content: space-around;
            margin-bottom: 5px; font-weight: bold; font-size: 1rem; flex-shrink: 0;
        }

        .legend { 
            display: flex; justify-content: center; flex-wrap: wrap; gap: 5px; 
            margin-bottom: 10px; flex-shrink: 0;
        }
        .legend-item { 
            display: flex; align-items: center; gap: 4px; 
            background: rgba(255, 255, 255, 0.8); padding: 2px 8px; 
            border-radius: 50px; border: 1px solid #8B4513; font-size: 0.75rem;
        }
        .legend-img { width: 22px; height: 22px; object-fit: contain; }

        .game-board { 
            flex-grow: 1;
            display: flex; flex-wrap: wrap; justify-content: center; align-items: center;
            background: rgba(255, 248, 235, 0.95); 
            border-radius: 15px; margin-bottom: 10px; 
            border: 2px dashed #8B4513; padding: 10px;
            overflow-y: auto;
        }

        /* AJUSTE CLAVE: Tamaño uniforme para todas las imágenes del tablero */
        .icon-img { 
            width: 45px; 
            height: 45px; 
            object-fit: contain; 
            margin: 5px; 
            filter: drop-shadow(2px 2px 2px rgba(0,0,0,0.2));
        }

        .options { 
            display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; 
            flex-shrink: 0; padding-bottom: 5px;
        }
        @media (min-width: 600px) { .options { grid-template-columns: repeat(4, 1fr); } }

        .btn-warrior { 
            font-family: 'Bangers', cursive; padding: 10px; 
            font-size: 1.5rem; cursor: pointer; border: none; 
            border-radius: 12px; background: linear-gradient(135deg, #8B4513, #D2691E); 
            color: white; box-shadow: 0 4px 0 #3E2723;
            transition: all 0.1s;
        }
        .btn-warrior:active { transform: translateY(2px); box-shadow: 0 2px 0 #3E2723; }

        .feedback { 
            position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            font-family: 'Bangers', cursive;
            font-size: clamp(4rem, 15vw, 7rem);
            z-index: 3000; pointer-events: none;
            text-shadow: 4px 4px 15px rgba(0,0,0,0.6);
            display: none;
        }

        @keyframes popIn {
            0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            50% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; }
            100% { transform: translate(-50%, -50%) scale(1); opacity: 0; }
        }
    </style>
</head>
<body>

<audio id="bg-music" loop>
    <source src="https://assets.mixkit.co/music/preview/mixkit-african-drums-and-tribal-rhythm-628.mp3" type="audio/mpeg">
</audio>

<button id="mute-control" onclick="toggleMusic()">🔊</button>

<div class="container" id="main-container">
    <div id="start-screen">
        <h1>🦁 DESAFÍO ÁFRICA 🐘</h1>
        <div style="background: rgba(139,69,19,0.1); padding: 15px; border-radius: 15px; border: 2px dashed #8B4513; margin-bottom: 15px;">
            <div style="color: #2E7D32; font-weight: bold;">✨ ACIERTO: +10</div>
            <div style="color: #C62828; font-weight: bold;">⏳ FALLO: -5</div>
        </div>
        <button class="btn-warrior" style="padding: 15px 30px;" onclick="startGame()">¡A JUGAR!</button>
    </div>

    <div id="final-screen">
        <h2 style="font-family: 'Bangers', cursive; font-size: 2.5rem; color: #8B4513;">MISIÓN CUMPLIDA</h2>
        <div style="font-size: 4rem; font-family: 'Bangers', cursive;" id="final-points">0</div>
        <button class="btn-warrior" onclick="location.reload()">REINTENTAR</button>
    </div>

    <h1>DESAFÍO ÁFRICA</h1>

    <div class="timer-container"><div id="timer-bar"></div></div>
    
    <div class="stats-bar">
        <span>Ronda: <span id="current-round">1</span>/50</span>
        <span>Puntos: <span id="points">0</span></span>
    </div>

    <div class="legend" id="legend-box"></div>
    <div class="game-board" id="board"></div>
    <div class="options" id="options-container"></div>
    <div class="feedback" id="feedback"></div>
</div>

<script>
    const items = [
        { val: 10000, img: 'https://drive.google.com/thumbnail?id=1VEIbEv9Bc5NMgaU-23gEgyfVaS8eJqA0&sz=w400' },
        { val: 1000, img: 'https://drive.google.com/thumbnail?id=1PfHy6xNevx2xoiIO-mDskAHZsr23eaYb&sz=w400' },
        { val: 100, img: 'https://drive.google.com/thumbnail?id=1RdGVIIFKEMPJPEGr4W6tzU3J773fK8CN&sz=w400' },
        { val: 10, img: 'https://drive.google.com/thumbnail?id=1AVxqRVOaPcOiVNwBJv_v9hG7aqT-9qkP&sz=w400' },
        { val: 1, img: 'https://drive.google.com/thumbnail?id=1I_T_gt_1tHtaZ1c8fZDY7UVEVRGQwkjB&sz=w400' }
    ];

    let currentResult = 0, score = 0, round = 1, timeLeft = 35, timerInterval;
    const music = document.getElementById('bg-music');
    const muteBtn = document.getElementById('mute-control');
    const legendBox = document.getElementById('legend-box');

    items.forEach(item => {
        legendBox.innerHTML += `<div class="legend-item"><img src="${item.img}" class="legend-img"> = ${item.val.toLocaleString('es-ES')}</div>`;
    });

    function toggleMusic() {
        if (music.paused) { music.play(); muteBtn.innerText = "🔊"; } 
        else { music.pause(); muteBtn.innerText = "🔇"; }
    }

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        music.volume = 0.3;
        music.play().catch(() => {});
        initGame();
    }

    function startTimer() {
        clearInterval(timerInterval);
        timeLeft = 35;
        updateTimerBar();
        timerInterval = setInterval(() => {
            timeLeft--;
            updateTimerBar();
            if (timeLeft <= 0) { checkAnswer(null); }
        }, 1000);
    }

    function updateTimerBar() {
        document.getElementById('timer-bar').style.width = (timeLeft / 35) * 100 + "%";
    }

    function initGame() {
        if (round > 50) { showFinalResult(); return; }
        document.getElementById('current-round').innerText = round;
        const board = document.getElementById('board');
        const optionsContainer = document.getElementById('options-container');
        board.innerHTML = ''; optionsContainer.innerHTML = '';
        document.getElementById('feedback').style.display = 'none';
        currentResult = 0;

        let allImages = [];
        items.forEach(item => {
            const count = Math.floor(Math.random() * 5); 
            currentResult += count * item.val;
            for(let i = 0; i < count; i++) {
                const img = document.createElement('img');
                img.src = item.img; img.className = 'icon-img';
                img.style.transform = `rotate(${Math.random() * 20 - 10}deg)`;
                allImages.push(img);
            }
        });

        if (currentResult === 0) return initGame();
        allImages.sort(() => Math.random() - 0.5).forEach(img => board.appendChild(img));

        let options = [currentResult];
        while(options.length < 4) {
            let error = [1, 10, 100, 1000, -1, -10, -100, -1000][Math.floor(Math.random() * 8)];
            let fake = Math.abs(currentResult + error);
            if (fake !== 0 && !options.includes(fake)) options.push(fake);
        }
        
        options.sort(() => Math.random() - 0.5).forEach(opt => {
            const btn = document.createElement('button');
            btn.className = "btn-warrior";
            btn.innerText = opt.toLocaleString('es-ES');
            btn.onclick = () => checkAnswer(opt);
            optionsContainer.appendChild(btn);
        });
        startTimer();
    }

    function checkAnswer(selected) {
        clearInterval(timerInterval);
        const fb = document.getElementById('feedback');
        
        if(selected === currentResult) {
            fb.innerText = "+10 ✨"; 
            fb.style.color = "#FFD700"; 
            score += 10;
        } else {
            fb.innerText = "-5 🐾";
            fb.style.color = "#FF4500"; 
            score = Math.max(0, score - 5);
        }

        fb.style.display = "block";
        fb.style.animation = "popIn 1s forwards";

        document.getElementById('points').innerText = score;
        round++;
        setTimeout(initGame, 1000);
    }

    function showFinalResult() {
        document.getElementById('final-screen').style.display = 'flex';
        document.getElementById('final-points').innerText = score + " PUNTOS";
    }
</script>

</body>
</html>
