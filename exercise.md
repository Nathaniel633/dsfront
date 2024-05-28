---
layout: post
toc: true
title: Exercise
description:  
permalink: /exercise
type: ccc
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Exercise Cards</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }

        .controls {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
        }

        .controls input, .controls select {
            padding: 10px;
            font-size: 16px;
        }

        .container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
        }

        .card {
            background-color: #fff;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .card h3 {
            margin-top: 0;
        }

        .card button {
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 10px;
            cursor: pointer;
        }

        .card button:hover {
            background-color: #218838;
        }

        @media (max-width: 900px) {
            .container {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        @media (max-width: 600px) {
            .container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="controls">
        <input type="text" id="search-input" placeholder="Search exercises..." oninput="filterExercises()">
        <select id="sort-select" onchange="sortExercises()">
            <option value="name">Sort by Name</option>
            <option value="intensity">Sort by Intensity</option>
            <option value="calories_burned">Sort by Calories Burned</option>
            <option value="likes">Sort by Likes</option>
        </select>
    </div>
    <div class="container" id="exercise-container">
        <!-- Cards will be dynamically inserted here -->
    </div>

    <script>
        let exercises = [];

        document.addEventListener('DOMContentLoaded', () => {
            fetchExercises();
        });

        function fetchExercises() {
            fetch('http://localhost:5000/api/exercise/records')
                .then(response => response.json())
                .then(data => {
                    exercises = data;
                    displayExercises(exercises);
                })
                .catch(error => console.error('Error fetching exercises:', error));
        }

        function displayExercises(exercisesToDisplay) {
            const container = document.getElementById('exercise-container');
            container.innerHTML = '';

            exercisesToDisplay.forEach(exercise => {
                const card = document.createElement('div');
                card.classList.add('card');

                card.innerHTML = `
                    <h3>${exercise.name}</h3>
                    <p>Intensity: ${exercise.intensity}</p>
                    <p>Type: ${exercise.type}</p>
                    <p>Calories Burned: ${exercise.calories_burned}</p>
                    <p>Likes: <span id="likes-${exercise.id}">${exercise.likes}</span></p>
                    <button onclick="likeExercise(${exercise.id})">Like</button>
                `;

                container.appendChild(card);
            });
        }

        function likeExercise(exerciseId) {
            fetch(`http://localhost:5000/api/exercise/like/${exerciseId}`, {
                method: 'POST'
            })
            .then(response => response.json())
            .then(data => {
                if (data.message === 'Exercise liked successfully') {
                    const exercise = exercises.find(ex => ex.id === exerciseId);
                    if (exercise) {
                        exercise.likes += 1;
                        displayExercises(exercises);
                    }
                }
            })
            .catch(error => console.error('Error liking exercise:', error));
        }

        function filterExercises() {
            const searchInput = document.getElementById('search-input').value.toLowerCase();
            const filteredExercises = exercises.filter(exercise =>
                exercise.name.toLowerCase().includes(searchInput) ||
                exercise.intensity.toLowerCase().includes(searchInput) ||
                exercise.type.toLowerCase().includes(searchInput)
            );
            displayExercises(filteredExercises);
        }

        function sortExercises() {
            const sortBy = document.getElementById('sort-select').value;
            const sortedExercises = [...exercises].sort((a, b) => {
                if (sortBy === 'name' || sortBy === 'intensity' || sortBy === 'type') {
                    return a[sortBy].localeCompare(b[sortBy]);
                } else {
                    return b[sortBy] - a[sortBy];
                }
            });
            displayExercises(sortedExercises);
        }
    </script>
</body>
</html>
