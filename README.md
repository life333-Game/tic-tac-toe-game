<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tic Tac Toe</title>
  <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Press Start 2P', cursive;
      background-color: #111;
      color: #0f0;
      text-align: center;
      padding: 20px;
    }
    h1 {
      font-size: 18px;
      margin-bottom: 20px;
    }
    #modeSelect {
      margin-bottom: 20px;
    }
    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      gap: 5px;
      justify-content: center;
      margin: 0 auto;
    }
    .cell {
      background-color: #222;
      border: 2px solid #0f0;
      width: 100px;
      height: 100px;
      font-size: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
    }
    .cell:hover {
      background-color: #333;
    }
    .status {
      margin-top: 20px;
    }
    button {
      font-family: 'Press Start 2P', cursive;
      background: #0f0;
      color: #111;
      padding: 10px;
      border: none;
      margin-top: 20px;
      cursor: pointer;
    }
    @media (max-width: 400px) {
      .board {
        grid-template-columns: repeat(3, 80px);
      }
      .cell {
        width: 80px;
        height: 80px;
        font-size: 24px;
      }
    }
  </style>
</head>
<body>
  <h1>TIC TAC TOE</h1>
  <div id="modeSelect">
    <label><input type="radio" name="mode" value="two" checked> 2 Player</label>
    <label><input type="radio" name="mode" value="single"> Single Player</label>
  </div>
  <div class="board" id="board"></div>
  <div class="status" id="status"></div>
  <button onclick="resetGame()">Restart</button>

  <script>
    const boardElement = document.getElementById('board');
    const statusElement = document.getElementById('status');
    let board = Array(9).fill(null);
    let currentPlayer = 'X';
    let gameActive = true;
    let singlePlayer = false;

    function checkWinner(b) {
      const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6]
      ];
      for (const [a,b1,c] of winPatterns) {
        if (board[a] && board[a] === board[b1] && board[a] === board[c]) {
          return board[a];
        }
      }
      return board.includes(null) ? null : 'Draw';
    }

    function makeAIMove() {
      const emptyIndices = board.map((val, i) => val === null ? i : null).filter(i => i !== null);
      const move = emptyIndices[Math.floor(Math.random() * emptyIndices.length)];
      board[move] = 'O';
      render();
    }

    function handleClick(i) {
      if (!gameActive || board[i]) return;
      board[i] = currentPlayer;
      render();
      const result = checkWinner(board);
      if (result) {
        gameActive = false;
        statusElement.textContent = result === 'Draw' ? "It's a Draw!" : `${result} Wins!`;
        return;
      }
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      if (singlePlayer && currentPlayer === 'O') {
        setTimeout(() => {
          makeAIMove();
          const result = checkWinner(board);
          if (result) {
            gameActive = false;
            statusElement.textContent = result === 'Draw' ? "It's a Draw!" : `${result} Wins!`;
            return;
          }
          currentPlayer = 'X';
        }, 300);
      }
    }

    function render() {
      boardElement.innerHTML = '';
      board.forEach((cell, i) => {
        const div = document.createElement('div');
        div.className = 'cell';
        div.textContent = cell || '';
        div.onclick = () => handleClick(i);
        boardElement.appendChild(div);
      });
    }

    function resetGame() {
      board = Array(9).fill(null);
      currentPlayer = 'X';
      gameActive = true;
      statusElement.textContent = '';
      render();
    }

    document.querySelectorAll('input[name="mode"]').forEach(input => {
      input.addEventListener('change', () => {
        singlePlayer = input.value === 'single';
        resetGame();
      });
    });

    render();
  </script>
</body>
</html>
