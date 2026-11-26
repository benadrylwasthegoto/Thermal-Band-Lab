# Thermal-Band-Lab
Interactive virtual lab showing why a conductor's resistance rises and a semiconductor's falls with temperature: atomic animation, energy band diagrams, a virtual experiment bench and live data fitting for α, B and band gap.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Thermal Band Lab</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans:wght@400;500;600&family=Space+Grotesk:wght@500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0a0e13; --panel:#10161e; --panel2:#141c26; --line:#1f2b38; --line2:#2a3949;
  --text:#e6edf3; --muted:#8b9bab; --dim:#5d6d7e;
  --cu:#ee9a5c; --cu2:#ffbe8a; --semi:#5ab0ff; --semi2:#9fd0ff; --hole:#ffb347;
  --good:#5ee0a0; --bad:#ff6b6b; --amber:#ffd166; --violet:#b48cff;
  --r:14px;
}
*{box-sizing:border-box}
html,body{margin:0;background:var(--bg);color:var(--text);font-family:"IBM Plex Sans",system-ui,sans-serif;font-size:14px;line-height:1.45}
body{background:radial-gradient(1200px 600px at 10% -10%,#16304a55,transparent 60%),radial-gradient(900px 500px at 100% 0%,#4a2a1655,transparent 60%),var(--bg);min-height:100vh}
h1,h2,h3{font-family:"Space Grotesk",sans-serif;margin:0;letter-spacing:-.01em}
.mono{font-family:"IBM Plex Mono",monospace}
button,select,input{font:inherit;color:inherit}
.wrap{max-width:1440px;margin:0 auto;padding:18px 20px 60px}

/* header */
.top{display:flex;align-items:center;justify-content:space-between;gap:16px;margin-bottom:14px;flex-wrap:wrap}
.brand{display:flex;gap:14px;align-items:center}
.logo{width:46px;height:46px;border-radius:12px;background:linear-gradient(135deg,var(--cu),#8a4b22 45%,#1d4f7a 55%,var(--semi));display:grid;place-items:center;box-shadow:0 6px 24px #0008}
.logo svg{width:30px;height:30px}
.brand h1{font-size:24px}
.brand p{margin:2px 0 0;color:var(--muted);font-size:13px}
.btn{background:var(--panel2);border:1px solid var(--line2);border-radius:10px;padding:8px 13px;cursor:pointer;transition:.15s;display:inline-flex;align-items:center;gap:7px;white-space:nowrap}
.btn:hover{border-color:#3d5266;background:#18222e}
.btn.primary{background:linear-gradient(180deg,#2a6fb0,#1f5791);border-color:#3b82c4}
.btn.primary:hover{background:linear-gradient(180deg,#3480c6,#2463a3)}
.btn.warm{background:linear-gradient(180deg,#b5652c,#94501f);border-color:#d07a3a}
.btn.on{border-color:var(--good);box-shadow:0 0 0 1px var(--good) inset;color:var(--good)}
.btn:disabled{opacity:.45;cursor:not-allowed}
.btn.sm{padding:5px 10px;font-size:12.5px;border-radius:8px}

/* control bar */
.control{display:grid;grid-template-columns:minmax(260px,1.4fr) auto auto;gap:18px;align-items:center;padding:14px 18px;margin-bottom:16px}
.card{background:linear-gradient(180deg,var(--panel),#0e141b);border:1px solid var(--line);border-radius:var(--r);box-shadow:0 10px 30px #0006}
.sliderbox label{display:flex;justify-content:space-between;color:var(--muted);font-size:12px;text-transform:uppercase;letter-spacing:.08em;margin-bottom:6px}
.sliderbox label b{color:var(--text);font-family:"IBM Plex Mono";font-size:13px;letter-spacing:0}
input[type=range]{-webkit-appearance:none;appearance:none;width:100%;height:8px;border-radius:8px;background:linear-gradient(90deg,#2f7fd8,#5ab0ff 25%,#ffd166 60%,#ff7a45 85%,#e8453c);outline:none}
input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:20px;height:20px;border-radius:50%;background:#fff;border:3px solid #0a0e13;box-shadow:0 0 0 2px #fff,0 2px 10px #000a;cursor:pointer}
input[type=range]::-moz-range-thumb{width:16px;height:16px;border-radius:50%;background:#fff;border:3px solid #0a0e13;cursor:pointer}
input.plain[type=range]{background:var(--line2);height:6px}
.readouts{display:flex;gap:10px}
.ro{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:6px 12px;min-width:104px}
.ro small{display:block;color:var(--muted);font-size:10.5px;text-transform:uppercase;letter-spacing:.08em}
.ro span{font-family:"IBM Plex Mono";font-size:19px;font-weight:500}
.seg{display:flex;background:#0b1117;border:1px solid var(--line);border-radius:11px;padding:3px;gap:2px}
.seg button{background:none;border:0;padding:7px 12px;border-radius:8px;cursor:pointer;color:var(--muted);display:flex;align-items:center;gap:6px;white-space:nowrap}
.seg button .dot{width:8px;height:8px;border-radius:50%}
.seg button.active{background:var(--panel2);color:var(--text);box-shadow:0 0 0 1px var(--line2) inset}
.ctrl-right{display:flex;flex-direction:column;gap:8px;align-items:flex-end}
.speed{display:flex;gap:6px;align-items:center;color:var(--muted);font-size:12px}
.speed select{background:#0b1117;border:1px solid var(--line);border-radius:8px;padding:4px 6px}

/* grid */
.grid{display:grid;grid-template-columns:1.25fr 1fr;gap:16px}
.span2{grid-column:1/-1}
.card-h{display:flex;justify-content:space-between;align-items:flex-start;padding:14px 16px 8px;gap:10px}
.card-h h2{font-size:16.5px;display:flex;gap:9px;align-items:center}
.num{display:inline-grid;place-items:center;width:22px;height:22px;border-radius:7px;background:#1b2734;border:1px solid var(--line2);font-size:12px;font-family:"IBM Plex Mono";color:var(--muted)}
.card-h p{margin:3px 0 0;color:var(--muted);font-size:12.5px}
.tag{font-size:11px;color:var(--dim);border:1px solid var(--line);border-radius:6px;padding:2px 7px;white-space:nowrap}
.cv{display:block;width:100%}
.legend{display:flex;flex-wrap:wrap;gap:14px;padding:6px 16px 0;color:var(--muted);font-size:12px}
.legend i{display:inline-block;width:10px;height:10px;border-radius:50%;margin-right:6px;vertical-align:-1px}
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;padding:10px 16px 16px}
.stat{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:8px 10px}
.stat small{color:var(--muted);font-size:11px;display:block}
.stat b{font-family:"IBM Plex Mono";font-weight:500;font-size:15px}
.stat .bar{height:4px;border-radius:3px;background:var(--line);margin-top:6px;overflow:hidden}
.stat .bar i{display:block;height:100%;background:var(--semi);width:0;transition:width .2s}
.note{margin:0 16px 16px;padding:10px 12px;border-left:3px solid var(--semi);background:#0d151e;border-radius:0 8px 8px 0;color:#c4d1dd;font-size:13px}
.note b{color:var(--text)}

/* bench */
.bench-body{display:grid;grid-template-columns:1fr 250px;gap:12px;padding:0 16px 16px}
.bench-tools{display:flex;flex-wrap:wrap;gap:8px;padding:4px 16px 12px}
.toggles{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-top:10px}
.tg{display:flex;align-items:center;gap:8px;background:#0b1117;border:1px solid var(--line);border-radius:9px;padding:7px 9px;cursor:pointer;font-size:12.5px;user-select:none}
.tg input{display:none}
.tg .sw{width:28px;height:16px;border-radius:9px;background:#26323f;position:relative;flex:none;transition:.15s}
.tg .sw::after{content:"";position:absolute;width:12px;height:12px;border-radius:50%;background:#8b9bab;top:2px;left:2px;transition:.15s}
.tg input:checked + .sw{background:#2b5f47}
.tg input:checked + .sw::after{left:14px;background:var(--good)}
.tg span.t small{display:block;color:var(--dim);font-size:10.5px}
.logwrap{background:#0b1117;border:1px solid var(--line);border-radius:10px;display:flex;flex-direction:column;min-height:0}
.logwrap h3{font-size:12px;color:var(--muted);text-transform:uppercase;letter-spacing:.08em;padding:9px 10px 6px;font-family:"IBM Plex Sans";font-weight:500;display:flex;justify-content:space-between}
.logscroll{overflow:auto;flex:1;max-height:300px}
table{border-collapse:collapse;width:100%}
.log td,.log th{font-family:"IBM Plex Mono";font-size:11.5px;padding:3px 8px;text-align:right;border-top:1px solid #16202a}
.log th{color:var(--dim);font-weight:500;position:sticky;top:0;background:#0b1117}
.log td:first-child,.log th:first-child{text-align:left}
.empty{color:var(--dim);font-size:12px;padding:14px 10px;text-align:center}

/* alarm */
.alarm-body{padding:0 16px 16px;display:grid;gap:12px}
.alarm-svg{width:100%;height:auto;background:#0b1117;border:1px solid var(--line);border-radius:10px}
.alarm-row{display:flex;gap:10px;align-items:center}
.alarm-row label{color:var(--muted);font-size:12px;white-space:nowrap}
.alarm-row b{font-family:"IBM Plex Mono";min-width:58px;text-align:right}
.apps{display:flex;flex-wrap:wrap;gap:6px}
.apps span{font-size:11.5px;color:var(--muted);border:1px solid var(--line);border-radius:20px;padding:3px 9px}

/* analysis */
.runchips{display:flex;flex-wrap:wrap;gap:6px;padding:0 16px 10px}
.chip{display:flex;align-items:center;gap:6px;border:1px solid var(--line2);border-radius:20px;padding:3px 10px 3px 7px;font-size:12px;cursor:pointer;background:#0b1117;user-select:none}
.chip i{width:9px;height:9px;border-radius:50%}
.chip.off{opacity:.4;text-decoration:line-through}
.plots{display:grid;grid-template-columns:1fr 1fr;gap:12px;padding:0 16px}
.plot{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:8px 8px 4px}
.plot h3{font-size:13px;font-family:"IBM Plex Sans";font-weight:600;padding:2px 6px 0;display:flex;justify-content:space-between;align-items:baseline;gap:8px}
.plot h3 span{color:var(--muted);font-weight:400;font-size:11.5px;font-family:"IBM Plex Mono"}
.plot canvas{height:250px}
.summary{display:grid;grid-template-columns:1.5fr 1fr;gap:12px;padding:14px 16px 16px}
.sum td,.sum th{padding:7px 10px;border-top:1px solid var(--line);text-align:right;font-size:12.5px}
.sum th{color:var(--muted);font-weight:500;font-size:11.5px;text-transform:uppercase;letter-spacing:.05em;border-top:0}
.sum td:first-child,.sum th:first-child{text-align:left}
.sum td{font-family:"IBM Plex Mono"}
.sum td:first-child{font-family:"IBM Plex Sans"}
.sumbox{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:4px 4px 6px;overflow-x:auto}
.ok{color:var(--good)} .warn{color:var(--amber)} .badc{color:var(--bad)}
.insights{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:12px 14px}
.insights h3{font-size:13px;margin-bottom:6px}
.insights ul{margin:0;padding-left:18px;color:#c4d1dd;font-size:12.8px}
.insights li{margin:5px 0}

/* drawer */
.drawer{position:fixed;top:0;right:0;height:100vh;width:380px;max-width:92vw;background:#0f151d;border-left:1px solid var(--line2);box-shadow:-20px 0 50px #000a;transform:translateX(105%);transition:transform .3s ease;z-index:50;display:flex;flex-direction:column}
.drawer.open{transform:none}
.drawer header{padding:16px 18px 10px;border-bottom:1px solid var(--line);display:flex;justify-content:space-between;align-items:center}
.drawer header h2{font-size:17px}
.dots{display:flex;gap:6px;padding:12px 18px 0}
.dots i{flex:1;height:4px;border-radius:3px;background:var(--line2)}
.dots i.done{background:var(--good)} .dots i.cur{background:var(--semi)}
.gbody{padding:14px 18px;overflow:auto;flex:1}
.gbody h3{font-size:19px;margin-bottom:4px}
.gbody .kicker{color:var(--semi);font-size:11.5px;text-transform:uppercase;letter-spacing:.12em;font-weight:600}
.gbody p{color:#c4d1dd}
.q{background:#0b1117;border:1px solid var(--line);border-radius:10px;padding:10px 12px;margin:10px 0}
.q b{display:block;margin-bottom:8px;font-weight:500}
.choices{display:flex;gap:6px;flex-wrap:wrap}
.choice{border:1px solid var(--line2);background:#121a23;border-radius:8px;padding:6px 11px;cursor:pointer}
.choice.sel{border-color:var(--semi);background:#16304a}
.choice.right{border-color:var(--good);background:#143326}
.choice.wrong{border-color:var(--bad);background:#3a1919}
.feedback{margin-top:8px;font-size:13px;color:#c4d1dd}
.gfoot{padding:12px 18px;border-top:1px solid var(--line);display:flex;justify-content:space-between}
.check{display:flex;gap:8px;align-items:center;color:var(--muted);font-size:13px;margin-top:10px}
.check i{width:16px;height:16px;border-radius:50%;border:2px solid var(--line2);display:inline-block}
.check.done{color:var(--good)} .check.done i{background:var(--good);border-color:var(--good)}
.pulse{animation:pulse 1.4s ease 2}
@keyframes pulse{0%,100%{box-shadow:0 10px 30px #0006}50%{box-shadow:0 0 0 3px var(--semi),0 10px 30px #0006}}

.modal{position:fixed;inset:0;background:#000a;display:none;place-items:center;z-index:60}
.modal.open{display:grid}
.modal .box{width:min(620px,92vw);background:#0f151d;border:1px solid var(--line2);border-radius:14px;padding:16px}
.modal textarea{width:100%;height:280px;background:#0b1117;color:var(--text);border:1px solid var(--line);border-radius:8px;font-family:"IBM Plex Mono";font-size:12px;padding:10px;margin:10px 0}
.toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%) translateY(30px);background:#18222e;border:1px solid var(--line2);padding:9px 16px;border-radius:10px;opacity:0;transition:.25s;z-index:70;pointer-events:none}
.toast.show{opacity:1;transform:translateX(-50%)}
footer{color:var(--dim);font-size:12px;text-align:center;margin-top:24px}

@media (max-width:1100px){
  .grid{grid-template-columns:1fr}
  .control{grid-template-columns:1fr}
  .ctrl-right{align-items:flex-start}
  .summary{grid-template-columns:1fr}
}
@media (max-width:720px){
  .plots{grid-template-columns:1fr}
  .bench-body{grid-template-columns:1fr}
  .stats{grid-template-columns:repeat(2,1fr)}
  .readouts{flex-wrap:wrap}
  .seg{flex-wrap:wrap}
  .toggles{grid-template-columns:1fr}
}
</style>
</head>
<body>
<div class="wrap">

<header class="top">
  <div class="brand">
    <div class="logo"><svg viewBox="0 0 30 30" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round"><path d="M4 9h22M4 21h22"/><path d="M9 21v-4M15 21l0-6M21 21v-3" stroke-opacity=".7"/><circle cx="15" cy="9" r="2.4" fill="#fff"/></svg></div>
    <div>
      <h1>Thermal Band Lab</h1>
      <p>Temperature dependence of resistance in conductors and semiconductors · experiment + band-theory simulation</p>
    </div>
  </div>
  <div style="display:flex;gap:8px">
    <button class="btn primary" id="guideBtn">▶ Guided mode</button>
  </div>
</header>

<section class="card control">
  <div class="sliderbox">
    <label>Water-bath setpoint <b id="tsetVal">25.0 °C</b></label>
    <input type="range" id="tset" min="0" max="100" step="0.5" value="25" aria-label="Water bath setpoint temperature">
    <div style="display:flex;justify-content:space-between;color:var(--dim);font-size:11px;margin-top:4px" class="mono"><span>0 °C ice</span><span>50 °C</span><span>100 °C boiling</span></div>
  </div>
  <div class="readouts">
    <div class="ro"><small>Thermometer</small><span id="roBath">25.0</span><small style="display:inline;margin-left:3px">°C</small></div>
    <div class="ro"><small>Sample temp</small><span id="roSample">25.0</span><small style="display:inline;margin-left:3px">°C</small></div>
    <div class="ro"><small>Readings</small><span id="roN">0</span><small style="display:inline;margin-left:3px" id="roRun">run 1</small></div>
  </div>
  <div class="ctrl-right">
    <div class="seg" id="matSeg" role="tablist" aria-label="Material shown in the visual panels">
      <button data-m="cu" class="active"><span class="dot" style="background:var(--cu)"></span>Copper</button>
      <button data-m="th"><span class="dot" style="background:var(--semi)"></span>NTC thermistor</button>
      <button data-m="si"><span class="dot" style="background:#7fd1c7"></span>Silicon</button>
      <button data-m="ge"><span class="dot" style="background:var(--violet)"></span>Germanium</button>
    </div>
    <div class="speed">Sim speed
      <select id="speed"><option value="0.5">½×</option><option value="1" selected>1×</option><option value="2">2×</option><option value="4">4×</option></select>
    </div>
  </div>
</section>

<main class="grid">

  <!-- 1 atomic -->
  <section class="card" id="cardAtom">
    <div class="card-h">
      <div><h2><span class="num">1</span><span id="atomTitle">Inside copper: electrons and a vibrating lattice</span></h2>
      <p id="atomSub">Free electrons drift through the lattice of Cu⁺ ions. The hotter it is, the more the ions vibrate, so electrons scatter more often.</p></div>
      <span class="tag">motion exaggerated</span>
    </div>
    <canvas id="atom" class="cv" style="height:330px"></canvas>
    <div class="legend" id="atomLegend"></div>
    <div class="stats">
      <div class="stat"><small id="s1l">Free electrons</small><b id="s1">—</b></div>
      <div class="stat"><small id="s2l">Scattering rate</small><b id="s2">—</b><div class="bar"><i id="s2b"></i></div></div>
      <div class="stat"><small id="s3l">Drift / current</small><b id="s3">—</b><div class="bar"><i id="s3b"></i></div></div>
      <div class="stat"><small id="s4l">Resistance</small><b id="s4">—</b></div>
    </div>
  </section>

  <!-- 2 bands -->
  <section class="card" id="cardBand">
    <div class="card-h">
      <div><h2><span class="num">2</span>Energy band diagram</h2>
      <p>Left: allowed energy bands. Right: the Fermi–Dirac probability f(E) that a state at energy E is occupied at the current temperature.</p></div>
      <span class="tag mono" id="kTtag">kT = 25.7 meV</span>
    </div>
    <canvas id="band" class="cv" style="height:330px"></canvas>
    <div class="note" id="bandNote"></div>
  </section>

  <!-- 3 bench -->
  <section class="card" id="cardBench">
    <div class="card-h">
      <div><h2><span class="num">3</span>Virtual experiment bench</h2>
      <p>A copper coil and an NTC thermistor sit in the same water bath. Take a reading about every 5 °C between 5 and 80 °C, and repeat for 2–3 runs.</p></div>
      <span class="tag">Phase 1</span>
    </div>
    <div class="bench-tools">
      <button class="btn warm" id="recBtn">● Record reading</button>
      <button class="btn" id="autoRecBtn">Auto-record every 5 °C</button>
      <button class="btn primary" id="sweepBtn">⟳ Auto sweep 5 → 80 °C</button>
      <button class="btn" id="newRunBtn">+ New run</button>
      <button class="btn" id="undoBtn">↶ Undo</button>
      <button class="btn" id="clearBtn">Clear all</button>
    </div>
    <div class="bench-body">
      <div>
        <canvas id="bench" class="cv" style="aspect-ratio:640/360"></canvas>
        <div class="toggles">
          <label class="tg"><input type="checkbox" id="eNoise" checked><span class="sw"></span><span class="t">Meter noise<small>±0.2–0.4 % random scatter</small></span></label>
          <label class="tg"><input type="checkbox" id="eLag"><span class="sw"></span><span class="t">Thermal lag<small>sample trails the thermometer</small></span></label>
          <label class="tg"><input type="checkbox" id="eSelf"><span class="sw"></span><span class="t">Self-heating<small>measuring current warms the thermistor</small></span></label>
          <label class="tg"><input type="checkbox" id="eContact"><span class="sw"></span><span class="t">Contact resistance<small>+0.25 Ω from leads and clips</small></span></label>
          <label class="tg" style="grid-column:1/-1"><input type="checkbox" id="eCool"><span class="sw"></span><span class="t">Also record a cooling run in auto sweep<small>80 → 5 °C as a separate run (shows hysteresis when lag is on)</small></span></label>
        </div>
      </div>
      <div class="logwrap">
        <h3>Data log <span id="logCount" class="mono">0</span></h3>
        <div class="logscroll" id="logScroll"><div class="empty" id="logEmpty">No readings yet.<br>Press <b>Record</b> or run an <b>auto sweep</b>.</div>
          <table class="log" id="logTable" style="display:none"><thead><tr><th>Run</th><th>T °C</th><th>R<sub>Cu</sub> Ω</th><th>R<sub>NTC</sub> Ω</th></tr></thead><tbody></tbody></table></div>
        <div style="display:flex;gap:6px;padding:8px;border-top:1px solid var(--line);flex-wrap:wrap">
          <label class="btn sm" style="cursor:pointer">⇪ Import CSV<input type="file" id="csvIn" accept=".csv,.txt" style="display:none"></label>
          <button class="btn sm" id="exportBtn">⧉ Copy CSV</button>
        </div>
      </div>
    </div>
  </section>

  <!-- alarm -->
  <section class="card" id="cardAlarm">
    <div class="card-h">
      <div><h2><span class="num">★</span>Application: overheat / fire alarm</h2>
      <p>The NTC sits in a voltage divider. As it heats, its resistance drops, V<sub>out</sub> rises, and a comparator trips the alarm when V<sub>out</sub> passes V<sub>ref</sub>.</p></div>
    </div>
    <div class="alarm-body">
      <svg class="alarm-svg" id="alarmSvg" viewBox="0 0 420 210"></svg>
      <div class="alarm-row"><label>Trip temperature</label><input type="range" class="plain" id="alarmT" min="35" max="95" step="1" value="60"><b id="alarmTv">60 °C</b></div>
      <div class="apps"><span>🌡 Digital thermometers</span><span>🔥 Fire / overheat alarms</span><span>🔋 Battery-pack protection</span><span>⚡ Inrush-current limiters</span><span>☀ Solar cells and diodes use the same band physics</span></div>
    </div>
  </section>

  <!-- 4 analysis -->
  <section class="card span2" id="cardAnalysis">
    <div class="card-h">
      <div><h2><span class="num">4</span>Analysis: measured vs simulated</h2>
      <p>Solid lines are the band-theory model. Dots are your readings, coloured by run. Dashed lines are best fits to your data. Click a run to include it in or leave it out of the fits.</p></div>
      <span class="tag">Phase 2</span>
    </div>
    <div class="runchips" id="runChips"></div>
    <div class="plots">
      <div class="plot"><h3>Copper coil: R vs T <span id="pA">R = R₂₀[1 + α(T − 20)]</span></h3><canvas id="plotA" class="cv"></canvas></div>
      <div class="plot"><h3>NTC thermistor: R vs T <span id="pB">R = R₂₅ e^{B(1/T − 1/T₂₅)}</span></h3><canvas id="plotB" class="cv"></canvas></div>
      <div class="plot"><h3>Thermistor linearised: ln R vs 1/T <span id="pC">slope = B</span></h3><canvas id="plotC" class="cv"></canvas></div>
      <div class="plot"><h3>Carrier concentration vs T <span>n<sub>i</sub> ∝ T<sup>3/2</sup> e<sup>−E<sub>g</sub>/2kT</sup></span></h3><canvas id="plotD" class="cv"></canvas></div>
    </div>
    <div class="summary">
      <div class="sumbox"><table class="sum" id="sumTable"></table></div>
      <div class="insights"><h3>What your data says</h3><ul id="insights"></ul></div>
    </div>
  </section>
</main>

<footer>Model: copper α = 0.0039 /°C, R₂₀ = 10.00 Ω · NTC R₂₅ = 10.0 kΩ, B = 3950 K · Si E<sub>g</sub> = 1.12 eV, Ge E<sub>g</sub> = 0.67 eV · k = 8.617 × 10⁻⁵ eV/K</footer>
</div>

<!-- guided drawer -->
<aside class="drawer" id="guide" aria-label="Guided mode">
  <header><h2>Guided mode · Predict → Observe → Explain</h2><button class="btn sm" id="guideClose">✕</button></header>
  <div class="dots" id="gDots"></div>
  <div class="gbody" id="gBody"></div>
  <div class="gfoot"><button class="btn" id="gPrev">← Back</button><button class="btn primary" id="gNext">Next →</button></div>
</aside>

<div class="modal" id="modal"><div class="box">
  <h3 style="font-size:16px">Your data as CSV</h3>
  <p style="color:var(--muted);margin:4px 0 0;font-size:13px">It's been copied to your clipboard if your browser allowed it. If not, select all and copy from the box. Paste it into Excel, Google Sheets or your Python script (pandas.read_csv).</p>
  <textarea id="csvOut" readonly></textarea>
  <div style="display:flex;justify-content:flex-end;gap:8px"><button class="btn" id="modalSel">Select all</button><button class="btn primary" id="modalClose">Done</button></div>
</div></div>
<div class="toast" id="toast"></div>

<script>
(()=>{
"use strict";
/* ============ physics ============ */
const kB=8.617333e-5, C2K=273.15, Q=1.602e-19;
const CU={R20:10.0,a:0.0039};
const TH={R25:10000,B:3950};
const SEMI={si:{name:'Silicon',Eg:1.12,n300:1.0e10,mu:1350+480},ge:{name:'Germanium',Eg:0.67,n300:2.0e13,mu:3900+1900}};
const NCU=8.5e22;
const EG_TH=2*kB*TH.B;
const Rcu=T=>CU.R20*(1+CU.a*(T-20));
const Rth=T=>TH.R25*Math.exp(TH.B*(1/(T+C2K)-1/298.15));
function ni(m,T){const s=SEMI[m],Tk=T+C2K;return s.n300*Math.pow(Tk/300,1.5)*Math.exp(-s.Eg/(2*kB)*(1/Tk-1/300));}
const rho=(m,T)=>1/(ni(m,T)*Q*SEMI[m].mu);
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
const lerp=(a,b,t)=>a+(b-a)*t;
const rnd=(a,b)=>a+Math.random()*(b-a);
function gauss(){let u=0,v=0;while(!u)u=Math.random();while(!v)v=Math.random();return Math.sqrt(-2*Math.log(u))*Math.cos(2*Math.PI*v);}
function fmtR(v,d=3){if(!isFinite(v))return '—';const a=Math.abs(v);if(a>=1e9)return (v/1e9).toPrecision(d)+' GΩ';if(a>=1e6)return (v/1e6).toPrecision(d)+' MΩ';if(a>=1e3)return (v/1e3).toPrecision(d)+' kΩ';return v.toPrecision(d+1)+' Ω';}
function sci(v,d=2){if(!isFinite(v)||v<=0)return '—';const e=Math.floor(Math.log10(v));const m=v/Math.pow(10,e);return m.toFixed(d)+'×10'+sup(e);}
function sup(n){const m={'-':'⁻','0':'⁰','1':'¹','2':'²','3':'³','4':'⁴','5':'⁵','6':'⁶','7':'⁷','8':'⁸','9':'⁹'};return String(n).split('').map(c=>m[c]).join('');}
function carrierFrac(m,T){
  const f=t=>m==='th'?-TH.B/(t+C2K):Math.log(ni(m,t));
  return clamp((f(T)-f(0))/(f(100)-f(0)),0,1);
}
const egOf=m=>m==='si'?1.12:m==='ge'?0.67:m==='th'?EG_TH:0;
const MATCOL={cu:'#ee9a5c',th:'#5ab0ff',si:'#7fd1c7',ge:'#b48cff'};

/* ============ state ============ */
const S={
  tset:25,Tbath:25,Tsample:25,Tth:25,speed:1,mat:'cu',
  err:{noise:true,lag:false,self:false,contact:false,cool:false},
  autoRec:false,run:1,runs:{},records:[],
  sweep:null,lastBathForRec:25,
  meter:{cu:Rcu(25),th:Rth(25),t:0},
  alarmT:60,dirtyPlots:true
};
const RUNCOLS=['#5ab0ff','#ee9a5c','#5ee0a0','#ffd166','#b48cff','#ff6b9a','#7fd1c7','#c2c9d1'];
function runInfo(id){
  if(!S.runs[id]){const n=Object.keys(S.runs).length;S.runs[id]={id,label:id==='csv'?'Your data (CSV)':'Run '+id,color:id==='csv'?'#ffffff':RUNCOLS[n%RUNCOLS.length],on:true};}
  return S.runs[id];
}

/* ============ canvas helper ============ */
const DPR=()=>Math.min(window.devicePixelRatio||1,2);
function fit(c){const r=c.getBoundingClientRect();const d=DPR();const w=Math.max(10,Math.round(r.width)),h=Math.max(10,Math.round(r.height));
  if(c.width!==Math.round(w*d)||c.height!==Math.round(h*d)){c.width=Math.round(w*d);c.height=Math.round(h*d);c._resized=true;}
  const ctx=c.getContext('2d');ctx.setTransform(d,0,0,d,0,0);return {ctx,w,h};}

/* ============ atomic sims ============ */
class CopperSim{
  constructor(){this.w=0;this.h=0;}
  init(w,h){this.w=w;this.h=h;this.sp=46;const sp=this.sp;this.ions=[];
    const nx=Math.ceil(w/sp)+1,ny=Math.ceil(h/sp);this.ox=(w-(nx-1)*sp)/2;this.oy=(h-(ny-1)*sp)/2;
    for(let j=0;j<ny;j++)for(let i=0;i<nx;i++)this.ions.push({x0:this.ox+i*sp,y0:this.oy+j*sp,x:0,y:0,p1:Math.random()*6.28,p2:Math.random()*6.28,f1:rnd(5,8),f2:rnd(5,8),flash:0});
    this.nx=nx;this.ny=ny;this.el=[];
    for(let k=0;k<44;k++){const a=Math.random()*6.28;this.el.push({x:rnd(0,w),y:rnd(0,h),vx:Math.cos(a)*140,vy:Math.sin(a)*140,tr:[]});}
    this.t=0;this.coll=0;this.collRate=0;this.drift=0;}
  step(dt,T){
    this.t+=dt;const A=1.2+8.5*clamp(T,0,100)/100;
    for(const io of this.ions){io.x=io.x0+A*Math.sin(this.t*io.f1+io.p1);io.y=io.y0+A*Math.cos(this.t*io.f2+io.p2);io.flash=Math.max(0,io.flash-dt*3);}
    const ratio=Rcu(T)/Rcu(0);const G=2.4*(1+3*(ratio-1));this.G=G;
    const acc=95,vth=140;let sumvx=0;
    for(const e of this.el){
      e.vx+=acc*dt;
      if(Math.random()<G*dt){const a=Math.random()*6.28;e.vx=Math.cos(a)*vth;e.vy=Math.sin(a)*vth;this.coll++;
        const i=clamp(Math.round((e.x-this.ox)/this.sp),0,this.nx-1),j=clamp(Math.round((e.y-this.oy)/this.sp),0,this.ny-1);const io=this.ions[j*this.nx+i];if(io){io.flash=1;e.fx=io.x;e.fy=io.y;e.ft=1;}}
      e.x+=e.vx*dt;e.y+=e.vy*dt;sumvx+=e.vx;
      let wr=false;if(e.x>this.w){e.x-=this.w;wr=true}if(e.x<0){e.x+=this.w;wr=true}if(e.y>this.h){e.y-=this.h;wr=true}if(e.y<0){e.y+=this.h;wr=true}
      if(wr)e.tr=[];e.tr.push(e.x,e.y);if(e.tr.length>16)e.tr.splice(0,2);
      if(e.ft)e.ft=Math.max(0,e.ft-dt*4);
    }
    const vd=sumvx/this.el.length;this.drift=lerp(this.drift,vd,clamp(dt*1.5,0,1));
    this.collAcc=(this.collAcc||0)+dt;if(this.collAcc>=0.5){this.collRate=lerp(this.collRate||this.coll/this.collAcc,this.coll/this.collAcc,0.5);this.coll=0;this.collAcc=0;}
  }
  draw(ctx){
    const w=this.w,h=this.h;ctx.fillStyle='#0b1117';ctx.fillRect(0,0,w,h);
    // sea glow
    const g=ctx.createLinearGradient(0,0,w,h);g.addColorStop(0,'#0f1c2a');g.addColorStop(1,'#1a140e');ctx.fillStyle=g;ctx.fillRect(0,0,w,h);
    for(const io of this.ions){
      const rg=ctx.createRadialGradient(io.x-2,io.y-2,1,io.x,io.y,11);rg.addColorStop(0,'#ffd2ad');rg.addColorStop(.5,'#d9783a');rg.addColorStop(1,'#6b3412');
      ctx.fillStyle=rg;ctx.beginPath();ctx.arc(io.x,io.y,9,0,6.283);ctx.fill();
      if(io.flash>0){ctx.strokeStyle=`rgba(255,240,200,${io.flash})`;ctx.lineWidth=2;ctx.beginPath();ctx.arc(io.x,io.y,10+(1-io.flash)*10,0,6.283);ctx.stroke();}
      ctx.fillStyle='#3a1a06';ctx.font='600 8px IBM Plex Mono';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText('+',io.x,io.y+.5);
    }
    for(const e of this.el){
      if(e.tr.length>3){ctx.strokeStyle='rgba(90,176,255,.25)';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(e.tr[0],e.tr[1]);for(let k=2;k<e.tr.length;k+=2)ctx.lineTo(e.tr[k],e.tr[k+1]);ctx.stroke();}
      ctx.shadowColor='#5ab0ff';ctx.shadowBlur=8;ctx.fillStyle='#bfe2ff';ctx.beginPath();ctx.arc(e.x,e.y,3,0,6.283);ctx.fill();ctx.shadowBlur=0;
    }
    drawFieldArrow(ctx,w,h,'electrons drift →');
  }
}
function drawFieldArrow(ctx,w,h,txt){
  ctx.save();ctx.fillStyle='rgba(10,14,19,.75)';roundRect(ctx,10,h-34,186,24,7);ctx.fill();
  ctx.strokeStyle='#ffd166';ctx.fillStyle='#ffd166';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(52,h-22);ctx.lineTo(20,h-22);ctx.stroke();
  ctx.beginPath();ctx.moveTo(18,h-22);ctx.lineTo(26,h-27);ctx.lineTo(26,h-17);ctx.fill();
  ctx.font='500 11px IBM Plex Sans';ctx.textAlign='left';ctx.textBaseline='middle';ctx.fillText('E-field',58,h-22);ctx.fillStyle='#8b9bab';ctx.fillText(txt,100,h-22);ctx.restore();
}
function roundRect(ctx,x,y,w,h,r){ctx.beginPath();ctx.moveTo(x+r,y);ctx.arcTo(x+w,y,x+w,y+h,r);ctx.arcTo(x+w,y+h,x,y+h,r);ctx.arcTo(x,y+h,x,y,r);ctx.arcTo(x,y,x+w,y,r);ctx.closePath();}

class SemiSim{
  constructor(){this.w=0;}
  init(w,h,mat){this.w=w;this.h=h;this.mat=mat;const sp=54;this.sp=sp;
    const nx=Math.floor(w/sp)+1,ny=Math.floor(h/sp)+1;this.nx=nx;this.ny=ny;const ox=(w-(nx-1)*sp)/2,oy=(h-(ny-1)*sp)/2;
    this.atoms=[];for(let j=0;j<ny;j++)for(let i=0;i<nx;i++)this.atoms.push({x0:ox+i*sp,y0:oy+j*sp,x:0,y:0,p:Math.random()*6.28,f:rnd(5,8),flash:0});
    this.bonds=[];const idx=(i,j)=>j*nx+i;
    for(let j=0;j<ny;j++)for(let i=0;i<nx;i++){if(i<nx-1)this.bonds.push({a:idx(i,j),b:idx(i+1,j),h:true,hole:null,i,j});if(j<ny-1)this.bonds.push({a:idx(i,j),b:idx(i,j+1),h:false,hole:null,i,j});}
    const byAtom=this.atoms.map(()=>[]);this.bonds.forEach((b,k)=>{byAtom[b.a].push(k);byAtom[b.b].push(k);});
    this.bonds.forEach((b,k)=>{const s=new Set([...byAtom[b.a],...byAtom[b.b]]);s.delete(k);b.nb=[...s];});
    // wrap targets: leftmost horizontal bonds -> rightmost in same row
    this.bonds.forEach(b=>{if(b.h&&b.i===0){b.wrap=this.bonds.findIndex(c=>c.h&&c.j===b.j&&c.i===nx-2);}});
    this.el=[];this.holes=[];this.flashes=[];this.t=0;this.events=[];this.genAcc=0;this.churn=0;
  }
  bondMid(b){const A=this.atoms[b.a],B=this.atoms[b.b];return [(A.x+B.x)/2,(A.y+B.y)/2];}
  gen(){const free=this.bonds.map((b,k)=>k).filter(k=>!this.bonds[k].hole);if(!free.length)return;const k=free[Math.floor(Math.random()*free.length)];const b=this.bonds[k];
    const [x,y]=this.bondMid(b);const ho={bond:k,x,y,next:rnd(.2,.6)};b.hole=ho;this.holes.push(ho);
    const a=Math.random()*6.28;this.el.push({x,y,vx:Math.cos(a)*90,vy:Math.sin(a)*90,tr:[]});this.flashes.push({x,y,t:1,c:'90,176,255'});this.events.push('gen');}
  rec(){if(!this.el.length||!this.holes.length)return;const e=this.el.shift();const ho=this.holes.shift();this.bonds[ho.bond].hole=null;this.flashes.push({x:ho.x,y:ho.y,t:1,c:'255,179,71'});this.flashes.push({x:e.x,y:e.y,t:.7,c:'255,179,71'});this.events.push('rec');}
  step(dt,T){
    this.t+=dt;const A=0.6+3*clamp(T,0,100)/100;
    for(const a of this.atoms){a.x=a.x0+A*Math.sin(this.t*a.f+a.p);a.y=a.y0+A*Math.cos(this.t*a.f*1.13+a.p);}
    const target=Math.round(2+38*carrierFrac(this.mat,T));this.target=target;const n=this.el.length;
    if(n<target){this.genAcc+=dt*(1.2+(target-n)*0.9);while(this.genAcc>1&&this.el.length<target){this.gen();this.genAcc-=1;}}
    else if(n>target){this.genAcc+=dt*(1.2+(n-target)*0.9);while(this.genAcc>1&&this.el.length>target){this.rec();this.genAcc-=1;}}
    else this.genAcc=0;
    // dynamic equilibrium churn
    this.churn+=dt*0.06*n;if(this.churn>1){this.churn-=1;if(n>1){this.rec();this.gen();}}
    for(const e of this.el){e.vx+=gauss()*260*dt;e.vy+=gauss()*260*dt;e.vx*=1-1.2*dt;e.vy*=1-1.2*dt;e.vx+=40*dt;
      e.x+=e.vx*dt;e.y+=e.vy*dt;let wr=false;if(e.x>this.w){e.x-=this.w;wr=true}if(e.x<0){e.x+=this.w;wr=true}if(e.y>this.h){e.y-=this.h;wr=true}if(e.y<0){e.y+=this.h;wr=true}
      if(wr)e.tr=[];e.tr.push(e.x,e.y);if(e.tr.length>14)e.tr.splice(0,2);}
    for(const ho of this.holes){ho.next-=dt;if(ho.next<=0){ho.next=rnd(.25,.7);const b=this.bonds[ho.bond];
        let opts=b.nb.filter(k=>!this.bonds[k].hole);
        if(b.wrap!==undefined&&b.wrap>=0&&Math.random()<.35&&!this.bonds[b.wrap].hole){opts=[b.wrap];}
        if(opts.length){const [bx]=this.bondMid(b);const ws=opts.map(k=>{const [x]=this.bondMid(this.bonds[k]);return x<bx-2?2.6:x>bx+2?0.6:1;});
          let s=ws.reduce((p,c)=>p+c,0),r=Math.random()*s,pick=opts[0];for(let q=0;q<opts.length;q++){r-=ws[q];if(r<=0){pick=opts[q];break;}}
          b.hole=null;this.bonds[pick].hole=ho;const jump=Math.abs(this.bondMid(this.bonds[pick])[0]-bx)>this.sp*2;ho.bond=pick;if(jump){const [x,y]=this.bondMid(this.bonds[pick]);ho.x=x;ho.y=y;}}}
      const [tx,ty]=this.bondMid(this.bonds[ho.bond]);ho.x=lerp(ho.x,tx,clamp(dt*10,0,1));ho.y=lerp(ho.y,ty,clamp(dt*10,0,1));}
    this.flashes=this.flashes.filter(f=>(f.t-=dt*2)>0);
  }
  draw(ctx){
    const w=this.w,h=this.h;const g=ctx.createLinearGradient(0,0,w,h);g.addColorStop(0,'#0c1620');g.addColorStop(1,'#101a24');ctx.fillStyle=g;ctx.fillRect(0,0,w,h);
    const col=MATCOL[this.mat];
    ctx.lineWidth=2;
    for(const b of this.bonds){const A=this.atoms[b.a],B=this.atoms[b.b];ctx.strokeStyle='rgba(120,150,180,.22)';ctx.beginPath();ctx.moveTo(A.x,A.y);ctx.lineTo(B.x,B.y);ctx.stroke();
      const mx=(A.x+B.x)/2,my=(A.y+B.y)/2;const ox=b.h?0:4,oy=b.h?4:0;ctx.fillStyle='#8cc6ff';
      if(!b.hole){ctx.beginPath();ctx.arc(mx-ox*0+ (b.h?-5:0),my+(b.h?0:-5),2.2,0,6.283);ctx.fill();}
      ctx.beginPath();ctx.arc(mx+(b.h?5:0),my+(b.h?0:5),2.2,0,6.283);ctx.fill();}
    for(const a of this.atoms){const rg=ctx.createRadialGradient(a.x-3,a.y-3,1,a.x,a.y,14);rg.addColorStop(0,'#ffffff');rg.addColorStop(.35,col);rg.addColorStop(1,'#122233');ctx.fillStyle=rg;ctx.beginPath();ctx.arc(a.x,a.y,11,0,6.283);ctx.fill();
      ctx.fillStyle='#0a0e13';ctx.font='600 8.5px IBM Plex Mono';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText(this.mat==='si'?'Si':this.mat==='ge'?'Ge':'M',a.x,a.y+.5);}
    for(const f of this.flashes){ctx.strokeStyle=`rgba(${f.c},${f.t})`;ctx.lineWidth=2;ctx.beginPath();ctx.arc(f.x,f.y,6+(1-f.t)*18,0,6.283);ctx.stroke();}
    for(const ho of this.holes){ctx.strokeStyle='#ffb347';ctx.lineWidth=2.2;ctx.shadowColor='#ffb347';ctx.shadowBlur=8;ctx.beginPath();ctx.arc(ho.x,ho.y,5,0,6.283);ctx.stroke();ctx.shadowBlur=0;}
    for(const e of this.el){if(e.tr.length>3){ctx.strokeStyle='rgba(90,176,255,.3)';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(e.tr[0],e.tr[1]);for(let k=2;k<e.tr.length;k+=2)ctx.lineTo(e.tr[k],e.tr[k+1]);ctx.stroke();}
      ctx.shadowColor='#5ab0ff';ctx.shadowBlur=10;ctx.fillStyle='#d6ecff';ctx.beginPath();ctx.arc(e.x,e.y,3.6,0,6.283);ctx.fill();ctx.shadowBlur=0;}
    drawFieldArrow(ctx,w,h,'e⁻ →   holes ←');
  }
}

/* ============ band diagram ============ */
class BandSim{
  constructor(){this.cb=[];this.vh=[];this.jumps=[];}
  reset(){this.cb=[];this.vh=[];this.jumps=[];}
  gen(){this.jumps.push({x0:Math.random(),x1:Math.random(),t:0,up:true,off:-Math.log(Math.random()+1e-9)});}
  rec(){if(!this.cb.length)return;const c=this.cb.shift();this.vh.shift();this.jumps.push({x0:c.x,x1:Math.random(),t:0,up:false,off:c.off});}
  step(dt){for(const j of this.jumps){j.t+=dt*1.8;if(j.t>=1&&j.up&&!j.done){j.done=true;this.cb.push({x:j.x1,off:j.off,ph:Math.random()*6});this.vh.push({x:j.x0,off:-Math.log(Math.random()+1e-9),ph:Math.random()*6});}}
    this.jumps=this.jumps.filter(j=>j.t<1);}
  draw(ctx,w,h,mat,T,t){
    ctx.fillStyle='#0b1117';ctx.fillRect(0,0,w,h);
    const Tk=T+C2K,kT=kB*Tk;const L=40,R=w*0.6,top=18,bot=h-26,H=bot-top;const Emin=-1.9,Emax=1.9;
    const y=E=>top+(Emax-E)/(Emax-Emin)*H;
    // energy axis
    ctx.strokeStyle='#2a3949';ctx.lineWidth=1;ctx.beginPath();ctx.moveTo(L-10,bot);ctx.lineTo(L-10,top);ctx.stroke();
    ctx.fillStyle='#2a3949';ctx.beginPath();ctx.moveTo(L-10,top-6);ctx.lineTo(L-14,top+2);ctx.lineTo(L-6,top+2);ctx.fill();
    ctx.save();ctx.translate(14,(top+bot)/2);ctx.rotate(-Math.PI/2);ctx.fillStyle='#8b9bab';ctx.font='11px IBM Plex Sans';ctx.textAlign='center';ctx.fillText('Electron energy E (eV)',0,0);ctx.restore();
    const bw=R-L;
    if(mat==='cu'){
      // overlapping bands
      const y3a=y(-0.9),y3b=y(-1.85);ctx.fillStyle='rgba(180,140,255,.18)';ctx.fillRect(L+bw*.08,y3a,bw*.5,y3b-y3a);ctx.strokeStyle='rgba(180,140,255,.5)';ctx.strokeRect(L+bw*.08,y3a,bw*.5,y3b-y3a);
      const ya=y(1.7),yb=y(-1.8);ctx.strokeStyle='rgba(238,154,92,.6)';ctx.lineWidth=1.2;ctx.strokeRect(L+bw*.3,ya,bw*.66,yb-ya);
      // filled part with fuzzy edge
      const yF=y(0);const fz=Math.max(2,kT*10/(Emax-Emin)*H);
      const gr=ctx.createLinearGradient(0,yF-fz*2,0,yF+fz*2);gr.addColorStop(0,'rgba(238,154,92,0)');gr.addColorStop(.5,'rgba(238,154,92,.45)');gr.addColorStop(1,'rgba(238,154,92,.75)');
      ctx.fillStyle='rgba(238,154,92,.75)';ctx.fillRect(L+bw*.3,yF+fz*2,bw*.66,yb-yF-fz*2);ctx.fillStyle=gr;ctx.fillRect(L+bw*.3,yF-fz*2,bw*.66,fz*4);
      // electrons near EF jittering
      ctx.fillStyle='#ffe3c9';const nEx=Math.round(4+T/8);
      for(let k=0;k<nEx;k++){const px=L+bw*.33+((k*0.6180339)%1)*bw*.6;const o=Math.sin(t*2+k)*fz*1.6;ctx.beginPath();ctx.arc(px,yF+o,2.2,0,6.283);ctx.fill();}
      ctx.setLineDash([5,4]);ctx.strokeStyle='#ffd166';ctx.beginPath();ctx.moveTo(L,yF);ctx.lineTo(R+6,yF);ctx.stroke();ctx.setLineDash([]);
      ctx.font='600 11px IBM Plex Mono';ctx.fillStyle='#ffd166';ctx.textAlign='left';ctx.fillText('E_F',L+2,yF-6);
      ctx.fillStyle='#ffbe8a';ctx.font='12px IBM Plex Sans';ctx.fillText('4s conduction band (half filled)',L+bw*.32,ya+16);
      ctx.fillStyle='#c9b3ff';ctx.fillText('3d band (full)',L+bw*.1,y3b-8);
      ctx.fillStyle='#8b9bab';ctx.font='11px IBM Plex Sans';ctx.fillText('No band gap: bands overlap',L+bw*.32,ya+32);
      ctx.fillText('edge blur ×10 for visibility',L+bw*.32,yF+fz*2+18);
      // right: linear f(E) zoomed
      this.fPlotLinear(ctx,R+28,w-12,top,bot,T);
      return;
    }
    const Eg=egOf(mat),half=Eg/2;const col=MATCOL[mat];
    const yC=y(half),yV=y(-half);
    // CB
    let gr=ctx.createLinearGradient(0,y(Emax),0,yC);gr.addColorStop(0,'rgba(90,176,255,.04)');gr.addColorStop(1,'rgba(90,176,255,.16)');ctx.fillStyle=gr;ctx.fillRect(L,y(Emax),bw,yC-y(Emax));
    ctx.strokeStyle='rgba(90,176,255,.7)';ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(L,yC);ctx.lineTo(R,yC);ctx.stroke();
    // VB
    gr=ctx.createLinearGradient(0,yV,0,bot);gr.addColorStop(0,'rgba(127,170,210,.55)');gr.addColorStop(1,'rgba(60,90,120,.35)');ctx.fillStyle=gr;ctx.fillRect(L,yV,bw,bot-yV);
    ctx.strokeStyle='rgba(160,200,240,.8)';ctx.beginPath();ctx.moveTo(L,yV);ctx.lineTo(R,yV);ctx.stroke();
    ctx.fillStyle='rgba(220,236,255,.25)';for(let yy=yV+6;yy<bot;yy+=7)for(let xx=L+5+((yy/7)%2)*3.5;xx<R;xx+=7){ctx.fillRect(xx,yy,1.6,1.6);}
    // gap
    ctx.fillStyle='rgba(255,255,255,.02)';ctx.fillRect(L,yC,bw,yV-yC);
    const ax=R-34;ctx.strokeStyle='#e6edf3';ctx.lineWidth=1;ctx.beginPath();ctx.moveTo(ax,yC+3);ctx.lineTo(ax,yV-3);ctx.stroke();
    ctx.beginPath();ctx.moveTo(ax,yC+2);ctx.lineTo(ax-4,yC+9);ctx.lineTo(ax+4,yC+9);ctx.closePath();ctx.fill();ctx.beginPath();ctx.moveTo(ax,yV-2);ctx.lineTo(ax-4,yV-9);ctx.lineTo(ax+4,yV-9);ctx.closePath();ctx.fillStyle='#e6edf3';ctx.fill();
    ctx.font='600 12px IBM Plex Mono';ctx.fillStyle='#e6edf3';ctx.textAlign='right';ctx.fillText('E_g = '+Eg.toFixed(2)+' eV',ax-8,(yC+yV)/2+(mat==='ge'||mat==='th'?-6:4));
    if(mat==='th'){ctx.font='10.5px IBM Plex Sans';ctx.fillStyle='#8b9bab';ctx.fillText('effective, from B',ax-8,(yC+yV)/2+8);}
    ctx.setLineDash([5,4]);ctx.strokeStyle='#ffd166';ctx.beginPath();ctx.moveTo(L,y(0));ctx.lineTo(R+6,y(0));ctx.stroke();ctx.setLineDash([]);
    ctx.textAlign='left';ctx.font='600 11px IBM Plex Mono';ctx.fillStyle='#ffd166';ctx.fillText('E_F',L+4,y(0)-5);
    ctx.font='12px IBM Plex Sans';ctx.fillStyle='#9fd0ff';ctx.fillText('Conduction band',L+6,y(Emax)+16);ctx.fillStyle='#d6e6f5';ctx.fillText('Valence band (full of bonding electrons)',L+6,bot-8);
    const pxE=H/(Emax-Emin),ex=kT*8*pxE;
    // carriers
    for(const c of this.cb){const px=L+8+c.x*(bw-60);const py=yC-4-c.off*ex+Math.sin(t*3+c.ph)*1.5;ctx.shadowColor='#5ab0ff';ctx.shadowBlur=8;ctx.fillStyle='#d6ecff';ctx.beginPath();ctx.arc(px,py,3.4,0,6.283);ctx.fill();ctx.shadowBlur=0;}
    for(const v of this.vh){const px=L+8+v.x*(bw-60);const py=yV+5+v.off*ex*0.8;ctx.fillStyle='#0b1117';ctx.beginPath();ctx.arc(px,py,4,0,6.283);ctx.fill();ctx.strokeStyle='#ffb347';ctx.lineWidth=2;ctx.beginPath();ctx.arc(px,py,4,0,6.283);ctx.stroke();}
    for(const j of this.jumps){const e=j.t*(2-j.t);const x0=L+8+j.x0*(bw-60),x1=L+8+j.x1*(bw-60);let ya2=j.up?yV+3:yC-4,yb2=j.up?yC-4-j.off*ex:yV+3;
      const px=lerp(x0,x1,e),py=lerp(ya2,yb2,e)-Math.sin(e*Math.PI)*12;
      ctx.strokeStyle=j.up?'rgba(90,176,255,.55)':'rgba(255,179,71,.55)';ctx.lineWidth=1.5;ctx.setLineDash([3,3]);ctx.beginPath();ctx.moveTo(x0,ya2);ctx.quadraticCurveTo((x0+x1)/2,(ya2+yb2)/2-24,px,py);ctx.stroke();ctx.setLineDash([]);
      ctx.fillStyle=j.up?'#d6ecff':'#ffd7a0';ctx.beginPath();ctx.arc(px,py,3.4,0,6.283);ctx.fill();}
    this.fPlotLog(ctx,R+28,w-12,top,bot,y,half,T);
  }
  fPlotLog(ctx,x0,x1,top,bot,y,half,T){
    const lmin=-26;const X=lf=>x0+(lf-lmin)/(-lmin)*(x1-x0);
    ctx.strokeStyle='#1f2b38';ctx.lineWidth=1;ctx.strokeRect(x0,top,x1-x0,bot-top);
    ctx.font='10px IBM Plex Mono';ctx.fillStyle='#5d6d7e';ctx.textAlign='center';
    for(const v of [-20,-10,0]){const xx=X(v);ctx.strokeStyle='#16202a';ctx.beginPath();ctx.moveTo(xx,top);ctx.lineTo(xx,bot);ctx.stroke();ctx.fillText(v===0?'1':'10'+sup(v),xx,bot+12);}
    ctx.fillStyle='#8b9bab';ctx.font='10.5px IBM Plex Sans';ctx.fillText('occupancy f(E), log scale',(x0+x1)/2,bot+24);
    const curve=(Tc,style,lw)=>{const kT=kB*(Tc+C2K);ctx.strokeStyle=style;ctx.lineWidth=lw;ctx.beginPath();let first=true;
      for(let E=-1.9;E<=1.9;E+=0.01){const z=E/kT;let lf=z>30?-z/Math.LN10:-Math.log10(1+Math.exp(z));lf=Math.max(lmin,lf);const px=X(lf),py=y(E);if(first){ctx.moveTo(px,py);first=false}else ctx.lineTo(px,py);}ctx.stroke();};
    ctx.save();ctx.beginPath();ctx.rect(x0,top,x1-x0,bot-top);ctx.clip();
    ctx.fillStyle='rgba(90,176,255,.06)';ctx.fillRect(x0,top,x1-x0,y(half)-top);
    curve(0,'rgba(90,176,255,.35)',1);curve(100,'rgba(255,122,69,.4)',1);curve(T,'#e6edf3',2.2);
    const kT=kB*(T+C2K);const lfc=-(half/kT)/Math.LN10;const mx=X(Math.max(lmin,lfc)),my=y(half);
    ctx.fillStyle='#5ab0ff';ctx.beginPath();ctx.arc(mx,my,4.5,0,6.283);ctx.fill();ctx.restore();
    ctx.font='600 11px IBM Plex Mono';ctx.fillStyle='#9fd0ff';ctx.textAlign='left';
    const lbl='f(E_c) ≈ 10'+sup(Math.round(lfc*10)/10).replace('.','·');
    ctx.fillText('f(E_c) ≈ 10^'+lfc.toFixed(1),Math.min(mx+8,x1-112),my-8);
    ctx.font='10px IBM Plex Sans';ctx.fillStyle='rgba(90,176,255,.8)';ctx.fillText('0 °C',x0+4,top+12);ctx.fillStyle='rgba(255,122,69,.9)';ctx.fillText('100 °C',x0+34,top+12);ctx.fillStyle='#e6edf3';ctx.fillText('now',x0+76,top+12);
  }
  fPlotLinear(ctx,x0,x1,top,bot,T){
    const span=0.25;const yy=E=>top+(span-E)/(2*span)*(bot-top);const X=f=>x0+f*(x1-x0);
    ctx.strokeStyle='#1f2b38';ctx.lineWidth=1;ctx.strokeRect(x0,top,x1-x0,bot-top);
    ctx.font='10px IBM Plex Mono';ctx.fillStyle='#5d6d7e';ctx.textAlign='center';[0,.5,1].forEach(v=>ctx.fillText(v,X(v),bot+12));
    ctx.fillStyle='#8b9bab';ctx.font='10.5px IBM Plex Sans';ctx.fillText('f(E), zoom ±0.25 eV about E_F',(x0+x1)/2,bot+24);
    const curve=(Tc,s,lw)=>{const kT=kB*(Tc+C2K);ctx.strokeStyle=s;ctx.lineWidth=lw;ctx.beginPath();for(let E=-span;E<=span+1e-9;E+=0.0025){const f=1/(1+Math.exp(E/kT));const px=X(f),py=yy(E);E===-span?ctx.moveTo(px,py):ctx.lineTo(px,py);}ctx.stroke();};
    curve(0,'rgba(90,176,255,.45)',1);curve(100,'rgba(255,122,69,.5)',1);curve(T,'#e6edf3',2.2);
    ctx.setLineDash([4,4]);ctx.strokeStyle='#ffd166';ctx.beginPath();ctx.moveTo(x0,yy(0));ctx.lineTo(x1,yy(0));ctx.stroke();ctx.setLineDash([]);
    const kT=kB*(T+C2K);ctx.fillStyle='#8b9bab';ctx.font='10.5px IBM Plex Sans';ctx.textAlign='left';
    ctx.fillText('Only states within a few kT',x0+6,top+14);ctx.fillText('of E_F change, so the number',x0+6,top+27);ctx.fillText('of free electrons stays fixed',x0+6,top+40);
  }
}

/* ============ bench drawing ============ */
function drawBench(ctx,w,h,t){
  const s=w/640;ctx.save();ctx.scale(s,s);const W=640,H=360;
  ctx.fillStyle='#0b1117';ctx.fillRect(0,0,W,H);
  const g=ctx.createLinearGradient(0,0,0,H);g.addColorStop(0,'#0e1720');g.addColorStop(1,'#0b1117');ctx.fillStyle=g;ctx.fillRect(0,0,W,H);
  // table
  ctx.fillStyle='#1a2430';ctx.fillRect(0,318,W,42);ctx.fillStyle='#24313f';ctx.fillRect(0,316,W,3);
  const Tb=S.Tbath;const heating=S.tset>Tb+0.6,cooling=S.tset<Tb-0.6;
  // hot plate
  ctx.fillStyle='#222c38';roundRect(ctx,150,304,260,14,4);ctx.fill();
  if(heating){ctx.fillStyle=`rgba(255,90,40,${0.35+0.25*Math.sin(t*6)})`;roundRect(ctx,160,306,240,6,3);ctx.fill();}
  // beaker
  const bx=168,by=110,bw=224,bh=194;const wl=146;
  const tt=clamp(Tb/100,0,1);const hue=lerp(212,368,tt)%360;
  ctx.fillStyle=`rgba(${Math.round(lerp(43,200,tt))},${Math.round(lerp(120,70,tt))},${Math.round(lerp(200,60,tt))},.42)`;ctx.fillRect(bx+3,wl,bw-6,by+bh-wl-3);
  const sh=ctx.createLinearGradient(bx,0,bx+bw,0);sh.addColorStop(0,'rgba(255,255,255,.07)');sh.addColorStop(.15,'rgba(255,255,255,0)');sh.addColorStop(.85,'rgba(255,255,255,0)');sh.addColorStop(1,'rgba(255,255,255,.06)');ctx.fillStyle=sh;ctx.fillRect(bx,by,bw,bh);
  ctx.strokeStyle=`hsla(${hue},70%,70%,.6)`;ctx.lineWidth=1.5;ctx.beginPath();for(let x=bx+3;x<=bx+bw-3;x+=4){const yy=wl+Math.sin(x*.08+t*2)*1.2;x===bx+3?ctx.moveTo(x,yy):ctx.lineTo(x,yy);}ctx.stroke();
  ctx.strokeStyle='#9fb3c6';ctx.lineWidth=2.5;ctx.beginPath();ctx.moveTo(bx-6,by-4);ctx.lineTo(bx,by);ctx.lineTo(bx,by+bh);ctx.lineTo(bx+bw,by+bh);ctx.lineTo(bx+bw,by);ctx.stroke();
  ctx.strokeStyle='rgba(159,179,198,.5)';ctx.lineWidth=1;for(let k=1;k<=4;k++){ctx.beginPath();ctx.moveTo(bx+bw-22,by+bh-k*36);ctx.lineTo(bx+bw-8,by+bh-k*36);ctx.stroke();}
  // bubbles
  if(Tb>78){const n=Math.round((Tb-78)/2.2);for(let k=0;k<n;k++){const ph=(t*0.7+k*0.37)%1;const xx=bx+20+((k*73)%(bw-40));const yy=by+bh-8-ph*(by+bh-wl-10);ctx.strokeStyle='rgba(255,255,255,.5)';ctx.lineWidth=1;ctx.beginPath();ctx.arc(xx+Math.sin(t*5+k)*2,yy,1.5+ph*2.5,0,6.283);ctx.stroke();}}
  // steam
  if(Tb>55){ctx.strokeStyle=`rgba(220,230,240,${clamp((Tb-55)/60,0,.4)})`;ctx.lineWidth=2;for(let k=0;k<4;k++){const xx=bx+40+k*45;ctx.beginPath();for(let q=0;q<30;q++){const yy=wl-8-q*2.2;const ph=t*2+k;ctx.lineTo(xx+Math.sin(q*.25+ph)*5,yy);}ctx.stroke();}}
  // ice
  if(Tb<14||(cooling&&S.tset<20)){const n=3;for(let k=0;k<n;k++){const xx=bx+28+k*58+Math.sin(t+k)*3,yy=wl-6+Math.sin(t*1.3+k)*1.5;const sz=14+(k%2)*4;ctx.save();ctx.translate(xx,yy);ctx.rotate(.2*k-.2);ctx.fillStyle='rgba(210,240,255,.55)';ctx.strokeStyle='rgba(255,255,255,.8)';ctx.lineWidth=1;roundRect(ctx,-sz/2,-sz/2,sz,sz,3);ctx.fill();ctx.stroke();ctx.restore();}}
  // kettle
  if(heating){ctx.save();ctx.translate(70,40);ctx.rotate(.45);ctx.fillStyle='#b8c4cf';roundRect(ctx,-30,-22,60,44,10);ctx.fill();ctx.fillStyle='#8b9bab';ctx.fillRect(-24,-30,48,8);ctx.strokeStyle='#8b9bab';ctx.lineWidth=5;ctx.beginPath();ctx.arc(-34,0,12,Math.PI*.5,Math.PI*1.5);ctx.stroke();
    ctx.fillStyle='#b8c4cf';ctx.beginPath();ctx.moveTo(28,-6);ctx.lineTo(52,-18);ctx.lineTo(54,-12);ctx.lineTo(30,6);ctx.fill();ctx.restore();
    ctx.strokeStyle='rgba(150,200,255,.55)';ctx.lineWidth=3;ctx.beginPath();ctx.moveTo(116,46);ctx.quadraticCurveTo(140,70,176,wl);ctx.stroke();
    ctx.fillStyle='#ff9a6a';ctx.font='600 11px IBM Plex Sans';ctx.textAlign='left';ctx.fillText('heating…',30,98);}
  if(cooling){ctx.fillStyle='#9fd0ff';ctx.font='600 11px IBM Plex Sans';ctx.textAlign='left';ctx.fillText('adding ice…',30,98);}
  // copper coil
  const cx0=196,cx1=262,cy=262;ctx.lineWidth=3;
  for(let k=0;k<9;k++){const x=cx0+k*(cx1-cx0)/8;ctx.strokeStyle='#8a4a1f';ctx.beginPath();ctx.ellipse(x,cy,5,22,0,Math.PI*.5,Math.PI*1.5);ctx.stroke();}
  for(let k=0;k<9;k++){const x=cx0+k*(cx1-cx0)/8;ctx.strokeStyle='#ee9a5c';ctx.beginPath();ctx.ellipse(x,cy,5,22,0,-Math.PI*.5,Math.PI*.5);ctx.stroke();}
  ctx.strokeStyle='#ee9a5c';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(cx0,cy-22);ctx.lineTo(cx0,96);ctx.moveTo(cx1+5,cy-22);ctx.lineTo(cx1+5,96);ctx.stroke();
  ctx.fillStyle='#ffbe8a';ctx.font='600 10.5px IBM Plex Sans';ctx.textAlign='center';ctx.fillText('copper coil',(cx0+cx1)/2,300);
  // thermistor
  const tx=310,ty=272;ctx.strokeStyle='#9fb3c6';ctx.lineWidth=1.6;ctx.beginPath();ctx.moveTo(tx-3,ty);ctx.lineTo(tx-3,96);ctx.moveTo(tx+3,ty);ctx.lineTo(tx+3,96);ctx.stroke();
  const tg=ctx.createRadialGradient(tx-2,ty+2,1,tx,ty+4,9);tg.addColorStop(0,'#6db6ff');tg.addColorStop(1,'#123456');ctx.fillStyle=tg;ctx.beginPath();ctx.ellipse(tx,ty+5,7,9,0,0,6.283);ctx.fill();
  ctx.fillStyle='#9fd0ff';ctx.fillText('NTC',tx,300);
  // thermometer
  const thx=356;ctx.fillStyle='rgba(230,240,250,.12)';roundRect(ctx,thx-6,36,12,250,6);ctx.fill();ctx.strokeStyle='rgba(230,240,250,.5)';ctx.lineWidth=1;roundRect(ctx,thx-6,36,12,250,6);ctx.stroke();
  ctx.fillStyle='#e8453c';ctx.beginPath();ctx.arc(thx,290,10,0,6.283);ctx.fill();
  const colTop=280-clamp(Tb,0,105)/105*228;ctx.fillRect(thx-2.5,colTop,5,284-colTop);
  ctx.fillStyle='rgba(230,240,250,.5)';ctx.font='8.5px IBM Plex Mono';ctx.textAlign='left';
  for(let v=0;v<=100;v+=20){const yy=280-v/105*228;ctx.fillRect(thx+6,yy,5,1);ctx.fillText(v,thx+13,yy+3);}
  // digital readout
  ctx.fillStyle='#071016';roundRect(ctx,thx-38,8,92,24,6);ctx.fill();ctx.strokeStyle='#2a3949';ctx.stroke();
  ctx.fillStyle='#ff8a6a';ctx.font='600 14px IBM Plex Mono';ctx.textAlign='center';ctx.fillText(S.Tbath.toFixed(1)+' °C',thx+8,25);
  // meters
  meter(ctx,14,130,128,170,'COPPER COIL','Ω',S.meter.cu,'#ee9a5c',2);
  meter(ctx,498,130,128,170,'NTC THERMISTOR','kΩ',S.meter.th/1000,'#5ab0ff',3);
  // wires
  wire(ctx,cx0,96,92,148,'#e74c3c');wire(ctx,cx1+5,96,112,148,'#222');
  wire(ctx,tx-3,96,546,148,'#e74c3c');wire(ctx,tx+3,96,566,148,'#222');
  if(S.err.contact){ctx.fillStyle='#ffd166';ctx.font='10px IBM Plex Sans';ctx.textAlign='center';ctx.fillText('oxidised clips',150,90);}
  ctx.restore();
}
function wire(ctx,x0,y0,x1,y1,c){ctx.strokeStyle=c==='#222'?'#4a5562':c;ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(x0,y0);ctx.bezierCurveTo(x0,y0-50,x1,y1-60,x1,y1);ctx.stroke();ctx.fillStyle='#ccc';ctx.beginPath();ctx.arc(x0,y0,2.5,0,6.283);ctx.fill();}
function meter(ctx,x,y,w,h,label,unit,val,col,dec){
  ctx.fillStyle='#1d2631';roundRect(ctx,x,y,w,h,12);ctx.fill();ctx.strokeStyle=col;ctx.globalAlpha=.6;ctx.lineWidth=1.5;roundRect(ctx,x,y,w,h,12);ctx.stroke();ctx.globalAlpha=1;
  ctx.fillStyle='#0c1a12';roundRect(ctx,x+10,y+30,w-20,42,6);ctx.fill();
  ctx.fillStyle='#7dffb2';ctx.font='600 22px IBM Plex Mono';ctx.textAlign='right';ctx.fillText(val.toFixed(dec),x+w-16,y+60);
  ctx.fillStyle='#4fbf80';ctx.font='10px IBM Plex Mono';ctx.fillText(unit,x+w-16,y+70);
  ctx.fillStyle='#8b9bab';ctx.font='600 9.5px IBM Plex Sans';ctx.textAlign='center';ctx.fillText(label,x+w/2,y+20);
  ctx.strokeStyle='#3a4757';ctx.lineWidth=6;ctx.beginPath();ctx.arc(x+w/2,y+112,22,0,6.283);ctx.stroke();ctx.fillStyle='#2a3542';ctx.beginPath();ctx.arc(x+w/2,y+112,16,0,6.283);ctx.fill();
  ctx.strokeStyle='#e6edf3';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(x+w/2,y+112);ctx.lineTo(x+w/2+11,y+101);ctx.stroke();
  ctx.fillStyle='#8b9bab';ctx.font='9px IBM Plex Mono';ctx.fillText('Ω',x+w/2+28,y+94);
  ctx.fillStyle='#e74c3c';ctx.beginPath();ctx.arc(x+w/2-16,y+152,5,0,6.283);ctx.fill();ctx.fillStyle='#4a5562';ctx.beginPath();ctx.arc(x+w/2+16,y+152,5,0,6.283);ctx.fill();
}

/* ============ measurement ============ */
function selfHeat(T){if(!S.err.self)return 0;const R=Rth(T),Rs=10000,V=5;const P=V*V*R/((R+Rs)*(R+Rs));return P/0.0008;}
function trueReadings(){return {cu:Rcu(S.Tsample)+(S.err.contact?0.25:0),th:Rth(S.Tth)+(S.err.contact?0.25:0)};}
function measure(){const r=trueReadings();let cu=r.cu,th=r.th,T=S.Tbath;
  if(S.err.noise){cu*=1+gauss()*0.002;th*=1+gauss()*0.004;T+=gauss()*0.12;}return {T,cu,th};}

/* ============ records ============ */
const logBody=document.querySelector('#logTable tbody');
function addRecord(rec){S.records.push(rec);runInfo(rec.run);S.dirtyPlots=true;renderLog();renderChips();}
function record(){const m=measure();addRecord({run:S.run,T:m.T,cu:m.cu,th:m.th,src:'sim'});}
function renderLog(){
  const has=S.records.length>0;document.getElementById('logTable').style.display=has?'':'none';document.getElementById('logEmpty').style.display=has?'none':'';
  document.getElementById('logCount').textContent=S.records.length;document.getElementById('roN').textContent=S.records.length;
  logBody.innerHTML=S.records.slice().reverse().map(r=>{const ri=runInfo(r.run);return `<tr><td><span style="color:${ri.color}">●</span> ${r.run==='csv'?'CSV':r.run}</td><td>${r.T.toFixed(1)}</td><td>${r.cu!=null&&isFinite(r.cu)?r.cu.toFixed(3):'—'}</td><td>${r.th!=null&&isFinite(r.th)?Math.round(r.th):'—'}</td></tr>`}).join('');
}
function renderChips(){
  const el=document.getElementById('runChips');const ids=[...new Set(S.records.map(r=>r.run))];
  if(!ids.length){el.innerHTML='<span style="color:var(--dim);font-size:12px">No runs yet. The plots show the model only.</span>';return;}
  el.innerHTML=ids.map(id=>{const r=runInfo(id);const n=S.records.filter(x=>x.run===id).length;return `<span class="chip ${r.on?'':'off'}" data-run="${id}"><i style="background:${r.color}"></i>${r.label} · ${n}</span>`}).join('');
  el.querySelectorAll('.chip').forEach(c=>c.onclick=()=>{const id=c.dataset.run;const key=id==='csv'?'csv':Number(id);runInfo(key).on=!runInfo(key).on;S.dirtyPlots=true;renderChips();});
}
function activeRecords(){return S.records.filter(r=>runInfo(r.run).on);}

/* ============ fits ============ */
function linfit(xs,ys){const n=xs.length;if(n<2)return null;let sx=0,sy=0,sxx=0,sxy=0;for(let i=0;i<n;i++){sx+=xs[i];sy+=ys[i];sxx+=xs[i]*xs[i];sxy+=xs[i]*ys[i];}
  const d=n*sxx-sx*sx;if(Math.abs(d)<1e-18)return null;const m=(n*sxy-sx*sy)/d,c=(sy-m*sx)/n;return {m,c};}
function r2(ys,pred){const mean=ys.reduce((a,b)=>a+b,0)/ys.length;let ss=0,sr=0;ys.forEach((y,i)=>{ss+=(y-mean)**2;sr+=(y-pred[i])**2;});return ss>0?1-sr/ss:NaN;}
function computeFits(){
  const recs=activeRecords();const out={};
  const cu=recs.filter(r=>isFinite(r.cu)&&r.cu>0);const th=recs.filter(r=>isFinite(r.th)&&r.th>0);
  const spread=a=>a.length?Math.max(...a.map(r=>r.T))-Math.min(...a.map(r=>r.T)):0;
  if(cu.length>=3&&spread(cu)>5){const f=linfit(cu.map(r=>r.T),cu.map(r=>r.cu));if(f){const R20=f.c+20*f.m;out.cu={m:f.m,c:f.c,R20,alpha:f.m/R20,r2:r2(cu.map(r=>r.cu),cu.map(r=>f.c+f.m*r.T)),n:cu.length};}}
  if(th.length>=3&&spread(th)>5){const xs=th.map(r=>1/(r.T+C2K)),ys=th.map(r=>Math.log(r.th));const f=linfit(xs,ys);if(f){const B=f.m;const R25=Math.exp(f.c+B/298.15);out.th={B,lnA:f.c,R25,Eg:2*kB*B,r2:r2(th.map(r=>r.th),th.map(r=>Math.exp(f.c+B/(r.T+C2K)))),r2lin:r2(ys,xs.map(x=>f.c+B*x)),n:th.length};}}
  return out;
}

/* ============ plotting ============ */
function niceStep(range,n){const raw=range/n;const p=Math.pow(10,Math.floor(Math.log10(raw)));const f=raw/p;return (f<1.5?1:f<3?2:f<7?5:10)*p;}
function plot(c,cfg){
  const {ctx,w,h}=fit(c);ctx.clearRect(0,0,w,h);const m={l:58,r:12,t:12,b:36};const pw=w-m.l-m.r,ph=h-m.t-m.b;
  const [x0,x1]=cfg.x,[y0,y1]=cfg.y;const lg=cfg.logY;
  const X=v=>m.l+(v-x0)/(x1-x0)*pw;const Y=v=>{if(lg){return m.t+ph-(Math.log10(v)-Math.log10(y0))/(Math.log10(y1)-Math.log10(y0))*ph;}return m.t+ph-(v-y0)/(y1-y0)*ph;};
  ctx.font='10.5px IBM Plex Mono';ctx.fillStyle='#5d6d7e';ctx.strokeStyle='#16202a';ctx.lineWidth=1;
  const xs=cfg.xStep||niceStep(x1-x0,6);ctx.textAlign='center';
  for(let v=Math.ceil(x0/xs-1e-9)*xs;v<=x1+1e-9;v+=xs){const px=X(v);ctx.beginPath();ctx.moveTo(px,m.t);ctx.lineTo(px,m.t+ph);ctx.stroke();ctx.fillText(cfg.xFmt?cfg.xFmt(v):+v.toFixed(6),px,m.t+ph+14);}
  ctx.textAlign='right';
  if(lg){for(let e=Math.ceil(Math.log10(y0));e<=Math.floor(Math.log10(y1));e++){const v=Math.pow(10,e);const py=Y(v);ctx.beginPath();ctx.moveTo(m.l,py);ctx.lineTo(m.l+pw,py);ctx.stroke();if(e%2===0||Math.log10(y1/y0)<8)ctx.fillText('10'+sup(e),m.l-6,py+3);}}
  else{const ys=niceStep(y1-y0,5);for(let v=Math.ceil(y0/ys-1e-9)*ys;v<=y1+1e-9;v+=ys){const py=Y(v);ctx.beginPath();ctx.moveTo(m.l,py);ctx.lineTo(m.l+pw,py);ctx.stroke();ctx.fillText(cfg.yFmt?cfg.yFmt(v):+v.toFixed(6),m.l-6,py+3);}}
  ctx.strokeStyle='#2a3949';ctx.strokeRect(m.l,m.t,pw,ph);
  ctx.fillStyle='#8b9bab';ctx.font='11px IBM Plex Sans';ctx.textAlign='center';ctx.fillText(cfg.xlabel,m.l+pw/2,h-6);
  ctx.save();ctx.translate(12,m.t+ph/2);ctx.rotate(-Math.PI/2);ctx.fillText(cfg.ylabel,0,0);ctx.restore();
  ctx.save();ctx.beginPath();ctx.rect(m.l,m.t,pw,ph);ctx.clip();
  if(cfg.vline!=null){const px=X(cfg.vline);ctx.strokeStyle='rgba(255,209,102,.35)';ctx.setLineDash([3,4]);ctx.beginPath();ctx.moveTo(px,m.t);ctx.lineTo(px,m.t+ph);ctx.stroke();ctx.setLineDash([]);}
  for(const L of cfg.lines||[]){ctx.strokeStyle=L.color;ctx.lineWidth=L.width||2;ctx.setLineDash(L.dash||[]);ctx.beginPath();L.pts.forEach((p,i)=>{if(lg&&p[1]<=0)return;const px=X(p[0]),py=Y(p[1]);i?ctx.lineTo(px,py):ctx.moveTo(px,py);});ctx.stroke();ctx.setLineDash([]);}
  for(const p of cfg.points||[]){const px=X(p.x),py=Y(p.y);ctx.fillStyle=p.color;ctx.strokeStyle='#0b1117';ctx.lineWidth=1.5;ctx.beginPath();
    if(p.shape==='d'){ctx.moveTo(px,py-5);ctx.lineTo(px+5,py);ctx.lineTo(px,py+5);ctx.lineTo(px-5,py);ctx.closePath();}else ctx.arc(px,py,4,0,6.283);ctx.fill();ctx.stroke();}
  for(const mk of cfg.markers||[]){const px=X(mk.x),py=Y(mk.y);ctx.fillStyle=mk.color;ctx.beginPath();ctx.arc(px,py,5,0,6.283);ctx.fill();ctx.strokeStyle='#fff';ctx.lineWidth=1.5;ctx.stroke();}
  ctx.restore();
  if(cfg.legend){ctx.font='11px IBM Plex Sans';let lx=m.l+10,ly=m.t+16;ctx.textAlign='left';
    const lw=Math.max(...cfg.legend.map(l=>ctx.measureText(l.label).width))+34;ctx.fillStyle='rgba(11,17,23,.85)';roundRect(ctx,lx-6,ly-12,lw,cfg.legend.length*16+6,6);ctx.fill();
    for(const l of cfg.legend){ctx.strokeStyle=l.color;ctx.fillStyle=l.color;ctx.lineWidth=2;ctx.setLineDash(l.dash||[]);if(l.dot){ctx.beginPath();ctx.arc(lx+9,ly-4,3.5,0,6.283);ctx.fill();}else{ctx.beginPath();ctx.moveTo(lx,ly-4);ctx.lineTo(lx+18,ly-4);ctx.stroke();}ctx.setLineDash([]);ctx.fillStyle='#c4d1dd';ctx.fillText(l.label,lx+24,ly);ly+=16;}}
  if(cfg.annot){ctx.font='600 11.5px IBM Plex Mono';ctx.textAlign='right';ctx.fillStyle=cfg.annotColor||'#ffd166';cfg.annot.split('\n').forEach((s,i)=>ctx.fillText(s,m.l+pw-8,m.t+ph-10-(cfg.annot.split('\n').length-1-i)*15));}
}
function range(vals,pad=0.06){let a=Math.min(...vals),b=Math.max(...vals);if(a===b){a-=1;b+=1;}const d=(b-a)*pad;return [a-d,b+d];}
function drawPlots(){
  const recs=activeRecords(),F=computeFits();const T=S.Tsample;
  const tx=[];for(let t=0;t<=100;t+=1)tx.push(t);
  // A copper
  const thA=tx.map(t=>[t,Rcu(t)]);const ptsA=recs.filter(r=>isFinite(r.cu)).map(r=>({x:r.T,y:r.cu,color:runInfo(r.run).color,shape:r.src==='csv'?'d':'o'}));
  const linesA=[{pts:thA,color:'#ee9a5c',width:2.2}];const legA=[{label:'model (α = 0.0039)',color:'#ee9a5c'}];
  if(F.cu){linesA.push({pts:[[0,F.cu.c],[100,F.cu.c+100*F.cu.m]],color:'#ffd166',dash:[6,4],width:1.6});legA.push({label:'best fit to data',color:'#ffd166',dash:[6,4]});}
  if(ptsA.length)legA.push({label:'readings',color:'#c4d1dd',dot:true});
  plot(document.getElementById('plotA'),{x:[0,100],y:range([...thA.map(p=>p[1]),...ptsA.map(p=>p.y)]),xlabel:'Temperature T (°C)',ylabel:'Resistance R (Ω)',lines:linesA,points:ptsA,vline:T,legend:legA,
    markers:[{x:T,y:Rcu(T),color:'#ee9a5c'}],annot:F.cu?`α = ${F.cu.alpha.toFixed(5)} /°C\nR² = ${F.cu.r2.toFixed(4)}`:'',xStep:10});
  // B thermistor
  const thB=tx.map(t=>[t,Rth(t)/1000]);const ptsB=recs.filter(r=>isFinite(r.th)).map(r=>({x:r.T,y:r.th/1000,color:runInfo(r.run).color,shape:r.src==='csv'?'d':'o'}));
  const linesB=[{pts:thB,color:'#5ab0ff',width:2.2}];const legB=[{label:'model (B = 3950 K)',color:'#5ab0ff'}];
  if(F.th){linesB.push({pts:tx.map(t=>[t,Math.exp(F.th.lnA+F.th.B/(t+C2K))/1000]),color:'#ffd166',dash:[6,4],width:1.6});legB.push({label:'best fit to data',color:'#ffd166',dash:[6,4]});}
  if(ptsB.length)legB.push({label:'readings',color:'#c4d1dd',dot:true});
  const yB=range([...thB.map(p=>p[1]),...ptsB.map(p=>p.y)]);yB[0]=Math.max(0,yB[0]);
  plot(document.getElementById('plotB'),{x:[0,100],y:yB,xlabel:'Temperature T (°C)',ylabel:'Resistance R (kΩ)',lines:linesB,points:ptsB,vline:T,legend:legB,markers:[{x:T,y:Rth(T)/1000,color:'#5ab0ff'}],
    annot:F.th?`R₂₅ = ${(F.th.R25/1000).toFixed(2)} kΩ\nR² = ${F.th.r2.toFixed(4)}`:'',xStep:10});
  // C ln R vs 1/T
  const invx=t=>1000/(t+C2K);const thC=tx.map(t=>[invx(t),Math.log(Rth(t))]);
  const ptsC=recs.filter(r=>isFinite(r.th)&&r.th>0).map(r=>({x:invx(r.T),y:Math.log(r.th),color:runInfo(r.run).color,shape:r.src==='csv'?'d':'o'}));
  const linesC=[{pts:thC,color:'#5ab0ff',width:2.2}];const legC=[{label:'model',color:'#5ab0ff'}];
  if(F.th){linesC.push({pts:[[invx(100),F.th.lnA+F.th.B*invx(100)/1000],[invx(0),F.th.lnA+F.th.B*invx(0)/1000]],color:'#ffd166',dash:[6,4],width:1.6});legC.push({label:'linear fit',color:'#ffd166',dash:[6,4]});}
  plot(document.getElementById('plotC'),{x:[invx(100)-0.02,invx(0)+0.02],y:range([...thC.map(p=>p[1]),...ptsC.map(p=>p.y)]),xlabel:'1/T (×10⁻³ K⁻¹)',ylabel:'ln R (R in Ω)',lines:linesC,points:ptsC,legend:legC,
    xFmt:v=>v.toFixed(2),xStep:0.1,yFmt:v=>v.toFixed(1),markers:[{x:invx(T),y:Math.log(Rth(T)),color:'#5ab0ff'}],
    annot:F.th?`slope B = ${Math.round(F.th.B)} K\nE_g ≈ 2k·B = ${F.th.Eg.toFixed(3)} eV`:'slope B = 3950 K (model)\nE_g ≈ 2k·B = '+EG_TH.toFixed(3)+' eV'});
  // D carriers
  const lnSi=tx.map(t=>[t,ni('si',t)]),lnGe=tx.map(t=>[t,ni('ge',t)]);
  plot(document.getElementById('plotD'),{x:[0,100],y:[1e8,1e24],logY:true,xlabel:'Temperature T (°C)',ylabel:'Carriers per cm³',xStep:10,vline:T,
    lines:[{pts:tx.map(t=>[t,NCU]),color:'#ee9a5c',width:2.2},{pts:lnGe,color:'#b48cff',width:2.2},{pts:lnSi,color:'#7fd1c7',width:2.2}],
    markers:[{x:T,y:NCU,color:'#ee9a5c'},{x:T,y:ni('ge',T),color:'#b48cff'},{x:T,y:ni('si',T),color:'#7fd1c7'}],
    legend:[{label:'Copper: constant ≈ 8.5×10²²',color:'#ee9a5c'},{label:'Germanium (E_g 0.67 eV)',color:'#b48cff'},{label:'Silicon (E_g 1.12 eV)',color:'#7fd1c7'}],
    annot:`at ${T.toFixed(0)} °C: Si ${sci(ni('si',T),1)}, Ge ${sci(ni('ge',T),1)}`,annotColor:'#c4d1dd'});
  renderSummary(F);
}
function pct(a,b){return Math.abs(a-b)/Math.abs(b)*100;}
function cls(p){return p<2?'ok':p<6?'warn':'badc';}
function renderSummary(F){
  const t=document.getElementById('sumTable');const row=(q,fv,ref,p,r)=>`<tr><td>${q}</td><td>${fv}</td><td>${ref}</td><td class="${p==null?'':cls(p)}">${p==null?'—':p.toFixed(2)+' %'}</td><td>${r==null?'—':r.toFixed(4)}</td></tr>`;
  let h='<tr><th>Quantity</th><th>From your data</th><th>Reference</th><th>% error</th><th>R²</th></tr>';
  h+=row('Copper temperature coefficient α (/°C)',F.cu?F.cu.alpha.toFixed(5):'—','0.0039 (textbook)',F.cu?pct(F.cu.alpha,0.0039):null,F.cu?F.cu.r2:null);
  h+=row('Copper R₂₀',F.cu?F.cu.R20.toFixed(3)+' Ω':'—','10.000 Ω (coil spec)',F.cu?pct(F.cu.R20,10):null,null);
  h+=row('Thermistor B constant',F.th?Math.round(F.th.B)+' K':'—','3950 K (datasheet)',F.th?pct(F.th.B,3950):null,F.th?F.th.r2lin:null);
  h+=row('Thermistor R₂₅',F.th?(F.th.R25/1000).toFixed(3)+' kΩ':'—','10.000 kΩ',F.th?pct(F.th.R25,10000):null,F.th?F.th.r2:null);
  h+=row('Estimated band gap E<sub>g</sub> = 2k·B',F.th?F.th.Eg.toFixed(3)+' eV':'—','0.67 eV (Ge) · 1.12 eV (Si)',F.th?pct(F.th.Eg,0.67):null,null);
  t.innerHTML=h;
  const ins=[];
  if(!F.cu&&!F.th){ins.push('Record at least 3 readings spread over more than 5 °C (or run an <b>auto sweep</b>) to fit α and B.');ins.push('Until then, the plots show the model only. The yellow dashed line marks the current sample temperature.');}
  if(F.cu)ins.push(`Copper's resistance <b>rises linearly</b>: slope ${(F.cu.m*1000).toFixed(2)} mΩ/°C, so α = slope ÷ R₂₀ = <b>${F.cu.alpha.toFixed(5)} /°C</b>. Hotter lattice → more phonon scattering → shorter mean free time.`);
  if(F.th)ins.push(`The thermistor's resistance <b>falls exponentially</b>. ln R vs 1/T is a straight line (R² = ${F.th.r2lin.toFixed(4)}), slope B = ${Math.round(F.th.B)} K, so E<sub>g</sub> ≈ <b>${F.th.Eg.toFixed(2)} eV</b>. That is close to germanium, but a real NTC is a doped metal-oxide ceramic, so treat this as an <i>effective activation energy</i>.`);
  if(S.err.contact&&F.cu)ins.push('<b>Contact resistance</b> adds a constant 0.25 Ω. The slope is unchanged, but R₂₀ is inflated, so the fitted α comes out about 2–3 % low. Use four-wire measurement or subtract the lead resistance.');
  if(S.err.self&&F.th)ins.push('<b>Self-heating</b>: the measuring current warms the bead above the water, so readings are low, most of all near room temperature where R ≈ R<sub>series</sub>. Use a smaller current.');
  if(S.err.lag){const runs=[...new Set(activeRecords().map(r=>r.run))];ins.push(runs.length>1?'<b>Thermal lag</b>: heating and cooling runs separate into a hysteresis loop because the sample trails the thermometer. Stir, heat slowly and wait before each reading.':'<b>Thermal lag</b> is on. Record a heating run and a cooling run to see the hysteresis loop.');}
  if(F.cu&&F.th)ins.push('<b>The key contrast</b>: copper changes by about 0.4 %/°C because its carrier number is fixed. The thermistor changes by about 4 %/°C because heat creates carriers across the gap.');
  document.getElementById('insights').innerHTML=ins.map(s=>`<li>${s}</li>`).join('');
}

/* ============ alarm ============ */
function drawAlarm(){
  const Tt=S.Tth,R=Rth(Tt),Rf=10000,V=5;const Vout=V*Rf/(R+Rf);const Vref=V*Rf/(Rth(S.alarmT)+Rf);const on=Vout>=Vref;const blink=on&&(Math.floor(performance.now()/250)%2===0);
  const svg=document.getElementById('alarmSvg');
  svg.innerHTML=`
  <defs><radialGradient id="led" cx=".5" cy=".5" r=".5"><stop offset="0" stop-color="${on?'#ffb3b3':'#3a1a1a'}"/><stop offset="1" stop-color="${on?(blink?'#ff2a2a':'#a01818'):'#200c0c'}"/></radialGradient></defs>
  <g stroke="#5d6d7e" stroke-width="2" fill="none">
   <path d="M40 30 H120"/><path d="M40 30 V180 H120"/>
   <path d="M120 30 V48"/><path d="M120 92 V108"/><path d="M120 152 V180"/>
   <path d="M120 100 H200 V86 H220"/><path d="M200 140 H220"/><path d="M180 140 H200"/>
   <path d="M290 105 H320"/><path d="M380 105 V180 H120"/>
  </g>
  <text x="28" y="22" fill="#8b9bab" font-size="11" font-family="IBM Plex Mono">+5 V</text>
  <rect x="108" y="48" width="24" height="44" rx="4" fill="#123456" stroke="#5ab0ff"/>
  <text x="138" y="66" fill="#9fd0ff" font-size="11" font-family="IBM Plex Sans">NTC</text>
  <text x="138" y="80" fill="#e6edf3" font-size="11" font-family="IBM Plex Mono">${fmtR(R,3)}</text>
  <rect x="108" y="108" width="24" height="44" rx="4" fill="#1d2631" stroke="#8b9bab"/>
  <text x="138" y="128" fill="#8b9bab" font-size="11" font-family="IBM Plex Sans">10 kΩ</text>
  <circle cx="120" cy="100" r="3.5" fill="#ffd166"/>
  <text x="150" y="96" fill="#ffd166" font-size="11" font-family="IBM Plex Mono">V<tspan font-size="8" dy="2">out</tspan><tspan dy="-2"> ${Vout.toFixed(2)} V</tspan></text>
  <path d="M220 70 L220 156 L290 113 Z" fill="#141c26" stroke="#c4d1dd" stroke-width="2"/>
  <text x="226" y="92" fill="#c4d1dd" font-size="13" font-family="IBM Plex Mono">+</text><text x="226" y="145" fill="#c4d1dd" font-size="13" font-family="IBM Plex Mono">−</text>
  <text x="140" y="160" fill="#8b9bab" font-size="10.5" font-family="IBM Plex Mono">V<tspan font-size="8" dy="2">ref</tspan><tspan dy="-2"> ${Vref.toFixed(2)} V</tspan></text>
  <circle cx="340" cy="105" r="16" fill="url(#led)" stroke="${on?'#ff6b6b':'#4a2020'}" stroke-width="2"/>
  ${on?`<circle cx="340" cy="105" r="${blink?28:22}" fill="none" stroke="#ff4a4a" stroke-opacity=".35" stroke-width="3"/>`:''}
  <text x="340" y="146" text-anchor="middle" fill="${on?'#ff6b6b':'#5d6d7e'}" font-size="12" font-weight="600" font-family="IBM Plex Sans">${on?'ALARM!':'normal'}</text>
  <text x="300" y="30" fill="#8b9bab" font-size="11" font-family="IBM Plex Sans">Sensor at ${Tt.toFixed(1)} °C</text>
  <rect x="300" y="38" width="100" height="6" rx="3" fill="#1f2b38"/><rect x="300" y="38" width="${clamp(Tt,0,100)}" height="6" rx="3" fill="${on?'#ff6b6b':'#5ee0a0'}"/>
  <rect x="${300+S.alarmT-1}" y="34" width="2" height="14" fill="#ffd166"/>`;
}

/* ============ guided mode ============ */
const G={i:0,pred:{cu:null,th:null},quiz:null,lagStarted:false};
const steps=[
 {k:'Step 1 · Predict',t:'What will happen when the water heats up?',body:()=>`<p>The bath will warm from 5 °C to 80 °C with both samples in it. Before you look, make a prediction.</p>
   <div class="q"><b>Copper coil resistance will…</b><div class="choices" data-q="cu">${['rise','fall','stay the same'].map(c=>`<span class="choice ${G.pred.cu===c?'sel':''}" data-c="${c}">${c}</span>`).join('')}</div></div>
   <div class="q"><b>NTC thermistor resistance will…</b><div class="choices" data-q="th">${['rise','fall','stay the same'].map(c=>`<span class="choice ${G.pred.th===c?'sel':''}" data-c="${c}">${c}</span>`).join('')}</div></div>
   <div class="check ${G.pred.cu&&G.pred.th?'done':''}"><i></i>Both predictions made</div>`,done:()=>G.pred.cu&&G.pred.th},
 {k:'Step 2 · Observe',t:'Run the experiment',body:()=>`<p>Start an automatic sweep. The bath heats steadily and a reading is logged every 5 °C. Watch both multimeters on the bench. One number climbs slowly; the other collapses.</p>
   <button class="btn primary" id="gSweep">⟳ Start auto sweep</button>
   <div class="check ${S.records.length>=10?'done':''}"><i></i>At least 10 readings logged (${S.records.length})</div>`,done:()=>S.records.length>=10},
 {k:'Step 3 · Explain',t:'Why opposite trends?',body:()=>{const ok=(p,a)=>p===a?'<span class="ok">✓ you predicted this</span>':'<span class="warn">✗ you predicted "'+(p||'—')+'"</span>';
   return `<p><b style="color:var(--cu)">Copper rose</b> (${ok(G.pred.cu,'rise')}). Copper always has about 8.5×10²² free electrons per cm³. Heat doesn't add more. It makes the ion lattice vibrate harder, so electrons scatter more often and drift more slowly.</p>
   <button class="btn sm" data-mat="cu">Show copper's atomic view</button>
   <p style="margin-top:14px"><b style="color:var(--semi)">The thermistor fell</b> (${ok(G.pred.th,'fall')}). In a semiconductor, electrons are locked in bonds. Breaking free costs the band-gap energy E<sub>g</sub>. The chance of that rises like e<sup>−E<sub>g</sub>/2kT</sup>, so a little heat makes many more carriers, and that outweighs the extra scattering.</p>
   <button class="btn sm" data-mat="th">Show the thermistor's band diagram</button>
   <p style="margin-top:14px;color:var(--muted)">Try dragging the setpoint slider and watch electrons jump the gap in panel 2.</p>`},done:()=>true},
 {k:'Step 4 · Analyse',t:'Measure the band gap from your graph',body:()=>{const F=computeFits();const eg=F.th?F.th.Eg:null;
   return `<p>In panel 4, the plot of <b>ln R against 1/T</b> is a straight line. Its slope is the B constant, and E<sub>g</sub> ≈ 2k·B.</p>
   <div class="q"><b>Your result: B = ${F.th?Math.round(F.th.B)+' K':'(need data)'} → E<sub>g</sub> ≈ ${eg?eg.toFixed(2)+' eV':'—'}</b>Which material's band gap is this closest to?
   <div class="choices" id="gQuiz" style="margin-top:8px">${['Silicon (1.12 eV)','Germanium (0.67 eV)','Copper (0 eV)'].map((c,i)=>`<span class="choice ${G.quiz===i?(i===1?'right':'wrong'):''}" data-i="${i}">${c}</span>`).join('')}</div>
   ${G.quiz!=null?`<div class="feedback">${G.quiz===1?'Correct, it is germanium-like. But a commercial NTC is a <b>doped metal-oxide ceramic</b> (Mn/Ni/Co oxides), not intrinsic Ge, so the value is really an effective activation energy.':'Not quite. Compare '+(eg?eg.toFixed(2):'0.68')+' eV with each gap. It sits right next to germanium\'s 0.67 eV.'}</div>`:''}</div>`},done:()=>G.quiz===1},
 {k:'Step 5 · Errors',t:'Why the real experiment is messier',body:()=>`<p>Real data never sits exactly on the model. Switch on <b>thermal lag</b> and record a heating run plus a cooling run. The two curves won't overlap, because the sample is always a few degrees behind the thermometer.</p>
   <button class="btn primary" id="gLag">Turn on lag + run heating & cooling</button>
   <p style="margin-top:12px">Also try <b>contact resistance</b> (it lowers the fitted α) and <b>self-heating</b> (it lowers the thermistor readings). The <i>What your data says</i> box explains each effect.</p>
   <div class="check ${G.lagStarted?'done':''}"><i></i>Hysteresis experiment started</div>`,done:()=>G.lagStarted},
 {k:'Step 6 · Apply',t:'Put the physics to work',body:()=>`<p>Because an NTC's resistance changes about 10× faster per degree than copper's, it makes a sensitive, cheap temperature sensor.</p><p>Scroll to the <b>overheat alarm</b>, set a trip temperature, then raise the bath until the LED fires. The same band-gap physics powers diodes, LEDs and solar cells.</p>
   <button class="btn" id="gAlarm">Show me the alarm</button>
   <p style="margin-top:14px;color:var(--muted)">Future work from your project: a doping simulation, LDR and diode studies, and an Arduino digital thermometer using this exact B-equation.</p>`,done:()=>true}
];
function renderGuide(){
  const st=steps[G.i];document.getElementById('gDots').innerHTML=steps.map((s,i)=>`<i class="${i<G.i?'done':i===G.i?'cur':''}"></i>`).join('');
  document.getElementById('gBody').innerHTML=`<div class="kicker">${st.k}</div><h3>${st.t}</h3>${st.body()}`;
  document.getElementById('gPrev').disabled=G.i===0;const nx=document.getElementById('gNext');nx.textContent=G.i===steps.length-1?'Finish ✓':'Next →';nx.disabled=!st.done();
  const b=document.getElementById('gBody');
  b.querySelectorAll('.choices[data-q] .choice').forEach(c=>c.onclick=()=>{G.pred[c.parentNode.dataset.q]=c.dataset.c;renderGuide();});
  b.querySelectorAll('[data-mat]').forEach(x=>x.onclick=()=>{setMat(x.dataset.mat);flash(x.dataset.mat==='cu'?'cardAtom':'cardBand');});
  const sw=b.querySelector('#gSweep');if(sw)sw.onclick=()=>{startSweep();flash('cardBench');};
  b.querySelectorAll('#gQuiz .choice').forEach(c=>c.onclick=()=>{G.quiz=+c.dataset.i;renderGuide();});
  const gl=b.querySelector('#gLag');if(gl)gl.onclick=()=>{setErr('lag',true);setErr('cool',true);newRun();startSweep();G.lagStarted=true;flash('cardAnalysis');renderGuide();};
  const ga=b.querySelector('#gAlarm');if(ga)ga.onclick=()=>flash('cardAlarm');
}
function flash(id){const el=document.getElementById(id);el.scrollIntoView({behavior:'smooth',block:'center'});el.classList.remove('pulse');void el.offsetWidth;el.classList.add('pulse');}
let lastGuideDone=null;
setInterval(()=>{if(!document.getElementById('guide').classList.contains('open'))return;const d=steps[G.i].done();const key=G.i+':'+d+':'+S.records.length;if(key!==lastGuideDone){lastGuideDone=key;if(G.i===1||G.i===3||G.i===4)renderGuide();else document.getElementById('gNext').disabled=!d;}},700);

/* ============ UI wiring ============ */
const $=id=>document.getElementById(id);
const atomC=$('atom'),bandC=$('band'),benchC=$('bench');
const cuSim=new CopperSim(),semiSim=new SemiSim(),bandSim=new BandSim();
function toast(msg){const t=$('toast');t.textContent=msg;t.classList.add('show');clearTimeout(t._h);t._h=setTimeout(()=>t.classList.remove('show'),1800);}
function setTset(v,fromUser){S.tset=clamp(v,0,100);$('tset').value=S.tset;$('tsetVal').textContent=S.tset.toFixed(1)+' °C';if(fromUser&&S.sweep){S.sweep=null;updateSweepBtn();toast('Auto sweep stopped');}}
$('tset').addEventListener('input',e=>setTset(+e.target.value,true));
$('speed').onchange=e=>S.speed=+e.target.value;
$('matSeg').querySelectorAll('button').forEach(b=>b.onclick=()=>setMat(b.dataset.m));
const ATOM_TXT={
  cu:['Inside copper: electrons and a vibrating lattice','Free electrons (blue) drift through a lattice of Cu⁺ ions. Heating makes the ions vibrate harder, so electrons scatter more often (flashes) and drift more slowly.',
      [['#bfe2ff','free electron'],['#ee9a5c','Cu⁺ ion (vibrating)'],['#fff0c8','scattering event']]],
  th:['Inside an NTC thermistor (semiconductor model)','Electrons are held in bonds. Heat breaks some loose (flash). Each one leaves behind a hole, which acts as a positive carrier. More heat means more carriers and lower resistance.',
      [['#d6ecff','free electron'],['#ffb347','hole (missing bond electron)'],['#8cc6ff','bond electrons'],['#5ab0ff','metal-oxide site']]],
  si:['Inside intrinsic silicon','Each Si atom shares four covalent bonds. Breaking one costs 1.12 eV, which is rare at room temperature, so pure Si is a poor conductor. The carrier count climbs steeply as it heats.',
      [['#d6ecff','free electron'],['#ffb347','hole'],['#8cc6ff','bond electrons'],['#7fd1c7','Si atom']]],
  ge:['Inside intrinsic germanium','Ge has a smaller gap (0.67 eV), so about 2000× more carriers than Si at room temperature, and its count still rises exponentially with T.',
      [['#d6ecff','free electron'],['#ffb347','hole'],['#8cc6ff','bond electrons'],['#b48cff','Ge atom']]]
};
function setMat(m){S.mat=m;$('matSeg').querySelectorAll('button').forEach(b=>b.classList.toggle('active',b.dataset.m===m));
  const tx=ATOM_TXT[m];$('atomTitle').textContent=tx[0];$('atomSub').textContent=tx[1];$('atomLegend').innerHTML=tx[2].map(l=>`<span><i style="background:${l[0]}"></i>${l[1]}</span>`).join('');
  const {w,h}=fit(atomC);if(m==='cu')cuSim.init(w,h);else{semiSim.init(w,h,m);bandSim.reset();}
}
function setErr(k,v){S.err[k]=v;const map={noise:'eNoise',lag:'eLag',self:'eSelf',contact:'eContact',cool:'eCool'};$(map[k]).checked=v;S.dirtyPlots=true;}
[['eNoise','noise'],['eLag','lag'],['eSelf','self'],['eContact','contact'],['eCool','cool']].forEach(([id,k])=>$(id).onchange=e=>{S.err[k]=e.target.checked;S.dirtyPlots=true;});
$('recBtn').onclick=()=>{record();toast(`Recorded ${S.Tbath.toFixed(1)} °C`);};
$('autoRecBtn').onclick=()=>{S.autoRec=!S.autoRec;$('autoRecBtn').classList.toggle('on',S.autoRec);};
function newRun(){const used=S.records.filter(r=>r.run===S.run).length;if(used)S.run++;$('roRun').textContent='run '+S.run;}
$('newRunBtn').onclick=()=>{newRun();toast('Now on run '+S.run);};
$('undoBtn').onclick=()=>{if(S.records.length){S.records.pop();S.dirtyPlots=true;renderLog();renderChips();}};
const confirmClear={t:0};
$('clearBtn').onclick=()=>{const now=Date.now();if(now-confirmClear.t>2500){confirmClear.t=now;$('clearBtn').textContent='Click again to clear';setTimeout(()=>$('clearBtn').textContent='Clear all',2500);return;}
  S.records=[];S.runs={};S.run=1;S.sweep=null;updateSweepBtn();$('roRun').textContent='run 1';$('clearBtn').textContent='Clear all';S.dirtyPlots=true;renderLog();renderChips();};
function startSweep(){if(S.sweep){S.sweep=null;updateSweepBtn();return;}if(S.records.some(r=>r.run===S.run))newRun();S.sweep={phase:'prep'};setTset(5);updateSweepBtn();}
function updateSweepBtn(){const b=$('sweepBtn');b.innerHTML=S.sweep?'■ Stop sweep':'⟳ Auto sweep 5 → 80 °C';b.classList.toggle('on',!!S.sweep);}
$('sweepBtn').onclick=startSweep;
$('exportBtn').onclick=()=>{const csv='run,T_C,R_copper_ohm,R_thermistor_ohm\n'+S.records.map(r=>`${r.run},${r.T.toFixed(2)},${isFinite(r.cu)?r.cu.toFixed(4):''},${isFinite(r.th)?r.th.toFixed(1):''}`).join('\n');
  $('csvOut').value=csv;$('modal').classList.add('open');try{navigator.clipboard.writeText(csv).then(()=>toast('Copied to clipboard'),()=>{});}catch(e){}};
$('modalClose').onclick=()=>$('modal').classList.remove('open');$('modalSel').onclick=()=>{$('csvOut').select();};
$('csvIn').onchange=e=>{const f=e.target.files[0];if(!f)return;const rd=new FileReader();rd.onload=()=>{const n=importCSV(rd.result);toast(n?`Imported ${n} readings`:'No usable rows found');e.target.value='';};rd.readAsText(f);};
function importCSV(text){
  const lines=text.split(/\r?\n/).map(l=>l.trim()).filter(Boolean);if(!lines.length)return 0;const sep=lines[0].includes(';')&&!lines[0].includes(',')?';':lines[0].includes('\t')?'\t':',';
  let head=lines[0].split(sep).map(s=>s.trim().toLowerCase());let start=0,ti=0,ci=1,hi=2;
  if(head.some(hh=>/[a-z]/.test(hh))){start=1;const f=re=>head.findIndex(hh=>re.test(hh));
    ti=f(/temp|^t(_|\b|\s|$)|°c|deg/);ci=f(/cu|copper|coil|metal/);hi=f(/th|ntc|semi|therm/);
    if(ti<0)ti=0;if(ci<0&&hi<0){ci=1;hi=2;}}
  let n=0;for(let i=start;i<lines.length;i++){const p=lines[i].split(sep).map(s=>parseFloat(s));const T=p[ti];if(!isFinite(T))continue;
    const cu=ci>=0?p[ci]:NaN,th=hi>=0?p[hi]:NaN;if(!isFinite(cu)&&!isFinite(th))continue;S.records.push({run:'csv',T,cu:isFinite(cu)?cu:NaN,th:isFinite(th)?th:NaN,src:'csv'});n++;}
  runInfo('csv');S.dirtyPlots=true;renderLog();renderChips();return n;}
$('alarmT').oninput=e=>{S.alarmT=+e.target.value;$('alarmTv').textContent=S.alarmT+' °C';};
$('guideBtn').onclick=()=>{$('guide').classList.add('open');renderGuide();};
$('guideClose').onclick=()=>$('guide').classList.remove('open');
$('gPrev').onclick=()=>{if(G.i>0){G.i--;renderGuide();}};
$('gNext').onclick=()=>{if(G.i<steps.length-1){G.i++;renderGuide();}else{$('guide').classList.remove('open');toast('Guided tour complete 🎉');}};
document.addEventListener('keydown',e=>{if(e.key==='Escape'){$('guide').classList.remove('open');$('modal').classList.remove('open');}});

/* ============ main loop ============ */
let last=performance.now(),plotT=0,uiT=0,simTime=0;
function physics(dt){
  if(S.sweep){const sw=S.sweep;const rate=1.3;
    if(sw.phase==='prep'){setTset(5);if(Math.abs(S.Tbath-5)<0.4&&Math.abs(S.Tsample-5)<0.6){sw.phase='up';S.lastBathForRec=S.Tbath;}}
    else if(sw.phase==='up'){setTset(Math.min(84,S.tset+rate*dt));if(S.Tbath>=80.3){if(S.err.cool){newRun();sw.phase='down';S.lastBathForRec=S.Tbath;}else{S.sweep=null;setTset(80);updateSweepBtn();toast('Sweep complete');}}}
    else if(sw.phase==='down'){setTset(Math.max(1,S.tset-rate*dt));if(S.Tbath<=4.7){S.sweep=null;updateSweepBtn();toast('Heating + cooling runs complete');}}
  }
  const tb=2.2;S.Tbath+=(S.tset-S.Tbath)*clamp(dt/tb,0,1);
  const tl=S.err.lag?4.5:0.15;S.Tsample+=(S.Tbath-S.Tsample)*clamp(dt/tl,0,1);
  S.Tth=S.Tsample+selfHeat(S.Tsample);
  const recOn=S.autoRec||(S.sweep&&S.sweep.phase!=='prep');
  if(recOn){const a=S.lastBathForRec,b=S.Tbath;const lo=Math.min(a,b),hi=Math.max(a,b);for(let v=5;v<=80;v+=5){if(v>lo&&v<=hi&&Math.abs(b-a)<5){record();break;}}}
  S.lastBathForRec=S.Tbath;
}
function frame(now){
  const rdt=Math.min(0.05,(now-last)/1000);last=now;const dt=rdt*S.speed;simTime+=rdt;
  physics(dt);
  // meters update at ~5 Hz
  S.meter.t+=rdt;if(S.meter.t>0.2){S.meter.t=0;const m=measure();S.meter.cu=m.cu;S.meter.th=m.th;}
  const T=S.Tsample;
  // atomic
  let a=fit(atomC);if(atomC._resized){atomC._resized=false;setMat(S.mat);a=fit(atomC);}
  if(S.mat==='cu'){cuSim.step(rdt,T);cuSim.draw(a.ctx);}else{semiSim.step(rdt,T);for(const ev of semiSim.events)ev==='gen'?bandSim.gen():bandSim.rec();semiSim.events=[];semiSim.draw(a.ctx);}
  // band
  const b=fit(bandC);bandSim.step(rdt);bandSim.draw(b.ctx,b.w,b.h,S.mat,T,simTime);
  // bench
  const be=fit(benchC);drawBench(be.ctx,be.w,be.h,simTime);
  // ui
  uiT+=rdt;if(uiT>0.12){uiT=0;updateUI();}
  plotT+=rdt;if(plotT>0.25){plotT=0;drawPlots();}
  requestAnimationFrame(frame);
}
function updateUI(){
  const T=S.Tsample,m=S.mat;$('roBath').textContent=S.Tbath.toFixed(1);$('roSample').textContent=T.toFixed(1);
  $('kTtag').textContent=`kT = ${(kB*(T+C2K)*1000).toFixed(1)} meV  ·  T = ${(T+C2K).toFixed(1)} K`;
  if(m==='cu'){
    $('s1l').textContent='Free electrons (per cm³)';$('s1').textContent='8.5×10²² (fixed)';
    $('s2l').textContent='Scattering rate vs 20 °C';const sr=Rcu(T)/Rcu(20);$('s2').textContent='×'+sr.toFixed(3);$('s2b').style.width=clamp((sr-0.85)/0.45*100,3,100)+'%';$('s2b').style.background='var(--cu)';
    $('s3l').textContent='Drift speed (relative)';const d=Rcu(0)/Rcu(T);$('s3').textContent=(d*100).toFixed(1)+' %';$('s3b').style.width=clamp((d-0.6)/0.4*100,3,100)+'%';$('s3b').style.background='var(--semi)';
    $('s4l').textContent='Coil resistance';$('s4').textContent=fmtR(Rcu(T),4);
    $('bandNote').innerHTML=`<b>Conductor:</b> copper's 4s band is only half filled, so there is no gap to cross and the number of free electrons does not change with temperature. Heat only blurs the occupancy edge within about kT (${(kB*(T+C2K)*1000).toFixed(0)} meV) of E<sub>F</sub>. Resistance rises because <b>lattice vibrations scatter electrons more</b>: R = R₀[1 + α(T − T₀)].`;
  }else{
    const Eg=egOf(m);const rel=m==='th'?TH.R25/Rth(T):ni(m,T)/ni(m,25);
    $('s1l').textContent=m==='th'?'Carriers vs 25 °C':'Intrinsic carriers nᵢ (cm⁻³)';$('s1').textContent=m==='th'?'×'+rel.toPrecision(3):sci(ni(m,T));
    $('s2l').textContent='Gap-crossing factor e^(−Eg/2kT)';const fx=Math.exp(-Eg/(2*kB*(T+C2K)));$('s2').textContent=sci(fx,2);$('s2b').style.width=(carrierFrac(m,T)*100)+'%';$('s2b').style.background='var(--hole)';
    $('s3l').textContent='Conductivity vs 25 °C';$('s3').textContent='×'+rel.toPrecision(3);$('s3b').style.width=clamp(Math.log10(rel)/2*50+50,3,100)+'%';$('s3b').style.background='var(--semi)';
    $('s4l').textContent=m==='th'?'Thermistor resistance':'R of 1 cm × 1 mm² bar';$('s4').textContent=m==='th'?fmtR(Rth(T),3):fmtR(rho(m,T)*100,3);
    const nm=m==='th'?'NTC thermistor':SEMI[m].name;
    $('bandNote').innerHTML=`<b>Semiconductor (${nm}):</b> the valence band is full and the conduction band is empty, separated by a gap of <b>${Eg.toFixed(2)} eV</b>. Thermal energy kT is only ${(kB*(T+C2K)*1000).toFixed(1)} meV, so only the Boltzmann tail crosses, with n ∝ T<sup>3/2</sup>e<sup>−E<sub>g</sub>/2kT</sup>. At ${T.toFixed(0)} °C there are <b>${rel.toPrecision(3)}×</b> as many carriers as at 25 °C, so resistance <b>falls</b>.${m==='th'?' (A real NTC is a doped oxide; E<sub>g</sub> here is the effective value 2k·B.)':''}`;
  }
  drawAlarm();
}

/* init */
setMat('cu');renderLog();renderChips();setTset(25);
requestAnimationFrame(frame);
})();
</script>
</body>
</html>


Thermal Band Lab
Temperature Dependence of Resistance in Conductors and Semiconductors

An interactive virtual lab and band-theory simulation, built for an experiential learning project

📖 Table of Contents
About the Project
The Physics
Features
Getting Started
How to Use It
Importing Your Own Lab Data
Model Parameters
How It Works (Technical)
Project Structure
Assumptions and Limitations
Future Work
References
License
🎯 About the Project

Materials are classified as conductors, semiconductors or insulators according to their electronic energy band structure. Temperature affects conductors and semiconductors in opposite ways:

Material	What happens when heated	Why
Conductor (copper)	Resistance rises slowly and linearly	Lattice vibrations scatter electrons more often
Semiconductor (NTC thermistor, Si, Ge)	Resistance falls steeply and exponentially	Heat lifts more electrons across the band gap and creates new charge carriers

Problem statement: Band theory is abstract and hard to visualise. How can real measurements and a simulation be combined to show why a conductor and a semiconductor respond differently to temperature?

This project has two phases:

Phase 1, Experiment: A copper coil and an NTC thermistor are placed in a water bath. Resistance is recorded about every 5 °C from ~5 °C to ~80 °C over 2–3 runs.
Phase 2, Simulation: A band-theory model computes R vs T and carrier concentration vs T, fits the measured data, and compares theory with experiment.

Thermal Band Lab is the interactive simulation for Phase 2. It also includes a virtual version of the Phase 1 bench, so the full experiment can be shown, repeated and analysed in a browser.

Objectives
Measure resistance vs temperature for a copper coil and an NTC thermistor.
Extract the temperature coefficient α (copper), the B constant (thermistor) and an estimated band gap Eg.
Simulate both materials using band-theory equations.
Compare measured and simulated results and explain the differences.
⚛️ The Physics
1. Conductor: copper

Copper has about 8.5 × 10²² free electrons per cm³, and that number does not change with temperature. When copper is heated, its ions vibrate more strongly (more phonons). Electrons collide with the vibrating lattice more often, so their mean free time drops and resistance rises:

$$R = R_0\,[1 + \alpha\,(T - T_0)] \qquad \alpha_{Cu} \approx 0.0039\ /^\circ\text{C}$$

In band terms, copper's 4s band is only half filled and overlaps the 3d band. There is no band gap, so heat only blurs the electron occupancy within a few kT of the Fermi level.

2. Semiconductor: intrinsic Si and Ge

In a semiconductor, the valence band is full and the conduction band is empty. A band gap Eg separates them. An electron must gain at least Eg to become a free carrier, and each one that does leaves behind a hole. The intrinsic carrier concentration is:

$$n_i \propto T^{3/2}\,\exp\!\left(-\frac{E_g}{2kT}\right)$$

Because kT ≈ 25 meV at room temperature and Eg is around 1 eV, only the exponential tail of the Fermi–Dirac distribution reaches the conduction band. A small rise in T therefore produces a large rise in carriers. That outweighs the extra scattering, so resistance falls.

Material	Band gap Eg
Silicon	1.12 eV
Germanium	0.67 eV
3. NTC thermistor model

A thermistor follows the B-parameter equation:

$$R = R_0\,\exp\!\left[B\left(\frac{1}{T} - \frac{1}{T_0}\right)\right]$$

Taking logarithms gives a straight line:

$$\ln R = \frac{B}{T} + \text{const.}$$

So the slope of ln R vs 1/T equals B. Comparing with the semiconductor equation gives an estimate of the band gap:

$$E_g \approx 2k \cdot B$$

For B = 3950 K this gives Eg ≈ 0.68 eV, close to germanium. A commercial NTC is actually a doped metal-oxide ceramic, so this is best read as an effective activation energy.

✨ Features
1 · Atomic view: inside the material

A live animation of what is happening at the atomic scale:

Copper: free electrons drift through a lattice of Cu⁺ ions. As temperature rises, the ions vibrate harder, scattering events (flashes) become more frequent, and the drift slows.
Semiconductors: a covalent lattice with bond electrons. Heat breaks bonds, which creates electron–hole pairs. Electrons drift one way and holes hop the other way. Generation and recombination run continuously, so the dynamic equilibrium is visible.
Live readouts: carrier density, scattering rate, drift/conductivity and resistance.
2 · Energy band diagram
Valence band, conduction band and band gap drawn to scale for Si, Ge and the thermistor (effective Eg from B).
Electrons visibly jump the gap when they are generated and fall back when they recombine. This is synchronised with the atomic view.
A side plot of the Fermi–Dirac occupancy f(E) on a log scale, with 0 °C and 100 °C reference curves, and a live readout of f(E꜀) at the conduction-band edge.
For copper: overlapping 4s/3d bands, a smeared Fermi edge, and a zoomed linear f(E) plot showing why the carrier count stays fixed.
3 · Virtual experiment bench (Phase 1)

Virtual bench

An animated beaker with a kettle, ice, bubbles and steam, a thermometer, a copper coil, an NTC bead and two live multimeters.
Record reading, auto-record every 5 °C, or a one-click auto sweep 5 → 80 °C, with an optional cooling run back to 5 °C.
Multiple runs, undo, clear, and a live data log.
Toggleable real-world error sources (the same ones discussed in the project report):
📈 Meter noise: random scatter of ±0.2–0.4 %
⏱️ Thermal lag: the sample trails the thermometer, which produces hysteresis between heating and cooling runs
🔥 Self-heating: the measuring current in a 5 V / 10 kΩ divider warms the thermistor bead
🔌 Contact resistance: a +0.25 Ω offset from leads and clips, which biases the fitted α low
4 · Analysis dashboard (Phase 2)

Analysis dashboard Four live plots, each overlaying model (solid) + readings (dots, coloured by run) + best fit (dashed):

Copper R vs T, with a linear fit giving α and R²
Thermistor R vs T, with an exponential fit giving R₂₅ and R²
ln R vs 1/T, with a linear fit whose slope gives B and Eg ≈ 2k·B
Carrier concentration vs T (log scale) for copper, Ge and Si

Also included:

An auto-generated summary table of fitted α, R₂₀, B, R₂₅ and Eg against textbook/datasheet values, with % error (colour-coded) and R².
A "What your data says" panel that interprets the results in plain language and explains the effect of each error source you switched on.
Clickable run chips to include runs in the fits or leave them out.
★ Application: overheat / fire alarm

An NTC in a voltage divider feeding a comparator. Choose a trip temperature, heat the bath and watch V꜀ᵤₜ cross V꜀ₑ𝒻 until the alarm LED fires.

▶ Guided mode: Predict → Observe → Explain

A six-step walkthrough designed for presenting to a class or teacher:

Predict: will each resistance rise, fall or stay the same?
Observe: run the automatic sweep.
Explain: compare the results with your predictions, with links into the atomic and band views.
Analyse: measure Eg from the graph and decide whether it is closer to Si or Ge.
Errors: turn on thermal lag and produce a hysteresis loop.
Apply: the overheat alarm and real-world applications.
🚀 Getting Started

No installation, build step or server is needed.

Option A: open it directly

Download or clone this repository.
Double-click index.html, or open it in Chrome, Edge, Firefox or Safari.
bash
git clone https://github.com/<your-username>/thermal-band-lab.git
cd thermal-band-lab
# then open index.html in your browser

Option B: GitHub Pages (shareable link)

Push the repository to GitHub.
Go to Settings → Pages → Deploy from branch → main / root.
Your lab will be live at https://<your-username>.github.io/thermal-band-lab/.

💡 The simulation works fully offline. The only external resource is Google Fonts; without internet, the page falls back to system fonts.

🧭 How to Use It
Control	What it does
Water-bath setpoint slider	Sets the target bath temperature (0–100 °C). The bath heats or cools towards it realistically.
Material switch	Chooses which material the atomic view and band diagram show: Copper, NTC thermistor, Silicon or Germanium.
Sim speed	½×, 1×, 2× or 4× for the bath and sweep dynamics.
Record reading	Logs the current thermometer reading and both resistances.
Auto-record every 5 °C	Logs a reading each time the thermometer crosses a multiple of 5 °C.
Auto sweep	Cools to 5 °C, then ramps to 80 °C while recording automatically (and optionally cools back down as a new run).
Error toggles	Switch noise, thermal lag, self-heating and contact resistance on or off.
Copy CSV	Copies all readings as CSV for Excel, Google Sheets or Python.
Import CSV	Loads your real measured data into the analysis.

Suggested 5-minute demo:

Set the speed to 2× and press Auto sweep. Watch the copper meter creep up while the NTC meter collapses.
Switch the material between Copper → Silicon → Germanium and drag the slider to watch carriers appear across the gap.
Read α, B and Eg from the analysis panel.
Turn on Thermal lag and cooling run, then sweep again to see the hysteresis loop.
Finish with the fire alarm.
📥 Importing Your Own Lab Data

Export your Phase 1 measurements as a .csv file. Headers are detected automatically: any column containing temp/T, cu/copper/coil, or th/ntc/therm is matched. Without headers, the order is assumed to be T, R_copper, R_thermistor.

csv
T_C,R_copper_ohm,R_thermistor_ohm
5.0,9.42,25300
10.1,9.61,19850
15.0,9.80,15650
20.2,10.01,12450
...
Temperature is in °C, and resistances are in ohms (Ω).
Either resistance column may be left blank.
Imported points appear as white diamonds labelled Your data (CSV) and are included in all fits.
📐 Model Parameters
Parameter	Value	Source
Copper α	0.0039 /°C	Textbook value
Copper coil R₂₀	10.00 Ω	Assumed coil specification
Copper free-electron density	8.5 × 10²² cm⁻³	Kittel
NTC R₂₅	10.0 kΩ	Typical datasheet
NTC B constant	3950 K	Typical datasheet
Si band gap	1.12 eV	Streetman
Ge band gap	0.67 eV	Streetman
nᵢ (Si, 300 K)	1.0 × 10¹⁰ cm⁻³	Standard value
nᵢ (Ge, 300 K)	2.0 × 10¹³ cm⁻³	Standard value
Mobilities μₙ + μₚ	Si: 1350 + 480, Ge: 3900 + 1900 cm²/V·s	Standard values
Boltzmann constant k	8.617 × 10⁻⁵ eV/K	CODATA
Self-heating dissipation constant	0.8 mW/°C	Assumed
Thermal lag time constant	4.5 s (simulation time)	Assumed
Contact resistance	0.25 Ω	Assumed
🛠 How It Works (Technical)
Pure HTML5 + CSS + vanilla JavaScript in a single self-contained file, with no frameworks or chart libraries.
Rendering: every animation and plot is drawn on <canvas> with device-pixel-ratio scaling for sharp output. The alarm circuit is live SVG.
Atomic simulation:
Copper: particle model with a constant Fermi speed, a drift acceleration from the applied field, and stochastic scattering at a rate proportional to R(T) (exaggerated for visibility). Drift velocity ≈ a/Γ emerges naturally.
Semiconductor: a lattice graph of atoms and bonds. Electron–hole pairs are generated or recombined towards a target count mapped from ln nᵢ(T). Holes hop between neighbouring bonds with a field bias, and electrons follow a biased random walk.
Bath dynamics: first-order (exponential) relaxation of bath → sample temperature, with an optional lag time constant.
Measurement model: true R(T), plus optional Gaussian noise, a contact offset, and self-heating computed from P = V²R/(R + Rₛ)².
Fitting: ordinary least-squares linear regression:
Copper: R vs T → slope m, intercept c → R₂₀ = c + 20m, α = m / R₂₀
Thermistor: ln R vs 1/T(K) → slope = B, R₂₅ from the intercept, Eg = 2kB
R² is reported for each fit.
Band diagram: the Fermi–Dirac function f(E) = 1 / (1 + e^{(E−E_F)/kT}) is evaluated in a numerically stable way on a log scale.
📁 Project Structure
thermal-band-lab/
├── index.html          # The complete simulation (HTML + CSS + JS)
├── README.md           # This file
└── screenshots/
    ├── visual-panels.png
    ├── bench.png
    └── analysis.png
⚠️ Assumptions and Limitations
Visual exaggeration: lattice vibration amplitude, scattering-rate contrast and the Fermi-edge blur are exaggerated so the effects are visible. All numbers shown in the readouts and plots use the real equations.
Copper model is linear (constant α), which is accurate over 0–100 °C but not near cryogenic temperatures.
Semiconductors are intrinsic, so doping is not modelled. The band gap is treated as temperature-independent (Varshni shift ignored), and mobility is held constant (its T^−3/2 phonon dependence is ignored).
Carrier animation count is normalised per material on a log scale. Use the numeric readouts and plot 4 to compare absolute densities between Si and Ge.
NTC thermistors are doped transition-metal oxides, not intrinsic semiconductors. The extracted Eg is an effective activation energy.
Error sources use simplified, illustrative magnitudes.
🔭 Future Work
[ ] Doping simulation: n-type and p-type slider, extrinsic and saturation regions
[ ] Temperature-dependent band gap (Varshni) and mobility
[ ] LDR (light-dependent resistor) and p–n diode I–V vs temperature modules
[ ] Arduino digital thermometer: live serial data streamed into the analysis
[ ] Companion Python notebook (NumPy, Pandas, Matplotlib, SciPy) reproducing all fits
[ ] Steinhart–Hart thermistor model for comparison with the B-parameter model
📚 References
C. Kittel, Introduction to Solid State Physics, Wiley.
B. G. Streetman & S. K. Banerjee, Solid State Electronic Devices, Pearson.
NTC thermistor manufacturer datasheets (B-parameter and R₂₅ specifications).
NumPy, SciPy and Matplotlib documentation (used for the Phase 2 Python analysis).
🙌 Acknowledgements

Developed as part of an Experiential Learning Project on the temperature dependence of resistance in conductors and semiconductors.

📄 License

Released under the MIT License. Feel free to use, modify and share for teaching and learning.


<img width="1400" height="800" alt="image" src="https://github.com/user-attachments/assets/6eb14fa5-bd79-4500-ac39-f57b9448a467" />
<img width="1400" height="669" alt="image" src="https://github.com/user-attachments/assets/c1d1b9e8-0346-415e-a73f-4cef4880442b" />
<img width="1204" height="996" alt="image" src="https://github.com/user-attachments/assets/84b7b9ec-f73d-4cb7-9bad-bf877d9033f2" />
