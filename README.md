<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Spinning Wheel</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background: linear-gradient(135deg, #4facfe, #00f2fe);
      min-height: 100vh;
      margin: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }
    h1 {
      color: white;
      text-shadow: 2px 2px 5px rgba(0,0,0,0.4);
    }
    #wheelCanvas {
      border-radius: 50%;
      box-shadow: 0 8px 20px rgba(0,0,0,0.4);
      margin: 20px;
    }
    .controls {
      margin-top: 20px;
      display: flex;
      gap: 10px;
      justify-content: center;
      flex-wrap: wrap;
    }
    input {
      padding: 10px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
    }
    button {
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      color: white;
      background: #ff6a00;
      transition: 0.3s;
    }
    button:hover {
      background: #ff9500;
    }
    #result {
      margin-top: 20px;
      font-size: 20px;
      color: white;
      font-weight: bold;
      text-shadow: 2px 2px 5px rgba(0,0,0,0.4);
    }
  </style>
</head>
<body>
  <h1>🎡 Spinning Wheel</h1>
  <canvas id="wheelCanvas" width="400" height="400"></canvas>
  
  <div class="controls">
    <input type="text" id="itemInput" placeholder="Add a label..." />
    <button onclick="addItem()">Add</button>
    <button onclick="spinWheel()">Spin 🎯</button>
  </div>

  <div id="result"></div>

  <script>
    const canvas = document.getElementById("wheelCanvas");
    const ctx = canvas.getContext("2d");
    let items = ["Yes", "No", "Maybe", "Try Again"];
    let startAngle = 0;
    let arc = Math.PI / (items.length / 2);
    let spinTimeout = null;
    let spinAngleStart = 0;
    let spinTime = 0;
    let spinTimeTotal = 0;

    function drawWheel() {
      const outsideRadius = 180;
      const textRadius = 140;
      const insideRadius = 50;

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = "white";
      ctx.lineWidth = 2;
      ctx.font = "bold 16px Arial";

      arc = Math.PI / (items.length / 2);

      for (let i = 0; i < items.length; i++) {
        const angle = startAngle + i * arc;
        ctx.fillStyle = i % 2 === 0 ? "#ff6a00" : "#ffcc00";

        ctx.beginPath();
        ctx.arc(200, 200, outsideRadius, angle, angle + arc, false);
        ctx.arc(200, 200, insideRadius, angle + arc, angle, true);
        ctx.fill();
        ctx.save();

        ctx.fillStyle = "black";
        ctx.translate(
          200 + Math.cos(angle + arc / 2) * textRadius,
          200 + Math.sin(angle + arc / 2) * textRadius
        );
        ctx.rotate(angle + arc / 2 + Math.PI / 2);
        const text = items[i];
        ctx.fillText(text, -ctx.measureText(text).width / 2, 0);
        ctx.restore();
      }

      // Arrow
      ctx.fillStyle = "white";
      ctx.beginPath();
      ctx.moveTo(200 - 10, 20);
      ctx.lineTo(200 + 10, 20);
      ctx.lineTo(200, 50);
      ctx.fill();
    }

    function spinWheel() {
      spinAngleStart = Math.random() * 10 + 10;
      spinTime = 0;
      spinTimeTotal = Math.random() * 3000 + 4000;
      rotateWheel();
    }

    function rotateWheel() {
      spinTime += 30;
      if (spinTime >= spinTimeTotal) {
        stopRotateWheel();
        return;
      }
      const spinAngle =
        spinAngleStart - easeOut(spinTime, 0, spinAngleStart, spinTimeTotal);
      startAngle += (spinAngle * Math.PI) / 180;
      drawWheel();
      spinTimeout = setTimeout(rotateWheel, 30);
    }

    function stopRotateWheel() {
      clearTimeout(spinTimeout);
      const degrees = (startAngle * 180) / Math.PI + 90;
      const arcd = (arc * 180) / Math.PI;
      const index = Math.floor((360 - (degrees % 360)) / arcd);
      const result = items[index];
      document.getElementById("result").textContent = `Result: ${result}`;
    }

    function easeOut(t, b, c, d) {
      const ts = (t /= d) * t;
      const tc = ts * t;
      return b + c * (tc + -3 * ts + 3 * t);
    }

    function addItem() {
      const input = document.getElementById("itemInput");
      const value = input.value.trim();
      if (value) {
        items.push(value);
        input.value = "";
        drawWheel();
      }
    }

    drawWheel();
  </script>
</body>
</html># The-spin-wheel
Spin whel
