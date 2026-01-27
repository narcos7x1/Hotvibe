<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Exclusive Content Store</title>
<meta name="description" content="Premium exclusive packs. Instant access. Clean layout, built to convert.">
<style>
:root {
  --bg:#040102; --card:#140405; --accent:#ff2a1a; --accent2:#ffb703;
  --text:#fff3ef; --muted:#f1a999; --glass:rgba(255,255,255,0.03);
  --radius:16px; --shadow:0 10px 30px rgba(0,0,0,.6);
}
*{box-sizing:border-box; margin:0; padding:0;}
body{
  font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
  background:radial-gradient(900px 500px at 80% -20%,#5a140c,transparent),
             radial-gradient(800px 400px at -10% 10%,#3b0c08,transparent),
             linear-gradient(180deg,#050101,var(--bg));
  color:var(--text); -webkit-font-smoothing:antialiased; overflow-x:hidden;
}
header{
  position:sticky; top:0; background:linear-gradient(180deg,rgba(0,0,0,.25),transparent);
  backdrop-filter:blur(6px); z-index:40; border-bottom:1px solid rgba(255,255,255,0.03);
}
.header-wrap{
  max-width:1400px; margin:0 auto; padding:18px; display:flex;
  align-items:center; justify-content:space-between; gap:16px;
}
.brand{font-weight:900; letter-spacing:2px; font-size:20px;}
.controls{display:flex; gap:12px; align-items:center;}
.search-wrap{position:relative; display:flex; gap:6px;}
.search-wrap input{
  padding:12px 14px; border-radius:12px; border:1px solid rgba(255,255,255,0.04);
  background:var(--glass); color:var(--text); min-width:180px; transition:0.2s;
}
.search-wrap select{
  padding:12px 14px; border-radius:12px; border:1px solid rgba(255,255,255,0.04);
  background:var(--glass); color:var(--text); cursor:pointer; transition:0.2s;
}
.search-wrap input:focus, .search-wrap select:focus{outline:none; border-color:var(--accent);}
.search-wrap .icon{
  position:absolute; right:12px; top:50%; transform:translateY(-50%); opacity:.9;
}
.container{max-width:1400px; margin:26px auto; padding:0 18px;}
.hero{display:flex; align-items:center; gap:28px; margin-bottom:24px;}
.hero .left{flex:1;}
.hero h1{margin:0; font-size:36px; line-height:1.02;}
.hero p{color:var(--muted); margin-top:8px;}
.grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(300px,1fr)); gap:24px; position:relative;}
.card{
  position:relative; border-radius:var(--radius); background:linear-gradient(180deg,#1b0706,#100303);
  overflow:hidden; border:1px solid rgba(255,255,255,0.03); box-shadow:var(--shadow);
  transition:transform .28s cubic-bezier(.2,.9,.3,1), box-shadow .28s;
}
.card:hover{transform:translateY(-12px) scale(1.02); box-shadow:0 35px 90px rgba(0,0,0,.85);}
.media{position:relative; height:0; padding-bottom:66%; overflow:hidden;}
.media img,.media video{position:absolute; inset:0; width:100%; height:100%; object-fit:cover; display:block; cursor:pointer; transition:transform .4s, filter .4s;}
.media img:hover,.media video:hover{transform:scale(1.05); filter:brightness(1.1);}
.media::after{
  content:''; position:absolute; left:0; right:0; bottom:0; height:45%; background:linear-gradient(180deg,transparent,rgba(0,0,0,.7));
}
.info{padding:16px; opacity:0; transform:translateY(20px); transition:opacity .4s, transform .4s;}
.card.loaded .info{opacity:1; transform:translateY(0);}
.title{font-weight:900; margin:0 0 8px; font-size:16px; white-space:nowrap; overflow:hidden; text-overflow:ellipsis;}
.meta{display:flex; align-items:center; justify-content:space-between; gap:12px;}
.price{font-weight:900; color:var(--accent2);}
.actions{display:flex; gap:10px; margin-top:12px;}
.btn{flex:1; border-radius:12px; padding:12px 14px; font-weight:900; border:none;
     background:linear-gradient(135deg,var(--accent),#ff6a2a); color:#140807; cursor:pointer; transition:0.2s;}
.btn:hover{opacity:.9; transform:scale(1.02);}
.btn.secondary{background:transparent; color:var(--text); border:1px solid rgba(255,255,255,0.03);}
.modal{position:fixed; inset:0; background:rgba(0,0,0,.7); display:flex; align-items:center; justify-content:center; z-index:80; opacity:0; pointer-events:none; transition:opacity .2s;}
.modal.open{opacity:1; pointer-events:auto;}
.modal .box{max-width:1100px; width:96%; max-height:90%; background:#000; border-radius:12px; overflow:hidden;}
.modal video,.modal img{width:100%; height:auto; display:block;}
.skeleton{
  border-radius:var(--radius); background:linear-gradient(90deg,#1a0b0a,#140405,#1a0b0a);
  height:220px; animation:skeleton 1.2s infinite linear;
}
@keyframes skeleton{0%{background-position:-200px 0;}100%{background-position:200px 0;}}
@media(max-width:720px){
  .hero{flex-direction:column; align-items:flex-start;}
  .brand{font-size:18px;}
}
</style>
</head>
<body>

<header>
  <div class="header-wrap">
    <div class="brand">EXCLUSIVE STORE</div>
    <div class="controls">
      <div class="search-wrap">
        <input id="q" placeholder="Search packs..." oninput="filterPacks()" aria-label="Search packs"/>
        <select id="typeFilter" onchange="filterPacks()">
          <option value="all">All</option>
          <option value="image">Images</option>
          <option value="video">Videos</option>
        </select>
      </div>
    </div>
  </div>
</header>

<main class="container">
  <section class="hero">
    <div class="left">
      <h1>Premium Exclusive Packs</h1>
      <p>Handpicked collections. Instant access vibe. Beautiful storefront designed to convert.</p>
    </div>
  </section>
  <section id="grid" class="grid"></section>
</main>

<div id="modal" class="modal" onclick="closeModal(event)">
  <div class="box" id="modalBox"></div>
</div>

<script>
const packs = [
  // FIXED PACKS
  {title:'Sexxyy-Daisy', src:'https://files.catbox.moe/up90wd.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Luminatria', src:'https://files.catbox.moe/lbvlcv.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'FaeXXXFae', src:'https://files.catbox.moe/1ktkca.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Karenka9', src:'https://files.catbox.moe/n7aerm.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  // EXTRA PACKS
  {title:'Pack 1', src:'https://files.catbox.moe/edb5c0.png', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 2', src:'https://files.catbox.moe/pwltnh.png', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 3', src:'https://files.catbox.moe/53v8s2.mp4', type:'video', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 4', src:'https://files.catbox.moe/k4ski4.mp4', type:'video', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 5', src:'https://files.catbox.moe/rakh1g.png', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 6', src:'https://files.catbox.moe/1o0drn.png', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Pack 7', src:'https://files.catbox.moe/eyfxt2.png', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'RedRoseLaCubana', src:'https://files.catbox.moe/7ih8du.jpeg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'GoddessOnline', src:'https://files.catbox.moe/t48ax9.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'BrattyDeath', src:'https://files.catbox.moe/jt3xrt.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'SkyBellaa', src:'https://files.catbox.moe/ncfypb.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'FlexiWithSophie', src:'https://files.catbox.moe/61tl6z.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'HelloTrinity', src:'https://files.catbox.moe/2igvjo.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'Alice_Rojas09', src:'https://files.catbox.moe/3ofudv.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'PremiumBabyy', src:'https://files.catbox.moe/cxbu4l.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'ManuzinhaBlack', src:'https://files.catbox.moe/o1nfog.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'ExoticPetiteShi', src:'https://files.catbox.moe/0yzj3h.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'MiliLuv', src:'https://files.catbox.moe/draar2.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'5x1.5 Pack', src:'https://files.catbox.moe/qklo0p.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'MangoBeetle', src:'https://files.catbox.moe/grm2ea.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'AdoreXKeya', src:'https://files.catbox.moe/a0rglv.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'ZoeyUso', src:'https://files.catbox.moe/cfbyll.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'SolesOfYanna', src:'https://files.catbox.moe/vosb3t.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'MsLyLy00', src:'https://files.catbox.moe/bo663u.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'DigitalSluut', src:'https://files.catbox.moe/vpae0q.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'5x1.4 Pack', src:'https://files.catbox.moe/t3jich.jpg', type:'image', link:'https://t.me/Hotvibespacks'},
  {title:'HazelGrandyy', src:'https://files.catbox.moe/sielgk.jpg', type:'image', link:'https://t.me/Hotvibespacks'}
];

// SCROLL INFINITO
let displayIndex = 0;
const batch = 6;

function renderBatch(filteredPacks){
  const grid = document.getElementById('grid');
  const batchPacks = filteredPacks.slice(displayIndex, displayIndex+batch);
  batchPacks.forEach(p=>{
    const card = document.createElement('div'); card.className='card skeleton';
    const media = document.createElement('div'); media.className='media';
    if(p.type==='video'){
      const v=document.createElement('video'); v.src=p.src; v.controls=true; v.preload='metadata'; v.playsInline=true;
      v.onclick=()=>openModal('video',p.src); media.appendChild(v);
    } else {
      const img=document.createElement('img'); img.src=p.src; img.alt=p.title;
      img.loading='lazy'; img.decoding='async'; img.onclick=()=>openModal('image',p.src);
      media.appendChild(img);
    }
    const info=document.createElement('div'); info.className='info';
    info.innerHTML=`<h3 class="title">${p.title}</h3>
                     <div class="meta"><div class="price">$9.99</div></div>
                     <div class="actions">
                       <button class="btn" onclick="location.href='${p.link}'">PAY</button>
                       <a class="btn secondary" href="${p.link}" target="_blank" rel="noopener">TELEGRAM</a>
                     </div>`;
    card.append(media, info);
    grid.appendChild(card);
    setTimeout(()=>card.classList.add('loaded'), 100);
  });
  displayIndex+=batch;
}

function filterPacks(){
  const q=document.getElementById('q').value.toLowerCase();
  const type=document.getElementById('typeFilter').value;
  displayIndex=0; document.getElementById('grid').innerHTML='';
  const filtered = packs.filter(p=>{
    const matchesType = (type==='all' || p.type===type);
    return matchesType && p.title.toLowerCase().includes(q);
  });
  renderBatch(filtered);
}

window.addEventListener('scroll', ()=>{
  const type=document.getElementById('typeFilter').value;
  const q=document.getElementById('q').value.toLowerCase();
  const filtered = packs.filter(p=>{
    const matchesType = (type==='all' || p.type===type);
    return matchesType && p.title.toLowerCase().includes(q);
  });
  if(window.innerHeight + window.scrollY >= document.body.offsetHeight-300){
    if(displayIndex<filtered.length) renderBatch(filtered);
  }
});

function openModal(type,src){
  const modal=document.getElementById('modal'); const box=document.getElementById('modalBox'); box.innerHTML='';
  if(type==='video'){ const v=document.createElement('video'); v.src=src; v.controls=true; v.autoplay=true; v.style.width='100%'; box.appendChild(v);}
  else{ const i=document.createElement('img'); i.src=src; box.appendChild(i);}
  modal.classList.add('open');
}
function closeModal(e){ if(e.target.id==='modal'){document.getElementById('modal').classList.remove('open'); document.getElementById('modalBox').innerHTML='';}}

filterPacks();
</script>

</body>
</html>
