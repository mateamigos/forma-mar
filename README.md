<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Desafío Matemático Submarino</title>
    <style>
        * { box-sizing: border-box; }

        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            text-align: center; 
            background: url('https://drive.google.com/thumbnail?id=1o-_LkZu7U_XRxp41q6yBwz0I_DIjL-VM&sz=w1200') no-repeat center center fixed; 
            background-size: cover;
            color: #fff; 
            margin: 0;
            height: 100vh;
            width: 100vw;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }

        .container { 
            width: 95%;
            max-width: 1000px;
            height: 95vh;
            background: rgba(255, 255, 255, 0.88); 
            backdrop-filter: blur(8px);
            padding: clamp(10px, 3vw, 20px); 
            border-radius: 20px; 
            box-shadow: 0 12px 40px rgba(0,0,0,0.4); 
            border: 4px solid #00acc1;
            color: #004d40;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        h1 { 
            margin: 0 0 10px 0; 
            color: #007c91; 
            text-transform: uppercase; 
            font-size: clamp(1rem, 4vw, 1.5rem); 
        }

        .timer-container {
            width: 100%;
            height: 12px;
            background: #e0e0e0;
            border-radius: 50px;
            margin-bottom: 10px;
            overflow: hidden;
            flex-shrink: 0;
        }
        #timer-bar {
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, #ff5252, #ff1744);
            transition: width 1s linear;
        }

        .stats-bar {
            display: flex;
            justify-content: space-around;
            margin-bottom: 10px;
            font-weight: bold;
            font-size: clamp(0.9rem, 3vw, 1.2rem);
            flex-shrink: 0;
        }

        .legend { 
            display: flex; 
            justify-content: center; 
            flex-wrap: wrap; 
            gap: 5px; 
            margin-bottom: 10px;
            flex-shrink: 0;
        }
        .legend-item { 
            display: flex; 
            align-items: center; 
            gap: 5px; 
            background: white; 
            padding: 5px 12px; 
            border-radius: 50px;
            border: 2px solid #b2ebf2; 
            font-size: clamp(0.8rem, 2.5vw, 1rem);
            font-weight: 800;
        }

        .small-icon { 
            width: clamp(30px, 8vw, 60px); 
            height: auto; 
        }

        .game-board { 
            flex-grow: 1; 
            display: flex; 
            flex-wrap: wrap; 
            justify-content: center; 
            align-items: center; 
            align-content: center;
            background: rgba(255, 255, 255, 0.5); 
            border-radius: 15px; 
            margin-bottom: 15px; 
            border: 2px dashed #00acc1;
            overflow: hidden; 
            padding: 10px;
        }

        /* --- ESTILO DE ICONOS DEL TABLERO --- */
        .icon-img { 
            width: clamp(50px, 12vw, 100px); 
            height: auto; 
            margin: -15px -20px; 
            flex-shrink: 0; 
            filter: drop-shadow(0px 4px 2px rgba(0,0,0,0.3));
            user-select: none;
        }

        /* MODIFICACIÓN: Iconos más grandes específicamente en móviles */
        @media (max-width: 600px) {
            .icon-img {
                width: clamp(80px, 18vw, 110px); /* Aumentado el tamaño mínimo de 50px a 80px */
                margin: -12px -15px; /* Ajuste de margen para que no se amontonen */
            }
        }

        .options { 
            display: grid; 
            grid-template-columns: repeat(2, 1fr); 
            gap: 10px; 
            flex-shrink: 0;
        }

        @media (min-width: 600px) {
            .options { grid-template-columns: repeat(4, 1fr); }
        }

        button { 
            padding: clamp(10px, 3vh, 20px); 
            font-size: clamp(1rem, 4vw, 1.5rem); 
            font-weight: bold; 
            cursor: pointer; 
            border: none; 
            border-radius: 12px; 
            background: linear-gradient(135deg, #42a5f5, #1e88e5); 
            color: white; 
            box-shadow: 0 4px 0 #1565c0;
            touch-action: manipulation; 
        }
        button:active { transform: translateY(2px); box-shadow: 0 1px 0 #1565c0; }

        #final-screen {
            display: none;
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: white; border-radius: 20px;
            flex-direction: column; align-items: center; justify-content: center;
            z-index: 10;
        }

        .feedback { 
            font-size: clamp(1rem, 4vw, 1.3rem); 
            font-weight: bold; 
            height: 30px; 
            margin-top: 5px;
            flex-shrink: 0;
        }

        @media (max-height: 500px) {
            h1 { display: none; } 
            .container { height: 98vh; padding: 5px; }
            .legend { margin-bottom: 5px; }
            .game-board { margin-bottom: 5px; }
            .icon-img { width: clamp(50px, 10vh, 70px); }
        }
    </style>
</head>
<body>

<div class="container" id="main-container">
    <div id="final-screen">
        <h2 style="font-size: clamp(1.5rem, 6vw, 2.5rem); color: #007c91;">¡FIN DEL JUEGO!</h2>
        <p style="font-size: clamp(1rem, 4vw, 1.5rem);">Tu puntuación final es:</p>
        <div style="font-size: clamp(2.5rem, 10vw, 4rem); font-weight: bold; color: #004d40; margin: 10px 0;" id="final-points">0</div>
        <button onclick="location.reload()" style="padding: 15px 30px;">Jugar de nuevo</button>
    </div>

    <h1>Desafío Matemático Submarino</h1>

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
        { name: 'mil', val: 1000, img: 'https://drive.google.com/thumbnail?id=1Y-zGahugC-JMPPzdcDrnJvmnEPVYRGq5&sz=w800' },
        { name: 'cien', val: 100, img: 'https://drive.google.com/thumbnail?id=1OHiP5Kvty5rTZlQIT6tnroX94_vQqXwS&sz=w800' },
        { name: 'diez', val: 10, img: 'https://drive.google.com/thumbnail?id=1x1-VYLevq4cNwwtAyAnwujjCxCU-Jazh&sz=w800' },
        { name: 'uno', val: 1, img: 'https://drive.google.com/thumbnail?id=1d24q9ReryV_6lZJXEEybruDEhoYLrn-6&sz=w800' }
    ];

    let currentResult = 0;
    let score = 0;
    let round = 1;
    let timeLeft = 20;
    let timerInterval;

    const legendBox = document.getElementById('legend-box');
    items.forEach(item => {
        legendBox.innerHTML += `<div class="legend-item"><img src="${item.img}" class="small-icon"> = ${item.val.toLocaleString('es-ES')}</div>`;
    });

    function startTimer() {
        clearInterval(timerInterval);
        timeLeft = 20;
        updateTimerBar();
        timerInterval = setInterval(() => {
            timeLeft--;
            updateTimerBar();
            if (timeLeft <= 0) {
                clearInterval(timerInterval);
                checkAnswer(null);
            }
        }, 1000);
    }

    function updateTimerBar() {
        const bar = document.getElementById('timer-bar');
        const percentage = (timeLeft / 20) * 100;
        bar.style.width = percentage + "%";
    }

    function initGame() {
        if (round > 50) {
            showFinalResult();
            return;
        }

        document.getElementById('current-round').innerText = round;
        const board = document.getElementById('board');
        const optionsContainer = document.getElementById('options-container');
        const feedback = document.getElementById('feedback');
         
        board.innerHTML = '';
        optionsContainer.innerHTML = '';
        feedback.innerText = '';
        currentResult = 0;

        let allImages = [];
        items.forEach(item => {
            const count = Math.floor(Math.random() * 5); 
            currentResult += count * item.val;
            for(let i = 0; i < count; i++) {
                const img = document.createElement('img');
                img.src = item.img;
                img.className = 'icon-img';
                img.style.transform = `rotate(${Math.random() * 30 - 15}deg) translateY(${Math.random() * 10 - 5}px)`;
                allImages.push(img);
            }
        });

        if (currentResult === 0) return initGame();

        allImages.sort(() => Math.random() - 0.5);
        allImages.forEach(img => board.appendChild(img));

        let options = [currentResult];
        while(options.length < 4) {
            let error = [1, 10, 100, -1, -10, -100][Math.floor(Math.random() * 6)];
            let fake = Math.abs(currentResult + error);
            if (fake !== 0 && !options.includes(fake)) options.push(fake);
        }
        options.sort(() => Math.random() - 0.5);

        options.forEach(opt => {
            const btn = document.createElement('button');
            btn.innerText = opt.toLocaleString('es-ES');
            btn.onclick = () => checkAnswer(opt);
            optionsContainer.appendChild(btn);
        });

        startTimer();
    }

    function checkAnswer(selected) {
        clearInterval(timerInterval);
        const feedback = document.getElementById('feedback');
        
        if(selected === currentResult) {
            feedback.innerText = "¡EXCELENTE! +10 🌊";
            feedback.style.color = "#2e7d32";
            score += 10;
        } else {
            feedback.innerText = selected === null ? "¡TIEMPO AGOTADO! -5 ⏰" : "¡SIGUE CONTANDO! -5 💡";
            feedback.style.color = "#c62828";
            score = Math.max(0, score - 5);
        }

        document.getElementById('points').innerText = score;
        round++;
        setTimeout(initGame, 1200);
    }

    function showFinalResult() {
        document.getElementById('final-screen').style.display = 'flex';
        document.getElementById('final-points').innerText = score;
    }

    initGame();
</script>

</body>
</html>
