
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>sofyan-MZNE</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;700&family=Syne:wght@800&display=swap" rel="stylesheet"/>
<style>
  :root { --bg:#060810; --accent:#00aaff; }
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
  body{
    background:var(--bg);font-family:'JetBrains Mono',monospace;
    min-height:100vh;overflow-x:hidden;
    display:flex;flex-direction:column;align-items:center;
    justify-content:center;gap:56px;padding:60px 24px;
  }
  #matrix-canvas{position:fixed;top:0;left:0;width:100%;height:100%;opacity:.045;pointer-events:none;z-index:0}
  body::before{
    content:'';position:fixed;inset:0;
    background-image:linear-gradient(rgba(0,170,255,.025) 1px,transparent 1px),linear-gradient(90deg,rgba(0,170,255,.025) 1px,transparent 1px);
    background-size:44px 44px;pointer-events:none;z-index:0;
  }

  /* Hero */
  .hero{position:relative;z-index:1;text-align:center;animation:fadeUp 1s ease both}
  .hero-tag{font-size:10px;letter-spacing:4px;color:var(--accent);text-transform:uppercase;opacity:.6;margin-bottom:12px}
  .hero h1{
    font-family:'Syne',sans-serif;font-size:clamp(2.6rem,7vw,5rem);
    font-weight:800;letter-spacing:-1px;line-height:1;
    background:linear-gradient(135deg,#fff 0%,#00aaff 55%,#0040cc 100%);
    -webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
  }
  .hero-role{margin-top:16px;font-size:12px;color:rgba(255,255,255,.28);letter-spacing:3px;text-transform:uppercase}
  .hero-role span{color:var(--accent);opacity:.85}
  .cursor{display:inline-block;width:3px;height:1em;background:var(--accent);margin-left:3px;vertical-align:middle;animation:blink 1s step-end infinite}

  /* Contrib */
  .contrib-wrap{position:relative;z-index:1;text-align:center;display:flex;flex-direction:column;align-items:center;animation:fadeUp 1s .35s ease both;opacity:0;animation-fill-mode:forwards}
  .contrib-label{font-size:9px;letter-spacing:4px;text-transform:uppercase;color:var(--accent);opacity:.45;margin-bottom:16px}
  #contrib-canvas{border-radius:10px;display:block;margin:0 auto;box-shadow:0 0 50px rgba(0,100,255,.1)}

  /* Activity list */
  .activity-list{margin-top:20px;display:flex;flex-direction:column;gap:8px;width:100%;max-width:680px}
  .activity-item{
    display:flex;align-items:center;gap:12px;
    background:rgba(0,30,70,.35);border:1px solid rgba(0,170,255,.1);
    border-radius:8px;padding:10px 16px;
    opacity:0;transform:translateX(-20px);
    transition:opacity .45s ease,transform .45s ease,border-color .3s,background .3s;
  }
  .activity-item.visible{opacity:1;transform:translateX(0)}
  .activity-item:hover{border-color:rgba(0,170,255,.4);background:rgba(0,50,110,.45)}
  .act-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;background:#55ccff;box-shadow:0 0 8px #55ccff}
  .act-label{font-size:10px;color:rgba(255,255,255,.3);letter-spacing:1px;width:100px;flex-shrink:0}
  .act-bar-wrap{flex:1;height:3px;background:rgba(0,170,255,.08);border-radius:99px;overflow:hidden}
  .act-bar{height:100%;border-radius:99px;background:linear-gradient(90deg,#004aaa,#55ccff);width:0;transition:width 1.1s cubic-bezier(.16,1,.3,1)}
  .act-text{font-size:10px;color:rgba(0,170,255,.55);letter-spacing:1px;white-space:nowrap;width:80px;text-align:right}

  /* Loading state */
  .loading-msg{font-size:10px;color:rgba(0,170,255,.4);letter-spacing:2px;margin-top:12px;animation:pulse 1.5s ease infinite}
  @keyframes pulse{0%,100%{opacity:.4}50%{opacity:1}}

  /* Contact */
  .contact-row{position:relative;z-index:1;display:flex;gap:14px;flex-wrap:wrap;justify-content:center;animation:fadeUp 1s .65s ease both;opacity:0;animation-fill-mode:forwards}
  .btn{
    display:inline-flex;align-items:center;gap:9px;padding:12px 24px;border-radius:8px;
    font-family:'JetBrains Mono',monospace;font-size:11px;letter-spacing:1.5px;
    text-decoration:none;text-transform:uppercase;
    border:1px solid rgba(0,170,255,.22);color:rgba(255,255,255,.55);
    background:rgba(0,20,55,.55);transition:all .25s;
  }
  .btn:hover{border-color:var(--accent);color:var(--accent);background:rgba(0,50,100,.5);box-shadow:0 0 22px rgba(0,170,255,.18);transform:translateY(-3px)}

  @keyframes fadeUp{from{opacity:0;transform:translateY(26px)}to{opacity:1;transform:translateY(0)}}
  @keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
</style>
</head>
<body>

<canvas id="matrix-canvas"></canvas>

<div class="hero">
  <div class="hero-tag">// profile</div>
  <h1>sofyan-MZNE<span class="cursor"></span></h1>
  <div class="hero-role">
    <span>Cybersecurity</span> &nbsp;&amp;&nbsp; <span>AI</span> Student
  </div>
</div>

<div class="contrib-wrap">
  <div class="contrib-label">Contributions</div>
  <canvas id="contrib-canvas"></canvas>
  <div class="activity-list" id="activity-list">
    <div class="loading-msg" id="loading-msg">⟳ Loading GitHub data...</div>
  </div>
</div>

<div class="contact-row">
  <a class="btn" href="mailto:sofyanmeziane1@gmail.com">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <rect x="2" y="4" width="20" height="16" rx="2"/><path d="m2 7 10 7 10-7"/>
    </svg>
    Email
  </a>
  <a class="btn" href="https://www.linkedin.com/in/sofyan-meziane-15a394373?utm_source=share_via&utm_content=profile&utm_medium=member_android" target="_blank" rel="noopener">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor">
      <path d="M20.45 20.45h-3.56v-5.57c0-1.33-.03-3.04-1.85-3.04-1.85 0-2.13 1.45-2.13 2.94v5.67H9.35V9h3.41v1.56h.05c.48-.9 1.64-1.85 3.37-1.85 3.6 0 4.27 2.37 4.27 5.45v6.29zM5.34 7.43a2.07 2.07 0 1 1 0-4.14 2.07 2.07 0 0 1 0 4.14zM7.12 20.45H3.56V9h3.56v11.45z"/>
    </svg>
    LinkedIn
  </a>
</div>

<script>
const GITHUB_USER = 'sofyan-MZNE';

/* ════════ MATRIX RAIN ════════ */
const mc=document.getElementById('matrix-canvas'),mct=mc.getContext('2d');
let cols,drops;
const CHARS='01アイウエオカキクケコサシスセソ{}[]<>/\\|=-+*';
function initMatrix(){mc.width=window.innerWidth;mc.height=window.innerHeight;cols=Math.floor(mc.width/14);drops=Array(cols).fill(0).map(()=>Math.random()*-60)}
function drawMatrix(){
  mct.fillStyle='rgba(6,8,16,0.055)';mct.fillRect(0,0,mc.width,mc.height);
  drops.forEach((y,i)=>{
    mct.fillStyle=Math.random()>.94?'#ffffff':'#00aaff';
    mct.font='13px JetBrains Mono,monospace';
    mct.fillText(CHARS[Math.floor(Math.random()*CHARS.length)],i*14,y*14);
    if(y*14>mc.height&&Math.random()>.975)drops[i]=0;
    drops[i]++;
  });
}
initMatrix();setInterval(drawMatrix,48);window.addEventListener('resize',initMatrix);

/* ════════ CONTRIBUTION GRID ════════ */
const cc=document.getElementById('contrib-canvas'),cct=cc.getContext('2d');
const WEEKS=52,DAYS=7,CELL=13,GAP=3,STEP=CELL+GAP,PAD_L=30,PAD_T=22;
const maxW=Math.min(window.innerWidth-48,WEEKS*STEP+PAD_L+12);
cc.width=maxW;cc.height=DAYS*STEP+PAD_T+18;

// Will be filled from API
let gridData=Array.from({length:WEEKS},()=>Array.from({length:DAYS},()=>({level:0,count:0})));
const COLORS=['#090d1a','#003580','#0060c0','#0099ee','#55ccff'];
const DAY_LABELS=['','Mon','','Wed','','Fri',''];

function drawLabels(){
  cct.fillStyle='rgba(0,170,255,0.28)';cct.font='9px JetBrains Mono,monospace';
  DAY_LABELS.forEach((l,d)=>l&&cct.fillText(l,2,PAD_T+d*STEP+CELL-1));
}
function drawCell(w,d,alpha){
  const{level}=gridData[w][d];
  const x=PAD_L+w*STEP,y=PAD_T+d*STEP;
  cct.globalAlpha=alpha;cct.fillStyle=COLORS[level];
  cct.shadowColor=level>=3?'#00aaff':'transparent';
  cct.shadowBlur=level===4?9:level===3?4:0;
  cct.beginPath();cct.roundRect(x,y,CELL,CELL,3);cct.fill();
  cct.shadowBlur=0;cct.globalAlpha=1;
}

let revealed=0;
function revealNext(){
  if(revealed>=WEEKS){startPulse();return;}
  cct.clearRect(0,0,cc.width,cc.height);drawLabels();
  for(let w=0;w<revealed;w++)for(let d=0;d<DAYS;d++)drawCell(w,d,1);
  for(let d=0;d<DAYS;d++)drawCell(revealed,d,0.5+Math.random()*0.5);
  revealed++;setTimeout(revealNext,26);
}
let pv=0,pd=1;
function startPulse(){
  (function frame(){
    pv+=0.012*pd;if(pv>=1||pv<=0)pd*=-1;
    cct.clearRect(0,0,cc.width,cc.height);drawLabels();
    for(let w=0;w<WEEKS;w++)for(let d=0;d<DAYS;d++)
      drawCell(w,d,gridData[w][d].level===4?0.72+pv*0.28:1);
    requestAnimationFrame(frame);
  })();
}

/* ════════ GITHUB API ════════ */
async function fetchGitHubData(){
  try {
    // 1. Fetch all repos
    const reposRes = await fetch(`https://api.github.com/users/${GITHUB_USER}/repos?per_page=100&sort=updated`);
    const repos = await reposRes.json();

    if(!Array.isArray(repos)) throw new Error('Bad repos response');

    // 2. Get commits from last 4 weeks across all repos
    const now = new Date();
    const weeks = [0,1,2,3].map(i => {
      const end   = new Date(now); end.setDate(now.getDate() - i*7);
      const start = new Date(end); start.setDate(end.getDate() - 6);
      return { label: i===0?'This week':i===1?'Last week':`${i} weeks ago`, start, end, count:0 };
    });

    // Fetch commits for top 5 most recently updated repos
    const topRepos = repos.slice(0, 5);
    await Promise.all(topRepos.map(async repo => {
      try {
        const since = new Date(now); since.setDate(now.getDate()-28);
        const url = `https://api.github.com/repos/${GITHUB_USER}/${repo.name}/commits?since=${since.toISOString()}&per_page=100`;
        const res = await fetch(url);
        if(!res.ok) return;
        const commits = await res.json();
        if(!Array.isArray(commits)) return;

        commits.forEach(commit => {
          const date = new Date(commit.commit?.author?.date);
          weeks.forEach(w => {
            if(date >= w.start && date <= w.end) w.count++;
          });

          // Fill grid data
          const daysAgo = Math.floor((now - date) / 86400000);
          const weekIdx = WEEKS - 1 - Math.floor(daysAgo / 7);
          const dayIdx  = date.getDay();
          if(weekIdx >= 0 && weekIdx < WEEKS && dayIdx < DAYS) {
            gridData[weekIdx][dayIdx].count++;
            const c = gridData[weekIdx][dayIdx].count;
            gridData[weekIdx][dayIdx].level = c>=8?4:c>=5?3:c>=3?2:c>=1?1:0;
          }
        });
      } catch(e){}
    }));

    // Render activity list
    renderActivityList(weeks);

  } catch(err) {
    // Fallback to demo data
    console.warn('GitHub API error, using demo data', err);
    useDemoData();
  }
}

function renderActivityList(weeks) {
  const container = document.getElementById('activity-list');
  container.innerHTML = '';
  const max = Math.max(...weeks.map(w=>w.count), 1);

  weeks.forEach((w, i) => {
    const pct = Math.round((w.count / max) * 100);
    const dotClass = pct>75?'l4':pct>50?'l3':pct>25?'l2':'l1';
    const item = document.createElement('div');
    item.className = 'activity-item';
    item.innerHTML = `
      <div class="act-dot ${dotClass}"></div>
      <span class="act-label">${w.label}</span>
      <div class="act-bar-wrap"><div class="act-bar"></div></div>
      <span class="act-text">${w.count} commit${w.count!==1?'s':''}</span>
    `;
    container.appendChild(item);
    setTimeout(() => {
      item.classList.add('visible');
      setTimeout(() => { item.querySelector('.act-bar').style.width = (pct||2)+'%'; }, 200);
    }, 200 + i*160);
  });
}

function useDemoData(){
  // Fill grid with random data
  gridData=Array.from({length:WEEKS},()=>Array.from({length:DAYS},()=>{
    const r=Math.random();
    return{level:r>.93?4:r>.83?3:r>.70?2:r>.52?1:0,count:0};
  }));
  const demoWeeks=[
    {label:'This week',count:12},{label:'Last week',count:9},
    {label:'2 weeks ago',count:6},{label:'3 weeks ago',count:3}
  ];
  renderActivityList(demoWeeks);
}

// Start everything
fetchGitHubData().then(() => {
  setTimeout(revealNext, 400);
});
</script>
</body>
</html>
