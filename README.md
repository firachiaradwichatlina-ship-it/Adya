# Adya
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Adya Surya Adiana - Heart Pattern</title>
  <style>
    body {
      background: #fff0f6;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    canvas {
      background: transparent;
      display: block;
    }
    h2 {
      text-align: center;
      color: #e75480;
      font-family: 'Comic Sans MS', cursive, sans-serif;
    }
  </style>
</head>
<body>
  <div>
    <h2>Will you accept me as your girlfriend, Adya Surya Adiana?</h2>
    <canvas id="heartCanvas" width="600" height="600"></canvas>
  </div>
  <script>
    const canvas = document.getElementById('heartCanvas');
    const ctx = canvas.getContext('2d');
    const name = "Adya Surya Adiana";
    const fontSize = 28;
    let angleOffset = 0;

    function heartPath(t, size = 180, centerX = 300, centerY = 320) {
      // Heart parametric equations
      const x = centerX + size * 16 * Math.pow(Math.sin(t), 3);
      const y = centerY - size * (13 * Math.cos(t) - 5 * Math.cos(2 * t)
        - 2 * Math.cos(3 * t) - Math.cos(4 * t));
      return {x, y};
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      // Draw heart (for background)
      ctx.save();
      ctx.beginPath();
      for (let t = 0; t <= Math.PI * 2; t += 0.01) {
        const {x, y} = heartPath(t);
        if (t === 0) ctx.moveTo(x, y);
        else ctx.lineTo(x, y);
      }
      ctx.closePath();
      ctx.strokeStyle = "#ff69b4";
      ctx.lineWidth = 5;
      ctx.stroke();
      ctx.restore();

      // Draw rotating text along heart
      ctx.save();
      ctx.font = `${fontSize}px 'Comic Sans MS', cursive, sans-serif`;
      ctx.fillStyle = "#e75480";
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      // Distribute characters evenly along the heart path
      const totalSteps = name.length;
      for (let i = 0; i < name.length; i++) {
        let t = (i / totalSteps) * Math.PI * 2 + angleOffset;
        const {x, y} = heartPath(t);
        // Optional: rotate each character for natural alignment
        const delta = 0.001;
        const p1 = heartPath(t);
        const p2 = heartPath(t + delta);
        const angle = Math.atan2(p2.y - p1.y, p2.x - p1.x);

        ctx.save();
        ctx.translate(x, y);
        ctx.rotate(angle);
        ctx.fillText(name[i], 0, 0);
        ctx.restore();
      }
      ctx.restore();
    }

    function animate() {
      angleOffset += 0.01;
      draw();
      requestAnimationFrame(animate);
    }

    animate();
  </script>
</body>
</html>