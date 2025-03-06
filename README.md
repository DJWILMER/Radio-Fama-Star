:root {
  --primary: #ff5757;
  --secondary: #ffbe0b;
  --background: #fff5e6;
  --text: #2d3436;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Poppins', sans-serif;
  background: var(--background);
  color: var(--text);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.header {
  width: 100%;
  padding: 2rem;
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  text-align: center;
  color: white;
  position: relative;
  overflow: hidden;
}

.header::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 50px;
  background: var(--background);
  clip-path: polygon(0 100%, 100% 100%, 100% 0, 0 100%);
}

.radio-container {
  max-width: 800px;
  width: 90%;
  margin: 2rem auto;
  background: white;
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

.player-controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.play-button {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: none;
  background: var(--primary);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s, background-color 0.2s;
  box-shadow: 0 4px 15px rgba(255, 87, 87, 0.3);
}

.play-button:hover {
  transform: scale(1.05);
  background: #ff4242;
}

.visualizer {
  display: flex;
  gap: 4px;
  height: 60px;
  align-items: center;
}

.bar {
  width: 4px;
  height: 20px;
  background: var(--primary);
  border-radius: 2px;
  animation: bounce 1.2s ease infinite;
  transform-origin: bottom;
}

@keyframes bounce {
  0%, 100% { transform: scaleY(0.3); }
  50% { transform: scaleY(1); }
}

.now-playing {
  text-align: center;
  margin-top: 2rem;
  padding: 1rem;
  border-top: 2px solid #f0f0f0;
}

.station-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 2px solid #f0f0f0;
}

.social-links {
  display: flex;
  gap: 1rem;
}

.social-links a {
  color: var(--text);
  text-decoration: none;
  transition: color 0.2s;
}

.social-links a:hover {
  color: var(--primary);
}

</style>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
</head>
<body>

<header class="header">
  <h1>RADIO FAMA STAR</h1>
  <p>Tu música favorita, las 24 horas del día</p>
</header>

<main class="radio-container">
  <div class="player-controls">
    <button class="play-button" id="playBtn">
      <svg width="30" height="30" viewBox="0 0 24 24" fill="white">
        <path d="M8 5v14l11-7z"/>
      </svg>
    </button>
    
    <div class="visualizer" id="visualizer" style="display: none;">
      <div class="bar" style="animation-delay: -0.4s"></div>
      <div class="bar" style="animation-delay: -0.2s"></div>
      <div class="bar" style="animation-delay: 0s"></div>
      <div class="bar" style="animation-delay: -0.6s"></div>
      <div class="bar" style="animation-delay: -0.8s"></div>
      <div class="bar" style="animation-delay: -0.2s"></div>
      <div class="bar" style="animation-delay: -0.4s"></div>
    </div>
  </div>

  <div class="now-playing">
    <h2>En vivo</h2>
    <p id="currentTrack">Presiona play para escuchar</p>
  </div>

  <div class="station-info">
    <div>
      <p>DJ Alex -TLf. 932 373 367</p>
      <p>Transmitiendo desde Olmos, Lambayeque Chiclayo peru </p>
    </div>
    <div class="social-links">
      <a href=" https://www.facebook.com/profile.php?id=61559775170766" target="_blank"><i class="fab fa-facebook"></i></a>
      <a href=" https://wa.link/8ne02m" target="_blank"><i class="fab fa-whatsapp"></i></a>
      <a href=" https://www.instagram.com/alexitokalle" target="_blank"><i class="fab fa-instagram"></i></a>
    </div>
  </div>
</main>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const audio = new Audio('https://stream.zeno.fm/cx4zdpac3ilvv?an');
  const playBtn = document.getElementById('playBtn');
  const visualizer = document.getElementById('visualizer');
  const currentTrack = document.getElementById('currentTrack');
  let isPlaying = false;

  playBtn.addEventListener('click', () => {
    if (!isPlaying) {
      audio.play()
        .then(() => {
          isPlaying = true;
          playBtn.innerHTML = `
            <svg width="30" height="30" viewBox="0 0 24 24" fill="white">
              <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>
            </svg>
          `;
          visualizer.style.display = 'flex';
          currentTrack.textContent = 'Radio Fama Star - Transmisión en vivo';
        })
        .catch(error => {
          console.error('Error playing stream:', error);
          currentTrack.textContent = 'Error al reproducir. Intenta nuevamente.';
        });
    } else {
      audio.pause();
      isPlaying = false;
      playBtn.innerHTML = `
        <svg width="30" height="30" viewBox="0 0 24 24" fill="white">
          <path d="M8 5v14l11-7z"/>
        </svg>
      `;
      visualizer.style.display = 'none';
      currentTrack.textContent = 'Presiona play para escuchar';
    }
  });

  audio.addEventListener('error', () => {
    isPlaying = false;
    visualizer.style.display = 'none';
    currentTrack.textContent = 'Error en la transmisión. Por favor intenta más tarde.';
    playBtn.innerHTML = `
      <svg width="30" height="30" viewBox="0 0 24 24" fill="white">
        <path d="M8 5v14l11-7z"/>
      </svg>
    `;
  });

  // Optional: Update metadata if available
  audio.addEventListener('metadata', (e) => {
    if (e.detail && e.detail.title) {
      currentTrack.textContent = e.detail.title;
    }
  });
});
</script>

</body></html>
