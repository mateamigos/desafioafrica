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
            height: 100vh; width: 100vw;
            display: flex; align-items: center; justify-content: center;
            overflow: hidden;
        }

        /* Botón de Silencio */
        #mute-control {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 100;
            background: rgba(139, 69, 19, 0.8);
            border: 2px solid #FFD700;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            cursor: pointer;
            font-size: 1.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 8px rgba(0,0,0,0.5);
        }

        .container { 
            width: 98%; max-width: 1100px; height: 98vh;
            background: rgba(255, 245, 230, 0.6); 
            backdrop-filter: blur(5px);
            padding: 15px; border-radius: 20px; 
            box-shadow: 0 12px 40px rgba(0,0,0,0.5); 
            border: 4px solid #8B4513;
            color: #3E2723;
            display: flex; flex-direction: column; position: relative;
        }

        h1 { 
            margin: 0 0 5px 0; font-family: 'Bangers', cursive;
            color: #8B4513; text-transform: uppercase; 
            font-size: clamp(2rem, 6vw, 3rem);
            text-shadow: 2px 2px 0px rgba(255,255,255,0.8);
        }

        #start-screen, #final-screen {
            display: flex; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(255, 248, 240, 0.98); border-radius: 15px;
            flex-direction: column; align-items: center; justify-content: center;
            z-index: 20; color: #3E2723; padding: 20px; border: 4px solid #8B4513;
        }

        #final-screen { display: none; }

        .rules-container {
            background: rgba(139, 69, 19, 0.1); padding: 20px;
            border-radius: 15px; border: 2px dashed #8B4513;
            margin: 20px 0; max-width: 90%;
        }
        .rule-item { font-size: 1.2rem; margin: 10px 0; font-weight: bold; }
        .gain { color: #2E7D32; }
        .loss { color: #C62828; }

        .timer-container {
            width: 100%; height: 10px; background: rgba(139, 69, 19, 0.2);
            border-radius: 50px; margin-bottom: 8px; overflow: hidden;
            border: 1px solid #8B4513;
        }
        #timer-bar {
            width: 100%; height: 100%;
            background: linear-gradient(90deg, #FFD700, #FF4500);
            transition: width 1s linear;
        }

        .stats-bar {
            display: flex; justify-content: space-around;
            margin-bottom: 8px; font-weight: bold;
            font-size: clamp(0.8rem, 2.5vw, 1.1rem);
        }

        .legend { display: flex; justify-content: center; flex-wrap: wrap; gap: 8px; margin-bottom: 10px; }
        .legend-item { 
            display: flex; align-items: center; gap: 4px; 
            background: rgba(255, 255, 255, 0.8); padding: 4px 10px; 
            border-radius: 50px; border: 1.5px solid #8B4513; 
            font-size: 0.8rem; font-weight: 800;
        }
        
        /* Ajuste solicitado: Imagen de leyenda uniforme */
        .legend-img { width: 22px; height: 22px; object-fit: contain; }

        .game-board { 
            flex-grow: 2; display: flex; flex-wrap: wrap; 
            justify-content: center; align-items: center;
            background: rgba(255, 248, 235, 0.95); 
            border-radius: 15px; margin-bottom: 10px; 
            border: 2px dashed #8B4513; padding: 20px;
        }

        .icon-img { width: 50px; height: 50px; object-fit: contain; margin: 5px; }

        .options { display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; }
        @media (min-width: 600px) { .options { grid-template-columns: repeat(4, 1fr); } }

        .btn-warrior { 
            font-family: 'Bangers', cursive; padding: 15px 45px; 
            font-size: 1.8rem; letter-spacing: 2px; cursor: pointer; border: none; 
            border-radius: 12px; background: linear-gradient(135deg, #8B4513, #D2691E, #8B0000); 
            color: white; box-shadow: 0 6px 0 #3E2723;
            text-shadow: 2px 2px 2px rgba(0,0,0,0.5); transition: all 0.1s;
        }
        .btn-warrior:active { transform: translateY(4px); box-shadow: 0 2px 0 #3E2723; }

        .feedback { font-size: 1rem; font-weight: bold; height: 25px; margin-top: 5px; }
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
        <div class="rules-container">
            <div class="rule-item gain">✨ ACIERTO: +10 Puntos</div>
            <div class="rule-item loss">⏳ FALLO / TIEMPO: -5 Puntos</div>
        </div>
        <p style="font-family: 'Segoe UI'; margin-bottom: 25px;">Suma los símbolos. 50 rondas. 35 segundos cada una.</p>
        <button class="btn-warrior" onclick="startGame()">COMENZAR DESAFÍO</button>
    </div>

    <div id="final-screen">
        <h2 style="font-family: 'Bangers', cursive; font-size: 3rem; color: #8B4513;">¡MISIÓN CUMPLIDA!</h2>
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
        // Ajuste solicitado: Uso de la clase legend-img para uniformidad
        legendBox.innerHTML += `<div class="legend-item"><img src="${item.img}" class="legend-img"> = ${item.val.toLocaleString('es-ES')}</div>`;
    });

    function toggleMusic() {
        if (music.paused) {
            music.play();
            muteBtn.innerText = "🔊";
        } else {
            music.pause();
            muteBtn.innerText = "🔇";
        }
    }

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        music.volume = 0.4;
        music.play().catch(() => console.log("Audio requiere interacción"));
        initGame();
    }

    function startTimer() {
        clearInterval(timerInterval);
        timeLeft = 35; 
        updateTimerBar();
        timerInterval = setInterval(() => {
            timeLeft--;
            updateTimerBar();
            if (timeLeft <= 0) { clearInterval(timerInterval); checkAnswer(null); }
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
        document.getElementById('feedback').innerText = '';
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
            btn.style.fontSize = "1.4rem"; btn.style.padding = "10px";
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
            fb.innerText = "¡EXCELENTE! +10 ✨"; fb.style.color = "#2E7D32"; score += 10;
        } else {
            fb.innerText = selected === null ? "¡TIEMPO AGOTADO! -5 ⏳" : "¡SIGUE CONTANDO! -5 🤔";
            fb.style.color = "#C62828"; score = Math.max(0, score - 5);
        }
        document.getElementById('points').innerText = score;
        round++;
        setTimeout(initGame, 1000);
    }

    function showFinalResult() {
        music.volume = 0.1;
        document.getElementById('final-screen').style.display = 'flex';
        document.getElementById('final-points').innerText = score + " PUNTOS";
    }
</script>

</body>
</html>
