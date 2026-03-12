<!DOCTYPE html>
<html>

<head>
    <title>Geometry Dash</title>

    <style>
        body {
            margin: 0;
            overflow: hidden;
            font-family: Arial;
            background: #111;
            color: white;
            text-align: center;
        }

        canvas {
            display: block;
        }

        .menu {
            position: absolute;
            top: 35%;
            width: 100%;
        }

        button {
            padding: 18px 40px;
            font-size: 22px;
            margin: 8px;
            cursor: pointer;
        }

        #percent {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 30px;
        }

        #home {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 18px;
            padding: 8px 18px;
        }
    </style>

</head>

<body>

    <div id="menu" class="menu">
        <h1 style="font-size:60px">Geometry Dash</h1>
        <button onclick="showLevels()">Play</button>
    </div>

    <div id="levels" class="menu" style="display:none">
        <h2>Select Level</h2>
        <button onclick="startLevel(1)">Level 1</button>
        <button onclick="startLevel(2)">Level 2</button>
        <button onclick="startLevel(3)">Level 3</button>
        <button onclick="startLevel(4)">Level 4</button>
        <button onclick="startLevel(5)">Level 5</button>
        <br><br>
        <button onclick="goMenu()">Back</button>
    </div>

    <div id="percent" style="display:none">0%</div>
    <button id="home" onclick="goMenu()" style="display:none">Home</button>

    <canvas id="game"></canvas>

    <script>

        const canvas = document.getElementById("game");
        const ctx = canvas.getContext("2d");

        function resize() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        resize();
        window.addEventListener("resize", resize);

        let player = {
            x: 200,
            y: 0,
            size: 50,
            vy: 0,
            rotation: 0,
            jumping: false,
            mode: "cube",
            onSurface: false
        };

        let gravity = 1.1;
        let jumpForce = -18;
        let speed = 8.5;

        let camera = 0;
        let running = false;
        let levelLength = 3500;

        let spikes = [];
        let pads = [];
        let portals = [];
        let blocks = [];

        let ground = 120;
        let currentLevel = 1;

        let holding = false;

        document.addEventListener("keydown", e => {
            if (e.code === "Space") holding = true;
        });

        document.addEventListener("keyup", e => {
            if (e.code === "Space") holding = false;
        });

        document.addEventListener("mousedown", () => holding = true);
        document.addEventListener("mouseup", () => holding = false);

        function showLevels() {
            menu.style.display = "none";
            levels.style.display = "block";
        }

        function goMenu() {

            running = false;

            menu.style.display = "block";
            levels.style.display = "none";
            percent.style.display = "none";
            home.style.display = "none";

        }

        function startLevel(level) {

            currentLevel = level;

            menu.style.display = "none";
            levels.style.display = "none";

            percent.style.display = "block";
            home.style.display = "block";

            player.y = canvas.height - ground - player.size;
            player.vy = 0;
            player.rotation = 0;
            player.mode = "cube";

            camera = 0;

            createLevel(level);

            running = true;

            requestAnimationFrame(loop);

        }

        function restart() {

            player.y = canvas.height - ground - player.size;
            player.vy = 0;
            player.rotation = 0;
            player.mode = "cube";

            camera = 0;

            createLevel(currentLevel);

            running = true;

            requestAnimationFrame(loop);

        }

        function createLevel(level) {

            spikes = [];
            pads = [];
            portals = [];
            blocks = [];

            if (level === 1) {

                spikes = [
                    { x: 450, y: 0 },
                    { x: 500, y: 0 },
                    { x: 550, y: 0 },
                    { x: 450, y: 150 },
                    { x: 500, y: 150 },
                    { x: 550, y: 150 },
                    { x: 1000, y: 0 },
                    { x: 1170, y: 60 },
                    { x: 1240, y: 0 },
                    { x: 1300, y: 0 },
                    { x: 1360, y: 0 },
                    { x: 1670, y: 0 },
                    { x: 1720, y: 0 },
                    { x: 1770, y: 0 },
                    { x: 2000, y: 190 },
                    { x: 2150, y: 100 },
                    { x: 1960, y: 45 },
                    { x: 1880, y: 0 },
                    { x: 2200, y: 60 },
                    { x: 2200, y: 240 },
                    { x: 2260, y: 60 },
                    { x: 2260, y: 240 },
                    { x: 2320, y: 10 },
                    { x: 2320, y: 240 },
                    { x: 2380, y: 70 },
                    { x: 2520, y: 170 },




                ];
                pads = [
                    { x: 1210, y: 120 }
                ];
                portals = [
                    { x: 300, type: "ship" },
                    { x: 650, type: "cube" },
                    { x: 1840, type: "ship" },
                ];
                blocks = [
                    { x: 1060, size: 60, yOffset: 0 },
                    { x: 1210, size: 60, yOffset: 60 },
                    { x: 1180, size: 60, yOffset: 0 },
                    { x: 1120, size: 60, yOffset: 0 },
                    { x: 1420, size: 60, yOffset: 0 },
                    { x: 2000, size: 100, yOffset: 0 },
                    { x: 2000, size: 100, yOffset: 250 },
                    { x: 2100, size: 100, yOffset: 250 },
                    { x: 2100, size: 100, yOffset: 0 },
                    { x: 2400, size: 100, yOffset: 0 },
                    { x: 2400, size: 100, yOffset: 200 },
                    { x: 2500, size: 100, yOffset: 200 },
                    { x: 2500, size: 100, yOffset: 0 },
                ];

            }

            if (level === 2) {

                spikes = [
                    { x: 800, y: 0 },
                    { x: 1200, y: 0 }
                ];
                pads = [
                    { x: 1000, y: 0 }
                ];
                blocks = [
                    { x: 700, size: 60, yOffset: 80 },
                    { x: 1400, size: 60, yOffset: 140 }
                ];

            }

            if (level === 3) {

                spikes = [
                    { x: 900, y: 0 },
                    { x: 1300, y: 0 }
                ];
                pads = [
                    { x: 1000, y: 0 }
                ];
                blocks = [
                    { x: 650, size: 60, yOffset: 60 },
                    { x: 1250, size: 60, yOffset: 140 },
                    { x: 1550, size: 60, yOffset: 100 }
                ];

            }

            if (level === 4) {

                spikes = [
                    { x: 900, y: 0 },
                    { x: 1200, y: 0 }
                ];
                pads = [
                    { x: 1000, y: 0 }
                ];
                portals = [{ x: 1600, type: "ship" }];
                blocks = [
                    { x: 700, size: 60, yOffset: 80 },
                    { x: 1300, size: 60, yOffset: 120 }
                ];

            }

            if (level === 5) {

                spikes = [
                    { x: 900, y: 0 },
                    { x: 1200, y: 0 },
                    { x: 1500, y: 0 }
                ];
                pads = [
                    { x: 1000, y: 0 }
                ];
                portals = [
                    { x: 2300, type: "ship" }
                ];
                blocks = [
                    { x: 800, size: 60, yOffset: 60 },
                    { x: 1700, size: 60, yOffset: 140 },
                    { x: 2000, size: 60, yOffset: 100 }
                ];

            }

        }

        function die() {
            running = false;

            setTimeout(restart, 700);

        }

        function update() {

            camera += speed;

            let groundY = canvas.height - ground;
            let playerWorldX = player.x + camera;
            let prevY = player.y;

            if (player.mode === "cube") {

                if (holding && player.onSurface) {
                    player.vy = jumpForce;
                    player.jumping = true;
                }

                player.onSurface = false;
                player.vy += gravity;
                player.y += player.vy;

                let onBlock = false;

                for (let block of blocks) {

                    let blockTop = groundY - block.yOffset - block.size;
                    let blockLeft = block.x;
                    let blockRight = block.x + block.size;

                    if (
                        playerWorldX + player.size > blockLeft &&
                        playerWorldX < blockRight &&
                        prevY + player.size <= blockTop &&
                        player.y + player.size >= blockTop
                    ) {
                        player.y = blockTop - player.size;
                        player.vy = 0;
                        player.jumping = false;
                        player.onSurface = true;
                        onBlock = true;
                    }

                }

                if (!onBlock && player.y > groundY - player.size) {
                    player.y = groundY - player.size;
                    player.vy = 0;
                    player.jumping = false;
                    player.onSurface = true;
                }

                if (player.jumping) {
                    player.rotation += 0.2;
                }

            }

            if (player.mode === "ship") {

                if (holding) {
                    player.vy -= 0.8;
                } else {
                    player.vy += 0.8;
                }

                player.y += player.vy;

                player.rotation = player.vy * 0.05;

                if (player.y < 0) player.y = 0;
                if (player.y > groundY - player.size) {
                    player.y = groundY - player.size;
                    player.vy = 0;
                }

            }

            for (let pad of pads) {

                let px = pad.x - camera;
                let padTop = groundY - 10 - pad.y;

                if (
                    player.x + player.size > px &&
                    player.x < px + 60 &&
                    player.y + player.size > padTop
                ) {
                    player.vy = -15;
                }

            }

            for (let portal of portals) {

                let px = portal.x - camera;

                if (player.x + player.size > px && player.x < px + 60) {

                    if (portal.type === "gravity") {
                        gravity *= -1;
                    }

                    if (portal.type === "ship") {
                        player.mode = "ship";
                        player.vy = 0;
                    }

                    if (portal.type === "cube") {
                        player.mode = "cube";
                        player.vy = 0;
                        player.rotation = 0;
                    }

                }

            }

            for (let block of blocks) {

                let blockLeft = block.x;
                let blockRight = block.x + block.size;
                let blockTop = groundY - block.yOffset - block.size;
                let blockBottom = groundY - block.yOffset;

                if (
                    playerWorldX + player.size > blockLeft &&
                    playerWorldX < blockRight &&
                    player.y + player.size > blockTop &&
                    player.y < blockBottom
                ) {
                    if (player.mode === "ship") {
                        if (player.vy >= 0) {
                            player.y = blockTop - player.size;
                            player.vy = 0;
                        } else {
                            player.y = blockBottom;
                            player.vy = 0;
                        }
                    } else {
                        die();
                    }
                }

            }

            for (let s of spikes) {

                let sx = s.x - camera;
                let spikeTop = groundY - 60 - s.y;
                let spikeBottom = groundY - s.y;

                if (
                    player.x + player.size > sx &&
                    player.x < sx + 60 &&
                    player.y + player.size > spikeTop &&
                    player.y < spikeBottom
                ) {
                    die();
                }

            }

            let p = Math.floor((camera / levelLength) * 100);
            percent.innerText = Math.min(100, p) + "%";

            if (p >= 100) {
                running = false;
                setTimeout(() => {
                    alert("Level Complete!");
                    goMenu();
                }, 400);
            }

        }

        function draw() {

            ctx.fillStyle = "#1a1a1a";
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            let groundY = canvas.height - ground;

            ctx.fillStyle = "white";
            ctx.fillRect(0, groundY, canvas.width, ground);

            ctx.fillStyle = "yellow";

            for (let pad of pads) {

                let px = pad.x - camera;
                ctx.fillRect(px, groundY - 20 - pad.y, 60, 20);

            }

            ctx.fillStyle = "#3aa3ff";

            for (let block of blocks) {

                let bx = block.x - camera;
                let by = groundY - block.yOffset - block.size;
                ctx.fillRect(bx, by, block.size, block.size);

            }

            for (let portal of portals) {

                let px = portal.x - camera;
                ctx.fillStyle = portal.type === "cube" ? "#4bdfff" : "purple";
                ctx.fillRect(px, groundY - 120, 40, 120);

            }

            ctx.fillStyle = "red";

            for (let s of spikes) {

                let sx = s.x - camera;
                let spikeTop = groundY - 60 - s.y;
                let spikeBottom = groundY - s.y;

                ctx.beginPath();
                ctx.moveTo(sx, spikeBottom);
                ctx.lineTo(sx + 60, spikeBottom);
                ctx.lineTo(sx + 30, spikeTop);
                ctx.closePath();
                ctx.fill();

            }

            ctx.save();

            ctx.translate(player.x + player.size / 2, player.y + player.size / 2);
            ctx.rotate(player.rotation);

            ctx.fillStyle = "lime";
            ctx.fillRect(-player.size / 2, -player.size / 2, player.size, player.size);

            ctx.restore();

        }

        function loop() {

            if (!running) return;

            update();
            draw();

            requestAnimationFrame(loop);

        }

    </script>

</body>

</html># geometry-dash
