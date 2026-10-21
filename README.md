 <!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#0f5132">
<meta name="description" content="TransGuard interactive software proof of concept: simulated transformer tamper, overheating, power and connectivity events.">

<title>TransGuard Nigeria | Interactive Proof of Concept</title>

<style>
:root {
  --green:#147A42;
  --dark:#0F5132;
  --ink:#12281D;
  --muted:#4F6A5B;
  --line:#D2E4D8;
  --mist:#E9F4ED;
  --tile:#F3F8F5;
  --warn:#924F00;
  --warnbg:#FFF2D9;
  --red:#B42318;
  --redbg:#FDECEA;
  --blue:#185ABC;
  --bluebg:#EAF1FF;
}

* { box-sizing:border-box; }

html { scroll-behavior:smooth; }

body {
  margin:0;
  background:#fff;
  color:var(--ink);
  font:15px/1.5 system-ui,Arial,sans-serif;
}

button,select { font:inherit; }
button { cursor:pointer; }
button:disabled { opacity:.45;cursor:not-allowed; }

h1 {
  font-size:clamp(32px,5vw,52px);
  line-height:1.1;
  margin:10px 0;
  color:var(--dark);
}

h2 {
  font-size:26px;
  color:var(--dark);
  margin:0 0 12px;
}

h3 {
  font-size:15px;
  color:var(--dark);
  margin:0 0 10px;
}

.wrap {
  max-width:1360px;
  margin:auto;
  padding:0 20px;
}

header {
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:15px;
  flex-wrap:wrap;
  padding:15px 25px;
  border-bottom:3px solid var(--green);
}

.brand {
  color:var(--dark);
  font-size:23px;
  font-weight:800;
  text-decoration:none;
}

.brand small {
  display:block;
  font-size:11px;
  color:var(--muted);
  font-weight:500;
}

nav {
  display:flex;
  gap:15px;
  flex-wrap:wrap;
}

nav a {
  text-decoration:none;
  color:var(--dark);
  font-weight:700;
  font-size:13px;
}

.hero {
  padding:45px 0;
  background:linear-gradient(120deg,#E1F0E6,#fff);
}

.hero p {
  max-width:850px;
  font-size:18px;
}

.pills {
  display:flex;
  flex-wrap:wrap;
  gap:8px;
}

.pill {
  background:var(--warnbg);
  color:var(--warn);
  border-radius:30px;
  padding:5px 12px;
  font-size:12px;
  font-weight:800;
}

.pill.green {
  background:var(--green);
  color:white;
}

.btn {
  display:inline-block;
  border:1px solid var(--green);
  background:white;
  color:var(--dark);
  border-radius:8px;
  padding:10px 15px;
  font-weight:700;
  font-size:13px;
  min-height:42px;
  text-decoration:none;
}

.btn.primary {
  background:var(--green);
  color:white;
}

.btn.danger {
  color:var(--red);
  border-color:#E7BBB8;
}

.actions {
  display:flex;
  flex-wrap:wrap;
  gap:8px;
  margin-top:14px;
}

section { padding:30px 0; }

.console {
  background:var(--mist);
  padding:14px;
  border-radius:14px;
}

.toolbar {
  display:flex;
  align-items:center;
  flex-wrap:wrap;
  gap:9px;
  background:white;
  padding:11px;
  margin-bottom:11px;
  border:1px solid var(--line);
  border-radius:10px;
}

.scenario {
  padding:10px 13px;
  border:1px solid var(--line);
  background:white;
  color:var(--dark);
  border-radius:8px;
  font-weight:700;
}

.scenario.active {
  background:var(--green);
  color:white;
}

.grid {
  display:grid;
  grid-template-columns:190px minmax(0,1fr) 310px;
  gap:12px;
  align-items:start;
}

.stack {
  display:flex;
  flex-direction:column;
  gap:12px;
  min-width:0;
}

.card {
  background:white;
  border:1px solid var(--line);
  border-radius:10px;
  padding:14px;
  min-width:0;
}

.metrics {
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:7px;
}

.metric {
  padding:10px 5px;
  text-align:center;
  border-radius:8px;
  background:var(--tile);
}

.metric small {
  display:block;
  font-size:11px;
  color:var(--muted);
}

.metric b { font-size:16px; }

.metric.warning {
  background:var(--warnbg);
  color:var(--warn);
}

.metric.critical {
  background:var(--redbg);
  color:var(--red);
}

.two-col {
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:12px;
}

.status-rail {
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:8px;
  margin-top:15px;
}

.strip {
  background:var(--tile);
  padding:10px;
  border-radius:8px;
}

.strip small {
  display:block;
  color:var(--muted);
  font-size:11px;
}

.strip b { font-size:15px; }

.strip.critical { background:var(--redbg); }
.strip.warning { background:var(--warnbg); }

.kv {
  display:flex;
  justify-content:space-between;
  gap:8px;
  padding:7px 0;
  border-bottom:1px solid var(--line);
  font-size:13px;
}

.kv:last-child { border-bottom:0; }

.banner {
  padding:10px;
  border-radius:8px;
  margin:10px 0;
  font-weight:700;
  font-size:13px;
}

.banner.good {
  background:var(--tile);
  color:var(--dark);
}

.banner.warning {
  background:var(--warnbg);
  color:var(--warn);
}

.banner.critical {
  background:var(--redbg);
  color:var(--red);
}

.flow {
  display:flex;
  flex-wrap:wrap;
  gap:5px;
}

.step {
  padding:6px 9px;
  border:1px solid var(--line);
  border-radius:25px;
  font-size:11px;
}

.step.on {
  background:var(--green);
  color:white;
}

.incident {
  padding:10px;
  border:1px solid var(--line);
  border-left:4px solid var(--green);
  border-radius:7px;
  margin-bottom:8px;
}

.incident.warning { border-left-color:orange; }
.incident.critical { border-left-color:var(--red); }

.incident p {
  margin:5px 0;
  font-size:12px;
}

.incident small {
  font-size:11px;
  color:var(--muted);
}

.events {
  list-style:none;
  padding:0;
  max-height:300px;
  overflow:auto;
}

.events li {
  padding:7px 0;
  border-bottom:1px solid var(--line);
  font-size:12px;
}

.note {
  background:var(--tile);
  border-left:4px solid var(--green);
  padding:12px;
  border-radius:5px;
  margin:12px 0;
}

.tiny {
  font-size:12px;
  color:var(--muted);
}

.site-list {
  list-style:none;
  margin:12px 0;
  padding:0;
}

.site-list li {
  padding:8px;
  border-radius:6px;
  font-size:13px;
}

.site-list .current {
  background:#E3F2E8;
  font-weight:800;
}

.split {
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:15px;
}

table {
  width:100%;
  border-collapse:collapse;
}

th,td {
  border:1px solid var(--line);
  padding:10px;
  text-align:left;
  vertical-align:top;
}

th { background:var(--mist); }

footer {
  background:var(--dark);
  color:white;
  padding:28px 0;
  font-size:13px;
}

footer a { color:white; }

@media(max-width:1150px) {
  .grid { grid-template-columns:1fr 300px; }
  .sidebar { grid-column:1/-1; }
}

@media(max-width:850px) {
  .grid { grid-template-columns:1fr; }
}

@media(max-width:640px) {
  .wrap { padding:0 12px; }
  .two-col,.split { grid-template-columns:1fr; }
  .status-rail { grid-template-columns:1fr 1fr; }
  .hero { padding:28px 0; }
  .metrics { gap:5px; }
}
</style>
</head>

<body>

<header>
  <a href="#" class="brand">
    ⚡ TransGuard
    <small>Covenant Energy Technology • Nigeria</small>
  </a>

  <nav>
    <a href="#console">Interactive Console</a>
    <a href="#proof">Proof of Concept</a>
    <a href="#case">Community Context</a>
  </nav>
</header>

<main>

<div class="hero">
  <div class="wrap">

    <div class="pills">
      <span class="pill green">Interactive Proof of Concept</span>
      <span class="pill">SIMULATED DATA</span>
      <span class="pill">NO LIVE TRANSFORMER CONNECTED</span>
    </div>

    <h1>Protect transformers before damage becomes an outage.</h1>

    <p>
      TransGuard is a proposed grid-independent retrofit monitoring
      system for distribution transformers. This interactive simulation
      demonstrates tamper detection, thermal warnings, operator
      decisions and continuity during network outages.
    </p>

    <div class="actions">
      <a class="btn primary" href="#console">Run Interactive Simulation ↓</a>
      <a class="btn" href="#proof">What This Demonstrates</a>
    </div>

  </div>
</div>

<section id="console">
<div class="wrap">

<h2>Interactive Transformer Monitoring Console</h2>

<p class="tiny">
  Choose a scenario and press Play. All transformer readings,
  alarms, responder actions and incident records are simulated.
</p>

<div class="console">

  <div class="toolbar" id="scenarios"></div>

  <div class="toolbar">

    <button class="btn primary" id="play">▶ Play</button>

    <button class="btn" id="reset">Reset</button>

    <button class="btn" id="next">Next Milestone ⏭</button>

    <label>
      Speed
      <select id="speed">
        <option value="1">1×</option>
        <option value="2">2×</option>
        <option value="4">4×</option>
        <option value="8">8×</option>
      </select>
    </label>

    <strong id="clock"></strong>

  </div>

  <div class="note" id="instructions"></div>

  <div class="grid">

    <aside class="card sidebar">

      <h3>Transformer Site Registry</h3>

      <strong>TransGuard Nigeria</strong>

      <p class="tiny">
        One simulated site. Other entries are illustrative.
      </p>

      <ul class="site-list">
        <li class="current">🟢 LAG0024 • ACTIVE DEMO</li>
        <li>LAG0007 • Illustrative</li>
        <li>LAG0031 • Illustrative</li>
      </ul>

      <p class="tiny">All site readings are fictional.</p>

    </aside>

    <div class="stack">

      <div class="card">

        <h3>LAG0024 — Distribution Transformer</h3>

        <p class="tiny">
          Illustrative 300 kVA, 415 V transformer • Node TG-0024
        </p>

        <div class="status-rail">

          <div class="strip" id="st-power">
            <small>Grid Supply</small>
            <b id="grid-state">Online</b>
          </div>

          <div class="strip">
            <small>Solar Output</small>
            <b id="solar">0 W</b>
          </div>

          <div class="strip">
            <small>Battery</small>
            <b id="battery-head">94%</b>
          </div>

          <div class="strip" id="st-remote">
            <small>Remote Display</small>
            <b id="remote-age">Current</b>
          </div>

        </div>
      </div>

      <div class="two-col">

        <div class="card">
          <h3>Transformer Condition</h3>

          <div class="metrics">
            <div class="metric" id="oil-level">
              <small>Oil Level</small><b>82%</b>
            </div>

            <div class="metric" id="oil-temp">
              <small>Oil Temperature</small><b>58°C</b>
            </div>

            <div class="metric" id="asset-health">
              <small>Asset Status</small><b>Normal</b>
            </div>
          </div>
        </div>

        <div class="card">
          <h3>Electrical Readings</h3>

          <div class="metrics">
            <div class="metric" id="load">
              <small>Load</small><b>186 kVA</b>
            </div>

            <div class="metric" id="voltage">
              <small>Voltage</small><b>415 V</b>
            </div>

            <div class="metric" id="current">
              <small>Current</small><b>259 A</b>
            </div>
          </div>
        </div>

        <div class="card">
          <h3>Security Monitoring</h3>

          <div class="metrics">
            <div class="metric" id="enclosure">
              <small>Enclosure</small><b>Closed</b>
            </div>

            <div class="metric" id="vibration">
              <small>Vibration</small><b>Normal</b>
            </div>

            <div class="metric" id="maintenance">
              <small>Maintenance Permit</small><b>None</b>
            </div>
          </div>
        </div>

        <div class="card">
          <h3>Backup Power</h3>

          <div class="metrics">
            <div class="metric" id="batt">
              <small>Battery</small><b>94%</b>
            </div>

            <div class="metric" id="backup">
              <small>Estimated Backup</small><b>14.1 h</b>
            </div>

            <div class="metric" id="node">
              <small>Node Load</small><b>16 W</b>
            </div>
          </div>

          <p class="tiny">
            Illustrative battery model: 12.8 V × 25 Ah,
            75% usable capacity, approximately 240 Wh.
            Actual endurance requires physical testing.
          </p>
        </div>

      </div>

      <div class="card">

        <h3>Operator Response Workflow</h3>

        <div id="overall
