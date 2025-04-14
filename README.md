<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ESP32 Motor Control</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      text-align: center;
      padding: 30px;
    }
    h2 {
      color: #333;
    }
    .button {
      padding: 15px 30px;
      margin: 10px;
      font-size: 18px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
    }
    .on {
      background-color: #4CAF50;
      color: white;
    }
    .off {
      background-color: #f44336;
      color: white;
    }
    input[type=range] {
      width: 80%;
      margin: 10px 0;
    }
    input[type=number] {
      width: 80px;
      padding: 5px;
      font-size: 16px;
    }
    .section {
      margin: 20px 0;
      padding: 15px;
      background: white;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h2>ESP32 IoT Controller</h2>

  <div class="section">
    <button id="toggleBtn" class="button off">OFF</button>
  </div>

  <div class="section">
    <h3>Speed Control (PWM 0-255)</h3>
    <input type="range" id="speedSlider" min="0" max="255" value="127">
    <br>
    <input type="number" id="speedInput" min="0" max="255" value="127">
  </div>

  <div class="section">
    <h3>Direction</h3>
    <button onclick="sendCommand('CW')" class="button">Clockwise</button>
    <button onclick="sendCommand('CCW')" class="button">Counter-Clockwise</button>
  </div>

  <script>
    const toggleBtn = document.getElementById("toggleBtn");
    let isOn = false;

    toggleBtn.addEventListener("click", () => {
      isOn = !isOn;
      toggleBtn.textContent = isOn ? "ON" : "OFF";
      toggleBtn.className = isOn ? "button on" : "button off";
      sendCommand(isOn ? "ON" : "OFF");
    });

    const speedSlider = document.getElementById("speedSlider");
    const speedInput = document.getElementById("speedInput");

    speedSlider.addEventListener("input", () => {
      speedInput.value = speedSlider.value;
      sendCommand("SPEED:" + speedSlider.value);
    });

    speedInput.addEventListener("input", () => {
      let val = parseInt(speedInput.value);
      if (isNaN(val) || val < 0 || val > 255) return;
      speedSlider.value = val;
      sendCommand("SPEED:" + val);
    });

    function sendCommand(cmd) {
      // Replace with your ESP32 IP address
      const esp32Ip = "http://192.168.4.1";
      fetch(`${esp32Ip}/cmd?val=${cmd}`)
        .then(response => response.text())
        .then(data => console.log("Command sent:", cmd))
        .catch(error => console.error("Error sending command:", error));
    }
  </script>
</body>
</html>
