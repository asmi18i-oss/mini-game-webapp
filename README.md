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
}
