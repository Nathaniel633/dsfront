---
layout: post
toc: true
title: FITNESS ML
description: fitness machine learning 
courses: { csp: {week: 25} }
type: ccc
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calorie Burn Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            color: #333;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 600px;
            margin: 50px auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #8a2be2; /* Purple */
        }
        label {
            font-weight: bold;
        }
        input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        #calculate-calories {
            display: block;
            width: 50%; /* Less width */
            margin: 0 auto; /* Center align */
            padding: 10px;
            background-color: #8a2be2; /* Purple */
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #calculate-calories:hover {
            background-color: #6a1a9a; /* Darker purple */
        }
        #message {
            margin-top: 20px;
            text-align: center;
            font-weight: bold;
            color: #8a2be2; /* Purple */
        }
        #fitness-message {
            text-align: center;
            font-style: italic;
            color: #8a2be2; /* Purple */
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Calorie Burn Calculator</h1>
    <form>
        <label for="duration">Duration (minutes):</label>
        <input type="number" id="duration" placeholder="Enter duration in minutes (e.g., 30)" required>
        <label for="bpm">BPM (Beats per Minute):</label>
        <input type="number" id="bpm" placeholder="Enter BPM (e.g., 150)" required>
        <label for="intensity">Intensity (1-10):</label>
        <input type="number" id="intensity" min="1" max="10" placeholder="Enter intensity level (e.g., 7)" required>
        <button type="button" id="calculate-calories" onclick="calculateCalories()">Calculate Calories</button>
    </form>
    <div id="message"></div>
    <div id="fitness-message"></div>
</div>

<script>
    function calculateCalories() {
        var duration = document.getElementById('duration').value;
        var bpm = document.getElementById('bpm').value;
        var intensity = document.getElementById('intensity').value;

        // Prepare the data to send to the backend API
        var data = {
            "Duration": parseInt(duration),
            "BPM": parseInt(bpm),
            "Intensity": parseInt(intensity)
        };

        // Make a POST request to the backend API endpoint
        fetch('http://127.0.0.1:8086/api/fitness/predict', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        })
        .then(response => response.json())
        .then(data => {
            // Display the predicted calorie burn from the backend response
            var predictedCalories = data.predicted_calories.toFixed(2);
            document.getElementById('message').innerText = "Predicted Calorie Burn: " + predictedCalories;

            // Display a random motivational message
            var messages = [
                "Keep up the great work!",
                "Exercise is key to a healthy lifestyle.",
                "You're one step closer to your fitness goals!",
                "Don't stop now, you're doing amazing!",
                "Every step counts towards a healthier you!",
                "Your brain burns 400-500 calories per day!",

                "Exercise improves mood: Regular physical activity can help boost your mood and reduce feelings of anxiety and depression. Exercise stimulates the production of endorphins, which are natural mood lifters, leading to a sense of well-being and happiness.",

                "Exercise strengthens bones and muscles: Weight-bearing exercises, such as walking, jogging, and weightlifting, help build and maintain bone density, reducing the risk of osteoporosis and fractures. Additionally, resistance training strengthens muscles, improving balance and reducing the risk of falls, especially in older adults.",

                "Exercise improves sleep quality: Engaging in regular physical activity can help regulate sleep patterns and improve sleep quality. Exercise increases the production of serotonin, a neurotransmitter that promotes relaxation and helps regulate sleep-wake cycles, leading to better sleep.",

                "Exercise boosts cognitive function: Physical activity has been shown to enhance cognitive function and protect against age-related cognitive decline. Regular exercise increases blood flow to the brain, promotes the growth of new brain cells, and improves memory, concentration, and problem-solving skills.",

                "Exercise reduces the risk of chronic diseases: Regular exercise plays a crucial role in preventing and managing chronic diseases, such as heart disease, diabetes, and cancer. Physical activity helps control weight, lower blood pressure and cholesterol levels, improve blood sugar regulation, and reduce inflammation, all of which contribute to better overall health and reduced disease risk."

            ];
            var randomMessage = messages[Math.floor(Math.random() * messages.length)];
            document.getElementById('fitness-message').innerText = randomMessage;
        })
        .catch(error => {
            console.error('Error:', error);
            // Display an error message if something goes wrong
            document.getElementById('message').innerText = "Error: Failed to calculate calories. Please try again later.";
        });
    }
</script>

</body>
</html>
