# Drive-games
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no" />
  <title>Garage Drift 3D Mobile</title>
  <style>
    :root {
      --bg: #071118;
      --panel: rgba(10, 18, 24, 0.82);
      --panel-border: rgba(255,255,255,0.12);
      --text: #edf8ff;
      --muted: #bfd0df;
      --green: #7ef29a;
      --accent: #4cc9f0;
      --danger: #ff6c72;
    }

    * { box-sizing: border-box; }

    html, body {
      margin: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      background: #050b11;
      color: var(--text);
      font-family: Arial, Helvetica, sans-serif;
      touch-action: none;
      -webkit-user-select: none;
      user-select: none;
    }

    #gameRoot {
      position: relative;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
    }

    canvas {
      display: block;
      width: 100%;
      height: 100%;
      background: #9cc6df;
      touch-action: none;
    }

    .screen {
      position: absolute;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(3,8,12,0.72);
      backdrop-filter: blur(8px);
      z-index: 35;
    }

    .screen.hidden { display: none; }

    .menu-card {
      width: min(760px, 84vw);
      background: rgba(10, 18, 25, 0.88);
      border: 1px solid var(--panel-border);
      border-radius: 22px;
      box-shadow: 0 24px 50px rgba(0,0,0,0.38);
      padding: 26px 28px;
    }

    .menu-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 20px;
      margin-bottom: 20px;
    }

    .menu-header h1 {
      margin: 0;
      font-size: clamp(1.8rem, 4vw, 3rem);
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--green);
    }

    .menu-sub {
      color: var(--muted);
      font-size: 0.9rem;
      line-height: 1.5;
      margin-bottom: 18px;
    }

    .menu-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 16px;
      margin-top: 18px;
    }

    .menu-box {
      background: rgba(255,255,255,0.03);
      border: 1px solid var(--panel-border);
      border-radius: 16px;
      padding: 16px;
    }

    .menu-box h3 {
      margin: 0 0 12px;
      font-size: 1rem;
      color: var(--text);
    }

    .menu-box p {
      margin: 0;
      color: var(--muted);
      font-size: 0.8rem;
      line-height: 1.5;
    }

    .menu-actions {
      display: flex;
      justify-content: flex-start;
      flex-wrap: wrap;
      gap: 12px;
      margin-top: 22px;
    }

    button {
      border: 1px solid rgba(255,255,255,0.14);
      background: rgba(255,255,255,0.04);
      color: var(--text);
      border-radius: 10px;
      padding: 10px 16px;
      cursor: pointer;
      font-weight: 700;
      transition: transform 0.12s ease, background 0.12s ease;
      touch-action: manipulation;
    }

    button:hover {
      transform: translateY(-1px);
      background: rgba(255,255,255,0.08);
    }

    button.primary {
      background: rgba(76, 201, 240, 0.18);
      border-color: rgba(76, 201, 240, 0.42);
    }

    .panel {
      position: absolute;
      left: 18px;
      top: 18px;
      width: min(330px, calc(100vw - 30px));
      background: var(--panel);
      border: 1px solid var(--panel-border);
      border-radius: 16px;
      padding: 16px 16px 12px;
      backdrop-filter: blur(8px);
      box-shadow: 0 18px 36px rgba(0,0,0,0.28);
      z-index: 20;
      transition: transform 0.25s ease, opacity 0.25s ease;
    }

    .panel.hidden {
      transform: translateX(-115%);
      opacity: 0;
      pointer-events: none;
    }

    .title {
      margin: 0 0 12px;
      text-transform: uppercase;
      letter-spacing: 0.10em;
      font-size: 0.82rem;
      color: var(--green);
    }

    .row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 12px;
      margin: 10px 0;
      color: var(--muted);
      font-size: 0.9rem;
    }

    .row label { color: var(--muted); }

    input[type="color"] {
      width: 58px;
      height: 30px;
      border: none;
      background: transparent;
      border-radius: 8px;
      cursor: pointer;
      padding: 0;
    }

    input[type="checkbox"] {
      width: 18px;
      height: 18px;
      accent-color: var(--accent);
    }

    input[type="range"] {
      width: 100%;
      accent-color: var(--accent);
      cursor: pointer;
    }

    .select-row {
      margin-top: 8px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      align-items: center;
    }

    .slot-btn {
      flex: 1 1 0;
      min-width: 62px;
      padding: 8px 10px;
      border-radius: 10px;
      border: 1px solid rgba(255,255,255,0.12);
      background: rgba(255,255,255,0.04);
      color: var(--text);
      cursor: pointer;
      font-weight: 700;
    }

    .slot-btn.active {
      background: rgba(76, 201, 240, 0.18);
      border-color: rgba(76, 201, 240, 0.4);
    }

    .small {
      margin-top: 10px;
      color: var(--muted);
      font-size: 0.78rem;
      line-height: 1.5;
    }

    .buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 12px;
    }

    .hud {
      position: absolute;
      right: 18px;
      top: 18px;
      min-width: 220px;
      background: rgba(12, 20, 28, 0.82);
      border: 1px solid var(--panel-border);
      border-radius: 14px;
      padding: 12px 14px;
      color: var(--muted);
      font-size: 0.85rem;
      line-height: 1.7;
      z-index: 20;
    }

    .hud strong { color: var(--text); }

    .mission {
      position: absolute;
      left: 50%;
      top: 18px;
      transform: translateX(-50%);
      background: rgba(8, 12, 18, 0.72);
      border: 1px solid var(--panel-border);
      border-radius: 14px;
      padding: 10px 14px;
      text-align: center;
      max-width: 520px;
      z-index: 20;
      font-size: 0.86rem;
      line-height: 1.5;
      color: var(--text);
    }

    .mission strong { color: var(--green); }

    .message {
      position: absolute;
      left: 50%;
      bottom: 26px;
      transform: translateX(-50%);
      padding: 10px 16px;
      background: rgba(8,12,18,0.8);
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      color: var(--muted);
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.18s ease;
      z-index: 20;
      font-size: 0.85rem;
    }

    .message.show { opacity: 1; }

    .wanted-indicator {
      position: absolute;
      left: 20px;
      bottom: 18px;
      padding: 10px 14px;
      background: rgba(10, 20, 26, 0.7);
      border: 1px solid var(--panel-border);
      border-radius: 12px;
      color: var(--muted);
      font-size: 0.82rem;
      z-index: 20;
    }

    .wanted-indicator strong { color: var(--danger); }

    .mobile-controls {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      height: 170px;
      z-index: 25;
      pointer-events: none;
      display: none;
    }

    .mobile-controls.visible { display: block; }

    .left-pad, .right-pad {
      position: absolute;
      bottom: 18px;
      pointer-events: auto;
    }

    .left-pad {
      left: 18px;
      width: 170px;
      height: 120px;
      border-radius: 18px;
      background: rgba(10,16,22,0.28);
      border: 1px solid var(--panel-border);
    }

    .right-pad {
      right: 18px;
      width: 190px;
      height: 120px;
      border-radius: 18px;
      background: rgba(10,16,22,0.28);
      border: 1px solid var(--panel-border);
    }

    .control-btn {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 58px;
      height: 58px;
      border-radius: 18px;
      background: rgba(255,255,255,0.09);
      border: 1px solid rgba(255,255,255,0.14);
      color: var(--text);
      font-size: 1.2rem;
      font-weight: 700;
      user-select: none;
      touch-action: manipulation;
      pointer-events: auto;
    }

    .control-btn:active {
      background: rgba(255,255,255,0.15);
      transform: scale(0.98);
    }

    .left-btn { left: 18px; top: 28px; }
    .right-btn { right: 18px; top: 28px; }
    .throttle-btn { right: 18px; top: 18px; }
    .brake-btn { right: 18px; bottom: 18px; }

    .garage-btn {
      position: absolute;
      right: 18px;
      top: 140px;
      width: 76px;
      height: 36px;
      border-radius: 12px;
      background: rgba(76, 201, 240, 0.15);
      border: 1px solid rgba(76, 201, 240, 0.35);
      font-size: 0.8rem;
    }

    @media (max-width: 900px) {
      .mission {
        max-width: min(78vw, 430px);
        font-size: 0.78rem;
      }

      .hud {
        right: 12px;
        top: 12px;
        min-width: 180px;
        font-size: 0.78rem;
      }

      .panel {
        left: 12px;
        top: 12px;
        width: min(290px, calc(100vw - 24px));
        padding: 14px;
      }
    }

    @media (max-width: 700px) {
      .panel {
        transform: scale(0.95);
        transform-origin: top left;
      }

      .mobile-controls { display: block; }

      .mission {
        top: 10px;
        max-width: 62vw;
        font-size: 0.74rem;
      }

      .hud {
        min-width: 150px;
        font-size: 0.72rem;
      }

      .panel {
        width: min(290px, calc(100vw - 22px));
      }
    }
  </style>
</head>
<body>
  <div id="gameRoot">
    <div id="startScreen" class="screen">
      <div class="menu-card">
        <div class="menu-header">
          <h1>Garage Drift 3D</h1>
          <button id="startBtn" class="primary">Start</button>
        </div>

        <div class="menu-sub">
          Drive the city, tune your ride, drift for points, hit checkpoints, and stay ahead of the police.
        </div>

        <div class="menu-grid">
          <div class="menu-box">
            <h3>Garage</h3>
            <p>Save up to 4 custom builds across different slots. Switch instantly.</p>
          </div>

          <div class="menu-box">
            <h3>Drift Tuning</h3>
            <p>Upgrade engine, grip, and drift balance for different driving styles.</p>
          </div>

          <div class="menu-box">
            <h3>Mission City</h3>
            <p>Complete checkpoint routes while traffic and police react.</p>
          </div>

          <div class="menu-box">
            <h3>Police Chase</h3>
            <p>High-speed driving raises wanted level. Escape or evade the patrol units.</p>
          </div>
        </div>

        <div class="menu-actions">
          <button id="garageBtn" class="primary">Open Garage</button>
          <button id="tuneBtn">Open Tuning</button>
        </div>
      </div>
    </div>

    <div id="garagePanel" class="panel">
      <h2 class="title">Garage</h2>

      <div class="select-row" id="slotButtons"></div>

      <div class="row">
        <label for="paintColor">Paint</label>
        <input id="paintColor" type="color" value="#ff6b6b" />
      </div>

      <div class="row">
        <label for="rimColor">Rims</label>
        <input id="rimColor" type="color" value="#1f2933" />
      </div>

      <div class="row">
        <label for="accentColor">Neon</label>
        <input id="accentColor" type="color" value="#4cc9f0" />
      </div>

      <div class="row">
        <label for="spoiler">Spoiler</label>
        <input id="spoiler" type="checkbox" checked />
      </div>

      <div class="row" style="display:block;">
        <label for="engine" style="display:block; margin-bottom:6px;">Engine</label>
        <input id="engine" type="range" min="1" max="100" value="72" />
      </div>

      <div class="row" style="display:block;">
        <label for="handling" style="display:block; margin-bottom:6px;">Grip</label>
        <input id="handling" type="range" min="1" max="100" value="58" />
      </div>

      <div class="row" style="display:block;">
        <label for="drift" style="display:block; margin-bottom:6px;">Drift</label>
        <input id="drift" type="range" min="1" max="100" value="60" />
      </div>

      <div class="buttons">
        <button class="primary" id="saveBtn">Save Slot</button>
        <button id="loadBtn">Load</button>
        <button id="resetBtn">Reset</button>
      </div>

      <div class="small">
        Save different builds per slot. Tune for grip, speed, and drift angle.
      </div>
    </div>

    <div id="missionText" class="mission"><strong>Mission:</strong> Drive to checkpoint 1.</div>

    <div class="hud">
      <div><strong>Speed:</strong> <span id="speedReadout">0</span> km/h</div>
      <div><strong>Gear:</strong> <span id="gearReadout">N</span></div>
      <div><strong>Garage:</strong> <span id="garageStatus">Away</span></div>
      <div><strong>Health:</strong> <span id="healthReadout">100%</span></div>
      <div><strong>Wanted:</strong> <span id="wantedReadout">0</span></div>
      <div><strong>Drift:</strong> <span id="driftScoreReadout">0</span></div>
    </div>

    <div class="wanted-indicator">
      <strong>Police:</strong> <span id="policeStatus">Idle</span>
    </div>

    <div class="mobile-controls visible">
      <div class="left-pad">
        <div class="control-btn left-btn" data-key="ArrowLeft">◀</div>
        <div class="control-btn right-btn" data-key="ArrowRight">▶</div>
      </div>

      <div class="right-pad">
        <div class="control-btn throttle-btn" data-key="ArrowUp">▲</div>
        <div class="control-btn brake-btn" data-key="Space">■</div>
      </div>

      <button class="garage-btn" id="mobileGarageBtn">Garage</button>
    </div>

    <div id="messageBox" class="message">Ready.</div>
  </div>

  <script type="module">
    import * as THREE from "https://cdn.jsdelivr.net/npm/three@0.161.0/build/three.module.js";

    const STORAGE_KEY = "garage_drift_3d_slots_v1";
    const START_SCREEN = document.getElementById("startScreen");
    const garagePanel = document.getElementById("garagePanel");
    const missionText = document.getElementById("missionText");
    const messageBox = document.getElementById("messageBox");
    const speedReadout = document.getElementById("speedReadout");
    const gearReadout = document.getElementById("gearReadout");
    const garageStatus = document.getElementById("garageStatus");
    const healthReadout = document.getElementById("healthReadout");
    const wantedReadout = document.getElementById("wantedReadout");
    const driftScoreReadout = document.getElementById("driftScoreReadout");
    const policeStatus = document.getElementById("policeStatus");
    const slotButtonsWrap = document.getElementById("slotButtons");
    const mobileGarageBtn = document.getElementById("mobileGarageBtn");

    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xa8d0eb);
    scene.fog = new THREE.Fog(0xa8d0eb, 50, 200);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    document.getElementById("gameRoot").appendChild(renderer.domElement);

    const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 500);
    camera.position.set(0, 9, 16);

    const hemi = new THREE.HemisphereLight(0xffffff, 0x4d5f4f, 1.12);
    scene.add(hemi);

    const sun = new THREE.DirectionalLight(0xffffff, 1.25);
    sun.position.set(30, 45, 25);
    sun.castShadow = true;
    sun.shadow.mapSize.set(2048, 2048);
    sun.shadow.camera.left = -120;
    sun.shadow.camera.right = 120;
    sun.shadow.camera.top = 120;
    sun.shadow.camera.bottom = -120;
    scene.add(sun);

    const world = { width: 220, height: 220 };
    const garageArea = { x: -42, z: 46, w: 18, h: 12 };
    const missionTargets = [
      { x: -58, z: 56, name: "Garage Exit" },
      { x: 68, z: -58, name: "North Market" },
      { x: 82, z: 110, name: "East Station" },
      { x: -74, z: 112, name: "West Depot" }
    ];

    const roads = [
      { x: -110, z: -4, w: 220, d: 10 },
      { x: -110, z: 78, w: 220, d: 10 },
      { x: -110, z: -76, w: 220, d: 10 },
      { x: -110, z: 150, w: 220, d: 10 },
      { x: -2, z: -105, w: 10, d: 220 },
      { x: 72, z: -105, w: 10, d: 220 },
      { x: -76, z: -105, w: 10, d: 220 }
    ];

    const buildings = [
      { x: -80, z: -90, w: 24, d: 18, h: 18, color: 0x586d80 },
      { x: -25, z: -90, w: 22, d: 18, h: 15, color: 0x687f90 },
      { x: 35, z: -90, w: 20, d: 18, h: 16, color: 0x526c84 },
      { x: 82, z: -90, w: 26, d: 16, h: 18, color: 0x455e72 },

      { x: -92, z: -20, w: 18, d: 20, h: 18, color: 0x5a7588 },
      { x: 52, z: -6, w: 32, d: 18, h: 18, color: 0x60798d },
      { x: 92, z: -18, w: 24, d: 18, h: 15, color: 0x576e7d },

      { x: -90, z: 52, w: 26, d: 18, h: 18, color: 0x53677b },
      { x: -35, z: 82, w: 18, d: 18, h: 18, color: 0x6a788f },
      { x: 22, z: 82, w: 24, d: 18, h: 16, color: 0x4d647d },
      { x: 82, z: 76, w: 30, d: 20, h: 18, color: 0x6a7d90 },

      { x: -90, z: 113, w: 22, d: 18, h: 18, color: 0x5f7281 },
      { x: -10, z: 118, w: 20, d: 18, h: 18, color: 0x70819a },
      { x: 58, z: 118, w: 28, d: 18, h: 18, color: 0x607582 },
      { x: 96, z: 112, w: 22, d: 18, h: 18, color: 0x485e73 }
    ];

    const ui = {
      paint: document.getElementById("paintColor"),
      rims: document.getElementById("rimColor"),
      accent: document.getElementById("accentColor"),
      spoiler: document.getElementById("spoiler"),
      engine: document.getElementById("engine"),
      handling: document.getElementById("handling"),
      drift: document.getElementById("drift"),
      saveBtn: document.getElementById("saveBtn"),
      loadBtn: document.getElementById("loadBtn"),
      resetBtn: document.getElementById("resetBtn"),
      startBtn: document.getElementById("startBtn"),
      garageBtn: document.getElementById("garageBtn"),
      tuneBtn: document.getElementById("tuneBtn")
    };

    const defaultConfig = {
      paint: "#ff6b6b",
      rims: "#1f2933",
      neon: "#4cc9f0",
      spoiler: true,
      enginePower: 72,
      handling: 58,
      drift: 60
    };

    const slotNames = ["Slot 1", "Slot 2", "Slot 3", "Slot 4"];
    let selectedSlot = 0;
    let garageSlots = [];
    let gameStarted = false;
    let currentMission = 0;
    let messageTimer = 0;
    let wantedLevel = 0;
    let driftScore = 0;
    let driftActive = false;

    const customState = { ...defaultConfig };

    const player = {
      group: null,
      heading: 0,
      speed: 0,
      acceleration: 18,
      speedLimit: 28,
      turnStrength: 1.4,
      health: 100,
      radius: 2.2,
      driftCharge: 0
    };

    const keys = {};
    const npcCars = [];
    const policeCars = [];
    const npcRoutes = [
      [ {x: -90, z: -50}, {x: 90, z: -50}, {x: 90, z: 30}, {x: -90, z: 30} ],
      [ {x: 90, z: 50}, {x: -90, z: 50}, {x: -90, z: -30}, {x: 90, z: -30} ],
      [ {x: -20, z: 120}, {x: 20, z: 120}, {x: 20, z: -80}, {x: -20, z: -80} ],
      [ {x: 72, z: -100}, {x: 72, z: 100}, {x: -72, z: 100}, {x: -72, z: -100} ]
    ];

    function showMessage(text, ttl = 1800) {
      messageBox.textContent = text;
      messageBox.classList.add("show");
      messageTimer = performance.now() + ttl;
    }

    function clamp(value, min, max) {
      return Math.min(Math.max(value, min), max);
    }

    function isBlocked(x, z, radius) {
      for (const b of buildings) {
        const hitX = x + radius > b.x - b.w / 2 && x - radius < b.x + b.w / 2;
        const hitZ = z + radius > b.z - b.d / 2 && z - radius < b.z + b.d / 2;
        if (hitX && hitZ) return true;
      }

      const edge = world.width / 2 - 2;
      if (Math.abs(x) > edge || Math.abs(z) > edge) return true;
      return false;
    }

    function buildSlotButtons() {
      slotButtonsWrap.innerHTML = "";
      slotNames.forEach((slotName, idx) => {
        const btn = document.createElement("button");
        btn.className = "slot-btn" + (idx === selectedSlot ? " active" : "");
        btn.textContent = slotName;
        btn.addEventListener("click", () => {
          selectedSlot = idx;
          loadSelectedSlot();
          renderSlotButtons();
        });
        slotButtonsWrap.appendChild(btn);
      });
    }

    function renderSlotButtons() {
      const buttons = slotButtonsWrap.querySelectorAll(".slot-btn");
      buttons.forEach((btn, idx) => {
        btn.classList.toggle("active", idx === selectedSlot);
      });
    }

    function defaultGarageSlots() {
      return slotNames.map((_, idx) => ({
        name: slotNames[idx],
        paint: idx === 0 ? "#ff6b6b" : "#3ec