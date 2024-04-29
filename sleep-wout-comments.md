<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sleep Data Management</title>
    <style>
        body {
            margin: 0 20px;
            background-color: #fce4ec;
            font-family: Arial, sans-serif;
        }
        form {
            margin-bottom: 20px;
        }
        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
        }
        input[type="number"],
        input[type="text"],
        select {
            width: calc(100% - 20px);
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        input[type="number"],
        input[type="text"] {
            width: calc(100% - 20px);
            max-width: 150px;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        select {
            width: calc(100% - 20px);
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
        }
        button {
            background-color: #ff4081;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        table {
            width: calc(100% - 40px);
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #ef85a8;
            font-size: 14px;
            white-space: nowrap;
        }
        .edit-delete-buttons {
            margin-top: 10px;
        }
        .edit-delete-buttons button {
            background-color: #e91e63;
            color: white;
            padding: 5px 10px;
            margin: 2px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h3>Input Hours of Sleep</h3>
    <form id="formToGetOneSleepDetail">
        <label for="Sleep"><b>How many hours of sleep did you get?:</b></label>
        <input type="number" id="Sleep" name="Sleep" min="0" />
        <button type="submit" value="btnToGetSleepDetail" id="get_sleepy">See Sleep Details</button>
        <div id="getOneSleepDetailResponse"></div>
        <div id="sleepFacts"></div>
    </form> 
    <h3>All Sleep Data</h3>
    <table id="SleepTable">
        <thead>
            <tr>
                <th>Person ID</th>
                <th>Gender</th>
                <th>Age</th>
                <th>Occupation</th>
                <th>Sleep Duration (hours)</th>
                <th>Quality of Sleep</th>
                <th>Physical Activity Level</th>
                <th>Stress Level</th>
                <th>BMI Category</th>
                <th>Blood Pressure</th>
                <th>Heart Rate</th>
                <th>Daily Steps</th>
                <th>Sleep Disorder</th>
            </tr>
        </thead>
        <tbody id="sleepDataBody"></tbody>
    </table>

    <div class="edit-delete-buttons">
        <button>Edit</button>
        <button>Delete</button>
    </div>
</body>
</html>

<script type="module">
    function displaySleepFacts(hoursSlept) {
        const factsContainer = document.getElementById("sleepFacts");
        factsContainer.innerHTML = "";
        let fact = "";
        if (hoursSlept < 7) {
            const tooLittleFacts = [
                "Lack of sleep can impair cognitive function.",
                "Chronic sleep deprivation may increase the risk of heart disease.",
                "Sleep deprivation can lead to mood swings and irritability.",
                "Insufficient sleep has been linked to an increased risk of obesity and metabolic disorders due to disruptions in hormone regulation.",
                "Prolonged sleep deprivation may elevate stress levels and contribute to the development of mental health disorders such as anxiety and depression."
            ];
            fact = tooLittleFacts[Math.floor(Math.random() * tooLittleFacts.length)];
        } else if (hoursSlept >= 7 && hoursSlept <= 9) {
            const goodScheduleFacts = [
                "Consistent sleep schedule helps regulate your body's internal clock.",
                "Quality sleep improves memory and cognitive function.",
                "Avoid caffeine and heavy meals before bedtime for better sleep.",
                "Maintaining a consistent sleep schedule of 7-9 hours per night helps regulate the body's internal clock, promoting overall well-being and alertness.",
                "Maintaining a healthy sleep duration of 7-9 hours each night is associated with a lower risk of obesity and metabolic disorders, as proper sleep contributes to hormonal balance and appetite regulation."
            ];
            fact = goodScheduleFacts[Math.floor(Math.random() * goodScheduleFacts.length)];
        } else if (hoursSlept > 9) {
            const tooMuchFacts = [
                "Oversleeping may increase the risk of obesity and diabetes.",
                "Excessive sleep can lead to daytime drowsiness and lethargy.",
                "Long sleep duration may be a sign of underlying health issues.",
                "Hey! WAKE UP!"
            ];
            fact = tooMuchFacts[Math.floor(Math.random() * tooMuchFacts.length)];
        }
        factsContainer.innerHTML = `<h4>Sleep Fact:</h4><p>${fact}</p>`;
    }

    document.addEventListener("DOMContentLoaded", function() {
        const API_BASE_URL = 'http://127.0.0.1:8080';
        const API_URL = API_BASE_URL + '/api/sleeps/';
        loadAllSleepData();
        const getSleepButton = document.getElementById("get_sleepy");
        getSleepButton.addEventListener("click", getOneSleep);

        function loadAllSleepData() {
            fetch(API_URL, { method: 'GET' })
            .then(response => response.json())
            .then(data => {
                displayItems(data);
                console.log("Response from getOneSleep function:", data);
            })
            .catch(error => {
                console.error("Error calling get all sleep data:", error);
            });
        }

        function displayItems(items) {
            const table = document.getElementById("SleepTable");
            const tableBody = table.querySelector("tbody");
            tableBody.innerHTML = "";
            items.forEach((sleep) => {
                const row = tableBody.insertRow();
                row.setAttribute("data-id", sleep.id);
                const dataMapping = {
                    "Person ID": "id",
                    "Gender": "gender",
                    "Age": "age",
                    "Occupation": "occupation",
                    "Sleep Duration": "sleep_duration",
                    "Quality of Sleep": "quality_of_sleep",
                    "Physical Activity Level": "physical_activity_level",
                    "Stress Level": "stress_level",
                    "BMI Category": "bmi_category",
                    "Blood Pressure": "blood_pressure",
                    "Heart Rate": "heart_rate",
                    "Daily Steps": "daily_steps",
                    "Sleep Disorder": "sleep_disorder"
                };
                for (const category in dataMapping) {
                    if (dataMapping.hasOwnProperty(category)) {
                        const cell = row.insertCell();
                        const value = sleep[dataMapping[category]];
                        if (category === "Sleep Duration") {
                            cell.innerText = `${value} hours`;
                        } else {
                            cell.innerText = value;
                        }
                    }
                }
            });
        }

        function getOneSleep(event) {
            event.preventDefault();
            const form = document.getElementById('formToGetOneSleepDetail');
            const sleepInput = form.elements['Sleep'];
            const sleepDuration = parseFloat(sleepInput.value);
            if (!isNaN(sleepDuration)) {
                console.log("Sleep duration:", sleepDuration);
                displaySleepFacts(sleepDuration);
                fetch(`${API_BASE_URL}/api/sleeps?sleep_duration=${sleepDuration}`, {
                    method: "GET"
                })
                .then(response => {
                    if (response.ok) {
                        return response.json();
                    } else {
                        console.error("Server error:", response.status);
                        throw new Error("Server error");
                    }
                })
                .then(data => {
                    const table = document.getElementById("SleepTable");
                    const tableBody = table.querySelector("tbody");
                    tableBody.innerHTML = "";
                    displayItems(data);
                })
                .catch(error => {
                    console.error("Error:", error);
                });
            } else {
                console.error("Invalid input for sleep duration");
            }
        }
    });
</script>
