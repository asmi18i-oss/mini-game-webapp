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
