<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Qwobble</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700&family=JetBrains+Mono:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0f1319;
    --bg-2:#161c25;
    --felt:#173a2f;
    --felt-2:#1f4c3d;
    --felt-line:#2c6551;
    --gold:#e8b94a;
    --gold-soft:#f3d489;
    --marble:#4f8ef7;
    --marble-2:#2e5fc9;
    --good:#5fca8d;
    --bad:#e0625c;
    --cream:#f2ede6;
    --muted:#8fa0ac;
    --card-back:#233047;
    --radius:16px;
    --shadow: 0 12px 30px rgba(0,0,0,.45);
  }
  *{box-sizing:border-box;}
  html,body{height:100%;}
  body{
    margin:0;
    background:
      radial-gradient(1200px 700px at 50% -10%, #1a2432 0%, var(--bg) 55%),
      var(--bg);
    color:var(--cream);
    font-family: 'Fredoka', system-ui, sans-serif;
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:24px;
    overflow-x:hidden;
  }
  .mono{font-family:'JetBrains Mono', monospace;}
  #app{
    width:100%;
    max-width:920px;
    background: linear-gradient(180deg, var(--bg-2), #10151d);
    border-radius:24px;
    border:1px solid #2a3442;
    box-shadow: var(--shadow);
    overflow:hidden;
  }
  header{
    padding:22px 28px 16px;
    display:flex;
    align-items:baseline;
    justify-content:space-between;
    border-bottom:1px solid #232d3a;
  }
  header h1{
    margin:0;
    font-size:28px;
    letter-spacing:0.5px;
    color:var(--gold-soft);
    text-shadow: 0 2px 0 rgba(0,0,0,.4);
  }
  header h1 span{color:var(--marble);}
  header .tag{color:var(--muted); font-size:13px; font-family:'JetBrains Mono',monospace;}
  .screen{display:none; padding:28px;}
  .screen.active{display:block; animation:fadeIn .35s ease;}
  @keyframes fadeIn{from{opacity:0; transform:translateY(6px);} to{opacity:1; transform:translateY(0);}}

  .btn{
    font-family:'Fredoka', sans-serif;
    font-weight:600;
    font-size:16px;
    padding:13px 26px;
    border-radius:999px;
    border:none;
    cursor:pointer;
    background: linear-gradient(180deg, var(--gold-soft), var(--gold));
    color:#382a0a;
    box-shadow: 0 6px 0 #a9791f, 0 8px 16px rgba(0,0,0,.35);
    transition: transform .08s ease, box-shadow .08s ease;
  }
  .btn:hover{transform:translateY(-1px);}
  .btn:active{transform:translateY(3px); box-shadow:0 3px 0 #a9791f, 0 4px 10px rgba(0,0,0,.3);}
  .btn:disabled{opacity:.4; cursor:not-allowed; transform:none; box-shadow:0 6px 0 #6b5417;}
  .btn.secondary{
    background:linear-gradient(180deg,#2c3b4d,#1c2735);
    color:var(--cream);
    box-shadow:0 6px 0 #0e141d, 0 8px 16px rgba(0,0,0,.3);
  }
  .btn.secondary:active{box-shadow:0 3px 0 #0e141d;}
  .center{text-align:center;}
  .row{display:flex; gap:12px; align-items:center; flex-wrap:wrap;}
  .between{justify-content:space-between;}
  .panel{
    background: linear-gradient(180deg, #141b25, #10151d);
    border:1px solid #232d3a;
    border-radius:var(--radius);
    padding:20px;
  }
  .subtitle{color:var(--muted); font-size:14px; margin:4px 0 20px;}
  .stat{font-family:'JetBrains Mono',monospace; color:var(--gold-soft);}

  /* MENU */
  .menu-hero{
    text-align:center;
    padding:20px 10px 8px;
  }
  .marble-row{font-size:38px; letter-spacing:6px; margin-bottom:10px;}
  .menu-hero p{color:var(--muted); max-width:480px; margin:10px auto 26px; line-height:1.5;}

  /* DICE */
  .dice-stage{display:flex; justify-content:center; gap:22px; padding:30px 0 10px;}
  .die{
    width:84px; height:84px; border-radius:18px;
    background: linear-gradient(145deg,#fdfdfd,#d8dee6);
    display:flex; align-items:center; justify-content:center;
    font-size:40px; font-weight:700; color:#1a2230;
    box-shadow: 0 10px 0 #98a3b3, 0 14px 20px rgba(0,0,0,.4);
    font-family:'JetBrains Mono',monospace;
  }
  .die.rolling{animation: shake .12s infinite;}
  @keyframes shake{
    0%{transform:rotate(0deg) translateY(0);}
    25%{transform:rotate(-8deg) translateY(-4px);}
    50%{transform:rotate(6deg) translateY(2px);}
    75%{transform:rotate(-4deg) translateY(-2px);}
    100%{transform:rotate(0deg) translateY(0);}
  }
  .dice-sum{text-align:center; font-size:20px; margin-top:14px; color:var(--muted);}
  .dice-sum b{color:var(--marble); font-size:26px;}

  /* PLANE */
  .plane-wrap{display:flex; gap:22px; flex-wrap:wrap;}
  .plane-board{
    position:relative;
    flex:1 1 480px;
    background: radial-gradient(circle at 50% 0%, var(--felt-2), var(--felt) 70%);
    border-radius:var(--radius);
    border:6px solid #0d2b21;
    padding:14px;
    min-height:340px;
  }
  .grid{
    display:grid;
    grid-template-columns: repeat(6, 1fr);
    gap:6px;
  }
  .cell{
    aspect-ratio:1;
    border-radius:8px;
    background: linear-gradient(180deg, #1c4636, #143229);
    border:1px solid var(--felt-line);
    display:flex; align-items:center; justify-content:center;
    text-align:center;
    font-size:9px;
    line-height:1.15;
    padding:2px;
    color:var(--muted);
    position:relative;
    transition: background .3s, box-shadow .3s;
  }
  .cell.good{color:#bff0d3;}
  .cell.bad{color:#f4c2be;}
  .cell.hit{
    box-shadow: 0 0 0 2px var(--gold), 0 0 14px rgba(232,185,74,.7) inset;
    background: linear-gradient(180deg,#2c5b47,#1c4636);
  }
  .marble{
    position:absolute;
    width:20px; height:20px; border-radius:50%;
    background: radial-gradient(circle at 32% 28%, #cfe6ff, var(--marble) 55%, var(--marble-2) 100%);
    box-shadow: 0 4px 6px rgba(0,0,0,.5);
    pointer-events:none;
    z-index:5;
    top:-24px; left:0;
  }
  .plane-side{flex:1 1 220px; min-width:220px;}
  .tally-list{list-style:none; margin:0; padding:0; max-height:280px; overflow-y:auto;}
  .tally-list li{
    display:flex; justify-content:space-between;
    padding:6px 10px; border-radius:8px; font-size:13px;
    margin-bottom:4px;
  }
  .tally-list li.good{background:rgba(95,202,141,.12); color:var(--good);}
  .tally-list li.bad{background:rgba(224,98,92,.12); color:var(--bad);}

  /* MODIFIER SUMMARY */
  .mod-grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(190px,1fr)); gap:10px; margin:18px 0;}
  .mod-card{
    background:#141b25; border:1px solid #232d3a; border-radius:12px; padding:12px 14px;
  }
  .mod-card .k{font-size:12px; color:var(--muted); text-transform:uppercase; letter-spacing:.06em;}
  .mod-card .v{font-family:'JetBrains Mono',monospace; font-size:20px; color:var(--gold-soft); margin-top:2px;}

  /* MATCHING */
  .match-top{display:flex; justify-content:space-between; align-items:center; margin-bottom:16px; flex-wrap:wrap; gap:10px;}
  .fail-dots{display:flex; gap:6px;}
  .fail-dot{width:12px; height:12px; border-radius:50%; background:#2c3b4d; border:1px solid #3a4a5e;}
  .fail-dot.used{background:var(--bad); border-color:var(--bad);}
  .preview-banner{
    text-align:center; padding:10px 16px; margin-bottom:14px;
    background: linear-gradient(180deg, rgba(232,185,74,.14), rgba(232,185,74,.05));
    border:1px solid rgba(232,185,74,.4);
    border-radius:12px;
    color:var(--gold-soft);
    font-family:'JetBrains Mono', monospace;
    font-size:14px;
    display:none;
  }
  .preview-banner.active{display:block;}
  .preview-banner .n{font-size:20px; margin-left:6px;}
  .card-grid{display:grid; gap:10px;}
  .card{
    aspect-ratio:1;
    border-radius:10px;
    cursor:pointer;
    perspective:600px;
  }
  .card-inner{
    width:100%; height:100%;
    position:relative;
    transform-style:preserve-3d;
    transition: transform .35s ease;
  }
  .card.flipped .card-inner, .card.matched .card-inner{transform:rotateY(180deg);}
  .card-face{
    position:absolute; inset:0;
    border-radius:10px;
    display:flex; align-items:center; justify-content:center;
    font-size:26px;
    backface-visibility:hidden;
  }
  .card-back{
    background: linear-gradient(145deg, var(--card-back), #182233);
    border:1px solid #33415a;
  }
  .card-back::after{content:'?'; color:#4a5a75; font-weight:700; font-size:22px; font-family:'JetBrains Mono',monospace;}
  .card-front{
    background: linear-gradient(145deg,#fdfdfd,#dbe2ea);
    transform:rotateY(180deg);
  }
  .card.matched .card-front{background: linear-gradient(145deg,#c6f3d7,#8fe0ac);}

  /* SPIN */
  .spin-wrap{display:flex; flex-direction:column; align-items:center; gap:18px;}
  .reel-window{
    width:100%; max-width:520px; height:96px;
    overflow:hidden; position:relative;
    border-radius:14px;
    background: linear-gradient(180deg,#141b25,#0d1219);
    border:2px solid var(--gold);
  }
  .reel-window::before, .reel-window::after{
    content:''; position:absolute; top:0; bottom:0; width:40%;
    z-index:2; pointer-events:none;
  }
  .reel-window::before{left:0; background:linear-gradient(90deg, #0d1219, transparent);}
  .reel-window::after{right:0; background:linear-gradient(-90deg, #0d1219, transparent);}
  .reel-track{
    position:absolute; top:0; left:0; height:100%;
    display:flex; align-items:center;
    transition: transform 2.6s cubic-bezier(.12,.85,.15,1);
  }
  .reel-tile{
    width:96px; height:76px; margin:0 6px;
    border-radius:10px;
    background: linear-gradient(180deg,#1f4c3d,#173a2f);
    border:1px solid var(--felt-line);
    display:flex; align-items:center; justify-content:center;
    font-family:'JetBrains Mono',monospace; font-weight:700; font-size:22px;
    color:var(--gold-soft);
    flex:0 0 auto;
  }
  .reel-tile.bonus{background:linear-gradient(180deg,#3a2c5c,#241a3d); color:#c9b3ff; font-size:13px; text-align:center;}
  .reel-pointer{
    position:absolute; left:50%; top:-10px; transform:translateX(-50%);
    width:0; height:0; border-left:10px solid transparent; border-right:10px solid transparent;
    border-top:14px solid var(--gold); z-index:3;
  }
  .spin-stats{display:flex; gap:26px; font-family:'JetBrains Mono',monospace;}
  .spin-stats div{text-align:center;}
  .spin-stats .n{font-size:26px; color:var(--gold-soft);}
  .spin-stats .l{font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:.06em;}

  /* PLINKO */
  #plinko-canvas{
    width:100%; max-width:640px; display:block; margin:0 auto;
    background: linear-gradient(180deg,#101820,#0b1218);
    border-radius:16px; border:2px solid #26333f;
  }
  .plinko-legend{display:flex; justify-content:center; gap:8px; margin-top:12px; flex-wrap:wrap;}
  .plinko-legend span{
    font-family:'JetBrains Mono',monospace; font-size:12px; padding:4px 10px; border-radius:999px;
    background:#141b25; border:1px solid #232d3a; color:var(--muted);
  }

  /* RESULT */
  .result-score{
    text-align:center; padding:36px 10px;
  }
  .result-score .big{
    font-family:'JetBrains Mono',monospace; font-size:64px; color:var(--gold-soft);
    text-shadow:0 0 26px rgba(232,185,74,.4);
  }
  .result-score .label{color:var(--muted); letter-spacing:.1em; text-transform:uppercase; font-size:13px;}
  .countdown{color:var(--muted); font-size:13px; margin-top:8px;}

  ::-webkit-scrollbar{width:8px;}
  ::-webkit-scrollbar-thumb{background:#2c3b4d; border-radius:4px;}
</style>
</head>
<body>
<div id="app">
  <header>
    <h1>Qw<span>o</span>bble</h1>
    <div class="tag" id="header-tag">roll &middot; wobble &middot; win</div>
  </header>

  <!-- MENU -->
  <section class="screen active" id="screen-menu">
    <div class="menu-hero">
      <div class="marble-row">🔵🟢🟣🟡🔴</div>
      <p>Roll two dice for marbles, wobble them across a board of modifiers, survive the matching game, spin for points, and cash out on the Plinko wall.</p>
      <button class="btn" onclick="Qwobble.startGame()">Roll the Dice</button>
    </div>
  </section>

  <!-- DICE -->
  <section class="screen" id="screen-dice">
    <h2 class="center" style="margin-top:0;">First Roll</h2>
    <p class="subtitle center">Your dice total is how many marbles you'll wobble onto the board.</p>
    <div class="dice-stage">
      <div class="die" id="die1">-</div>
      <div class="die" id="die2">-</div>
    </div>
    <div class="dice-sum" id="dice-sum-text"></div>
    <div class="center" style="margin-top:22px;">
      <button class="btn" id="btn-roll-dice" onclick="Qwobble.rollDice()">Roll</button>
    </div>
  </section>

  <!-- PLANE -->
  <section class="screen" id="screen-plane">
    <h2 style="margin-top:0;">The Wobble Board</h2>
    <p class="subtitle">Your marbles are dropping onto a randomized grid of modifiers.</p>
    <div class="plane-wrap">
      <div class="plane-board" id="plane-board">
        <div class="grid" id="plane-grid"></div>
      </div>
      <div class="plane-side">
        <div class="panel">
          <div class="k" style="color:var(--muted); font-size:12px; text-transform:uppercase; letter-spacing:.06em;">Marbles landed</div>
          <div class="stat" style="font-size:26px;"><span id="marbles-landed">0</span> / <span id="marbles-total">0</span></div>
          <ul class="tally-list" id="tally-list" style="margin-top:14px;"></ul>
        </div>
        <div class="center" style="margin-top:16px;">
          <button class="btn" id="btn-continue-mods" onclick="Qwobble.showModifierSummary()" disabled>Continue</button>
        </div>
      </div>
    </div>
  </section>

  <!-- MODIFIER SUMMARY -->
  <section class="screen" id="screen-modifiers">
    <h2 style="margin-top:0;">Modifiers Settled</h2>
    <p class="subtitle">Good and bad modifiers of the same kind cancel each other out.</p>
    <div class="mod-grid" id="mod-summary-grid"></div>
    <div class="center">
      <button class="btn" onclick="Qwobble.startMatching()">Start Matching Game</button>
    </div>
  </section>

  <!-- MATCHING -->
  <section class="screen" id="screen-matching">
    <div class="match-top">
      <div>
        <h2 style="margin:0;">Matching Game</h2>
        <p class="subtitle" style="margin:4px 0 0;">Clear the board without running out of attempts.</p>
      </div>
      <div>
        <div style="font-size:12px; color:var(--muted); text-transform:uppercase; letter-spacing:.06em; text-align:right;">Fails</div>
        <div class="fail-dots" id="fail-dots"></div>
      </div>
    </div>
    <div class="preview-banner" id="preview-banner">Memorize the board! Flipping face-down in <span class="n" id="preview-count">4</span></div>
    <div class="card-grid" id="card-grid"></div>
  </section>

  <!-- SPIN -->
  <section class="screen" id="screen-spin">
    <h2 class="center" style="margin-top:0;">Points Spin</h2>
    <p class="subtitle center">Every spin adds to your total. Withdraw once you're out of spins.</p>
    <div class="spin-wrap">
      <div class="reel-window" id="reel-window">
        <div class="reel-pointer"></div>
        <div class="reel-track" id="reel-track"></div>
      </div>
      <div class="spin-stats">
        <div><div class="n" id="spins-left">0</div><div class="l">Spins Left</div></div>
        <div><div class="n" id="spin-points">0</div><div class="l">Points</div></div>
        <div><div class="n" id="withdraw-mult">x1</div><div class="l">Withdrawal</div></div>
      </div>
      <div class="row" style="justify-content:center;">
        <button class="btn" id="btn-spin" onclick="Qwobble.doSpin()">Spin</button>
        <button class="btn secondary" id="btn-withdraw" onclick="Qwobble.withdraw()" disabled>Withdraw</button>
      </div>
    </div>
  </section>

  <!-- PLINKO -->
  <section class="screen" id="screen-plinko">
    <h2 class="center" style="margin-top:0;">Final Drop: Plinko</h2>
    <p class="subtitle center" id="plinko-sub"></p>
    <canvas id="plinko-canvas" width="640" height="440"></canvas>
    <div class="plinko-legend" id="plinko-legend"></div>
    <div class="center" style="margin-top:16px;">
      <button class="btn" id="btn-drop" onclick="Qwobble.startPlinko()">Drop Balls</button>
    </div>
  </section>

  <!-- RESULT -->
  <section class="screen" id="screen-result">
    <div class="result-score">
      <div class="label">Final Score</div>
      <div class="big" id="final-score">0</div>
      <div class="countdown" id="countdown-text">Returning to menu in 5s...</div>
    </div>
  </section>
</div>

<script>
(function(){
  "use strict";

  // ---------- Modifier pool ----------
  const MOD_POOL = [
    {key:'die', label:'+1 Die', type:'good', group:'die', val:1},
    {key:'roll', label:'+1 Roll', type:'good', group:'roll', val:1},
    {key:'spin', label:'+1 Spin', type:'good', group:'spin', val:1},
    {key:'luck', label:'+1 Luck', type:'good', group:'luck', val:1},
    {key:'match', label:'+1 Match', sub:'+2 guaranteed cards', type:'good', group:'match', val:1},
    {key:'nonmatch', label:'-1 Non-Matching Cards', type:'good', group:'cards', val:-1},
    {key:'gravDown', label:'-1 Gravity', type:'good', group:'gravity', val:-1},
    {key:'bounce', label:'+1 Bounce', type:'good', group:'bounce', val:1},
    {key:'gravUp', label:'+1 Gravity', type:'good', group:'gravity', val:1},
    {key:'withUp', label:'x2 Withdrawal', type:'good', group:'withdraw', val:1},
    {key:'matchAtt', label:'+1 Matching Attempt', type:'good', group:'matchAtt', val:1},
    {key:'rowUp', label:'+1 Plinko Row', type:'good', group:'row', val:1},

    {key:'rollDown', label:'-1 Roll', type:'bad', group:'roll', val:-1},
    {key:'cardsUp', label:'+2 Matching Cards', type:'bad', group:'cards', val:2},
    {key:'withDown', label:'/2 Withdrawal', type:'bad', group:'withdraw', val:-1},
    {key:'bounceDown', label:'-1 Bounce', type:'bad', group:'bounce', val:-1},
    {key:'luckDown', label:'-1 Luck', type:'bad', group:'luck', val:-1},
    {key:'dieDown', label:'/2 Dice', type:'bad', group:'die', val:-1},
    {key:'matchAttDown', label:'-1 Matching Attempt', type:'bad', group:'matchAtt', val:-1},
    {key:'rowDown', label:'-1 Plinko Row', type:'bad', group:'row', val:-1},
  ];

  const GRID_COLS = 6, GRID_ROWS = 5;

  // ---------- State ----------
  let S = {};
  function freshState(){
    S = {
      diceSum:0,
      marbleCount:0,
      collected:[],           // list of mod objects landed on
      mods:{},                 // netted values
      grid:[],                 // grid cell -> mod
      allowedFails:3,
      fails:0,
      pairs:0,
      cardsData:[],
      flipped:[],
      lockBoard:false,
      matchedPairs:0,
      spinsLeft:0,
      spinPoints:0,
      withdrawMult:1,
      withdrawnPoints:0,
      plinkoRows:12,
    };
  }
  freshState();

  function rand(n){ return Math.floor(Math.random()*n); }
  function pick(arr){ return arr[rand(arr.length)]; }
  function clamp(v,lo,hi){ return Math.max(lo, Math.min(hi, v)); }

  function showScreen(id){
    document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  // ---------- Flow: menu -> dice ----------
  function startGame(){
    freshState();
    document.getElementById('btn-roll-dice').disabled = false;
    document.getElementById('btn-roll-dice').textContent = 'Roll';
    document.getElementById('die1').textContent = '-';
    document.getElementById('die2').textContent = '-';
    document.getElementById('dice-sum-text').innerHTML = '';
    showScreen('screen-dice');
  }

  function rollDice(){
    const btn = document.getElementById('btn-roll-dice');
    btn.disabled = true;
    const d1 = document.getElementById('die1');
    const d2 = document.getElementById('die2');
    d1.classList.add('rolling'); d2.classList.add('rolling');
    let ticks = 0;
    const iv = setInterval(()=>{
      d1.textContent = 1+rand(6);
      d2.textContent = 1+rand(6);
      ticks++;
      if(ticks>14){
        clearInterval(iv);
        d1.classList.remove('rolling'); d2.classList.remove('rolling');
        const v1 = 1+rand(6), v2 = 1+rand(6);
        d1.textContent = v1; d2.textContent = v2;
        S.diceSum = v1+v2;
        S.marbleCount = S.diceSum;
        document.getElementById('dice-sum-text').innerHTML =
          `Rolled <b>${v1} + ${v2} = ${S.diceSum}</b> &mdash; that's ${S.diceSum} marbles`;
        setTimeout(()=>{ goToPlane(); }, 900);
      }
    }, 70);
  }

  // ---------- Plane / marble wobble ----------
  function buildGrid(){
    S.grid = [];
    for(let i=0;i<GRID_COLS*GRID_ROWS;i++){
      S.grid.push(pick(MOD_POOL));
    }
  }

  function renderGrid(){
    const el = document.getElementById('plane-grid');
    el.innerHTML = '';
    S.grid.forEach((m, idx)=>{
      const c = document.createElement('div');
      c.className = 'cell ' + m.type;
      c.dataset.idx = idx;
      c.innerHTML = m.label + (m.sub? `<br><span style="opacity:.6">${m.sub}</span>`:'');
      el.appendChild(c);
    });
  }

  function goToPlane(){
    buildGrid();
    renderGrid();
    S.collected = [];
    document.getElementById('marbles-total').textContent = S.marbleCount;
    document.getElementById('marbles-landed').textContent = '0';
    document.getElementById('tally-list').innerHTML = '';
    document.getElementById('btn-continue-mods').disabled = true;
    showScreen('screen-plane');
    setTimeout(dropNextMarble, 500);
  }

  function dropNextMarble(){
    const landed = S.collected.length;
    if(landed >= S.marbleCount){
      document.getElementById('btn-continue-mods').disabled = false;
      return;
    }
    const board = document.getElementById('plane-board');
    const grid = document.getElementById('plane-grid');
    const cells = grid.querySelectorAll('.cell');
    const targetIdx = rand(cells.length);
    const targetCell = cells[targetIdx];
    const boardRect = board.getBoundingClientRect();
    const cellRect = targetCell.getBoundingClientRect();

    const marble = document.createElement('div');
    marble.className = 'marble';
    const startX = boardRect.width/2 - 10 + (rand(40)-20);
    marble.style.left = startX + 'px';
    marble.style.top = '-24px';
    board.appendChild(marble);

    const endX = cellRect.left - boardRect.left + cellRect.width/2 - 10;
    const endY = cellRect.top - boardRect.top + cellRect.height/2 - 10;

    requestAnimationFrame(()=>{
      marble.style.transition = 'left .55s cubic-bezier(.3,.6,.4,1), top .55s cubic-bezier(.5,0,.3,1)';
      marble.style.left = endX + 'px';
      marble.style.top = endY + 'px';
    });

    setTimeout(()=>{
      marble.style.transition = 'top .18s ease, transform .18s ease';
      marble.style.top = (endY-6)+'px';
      setTimeout(()=>{ marble.style.top = endY+'px'; }, 180);

      targetCell.classList.add('hit');
      const mod = S.grid[targetIdx];
      S.collected.push(mod);
      document.getElementById('marbles-landed').textContent = S.collected.length;
      renderTally();

      setTimeout(()=>{ marble.remove(); dropNextMarble(); }, 380);
    }, 560);
  }

  function renderTally(){
    const counts = {};
    S.collected.forEach(m=>{
      const k = m.key;
      counts[k] = counts[k] || {mod:m, n:0};
      counts[k].n++;
    });
    const list = document.getElementById('tally-list');
    list.innerHTML = '';
    Object.values(counts).forEach(({mod,n})=>{
      const li = document.createElement('li');
      li.className = mod.type;
      li.innerHTML = `<span>${mod.label}</span><span class="mono">x${n}</span>`;
      list.appendChild(li);
    });
  }

  // ---------- Modifier netting ----------
  function computeMods(){
    const net = {};
    S.collected.forEach(m=>{
      net[m.group] = (net[m.group]||0) + m.val;
    });
    const g = k => net[k]||0;

    const diceCount = clamp(2 + g('die'), 1, 6);
    const rollsBase = clamp(2 + g('roll'), 0, 12);
    const spinPerRoll = clamp(1 + g('spin'), 0, 6);
    const totalSpins = rollsBase * spinPerRoll;
    const luck = g('luck');
    const bounceMin = clamp(2 + g('bounce'), 1, 20);
    const bounceMax = clamp(9 + g('bounce'), bounceMin+1, 30);
    const gravity = g('gravity');
    const withdraw = g('withdraw');
    const withdrawMult = withdraw>0 ? 2 : (withdraw<0 ? 0.5 : 1);
    const matchAttempts = clamp(5 + g('matchAtt'), 1, 12);
    const plinkoRows = clamp(12 + g('row'), 4, 20);
    const cardsAdjust = g('cards');
    const guaranteedPairs = g('match');

    S.mods = {
      diceCount, rollsBase, spinPerRoll, totalSpins, luck,
      bounceMin, bounceMax, gravity, withdraw, withdrawMult,
      matchAttempts, plinkoRows, cardsAdjust, guaranteedPairs
    };
    S.allowedFails = matchAttempts;
    S.spinsLeft = totalSpins;
    S.withdrawMult = withdrawMult;
    S.plinkoRows = plinkoRows;
  }

  function showModifierSummary(){
    computeMods();
    const m = S.mods;
    const grid = document.getElementById('mod-summary-grid');
    const withdrawLabel = m.withdrawMult===2?'x2':(m.withdrawMult===0.5?'/2':'x1');
    const entries = [
      ['Dice (2nd roll)', m.diceCount + ' die'],
      ['Rolls', m.rollsBase],
      ['Spins per Roll', m.spinPerRoll],
      ['Total Spins', m.totalSpins],
      ['Luck', (m.luck>0?'+':'')+m.luck],
      ['Bounces', `${m.bounceMin}\u2013${m.bounceMax}`],
      ['Gravity', (m.gravity>0?'+':'')+m.gravity],
      ['Withdrawal', withdrawLabel],
      ['Matching Attempts', m.matchAttempts],
      ['Plinko Rows', m.plinkoRows],
      ['Card Count Shift', (m.cardsAdjust>0?'+':'')+m.cardsAdjust],
      ['Guaranteed Pairs', m.guaranteedPairs],
    ];
    grid.innerHTML = '';
    entries.forEach(([k,v])=>{
      const d = document.createElement('div');
      d.className = 'mod-card';
      d.innerHTML = `<div class="k">${k}</div><div class="v">${v}</div>`;
      grid.appendChild(d);
    });
    showScreen('screen-modifiers');
  }

  // ---------- Matching game ----------
  const SYMBOLS = ['🔵','🔴','🟢','🟡','🟣','🟠','⚪','🟤','🔶','🔷','⭐','💠'];

  const PREVIEW_SECONDS = 4;

  function startMatching(){
    let totalCards = 16 + S.mods.cardsAdjust;
    totalCards = clamp(totalCards, 6, 32);
    if(totalCards % 2 !== 0) totalCards += 1;
    let pairs = totalCards/2;

    let guaranteed = clamp(S.mods.guaranteedPairs, 0, pairs-1<0?0:pairs-1);
    S.pairs = pairs;
    S.fails = 0;
    S.matchedPairs = 0;
    S.flipped = [];
    S.lockBoard = true;

    const symbolSet = [];
    for(let i=0;i<pairs;i++) symbolSet.push(SYMBOLS[i % SYMBOLS.length] + (Math.floor(i/SYMBOLS.length)||''));

    let deck = [];
    symbolSet.forEach((s,i)=>{ deck.push({id:i, sym:s}); deck.push({id:i, sym:s}); });
    // shuffle
    for(let i=deck.length-1;i>0;i--){ const j = rand(i+1); [deck[i],deck[j]]=[deck[j],deck[i]]; }

    S.cardsData = deck.map((c,i)=>({...c, cardIndex:i, matched:false}));

    // Pre-select guaranteed pairs so they stay revealed after the preview ends.
    if(guaranteed>0){
      const ids = [...new Set(S.cardsData.map(c=>c.id))];
      for(let i=0;i<guaranteed && i<ids.length;i++){
        S.cardsData.filter(c=>c.id===ids[i]).forEach(c=>{ c.matched = true; });
        S.matchedPairs++;
      }
    }

    renderFailDots();
    renderCardGrid();
    showScreen('screen-matching');
    runPreview(guaranteed);
  }

  function runPreview(guaranteed){
    const banner = document.getElementById('preview-banner');
    const countEl = document.getElementById('preview-count');
    banner.classList.add('active');
    let t = PREVIEW_SECONDS;
    countEl.textContent = t;
    const iv = setInterval(()=>{
      t--;
      if(t<=0){
        clearInterval(iv);
        banner.classList.remove('active');
        endPreview(guaranteed);
      } else {
        countEl.textContent = t;
      }
    }, 1000);
  }

  function endPreview(guaranteed){
    S.cardsData.forEach(c=>{
      if(!c.matched){
        const el = cardEl(c.cardIndex);
        el.classList.remove('flipped');
      }
    });
    S.lockBoard = false;
    if(guaranteed>0){
      const banner = document.getElementById('preview-banner');
      banner.textContent = `${guaranteed} pair${guaranteed>1?'s':''} auto-matched from your +1 Match modifiers!`;
      banner.classList.add('active');
      setTimeout(()=>banner.classList.remove('active'), 2200);
    }
    checkMatchWin();
  }

  function renderFailDots(){
    const el = document.getElementById('fail-dots');
    el.innerHTML = '';
    for(let i=0;i<S.allowedFails;i++){
      const d = document.createElement('div');
      d.className = 'fail-dot' + (i<S.fails? ' used':'');
      el.appendChild(d);
    }
  }

  function renderCardGrid(){
    const grid = document.getElementById('card-grid');
    const n = S.cardsData.length;
    const cols = Math.min(8, Math.ceil(Math.sqrt(n*1.6)));
    grid.style.gridTemplateColumns = `repeat(${cols}, 1fr)`;
    grid.innerHTML = '';
    S.cardsData.forEach(c=>{
      const el = document.createElement('div');
      el.className = 'card' + (c.matched? ' flipped matched':' flipped');
      el.dataset.index = c.cardIndex;
      el.innerHTML = `<div class="card-inner">
        <div class="card-face card-back"></div>
        <div class="card-face card-front">${c.sym.replace(/\d+$/,'')}</div>
      </div>`;
      el.addEventListener('click', ()=>flipCard(c.cardIndex));
      grid.appendChild(el);
    });
  }

  function cardEl(idx){ return document.querySelector(`.card[data-index="${idx}"]`); }

  function flipCard(idx){
    if(S.lockBoard) return;
    const data = S.cardsData[idx];
    if(data.matched) return;
    const el = cardEl(idx);
    if(el.classList.contains('flipped')) return;
    if(S.flipped.length>=2) return;

    el.classList.add('flipped');
    S.flipped.push(idx);

    if(S.flipped.length===2){
      S.lockBoard = true;
      const [a,b] = S.flipped;
      const da = S.cardsData[a], db = S.cardsData[b];
      if(da.id === db.id){
        setTimeout(()=>{
          da.matched = db.matched = true;
          cardEl(a).classList.add('matched');
          cardEl(b).classList.add('matched');
          S.matchedPairs++;
          S.flipped = [];
          S.lockBoard = false;
          checkMatchWin();
        }, 450);
      } else {
        S.fails++;
        renderFailDots();
        setTimeout(()=>{
          cardEl(a).classList.remove('flipped');
          cardEl(b).classList.remove('flipped');
          S.flipped = [];
          S.lockBoard = false;
          if(S.fails >= S.allowedFails){
            setTimeout(failMatching, 400);
          }
        }, 700);
      }
    }
  }

  function checkMatchWin(){
    if(S.matchedPairs >= S.pairs){
      setTimeout(()=>{ goToSpin(); }, 600);
    }
  }

  function failMatching(){
    alert('Out of attempts! Back to the dice you go.');
    startGame();
  }

  // ---------- Spin ----------
  const BASE_SEGMENTS = [
    {label:'10', type:'add', val:10},
    {label:'25', type:'add', val:25},
    {label:'50', type:'add', val:50},
    {label:'\u00d72', type:'mult', val:2},
    {label:'\u00d75', type:'mult', val:5},
  ];

  function buildReelSegments(){
    const segs = BASE_SEGMENTS.slice();
    if(S.mods.spinPerRoll > 0){
      segs.push({label:'BONUS DICE', type:'bonus'});
    }
    return segs;
  }

  function weightedPick(segs){
    const tierLow = ['10','25'];
    const tierHigh = ['\u00d75','BONUS DICE'];
    const luck = S.mods.luck||0;
    const weights = segs.map(s=>{
      let w = 10;
      if(tierLow.includes(s.label)) w += -luck*2.5;
      if(tierHigh.includes(s.label)) w += luck*2.5;
      return Math.max(1, w);
    });
    const total = weights.reduce((a,b)=>a+b,0);
    let r = Math.random()*total;
    for(let i=0;i<segs.length;i++){
      r -= weights[i];
      if(r<=0) return i;
    }
    return segs.length-1;
  }

  function goToSpin(){
    S.spinPoints = 0;
    S.spinsLeft = S.mods.totalSpins;
    document.getElementById('spins-left').textContent = S.spinsLeft;
    document.getElementById('spin-points').textContent = S.spinPoints;
    document.getElementById('withdraw-mult').textContent =
      S.withdrawMult===2?'x2':(S.withdrawMult===0.5?'/2':'x1');
    document.getElementById('btn-spin').disabled = false;
    document.getElementById('btn-withdraw').disabled = false;

    buildReel(null);
    showScreen('screen-spin');

    if(S.spinsLeft<=0){
      document.getElementById('btn-spin').disabled = true;
      setTimeout(()=>{
        alert('No spins available from your modifiers \u2014 skipping straight to withdrawal.');
      }, 200);
    }
  }

  function buildReel(landIndex){
    const segs = buildReelSegments();
    const track = document.getElementById('reel-track');
    track.style.transition = 'none';
    track.innerHTML = '';
    const loops = 6;
    const totalTiles = segs.length*loops + (landIndex!=null? (landIndex+1) : 0);
    let tiles = [];
    for(let i=0;i<totalTiles;i++) tiles.push(segs[i%segs.length]);
    if(landIndex==null){
      tiles = tiles.slice(0, segs.length*3);
    }
    tiles.forEach(s=>{
      const t = document.createElement('div');
      t.className = 'reel-tile' + (s.type==='bonus'?' bonus':'');
      t.textContent = s.label;
      track.appendChild(t);
    });
    track.style.transform = 'translateX(0px)';
    return {segs, tileWidth:108};
  }

  function doSpin(){
    if(S.spinsLeft<=0) return;
    document.getElementById('btn-spin').disabled = true;
    document.getElementById('btn-withdraw').disabled = true;

    const segs = buildReelSegments();
    const landIndex = weightedPick(segs);
    const {tileWidth} = buildReel(landIndex);
    const track = document.getElementById('reel-track');
    const windowWidth = document.getElementById('reel-window').offsetWidth || 520;
    const finalTileIndex = segs.length*6 + landIndex; // matches tile sequence built with loops=6 then extra
    const targetX = -(finalTileIndex*tileWidth) + (windowWidth/2 - tileWidth/2);

    requestAnimationFrame(()=>{
      track.style.transition = 'transform 2.6s cubic-bezier(.12,.85,.15,1)';
      track.style.transform = `translateX(${targetX}px)`;
    });

    setTimeout(()=>{
      applySpinResult(segs[landIndex]);
      S.spinsLeft--;
      document.getElementById('spins-left').textContent = S.spinsLeft;
      document.getElementById('spin-points').textContent = S.spinPoints;
      document.getElementById('btn-withdraw').disabled = false;
      if(S.spinsLeft>0){
        document.getElementById('btn-spin').disabled = false;
      }
    }, 2750);
  }

  function applySpinResult(seg){
    if(seg.type==='add'){
      S.spinPoints += seg.val;
    } else if(seg.type==='mult'){
      S.spinPoints = Math.round(S.spinPoints * seg.val);
    } else if(seg.type==='bonus'){
      const n = S.mods.diceCount;
      let sum = 0;
      for(let i=0;i<n;i++) sum += 1+rand(6);
      const bonus = sum*10;
      S.spinPoints += bonus;
      setTimeout(()=>alert(`Bonus Dice Modifier! Rolled ${n}d6 = ${sum} \u2192 +${bonus} points`), 100);
    }
  }

  function withdraw(){
    S.withdrawnPoints = Math.floor(S.spinPoints * S.withdrawMult);
    goToPlinko();
  }

  // ---------- Plinko ----------
  const PLINKO_MULTS = [5,2,1,0.5,1,2,5];
  let plinkoBalls = [];
  let plinkoAnim = null;

  function goToPlinko(){
    document.getElementById('plinko-sub').textContent =
      `Withdrawn ${S.withdrawnPoints} points \u2014 ${S.plinkoRows} rows, 7 slots`;
    const legend = document.getElementById('plinko-legend');
    legend.innerHTML = '';
    PLINKO_MULTS.forEach(m=>{
      const s = document.createElement('span');
      s.textContent = 'x'+m;
      legend.appendChild(s);
    });
    document.getElementById('btn-drop').disabled = false;
    drawPlinkoBoard();
    showScreen('screen-plinko');
  }

  function plinkoLayout(){
    const canvas = document.getElementById('plinko-canvas');
    const W = canvas.width, H = canvas.height;
    const rows = S.plinkoRows;
    const topY = 40, bottomY = H-70;
    const rowGap = (bottomY-topY)/rows;
    const colGap = Math.min(28, (W-60)/rows);
    return {W,H,rows,topY,bottomY,rowGap,colGap,cx:W/2};
  }

  function drawPlinkoBoard(highlightBins){
    const canvas = document.getElementById('plinko-canvas');
    const ctx = canvas.getContext('2d');
    const {W,H,rows,topY,rowGap,colGap,cx} = plinkoLayout();
    ctx.clearRect(0,0,W,H);
    // pegs
    ctx.fillStyle = '#e8b94a';
    for(let r=0;r<rows;r++){
      const y = topY + r*rowGap;
      const count = r+2;
      for(let i=0;i<count;i++){
        const x = cx + (i-(count-1)/2)*colGap;
        ctx.beginPath();
        ctx.arc(x,y,3.2,0,Math.PI*2);
        ctx.fill();
      }
    }
    // slots
    const slotY = topY + rows*rowGap + 6;
    const slotW = (rows+2)*colGap/7;
    const startX = cx - (rows+2)*colGap/2;
    for(let i=0;i<7;i++){
      const x = startX + i*slotW;
      ctx.fillStyle = highlightBins && highlightBins[i] ? '#e8b94a' : '#1c4636';
      ctx.strokeStyle = '#2c6551';
      ctx.beginPath();
      ctx.roundRect ? ctx.roundRect(x+2, slotY, slotW-4, 40, 6) : ctx.rect(x+2, slotY, slotW-4, 40);
      ctx.fill(); ctx.stroke();
      ctx.fillStyle = highlightBins && highlightBins[i] ? '#382a0a' : '#bff0d3';
      ctx.font = '600 15px JetBrains Mono, monospace';
      ctx.textAlign = 'center';
      ctx.fillText('x'+PLINKO_MULTS[i], x+slotW/2, slotY+25);
    }
    // draw active balls
    if(plinkoBalls){
      plinkoBalls.forEach(b=>{
        if(b.done) return;
        ctx.beginPath();
        ctx.fillStyle = b.color;
        ctx.arc(b.x, b.y, 6, 0, Math.PI*2);
        ctx.fill();
      });
    }
  }

  function computeBalls(){
    let pts = S.withdrawnPoints;
    let balls = [];
    let tenCount = Math.floor(pts/10);
    let remainder = pts % 10;
    const MAX_SIM = 18;
    let simCount = Math.min(tenCount, MAX_SIM);
    for(let i=0;i<simCount;i++) balls.push({value:10});
    let extraAutoValue = 0;
    if(tenCount > MAX_SIM){
      extraAutoValue += (tenCount-MAX_SIM)*10;
    }
    if(remainder>0){
      if(balls.length < MAX_SIM+1){
        balls.push({value:remainder});
      } else {
        extraAutoValue += remainder;
      }
    }
    if(balls.length===0 && pts===0){
      balls.push({value:0});
    }
    return {balls, extraAutoValue};
  }

  function binForOffset(offset, rows){
    const norm = Math.abs(offset)/rows;
    let tier;
    if(norm > 0.72) tier = 0;
    else if(norm > 0.42) tier = 1;
    else if(norm > 0.14) tier = 2;
    else tier = 3;
    if(tier===3) return 3;
    return offset < 0 ? (3-tier) : (3+tier);
  }

  const PLINKO_ROW_MS = 260;      // time to travel from one peg row to the next
  const PLINKO_RELEASE_MS = 340;  // stagger between balls being let off

  function easeInOutQuad(t){
    return t<0.5 ? 2*t*t : 1-Math.pow(-2*t+2,2)/2;
  }

  function startPlinko(){
    document.getElementById('btn-drop').disabled = true;
    const {balls, extraAutoValue} = computeBalls();
    const {rows,topY,rowGap,colGap,cx} = plinkoLayout();
    const colors = ['#4f8ef7','#e8b94a','#5fca8d','#e0625c','#c9b3ff','#f3d489'];

    plinkoBalls = balls.map((b,i)=>{
      // Each row is a fresh 50/50 bounce off the peg dead-center of it,
      // sending the ball toward the center of the next peg down the line.
      const path = [];
      let offset = 0;
      for(let r=0;r<rows;r++){
        offset += Math.random()<0.5 ? -1 : 1;
        path.push(offset);
      }
      const bin = binForOffset(offset, rows);
      return {
        value:b.value, path, bin, x:cx, y:topY-10,
        done:false, color:colors[i%colors.length],
        delay:i*PLINKO_RELEASE_MS
      };
    });

    let score = extraAutoValue * 1; // overflow balls that skipped animation, counted at face value
    const startTime = performance.now();

    function animate(now){
      let allDone = true;
      const {rows,topY,rowGap,colGap,cx} = plinkoLayout();
      plinkoBalls.forEach(b=>{
        const elapsed = now - startTime - b.delay;
        if(elapsed < 0){ allDone=false; return; }
        const rowFloat = elapsed / PLINKO_ROW_MS;
        if(rowFloat < rows){
          allDone = false;
          const r = Math.floor(rowFloat);
          const frac = rowFloat - r;
          const eased = easeInOutQuad(frac);
          const offsetAtR = b.path[Math.min(r, b.path.length-1)];
          const prevOffset = r>0? b.path[r-1] : 0;
          const curOffset = prevOffset + (offsetAtR-prevOffset)*eased;
          b.x = cx + curOffset*colGap*0.5;
          // small hop between pegs to sell the bounce
          const hop = Math.sin(frac*Math.PI) * (rowGap*0.22);
          b.y = topY + (r+frac)*rowGap - hop;
        } else if(!b.done){
          b.done = true;
          const mult = PLINKO_MULTS[b.bin];
          score += b.value * mult;
        }
      });
      drawPlinkoBoard();
      if(!allDone){
        plinkoAnim = requestAnimationFrame(animate);
      } else {
        finishPlinko(Math.round(score));
      }
    }
    plinkoAnim = requestAnimationFrame(animate);
  }

  function finishPlinko(score){
    setTimeout(()=>{
      showResult(score);
    }, 500);
  }

  // ---------- Result ----------
  function showResult(score){
    document.getElementById('final-score').textContent = score;
    showScreen('screen-result');
    let t = 5;
    const cd = document.getElementById('countdown-text');
    cd.textContent = `Returning to menu in ${t}s...`;
    const iv = setInterval(()=>{
      t--;
      if(t<=0){
        clearInterval(iv);
        showScreen('screen-menu');
      } else {
        cd.textContent = `Returning to menu in ${t}s...`;
      }
    }, 1000);
  }

  // ---------- expose ----------
  window.Qwobble = {
    startGame, rollDice, showModifierSummary, startMatching,
    doSpin, withdraw, startPlinko
  };
})();
</script>
</body>
</html>
