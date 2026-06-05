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
    :root {
      --frame-size: 64px;
    }

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

    #header{
      padding-bottom: 8px;
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
      position: relative;
    }

    /* top + bottom */
    .model-container::before {
        content: "";
        position: absolute;
        inset: calc(var(--frame-size) / -2) 0;
        pointer-events: none;
        z-index: 1;

        background:
            url("{{ '/assets/img/frame-outline-x.png' | relative_url }}") top repeat-x,
            url("{{ '/assets/img/frame-outline-x.png' | relative_url }}") bottom repeat-x;

        background-size:
            auto var(--frame-size),
            auto var(--frame-size);
    }

    .model-container::after {
        content: "";
        position: absolute;
        inset: 0;

        background:
            url("{{ '/assets/img/frame-outline-y.png' | relative_url }}")
            left calc(var(--frame-size) / -2) top 0 repeat-y,

            url("{{ '/assets/img/frame-outline-y.png' | relative_url }}")
            right calc(var(--frame-size) / -2) top 0 repeat-y;

        background-size:
            var(--frame-size) auto,
            var(--frame-size) auto;

        pointer-events: none;
    }

    #art-caption {
        z-index: 2;
    }

    #art-caption {
      position: absolute;
      right: 32px;
      bottom: 32px;

      text-align: right;
      line-height: 1.4;

      color: white;
      text-shadow: 0 0 8px rgba(0,0,0,0.8);

      pointer-events: none;
    }

    /* FIXED CONTROLS HEIGHT */
    .controls {
      flex: 0 0 128px;

      display: flex;
      justify-content: center;
      align-items: center;
      gap: 64px;
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
    
    #prevBtn,
    #nextBtn {
        width: 92px;
        height: 100%;
        background: url("{{ '/assets/img/button-right.png' | relative_url }}") center/contain no-repeat;
        border: none;
        cursor: pointer;
    }

    /* Flip horizontally */
    #prevBtn {
        transform: scaleX(-1);
    }
  </style>
</head>

<body>

  <div class="art-viewer">

  <img id="header" src="{{ '/assets/img/header-meinland.png' | relative_url }}" />

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
    <button id="prevBtn"></button>
    <button id="nextBtn"></button>
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