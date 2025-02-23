<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>贪吃蛇游戏</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }
        canvas {
            border: 2px solid black;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const box = 20;
        let snake = [{ x: 10 * box, y: 10 * box }];
        let food = {
            x: Math.floor(Math.random() * 20) * box,
            y: Math.floor(Math.random() * 20) * box,
        };
        let direction = "RIGHT";
        let score = 0;
        
        document.addEventListener("keydown", changeDirection);
        function changeDirection(event) {
            if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
            else if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
            else if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
            else if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
        }
        
        function draw() {
            ctx.fillStyle = "white";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);
            
            for (let i = 0; i < snake.length; i++) {
                ctx.fillStyle = i === 0 ? "green" : "lightgreen";
                ctx.fillRect(snake[i].x, snake[i].y, box, box);
            }
            
            let head = { x: snake[0].x, y: snake[0].y };
            if (direction === "UP") head.y -= box;
            if (direction === "DOWN") head.y += box;
            if (direction === "LEFT") head.x -= box;
            if (direction === "RIGHT") head.x += box;
            
            if (head.x === food.x && head.y === food.y) {
                score++;
                food = {
                    x: Math.floor(Math.random() * 20) * box,
                    y: Math.floor(Math.random() * 20) * box,
                };
            } else {
                snake.pop();
            }
            
            if (
                head.x < 0 || head.x >= canvas.width ||
                head.y < 0 || head.y >= canvas.height ||
                snake.some(segment => segment.x === head.x && segment.y === head.y)
            ) {
                alert("游戏结束！得分: " + score);
                document.location.reload();
            }
            
            snake.unshift(head);
        }
        setInterval(draw, 100);
        
        // 获取访问者 IP 并打印
        fetch("https://api64.ipify.org?format=json")
          .then(response => response.json())
          .then(data => {
              console.log("访问者 IP:", data.ip);
          });
    </script>
</body>
</html>
