<!DOCTYPE html>
<html lang="he">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ahuva Bar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            direction: rtl; /* Right-to-left direction for Hebrew */
            background-color: #f4f4f4; /* Light background color */
            color: #333; /* Dark text color for better readability */
        }
        h1 {
            text-align: center;
            color: #2c3e50; /* Dark blue color for the header */
        }
        form {
            background: #fff; /* White background for the form */
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1); /* Subtle shadow */
            max-width: 600px;
            margin: auto;
        }
        label {
            font-weight: bold;
        }
        input, button {
            display: block;
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #27ae60; /* Green background for buttons */
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #2ecc71; /* Slightly lighter green on hover */
        }
        #result, #workerResult {
            margin-top: 20px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>Ahuva Bar</h1>

    <form id="workerForm">
        <label for="workerName">שם עובד:</label>
        <input type="text" id="workerName" name="workerName" required>
        <label for="hours">שעות עבודה:</label>
        <input type="number" id="hours" name="hours" min="0" required>
        <label for="minutes">דקות עבודה:</label>
        <input type="number" id="minutes" name="minutes" min="0" max="59" required>
        <button type="button" id="addWorkerButton">הוסף עובד</button>
    </form>

    <div id="result"></div>

    <h2>הכנס סכום טיפים וחישוב</h2>
    <form id="totalTipsForm">
        <label for="totalTips">סך כל הטיפים:</label>
        <input type="number" id="totalTips" name="totalTips" min="0" required>
        <button type="submit">חשב טיפ</button>
    </form>

    <div id="workerResult"></div>

    <script>
        let workers = [];
        let totalHours = 0;

        document.getElementById('addWorkerButton').addEventListener('click', function() {
            let workerName = document.getElementById('workerName').value;
            let hours = parseInt(document.getElementById('hours').value);
            let minutes = parseInt(document.getElementById('minutes').value);

            function convertToDecimalTime(hours, minutes) {
                let decimalMinutes = minutes * (100 / 60);
                return hours + (decimalMinutes / 100);
            }

            let decimalTime = convertToDecimalTime(hours, minutes);
            totalHours += decimalTime;

            workers.push({
                name: workerName,
                hours: decimalTime
            });

            document.getElementById('workerName').value = '';
            document.getElementById('hours').value = '';
            document.getElementById('minutes').value = '';

            document.getElementById('result').textContent = `סך כל השעות: ${totalHours.toFixed(2)}`;
        });

        document.getElementById('totalTipsForm').addEventListener('submit', function(event) {
            event.preventDefault();
            let totalTips = parseFloat(document.getElementById('totalTips').value);
            let allocation = totalHours * 6;
            let netPerHour = (totalTips - allocation) / totalHours;

            document.getElementById('result').textContent += `, הפרשה: ${allocation.toFixed(2)}, נטו לשעה: ${netPerHour.toFixed(2)}`;

            let workerResults = workers.map(worker => {
                let amount = worker.hours * netPerHour;
                return `<p>${worker.name}: ${amount.toFixed(2)}</p>`;
            }).join('');

            document.getElementById('workerResult').innerHTML = workerResults;
        });
    </script>
</body>
</html>
