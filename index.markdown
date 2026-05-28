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

    /* MODEL VIEWER TAKES REMAINING SPACE */
    model-viewer {
      flex: 1 1 auto;
      width: 100%;
      background: #222;
      display: block;
      min-height: 0;
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

    <h1>meinland</h1>

    <model-viewer
      id="viewer"
      src="{{ '/assets/models/test-ar.glb' | relative_url }}"
      camera-controls
      auto-rotate
      shadow-intensity="1"
      ar
    >
    </model-viewer>

    <div class="controls">
      <button id="prevBtn">←</button>
      <button id="nextBtn">→</button>
    </div>

  </div>

  <script>
    const viewer = document.getElementById("viewer");

    const models = [
      "{{ '/assets/models/test-ar.glb' | relative_url }}",
    ];

    let currentIndex = 0;

    function updateModel() {
      viewer.src = models[currentIndex];
    }

    document.getElementById("nextBtn").addEventListener("click", () => {
      currentIndex++;

      if (currentIndex >= models.length) {
        currentIndex = 0;
      }

      updateModel();
    });

    document.getElementById("prevBtn").addEventListener("click", () => {
      currentIndex--;

      if (currentIndex < 0) {
        currentIndex = models.length - 1;
      }

      updateModel();
    });
  </script>

</body>