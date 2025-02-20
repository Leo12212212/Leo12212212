<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galactic Quiz Battle</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game-container">
        <!-- Menú principal -->
        <div id="main-menu">
            <h1>¡Bienvenido a Galactic Quiz Battle!</h1>
            <button onclick="startGame()">Iniciar Juego</button>
        </div>

        <!-- Juego en progreso -->
        <div id="game-play" style="display:none;">
            <h2 id="level-info">Nivel 1</h2>
            <div id="alien-counter">Alienígenas eliminados: <span id="aliens-eliminados">0</span>/15</div>
            <div id="quiz-question"></div>
            <div id="answers"></div>
            <button id="next-level-btn" style="display:none;" onclick="nextLevel()">Continuar</button>
        </div>

        <!-- Tienda -->
        <div id="store" style="display:none;">
            <h2>Tienda</h2>
            <p>Monedas: <span id="coins">0</span></p>
            <p>Diamantes: <span id="diamonds">0</span></p>
            <button onclick="buyUpgrade()">Comprar Mejora de Velocidad (100 monedas)</button>
            <button onclick="buyPowerup()">Comprar Power-up (5 diamantes)</button>
            <button onclick="backToGame()">Regresar al juego</button>
        </div>
    </div>

    <script src="game.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    background-color: #1a1a1a;
    color: white;
    text-align: center;
}

#game-container {
    padding: 20px;
}

button {
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
    margin: 10px;
}

button:hover {
    background-color: #45a049;
}

h1, h2 {
    font-size: 2rem;
}

#quiz-question {
    font-size: 1.5rem;
    margin: 20px;
}

#answers button {
    margin: 10px;
    font-size: 1rem;
}

let level = 1;
let aliensEliminados = 0;
let coins = 0;
let diamonds = 0;

const questions = [
    { question: "¿Cuál es el color primario?", answers: ["Rojo", "Verde", "Azul"], correct: 0 },
    { question: "¿Qué forma tiene un círculo?", answers: ["Cuadrada", "Redonda", "Triangular"], correct: 1 },
    { question: "¿Qué es una tipografía sans-serif?", answers: ["Sin adornos", "Con adornos", "Con serifas"], correct: 0 },
    // Añadir más preguntas para niveles más altos
];

function startGame() {
    document.getElementById('main-menu').style.display = 'none';
    document.getElementById('game-play').style.display = 'block';
    showQuestion();
}

function showQuestion() {
    if (aliensEliminados >= 15) {
        // Si se eliminan 15 alienígenas, el jugador completa el nivel
        completeLevel();
        return;
    }

    // Mostrar pregunta
    let currentQuestion = questions[level - 1];
    document.getElementById('quiz-question').innerText = currentQuestion.question;

    // Mostrar respuestas
    let answersHtml = '';
    currentQuestion.answers.forEach((answer, index) => {
        answersHtml += `<button onclick="answerQuestion(${index})">${answer}</button>`;
    });
    document.getElementById('answers').innerHTML = answersHtml;
}

function answerQuestion(selectedAnswer) {
    const currentQuestion = questions[level - 1];
    if (selectedAnswer === currentQuestion.correct) {
        coins += 50;  // Ganar 50 monedas por eliminar un alien
        aliensEliminados++;
        document.getElementById('aliens-eliminados').innerText = aliensEliminados;
        showQuestion();  // Avanzar a la siguiente pregunta
    } else {
        alert("Respuesta incorrecta. Intenta de nuevo.");
    }
}

function completeLevel() {
    diamonds += 1;  // Ganar 1 diamante por completar el nivel
    alert("¡Nivel completado!");
    document.getElementById('next-level-btn').style.display = 'block';
}

function nextLevel() {
    if (level < 20) {
        level++;
        aliensEliminados = 0;
        document.getElementById('aliens-eliminados').innerText = aliensEliminados;
        document.getElementById('level-info').innerText = `Nivel ${level}`;
        showQuestion();
        document.getElementById('next-level-btn').style.display = 'none';
    } else {
        alert("¡Felicidades! Has completado todos los niveles.");
    }
}

function openStore() {
    document.getElementById('game-play').style.display = 'none';
    document.getElementById('store').style.display = 'block';
}

function backToGame() {
    document.getElementById('store').style.display = 'none';
    document.getElementById('game-play').style.display = 'block';
}

function buyUpgrade() {
    if (coins >= 100) {
        coins -= 100;
        alert("¡Mejora de velocidad comprada!");
    } else {
        alert("No tienes suficientes monedas.");
    }
}

function buyPowerup() {
    if (diamonds >= 5) {
        diamonds -= 5;
        alert("¡Power-up comprado!");
    } else {
        alert("No tienes suficientes diamantes.");
    }
}


