<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            background: linear-gradient(135deg, #1e3c72, #2a5298, #76b852, #8dc26f);
            background-size: 400% 400%;
            animation: gradientBackground 15s ease infinite;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        @keyframes gradientBackground {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        canvas {
            display: block;
            background: #333;
            border: 2px solid #fff;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }

        #gameMenu {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-family: Arial, sans-serif;
            font-size: 24px;
            z-index: 10;
        }

        #gameMenu button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 18px;
            background: #555;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s;
        }

        #gameMenu button:hover {
            background: #777;
        }

        #gameInfo {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-family: Arial, sans-serif;
            font-size: 16px;
            z-index: 1;
        }
    </style>
</head>
<body>
    <div id="gameMenu">
        <h1>Car Drift Game</h1>
        <button id="startGame">Start Game</button>
        <button id="instructions">Instructions</button>
    </div>

    <canvas id="gameCanvas" width="800" height="600" style="display: none;"></canvas>
    <div id="gameInfo" style="display: none;">Use arrow keys to control the car! Drive and drift around the track.</div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const gameMenu = document.getElementById("gameMenu");
        const gameInfo = document.getElementById("gameInfo");
        const startGameButton = document.getElementById("startGame");
        const instructionsButton = document.getElementById("instructions");

        // Game constants
        const trackColor = "#555";
        const trackMargin = 50;
        const carWidth = 40;
        const carHeight = 20;
        const maxSpeed = 7;
        const driftFactor = 0.9;
        const friction = 0.05;

        // Track layout
        const track = [
            { x: 100, y: 100, width: 600, height: 400 },
            { x: 150, y: 150, width: 500, height: 300 },
        ];

        // Car state
        let car = {
            x: canvas.width / 2,
            y: canvas.height - 100,
            angle: 0,
            speed: 0,
        };

        // Controls
        const keys = {
            ArrowUp: false,
            ArrowDown: false,
            ArrowLeft: false,
            ArrowRight: false,
        };

        // Event listeners for key presses
        document.addEventListener("keydown", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = true;
        });

        document.addEventListener("keyup", (e) => {
            if (keys.hasOwnProperty(e.key)) keys[e.key] = false;
        });

        startGameButton.addEventListener("click", () => {
            gameMenu.style.display = "none";
            canvas.style.display = "block";
            gameInfo.style.display = "block";
            gameLoop();
        });

        instructionsButton.addEventListener("click", () => {
            alert("Instructions:\n\nUse the arrow keys to control the car:\n- Up Arrow: Accelerate\n- Down Arrow: Brake/Reverse\n- Left Arrow: Turn Left\n- Right Arrow: Turn Right\n\nDrive and drift around the track! Stay on the road!");
        });

        function updateCar() {
            // Accelerate or decelerate
            if (keys.ArrowUp) car.speed += 0.2;
            if (keys.ArrowDown) car.speed -= 0.2;

            // Apply friction
            car.speed *= 1 - friction;

            // Limit speed
            car.speed = Math.min(maxSpeed, Math.max(-maxSpeed, car.speed));

            // Turn car
            if (keys.ArrowLeft) car.angle -= 0.04 * (car.speed / maxSpeed);
            if (keys.ArrowRight) car.angle += 0.04 * (car.speed / maxSpeed);

            // Simulate drifting
            car.x += Math.sin(car.angle) * car.speed * driftFactor;
            car.y -= Math.cos(car.angle) * car.speed;

            // Keep car within canvas boundaries
            if (car.x < 0) car.x = 0;
            if (car.x > canvas.width) car.x = canvas.width;
            if (car.y < 0) car.y = 0;
            if (car.y > canvas.height) car.y = canvas.height;
        }

        function drawTrack() {
            // Draw track layers
            track.forEach((section, index) => {
                ctx.fillStyle = index % 2 === 0 ? "#777" : "#444";
                ctx.fillRect(section.x, section.y, section.width, section.height);
            });

            // Draw track boundary
            ctx.strokeStyle = "#fff";
            ctx.lineWidth = 2;
            ctx.strokeRect(track[0].x, track[0].y, track[0].width, track[0].height);
        }

        function drawCar() {
            ctx.save();
            ctx.translate(car.x, car.y);
            ctx.rotate(car.angle);
            ctx.fillStyle = "red";
            ctx.fillRect(-carWidth / 2, -carHeight / 2, carWidth, carHeight);
            ctx.fillStyle = "black";
            ctx.fillRect(-carWidth / 2 + 5, -carHeight / 2 + 5, carWidth - 10, carHeight - 10);
            ctx.restore();
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawTrack();
            updateCar();
            drawCar();
            requestAnimationFrame(gameLoop);
        }
    </script>
</body>
</html>
nas-car4
