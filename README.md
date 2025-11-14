<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FALCON HUB - Game Library</title>
    <link rel="icon" type="image/x-icon" href="https://cdn.iconscout.com/icon/free/png-512/free-google-drive-logo-icon-download-in-svg-png-gif-file-formats--workspace-logos-icons-2416659.png?f=webp&w=256">
    <style>
        body {
            font-family: sans-serif;
            margin: 0;
            padding: 0;
            background-color: #121212;
            color: #e0e0e0;
            overflow: hidden;
        }

        #rain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .rain-drop {
            position: absolute;
            background-color: rgba(255, 255, 255, 0.5);
            width: 1px;
            height: 10px;
            border-radius: 2px;
            animation: fall linear infinite;
        }

        @keyframes fall {
            to {
                transform: translateY(100vh);
            }
        }

        nav {
            background-color: #1e1e1e;
            overflow: hidden;
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 1px solid #333;
        }

        nav a {
            float: left;
            display: block;
            color: #e0e0e0;
            text-align: center;
            padding: 14px 16px;
            text-decoration: none;
            transition: background-color 0.3s, color 0.3s;
        }

        nav a:hover {
            background-color: #333;
            color: #fff;
        }

        #homeScreen, #gameList, #aboutSection, #gameDisplay {
            padding: 50px;
            text-align: center;
            display: none;
        }

        .gameItem {
            border: 1px solid #333;
            margin: 10px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            width: 200px;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
            display: inline-block;
            background-color: #222;
            border-radius: 5px;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .gameItem:hover {
            transform: translateY(-5px);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.4);
        }

        #gameDisplay iframe {
            width: 100%;
            height: 75vh; /* Using viewport height for better responsiveness */
            border: none;
            background-color: #121212;
        }

        button {
            background-color: #555;
            color: #e0e0e0;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #777;
        }

        #loadingIndicator {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 20px;
            border-radius: 5px;
        }
    </style>
</head>
<body>

<div id="rain"></div>

<nav>
    <a href="#home" onclick="showSection('homeScreen')" aria-label="Home">Home</a>
    <a href="#games" onclick="showSection('gameList')" aria-label="Games">Games</a>
    <a href="#about" onclick="showSection('aboutSection')" aria-label="About">About</a>
</nav>

<div id="homeScreen">
    <h1>FALCON HUB</h1>
    <p>Explore a variety of exciting games!</p>
    <button onclick="showSection('gameList')">View Games</button>
</div>

<div id="gameList"></div>

<div id="gameDisplay">
    <div id="gameContent"></div>
    <button onclick="hideGame()">Back to List</button>
    <div>

    </div>
</div>

<div id="aboutSection">
    <h2>About This Site</h2>
    <p>This is a simple game library created to display some fun web games.</p>
</div>

<div id="loadingIndicator">Loading...</div>

<script>
    const homeScreen = document.getElementById('homeScreen');
    const gameList = document.getElementById('gameList');
    const gameDisplay = document.getElementById('gameDisplay');
    const gameContent = document.getElementById('gameContent');
    const aboutSection = document.getElementById('aboutSection');
    const rainContainer = document.getElementById('rain');
    const loadingIndicator = document.getElementById('loadingIndicator');

    // Variable storing game names
    const gameNames = {
        game1: 'Drift boss',
        game2: 'Snow rider',
        game3: 'Slope',
        game4: 'Roof top snipers',
        game5: 'Granny',
        game6: 'Granny 2',
        game7: 'Polly track',
        game8: '1v1.lol',
        game9: 'bitlife',
        game10: 'basket random',
        game11: 'gta 2',
        game12: 'volly random',
        game13: '2048',
        game14: 'angry bird',
        game15: 'basket legends',
        game16: 'boxing random',
        game17: 'cluster rush',
        game18: 'cookie clicker',
        game19: 'fnaf',
        game20: 'football legends'
    };

    // Games object using gameNames variable
    const games = {
        game1: { title: gameNames.game1, url: 'game1.html' },
        game2: { title: gameNames.game2, url: 'game2.html' },
        game3: { title: gameNames.game3, url: 'game3.html' },
        game4: { title: gameNames.game4, url: 'game4.html' },
        game5: { title: gameNames.game5, url: 'game5.html' },
        game6: { title: gameNames.game6, url: 'game6.html' },
        game7: { title: gameNames.game7, url: 'game7.html' },
        game8: { title: gameNames.game8, url: 'game8.html' },
        game9: { title: gameNames.game9, url: 'game9.html' },
        game10: { title: gameNames.game10, url: 'game10.html' },
        game11: { title: gameNames.game11, url: 'game11.html' },
        game12: { title: gameNames.game12, url: 'game12.html' },
        game13: { title: gameNames.game13, url: 'game13.html' },
        game14: { title: gameNames.game14, url: 'game14.html' },
        game15: { title: gameNames.game15, url: 'game15.html' },
        game16: { title: gameNames.game16, url: 'game16.html' },
        game17: { title: gameNames.game17, url: 'game17.html' },
        game18: { title: gameNames.game18, url: 'game18.html' },
        game19: { title: gameNames.game19, url: 'game19.html' },
        game20: { title: gameNames.game20, url: 'game20.html' }
    };

    function createRain() {
        for (let i = 0; i < 100; i++) {
            const drop = document.createElement('div');
            drop.classList.add('rain-drop');
            drop.style.left = `${Math.random() * 100}vw`;
            drop.style.animationDuration = `${Math.random() * 2 + 1}s`;
            drop.style.animationDelay = `${Math.random() * -5}s`;
            rainContainer.appendChild(drop);
        }
    }

    createRain();

    function showSection(sectionId) {
        homeScreen.style.display = 'none';
        gameList.style.display = 'none';
        gameDisplay.style.display = 'none';
        aboutSection.style.display = 'none';

        document.getElementById(sectionId).style.display = 'block';

        if (sectionId === 'gameList') {
            if (gameList.innerHTML === "") {
                populateGameList();
            }
        }
    }

    function loadGame(gameId) {
        const game = games[gameId];
        if (game) {
            loadingIndicator.style.display = 'block';
            gameContent.innerHTML = `<iframe src="${game.url}" style="width:100%;height:75vh;border:none;" onload="hideLoading()" onerror="handleGameLoadError()"></iframe>`;
            gameDisplay.style.display = 'block';
            gameList.style.display = 'none';
        }
    }

    function hideLoading() {
        loadingIndicator.style.display = 'none';
    }

    function handleGameLoadError() {
        loadingIndicator.style.display = 'none';
        gameContent.innerHTML = `<p style="color: red;">Failed to load game. Please try again later.</p>`;
    }

    gameList.addEventListener('click', (event) => {
        if (event.target.classList.contains('gameItem')) {
            const gameId = event.target.dataset.game;
            loadGame(gameId);
        }
    });

    function hideGame() {
        gameDisplay.style.display = 'none';
        gameList.style.display = 'block';
    }

    // Dynamically populate game list
    function populateGameList() {
        gameList.innerHTML = '';
        for (const gameId in gameNames) {
            const gameDiv = document.createElement('div');
            gameDiv.classList.add('gameItem');
            gameDiv.dataset.game = gameId;
            gameDiv.textContent = gameNames[gameId];
            gameList.appendChild(gameDiv);
        }
    }

    showSection('homeScreen');
</script>

</body>
</html>
