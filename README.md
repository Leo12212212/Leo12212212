// Configuración del juego
const config = {
    type: Phaser.AUTO,
    width: 800,
    height: 600,
    scene: { preload, create, update },
    physics: { default: 'arcade', arcade: { gravity: { y: 0 }, debug: false } }
};

const game = new Phaser.Game(config);
let player, aliens, score = 0, coins = 0, diamonds = 0;

function preload() {
    this.load.image('background', 'assets/background.png');
    this.load.image('player', 'assets/player.png');
    this.load.image('alien', 'assets/alien.png');
}

function create() {
    this.add.image(400, 300, 'background');
    player = this.physics.add.sprite(400, 500, 'player').setCollideWorldBounds(true);
    aliens = this.physics.add.group();
    
    for (let i = 0; i < 15; i++) {
        let x = Phaser.Math.Between(50, 750);
        let y = Phaser.Math.Between(50, 300);
        let alien = aliens.create(x, y, 'alien');
    }
}

function update() {
    // Lógica de movimiento del jugador y colisiones
}

function showQuestion(alien) {
    let question = getQuestion();
    let userAnswer = prompt(question.text);
    if (userAnswer.toLowerCase() === question.answer.toLowerCase()) {
        alien.destroy();
        score++;
        coins += 50;
        if (score % 15 === 0) diamonds++;
    }
}

function getQuestion() {
    let questions = [
        { text: '¿Qué es el color primario?', answer: 'Rojo' },
        { text: '¿Cuántos lados tiene un triángulo?', answer: '3' }
    ];
    return questions[Phaser.Math.Between(0, questions.length - 1)];
}
