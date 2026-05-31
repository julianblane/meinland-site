---
---
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>meinland</title>

  <script type="module"
    src="https://unpkg.com/@google/model-viewer/dist/model-viewer.min.js">
  </script>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background: #111;
      color: white;
      font-family: Arial, sans-serif;
      max-width: 768px;
    }

    /* FULLSCREEN LAYOUT */
    .art-viewer {
      height: 100dvh;
      display: flex;
      flex-direction: column;
    }

    /* FIXED TITLE HEIGHT */
    h1 {
      flex: 0 0 80px;

      display: flex;
      align-items: center;
      justify-content: center;

      font-size: 2rem;
    }

    .model-container {
      position: relative;
      flex: 1 1 auto;
      min-height: 0;
    }

    .model-container model-viewer {
      width: 100%;
      height: 100%;
      background: #222;
    }

    #art-caption {
      position: absolute;
      right: 20px;
      bottom: 20px;

      text-align: right;
      line-height: 1.4;

      color: white;
      text-shadow: 0 0 8px rgba(0,0,0,0.8);

      pointer-events: none;
    }

    /* FIXED CONTROLS HEIGHT */
    .controls {
      flex: 0 0 100px;

      display: flex;
      justify-content: center;
      align-items: center;
      gap: 40px;
    }

    .controls button {
      font-size: 32px;
      padding: 10px 25px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      background: #333;
      color: white;
      transition: 0.2s;
    }

    .controls button:hover {
      background: #555;
    }
  </style>
</head>

<body>

  <div class="art-viewer">

  <img src="{{ '/assets/img/header-meinland.png' | relative_url }}" />

  <div class="model-container">
    <model-viewer
      id="viewer"
      src="{{ '/assets/models/canvas-lasania.glb' | relative_url }}"
      camera-controls
      auto-rotate
      shadow-intensity="1"
      ar
      ar-placement="wall"
    >
    <button slot="ar-button">
    </button>
    </model-viewer>

    <p id="art-caption">
      Lasania<br>
      18x13cm<br>
      oleo sobre lienzo
    </p>
  </div>

  <div class="controls">
    <button id="prevBtn">←</button>
    <button id="nextBtn">→</button>
  </div>

  </div>

  <script>
    const viewer = document.getElementById("viewer");
    const artCaption = document.getElementById("art-caption");

    const art = {{site.data.art | jsonify}}

    let currentIndex = 0;

    function updateModel() {
      var artwork = art[currentIndex];
      viewer.src = artwork.src;
      artCaption.innerHTML = [
        artwork.name,
        artwork.technique,
        artwork.size
      ].join("<br>");
    }

    document.getElementById("nextBtn").addEventListener("click", () => {
      currentIndex++;

      if (currentIndex >= art.length) {
        currentIndex = 0;
      }

      updateModel();
    });

    document.getElementById("prevBtn").addEventListener("click", () => {
      currentIndex--;

      if (currentIndex < 0) {
        currentIndex = art.length - 1;
      }

      updateModel();
    });

    updateModel();
  </script>

</body>