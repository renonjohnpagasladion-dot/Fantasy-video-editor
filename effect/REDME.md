<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fantasy Editor</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #100d20;
  color: white;
}

header {
  text-align: center;
  padding: 24px 12px;
  background: linear-gradient(135deg, #25104b, #7028a0);
}

header h1 {
  margin: 0;
  font-size: 28px;
}

main {
  max-width: 650px;
  margin: auto;
  padding: 16px;
}

.panel {
  background: #211936;
  border: 1px solid #49366c;
  border-radius: 16px;
  padding: 16px;
  margin-bottom: 18px;
}

h2 {
  color: #e0b8ff;
  font-size: 19px;
}

input, button {
  width: 100%;
  margin-top: 10px;
  padding: 13px;
  border-radius: 10px;
  font-size: 15px;
}

input {
  background: white;
  color: #222;
}

button {
  border: 1px solid #7954a5;
  background: #382453;
  color: white;
  font-weight: bold;
}

button.selected {
  background: #a64cff;
  border: 2px solid white;
}

#preview {
  position: relative;
  width: 100%;
  min-height: 200px;
  background: black;
  border-radius: 12px;
  overflow: hidden;
  margin-top: 12px;
}

#video {
  display: block;
  width: 100%;
  max-height: 420px;
}

#effectVideo {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: contain;
  inset: 0;
  pointer-events: none;
  display: none;
  mix-blend-mode: screen;
}

.effects {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.effects button {
  margin: 0;
  min-height: 65px;
}

.hint {
  color: #c8bddb;
  font-size: 13px;
  line-height: 1.5;
}

footer {
  text-align: center;
  padding: 20px;
  color: #b7a9ca;
  font-size: 12px;
}
</style>
</head>

<body>

<header>
  <h1>💎 Fantasy Editor</h1>
  <p>Bring your magical imagination to life!</p>
</header>

<main>

<section class="panel">
  <h2>🎬 1. Choose Your Video</h2>
  <input id="videoFile" type="file" accept="video/*">

  <div id="preview">
    <video id="video" controls playsinline></video>
    <video id="effectVideo" muted playsinline loop></video>
  </div>

  <p class="hint">
    Upload your original video, then choose a magical effect.
  </p>
</section>

<section class="panel">
  <h2>✨ 2. Choose a Brilyante Effect</h2>

  <div class="effects">
    <button data-effect="fire">🔥 Brilyante ng Apoy</button>
    <button data-effect="water">💧 Brilyante ng Tubig</button>
    <button data-effect="wind">🌪️ Brilyante ng Hangin</button>
    <button data-effect="earth">🌿 Brilyante ng Lupa</button>
  </div>

  <button id="removeEffect">Remove Effect</button>

  <p class="hint">
    Upload your effect videos into the effects folder in your GitHub
    repository, then connect their filenames in the code.
  </p>
</section>

<section class="panel">
  <h2>🎨 3. Adjust Your Effect</h2>

  <label for="effectSize">Effect Size</label>
  <input id="effectSize" type="range" min="20" max="150" value="100">

  <label for="effectX">Horizontal Position</label>
  <input id="effectX" type="range" min="0" max="100" value="50">

  <label for="effectY">Vertical Position</label>
  <input id="effectY" type="range" min="0" max="100" value="50">
</section>

<section class="panel">
  <h2>📤 4. Export</h2>
  <p class="hint">
    This version previews the effect. Exporting a finished video with
    the effect permanently applied requires additional rendering code.
  </p>
</section>

</main>

<footer>Fantasy Editor · Version 2.0</footer>

<script>
const video = document.getElementById("video");
const effectVideo = document.getElementById("effectVideo");

document.getElementById("videoFile").addEventListener("change", event => {
  const file = event.target.files[0];
  if (!file) return;

  video.src = URL.createObjectURL(file);
  video.load();
});

const effectFiles = {
  fire: "effects/fire.mp4",
  water: "effects/water.mp4",
  wind: "effects/wind.mp4",
  earth: "effects/earth.mp4"
};

document.querySelectorAll("[data-effect]").forEach(button => {
  button.addEventListener("click", async () => {
    document.querySelectorAll("[data-effect]").forEach(item => {
      item.classList.remove("selected");
    });

    button.classList.add("selected");

    effectVideo.src = effectFiles[button.dataset.effect];
    effectVideo.style.display = "block";

    try {
      await effectVideo.play();
    } catch (error) {
      console.log("Tap the video or select the effect again to play it.");
    }

    updateEffect();
  });
});

document.getElementById("removeEffect").addEventListener("click", () => {
  effectVideo.pause();
  effectVideo.removeAttribute("src");
  effectVideo.load();
  effectVideo.style.display = "none";

  document.querySelectorAll("[data-effect]").forEach(button => {
    button.classList.remove("selected");
  });
});

function updateEffect() {
  const size = Number(document.getElementById("effectSize").value);
  const x = Number(document.getElementById("effectX").value);
  const y = Number(document.getElementById("effectY").value);

  effectVideo.style.width = size + "%";
  effectVideo.style.height = size + "%";
  effectVideo.style.left = x + "%";
  effectVideo.style.top = y + "%";
  effectVideo.style.inset = "auto";
  effectVideo.style.transform = "translate(-50%, -50%)";
}

["effectSize", "effectX", "effectY"].forEach(id => {
  document.getElementById(id).addEventListener("input", updateEffect);
});
</script>

</body>
</html>
