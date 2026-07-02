[clothing_vocab_practice.html](https://github.com/user-attachments/files/29584924/clothing_vocab_practice.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>옷장 어휘 연습</title>
<style>
  :root{
    --indigo:#2F3E5C;
    --indigo-dark:#20293D;
    --gold:#D9A441;
    --linen:#F1EEE6;
    --card:#FFFFFF;
    --ink:#25293A;
    --muted:#6B7080;
    --good:#4C8B5B;
    --bad:#C4574A;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:var(--linen);
    font-family:'Noto Sans KR','Pretendard',system-ui,sans-serif;
    color:var(--ink);
    padding:24px 16px 60px;
  }
  .wrap{max-width:960px;margin:0 auto;}

  header{
    background:var(--indigo);
    color:#fff;
    border-radius:18px;
    padding:28px 28px 22px;
    position:relative;
    overflow:hidden;
    margin-bottom:24px;
  }
  header::before{
    content:"";
    position:absolute; top:18px; left:18px;
    width:14px; height:14px; border-radius:50%;
    background:var(--linen);
    box-shadow: inset 0 0 0 3px var(--indigo-dark);
  }
  header h1{margin:0 0 6px 40px; font-size:1.6rem; letter-spacing:.02em;}
  header p{margin:0 0 0 40px; color:#D8DCE8; font-size:.92rem;}
  header .tag-line{margin-top:16px; height:0; border-top:2px dashed rgba(255,255,255,.35);}

  nav.tabs{display:flex; gap:8px; margin-bottom:20px; flex-wrap:wrap;}
  nav.tabs button{
    flex:1 1 auto; min-width:150px; padding:12px 10px;
    border:2px solid var(--indigo); background:var(--card); color:var(--indigo);
    border-radius:12px; font-weight:700; font-size:.86rem; cursor:pointer; font-family:inherit;
  }
  nav.tabs button.active{background:var(--indigo); color:#fff;}

  section.panel{display:none;}
  section.panel.active{display:block;}

  .card{background:var(--card); border-radius:16px; padding:22px; margin-bottom:18px; box-shadow:0 1px 3px rgba(0,0,0,.06);}
  .card h2{margin:0 0 4px; font-size:1.15rem; color:var(--indigo-dark);}
  .card .sub{color:var(--muted); font-size:.88rem; margin-bottom:16px;}

  .match-toolbar{display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:10px; margin-bottom:16px;}
  .dir-toggle{display:inline-flex; border:2px solid var(--indigo); border-radius:10px; overflow:hidden;}
  .dir-toggle button{border:none; background:var(--card); color:var(--indigo); padding:8px 14px; font-weight:700; font-size:.85rem; cursor:pointer; font-family:inherit;}
  .dir-toggle button.on{background:var(--gold); color:#3A2A05;}
  .score-pill{background:var(--linen); border:1px solid #e0dccf; border-radius:999px; padding:6px 14px; font-size:.85rem; font-weight:700; color:var(--indigo-dark);}
  .round-nav{display:flex; gap:6px; flex-wrap:wrap; margin-bottom:16px;}
  .round-nav button{width:34px; height:34px; border-radius:50%; border:2px solid var(--indigo); background:#fff; color:var(--indigo); font-weight:700; cursor:pointer; font-family:inherit;}
  .round-nav button.on{background:var(--indigo); color:#fff;}

  .match-grid{display:grid; grid-template-columns:1fr 1fr; gap:14px 20px;}
  .match-col{display:flex; flex-direction:column; gap:10px;}
  .match-item{
    border:2px solid #DAD5C6; background:#FCFBF8; border-radius:12px; padding:10px;
    cursor:pointer; text-align:center; font-family:inherit; color:var(--ink);
    transition:.12s; line-height:1.35; font-size:.95rem;
    display:flex; align-items:center; justify-content:center; min-height:60px;
  }
  .match-item.word-card{font-weight:700; font-size:1.02rem;}
  .match-item .icon{width:56px; height:56px;}
  .match-item:hover{border-color:var(--gold);}
  .match-item.selected{border-color:var(--indigo); background:#EEF1F7;}
  .match-item.correct{border-color:var(--good); background:#EAF4EC; color:var(--good); cursor:default;}
  .match-item.wrong{border-color:var(--bad); animation:shake .3s;}
  @keyframes shake{0%,100%{transform:translateX(0);} 25%{transform:translateX(-4px);} 75%{transform:translateX(4px);}}

  .word-bank{display:flex; flex-wrap:wrap; gap:8px; margin-bottom:18px; padding:14px; background:var(--linen); border-radius:12px;}
  .chip{border:2px solid var(--indigo); background:#fff; color:var(--indigo); padding:7px 13px; border-radius:999px; font-size:.88rem; font-weight:700; cursor:pointer; font-family:inherit;}
  .chip.used{opacity:.35; cursor:default;}
  .chip.active-pick{background:var(--gold); border-color:var(--gold); color:#3A2A05;}

  .fib-sentence{padding:12px 4px; border-bottom:1px dashed #DDD8C9; font-size:1rem; line-height:1.8;}
  .fib-sentence:last-child{border-bottom:none;}
  .blank{display:inline-block; min-width:70px; border-bottom:2px solid var(--indigo); text-align:center; font-weight:700; color:var(--indigo-dark); padding:0 4px; cursor:pointer;}
  .blank.filled{color:var(--ink); background:#EEF1F7; border-radius:6px;}
  .blank.correct{color:var(--good); border-bottom-color:var(--good); background:#EAF4EC; border-radius:6px;}
  .blank.wrong{color:var(--bad); border-bottom-color:var(--bad); background:#FBEAE7; border-radius:6px;}

  .actions{display:flex; gap:10px; margin-top:16px; flex-wrap:wrap;}
  .btn{border:none; border-radius:10px; padding:11px 20px; font-weight:700; font-size:.9rem; cursor:pointer; font-family:inherit;}
  .btn.primary{background:var(--indigo); color:#fff;}
  .btn.ghost{background:#fff; border:2px solid var(--indigo); color:var(--indigo);}
  .result-msg{margin-top:12px; font-weight:700; font-size:.92rem;}

  @media (max-width:560px){
    .match-grid{grid-template-columns:1fr;}
    header h1{font-size:1.3rem;}
  }
</style>
</head>
<body>
<div class="wrap">

  <header>
    <h1>옷장 어휘 연습</h1>
    <p>2급 초반 · 쉬운 옷 어휘와 표현</p>
    <div class="tag-line"></div>
  </header>

  <nav class="tabs">
    <button class="tab-btn active" data-tab="match-noun">① 옷 이름 매칭</button>
    <button class="tab-btn" data-tab="match-adj">② 표현 매칭</button>
    <button class="tab-btn" data-tab="fib-noun">③ 빈칸 · 옷 이름</button>
    <button class="tab-btn" data-tab="fib-adj">④ 빈칸 · 표현</button>
  </nav>

  <section class="panel active" id="panel-match-noun">
    <div class="card">
      <h2>단어와 그림을 연결하세요</h2>
      <div class="sub">카드를 하나씩 눌러서 짝을 맞춰 보세요.</div>
      <div class="match-toolbar">
        <div class="dir-toggle">
          <button id="nDirWordFirst" class="on">단어 → 그림</button>
          <button id="nDirPicFirst">그림 → 단어</button>
        </div>
        <div class="score-pill" id="nScore">맞은 개수 0 / 0</div>
      </div>
      <div class="round-nav" id="nRoundNav"></div>
      <div class="match-grid">
        <div class="match-col" id="nColLeft"></div>
        <div class="match-col" id="nColRight"></div>
      </div>
    </div>
  </section>

  <section class="panel" id="panel-match-adj">
    <div class="card">
      <h2>단어와 쉬운 뜻을 연결하세요</h2>
      <div class="sub">카드를 하나씩 눌러서 짝을 맞춰 보세요.</div>
      <div class="match-toolbar">
        <div class="dir-toggle">
          <button id="aDirWordFirst" class="on">단어 → 뜻</button>
          <button id="aDirMeaningFirst">뜻 → 단어</button>
        </div>
        <div class="score-pill" id="aScore">맞은 개수 0 / 0</div>
      </div>
      <div class="round-nav" id="aRoundNav"></div>
      <div class="match-grid">
        <div class="match-col" id="aColLeft"></div>
        <div class="match-col" id="aColRight"></div>
      </div>
    </div>
  </section>

  <section class="panel" id="panel-fib-noun">
    <div class="card">
      <h2>빈칸에 알맞은 옷 이름을 넣으세요</h2>
      <div class="sub">단어를 먼저 누르고, 넣을 빈칸을 누르세요.</div>
      <div class="word-bank" id="bankNoun"></div>
      <div id="sentencesNoun"></div>
      <div class="actions">
        <button class="btn primary" onclick="checkFib('noun')">정답 확인</button>
        <button class="btn ghost" onclick="resetFib('noun')">다시 하기</button>
      </div>
      <div class="result-msg" id="resultNoun"></div>
    </div>
  </section>

  <section class="panel" id="panel-fib-adj">
    <div class="card">
      <h2>빈칸에 알맞은 표현을 넣으세요</h2>
      <div class="sub">아요/어요 형태예요. 알맞은 것을 골라 넣으세요.</div>
      <div class="word-bank" id="bankAdj"></div>
      <div id="sentencesAdj"></div>
      <div class="actions">
        <button class="btn primary" onclick="checkFib('adj')">정답 확인</button>
        <button class="btn ghost" onclick="resetFib('adj')">다시 하기</button>
      </div>
      <div class="result-msg" id="resultAdj"></div>
    </div>
  </section>

</div>

<script>
const ICONS = {
  tshirt: `<polygon points="50,8 34,16 14,32 24,46 34,38 34,92 66,92 66,38 76,46 86,32 66,16" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <path d="M40,8 Q50,20 60,8" fill="none" stroke="#2F3E5C" stroke-width="4" stroke-linecap="round"/>`,
  blouse: `<polygon points="50,8 34,16 14,32 24,46 34,38 34,92 66,92 66,38 76,46 86,32 66,16" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <path d="M40,10 L50,26 L60,10" fill="none" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round" stroke-linecap="round"/>
    <line x1="50" y1="26" x2="50" y2="85" stroke="#2F3E5C" stroke-width="3"/>
    <circle cx="50" cy="42" r="2.5" fill="#2F3E5C"/><circle cx="50" cy="58" r="2.5" fill="#2F3E5C"/><circle cx="50" cy="74" r="2.5" fill="#2F3E5C"/>`,
  sweater: `<polygon points="50,8 30,18 10,34 22,50 32,40 32,92 68,92 68,40 78,50 90,34 70,18" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="38" y1="14" x2="62" y2="14" stroke="#2F3E5C" stroke-width="3"/>
    <line x1="34" y1="88" x2="66" y2="88" stroke="#2F3E5C" stroke-width="3"/>`,
  socks: `<path d="M35,12 H65 V50 H80 Q88,50 88,60 V72 Q88,88 70,88 H45 Q35,88 35,75 Z" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="35" y1="26" x2="65" y2="26" stroke="#2F3E5C" stroke-width="3"/>`,
  necktie: `<polygon points="42,10 58,10 62,24 50,20 38,24" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <polygon points="42,24 58,24 66,60 50,92 34,60" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="38" y1="45" x2="62" y2="45" stroke="#2F3E5C" stroke-width="3"/>`,
  gloves: `<rect x="28" y="42" width="44" height="42" rx="10" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>
    <rect x="30" y="14" width="9" height="30" rx="4" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>
    <rect x="42" y="10" width="9" height="34" rx="4" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>
    <rect x="54" y="10" width="9" height="34" rx="4" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>
    <rect x="66" y="14" width="9" height="30" rx="4" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>
    <path d="M28,55 Q14,50 16,66 Q18,78 30,74" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="28" y1="78" x2="72" y2="78" stroke="#2F3E5C" stroke-width="3"/>`,
  hat: `<path d="M15,58 Q15,14 50,14 Q85,14 85,58 Z" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <ellipse cx="50" cy="60" rx="38" ry="9" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4"/>`,
  skirtshort: `<path d="M32,14 H68 L88,86 H12 Z" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <rect x="32" y="14" width="36" height="8" fill="#2F3E5C" opacity="0.15" stroke="#2F3E5C" stroke-width="3"/>`,
  jeans: `<path d="M25,10 H75 V50 L65,90 H53 L50,50 L47,90 H35 L25,50 Z" fill="#B9CDE5" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="32" y1="10" x2="32" y2="16" stroke="#2F3E5C" stroke-width="3"/>
    <line x1="50" y1="10" x2="50" y2="16" stroke="#2F3E5C" stroke-width="3"/>
    <line x1="68" y1="10" x2="68" y2="16" stroke="#2F3E5C" stroke-width="3"/>
    <line x1="40" y1="50" x2="37" y2="88" stroke="#2F3E5C" stroke-width="2"/>
    <line x1="60" y1="50" x2="63" y2="88" stroke="#2F3E5C" stroke-width="2"/>`,
  pants: `<path d="M25,10 H75 V50 L65,90 H53 L50,50 L47,90 H35 L25,50 Z" fill="#DAD5C6" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="38" y1="20" x2="36" y2="85" stroke="#2F3E5C" stroke-width="2" stroke-dasharray="2,3"/>
    <line x1="62" y1="20" x2="64" y2="85" stroke="#2F3E5C" stroke-width="2" stroke-dasharray="2,3"/>`,
  skirtlong: `<path d="M30,14 H70 L92,94 H8 Z" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <rect x="30" y="14" width="40" height="8" fill="#2F3E5C" opacity="0.15" stroke="#2F3E5C" stroke-width="3"/>
    <line x1="40" y1="22" x2="30" y2="90" stroke="#2F3E5C" stroke-width="2"/>
    <line x1="50" y1="22" x2="50" y2="92" stroke="#2F3E5C" stroke-width="2"/>
    <line x1="60" y1="22" x2="70" y2="90" stroke="#2F3E5C" stroke-width="2"/>`,
  suit: `<polygon points="50,8 30,18 14,34 24,48 34,40 34,92 66,92 66,40 76,48 86,34 70,18" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <path d="M42,12 L50,40 L58,12" fill="none" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <circle cx="50" cy="55" r="2.5" fill="#2F3E5C"/><circle cx="50" cy="68" r="2.5" fill="#2F3E5C"/>`,
  dress: `<path d="M40,8 L60,8 L66,26 L56,32 L80,92 H20 L44,32 L34,26 Z" fill="#F0DDAF" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="38" y1="45" x2="62" y2="45" stroke="#2F3E5C" stroke-width="2"/>`,
  coat: `<polygon points="50,6 28,16 12,32 22,46 32,38 32,94 68,94 68,38 78,46 88,32 72,16" fill="#F7F4EE" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <path d="M40,10 L50,36 L60,10" fill="none" stroke="#2F3E5C" stroke-width="4" stroke-linejoin="round"/>
    <line x1="32" y1="62" x2="68" y2="62" stroke="#2F3E5C" stroke-width="4"/>
    <rect x="46" y="58" width="8" height="8" fill="none" stroke="#2F3E5C" stroke-width="2"/>`,
};

function iconSvg(key){ return `<svg class="icon" viewBox="0 0 100 100">${ICONS[key]}</svg>`; }

const NOUNS = [
  {w:"블라우스", icon:"blouse"},
  {w:"스웨터", icon:"sweater"},
  {w:"티셔츠", icon:"tshirt"},
  {w:"양말", icon:"socks"},
  {w:"넥타이", icon:"necktie"},
  {w:"장갑", icon:"gloves"},
  {w:"모자", icon:"hat"},
  {w:"스커트", icon:"skirtshort"},
  {w:"청바지", icon:"jeans"},
  {w:"바지", icon:"pants"},
  {w:"치마", icon:"skirtlong"},
  {w:"양복", icon:"suit"},
  {w:"원피스", icon:"dress"},
  {w:"코트", icon:"coat"},
];

const ADJ = [
  {w:"비싸다", m:"'싸다'의 반대예요"},
  {w:"싸다", m:"'비싸다'의 반대예요"},
  {w:"길다", m:"'짧다'의 반대예요"},
  {w:"짧다", m:"'길다'의 반대예요"},
  {w:"밝다", m:"'어둡다'의 반대예요"},
  {w:"어둡다", m:"'밝다'의 반대예요"},
  {w:"잘 맞다", m:"사이즈가 딱 좋아요"},
  {w:"크다", m:"'작다'의 반대예요"},
  {w:"작다", m:"'크다'의 반대예요"},
  {w:"어울리다", m:"저한테 예뻐요"},
  {w:"마음에 들다", m:"이 옷이 좋아요"},
];

function shuffle(arr){
  const a = [...arr];
  for(let i=a.length-1;i>0;i--){
    const j = Math.floor(Math.random()*(i+1));
    [a[i],a[j]]=[a[j],a[i]];
  }
  return a;
}

function makeMatcher(opts){
  const state = {mode1First:true, round:0, selected:null};

  function totalRounds(){ return Math.ceil(opts.data.length / opts.roundSize); }

  function buildRoundNav(){
    opts.els.roundNav.innerHTML = "";
    for(let i=0;i<totalRounds();i++){
      const b = document.createElement("button");
      b.textContent = i+1;
      if(i===state.round) b.classList.add("on");
      b.onclick = () => { state.round = i; render(); };
      opts.els.roundNav.appendChild(b);
    }
  }

  function makeBtn(entry, side, htmlContent, plainText){
    const btn = document.createElement("button");
    btn.className = "match-item" + (plainText ? " word-card" : "");
    btn.innerHTML = htmlContent;
    btn.dataset.key = entry.w;
    btn.dataset.side = side;
    btn.onclick = () => onClick(btn);
    return btn;
  }

  function onClick(btn){
    if(btn.classList.contains("correct")) return;
    if(!state.selected){
      document.querySelectorAll(".match-item.selected").forEach(el=>el.classList.remove("selected"));
      state.selected = btn; btn.classList.add("selected"); return;
    }
    if(state.selected === btn){ btn.classList.remove("selected"); state.selected = null; return; }
    if(state.selected.dataset.side === btn.dataset.side){
      state.selected.classList.remove("selected");
      state.selected = btn; btn.classList.add("selected"); return;
    }
    if(state.selected.dataset.key === btn.dataset.key){
      state.selected.classList.remove("selected");
      state.selected.classList.add("correct");
      btn.classList.add("correct");
      updateScore();
    } else {
      [state.selected, btn].forEach(el=>{ el.classList.add("wrong"); setTimeout(()=>el.classList.remove("wrong"),300); });
    }
    state.selected.classList.remove("selected");
    state.selected = null;
  }

  function updateScore(){
    const start = state.round*opts.roundSize;
    const roundTotal = opts.data.slice(start, start+opts.roundSize).length;
    const done = opts.els.colLeft.querySelectorAll(".match-item.correct").length;
    opts.els.score.textContent = `맞은 개수 ${done} / ${roundTotal}`;
  }

  function render(){
    buildRoundNav();
    state.selected = null;
    const start = state.round*opts.roundSize;
    const items = opts.data.slice(start, start+opts.roundSize);

    const leftItems = shuffle(items);
    const rightItems = shuffle(items);

    opts.els.colLeft.innerHTML = "";
    opts.els.colRight.innerHTML = "";

    leftItems.forEach(it => {
      const r = state.mode1First ? opts.renderMode1(it) : opts.renderMode2(it);
      opts.els.colLeft.appendChild(makeBtn(it, "left", r.html, r.plain));
    });
    rightItems.forEach(it => {
      const r = state.mode1First ? opts.renderMode2(it) : opts.renderMode1(it);
      opts.els.colRight.appendChild(makeBtn(it, "right", r.html, r.plain));
    });

    updateScore();
  }

  opts.els.dirBtn1.onclick = () => {
    state.mode1First = true;
    opts.els.dirBtn1.classList.add("on"); opts.els.dirBtn2.classList.remove("on");
    render();
  };
  opts.els.dirBtn2.onclick = () => {
    state.mode1First = false;
    opts.els.dirBtn2.classList.add("on"); opts.els.dirBtn1.classList.remove("on");
    render();
  };

  render();
  return {render};
}

makeMatcher({
  data: NOUNS,
  roundSize: 5,
  els:{
    colLeft: document.getElementById("nColLeft"),
    colRight: document.getElementById("nColRight"),
    roundNav: document.getElementById("nRoundNav"),
    score: document.getElementById("nScore"),
    dirBtn1: document.getElementById("nDirWordFirst"),
    dirBtn2: document.getElementById("nDirPicFirst"),
  },
  renderMode1: (it) => ({html: it.w, plain:true}),
  renderMode2: (it) => ({html: iconSvg(it.icon), plain:false}),
});

makeMatcher({
  data: ADJ,
  roundSize: 4,
  els:{
    colLeft: document.getElementById("aColLeft"),
    colRight: document.getElementById("aColRight"),
    roundNav: document.getElementById("aRoundNav"),
    score: document.getElementById("aScore"),
    dirBtn1: document.getElementById("aDirWordFirst"),
    dirBtn2: document.getElementById("aDirMeaningFirst"),
  },
  renderMode1: (it) => ({html: it.w, plain:true}),
  renderMode2: (it) => ({html: it.m, plain:true}),
});

const FIB_NOUN = {
  bank: ["블라우스","스웨터","티셔츠","양말","넥타이","장갑","모자","스커트","청바지","바지","치마","양복","원피스","코트"],
  sentences: [
    {parts:["이 ", null, "는 색깔이 밝아요."], ans:"블라우스"},
    {parts:["이 ", null, "가 저한테 잘 맞아요."], ans:"스웨터"},
    {parts:["이 ", null, "는 정말 싸요."], ans:"티셔츠"},
    {parts:["이 ", null, "은 좀 짧아요."], ans:"양말"},
    {parts:["이 ", null, "가 정장에 잘 어울려요."], ans:"넥타이"},
    {parts:["이 ", null, "은 저한테 좀 커요."], ans:"장갑"},
    {parts:["이 ", null, "는 저한테 좀 작아요."], ans:"모자"},
    {parts:["이 ", null, "는 너무 길어요."], ans:"스커트"},
    {parts:["이 ", null, "가 마음에 들어요."], ans:"청바지"},
    {parts:["이 ", null, "는 색깔이 어두워요."], ans:"바지"},
    {parts:["이 ", null, "는 좀 비싸요."], ans:"치마"},
    {parts:["이 ", null, "이 아버지한테 잘 맞아요."], ans:"양복"},
  ]
};

const FIB_ADJ = {
  bank: ["비싸요","싸요","길어요","짧아요","밝아요","어두워요","잘 맞아요","커요","작아요","어울려요","마음에 들어요"],
  sentences: [
    {parts:["백화점 옷은 좀 ", null, "."], ans:"비싸요"},
    {parts:["시장 옷은 백화점보다 ", null, "."], ans:"싸요"},
    {parts:["이 원피스는 다리까지 ", null, "."], ans:"길어요"},
    {parts:["이 치마는 너무 ", null, "."], ans:"짧아요"},
    {parts:["이 블라우스는 색이 ", null, "."], ans:"밝아요"},
    {parts:["겨울에는 옷 색이 ", null, "."], ans:"어두워요"},
    {parts:["이 신발이 저한테 ", null, "."], ans:"잘 맞아요"},
    {parts:["이 모자는 저한테 좀 ", null, "."], ans:"커요"},
    {parts:["이 티셔츠는 저한테 좀 ", null, "."], ans:"작아요"},
    {parts:["이 색깔이 저한테 잘 ", null, "."], ans:"어울려요"},
    {parts:["이 가방이 정말 ", null, "."], ans:"마음에 들어요"},
  ]
};

let fibState = { noun: {picked:null, filled:{}}, adj: {picked:null, filled:{}} };

function renderFib(type){
  const data = type === "noun" ? FIB_NOUN : FIB_ADJ;
  const bankEl = document.getElementById(type === "noun" ? "bankNoun" : "bankAdj");
  const sentEl = document.getElementById(type === "noun" ? "sentencesNoun" : "sentencesAdj");

  bankEl.innerHTML = "";
  shuffle(data.bank).forEach(word => {
    const chip = document.createElement("button");
    chip.className = "chip";
    chip.textContent = word;
    chip.dataset.word = word;
    chip.onclick = () => pickChip(type, word, chip);
    bankEl.appendChild(chip);
  });

  sentEl.innerHTML = "";
  data.sentences.forEach((s, idx) => {
    const row = document.createElement("div");
    row.className = "fib-sentence";
    s.parts.forEach(p => {
      if(p === null){
        const blank = document.createElement("span");
        blank.className = "blank";
        blank.dataset.idx = idx;
        blank.textContent = "＿＿＿";
        blank.onclick = () => fillBlank(type, idx, blank);
        row.appendChild(blank);
      } else {
        row.appendChild(document.createTextNode(p));
      }
    });
    sentEl.appendChild(row);
  });

  document.getElementById(type === "noun" ? "resultNoun" : "resultAdj").textContent = "";
}

function pickChip(type, word, chipEl){
  if(chipEl.classList.contains("used")) return;
  document.querySelectorAll(`#bank${type==="noun"?"Noun":"Adj"} .chip`).forEach(c=>c.classList.remove("active-pick"));
  fibState[type].picked = {word, chipEl};
  chipEl.classList.add("active-pick");
}

function fillBlank(type, idx, blankEl){
  const state = fibState[type];
  if(!state.picked) return;
  const prevWord = state.filled[idx];
  if(prevWord){
    document.querySelectorAll(`#bank${type==="noun"?"Noun":"Adj"} .chip`).forEach(c=>{
      if(c.dataset.word === prevWord) c.classList.remove("used");
    });
  }
  state.filled[idx] = state.picked.word;
  blankEl.textContent = state.picked.word;
  blankEl.classList.add("filled");
  blankEl.classList.remove("correct","wrong");
  state.picked.chipEl.classList.add("used");
  state.picked.chipEl.classList.remove("active-pick");
  state.picked = null;
}

function checkFib(type){
  const data = type === "noun" ? FIB_NOUN : FIB_ADJ;
  const state = fibState[type];
  let correct = 0;
  data.sentences.forEach((s, idx) => {
    const blankEl = document.querySelector(`#sentences${type==="noun"?"Noun":"Adj"} .blank[data-idx="${idx}"]`);
    blankEl.classList.remove("correct","wrong");
    if(state.filled[idx] === s.ans){ blankEl.classList.add("correct"); correct++; }
    else if(state.filled[idx]){ blankEl.classList.add("wrong"); }
  });
  document.getElementById(type === "noun" ? "resultNoun" : "resultAdj").textContent =
    `${correct} / ${data.sentences.length} 개 맞았어요.`;
}

function resetFib(type){ fibState[type] = {picked:null, filled:{}}; renderFib(type); }

document.querySelectorAll(".tab-btn").forEach(btn => {
  btn.onclick = () => {
    document.querySelectorAll(".tab-btn").forEach(b=>b.classList.remove("active"));
    document.querySelectorAll(".panel").forEach(p=>p.classList.remove("active"));
    btn.classList.add("active");
    document.getElementById("panel-" + btn.dataset.tab).classList.add("active");
  };
});

renderFib("noun");
renderFib("adj");
</script>
</body>
</html>
