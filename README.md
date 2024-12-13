# TIC-TAC-TOE



HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <main>
        <h1>TIC TAC TOE</h1>
        <div class="container">
            <div class="game">
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
                <button class="box"></button>
            </div>
        </div>
        <button id="reset-button">RESET GAME</button>

    </main>
    <script src="app.js"></script>
</body>
</html>


CSS

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: rgb(162, 215, 228);
    text-align: center;
    font-family: Arial, sans-serif; 
}

.container {
    height: 70vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.game {
    height: 60vmin;
    width: 60vmin;
    display: flex;
    flex-wrap: wrap; 
    justify-content: center;
    align-items: center;
    gap: 1.5vmin;
}

.box {
    height: 18vmin;
    width: 18vmin;
    border-radius: 1rem;
    border: none;
    box-shadow: 0 0 1rem rgba(0, 0, 0, 0.1);
    font-size: 8vmin;
    color: rgb(111, 7, 7);
    display: flex; 
    justify-content: center; 
    align-items: center; 
}

#reset-button {
    padding: 1rem;
    font-size: 1.25rem;
    background-color: rgb(250, 32, 228);
    color: aliceblue;
    border-radius: 1rem;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s ease; 
}

#reset-button:hover {
    background-color: rgb(200, 20, 180); 
}

JAVASCRIPT

const boxes = document.querySelectorAll('.box');
const resetButton = document.getElementById('reset-button');
let currentPlayer = 'X';
let gameActive = true;
let board = ['', '', '', '', '', '', '', '', ''];

const winningCombinations = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];

function checkWinner() {
  for (const combination of winningCombinations) {
    const [a, b, c] = combination;
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      gameActive = false;
      highlightWinner(combination);
      return board[a];
    }
  }
  if (!board.includes('')) {
    gameActive = false;
    return 'Draw';
  }
  return null;
}

function highlightWinner(combination) {
  combination.forEach(index => {
    boxes[index].style.backgroundColor = 'lightgreen';
  });
}

function resetGame() {
  board = ['', '', '', '', '', '', '', '', ''];
  gameActive = true;
  currentPlayer = 'X';
  boxes.forEach(box => {
    box.textContent = '';
    box.style.backgroundColor = '';
  });
}

function aiMove() {
  let available = board.map((val, idx) => (val === '' ? idx : null)).filter(val => val !== null);
  let move = available[Math.floor(Math.random() * available.length)];
  board[move] = 'O';
  boxes[move].textContent = 'O';
  currentPlayer = 'X';
}

function handleBoxClick(e) {
  const index = Array.from(boxes).indexOf(e.target);
  if (board[index] || !gameActive) return;

  board[index] = currentPlayer;
  e.target.textContent = currentPlayer;

  let winner = checkWinner();
  if (winner) {
    if (winner === 'Draw') alert('Game is a draw!');
    else alert(`${winner} wins!`);
    return;
  }

  currentPlayer = currentPlayer === 'X' ? 'O' : 'X';

  if (currentPlayer === 'O' && gameActive) {
    setTimeout(aiMove, 500);
    setTimeout(() => {
      let winner = checkWinner();
      if (winner) {
        if (winner === 'Draw') alert('Game is a draw!');
        else alert(`${winner} wins!`);
      }
    }, 600);
  }
}

boxes.forEach(box => box.addEventListener('click', handleBoxClick));
resetButton.addEventListener('click', resetGame);



