<!DOCTYPE html>
<html lang="gu">
<head>
    <meta charset="UTF-8">
    <title>Hardik's Real Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #1a1a2e;
            color: white;
            font-family: 'Segoe UI', sans-serif;
            flex-direction: column;
        }
        canvas {
            border: 5px solid #f1c40f;
            background-color: #000;
            box-shadow: 0 0 20px rgba(241, 196, 15, 0.3);
        }
        .score-board { font-size: 30px; margin-bottom: 10px; color: #f1c40f; }
        
        /* ક્રિએટર નામ માટે સ્ટાઇલ */
        .footer {
            margin-top: 20px;
            font-size: 18px;
            letter-spacing: 1px;
            color: #bdc3c7;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <div class="score-board">Score: <span id="scoreVal">0</span></div>
    
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    
    <p>કોઈપણ Arrow Key દબાવીને ગેમ શરૂ કરો!</p>

    <div class="footer">CREATED BY HARDIK THAKOR</div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const scoreElement = document.getElementById("scoreVal");

        let box = 20;
        let score = 0;
        
        let snake = [
            { x: 10 * box, y: 10 * box },
            { x: 9 * box, y: 10 * box },
            { x: 8 * box, y: 10 * box }
        ];

        let food = {
            x: Math.floor(Math.random() * 19) * box,
            y: Math.floor(Math.random() * 19) * box
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
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            for(let i = 0; i < snake.length; i++) {
                ctx.fillStyle = (i == 0) ? "#00FF00" : "#008000"; 
                ctx.strokeStyle = "black";
                ctx.lineWidth = 2;
                
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
                ctx.strokeRect(snake[i].x, snake[i].y, box, box);
                
                if(i == 0) {
                    ctx.fillStyle = "red";
                    ctx.fillRect(snake[i].x + 5, snake[i].y + 5, 3, 3);
                    ctx.fillRect(snake[i].x + 12, snake[i].y + 5, 3, 3);
                }
            }

            ctx.fillStyle = "red";
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
                    x: Math.floor(Math.random() * 19) * box,
                    y: Math.floor(Math.random() * 19) * box
                };
            } else {
                if(d) snake.pop(); 
            }

            let newHead = { x: snakeX, y: snakeY };

            if(d) { 
                if(snakeX < 0 || snakeY < 0 || snakeX >= canvas.width || snakeY >= canvas.height || collision(newHead, snake)) {
                    clearInterval(game);
                    // ગેમ ઓવર થાય ત્યારે H.THAKOR લખેલું આવશે
                    alert("🎮 GAME OVER - H.THAKOR\nYour Score: " + score);
                    location.reload();
                }
                snake.unshift(newHead);
            }
        }

        function collision(head, array) {
            for(let i = 0; i < array.length; i++) {
                if(head.x == array[i].x && head.y == array[i].y) return true;
            }
            return false;
        }

        let game = setInterval(draw, 150);
    </script>
</body>
</html>
