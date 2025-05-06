# vesak
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Happy Vesak</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      font-family: sans-serif;
      background: black;
    }

    video#bg-video {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover; /* Fills the screen, may crop */
  z-index: -2;
}

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 0;
    }

    h1 {
      position: absolute;
      top: 30px;
      width: 100%;
      text-align: center;
      color: #fff;
      text-shadow: 0 0 15px #ffd700;
      z-index: 2;
      font-size: 2.5em;
    }

    .fade-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: black;
      z-index: -1;
      animation: fadeIn 2s ease-out forwards;
    }

    @keyframes fadeIn {
      from { opacity: 1; }
      to { opacity: 0; }
    }
  </style>
</head>
<body>

  <!-- Background Video -->
  <video id="bg-video" autoplay muted loop playsinline preload="auto">
    <source src="background.mp4" type="video/mp4" />
    Your browser does not support the video tag.
  </video>

  <div class="fade-overlay"></div>

  <canvas id="bg"></canvas>

  <h1>Happy Vesak!</h1>

  <!-- Background Music -->
  <audio id="bg-music" autoplay loop>
    <source src="music.mp3" type="audio/mp3" />
    Your browser does not support the audio element.
  </audio>

  <script>
    const canvas = document.getElementById('bg');
    const ctx = canvas.getContext('2d');
    let w = canvas.width = window.innerWidth;
    let h = canvas.height = window.innerHeight;

    window.addEventListener('resize', () => {
      w = canvas.width = window.innerWidth;
      h = canvas.height = window.innerHeight;
    });

    const lights = [];

    function createParticle(x = Math.random() * w, y = h, radius = Math.random() * 3 + 2, type = 'light') {
      lights.push({
        x,
        y,
        radius,
        type,
        speedY: Math.random() * 0.5 + 0.3,
        glow: type === 'lantern' ? 'rgba(255, 200, 100, 0.8)' : `hsla(${Math.random() * 60 + 40}, 100%, 80%, 0.8)`
      });
    }

    function draw() {
      ctx.clearRect(0, 0, w, h);
      for (let i = lights.length - 1; i >= 0; i--) {
        const p = lights[i];
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = p.glow;
        ctx.shadowColor = p.glow;
        ctx.shadowBlur = 20;
        ctx.fill();
        p.y -= p.speedY;
        if (p.y < -10) lights.splice(i, 1);
      }
      requestAnimationFrame(draw);
    }

    setInterval(() => {
      createParticle();
      if (Math.random() < 0.1) createParticle(Math.random() * w, h, 5 + Math.random() * 4, 'lantern');
    }, 200);

    document.addEventListener('click', (e) => {
      for (let i = 0; i < 10; i++) {
        createParticle(e.clientX + Math.random() * 40 - 20, e.clientY + Math.random() * 40 - 20);
      }
    });

    draw();
  </script>
</body>
</html>
