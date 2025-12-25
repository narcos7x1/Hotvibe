<!DOCTYPE html>
<html lang="en">

<head>  
<meta charset="utf-8" />  
<meta name="viewport" content="width=device-width,initial-scale=1" />  
<title>Exclusive Content Store</title>  
<meta name="description" content="Premium exclusive packages. For you, our valued customer! Happy Shopping" />  
<style>  
:root{  
  --bg:#040102;--card:#140405;--accent:#ff2a1a;--accent2:#ffb703;--text:#fff3ef;--muted:#f1a999;--hot:#ff0033;--new:#ffd166;--glass:rgba(255,255,255,0.03);  
}  
*{box-sizing:border-box}  
html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial}  
body{background:radial-gradient(900px 500px at 80% -20%,#5a140c,transparent),radial-gradient(800px 400px at -10% 10%,#3b0c08,transparent),linear-gradient(180deg,#050101,var(--bg));color:var(--text);-webkit-font-smoothing:antialiased}  
header{position:sticky;top:0;background:linear-gradient(180deg,rgba(0,0,0,.25),transparent);backdrop-filter:blur(6px);z-index:40;border-bottom:1px solid rgba(255,255,255,0.03)}  
.header-wrap{max-width:1400px;margin:0 auto;padding:18px;display:flex;align-items:center;justify-content:space-between;gap:16px}  
.brand{font-weight:900;letter-spacing:2px;font-size:20px}  
.controls{display:flex;gap:12px;align-items:center}  
.search-wrap{position:relative}  
.search-wrap input{padding:12px 44px 12px 14px;border-radius:12px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:var(--text);min-width:260px}  
.search-wrap .icon{position:absolute;right:12px;top:50%;transform:translateY(-50%);opacity:.9}  
.container{max-width:100%;margin:16px auto;padding:0 12px}  
.hero{display:flex;align-items:center;gap:28px;margin-bottom:24px}  
.hero .left{flex:1}  
.hero h1{margin:0;font-size:22px;line-height:1.15}  
.hero p{color:var(--muted);margin-top:8px}  
.actions-row{display:flex;gap:12px;margin-top:16px}  
.btn-large{background:linear-gradient(135deg,var(--accent),#ff6a2a);border:none;color:#140807;padding:12px 18px;border-radius:14px;font-weight:900;cursor:pointer}  
.grid{display:grid;grid-template-columns:1fr;gap:16px}  
.card{position:relative;border-radius:14px;background:linear-gradient(180deg,#1b0706,#100303);overflow:hidden;border:1px solid rgba(255,255,255,.03);box-shadow:0 10px 30px rgba(0,0,0,.6);transition:transform .28s cubic-bezier(.2,.9,.3,1),box-shadow .28s}  
.card:hover{transform:translateY(-10px);box-shadow:0 30px 80px rgba(0,0,0,.8)}  
.media{position:relative;height:0;padding-bottom:100%;overflow:hidden}  
.media img,.media video{position:absolute;inset:0;width:100%;height:100%;object-fit:cover;display:block}  
.media::after{content:'';position:absolute;left:0;right:0;bottom:0;height:45%;background:linear-gradient(180deg,transparent,rgba(0,0,0,.7));}  
.badge{position:absolute;top:12px;left:12px;padding:8px 12px;border-radius:999px;font-weight:900;font-size:12px;color:#140807}  
.badge.hot{background:linear-gradient(135deg,var(--hot),#ff6a2a)}  
.badge.new{background:linear-gradient(135deg,var(--new),#ffd166)}  
.info{padding:12px}  
.title{font-weight:900;margin:0 0 6px;font-size:14px}  
.meta{display:flex;align-items:center;justify-content:space-between;gap:12px}  
.price{font-weight:900;color:var(--accent2)}  
.actions{display:flex;gap:10px;margin-top:12px}  
.btn{flex:1;border-radius:10px;padding:8px 10px;font-weight:900;border:none;background:linear-gradient(135deg,var(--accent),#ff6a2a);color:#140807;cursor:pointer;font-size:13px}  
.btn.secondary{background:transparent;color:var(--text);border:1px solid rgba(255,255,255,.03)}  
.modal{position:fixed;inset:0;background:rgba(0,0,0,.7);display:flex;align-items:center;justify-content:center;z-index:80;opacity:0;pointer-events:none;transition:opacity .2s}  
.modal.open{opacity:1;pointer-events:auto}  
.modal .box{max-width:1100px;width:96%;max-height:90%;background:#000;border-radius:12px;overflow:hidden}  
.modal video,.modal img{width:100%;height:auto;display:block}  
@media(max-width:720px){.hero{flex-direction:column;align-items:flex-start}.header-wrap{padding:12px}.brand{font-size:14px}.search-wrap input{min-width:120px;padding:8px 34px 8px 10px}.grid{gap:12px}.price{font-size:13px}}  
</style>  
</head>  
<body>  
<header>  
  <div class="header-wrap">  
    <div class="brand">EXCLUSIVE STORE</div>  
    <div class="controls">  
      <div class="search-wrap">  
        <input id="q" placeholder="Search packs..." oninput="filterPacks()" />  
        <svg class="icon" width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" d="M21 21l-4.35-4.35"/><circle cx="11" cy="11" r="6" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>  
      </div>  
    </div>  
  </div>  
</header>  
<main class="container">  
  <section class="hero">  
    <div class="left">  
      <h1>Premium Exclusive Packs</h1>  
      <p>Premium exclusive packages. For you, our valued customer! Happy Shopping</p>  
    </div>  
  </section>  
  <section id="grid" class="grid"></section>  
</main>  
<div id="modal" class="modal" onclick="closeModal(event)"><div class="box" id="modalBox"></div></div>  
<script>  
const fixedPacks = [  
  {title:'Sexxyy-Daisy', src:'https://files.catbox.moe/up90wd.jpg', type:'image'},  
  {title:'Luminatria', src:'https://files.catbox.moe/lbvlcv.jpg', type:'image'},  
  {title:'FaeXXXFae', src:'https://files.catbox.moe/1ktkca.jpg', type:'image'},  
  {title:'Karenka9', src:'https://files.catbox.moe/n7aerm.jpg', type:'image'}  
];const extraPacks = [ {title:'Pack 1', src:'https://files.catbox.moe/edb5c0.png', type:'image'}, {title:'Pack 2', src:'https://files.catbox.moe/pwltnh.png', type:'image'}, {title:'Pack 3', src:'https://files.catbox.moe/53v8s2.mp4', type:'video'}, {title:'Pack 4', src:'https://files.catbox.moe/k4ski4.mp4', type:'video'}, {title:'Pack 5', src:'https://files.catbox.moe/rakh1g.png', type:'image'}, {title:'Pack 6', src:'https://files.catbox.moe/1o0drn.png', type:'image'}, {title:'Pack 7', src:'https://files.catbox.moe/eyfxt2.png', type:'image'}, {title:'Pack 8', src:'https://files.catbox.moe/nrl8s6.png', type:'image'}, {title:'Pack 9', src:'https://files.catbox.moe/zuypup.png', type:'image'}, {title:'Pack 10', src:'https://files.catbox.moe/xzfyfe.png', type:'image'}, {title:'Pack 11', src:'https://files.catbox.moe/tmz982.png', type:'image'}, {title:'Pack 12', src:'https://files.catbox.moe/y7ratw.mp4', type:'video'}, {title:'Pack 13', src:'https://files.catbox.moe/ij3efg.png', type:'image'}, {title:'RedRoseLaCubana', src:'https://files.catbox.moe/7ih8du.jpeg', type:'image'}, {title:'GoddessOnline', src:'https://files.catbox.moe/t48ax9.jpg', type:'image'}, {title:'BrattyDeath', src:'https://files.catbox.moe/jt3xrt.jpg', type:'image'}, {title:'SkyBellaa', src:'https://files.catbox.moe/ncfypb.jpg', type:'image'}, {title:'FlexiWithSophie', src:'https://files.catbox.moe/61tl6z.jpg', type:'image'}, {title:'HelloTrinity', src:'https://files.catbox.moe/2igvjo.jpg', type:'image'}, {title:'Alice_Rojas09', src:'https://files.catbox.moe/3ofudv.jpg', type:'image'}, {title:'PremiumBabyy', src:'https://files.catbox.moe/cxbu4l.jpg', type:'image'}, {title:'ManuzinhaBlack', src:'https://files.catbox.moe/o1nfog.jpg', type:'image'}, {title:'ExoticPetiteShi', src:'https://files.catbox.moe/0yzj3h.jpg', type:'image'}, {title:'MiliLuv', src:'https://files.catbox.moe/draar2.jpg', type:'image'}, {title:'5x1.5 Pack', src:'https://files.catbox.moe/qklo0p.jpg', type:'image'}, {title:'MangoBeetle', src:'https://files.catbox.moe/grm2ea.jpg', type:'image'}, {title:'AdoreXKeya', src:'https://files.catbox.moe/a0rglv.jpg', type:'image'}, {title:'ZoeyUso', src:'https://files.catbox.moe/cfbyll.jpg', type:'image'}, {title:'SolesOfYanna', src:'https://files.catbox.moe/vosb3t.jpg', type:'image'}, {title:'MsLyLy00', src:'https://files.catbox.moe/bo663u.jpg', type:'image'}, {title:'DigitalSluut', src:'https://files.catbox.moe/vpae0q.jpg', type:'image'}, {title:'5x1.4 Pack', src:'https://files.catbox.moe/t3jich.jpg', type:'image'}, {title:'HazelGrandyy', src:'https://files.catbox.moe/sielgk.jpg', type:'image'} ]; const packs = [...fixedPacks, ...extraPacks];  

function render(pcks){ 
  const grid=document.getElementById('grid'); 
  grid.innerHTML=''; 
  pcks.forEach((p)=>{ 
    const card=document.createElement('div');card.className='card'; 
    const media=document.createElement('div');media.className='media'; 
    const img=document.createElement('img');img.src=p.src;img.loading='lazy';img.decoding='async';img.alt=p.title;media.appendChild(img); 
    const info=document.createElement('div');info.className='info'; 
    const title=document.createElement('h3');title.className='title';title.textContent=p.title;info.appendChild(title); 
    const meta=document.createElement('div');meta.className='meta'; 
    const price=document.createElement('div');price.className='price';price.textContent='$20.00';meta.appendChild(price); 
    info.appendChild(meta); 
    const actions=document.createElement('div');actions.className='actions'; 
    const pay=document.createElement('a');pay.className='btn';pay.textContent='PAY';pay.href='https://www.paypal.com/ncp/payment/2TNYU8KBXQVJS'; 
    const tg=document.createElement('a');tg.className='btn secondary';tg.textContent='TELEGRAM';tg.href='https://t.me/Hotvibespacks';tg.target='_blank'; 
    actions.appendChild(pay);actions.appendChild(tg); 
    info.appendChild(actions); card.appendChild(media);card.appendChild(info); grid.appendChild(card); 
  }); 
} 

render(packs); 

function filterPacks(){const q=document.getElementById('q').value.toLowerCase();const filtered=packs.filter(p=>p.title.toLowerCase().includes(q));render(filtered)} 
function closeModal(e){if(e.target.id==='modal'){document.getElementById('modal').classList.remove('open');document.getElementById('modalBox').innerHTML=''}} 
</script>

</body>  
</html>
