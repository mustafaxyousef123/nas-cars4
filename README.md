<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Drift Game</title>
    <style>
        body {
            margin: 0;
            background: #222;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        canvas {
            display: block;
            background: #333;
            border: 2px solid #fff;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }

        #gameInfo {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-family: Arial, sans-serif;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <div id="gameInfo">Use arrow keys to control the car! Drive and drift around the track.</div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        // Game constants
        const trackColor = "#555";
        const trackMargin = 50;
        const carWidth = 40;
        const carHeight = 20;
        const maxSpeed = 7;
        const driftFactor = 0.9;
        const friction = 0.05;

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

            // Keep car within track boundaries
            if (car.x < trackMargin) car.x = trackMargin;
            if (car.x > canvas.width - trackMargin) car.x = canvas.width - trackMargin;
            if (car.y < trackMargin) car.y = trackMargin;
            if (car.y > canvas.height - trackMargin) car.y = canvas.height - trackMargin;
        }

        function drawTrack() {
            ctx.fillStyle = trackColor;
            ctx.fillRect(trackMargin, trackMargin, canvas.width - trackMargin * 2, canvas.height - trackMargin * 2);

            // Draw track boundary
            ctx.strokeStyle = "#fff";
            ctx.lineWidth = 2;
            ctx.strokeRect(trackMargin, trackMargin, canvas.width - trackMargin * 2, canvas.height - trackMargin * 2);
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

        gameLoop();
    </script>
</body>
</html>
# nas-cars4
