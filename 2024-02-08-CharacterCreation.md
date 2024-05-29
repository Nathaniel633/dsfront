---
permalink: /charactercreation
---
<head>
    <style>
        .candle {
            max-width: 100%;
        }
        .charactercreation {
            display: flex;
            align-items: center;
        }
        .bigtitle {
            flex: 1;
            margin-right: 20px;
        }
        .mediumtitle {
            flex: 1;
            margin-right: 20px;
        }
        .smalltitle {
            flex: 1;
            margin-right: 20px;
        }
        #table {
            display: none;
        }
    </style>
</head>
<body class="charactercreation">
    <!-- Failure Screen -->
    <h1 class="bigtitle">Create Your Character</h1>
    <h2 class="middletitle">Make your character here!</h2>
    <!-- Section 1: Name and class drop down selection -->
    <h2 class="smalltitle">Name:</h2>
    <input type="text" name="name" id="name" required><br>
    <h2 class="smalltitle">Class:</h2>
    <select id="class" name="class" onchange="showinfo()">
        <option value="">Pick a class</option>
        <option value="Knight">Knight</option>
        <option value="Mage">Mage</option>
        <option value="Rogue">Rogue</option>
        <option value="Shield Bearer">Shield Bearer</option>
        <option value="Grand Wizard">Grand Wizard</option>
    </select>
    <!-- Display data and image -->
    <div id="classInfo">
        <h3 id="class-description" class="class-details"></h3>
        <div id="table" class="class-details">
            <table>
                <thead>
                <tr>
                    <th>Class</th>
                    <th>Health</th>
                    <th>Attack</th>
                    <th>Range</th>
                    <th>Movement</th>
                </tr>
                </thead>
                <tbody id="result">
                    <tr>
                        <th scope="row">Mage</th>
                        <th scope="row">1000</th>
                        <th scope="row">10000</th>
                        <th scope="row">3</th>
                        <th scope="row">7</th>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
    <!-- Submit button -->
    <br>
    <button class="buttons" onclick="submitinfo()">Submit</button>
    <img class="candle" src="https://i.postimg.cc/wj2FYHpM/candle-removebg-preview.png">
    <!-- <script type="module">
        import uri from '{{site.baseurl}}/assets/js/api/config.js';
        var uri = uri
        console.log("HI" + uri)
        // import('{{site.baseurl}}/assets/js/api/config.js')
        //     .then(module => {
        //         window.uri = module.uri;
        //     })
        //     .catch(error => console.error('Error importing config module:', error));
    </script> -->
    <script>
        var uri;
            if (location.hostname === "localhost") {
                    uri = "http://localhost:8086";
            } else if (location.hostname === "127.0.0.1") {
                    uri = "http://127.0.0.1:8086";
            } else {
                    uri = "https://wrong.nighthawkcodingsociety.com";
            }
        console.log("BYE" + uri)
        const class_descriptions = {
            Knight: "You've selected the Knight class! Strong, loyal, and determined, this class features a balance between offense and defense.",
            Mage: "You've selected the Mage class! Intelligent and calm, this class features the ability to attack any space on the map at the cost of a lower health.",
            Rogue: "You've selected the Rogue class! Cunning and quick, this class features the ability to move multiple spaces at the cost of a lower health.",
            "Shield Bearer": "You've selected the Shield Bearer class! Sturdy and unwavering, this class features extra health.",
            "Grand Wizard": "You've selected the Grand Wizard class! This class is omnipotent, essentially unable to be beaten. Use this class for testing."
        }
        // Show info and fetch data to show it too
        function showinfo() {
            var selectedclass = document.getElementById("class").value;
            // var infodiv = document.getElementById("classInfo");
            // Hide all info divs initially
            // var allinfodivs = document.querySelectorAll(".class-details");
            // allinfodivs.forEach(function(div) {
            //     div.style.display = "none";
            // });
            // // Show the selected info div
            // var selectedinfodiv = document.getElementById(selectedclass + "-details");
            // if (selectedinfodiv) {
            //     selectedinfodiv.style.display = "block";
            // }
            var table = document.getElementById("table");
            if (table) {
                table.style.display = "block";
            }
            // Fetch stuff
            const url = uri + "/api/classes/";
            // const url = "http://127.0.0.1:8086/api/classes/";
            const options = {
                method: 'GET', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'include', // include, same-origin, omit
                headers: {
                    'Content-Type': 'application/json',
                },
            };
            const resultContainer = document.getElementById("result");
            fetch(url, options)
                // response is a RESTful "promise" on any successful fetch
                .then(response => {
                // check for response errors and display
                if (response.status !== 200) {
                    const errorMsg = 'Database response error: ' + response.status;
                    console.log(errorMsg);
                    const tr = document.createElement("tr");
                    const td = document.createElement("td");
                    td.innerHTML = errorMsg;
                    tr.appendChild(td);
                    resultContainer.appendChild(tr);
                    return;
                }
                // valid response will contain JSON data
                response.json().then(data => {
                    console.log(data);
                    const just_current_class = data.filter(obj => obj.classname === selectedclass);
                    console.log(just_current_class);
                    console.log(selectedclass);
                    for (const row of just_current_class) {
                        // tr and td build out for each row
                        const tr = document.createElement("tr");
                        const classname = document.createElement("td");
                        const health = document.createElement("td");
                        const attack = document.createElement("td");
                        const range = document.createElement("td");
                        const movement = document.createElement("td");
                        // data is specific to the API
                        classname.innerHTML = row.classname;
                        console.log(classname)
                        health.innerHTML = row.health; 
                        attack.innerHTML = row.attack;
                        range.innerHTML = row.range;
                        movement.innerHTML = row.movement;
                        // this builds td's into tr
                        tr.appendChild(classname);
                        tr.appendChild(health);
                        tr.appendChild(attack);
                        tr.appendChild(range);
                        tr.appendChild(movement);
                        // Remove the old rows
                        resultContainer.innerHTML = '';
                        // Append the row to table
                        resultContainer.appendChild(tr);
                        // if (range.innerHTML = true) {
                        //     range.innerHTML = 1
                        // }
                        // else {
                        //     range.innerHTML = 0
                        // }
                        // if (movement.innerHTML = true) {
                        //     movement.innerHTML = 1
                        // }
                        // else {
                        //     movement.innerHTML = 0
                        // }
                    }
                })
                // Show the description
                const classDetailContainer = document.getElementById("class-description");
                classDetailContainer.innerHTML = class_descriptions[selectedclass];
            })
            // Catch fetch errors
            .catch(err => {
                console.error(err);
                const tr = document.createElement("tr");
                const td = document.createElement("td");
                td.innerHTML = err + ": " + url;
                tr.appendChild(td);
                resultContainer.appendChild(tr);
            });
        };
        /*
        function submitinfo() {
            window.location.href='{{site.baseurl}}/gamescreen'
            const url = uri + "/api/currentchar/";
            // const url = "http://127.0.0.1:8086/api/currentchar/";
            // get class information from table (which should be updated with the get request)
            var table = document.getElementById("result");
            var row = table.getElementsByTagName("tr");
            var cells = row[0].getElementsByTagName("td");
            const body = {
                name: document.getElementById("name").value,
                classname: cells[0].innerText,
                health: cells[1].innerText,
                attack: cells[2].innerText,
                range: cells[3].innerText,
                movement: cells[4].innerText
            };
            const AuthOptions = {
                mode: 'cors', // no-cors, *cors, same-origin
                credentials: 'include', // include, same-origin, omit
                headers: {
                    'Content-Type': 'application/json',
                },
                method: 'PUT', // Override the method property
                cache: 'no-cache', // Set the cache property
                body: JSON.stringify(body)
            };
            // fetch the API
            fetch(url, AuthOptions)
            // response is a RESTful "promise" on any successful fetch
            .then(response => {
                // check for response errors and display
                if (response.status !== 200) {
                    // window.location.href = "{{site.baseurl}}/authorizationfail"; *update with link for error
                    return;
                }
                // valid response will contain JSON data
                response.json().then(data => {
                    window.location.href='{{site.baseurl}}/gamescreen'
                })
            })
            // catch fetch errors (ie ACCESS to server blocked)
            .catch(err => {
            console.log(err)
            });
        } */
    function submitinfo() {
        var table = document.getElementById("result");
        console.log(table)
        if (!table || table.rows.length === 0) {
            console.error("Table is not available or empty.");
            return; // Exit if table is not ready or empty
        }
        var row = table.rows[0]; // Assuming you want the first row
        var cells = row.cells;
        if (cells.length < 5) {
            console.error("Not enough data in the row.");
            return; // Exit if there are not enough cells
        };
        console.log(cells[0].innerText)
        const body = {
            name: document.getElementById("name").value,
            classname: cells[0].innerText,
            health: cells[1].innerText,
            attack: cells[2].innerText,
            range: cells[3].innerText === 'true', // Assuming the API expects a boolean
            movement: cells[4].innerText === 'true'
        };
        const AuthOptions = {
            mode: 'cors',
            credentials: 'include',
            headers: {
                'Content-Type': 'application/json',
            },
            method: 'PUT',
            cache: 'no-cache',
            body: JSON.stringify(body)
        };
        // Perform the API request
        fetch(uri + "/api/currentchar/", AuthOptions)
            .then(response => {
                if (response.status !== 200) {
                    console.error("Failed to update character:", response.status);
                    return;
                }
                return response.json();
            })
            .then(data => {
                if (data) {
                    console.log("Character updated successfully", data);
                    window.location.href = '{{site.baseurl}}/gamescreen'; // Redirect on success
                }
            })
            .catch(err => {
                console.error("Error during fetch operation:", err);
            });
        };
            window.submitinfo = submitinfo;
    </script>
</body>