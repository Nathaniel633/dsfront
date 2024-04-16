---
layout: home
search_exclude: true
permalink: /score
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Water Polo Score Tracker</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #006FB9;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1, h2 {
            text-align: center;
            color: #ADD8E6;
        }
        form {
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #ADD8E6;
        }
        input[type="text"],
        input[type="number"],
        button {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        button {
            background-color: #007bff;
            color: #fff;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #0056b3;
        }
        ul {
            list-style-type: none;
            padding: 0;
            color: #ADD8E6;
        }
        li {
            padding: 10px;
            border-bottom: 1px solid #ccc;
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: #ADD8E6;
        }
        li:last-child {
            border-bottom: none;
            color: #ADD8E6;
        }
        .delete-button {
            background-color: #dc3545;
            color: #fff;
            border: none;
            padding: 5px 10px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .delete-button:hover {
            background-color: #c82333;
        }
    </style>
</head>
<body>
    <h1>Water Polo Score Tracker</h1>
    <div class="container">
        <h2>Post Game Score</h2>
        <form id="post-form">
            <label for="home_team">Home Team:</label>
            <input type="text" id="home_team" name="home_team" required>
            <label for="away_team">Away Team:</label>
            <input type="text" id="away_team" name="away_team" required>
            <label for="home_score">Home Score:</label>
            <input type="number" id="home_score" name="home_score" required>
            <label for="away_score">Away Score:</label>
            <input type="number" id="away_score" name="away_score" required>
            <button type="submit">Submit</button>
        </form>
        <h2>Game Scores</h2>
        <ul id="score-list"></ul>
    </div>
    <script>
        // Function to fetch and display game scores
        function getGameScores() {
            fetch('http://127.0.0.1:8088/api/score/game_scores')
                .then(response => response.json())
                .then(data => {
                    const scoreList = document.getElementById('score-list');
                    scoreList.innerHTML = '';
                    for (const [gameId, score] of Object.entries(data)) {
                        const listItem = document.createElement('li');
                        listItem.textContent = `${score.home_team} ${score.home_score} - ${score.away_score} ${score.away_team} `;
                        const deleteButton = document.createElement('button');
                        deleteButton.textContent = 'Delete';
                        deleteButton.classList.add('delete-button');
                        deleteButton.addEventListener('click', () => deleteScore(gameId));
                        listItem.appendChild(deleteButton);
                        scoreList.appendChild(listItem);
                    }
                })
                .catch(error => console.error('Error:', error));
        }
        // Function to delete a game score
        function deleteScore(gameId) {
            fetch(`http://127.0.0.1:8088/api/score/delete_score/${gameId}`, {
                method: 'DELETE',
            })
            .then(response => {
                if (response.ok) {
                    getGameScores();
                } else {
                    console.error('Error:', response.status);
                }
            })
            .catch(error => console.error('Error:', error));
        }
        // Function to handle form submission and post game score
        document.getElementById('post-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const formData = new FormData(this);
            const data = Object.fromEntries(formData.entries());
            fetch('http://127.0.0.1:8088/api/score/post_score', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(data),
            })
            .then(response => {
                if (response.ok) {
                    return response.json();
                } else {
                    console.error('Error:', response.status);
                }
            })
            .then(data => {
                console.log('Success:', data);
                getGameScores();
                document.getElementById('post-form').reset();
            })
            .catch(error => console.error('Error:', error));
        });
        // Get initial game scores on page load
        getGame
