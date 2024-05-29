---
layout: post
title: Game Screen
hide: true
description: Game screen page for JWT authentication
permalink: /gamescreen
---

<html lang="en">
<head>
<meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        .black-box {
            background-color: black;
            color: white;
            padding: 100px;
            border-radius: 10px;
            border: 2px solid white;
            text-align: left;
            width: 880px;
        }
        .white-input {
            background-color: white;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            width: 30%;
            box-sizing: border-box; /* Include padding and border in the width calculation */  
        }
        #map {  /* Targeting the map image specifically */
        width: 100%;  /* Set width to 100% of its container */
        height: auto; /* Set height to auto to maintain aspect ratio */
        max-width: 800px; /* Optional: you can set a max-width if needed */
        display: block; /* Ensures it takes up its own line */
        margin: 0 auto; /* Centers the image in the container */
        }
    </style>
</head>
<select id="className" name="className" onchange="resetMap()">
        <option value="">Pick a class</option>
        <option value="Knight">Knight</option>
        <option value="Mage">Mage</option>
        <option value="Rogue">Rogue</option>
        <option value="Shield Bearer">Shield Bearer</option>
        <option value="Grand Wizard">Grand Wizard</option>
    </select>
<body>
    <img id="map" src="https://i.postimg.cc/x1YqnQJZ/MapPos1.jpg" alt="Game Screen" usemap="#gameMap">
    <div class="black-box">
        <!-- Ex: You are currently at point 1 on the map. You can move to the points 2, 3. Please input the number of the point you would like to move to. -->
        <!-- "1", "2, 3" needs to change + "move" should be able to change to "move or attack" + add text saying there's an enemy 1 step away from you -->
        <h2 id="boxtext">You are currently at point <span id="currentposition"></span> on the map. <span id="enemyalert"></span>You can <span id="actions"></span> the points <span id="possibleactionpositions"></span>. Please input the number of the point you would like to act on in the respective action box.</h2>
        <input type="number" min="1" max="9" class="white-input" placeholder="Type here..." id="playerinputmove">
        <button onclick="movement()">Move</button><br>
        <input type="number" min="1" max="9" class="white-input" placeholder="Type here..." id="playerinputattack">
        <button onclick="attack()">Attack</button>
    </div>
</body>

<script>
    // Define function to calculate damage
    async function calculateDamage() {
      var dataHealth = 0;
      // Fetch data to get HP
      const url = "http://{{site.baseurl}}/api/currentchar/";
      var options = {
          method: 'GET', // *GET, POST, PUT, DELETE, etc.
          mode: 'cors', // no-cors, *cors, same-origin
          cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
          credentials: 'include', // include, same-origin, omit
          headers: {
              'Content-Type': 'application/json',
          },
      };
      const response = await fetch(url, options);
      const data = await response.json();
      console.log(data);

      var dataObject = data[0];
      console.log(dataObject);

      dataHealth = dataObject["health"];
      console.log("EEE " + dataHealth);

      var newhealth = dataHealth - 1;
      console.log(newhealth)

      dataObject["health"] = newhealth;
      console.log(dataObject)

      // Send PUT request
      body = {
          // name: document.getElementById("name").value,
          classname: dataObject["className"],
          health: newhealth,
          attack: dataObject["attack"],
          range: dataObject["range"],
          movement: dataObject["movement"]
      };
      var AuthOptions = {
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
              // window.location.href='{{site.baseurl}}/gamescreen'
              console.log("Data updated")
          })
      })
      // catch fetch errors (ie ACCESS to server blocked)
      .catch(err => {
      console.log(err)
      });
      
      // Check if health is 0
      if (newhealth == 0) {
        window.location.href = '{{site.baseurl}}/losescreen';
      }
    };
    
    // Define variable for initial position and update it in text
    var position = 1;
    document.getElementById("currentposition").textContent = position;

    // Define object for the map images (MapPos1 means at point 1)
    var mapImages = {
        1: "https://i.postimg.cc/x1YqnQJZ/MapPos1.jpg",
        2: "https://i.postimg.cc/GmQtpfvm/MapPos2.jpg",
        3: "https://i.postimg.cc/SK1K6zT0/MapPos3.jpg",
        4: "https://i.postimg.cc/SKxsYGG7/MapPos4.jpg",
        5: "https://i.postimg.cc/ZYdqNn6g/MapPos5.jpg",
        6: "https://i.postimg.cc/rsxF2Z8c/MapPos6.jpg",
        7: "https://i.postimg.cc/RFjV8xxX/MapPos7.jpg",
        8: "https://i.postimg.cc/637pY5g8/MapPos8.jpg",
        9: "https://i.postimg.cc/dtcVjFB0/MapPos9.jpg"
    };

    var imagesKnight = {
      1: "https://i.postimg.cc/nhvSQRPr/gameboard-Knight1.png", 
      2: "https://i.postimg.cc/JnBpMpZR/gameboard-Knight2.png",
      3: "https://i.postimg.cc/tgn3xYT5/gameboard-Knight3.png",
      4: "https://i.postimg.cc/njVMrBpc/gameboard-Knight4.png",
      5: "https://i.postimg.cc/23ZNv3yz/gameboard-Knight5.png",
      6: "https://i.postimg.cc/dLcP9vZG/gameboard-Knight6.png",
      7: "https://i.postimg.cc/HrTqf22x/gameboard-Knight7.png",
      8: "https://i.postimg.cc/mPVvRHXS/gameboard-Knight8.png",
      9: "https://i.postimg.cc/94MvfQsH/gameboard-Knight9.png"
    };

    var imagesMage = {
      1: "https://i.postimg.cc/w1CgnJ7b/gameboard-Mage1.png",
      2: "https://i.postimg.cc/rd5MTm4y/gameboard-Mage2.png",
      3: "https://i.postimg.cc/tZyy4jd3/gameboard-Mage3.png",
      4: "https://i.postimg.cc/p5LHKJ6y/gameboard-Mage4.png",
      5: "https://i.postimg.cc/8JHGF0wq/gameboard-Mage5.png",
      6: "https://i.postimg.cc/s1jsF3gy/gameboard-Mage6.png",
      7: "https://i.postimg.cc/vD0MbC4n/gameboard-Mage7.png",
      8: "https://i.postimg.cc/fV9h7LB7/gameboard-Mage8.png",
      9: "https://i.postimg.cc/Y4NKVPch/gameboard-Mage9.png"
    };

    var imagesRogue = {
      1: "https://i.postimg.cc/8JTGwHFB/gameboard-Rogue1.png",
      2: "https://i.postimg.cc/zH39PPqW/gameboard-Rogue2.png",
      3: "https://i.postimg.cc/4K5q4TdV/gameboard-Rogue3.png",
      4: "https://i.postimg.cc/dhcx80ZG/gameboard-Rogue4.png",
      5: "https://i.postimg.cc/hfTCPqZv/gameboard-Rogue5.png",
      6: "https://i.postimg.cc/SnmGSYz1/gameboard-Rogue6.png",
      7: "https://i.postimg.cc/CRMjTyWW/gameboard-Rogue7.png",
      8: "https://i.postimg.cc/NL1TKft3/gameboard-Rogue8.png",
      9: "https://i.postimg.cc/KKM7Q7Xz/gameboard-Rogue9.png"
    };

    var imagesShield = {
      1: "https://i.postimg.cc/nCVwcW7t/gameboard-Shield1.png",
      2: "https://i.postimg.cc/ctxTCKvc/gameboard-Shield2.png",
      3: "https://i.postimg.cc/ftBmH8bx/gameboard-Shield3.png",
      4: "https://i.postimg.cc/phZYw2s5/gameboard-Shield4.png",
      5: "https://i.postimg.cc/Yvz9fh0F/gameboard-Shield5.png",
      6: "https://i.postimg.cc/tspRTpk4/gameboard-Shield6.png",
      7: "https://i.postimg.cc/rdbLSk1W/gameboard-Shield7.png",
      8: "https://i.postimg.cc/YhkBN8WX/gameboard-Shield8.png",
      9: "https://i.postimg.cc/F1vhG9tg/gameboard-Shield9.png"
    };

    var imagesWizard = {
      1: "https://i.postimg.cc/RNRQs1PC/gameboard-Wizard1.png",
      2: "https://i.postimg.cc/FY6PpgRd/gameboard-Wizard2.png",
      3: "https://i.postimg.cc/qhTxDmdR/gameboard-Wizard3.png",
      4: "https://i.postimg.cc/ykkcjqSj/gameboard-Wizard4.png",
      5: "https://i.postimg.cc/6yMw9SHk/gameboard-Wizard5.png",
      6: "https://i.postimg.cc/mcM0F6w2/gameboard-Wizard6.png",
      7: "https://i.postimg.cc/PpX2r6V7/gameboard-Wizard7.png",
      8: "https://i.postimg.cc/VS1FDGwN/gameboard-Wizard8.png",
      9: "https://i.postimg.cc/TKjVLVbv/gameboard-Wizard9.png"
    }
    
    // Define object for possible actions (movement and attack) depending on initial position and update it in text
    var possibleActionPositions = {
        1: [2, 3],
        2: [1, 3, 4, 5],
        3: [1, 2, 5, 6],
        4: [2, 5, 7],
        5: [2, 3, 4, 6, 7, 8],
        6: [3, 5, 8],
        7: [4, 5, 8, 9],
        8: [5, 6, 7, 9],
        9: [7, 8]
    }
    document.getElementById("possibleactionpositions").textContent = possibleActionPositions[position];

    // Define function to check if a number is in one of the possible actions
    var enemyposition = 9;
    var enemyspot = possibleActionPositions[enemyposition]
    function checkPosition(number, arr) {
      for (var i = 0; i < arr.length; i++) {
        if (arr[i] == number) {
          document.getElementById("enemyalert").textContent = "The enemy is one step away from you! ";
          document.getElementById("actions").textContent = "move to or attack";
          return;
        }
        else {
          document.getElementById("enemyalert").textContent = "";
          document.getElementById("actions").textContent = "move to";
        }
      }
    };
    checkPosition(position, enemyspot);

    // Function to pick a random number from 1 to max, and this is how to decide what the enemy is going to do and where
    function enemychoice(max) {
      var randomnumber = Math.random();
      var scalednumber = randomnumber * max;
      var endnumber = Math.floor(scalednumber) + 1;
      return endnumber;
    };

    // Function to pick a random number from an inputted array
    function enemychoice2(array) {
      const randomIndex = Math.floor(Math.random() * array.length);
      var moveposition = array[randomIndex];
      return moveposition;
    };

    // Define function for enemy movement
    function enemymove() {
      var moveposition = enemychoice2(enemyspot);
      while (moveposition == position) {
        moveposition = enemychoice2(enemyspot);
      };
      enemyposition = moveposition;
      enemyspot = possibleActionPositions[enemyposition]
      return;
    };
        
    // Define function for AI attack
    function enemyattack() {
      var attackposition = enemychoice2(enemyspot);
      if (attackposition == position) {
        calculateDamage();
      }
    };

    // Define function for player movement
    function resetMap() {
      var className = document.getElementById("className").value; // Get the selected class
      var map = document.getElementById("map"); // Reference to the map image element

      switch (className) {
          case "Knight":
              map.src = imagesKnight[position];
              break;
          case "Mage":
              map.src = imagesMage[position];
              break;
          case "Rogue":
              map.src = imagesRogue[position];
              break;
          case "Shield Bearer":
              map.src = imagesShield[position];
              break;
          case "Grand Wizard":
              map.src = imagesWizard[position];
              break;
          default:
              map.src = mapImages[position]; // Fallback to default map images if no class is selected
              break;
      }
}


    function movement() {
      // Set inputValue to the number entered
      inputmovement = document.getElementById("playerinputmove");
      inputValue = inputmovement.value;
      possiblemoves = possibleActionPositions[position];
      for (var i = 0; i < possiblemoves.length; i++) {
        if (possiblemoves[i] == inputValue) {
          // Set number entered as new postion and update text for current position, the possible places to act on, and map
          position = inputValue
          document.getElementById("currentposition").textContent = position;
          document.getElementById("possibleactionpositions").textContent = possibleActionPositions[position];
          var map = document.getElementById("map")
          if (className = "Knight") {
            map.src = imagesKnight[position]
          }
          if (className = "Mage") {
            map.src = imagesMage[position]
          }
          if (className = "Rogue") {
            map.src = imagesRogue[position]
          }
          if (className = "Shield Bearer") {
            map.src = imagesShield[position]
          }
          if (className = "Grand Wizard") {
            map.src = imagesWizard[position]
          }
          else {
            map.src = mapImages[position]
          }

          // Enemy act
          var choice = enemychoice(2)
          if (choice == 1) {
            enemymove();
            console.log("Enemy has moved to " + enemyposition)
            alert("Enemy has moved")
          }
          else if (choice == 2) {
            enemyattack();
            console.log("Enemy has attacked")
            alert("Enemy has attacked")
          }
          // Check positions and give alert if conditions met
          checkPosition(position, enemyspot)
          return;
        }
      }
      alert("Invalid number!")
    }

    // Define function for player attack
    function attack() {
      inputattack = document.getElementById("playerinputattack");
      inputattackValue = inputattack.value;
      possiblemoves = possibleActionPositions[position];
      for (var i = 0; i < possiblemoves.length; i++) {
        if (possiblemoves[i] == inputattackValue) {
          // Check if hit or not
          if (possiblemoves[i] == enemyposition) {
            alert("You Hit!")
            window.location.href='{{site.baseurl}}/winscreen'
            return;
          }
          else {
            alert("You Missed!")
          }
          // Enemy act
          var choice = enemychoice(2)
          if (choice == 1) {
            enemymove();
            console.log("Enemy has moved")
            alert("Enemy has moved")
          }
          else if (choice == 2) {
            enemyattack();
            console.log("Enemy has attacked")
            alert("Enemy has attacked")
          }
        }
      }
      alert("Invalid number!")
    }
</script>
