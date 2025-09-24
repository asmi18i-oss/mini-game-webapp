# mini-game-webapp
“Test your speed &amp; reflexes! ⏱️ Click as fast as you can in 10 seconds, unlock new levels, and compete on the online leaderboard 🏆” 
index.html<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Click Challenge Game</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="game-container">
    <h1>Click Challenge Game 🎯</h1>
    <p>Click the button as many times as you can in <span id="time">10</span> seconds!</p>
    
    <input type="text" id="playerName" placeholder="Enter your name">
    <button id="clickButton">Click Me!</button>
    <p>Score: <span id="score">0</span></p>
    <p>High Score: <span id="highScore">0</span></p>
    <p>Level: <span id="level">1</span></p>
    
    <button id="startButton">Start Game</button>
    <button id="resetButton">Reset Game</button>

    <h2>Online Leaderboard 🏆</h2>
    <ol id="leaderboard"></ol>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
  <script src="script.js"></script>
</body>
</html>
style.css
body {
  font-family: 'Arial', sans-serif;
  background: linear-gradient(to right, #43cea2, #185a9d);
  color: #fff;
  text-align: center;
  margin: 0;
  padding: 50px;
}

.game-container {
  background: rgba(0,0,0,0.3);
  padding: 30px;
  border-radius: 15px;
  display: inline-block;
}

button, input {
  padding: 15px 25px;
  margin: 10px;
  border-radius: 10px;
  border: none;
  cursor: pointer;
  font-size: 18px;
  transition: 0.3s;
}

#clickButton {
  background-color: #28a745;
  color: #fff;
}

#clickButton:hover {
  background-color: #3eda63;
}

#startButton {
  background-color: #007bff;
  color: #fff;
}

#startButton:hover {
  background-color: #339aff;
}

#resetButton {
  background-color: #dc3545;
  color: #fff;
}

#resetButton:hover {
  background-color: #e95c67;
}

#score, #highScore, #time, #level {
  font-weight: bold;
  font-size: 24px;
}

#leaderboard {
  margin-top: 20px;
  text-align: left;
  display: inline-block;
}// Firebase config (replace with your Firebase project config)
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT-default-rtdb.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

// Game variables
let score = 0;
let highScore = 0;
let level = 1;
let timeLeft = 10;
let gameRunning = false;
let timer;

// DOM elements
const playerNameInput = document.getElementById('playerName');
const clickButton = document.getElementById('clickButton');
const startButton = document.getElementById('startButton');
const resetButton = document.getElementById('resetButton');
const scoreDisplay = document.getElementById('score');
const highScoreDisplay = document.getElementById('highScore');
const timeDisplay = document.getElementById('time');
const levelDisplay = document.getElementById('level');
const leaderboardEl = document.getElementById('leaderboard');

// Sounds (optional - upload sounds folder if you want)
const clickSound = new Audio('sounds/click.mp3');
const levelUpSound = new Audio('sounds/levelup.mp3');

clickButton.disabled = true;

// Start game
startButton.addEventListener('click', () => {
  const playerName = playerNameInput.value.trim();
  if(!playerName) { alert("Enter your name!"); return; }

  if(!gameRunning) {
    score = 0;
    level = 1;
    timeLeft = 10;
    scoreDisplay.textContent = score;
    levelDisplay.textContent = level;
    timeDisplay.textContent = timeLeft;
    gameRunning = true;
    clickButton.disabled = false;

    timer = setInterval(() => {
      timeLeft--;
      timeDisplay.textContent = timeLeft;
      if(timeLeft <= 0) endGame(playerName);
    }, 1000);
  }
});

// Click button
clickButton.addEventListener('click', () => {
  if(gameRunning) {
    score++;
    scoreDisplay.textContent = score;
    clickSound.currentTime = 0;
    clickSound.play();

    if(score % 5 === 0) {
      level++;
      levelDisplay.textContent = level;
      levelUpSound.currentTime = 0;
      levelUpSound.play();
    }
  }
});

// Reset button
resetButton.addEventListener('click', () => {
  clearInterval(timer);
  score = 0;
  level = 1;
  timeLeft = 10;
  gameRunning = false;
  scoreDisplay.textContent = score;
  levelDisplay.textContent = level;
  timeDisplay.textContent = timeLeft;
  clickButton.disabled = true;
});

// End game and save to Firebase
function endGame(playerName) {
  clearInterval(timer);
  gameRunning = false;
  clickButton.disabled = true;

  if(score > highScore) {
    highScore = score;
    highScoreDisplay.textContent = highScore;
  }

  alert(`Time's up! Your Score: ${score}`);

  const newScoreRef = db.ref('leaderboard').push();
  newScoreRef.set({
    name: playerName,
    score: score
  });
}

// Listen for leaderboard updates
db.ref('leaderboard').orderByChild('score').limitToLast(5).on('value', snapshot => {
  const data = snapshot.val();
  const scores = [];
  for(let key in data) scores.push(data[key]);
  scores.sort((a,b) => b.score - a.score);

  leaderboardEl.innerHTML = '';
  scores.forEach((entry, i) => {
    const li = document.createElement('li');
    li.textContent = `${entry.name} - ${entry.score} points`;
    leaderboardEl.appendChild(li);
  });
});
script.js
