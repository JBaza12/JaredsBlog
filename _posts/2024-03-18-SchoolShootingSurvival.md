---
title: Titanic Simulator
layout: post
permalink: /titanic
type: tangibles
courses: { compsci: { week: 26 } }
---

<style>
        body {
            background-color: #F3F3F3;
            font-family: Arial, sans-serif;
        }
        h1 {
            color: #FFFFFF;
            text-transform: uppercase;
            margin-top: 20px;
        }
        form {
            background-color: #FFFFFF;
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #3A3A3A;
        }
        input[type="text"],
        input[type="number"],
        select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45A049;
        }
        #result {
            margin-top: 20px;
            background-color: #FFFFFF;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        #result h2 {
            color: #3A3A3A;
        }
        #result h3 {
            color: #FF0000;
        }
        p1 {
            color: #000000;
        }
        p {
            color: #FFFFFF;
        }
        audio {
            margin-top: 20px;
            display: block;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
<body>
    <form id="titanicForm">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br><br>
        <label for="pclass">Passenger Class:</label>
        <select id="pclass" name="pclass" required>
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select><br><br>
        <label for="sex">Sex:</label>
        <select id="sex" name="sex" required>
            <option value="male">Male</option>
            <option value="female">Female</option>
        </select><br><br>
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" required><br><br>
        <label for="sibsp">Siblings/Spouses Aboard:</label>
        <input type="number" id="sibsp" name="sibsp" required><br><br>
        <label for="parch">Parents/Children Aboard:</label>
        <input type="number" id="parch" name="parch" required><br><br>
        <label for="fare">Fare:</label>
        <input type="number" id="fare" name="fare" required><br><br>
        <label for="embarked">Embarked:</label>
        <select id="embarked" name="embarked" required>
            <option value="C">Cherbourg</option>
            <option value="Q">Queenstown</option>
            <option value="S">Southampton</option>
        </select><br><br>
        <label for="alone">Alone:</label>
        <input type="checkbox" id="alone" name="alone"><br><br>
        <button type="button" onclick="predictSurvival()">Predict Survival</button>
    </form>
    <div id="result"></div>
    <script>
    function predictSurvival() {
        var form = document.getElementById('titanicForm');
        var name = document.getElementById('name');
        var resultDiv = document.getElementById('result');
        var formData = new FormData(form);
        fetch('http://localhost:8084/api/titanic/predict', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify(Object.fromEntries(formData))
        })
        .then(response => response.json())
        .then(data => {
            resultDiv.innerHTML = '<h2>Prediction Result for ' + name.value + '</h2>';
            for (var key in data) {
                resultDiv.innerHTML += '<p1>' + key + ': ' + data[key] + '</p1>';
            }
            var deathProbability = parseFloat(data['Death probability']);
            var survivalProbability = parseFloat(data['Survival probability']);
            if (survivalProbability > deathProbability) {
                resultDiv.innerHTML += '<h3>Solid Odds</h3>';
            } else {
                resultDiv.innerHTML += '<h3>Not Looking So Good</h3>';
            }
        })
        .catch(error => {
            console.error('Error:', error);
        });
    }
    </script>
</body>
