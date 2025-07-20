# PRODIGY_WD_03
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe Game</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: linear-gradient(135deg, rgb(113, 28, 241), rgba(225, 13, 225, 0.693));
            font-family: Arial, sans-serif;
        }

        h1 {
            font-size: 2.5em;
            color: #333;
            margin-bottom: 20px;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 5px;
        }

        .cell {
            width: 100px;
            height: 100px;
            border: 2px solid #555;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5em;
            font-weight: bold;
            cursor: pointer;
            background-color: #ffffff;
            transition: background-color 0.3s;
        }

        .cell:hover {
            background-color: #f0f0f0;
        }

        .cell.disabled {
            cursor: not-allowed;
            opacity: 0.6;
        }

        #message {
            font-size: 1.5em;
            margin-top: 20px;
            color: #fff;
            font-weight: bold;
        }

        #resetButton {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1.2em;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        #resetButton:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Tic-Tac-Toe</h1>
    <div class="board" id="board"></div>
    <div id="message">Player X's turn</div>
    <button id="resetButton">Reset Game</button>

    <script>
        const board = document.getElementById('board');
        const cells = [];
        const message = document.getElementById('message');
        const resetButton = document.getElementById('resetButton');

        let currentPlayer = 'X';
        let gameBoard = ['', '', '', '', '', '', '', '', ''];
        let gameActive = true;

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

        // Create the game board
        function createBoard() {
            board.innerHTML = '';
            cells.length = 0;
            
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.dataset.index = i;
                cell.addEventListener('click', handleCellClick);
                board.appendChild(cell);
                cells.push(cell);
            }
        }

        function handleCellClick(e) {
            const cell = e.target;
            const index = cell.dataset.index;

            if (gameBoard[index] === '' && gameActive) {
                gameBoard[index] = currentPlayer;
                cell.textContent = currentPlayer;
                cell.classList.add('disabled');

                if (checkWin()) {
                    message.textContent = Player ${currentPlayer} wins!;
                    gameActive = false;
                    disableAllCells();
                } else if (checkDraw()) {
                    message.textContent = "It's a draw!";
                    gameActive = false;
                } else {
                    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                    message.textContent = Player ${currentPlayer}'s turn;
                }
            }
        }

        function checkWin() {
            return winningCombinations.some(combination => {
                return combination.every(index => {
                    return gameBoard[index] === currentPlayer;
                });
            });
        }

        function checkDraw() {
            return gameBoard.every(cell => cell !== '');
        }

        function disableAllCells() {
            cells.forEach(cell => {
                cell.classList.add('disabled');
            });
        }

        function resetGame() {
            currentPlayer = 'X';
            gameBoard = ['', '', '', '', '', '', '', '', ''];
            gameActive = true;
            message.textContent = Player ${currentPlayer}'s turn;
            
            cells.forEach(cell => {
                cell.textContent = '';
                cell.classList.remove('disabled');
            });
        }

        // Initialize the game
        createBoard();
        resetButton.addEventListener('click', resetGame);
    </script>
</body>
</html>
