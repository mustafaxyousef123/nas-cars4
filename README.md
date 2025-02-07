<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Car Drift Game</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background: linear-gradient(135deg, #1e3c72, #2a5298, #76b852, #8dc26f);
            background-size: 400% 400%;
            animation: gradientBackground 15s ease infinite;
        }

        @keyframes gradientBackground {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
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
    </style>
</head>
<body>
    <div id="gameMenu">
        <h1>3D Car Drift Game</h1>
        <button id="startGame">Start Game</button>
    </div>
    <script>
        let scene, camera, renderer, car, track;
        let keys = { ArrowUp: false, ArrowDown: false, ArrowLeft: false, ArrowRight: false };

        function init() {
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            renderer = new THREE.WebGLRenderer();
            renderer.setSize(window.innerWidth, window.innerHeight);
            document.body.appendChild(renderer.domElement);

            // Lighting
            const light = new THREE.DirectionalLight(0xffffff, 1);
            light.position.set(5, 10, 5);
            scene.add(light);

            // Track (Simple Plane)
            const trackGeometry = new THREE.PlaneGeometry(20, 40);
            const trackMaterial = new THREE.MeshStandardMaterial({ color: 0x444444 });
            track = new THREE.Mesh(trackGeometry, trackMaterial);
            track.rotation.x = -Math.PI / 2;
            scene.add(track);

            // Car (Basic Box)
            const carGeometry = new THREE.BoxGeometry(1, 0.5, 2);
            const carMaterial = new THREE.MeshStandardMaterial({ color: 0xff0000 });
            car = new THREE.Mesh(carGeometry, carMaterial);
            car.position.y = 0.25;
            scene.add(car);

            camera.position.set(0, 3, 5);
            camera.lookAt(car.position);

            document.addEventListener("keydown", (e) => { if (keys.hasOwnProperty(e.key)) keys[e.key] = true; });
            document.addEventListener("keyup", (e) => { if (keys.hasOwnProperty(e.key)) keys[e.key] = false; });
            animate();
        }

        function animate() {
            requestAnimationFrame(animate);

            if (keys.ArrowUp) car.position.z -= 0.1;
            if (keys.ArrowDown) car.position.z += 0.1;
            if (keys.ArrowLeft) car.rotation.y += 0.05;
            if (keys.ArrowRight) car.rotation.y -= 0.05;

            camera.position.x = car.position.x;
            camera.position.z = car.position.z + 5;
            camera.lookAt(car.position);

            renderer.render(scene, camera);
        }

        document.getElementById("startGame").addEventListener("click", () => {
            document.getElementById("gameMenu").style.display = "none";
            init();
        });
    </script>
</body>
</html>
nas-cars4
