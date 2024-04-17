---
permalink: /losescreen
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lose Screen</title>
    <style>
        body {
            display: flex;
            flex-direction: column; /* Adjusted to a column layout */
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .lose-message {
            text-align: center;
            padding: 20px;
            background-color: red;
            color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px; /* Added margin to create spacing */
        }
        .try-again-button {
            text-align: center;
        }
        .try-again-button button {
            background-color: #4caf50;
            color: #fff;
            padding: 10px 20px;
            border: 2px solid #4caf50; /* Added border property */
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="lose-message">
        <h1>Defeat!</h1>
        <p>You've lost the game!</p>
    </div>
    <div class="try-again-button">
        <a href="{{site.baseurl}}/main_menu">
            <button>Try Again!</button>
        </a>
    </div>
</body>
</html>