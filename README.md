# -
<!DOCTYPE html>

<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>자리 배치 프로그램 - 2학년 8반</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700;900&family=Space+Mono:wght@700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0f0f14;
    --surface: #1a1a24;
    --surface2: #222230;
    --accent: #6ee7b7;
    --accent2: #f59e0b;
    --danger: #ef4444;
    --text: #e2e8f0;
    --muted: #64748b;
    --board: #1e3a5f;
    --board-light: #2563a8;
    --desk-empty: #1e293b;
    --desk-taken: #064e3b;
    --desk-priority: #78350f;
    --desk-border: #334155;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg); color: var(--text);
    font-family: 'Noto Sans KR', sans-serif;
    min-height: 100vh; display: flex; flex-direction: column; align-items: center;
    padding: 24px 16px 48px;
  }
  .header { text-align: center; margin-bottom: 32px; }
  .header h1 {
    font-size: 2rem; font-weight: 900; letter-spacing: -0.02em;
    background: linear-gradient(135deg, var(--accent), #34d399, var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
    margin-bottom: 6px;
  }
  .header p { color: var(--muted); font-size: 0.85rem; }

.main-layout { display: flex; gap: 24px; width: 100%; max-width: 1000px; align-items: flex-start; }
.left-panel { flex: 0 0 240px; display: flex; flex-direction: column; gap: 16px; }

.card { background: var(–surface); border: 1px solid var(–surface2); border-radius: 12px; padding: 16px; }
.card h3 { font-size: 0.75rem; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; color: var(–accent); margin-bottom: 12px; }

.priority-input-wrap { display: flex; gap: 6px; margin-bottom: 10px; }
.priority-input-wrap input {
flex: 1; background: var(–surface2); border: 1px solid var(–desk-border); border-radius: 8px;
color: var(–text); font-family: ‘Noto Sans KR’, sans-serif; font-size: 0.9rem; padding: 8px 10px; outline: none; transition: border-color 0.2s;
}
.priority-input-wrap input:focus { border-color: var(–accent2); }

.btn { padding: 8px 14px; border: none; border-radius: 8px; font-family: ‘Noto Sans KR’, sans-serif; font-weight: 700; font-size: 0.85rem; cursor: pointer; transition: opacity 0.15s, transform 0.1s; }
.btn:active { transform: scale(0.96); }
.btn-amber { background: var(–accent2); color: #0f0f14; }
.btn-danger { background: var(–danger); color: #fff; }
.btn-ghost { background: var(–surface2); color: var(–text); border: 1px solid var(–desk-border); width: 100%; padding: 10px; }
.btn-download { background: linear-gradient(135deg, #1d4ed8, #2563eb); color: #fff; border: none; width: 100%; padding: 10px; }

.priority-tags { display: flex; flex-wrap: wrap; gap: 6px; min-height: 32px; }
.tag {
display: inline-flex; align-items: center; gap: 4px;
background: var(–desk-priority); border: 1px solid var(–accent2); border-radius: 20px;
padding: 3px 10px 3px 8px; font-size: 0.78rem; color: var(–accent2); cursor: pointer; transition: background 0.15s;
}
.tag:hover { background: #92400e; }
.tag .remove { font-size: 0.9rem; line-height: 1; }

.queue-list { display: flex; flex-direction: column; gap: 5px; max-height: 200px; overflow-y: auto; }
.queue-item { display: flex; align-items: center; justify-content: space-between; background: var(–surface2); border-radius: 8px; padding: 7px 10px; font-size: 0.8rem; gap: 6px; }
.queue-num { font-family: ‘Space Mono’, monospace; color: var(–accent); font-weight: 700; min-width: 28px; font-size: 0.75rem; }
.queue-name { flex: 1; font-size: 0.82rem; }
.queue-pos { color: var(–muted); font-size: 0.68rem; white-space: nowrap; }
.queue-priority-badge { background: var(–accent2); color: #0f0f14; font-size: 0.62rem; font-weight: 700; padding: 2px 5px; border-radius: 10px; }

.actions { display: flex; flex-direction: column; gap: 8px; }
.stats { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.stat-box { background: var(–surface2); border-radius: 8px; padding: 10px; text-align: center; }
.stat-val { font-family: ‘Space Mono’, monospace; font-size: 1.4rem; font-weight: 700; color: var(–accent); }
.stat-label { font-size: 0.68rem; color: var(–muted); margin-top: 2px; }

.right-panel { flex: 1; display: flex; flex-direction: column; gap: 12px; }
.blackboard {
background: linear-gradient(135deg, var(–board), var(–board-light)); border-radius: 10px; padding: 12px;
text-align: center; font-weight: 700; font-size: 0.9rem; letter-spacing: 0.15em;
color: #bfdbfe; border: 2px solid var(–board-light); box-shadow: 0 4px 20px rgba(37,99,168,0.3);
}
.classroom { background: var(–surface); border-radius: 12px; padding: 20px; border: 1px solid var(–surface2); }
.col-labels { display: grid; grid-template-columns: repeat(6, 1fr); gap: 8px; margin-bottom: 4px; }
.col-label { text-align: center; font-size: 0.6rem; color: var(–muted); font-family: ‘Space Mono’, monospace; }
.grid { display: grid; grid-template-columns: repeat(6, 1fr); gap: 8px; }

.desk {
aspect-ratio: 1; border-radius: 8px; border: 1.5px solid var(–desk-border); background: var(–desk-empty);
display: flex; flex-direction: column; align-items: center; justify-content: center;
cursor: default; transition: background 0.2s, border-color 0.2s; position: relative; user-select: none;
padding: 4px;
}
.desk.taken { background: var(–desk-taken); border-color: var(–accent); animation: popIn 0.35s cubic-bezier(.175,.885,.32,1.275); }
.desk.priority-seat { background: var(–desk-priority); border-color: var(–accent2); animation: popIn 0.35s cubic-bezier(.175,.885,.32,1.275); }
@keyframes popIn { 0%{transform:scale(0.5);opacity:0} 70%{transform:scale(1.12)} 100%{transform:scale(1);opacity:1} }

.seat-label { font-size: 0.48rem; color: var(–muted); position: absolute; top: 3px; left: 5px; font-weight: 400; }
.desk.taken .seat-label, .desk.priority-seat .seat-label { color: rgba(255,255,255,0.2); }

.desk-num {
font-family: ‘Space Mono’, monospace; font-size: 0.7rem; font-weight: 700; line-height: 1;
color: var(–muted);
}
.desk.taken .desk-num { color: rgba(110,231,183,0.7); }
.desk.priority-seat .desk-num { color: rgba(245,158,11,0.7); }

.desk-name {
font-size: 0.72rem; font-weight: 700; line-height: 1.2; text-align: center;
margin-top: 2px; word-break: keep-all;
}
.desk.taken .desk-name { color: var(–accent); }
.desk.priority-seat .desk-name { color: var(–accent2); }

.legend { display: flex; gap: 16px; flex-wrap: wrap; margin-top: 8px; justify-content: center; }
.legend-item { display: flex; align-items: center; gap: 6px; font-size: 0.72rem; color: var(–muted); }
.legend-dot { width: 12px; height: 12px; border-radius: 3px; border: 1.5px solid; }

/* ── 카운트다운 오버레이 ── */
.overlay {
display: none; position: fixed; inset: 0;
background: rgba(0,0,0,0.88); backdrop-filter: blur(8px);
z-index: 200; flex-direction: column; align-items: center; justify-content: center; gap: 20px;
}
.overlay.show { display: flex; }
.countdown-label { font-size: 1rem; color: var(–muted); letter-spacing: 0.25em; text-transform: uppercase; }
.countdown-num {
font-family: ‘Space Mono’, monospace; font-size: 10rem; font-weight: 700; line-height: 1;
color: #fff; text-shadow: 0 0 40px var(–accent2), 0 0 100px rgba(245,158,11,0.4);
}
.countdown-num.anim { animation: cPulse 0.9s cubic-bezier(.175,.885,.32,1.275); }
@keyframes cPulse { 0%{transform:scale(1.6);opacity:0} 60%{transform:scale(0.92)} 100%{transform:scale(1);opacity:1} }
.countdown-bar-wrap { width: 280px; height: 5px; background: #1e293b; border-radius: 99px; overflow: hidden; }
.countdown-bar { height: 100%; background: linear-gradient(90deg, var(–accent2), var(–accent)); border-radius: 99px; transition: width 0.9s linear; }

@media (max-width: 700px) {
.main-layout { flex-direction: column; }
.left-panel { flex: none; width: 100%; }
}
</style>

</head>
<body>

<div class="overlay" id="overlay">
  <div class="countdown-label" id="cLabel">자리 배치 시작까지</div>
  <div class="countdown-num" id="countNum">3</div>
  <div class="countdown-bar-wrap">
    <div class="countdown-bar" id="countBar" style="width:100%"></div>
  </div>
</div>

<div class="header">
  <h1>🏫 자리 배치 프로그램</h1>
  <p>2학년 8반 · 담임 김강호 · 6열 5행 · 29명 (12번 배준서 제외)</p>
</div>

<div class="main-layout">
  <div class="left-panel">

```
<div class="card">
  <h3>📌 앞자리 희망 학생</h3>
  <div class="priority-input-wrap">
    <input type="number" id="priorityInput" min="1" max="30" placeholder="번호 입력" />
    <button class="btn btn-amber" onclick="addPriority()">추가</button>
  </div>
  <div class="priority-tags" id="priorityTags"></div>
</div>

<div class="card">
  <h3>🎲 배치 결과</h3>
  <div class="queue-list" id="queueList">
    <div style="color:var(--muted);font-size:0.78rem;text-align:center;padding:16px 0">배치 시작 전</div>
  </div>
</div>

<div class="card">
  <h3>📊 현황</h3>
  <div class="stats">
    <div class="stat-box">
      <div class="stat-val" id="statSeated">0</div>
      <div class="stat-label">착석 완료</div>
    </div>
    <div class="stat-box">
      <div class="stat-val" id="statRemain">29</div>
      <div class="stat-label">대기 중</div>
    </div>
  </div>
</div>

<div class="card actions">
  <h3>⚙️ 제어</h3>
  <button class="btn btn-ghost" id="startBtn" onclick="startCountdown()">🎲 랜덤 배치 시작</button>
  <button class="btn btn-download" id="downloadBtn" onclick="downloadPNG()" style="display:none">📥 자리배치표 다운로드</button>
  <button class="btn btn-danger btn-ghost" onclick="resetAll()">🔄 초기화</button>
</div>
```

  </div>

  <div class="right-panel">
    <div class="blackboard">📋 칠판 (앞)</div>
    <div class="classroom">
      <div class="col-labels" id="colLabels"></div>
      <div class="grid" id="classroomGrid"></div>
      <div class="legend">
        <div class="legend-item"><div class="legend-dot" style="background:#1e293b;border-color:#334155"></div> 빈 자리</div>
        <div class="legend-item"><div class="legend-dot" style="background:#064e3b;border-color:#6ee7b7"></div> 착석</div>
        <div class="legend-item"><div class="legend-dot" style="background:#78350f;border-color:#f59e0b"></div> 앞자리 희망</div>
      </div>
    </div>
  </div>
</div>

<script>
// ── 학생 데이터 (사진 기반) ──────────────────────────
const STUDENTS = {
  1:  '곽동윤', 2:  '구자헌', 3:  '김근모', 4:  '김나균',
  5:  '김도훈', 6:  '김범수', 7:  '김성빈', 8:  '김세준',
  9:  '남동효', 10: '문기민', 11: '박준우', 12: '배준서',
  13: '손예준', 14: '손효인', 15: '송단우', 16: '신정재',
  17: '심준우', 18: '안윤현', 19: '안진후', 20: '안찬휘',
  21: '윤동욱', 22: '이규홍', 23: '이동욱', 24: '이승혁',
  25: '임현준', 26: '장민준', 27: '최세준', 28: '최호진',
  29: '하승준', 30: '황창제'
};

const ROWS = 5, COLS = 6, TOTAL = ROWS * COLS;
const ALL_STUDENTS = Object.keys(STUDENTS).map(Number).filter(n => n !== 12); // 12번 제외

let seats = Array(TOTAL).fill(null); // null or 번호
let priorityList = [];
let queue = [];
let started = false;

// ── Web Audio ──────────────────────────────────────
const actx = new (window.AudioContext || window.webkitAudioContext)();

function beep(freq, dur, type='square', vol=0.35) {
  const o=actx.createOscillator(), g=actx.createGain();
  o.connect(g); g.connect(actx.destination);
  o.type=type;
  o.frequency.setValueAtTime(freq, actx.currentTime);
  o.frequency.exponentialRampToValueAtTime(freq*0.4, actx.currentTime+dur);
  g.gain.setValueAtTime(vol, actx.currentTime);
  g.gain.exponentialRampToValueAtTime(0.001, actx.currentTime+dur);
  o.start(); o.stop(actx.currentTime+dur);
}

function noise(dur, cutoff=110, vol=0.06) {
  const buf=actx.createBuffer(1,actx.sampleRate*dur,actx.sampleRate);
  const d=buf.getChannelData(0);
  for(let i=0;i<d.length;i++) d[i]=(Math.random()*2-1)*(1-i/d.length);
  const src=actx.createBufferSource(), f=actx.createBiquadFilter(), g=actx.createGain();
  f.type='lowpass'; f.frequency.value=cutoff; g.gain.value=vol;
  src.buffer=buf; src.connect(f); f.connect(g); g.connect(actx.destination); src.start();
}

function playCountTension(n) {
  const speed=(4-n)*0.8+0.6;
  const baseFreq=[100,130,170][3-n];
  const beats=n===3?4:n===2?6:9;
  for(let i=0;i<beats;i++) {
    setTimeout(()=>beep(baseFreq+i*8, 0.09, 'sawtooth', 0.25+i*0.02), i*(220/speed));
  }
  noise(0.6, 80+(3-n)*20, 0.07+(3-n)*0.02);
  setTimeout(()=>{ beep(baseFreq*0.6, 0.25, 'square', 0.45); noise(0.3,60,0.1); }, beats*(220/speed)+30);
}

function playFanfare() {
  [523,659,784,523,659,784,1047].forEach((freq,i)=>{
    setTimeout(()=>beep(freq,0.3,'triangle',0.28), i*100);
  });
}

// ── 카운트다운 ─────────────────────────────────────
function startCountdown() {
  if(started){alert('이미 배치되었습니다. 초기화 후 다시 시작하세요.');return;}
  actx.resume();
  document.getElementById('startBtn').disabled=true;

  const overlay=document.getElementById('overlay');
  const countNum=document.getElementById('countNum');
  const countBar=document.getElementById('countBar');
  const cLabel=document.getElementById('cLabel');

  overlay.classList.add('show');
  let count=3;
  setCountNum(countNum, count);
  countBar.style.transition='none'; countBar.style.width='100%';
  playCountTension(count);

  const iv=setInterval(()=>{
    count--;
    if(count<=0){
      clearInterval(iv);
      cLabel.textContent='';
      countNum.className='';
      countNum.style.cssText='font-size:3rem;color:var(--accent);text-shadow:0 0 40px var(--accent);font-family:Noto Sans KR,sans-serif;';
      countNum.textContent='🎲 배치 중!';
      countBar.style.transition='width 0.9s linear'; countBar.style.width='0%';
      setTimeout(()=>{
        overlay.classList.remove('show');
        countNum.style.cssText=''; cLabel.textContent='자리 배치 시작까지';
        doAssign();
      },700);
      return;
    }
    countBar.style.transition='width 0.9s linear';
    countBar.style.width=`${(count/3)*100}%`;
    setCountNum(countNum,count);
    playCountTension(count);
  },1000);
}

function setCountNum(el,n){
  el.style.cssText='';
  el.className=''; el.textContent=n;
  void el.offsetWidth;
  el.className='countdown-num anim';
}

// ── 배치 ──────────────────────────────────────────
function shuffle(arr){
  const a=[...arr];
  for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]];}
  return a;
}

function doAssign(){
  started=true;
  const sp=shuffle(priorityList);
  const others=shuffle(ALL_STUDENTS.filter(n=>!priorityList.includes(n)));
  queue=[...sp,...others];

  queue.forEach((num,i)=>{
    setTimeout(()=>{
      const idx=nextFree();
      if(idx!==-1) placeSeat(idx,num,priorityList.includes(num));
      updateStats();
      if(i===queue.length-1){
        setTimeout(()=>{
          playFanfare();
          renderQueue();
          document.getElementById('downloadBtn').style.display='block';
          document.getElementById('startBtn').disabled=false;
        },200);
      }
    },i*60);
  });
}

function nextFree(){for(let i=0;i<TOTAL;i++) if(seats[i]===null) return i; return -1;}

function placeSeat(idx,num,isPri){
  seats[idx]=num;
  const d=document.getElementById(`desk-${idx}`);
  d.querySelector('.desk-num').textContent=`${num}번`;
  d.querySelector('.desk-name').textContent=STUDENTS[num];
  d.classList.remove('taken','priority-seat');
  void d.offsetWidth;
  d.classList.add(isPri?'priority-seat':'taken');
}

// ── 앞자리 희망 ────────────────────────────────────
function addPriority(){
  const input=document.getElementById('priorityInput');
  const val=parseInt(input.value);
  if(!val||val<1||val>30){alert('1~30 사이 번호를 입력하세요.');return;}
  if(!STUDENTS[val]){alert(`${val}번 학생이 없습니다.`);return;}
  if(priorityList.includes(val)){alert(`${val}번 ${STUDENTS[val]}은(는) 이미 추가되어 있습니다.`);return;}
  priorityList.push(val);
  input.value='';
  renderPriorityTags();
}
document.getElementById('priorityInput').addEventListener('keydown',e=>{if(e.key==='Enter')addPriority();});

function removePriority(num){priorityList=priorityList.filter(n=>n!==num);renderPriorityTags();}

function renderPriorityTags(){
  const wrap=document.getElementById('priorityTags');
  wrap.innerHTML='';
  if(!priorityList.length){wrap.innerHTML='<span style="color:var(--muted);font-size:0.75rem">없음</span>';return;}
  priorityList.forEach(num=>{
    const tag=document.createElement('div'); tag.className='tag';
    tag.innerHTML=`${num}번 ${STUDENTS[num]} <span class="remove" onclick="removePriority(${num})">×</span>`;
    wrap.appendChild(tag);
  });
}

// ── 결과 목록 ──────────────────────────────────────
function renderQueue(){
  const list=document.getElementById('queueList');
  list.innerHTML='';
  if(!started){list.innerHTML='<div style="color:var(--muted);font-size:0.78rem;text-align:center;padding:16px 0">배치 시작 전</div>';return;}
  queue.forEach(num=>{
    const idx=seats.indexOf(num);
    const r=Math.floor(idx/COLS)+1, c=idx%COLS+1;
    const item=document.createElement('div'); item.className='queue-item';
    item.innerHTML=`
      <span class="queue-num">${num}번</span>
      <span class="queue-name">${STUDENTS[num]}</span>
      <span class="queue-pos">${r}행${c}열</span>
      ${priorityList.includes(num)?'<span class="queue-priority-badge">앞</span>':''}
    `;
    list.appendChild(item);
  });
}

function updateStats(){
  const s=seats.filter(x=>x!==null).length;
  document.getElementById('statSeated').textContent=s;
  document.getElementById('statRemain').textContent=ALL_STUDENTS.length-s;
}

// ── PNG 다운로드 ────────────────────────────────────
function downloadPNG(){
  const CELL=96, PAD=20, HEADER=76;
  const W=COLS*CELL+PAD*2, H=ROWS*CELL+PAD*2+HEADER+40;
  const canvas=document.createElement('canvas');
  canvas.width=W; canvas.height=H;
  const c=canvas.getContext('2d');

  c.fillStyle='#0f0f14'; c.fillRect(0,0,W,H);

  // 제목
  c.fillStyle='#6ee7b7'; c.font='bold 20px "Noto Sans KR",sans-serif'; c.textAlign='center';
  c.fillText('2학년 8반 자리 배치표', W/2, 30);

  // 칠판
  c.fillStyle='#1e3a5f';
  c.beginPath(); c.roundRect(PAD,42,W-PAD*2,26,6); c.fill();
  c.fillStyle='#bfdbfe'; c.font='bold 12px "Noto Sans KR",sans-serif';
  c.fillText('📋 칠판 (앞)', W/2, 60);

  for(let r=0;r<ROWS;r++){
    for(let col=0;col<COLS;col++){
      const idx=r*COLS+col;
      const x=PAD+col*CELL, y=HEADER+PAD+r*CELL;
      const num=seats[idx], isPri=num&&priorityList.includes(num);

      c.fillStyle=num?(isPri?'#78350f':'#064e3b'):'#1e293b';
      c.strokeStyle=num?(isPri?'#f59e0b':'#6ee7b7'):'#334155';
      c.lineWidth=2;
      c.beginPath(); c.roundRect(x+3,y+3,CELL-6,CELL-6,8); c.fill(); c.stroke();

      // 행열
      c.fillStyle='#475569'; c.font='8px sans-serif'; c.textAlign='left';
      c.fillText(`${r+1}행${col+1}열`, x+7, y+15);

      if(num){
        // 번호
        c.fillStyle=isPri?'rgba(245,158,11,0.7)':'rgba(110,231,183,0.7)';
        c.font='bold 13px monospace'; c.textAlign='center';
        c.fillText(`${num}번`, x+CELL/2, y+CELL/2-4);
        // 이름
        c.fillStyle=isPri?'#f59e0b':'#6ee7b7';
        c.font='bold 15px "Noto Sans KR",sans-serif'; c.textAlign='center';
        c.fillText(STUDENTS[num], x+CELL/2, y+CELL/2+14);
      }
    }
  }

  // 범례
  const ly=HEADER+PAD+ROWS*CELL+14;
  [['#064e3b','#6ee7b7','일반 착석'],['#78350f','#f59e0b','앞자리 희망']].forEach(([bg,bd,label],i)=>{
    const lx=PAD+i*160;
    c.fillStyle=bg; c.strokeStyle=bd; c.lineWidth=1.5;
    c.beginPath(); c.roundRect(lx,ly,13,13,3); c.fill(); c.stroke();
    c.fillStyle='#94a3b8'; c.font='10px sans-serif'; c.textAlign='left';
    c.fillText(label, lx+17, ly+10);
  });

  const a=document.createElement('a');
  a.download='2학년8반_자리배치표.png';
  a.href=canvas.toDataURL('image/png');
  a.click();
}

// ── 초기화 ────────────────────────────────────────
function resetAll(){
  if(!confirm('모든 배치를 초기화할까요?')) return;
  seats=Array(TOTAL).fill(null);
  priorityList=[]; queue=[]; started=false;
  document.querySelectorAll('.desk').forEach(d=>{
    d.classList.remove('taken','priority-seat');
    d.querySelector('.desk-num').textContent='';
    d.querySelector('.desk-name').textContent='';
  });
  document.getElementById('downloadBtn').style.display='none';
  document.getElementById('startBtn').disabled=false;
  renderPriorityTags(); renderQueue(); updateStats();
}

// ── 그리드 빌드 ────────────────────────────────────
function buildGrid(){
  const grid=document.getElementById('classroomGrid');
  const colLbls=document.getElementById('colLabels');
  grid.innerHTML=''; colLbls.innerHTML='';
  for(let c=0;c<COLS;c++){
    const l=document.createElement('div'); l.className='col-label'; l.textContent=`${c+1}열`; colLbls.appendChild(l);
  }
  for(let r=0;r<ROWS;r++){
    for(let col=0;col<COLS;col++){
      const idx=r*COLS+col;
      const d=document.createElement('div'); d.className='desk'; d.id=`desk-${idx}`;
      d.innerHTML=`
        <span class="seat-label">${r+1}행${col+1}열</span>
        <span class="desk-num"></span>
        <span class="desk-name"></span>
      `;
      grid.appendChild(d);
    }
  }
}

buildGrid(); renderPriorityTags(); renderQueue(); updateStats();
</script>

</body>
</html>
