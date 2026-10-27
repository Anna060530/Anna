<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>數位名片產生器</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
:root{--acc:#5b4bff;--p:#fff;--t:#161d2f;--m:#667085;--f:#f2f4f8;--l:#e3e7ee;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--p:#171e30;--t:#eef1f8;--m:#9aa5ba;--f:#212a40;--l:#2c3650}}
:root[data-theme="dark"]{--p:#171e30;--t:#eef1f8;--m:#9aa5ba;--f:#212a40;--l:#2c3650}
html{scroll-padding-top:env(safe-area-inset-top,0px);background:#0c1120}
*{box-sizing:border-box}
body{margin:0;min-height:100vh;color:var(--t);font:15px/1.6 "Noto Sans TC","PingFang TC","Microsoft JhengHei",sans-serif;background:radial-gradient(700px 600px at 72% 40%,color-mix(in srgb,var(--acc) 38%,transparent),transparent 70%),#0c1120}
button,input,textarea{font:inherit;color:inherit}
:focus-visible{outline:3px solid var(--acc);outline-offset:2px}
.app{display:grid;grid-template-columns:minmax(0,480px) 1fr;gap:40px;max-width:1100px;margin:0 auto;padding:28px 24px;min-height:100vh}
.ed{background:var(--p);border-radius:28px;padding:24px;align-self:start;box-shadow:0 30px 60px -30px #000}
.brand{display:flex;align-items:center;gap:12px;margin-bottom:18px}
.mk{width:42px;height:42px;border-radius:13px;display:grid;place-items:center;font-size:21px;background:var(--acc);color:#fff}
.brand b{display:block;font-size:18px;font-weight:900;line-height:1.2}.brand span{font-size:13px;color:var(--m)}
.steps{display:grid;grid-template-columns:repeat(5,1fr);gap:6px;background:var(--f);padding:5px;border-radius:16px;margin-bottom:22px}
.steps button{border:0;background:none;border-radius:12px;padding:9px 4px;font-weight:700;font-size:14px;color:var(--m);cursor:pointer;transition:background .15s,color .15s}
.steps button[aria-selected=true]{background:var(--p);color:var(--acc);box-shadow:0 2px 8px -2px rgba(20,30,60,.25)}
.sec{display:none}.sec.on{display:block}
.sec h2{font-size:22px;margin:0 0 4px;font-weight:900}
.sec .sub{color:var(--m);margin:0 0 18px;font-size:14px}
.sec h3{font-size:15px;margin:24px 0 8px;font-weight:700}
.g{display:grid;grid-template-columns:1fr 1fr;gap:12px}.fu{grid-column:1/-1}
label.f{display:block;font-size:13px;font-weight:500;color:var(--m)}
.in{display:block;width:100%;margin-top:5px;min-height:44px;border:1.5px solid transparent;background:var(--f);border-radius:12px;padding:10px 13px;color:var(--t);transition:border-color .15s,box-shadow .15s,background .15s}
.in:focus{outline:0;background:var(--p);border-color:var(--acc);box-shadow:0 0 0 4px color-mix(in srgb,var(--acc) 18%,transparent)}
textarea.in{resize:vertical;min-height:76px}
.up{display:flex;align-items:center;gap:14px;padding:12px;border:1.5px dashed var(--l);border-radius:16px}
.th{width:60px;height:60px;border-radius:50%;background:var(--f) center/cover no-repeat;flex:none}.th.sq{border-radius:14px;background-size:contain}
.btn{border:1.5px solid var(--l);background:var(--p);border-radius:12px;padding:9px 16px;cursor:pointer;font-weight:700;transition:transform .12s,background .15s}
.btn:hover{background:var(--f)}.btn:active{transform:scale(.97)}
.btn.p{background:var(--acc);border-color:var(--acc);color:#fff}.btn.p:hover{filter:brightness(1.1)}
.btn.lg{width:100%;padding:14px;font-size:16px;border-radius:14px}
.btn.add{width:100%;border-style:dashed;color:var(--acc);background:none;border-color:color-mix(in srgb,var(--acc) 50%,var(--l))}
.row{display:grid;grid-template-columns:48px 1fr 1.3fr 34px 34px;gap:6px;align-items:center;margin-bottom:8px}
.row .in{margin:0}
.ico{width:48px;height:48px;border-radius:13px;border:1.5px dashed color-mix(in srgb,var(--acc) 45%,var(--l));background:var(--f);display:grid;place-items:center;font-size:21px;cursor:pointer;overflow:hidden;padding:0}
.ico img,.pi img,.soc img{width:100%;height:100%;object-fit:cover}
.x{border:0;background:none;color:var(--m);font-size:22px;cursor:pointer;border-radius:8px;height:34px}.x:hover{color:#e5384f}
.nav{display:flex;gap:10px;margin-top:28px}.nav .btn{flex:1}
.sw{display:flex;gap:10px;flex-wrap:wrap}
.sw button{width:38px;height:38px;border-radius:50%;border:3px solid var(--p);box-shadow:0 0 0 1.5px var(--l);cursor:pointer}
.sw button[aria-pressed=true]{box-shadow:0 0 0 3px var(--t)}
.guide{margin:0;padding:0;list-style:none;display:grid;gap:10px;counter-reset:s}
.guide li{counter-increment:s;display:grid;grid-template-columns:28px 1fr;gap:10px;font-size:14px}
.guide li::before{content:counter(s);width:28px;height:28px;border-radius:50%;background:var(--acc);color:#fff;font-weight:700;display:grid;place-items:center;font-size:13px}
.guide a{color:var(--acc);font-weight:700}
.pick{position:absolute;z-index:30;background:var(--p);border:1px solid var(--l);border-radius:16px;padding:8px;display:grid;grid-template-columns:repeat(6,1fr);gap:4px;box-shadow:0 14px 34px rgba(0,0,0,.3);width:290px}
.pick button{border:0;background:none;font-size:20px;border-radius:10px;cursor:pointer;height:42px;color:var(--t)}.pick button:hover{background:var(--f)}
.pick .upl{grid-column:1/-1;font-size:13px;height:auto;padding:9px;border:1px solid var(--l);font-weight:700}
.stage{position:sticky;top:28px;align-self:start;display:flex;justify-content:center}
.stage .phone{max-width:372px;border:10px solid #05080f;border-radius:48px;box-shadow:0 0 0 2px #2a3350,0 40px 80px -20px #000;position:relative}
.stage .phone::before{content:"";position:absolute;top:8px;left:50%;transform:translateX(-50%);width:84px;height:22px;border-radius:12px;background:#05080f;z-index:3}
.fab{display:none}
.blk{display:grid;grid-template-columns:auto 1fr 34px 34px;gap:8px;align-items:center;padding:8px 12px;border:1.5px solid var(--l);border-radius:14px;margin-bottom:8px}
.blk.off .bn{opacity:.45;text-decoration:line-through}.blk .x:disabled{opacity:.25;cursor:default}
.sw2{position:relative;width:42px;height:24px;display:block}.sw2 input{position:absolute;inset:0;opacity:0;cursor:pointer;margin:0;z-index:1}
.tg{position:absolute;inset:0;border-radius:99px;background:var(--l);transition:background .15s}
.tg::after{content:"";position:absolute;top:3px;left:3px;width:18px;height:18px;border-radius:50%;background:#fff;transition:transform .15s}
.sw2 input:checked+.tg{background:var(--acc)}.sw2 input:checked+.tg::after{transform:translateX(18px)}
.sw2 input:focus-visible+.tg{outline:3px solid var(--acc);outline-offset:2px}
.chips{display:flex;flex-wrap:wrap;gap:8px}
.chip{border:1.5px solid var(--l);background:var(--p);border-radius:999px;padding:7px 14px;cursor:pointer;font-weight:500;color:var(--t)}
.chip[aria-pressed=true]{background:var(--acc);border-color:var(--acc);color:#fff}
.row.off .in,.row.off .ico{opacity:.4}.eye{font-size:17px}.row.off .eye{opacity:.35}
.modal{position:fixed;inset:0;background:rgba(5,8,15,.75);backdrop-filter:blur(6px);display:none;place-items:center;z-index:50;padding:20px}.modal.on{display:grid}
.modal>div{background:#fff;color:#161d2f;border-radius:24px;padding:26px;text-align:center}
.toast{position:fixed;left:50%;bottom:calc(24px + env(safe-area-inset-bottom,0px));transform:translateX(-50%);background:#161d2f;color:#fff;padding:11px 20px;border-radius:999px;font-size:14px;opacity:0;pointer-events:none;transition:opacity .2s;z-index:60;max-width:90vw;box-shadow:0 10px 30px rgba(0,0,0,.4)}.toast.on{opacity:1}
@media (max-width:880px){
.app{grid-template-columns:1fr;padding:14px 12px 90px;gap:0}.ed{padding:18px;border-radius:22px}
.stage{position:static}.app[data-view=edit] .stage,.app[data-view=view] .ed{display:none}
.app[data-view=view]{padding-top:24px}.stage .phone{margin:0 auto}
.fab{display:block;position:fixed;right:16px;bottom:calc(18px + env(safe-area-inset-bottom,0px));z-index:40;border:0;border-radius:999px;background:var(--acc);color:#fff;font-weight:700;padding:13px 22px;box-shadow:0 12px 28px -8px #000}
.row{grid-template-columns:48px 1fr 34px 34px}.row .u{grid-column:2/5;grid-row:2}}
@media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
<style id="cardcss">
.phone{width:100%;max-width:430px;margin:0 auto;border-radius:30px;background:#fff;overflow:hidden;color:#161d2f;font-family:"Noto Sans TC","PingFang TC","Microsoft JhengHei",sans-serif;line-height:1.6;--c:#5b4bff}
.phone *{box-sizing:border-box}
.top{padding:50px 20px 78px;text-align:center;color:#fff;min-height:120px;background:radial-gradient(circle at 88% 12%,rgba(255,255,255,.24) 0 56px,transparent 57px),radial-gradient(circle at 6% 92%,rgba(255,255,255,.14) 0 84px,transparent 85px),linear-gradient(160deg,var(--c),color-mix(in srgb,var(--c) 55%,#000))}
.logo{background:#fff;padding:6px 12px;border-radius:10px;height:42px;max-width:180px;object-fit:contain;display:block;margin:0 auto 8px;box-shadow:0 6px 14px -6px rgba(0,0,0,.35)}
.co{font-weight:700;font-size:15px;letter-spacing:.06em}
.body{padding:0 20px 28px;margin-top:-58px;text-align:center}
.photo{width:112px;height:112px;border-radius:50%;border:5px solid #fff;background:#e3e7ee center/cover;margin:0 auto;display:grid;place-items:center;font-size:40px;font-weight:900;color:#667085;box-shadow:0 12px 26px -8px rgba(0,0,0,.35)}
.nm{font-size:26px;font-weight:900;margin:12px 0 0;line-height:1.25}
.en{color:#667085;font-size:14px;margin:0}
.tt{font-weight:700;margin:6px 0 0;color:var(--c)}
.bio{margin:10px 0 0;color:#394559;font-size:14.5px}
.tags{display:flex;gap:6px;justify-content:center;flex-wrap:wrap;margin-top:10px}
.tags span{background:color-mix(in srgb,var(--c) 11%,#fff);color:color-mix(in srgb,var(--c) 80%,#000);border-radius:999px;padding:2px 11px;font-size:12.5px;font-weight:500}
.pbtn{display:flex;align-items:center;justify-content:center;width:100%;border:0;border-radius:16px;padding:15px;font-weight:700;font-size:16px;cursor:pointer;text-decoration:none;margin-top:18px;color:#fff;background:linear-gradient(145deg,var(--c),color-mix(in srgb,var(--c) 68%,#000));box-shadow:0 12px 22px -10px var(--c)}
.pbtn.alt{background:#fff;color:var(--c);box-shadow:inset 0 0 0 2px var(--c);margin-top:10px}
.tiles{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:16px}
.tile{display:flex;align-items:center;gap:9px;padding:11px 12px;border-radius:16px;background:#f3f5f9;color:#161d2f;text-decoration:none;font-size:14px;font-weight:500;text-align:left;min-width:0;transition:transform .15s,box-shadow .15s}
.tile:hover{transform:translateY(-2px);box-shadow:0 10px 20px -12px rgba(20,32,43,.4)}
.tile span:last-child{overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.pi{width:32px;height:32px;border-radius:11px;background:#fff;display:grid;place-items:center;font-size:17px;flex:none;overflow:hidden;box-shadow:0 2px 6px -2px rgba(20,32,43,.25)}
.list{display:grid;gap:8px;margin-top:10px}
.list .tile{border:1px solid #e3e8ee;background:#fff;padding:12px 14px}
.list .tile::after{content:"\203A";margin-left:auto;font-size:20px;color:#9aa7b3}
.soc{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;margin-top:20px}
.soc a{width:48px;height:48px;border-radius:50%;background:#f3f5f9;display:grid;place-items:center;font-size:21px;text-decoration:none;overflow:hidden;transition:transform .15s}
.soc a:hover{transform:translateY(-2px)}
.bd{font-style:normal;font-weight:900;font-size:13px;letter-spacing:-.02em;color:var(--c,var(--acc))}
.addr{margin-top:18px;padding:12px;border-radius:14px;background:#f3f5f9;font-size:13.5px;color:#667085}
.addr a{color:var(--c);font-weight:700;text-decoration:none}
.qr{margin:20px auto 0;width:max-content;padding:12px;background:#fff;border:1.5px solid #e3e8ee;border-radius:18px}
.qr small{display:block;color:#667085;font-size:12px;margin-top:4px}
.top.np{padding:36px 20px 34px;min-height:0}.body.np{margin-top:0;padding-top:22px}
</style>
</head>
<body>
<div class="app" id="app" data-view="edit">
<main class="ed">
<div class="brand"><div class="mk">🪪</div><div><b>數位名片產生器</b><span>填好資料，右邊即時預覽</span></div></div>
<div class="steps" role="tablist" id="steps"></div>

<section class="sec" data-s="0">
<h2>基本資料</h2><p class="sub">對方第一眼會看到的內容。</p>
<div class="g">
<label class="f">全名<input class="in" data-k="name"></label>
<label class="f">英文名 / 別名<input class="in" data-k="en"></label>
<label class="f">頭銜 / 職稱<input class="in" data-k="title"></label>
<label class="f">公司 / 品牌名稱<input class="in" data-k="company"></label>
<div class="up fu"><div class="th" id="th-photo"></div><div><b>形象照</b><br><button class="btn" data-up="photo">上傳</button> <button class="btn" data-clr="photo">移除</button></div></div>
<div class="up fu"><div class="th sq" id="th-logo"></div><div><b>公司 Logo</b><br><button class="btn" data-up="logo">上傳</button> <button class="btn" data-clr="logo">移除</button></div></div>
<label class="f fu">一句話簡介<textarea class="in" data-k="bio" maxlength="80" placeholder="用 1～2 句話說明你能提供的價值"></textarea></label>
<label class="f fu">亮點標籤（用逗號分隔）<input class="in" data-k="tags" placeholder="品牌策略, 社群經營"></label>
</div>
</section>

<section class="sec" data-s="1">
<h2>聯絡方式</h2><p class="sub">點一下就能撥號、寫信或開啟通訊軟體。</p>
<div class="g">
<label class="f">電話<input class="in" data-k="phone" type="tel" placeholder="0912345678"></label>
<label class="f">Email<input class="in" data-k="email" type="email" placeholder="name@example.com"></label>
<label class="f fu">實體地址<input class="in" data-k="address"></label>
<label class="f fu">Google 地圖連結（留空會依地址自動產生）<input class="in" data-k="map" placeholder="https://maps.app.goo.gl/..."></label>
</div>
<h3>LINE、微信等通訊軟體</h3>
<div id="L-contact"></div><button class="btn add" data-add="contact">＋ 新增通訊軟體</button>
</section>

<section class="sec" data-s="2">
<h2>社群與作品</h2><p class="sub">每一列可選圖示或上傳自己的 icon，再貼上連結。</p>
<h3>社群平台</h3>
<div id="L-social"></div><button class="btn add" data-add="social">＋ 新增社群連結</button>
<h3>作品集、官網、簡報、價目表</h3>
<div id="L-work"></div><button class="btn add" data-add="work">＋ 新增連結</button>
<h3>行動呼籲按鈕</h3>
<div class="g">
<label class="f">按鈕文字<input class="in" data-k="ctaLabel" placeholder="預約諮詢"></label>
<label class="f">按鈕連結<input class="in" data-k="ctaUrl" placeholder="https://..."></label>
</div>
</section>

<section class="sec" data-s="3">
<h2>版面與背景</h2><p class="sub">用開關決定要不要顯示，用 ▲▼ 調整位置，也可以換背景。</p>
<h3 style="margin-top:0">區塊順序與顯示</h3><div id="blocks"></div>
<p class="sub" style="margin:-2px 0 0">頭像、姓名與頭銜固定在最上方。</p>
<h3>小項目（亮起代表顯示）</h3><div class="chips" id="parts"></div>
<h3>背景圖片</h3>
<div class="up"><div class="th sq" id="th-bgTop"></div><div><b>頂部橫幅背景</b><br><button class="btn" data-up="bgTop">上傳</button> <button class="btn" data-clr="bgTop">移除</button></div></div>
<div class="up" style="margin-top:10px"><div class="th sq" id="th-bgCard"></div><div><b>整張名片背景</b><br><button class="btn" data-up="bgCard">上傳</button> <button class="btn" data-clr="bgCard">移除</button></div></div>
</section>

<section class="sec" data-s="4">
<h2>完成並取得網址</h2><p class="sub">選好顏色，下載你的名片網頁，上傳後就會有專屬網址。</p>
<h3 style="margin-top:0">主題色</h3><div class="sw" id="sw"></div>
<h3>1. 下載名片網頁</h3>
<button class="btn p lg" id="dlh">下載我的名片網頁（index.html）</button>
<h3>2. 上傳到免費空間，取得專屬網址</h3>
<ol class="guide">
<li><span>開啟 <a href="https://app.netlify.com/drop" target="_blank" rel="noopener">Netlify Drop</a>，把檔案拖進去，馬上得到網址。想永久保留，請登入帳號並認領網站。也可以用 GitHub Pages 或 Cloudflare Pages。</span></li>
<li><span>把得到的網址貼在下面，再複製到 NFC。</span></li>
</ol>
<label class="f" style="margin-top:12px">你的專屬網址<input class="in" data-k="cardUrl" placeholder="https://你的名片.netlify.app"></label>
<h3>3. 寫入 NFC 卡</h3>
<ol class="guide">
<li><span>手機安裝 <b>NFC Tools</b>（iOS／Android 都有）。</span></li>
<li><span>選「寫入」→「新增紀錄」→「網址 / URL」，貼上專屬網址。</span></li>
<li><span>按「寫入」，把 NFC 卡貼近手機背面。完成後用另一支手機碰卡測試。</span></li>
</ol>
<div style="display:flex;gap:8px;margin-top:14px;flex-wrap:wrap"><button class="btn" id="cu">複製網址</button><button class="btn" id="dl">下載 .vcf</button><button class="btn" id="cp">複製 vCard</button><button class="btn" id="rs">重設範例</button></div>
</section>

<div class="nav"><button class="btn" id="bk">上一步</button><button class="btn p" id="nx">下一步</button></div>
</main>
<aside class="stage"><div class="phone" id="card"></div></aside>
</div>
<button class="fab" id="fab">預覽名片</button>
<div class="modal" id="md"><div><div id="bigqr"></div><p style="margin:10px 0 0">請對方用手機相機掃描</p></div></div>
<div class="toast" id="ts"></div>

<script>
var KEY="digital-card-v1",STEPS=["基本","聯絡","連結","版面","完成"],step=0;
var ICONS=["t:in","t:IG","t:f","t:Th","t:GH","t:Be","t:Md","t:X","t:YT","t:LINE","t:微信","💬","📞","✉️","🌐","📄","📍","📅","🛒","🎨","💼","🔗"];
var COLORS=["#5b4bff","#0d7a5f","#d6246e","#e0661a","#0e7490","#1d2433"];
var SAMPLE={name:"林雅婷",en:"Tina Lin",title:"品牌策略總監",company:"晨曦設計",photo:"",logo:"",bio:"我協助中小品牌把故事說清楚，讓客人記得你。",tags:"品牌策略, 社群經營, 活動企劃",phone:"0912345678",email:"tina@example.com",address:"台北市信義區松仁路 100 號",map:"",ctaLabel:"預約 30 分鐘諮詢",ctaUrl:"https://example.com/book",cardUrl:"",color:COLORS[0],
contact:[{icon:"t:LINE",label:"LINE",url:"https://line.me/ti/p/~yourid"}],
social:[{icon:"t:in",label:"LinkedIn",url:"https://linkedin.com/in/yourname"},{icon:"t:IG",label:"Instagram",url:"https://instagram.com/yourname"},{icon:"t:f",label:"Facebook",url:"https://facebook.com/yourname"}],
work:[{icon:"🌐",label:"官方網站",url:"https://example.com"},{icon:"📄",label:"服務價目表 PDF",url:"https://example.com/pricing.pdf"}]};
var S;try{S=JSON.parse(localStorage.getItem(KEY)||"null")}catch(e){S=null}
if(!S||typeof S!=="object"||!S.contact)S=JSON.parse(JSON.stringify(SAMPLE));
var $=function(s,r){return(r||document).querySelector(s)},$$=function(s,r){return[].slice.call((r||document).querySelectorAll(s))};
function esc(t){return String(t==null?"":t).replace(/[&<>"']/g,function(c){return{"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]})}
function save(){try{localStorage.setItem(KEY,JSON.stringify(S))}catch(e){}}
function toast(m){var t=$("#ts");t.textContent=m;t.classList.add("on");clearTimeout(toast.t);toast.t=setTimeout(function(){t.classList.remove("on")},2400)}
function href(u){u=(u||"").trim();if(!u||/^(javascript|data|vbscript):/i.test(u))return"";if(/^(https?:|tel:|mailto:|sms:|weixin:|line:)/i.test(u))return u;if(/^[\w.-]+@[\w.-]+\.\w+$/.test(u))return"mailto:"+u;return"https://"+u}
function ico(i){if(!i)return"🔗";if(i.indexOf("data:")===0)return'<img alt="" src="'+esc(i)+'">';if(i.indexOf("t:")===0)return'<i class="bd">'+esc(i.slice(2))+'</i>';return esc(i)}
function shrink(f,max,cb){var r=new FileReader();r.onload=function(){var im=new Image();im.onload=function(){var k=Math.min(1,max/Math.max(im.width,im.height)),c=document.createElement("canvas");c.width=Math.round(im.width*k);c.height=Math.round(im.height*k);c.getContext("2d").drawImage(im,0,0,c.width,c.height);cb(c.toDataURL(f.type==="image/png"?"image/png":"image/jpeg",.85))};im.onerror=function(){toast("無法讀取這張圖片")};im.src=r.result};r.readAsDataURL(f)}
function pickFile(max,cb){var f=document.createElement("input");f.type="file";f.accept="image/*";f.onchange=function(){if(f.files[0])shrink(f.files[0],max,cb)};f.click()}

/* steps */
function goStep(n){step=Math.max(0,Math.min(4,n));$$(".sec").forEach(function(s){s.classList.toggle("on",+s.dataset.s===step)});
$$("#steps button").forEach(function(b,i){b.setAttribute("aria-selected",i===step)});
$("#bk").style.visibility=step?"visible":"hidden";$("#nx").style.display=step===4?"none":"";scrollTo(0,0)}
$("#steps").innerHTML=STEPS.map(function(s,i){return'<button role="tab" data-st="'+i+'">'+s+'</button>'}).join("");
$("#bk").onclick=function(){goStep(step-1)};$("#nx").onclick=function(){goStep(step+1)};
$("#fab").onclick=function(){var a=$("#app");var v=a.dataset.view==="edit"?"view":"edit";a.dataset.view=v;this.textContent=v==="edit"?"預覽名片":"返回編輯";scrollTo(0,0)};

/* editor */
function fill(){$$("[data-k]").forEach(function(el){el.value=S[el.dataset.k]||""});$("#th-photo").style.backgroundImage=S.photo?'url("'+S.photo+'")':"";$("#th-logo").style.backgroundImage=S.logo?'url("'+S.logo+'")':"";["bgTop","bgCard"].forEach(function(k){$("#th-"+k).style.backgroundImage=S[k]?'url("'+S[k]+'")':""})}
function lists(){["contact","social","work"].forEach(function(k){var box=$("#L-"+k);box.innerHTML="";S[k].forEach(function(it,i){
var d=document.createElement("div");d.className="row"+(it.off?" off":"");
d.innerHTML='<button class="ico" aria-label="選擇圖示">'+ico(it.icon)+'</button><input class="in" placeholder="名稱" value="'+esc(it.label)+'"><input class="in u" placeholder="連結或帳號網址" value="'+esc(it.url)+'"><button class="x eye" aria-label="顯示或隱藏" title="顯示或隱藏">👁</button><button class="x del" aria-label="刪除">×</button>';
var n=$$("input",d);n[0].oninput=function(){it.label=this.value;save();paint()};n[1].oninput=function(){it.url=this.value;save();paint()};
$(".eye",d).onclick=function(){it.off=!it.off;d.classList.toggle("off",!!it.off);save();paint()};$(".del",d).onclick=function(){S[k].splice(i,1);save();lists();paint()};
$(".ico",d).onclick=function(e){e.stopPropagation();pick(this,it)};box.appendChild(d)})})}
function closePick(){$$(".pick").forEach(function(p){p.remove()})}
function pick(btn,it){closePick();var p=document.createElement("div");p.className="pick";
ICONS.forEach(function(e){var b=document.createElement("button");b.innerHTML=ico(e);b.onclick=function(){it.icon=e;done()};p.appendChild(b)});
var u=document.createElement("button");u.className="upl";u.textContent="上傳自己的圖示";u.onclick=function(){pickFile(96,function(d){it.icon=d;done()})};p.appendChild(u);
function done(){closePick();save();lists();paint()}
var r=btn.getBoundingClientRect();p.style.left=Math.max(8,Math.min(r.left+scrollX,innerWidth-300))+"px";p.style.top=(r.bottom+scrollY+6)+"px";document.body.appendChild(p)}
document.addEventListener("click",function(e){if(!e.target.closest(".pick"))closePick();var t=e.target.closest("button");if(!t)return;
if(t.dataset.st)goStep(+t.dataset.st);
if(t.dataset.add){S[t.dataset.add].push({icon:t.dataset.add==="work"?"🔗":"t:LINE",label:"",url:""});save();lists();paint();var r=$$("#L-"+t.dataset.add+" .row");$$("input",r[r.length-1])[0].focus()}
if(t.dataset.up){var f=t.dataset.up;pickFile(f==="photo"?480:f==="logo"?300:900,function(d){S[f]=d;save();fill();paint()})}
if(t.dataset.clr){S[t.dataset.clr]="";save();fill();paint()}});
document.addEventListener("input",function(e){var k=e.target.dataset&&e.target.dataset.k;if(k){S[k]=e.target.value;save();paint()}});
$("#sw").innerHTML=COLORS.map(function(c){return'<button style="background:'+c+'" data-c="'+c+'" aria-label="主題色 '+c+'" aria-pressed="false"></button>'}).join("");
$("#sw").onclick=function(e){var b=e.target.closest("[data-c]");if(b){S.color=b.dataset.c;save();paint()}};
$("#rs").onclick=function(){if(confirm("要清除目前內容並重設為範例嗎？")){S=JSON.parse(JSON.stringify(SAMPLE));save();fill();lists();paint()}};

/* vCard */
function vE(t){return String(t||"").replace(/\\/g,"\\\\").replace(/\n/g,"\\n").replace(/,/g,"\\,").replace(/;/g,"\\;")}
function vcard(){var L=["BEGIN:VCARD","VERSION:3.0","FN:"+vE(S.name||S.en),"N:"+vE(S.name)+";;;;"];
if(S.en)L.push("NICKNAME:"+vE(S.en));if(S.company)L.push("ORG:"+vE(S.company));if(S.title)L.push("TITLE:"+vE(S.title));
if(S.phone&&!hid("phone"))L.push("TEL;TYPE=CELL:"+S.phone.replace(/[^\d+]/g,""));if(S.email&&!hid("email"))L.push("EMAIL:"+S.email);
if(S.address)L.push("ADR;TYPE=WORK:;;"+vE(S.address)+";;;;");
var u=href(S.cardUrl);if(u)L.push("URL:"+u);if(S.bio)L.push("NOTE:"+vE(S.bio));
if(S.photo&&S.photo.indexOf("data:image/jpeg")===0)L.push("PHOTO;ENCODING=b;TYPE=JPEG:"+S.photo.split(",")[1]);
L.push("END:VCARD");return L.join("\r\n")}
function saveFile(name,data,type,okMsg){
var done=function(){var a=document.createElement("a");a.href=URL.createObjectURL(new Blob([data],{type:type}));a.download=name;document.body.appendChild(a);a.click();a.remove();toast(okMsg)};
if(typeof claude!=="undefined"&&claude.use&&/\.(html|txt|json|md)$/.test(name)){claude.use("downloads").then(function(d){if(!d)return done();return d.save({filename:name,data:data}).then(function(){toast(okMsg)},function(e){if(!e||e.code!=="declined")toast("下載失敗，請重試")})},done)}else done()}
function copy(t,m){(navigator.clipboard?navigator.clipboard.writeText(t):Promise.reject()).then(function(){toast(m)},function(){var a=document.createElement("textarea");a.value=t;document.body.appendChild(a);a.select();try{document.execCommand("copy");toast(m)}catch(e){toast("複製失敗")}a.remove()})}
$("#dl").onclick=function(){saveFile((S.name||"contact")+".vcf",vcard(),"text/vcard;charset=utf-8","已產生通訊錄檔")};
$("#cp").onclick=function(){copy(vcard(),"已複製，可貼到記事本存成 .vcf")};
$("#cu").onclick=function(){S.cardUrl?copy(href(S.cardUrl),"網址已複製"):toast("請先貼上你的專屬網址")};

/* card */
var BLK={bio:"簡介與標籤",save:"存入通訊錄按鈕",cta:"行動呼籲按鈕",contact:"聯絡方式",work:"作品與服務連結",social:"社群圖示",address:"地址與導航",qr:"QR Code"};
var DEFORD=["bio","save","cta","contact","work","social","address","qr"];
var PARTS={photo:"形象照",logo:"Logo",company:"公司名稱",en:"英文名",title:"頭銜",tags:"亮點標籤",phone:"電話",email:"Email"};
function hid(k){return !!(S.hide&&S.hide[k])}
function ord(){var o=(S.order||[]).filter(function(k){return BLK[k]});DEFORD.forEach(function(k){if(o.indexOf(k)<0)o.push(k)});return o}
function renderBlocks(){var o=ord(),b=$("#blocks");b.innerHTML="";
o.forEach(function(k,i){var d=document.createElement("div");d.className="blk"+(hid(k)?" off":"");
d.innerHTML='<label class="sw2"><input type="checkbox" aria-label="顯示 '+BLK[k]+'"'+(hid(k)?"":" checked")+'><span class="tg"></span></label><span class="bn">'+BLK[k]+'</span><button class="x" aria-label="上移"'+(i?"":" disabled")+'>▲</button><button class="x" aria-label="下移"'+(i<o.length-1?"":" disabled")+'>▼</button>';
var bt=$$("button",d);
$("input",d).onchange=function(){S.hide=S.hide||{};S.hide[k]=!this.checked;save();renderBlocks();paint()};
function mv(n){var j=i+n,t=o[i];o[i]=o[j];o[j]=t;S.order=o;save();renderBlocks();paint()}
bt[0].onclick=function(){mv(-1)};bt[1].onclick=function(){mv(1)};b.appendChild(d)})}
function renderParts(){$("#parts").innerHTML=Object.keys(PARTS).map(function(k){return'<button class="chip" data-part="'+k+'" aria-pressed="'+!hid(k)+'">'+PARTS[k]+'</button>'}).join("")}
document.addEventListener("click",function(e){var t=e.target.closest("[data-part]");if(t){var k=t.dataset.part;S.hide=S.hide||{};S.hide[k]=!S.hide[k];save();renderParts();paint()}});
function cardBg(){return S.bgCard?"background-image:linear-gradient(rgba(255,255,255,.88),rgba(255,255,255,.88)),url('"+S.bgCard+"');background-size:cover;background-position:center;":""}
function cardHTML(ex){var H=hid,np=H("photo"),B={};
var bg=S.bgTop?' style="background-image:linear-gradient(rgba(0,0,0,.28),rgba(0,0,0,.28)),url(\''+S.bgTop+'\');background-size:cover;background-position:center"':"";
var h='<div class="top'+(np?" np":"")+'"'+bg+'>'+(S.logo&&!H("logo")?'<img class="logo" alt="" src="'+esc(S.logo)+'">':"")+(S.company&&!H("company")?'<div class="co">'+esc(S.company)+'</div>':"")+'</div><div class="body'+(np?" np":"")+'">';
if(!np)h+='<div class="photo"'+(S.photo?' style="background-image:url(\''+S.photo+'\')"':"")+'>'+(S.photo?"":esc((S.name||S.en||"?").charAt(0)))+'</div>';
h+='<h1 class="nm">'+esc(S.name||"你的姓名")+'</h1>'+(S.en&&!H("en")?'<p class="en">'+esc(S.en)+'</p>':"")+(S.title&&!H("title")?'<p class="tt">'+esc(S.title)+'</p>':"");
var tg=(S.tags||"").split(/[,，、]/).map(function(x){return x.trim()}).filter(Boolean);
B.bio=(S.bio?'<p class="bio">'+esc(S.bio)+'</p>':"")+(tg.length&&!H("tags")?'<div class="tags">'+tg.map(function(t){return"<span>"+esc(t)+"</span>"}).join("")+'</div>':"");
B.save='<button class="pbtn" id="pv-save">存入手機通訊錄</button>';
B.cta=S.ctaLabel&&href(S.ctaUrl)?'<a class="pbtn alt" target="_blank" rel="noopener" href="'+esc(href(S.ctaUrl))+'">'+esc(S.ctaLabel)+'</a>':"";
var ti=[];if(S.phone&&!H("phone"))ti.push(['📞','撥打電話','tel:'+S.phone.replace(/[^\d+]/g,"")]);if(S.email&&!H("email"))ti.push(['✉️',S.email,'mailto:'+S.email]);
S.contact.forEach(function(i){if(href(i.url)&&!i.off)ti.push([i.icon,i.label||i.url,href(i.url)])});
B.contact=ti.length?'<div class="tiles">'+ti.map(function(t){return'<a class="tile" target="_blank" rel="noopener" href="'+esc(t[2])+'"><span class="pi">'+ico(t[0])+'</span><span>'+esc(t[1])+'</span></a>'}).join("")+'</div>':"";
var wk=S.work.filter(function(i){return href(i.url)&&!i.off});
B.work=wk.length?'<div class="list">'+wk.map(function(i){return'<a class="tile" target="_blank" rel="noopener" href="'+esc(href(i.url))+'"><span class="pi">'+ico(i.icon)+'</span><span>'+esc(i.label||i.url)+'</span></a>'}).join("")+'</div>':"";
var sc=S.social.filter(function(i){return href(i.url)&&!i.off});
B.social=sc.length?'<div class="soc">'+sc.map(function(i){return'<a target="_blank" rel="noopener" aria-label="'+esc(i.label||"社群連結")+'" href="'+esc(href(i.url))+'">'+ico(i.icon)+'</a>'}).join("")+'</div>':"";
var mp=href(S.map)||(S.address?"https://www.google.com/maps/search/?api=1&query="+encodeURIComponent(S.address):"");
B.address=S.address?'<p class="addr">'+esc(S.address)+'<br><a target="_blank" rel="noopener" href="'+esc(mp)+'">Google 地圖導航</a></p>':"";
B.qr='<div class="qr" id="qr"'+(ex?"":' role="button" tabindex="0" aria-label="放大 QR Code" style="cursor:pointer"')+'><div id="qrbox"></div><small>'+(ex?"掃描即可分享給朋友":"點擊放大，讓對方掃描")+'</small></div>';
ord().forEach(function(k){if(!H(k))h+=B[k]||""});return h+'</div>'}
function paint(){var c=S.color||COLORS[0];document.documentElement.style.setProperty("--acc",c);$$("#sw button").forEach(function(b){b.setAttribute("aria-pressed",b.dataset.c===c)});
var card=$("#card");card.style.setProperty("--c",c);card.innerHTML=cardHTML(false);card.style.backgroundImage=S.bgCard?cardBg().match(/^background-image:(.*?);background-size/)[1]:"";card.style.backgroundSize="cover";card.style.backgroundPosition="center";
$("#pv-save").onclick=function(){$("#dl").click()};drawQR($("#qrbox"),132);
$("#qr").onclick=function(){drawQR($("#bigqr"),260);$("#md").classList.add("on")};$("#qr").onkeydown=function(e){if(e.key==="Enter")this.click()}}
function drawQR(el,n){el.innerHTML="";var u=href(S.cardUrl)||"https://example.com";try{new QRCode(el,{text:u,width:n,height:n,correctLevel:QRCode.CorrectLevel.M})}catch(e){el.textContent="QR Code 載入失敗"}}
$("#md").onclick=function(){this.classList.remove("on")};
document.addEventListener("keydown",function(e){if(e.key==="Escape"){$("#md").classList.remove("on");closePick()}});

/* export standalone page */
function exportHtml(){var c=S.color||COLORS[0],css=$("#cardcss").textContent;
var vc=JSON.stringify(vcard()).replace(/</g,"\\u003c"),nm=JSON.stringify((S.name||"contact")+".vcf");
return'<!DOCTYPE html><html lang="zh-Hant"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>'+esc((S.name||"")+" "+(S.title||"")+" "+(S.company||""))+'</title><meta property="og:title" content="'+esc((S.name||"")+" | "+(S.company||""))+'"><meta property="og:description" content="'+esc(S.bio||"")+'"><link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet"><style>body{margin:0;background:#eceff5;display:flex;justify-content:center;padding:0}@media(min-width:520px){body{padding:32px 0}.phone{box-shadow:0 30px 60px -30px rgba(20,30,60,.5)}}@media(max-width:519px){.phone{border-radius:0}}.bd{--acc:'+c+'}</style><style>'+css+'</style></head><body><div class="phone" id="card" style="--c:'+c+';'+cardBg()+'">'+cardHTML(true)+'</div><script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"><\/script><script>var V='+vc+';document.getElementById("pv-save").onclick=function(){var a=document.createElement("a");a.href=URL.createObjectURL(new Blob([V],{type:"text/vcard;charset=utf-8"}));a.download='+nm+';document.body.appendChild(a);a.click();a.remove()};try{new QRCode(document.getElementById("qrbox"),{text:location.href.split("#")[0],width:132,height:132})}catch(e){}<\/script></body></html>'}
$("#dlh").onclick=function(){saveFile("index.html",exportHtml(),"text/html;charset=utf-8","已下載，接著上傳到免費空間")};

S.hide=S.hide||{};fill();lists();renderBlocks();renderParts();paint();goStep(0);
</script>
</body>
</html>
