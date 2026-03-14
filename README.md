<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background: #0f172a; /* Koyu modern arka plan */
            font-family: 'Segoe UI', sans-serif;
            color: white;
            flex-direction: column;
        }
        canvas {
            border: 2px solid #334155;
            border-radius: 15px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.5);
            background: #1e293b;
        }
        .score {
            font-size: 24px;
            margin-bottom: 10px;
            font-weight: bold;
            color: #38bdf8;
        }
    </style>
</head>
<body>
    <div class="score">Skor: <span id="scoreVal">0</span></div>
    <canvas id="snakeGame" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById("snakeGame");
        const ctx = canvas.getContext("2d");
        const scoreElement = document.getElementById("scoreVal");

        let box = 20;
        let score = 0;
        let snake = [{x: 10 * box, y: 10 * box}];
        let food = {
            x: Math.floor(Math.random() * 19 + 1) * box,
            y: Math.floor(Math.random() * 19 + 1) * box
        };
        let d;

        document.addEventListener("keydown", direction);

        function direction(event) {
            if(event.keyCode == 37 && d != "RIGHT") d = "LEFT";
            else if(event.keyCode == 38 && d != "DOWN") d = "UP";
            else if(event.keyCode == 39 && d != "LEFT") d = "RIGHT";
            else if(event.keyCode == 40 && d != "UP") d = "DOWN";
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            for(let i = 0; i < snake.length; i++) {
                // Yılanın gövdesi için gradyan efekt
                ctx.fillStyle = (i == 0) ? "#38bdf8" : "#0ea5e9";
                ctx.beginPath();
                ctx.roundRect(snake[i].x, snake[i].y, box-2, box-2, 5);
                ctx.fill();
            }

            // Yemek (Modern bir mücevher gibi)
            ctx.fillStyle = "#fbbf24";
            ctx.beginPath();
            ctx.arc(food.x + box/2, food.y + box/2, box/2 - 2, 0, Math.PI * 2);
            ctx.fill();

            let snakeX = snake[0].x;
            let snakeY = snake[0].y;

            if( d == "LEFT") snakeX -= box;
            if( d == "UP") snakeY -= box;
            if( d == "RIGHT") snakeX += box;
            if( d == "DOWN") snakeY += box;

            if(snakeX == food.x && snakeY == food.y) {
                score++;
                scoreElement.innerHTML = score;
                food = {
                    x: Math.floor(Math.random() * 19 + 1) * box,
                    y: Math.floor(Math.random() * 19 + 1) * box
                };
            } else {
                snake.pop();
            }

            let newHead = { x: snakeX, y: snakeY };

            // Oyun bitti kontrolleri
            if(snakeX < 0 || snakeY < 0 || snakeX >= canvas.width || snakeY >= canvas.height || collision(newHead, snake)) {
                clearInterval(game);
                alert("Oyun Bitti! Skorun: " + score);
                location.reload();
            }

            snake.unshift(newHead);
        }

        function collision(head, array) {
            for(let i = 0; i < array.length; i++) {
                if(head.x == array[i].x && head.y == array[i].y) return true;
            }
            return false;
        }

        let game = setInterval(draw, 100);
    </script>
</body>
</html>
