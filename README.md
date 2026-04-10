<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
<meta name="theme-color" content="#0a2218"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-title" content="Health is Wealth"/>
<meta name="description" content="Health is Wealth — Walk, Earn & Win"/>
<title>Health is Wealth — Walk & Earn</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700;800&family=DM+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet"/>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>
<style>
:root{
  --g1:#071c10;--g2:#0d4a28;--g3:#12803e;--g4:#1ab358;
  --gold:#f0b429;--gold2:#fad369;--gold3:#8a6210;
  --bg:#f2faf5;--bg2:#e0f4ea;--bg3:#c8ead8;
  --surf:#fff;--bdr:#b8ddc8;--bdr2:#90c8a8;
  --txt:#061810;--txt2:#1a3828;--txt3:#2e6040;--txt4:#5a9070;
  --green:#0d9e4a;--green2:#c8f0dc;
  --red:#c82020;--red2:#ffe0e0;
  --amber:#b86a00;--amber2:#fff0cc;
  --blue:#1448b8;--blue2:#dceeff;
  --purple:#6820d0;--purple2:#eedeff;
  --e1:0 2px 10px rgba(7,28,16,.08);
  --e2:0 8px 24px rgba(7,28,16,.13);
  --e3:0 16px 48px rgba(7,28,16,.18);
  --e4:0 28px 70px rgba(0,0,0,.28);
  --r:16px;--ra:24px;--rb:12px;
  --ease:cubic-bezier(.4,0,.2,1);
  --spring:cubic-bezier(.34,1.56,.64,1);
}
[data-theme=dark]{
  --bg:#060f08;--bg2:#0a1a0e;--bg3:#0f2416;
  --surf:#0d1f12;--bdr:#1a3422;--bdr2:#244830;
  --txt:#d8f0e2;--txt2:#a0c8b0;--txt3:#609870;--txt4:#3a7050;
  --green2:#0a2416;--red2:#280808;--amber2:#180f00;--blue2:#080e20;
}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;-webkit-font-smoothing:antialiased;}
html,body{height:100%;overflow-x:hidden;}
body{font-family:"DM Sans",sans-serif;background:var(--bg);color:var(--txt);min-height:100vh;}
h1,h2,h3{font-family:"Playfair Display",serif;}
button,input,select,textarea{font-family:"DM Sans",sans-serif;}
::-webkit-scrollbar{width:4px;}
::-webkit-scrollbar-thumb{background:var(--bdr2);border-radius:2px;}

/* SPLASH */
#splash{position:fixed;inset:0;z-index:9999;background:linear-gradient(160deg,#040e08,#071c10,#0a3018);display:flex;flex-direction:column;align-items:center;justify-content:center;transition:opacity .8s,visibility .8s;}
#splash.gone{opacity:0;visibility:hidden;pointer-events:none;}
.sp-logo{width:90px;height:90px;border-radius:50%;background:radial-gradient(circle,#1ab358,#071c10);border:2px solid rgba(240,180,41,.3);display:flex;align-items:center;justify-content:center;font-size:2.6rem;animation:pulse-ring 2s ease infinite;margin-bottom:20px;}
@keyframes pulse-ring{0%,100%{box-shadow:0 0 0 0 rgba(240,180,41,.4);}50%{box-shadow:0 0 0 20px rgba(240,180,41,0);}}
.sp-title{font-family:"Playfair Display",serif;font-size:2rem;color:#fff;font-weight:800;letter-spacing:1px;}
.sp-title span{color:var(--gold2);}
.sp-sub{font-size:.7rem;color:rgba(255,255,255,.35);letter-spacing:4px;text-transform:uppercase;margin-top:6px;}
.sp-bar{width:200px;height:2px;background:rgba(255,255,255,.08);border-radius:1px;margin-top:30px;overflow:hidden;}
.sp-fill{height:100%;background:linear-gradient(90deg,transparent,var(--gold2),transparent);animation:sp-sweep 2.4s ease-in-out forwards;}
@keyframes sp-sweep{from{transform:translateX(-100%);}to{transform:translateX(100%);}}

/* AUTH */
#authScreen{min-height:100vh;display:none;flex-direction:column;align-items:center;justify-content:center;background:linear-gradient(160deg,#040e08,#071c10,#0a3018);padding:24px 20px;}
.auth-box{width:100%;max-width:390px;background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.12);border-radius:var(--ra);padding:32px 24px;backdrop-filter:blur(20px);box-shadow:0 32px 80px rgba(0,0,0,.5);}
.auth-logo{text-align:center;margin-bottom:24px;}
.auth-ico{font-size:3rem;filter:drop-shadow(0 0 18px rgba(26,179,88,.5));margin-bottom:10px;}
.auth-logo h1{font-size:1.9rem;font-weight:800;color:#fff;letter-spacing:.5px;}
.auth-logo h1 span{color:var(--gold2);}
.auth-logo p{font-size:.68rem;color:rgba(255,255,255,.3);letter-spacing:3px;text-transform:uppercase;margin-top:5px;}
.auth-tabs{display:flex;background:rgba(0,0,0,.3);border-radius:12px;padding:4px;margin-bottom:22px;gap:4px;}
.auth-tab{flex:1;padding:10px;border:none;border-radius:9px;cursor:pointer;font-size:.84rem;font-weight:600;transition:.25s;background:transparent;color:rgba(255,255,255,.38);}
.auth-tab.on{background:linear-gradient(135deg,#0d6e3a,#1ab358);color:#fff;box-shadow:0 4px 14px rgba(13,110,58,.45);}
.ai{width:100%;padding:13px 14px;margin-bottom:10px;background:rgba(255,255,255,.07);border:1.5px solid rgba(255,255,255,.13);border-radius:11px;color:#fff;font-size:.9rem;outline:none;transition:.25s;}
.ai:focus{border-color:var(--g4);box-shadow:0 0 0 3px rgba(26,179,88,.18);}
.ai::placeholder{color:rgba(255,255,255,.28);}
.pw-row{position:relative;margin-bottom:10px;}
.pw-row .ai{margin-bottom:0;padding-right:44px;}
.pw-eye{position:absolute;right:13px;top:50%;transform:translateY(-50%);background:none;border:none;color:rgba(255,255,255,.38);cursor:pointer;font-size:1rem;padding:2px;line-height:1;}
.btn-gold{width:100%;padding:14px;border:none;border-radius:11px;cursor:pointer;background:linear-gradient(135deg,#8a6210,#f0b429,#fad369);color:#071c10;font-weight:800;font-size:.92rem;letter-spacing:.4px;box-shadow:0 6px 24px rgba(240,180,41,.45);transition:.25s;}
.btn-gold:hover{transform:translateY(-2px);box-shadow:0 10px 30px rgba(240,180,41,.5);}
.btn-gold:disabled{opacity:.5;cursor:not-allowed;transform:none;}
.auth-err{color:#ff9090;font-size:.76rem;text-align:center;min-height:18px;margin-top:6px;font-weight:500;}
.auth-ok{color:#6ee7b7;font-size:.76rem;text-align:center;font-weight:500;}
.auth-forgot{text-align:right;margin-bottom:10px;}
.auth-forgot button{background:none;border:none;color:rgba(255,255,255,.35);font-size:.74rem;cursor:pointer;}
.auth-admin-link{text-align:center;margin-top:18px;padding-top:14px;border-top:1px solid rgba(255,255,255,.07);}
.auth-admin-link button{background:none;border:none;color:rgba(255,255,255,.18);font-size:.68rem;cursor:pointer;letter-spacing:.5px;}

/* TOP BAR */
#userApp{display:none;min-height:100vh;padding-bottom:72px;}
.topbar{background:linear-gradient(135deg,#071c10,#0d6e3a);position:sticky;top:0;z-index:400;display:flex;align-items:center;justify-content:space-between;padding:12px 16px;box-shadow:0 4px 20px rgba(0,0,0,.38);}
.tb-brand{display:flex;align-items:center;gap:8px;cursor:pointer;}
.tb-ico{font-size:1.4rem;}
.tb-name{font-family:"Playfair Display",serif;font-size:1rem;font-weight:700;color:#fff;}
.tb-name span{color:var(--gold2);}
.tb-right{display:flex;align-items:center;gap:6px;}
.tb-pill{background:rgba(240,180,41,.18);border:1px solid rgba(240,180,41,.35);color:var(--gold2);padding:4px 10px;border-radius:16px;font-size:.72rem;font-weight:700;cursor:pointer;}
.tb-btn{background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.18);color:rgba(255,255,255,.65);padding:5px 9px;border-radius:8px;cursor:pointer;font-size:.78rem;}
.tb-logout{background:rgba(200,32,32,.15);border:1px solid rgba(200,32,32,.3);color:#ff9090;padding:5px 10px;border-radius:8px;cursor:pointer;font-size:.72rem;}

/* BOTTOM NAV */
.bnav{position:fixed;bottom:0;left:0;right:0;z-index:400;background:var(--surf);border-top:2px solid var(--bdr);display:none;padding-bottom:env(safe-area-inset-bottom);box-shadow:0 -4px 24px rgba(0,0,0,.1);}
.bn-item{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:2px;padding:8px 2px;background:none;border:none;cursor:pointer;transition:.2s;}
.bn-item.on{color:var(--green);}
.bn-item.on .bn-ico{transform:translateY(-3px) scale(1.15);}
.bn-item:not(.on){color:var(--txt4);}
.bn-ico{font-size:1.15rem;line-height:1;transition:.2s;}
.bn-lbl{font-size:.5rem;font-weight:700;letter-spacing:.2px;text-transform:uppercase;white-space:nowrap;}

/* PAGES */
.pg{display:none;}.pg.on{display:block;}
.wrap{max-width:480px;margin:0 auto;padding:14px 14px 16px;}

.card{background:var(--surf);border:1.5px solid var(--bdr);border-radius:var(--r);padding:16px;box-shadow:var(--e1);margin-bottom:12px;}
.card-hd{font-family:"Playfair Display",serif;font-size:1.05rem;font-weight:700;color:var(--txt);margin-bottom:12px;display:flex;align-items:center;gap:8px;}
.card-hd::before{content:"";width:3px;height:16px;background:linear-gradient(180deg,var(--g4),var(--gold));border-radius:2px;flex-shrink:0;}
.sect-hd{font-size:.68rem;color:var(--txt4);letter-spacing:2.5px;text-transform:uppercase;margin:16px 0 8px;display:flex;align-items:center;gap:8px;}
.sect-hd::after{content:"";flex:1;height:1px;background:var(--bdr);}
.btn{display:inline-flex;align-items:center;justify-content:center;gap:6px;padding:10px 16px;border:none;border-radius:10px;cursor:pointer;font-weight:700;font-size:.84rem;transition:.2s;font-family:"DM Sans",sans-serif;}
.btn-primary{background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);}
.btn-primary:hover{transform:translateY(-1px);box-shadow:0 6px 20px rgba(13,110,58,.35);}
.btn-red{background:linear-gradient(135deg,#8a1010,#c82020);color:#fff;}
.btn-outline{background:transparent;border:1.5px solid var(--bdr);color:var(--txt2);}
.btn-full{width:100%;display:flex;}
.btn:disabled{opacity:.45;cursor:not-allowed;transform:none!important;}
.inp{width:100%;padding:11px 13px;border:1.5px solid var(--bdr);border-radius:10px;font-size:.88rem;outline:none;background:var(--bg);color:var(--txt);font-family:"DM Sans",sans-serif;margin-bottom:10px;}
.inp:focus{border-color:var(--g3);background:var(--surf);box-shadow:0 0 0 3px rgba(18,128,62,.12);}
.inp::placeholder{color:var(--txt4);}
.empty{text-align:center;padding:30px 20px;color:var(--txt4);}
.empty-ico{font-size:2.5rem;margin-bottom:10px;}
.empty-txt{font-size:.84rem;}

/* HERO BANNER */
.hero-banner{background:linear-gradient(135deg,#071c10 0%,#0d6e3a 55%,#14a050 100%);border-radius:20px;padding:22px 18px;margin-bottom:14px;position:relative;overflow:hidden;box-shadow:0 12px 40px rgba(7,28,16,.3);}
.hero-banner::before{content:"";position:absolute;right:-50px;top:-50px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle,rgba(240,180,41,.15),transparent);}
.hero-banner::after{content:"";position:absolute;left:-30px;bottom:-40px;width:130px;height:130px;border-radius:50%;background:radial-gradient(circle,rgba(26,179,88,.12),transparent);}
.hero-greet{color:rgba(255,255,255,.55);font-size:.76rem;margin-bottom:2px;}
.hero-name{color:#fff;font-family:"Playfair Display",serif;font-size:1.5rem;font-weight:700;margin-bottom:14px;}
.hero-name span{color:var(--gold2);}
.hero-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;}
.hero-stat{background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.14);border-radius:12px;padding:10px;text-align:center;backdrop-filter:blur(4px);}
.hero-stat-v{font-family:"Playfair Display",serif;font-size:1.3rem;font-weight:700;color:var(--gold2);}
.hero-stat-l{font-size:.56rem;color:rgba(255,255,255,.45);margin-top:2px;letter-spacing:.5px;}

/* STAT CARDS */
.stat-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
.stat-card{background:var(--surf);border:1.5px solid var(--bdr);border-radius:var(--r);padding:14px;box-shadow:var(--e1);position:relative;overflow:hidden;}
.stat-card::before{content:"";position:absolute;top:0;left:0;right:0;height:3px;border-radius:3px 3px 0 0;}
.sc-walk::before{background:linear-gradient(90deg,#f0b429,#fad369);}
.sc-earn::before{background:linear-gradient(90deg,#0d9e4a,#1ab358);}
.sc-ref::before{background:linear-gradient(90deg,#6820d0,#a050f8);}
.sc-wallet::before{background:linear-gradient(90deg,#1448b8,#38a0f8);}
.stat-v{font-family:"Playfair Display",serif;font-size:1.7rem;font-weight:700;color:var(--txt);line-height:1;}
.stat-l{font-size:.64rem;color:var(--txt4);margin-top:2px;letter-spacing:.5px;text-transform:uppercase;}
.stat-sub{font-size:.7rem;color:var(--txt3);margin-top:4px;}
.progress-bar{height:6px;background:var(--bg2);border-radius:3px;overflow:hidden;margin-top:8px;}
.pb-fill{height:100%;border-radius:3px;transition:width .7s var(--ease);}
.pb-walk{background:linear-gradient(90deg,#f0b429,#fad369);}
.pb-earn{background:linear-gradient(90deg,#0d9e4a,#1ab358);}

/* PRIZES SECTION */
.prize-banner{background:linear-gradient(135deg,#1a0a38,#3818a0,#2a0870);border-radius:20px;padding:20px;margin-bottom:14px;position:relative;overflow:hidden;box-shadow:0 12px 40px rgba(26,10,56,.4);}
.prize-banner::before{content:"✨";position:absolute;right:10px;top:10px;font-size:3rem;opacity:.15;}
.prize-title{font-family:"Playfair Display",serif;font-size:1.1rem;color:#fff;font-weight:700;margin-bottom:4px;}
.prize-sub{font-size:.72rem;color:rgba(255,255,255,.45);margin-bottom:14px;}
.prize-list{display:flex;flex-direction:column;gap:8px;}
.prize-item{display:flex;align-items:center;gap:12px;background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.12);border-radius:12px;padding:12px 14px;}
.prize-rank{width:36px;height:36px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1.2rem;font-weight:800;flex-shrink:0;}
.pr-1{background:linear-gradient(135deg,#FFD700,#FFA500);color:#fff;}
.pr-2{background:linear-gradient(135deg,#C0C0C0,#A0A0A0);color:#fff;}
.pr-3{background:linear-gradient(135deg,#CD7F32,#A0520A);color:#fff;}
.prize-info{flex:1;}
.prize-name{font-weight:700;font-size:.9rem;color:#fff;}
.prize-amt{font-size:1.1rem;font-weight:800;color:var(--gold2);font-family:"Playfair Display",serif;}
.prize-who{font-size:.7rem;color:rgba(255,255,255,.45);margin-top:1px;}
.prize-ico-big{font-size:1.8rem;}

/* HOW IT WORKS */
.how-step{display:flex;align-items:flex-start;gap:12px;padding:12px 0;border-bottom:1px solid var(--bdr);}
.how-step:last-child{border-bottom:none;}
.how-num{width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);display:flex;align-items:center;justify-content:center;font-size:.78rem;font-weight:800;flex-shrink:0;}
.how-body{flex:1;}
.how-title{font-size:.88rem;font-weight:700;color:var(--txt2);}
.how-desc{font-size:.76rem;color:var(--txt4);margin-top:3px;line-height:1.5;}
.how-badge{display:inline-block;background:var(--gold2);color:#071c10;padding:2px 8px;border-radius:20px;font-size:.65rem;font-weight:800;margin-top:4px;}

/* GPS SECTION */
.gps-card{background:linear-gradient(135deg,#061410,#0a2a18,#0d6e3a);border-radius:20px;padding:20px;margin-bottom:14px;position:relative;overflow:hidden;box-shadow:0 14px 50px rgba(0,0,0,.3);}
.gps-card::after{content:"";position:absolute;right:-40px;bottom:-40px;width:150px;height:150px;border-radius:50%;background:radial-gradient(circle,rgba(240,180,41,.08),transparent);}
.gps-status-row{display:flex;align-items:center;gap:10px;margin-bottom:14px;}
.gps-dot{width:10px;height:10px;border-radius:50%;background:#4ade80;flex-shrink:0;}
.gps-dot.off{background:#6b7280;animation:none;}
.gps-dot.live{animation:blink 1.5s ease infinite;}
.gps-dot.paused{background:var(--gold2);animation:blink 1s ease infinite;}
@keyframes blink{0%,100%{opacity:1;}50%{opacity:.3;}}
.gps-status-txt{font-size:.84rem;color:rgba(255,255,255,.75);font-weight:600;}
.gps-metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:14px;}
.gps-metric{background:rgba(255,255,255,.09);border:1px solid rgba(255,255,255,.1);border-radius:10px;padding:10px;text-align:center;}
.gps-metric-v{font-family:"Playfair Display",serif;font-size:1.4rem;font-weight:700;color:var(--gold2);}
.gps-metric-l{font-size:.56rem;color:rgba(255,255,255,.4);margin-top:2px;}
.gps-prog-wrap{background:rgba(255,255,255,.08);border-radius:8px;height:8px;margin-bottom:10px;overflow:hidden;}
.gps-prog-fill{height:100%;background:linear-gradient(90deg,#f0b429,#fad369);border-radius:8px;transition:width .4s var(--ease);}
.gps-cap-txt{font-size:.66rem;color:rgba(255,255,255,.3);text-align:right;margin-bottom:12px;}
.gps-btns{display:flex;gap:8px;}
.gps-btn{flex:1;padding:12px;border:none;border-radius:10px;cursor:pointer;font-size:.84rem;font-weight:700;display:flex;align-items:center;justify-content:center;gap:6px;font-family:"DM Sans",sans-serif;}
.gps-start{background:linear-gradient(135deg,#0d6e3a,#1ab358);color:#fff;}
.gps-stop{background:linear-gradient(135deg,#8a1010,#c82020);color:#fff;}
.gps-pause{background:rgba(255,255,255,.12);color:#fff;border:1px solid rgba(255,255,255,.18);}
.gps-locked{background:rgba(255,255,255,.05);border:1.5px solid rgba(240,180,41,.25);border-radius:12px;padding:16px;text-align:center;margin-bottom:14px;}

/* REFERRAL BOX */
.ref-box{background:linear-gradient(135deg,#071c10,#0f3820,#0d6e3a);border-radius:20px;padding:22px;margin-bottom:14px;position:relative;overflow:hidden;box-shadow:0 12px 40px rgba(7,28,16,.3);}
.ref-box::before{content:"🌿";position:absolute;right:-5px;top:-10px;font-size:6rem;opacity:.06;}
.ref-reward{display:flex;align-items:center;gap:14px;background:rgba(240,180,41,.15);border:1px solid rgba(240,180,41,.3);border-radius:12px;padding:12px 14px;margin-bottom:14px;}
.ref-reward-amt{font-family:"Playfair Display",serif;font-size:2rem;font-weight:700;color:var(--gold2);line-height:1;}
.ref-reward-info{flex:1;}
.ref-reward-title{font-weight:700;font-size:.88rem;color:#fff;}
.ref-reward-sub{font-size:.72rem;color:rgba(255,255,255,.5);margin-top:2px;line-height:1.4;}
.ref-code-box{background:rgba(0,0,0,.3);border:1px solid rgba(255,255,255,.1);border-radius:12px;padding:14px;margin-bottom:12px;}
.ref-code-lbl{font-size:.65rem;color:rgba(255,255,255,.35);letter-spacing:2px;text-transform:uppercase;margin-bottom:6px;}
.ref-code-val{font-family:"Playfair Display",serif;font-size:1.8rem;font-weight:700;color:var(--gold2);letter-spacing:3px;}
.ref-url{font-size:.68rem;color:rgba(255,255,255,.3);margin-top:4px;word-break:break-all;}
.ref-btns{display:flex;gap:8px;flex-wrap:wrap;}
.ref-btn{flex:1;min-width:80px;padding:9px 12px;border:none;border-radius:9px;cursor:pointer;font-size:.74rem;font-weight:700;display:flex;align-items:center;justify-content:center;gap:5px;font-family:"DM Sans",sans-serif;}
.rb-copy{background:rgba(255,255,255,.12);color:#fff;border:1px solid rgba(255,255,255,.18);}
.rb-wa{background:#25d366;color:#fff;}
.rb-tg{background:#0088cc;color:#fff;}
.recruit-item{display:flex;align-items:center;gap:12px;padding:11px 0;border-bottom:1px solid var(--bdr);}
.recruit-item:last-child{border-bottom:none;}
.recruit-av{width:38px;height:38px;border-radius:50%;background:linear-gradient(135deg,#071c10,#0d6e3a);display:flex;align-items:center;justify-content:center;color:var(--gold2);font-weight:700;font-size:.84rem;flex-shrink:0;}
.rb-paid{background:var(--green2);color:var(--green);padding:3px 8px;border-radius:5px;font-size:.64rem;font-weight:800;}
.rb-pending{background:var(--amber2);color:var(--amber);padding:3px 8px;border-radius:5px;font-size:.64rem;font-weight:800;}

/* WALLET */
.wallet-hero{background:linear-gradient(135deg,#040c08,#071c10,#0d6e3a);border-radius:20px;padding:26px 20px;text-align:center;margin-bottom:14px;position:relative;overflow:hidden;box-shadow:0 16px 50px rgba(0,0,0,.32);}
.wallet-lbl{font-size:.68rem;color:rgba(255,255,255,.35);letter-spacing:3px;text-transform:uppercase;}
.wallet-amt{font-family:"Playfair Display",serif;font-size:3rem;font-weight:700;color:#fff;margin:6px 0 2px;}
.wallet-amt span{color:var(--gold2);}
.wallet-sub{font-size:.74rem;color:rgba(255,255,255,.4);}
.txn-item{display:flex;align-items:center;gap:12px;padding:11px 0;border-bottom:1px solid var(--bdr);}
.txn-item:last-child{border-bottom:none;}
.txn-ico{width:38px;height:38px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1rem;flex-shrink:0;}
.txn-ico-in{background:var(--green2);}
.txn-ico-out{background:var(--red2);}
.txn-desc{flex:1;font-size:.84rem;font-weight:600;color:var(--txt2);}
.txn-status{font-size:.66rem;font-weight:800;text-transform:uppercase;margin-top:1px;}
.txn-in{color:var(--green);font-weight:800;}
.txn-out{color:var(--red);font-weight:800;}

/* UPI SHEET */
.upi-sheet{position:fixed;inset:0;z-index:800;background:rgba(0,0,0,.65);backdrop-filter:blur(6px);display:flex;align-items:flex-end;justify-content:center;opacity:0;visibility:hidden;transition:.3s;}
.upi-sheet.on{opacity:1;visibility:visible;}
.upi-content{background:var(--surf);border-radius:24px 24px 0 0;padding:24px 20px 40px;width:100%;max-width:480px;transform:translateY(100%);transition:.35s var(--spring);max-height:92vh;overflow-y:auto;}
.upi-sheet.on .upi-content{transform:translateY(0);}
.upi-handle{width:40px;height:4px;background:var(--bdr2);border-radius:2px;margin:0 auto 20px;}
.upi-amount{background:linear-gradient(135deg,#071c10,#0d6e3a);border-radius:14px;padding:16px;text-align:center;margin-bottom:16px;}
.upi-step{background:var(--bg);border:1.5px solid var(--bdr);border-radius:12px;padding:14px;margin-bottom:12px;display:flex;align-items:flex-start;gap:12px;}
.upi-step-num{width:26px;height:26px;background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:.72rem;font-weight:800;flex-shrink:0;margin-top:1px;}
.upi-id-box{background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);padding:10px 16px;border-radius:10px;font-size:1rem;font-weight:800;letter-spacing:.5px;margin:8px 0;text-align:center;}
.upi-app-btn{width:100%;padding:13px;border:none;border-radius:12px;cursor:pointer;font-size:.9rem;font-weight:700;margin-bottom:10px;display:flex;align-items:center;justify-content:center;gap:8px;font-family:"DM Sans",sans-serif;transition:.2s;}
.upi-app-btn:hover{transform:translateY(-1px);}
.upi-gpay{background:linear-gradient(135deg,#4285f4,#1967d2);color:#fff;}
.upi-phone{background:linear-gradient(135deg,#7b2ff7,#5e12d4);color:#fff;}
.upi-bhim{background:linear-gradient(135deg,#0052a3,#003880);color:#fff;}
.upi-other{background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);}
.upi-inp{width:100%;padding:12px 14px;border:2px solid var(--bdr);border-radius:10px;font-size:.9rem;outline:none;background:var(--bg);color:var(--txt);font-family:"DM Sans",sans-serif;}
.upi-inp:focus{border-color:var(--g3);}

/* SUCCESS MODAL */
.modal-bg{position:fixed;inset:0;z-index:900;background:rgba(0,0,0,.7);backdrop-filter:blur(8px);display:flex;align-items:center;justify-content:center;padding:20px;opacity:0;visibility:hidden;transition:.3s;}
.modal-bg.on{opacity:1;visibility:visible;}
.modal-box{background:var(--surf);border-radius:24px;padding:32px 24px;width:100%;max-width:360px;text-align:center;box-shadow:var(--e4);animation:modal-in .4s var(--spring) both;}
@keyframes modal-in{from{opacity:0;transform:scale(.85);}to{opacity:1;transform:none;}}
.modal-ico{font-size:3.5rem;margin-bottom:12px;}

/* PROFILE */
.profile-avatar{width:72px;height:72px;border-radius:50%;background:linear-gradient(135deg,#071c10,#0d6e3a);display:flex;align-items:center;justify-content:center;font-size:2rem;margin:0 auto 12px;border:3px solid var(--gold2);}
.profile-badge{display:inline-block;padding:4px 12px;border-radius:20px;font-size:.72rem;font-weight:700;margin-top:4px;}
.badge-paid{background:var(--green2);color:var(--green);}
.badge-free{background:var(--amber2);color:var(--amber);}

/* ADMIN */
#adminApp{display:none;min-height:100vh;background:#f0faf5;}
.admin-hd{background:linear-gradient(135deg,#071c10,#0d6e3a);padding:15px 20px;display:flex;align-items:center;justify-content:space-between;box-shadow:0 3px 20px rgba(0,0,0,.4);}
.admin-hd h1{color:#fff;font-size:1.05rem;font-weight:800;display:flex;align-items:center;gap:8px;}
.admin-hd h1 span{color:var(--gold2);}
.admin-tabs{background:#fff;border-bottom:2px solid #b0d8c0;display:flex;overflow-x:auto;padding:0 8px;}
.admin-tab{padding:12px 16px;border:none;background:transparent;font-size:.8rem;font-weight:700;cursor:pointer;color:#5a9070;border-bottom:3px solid transparent;margin-bottom:-2px;white-space:nowrap;transition:.2s;}
.admin-tab.on{color:#071c10;border-bottom-color:#f0b429;}
.admin-content{max-width:1100px;margin:0 auto;padding:16px;}
.admin-stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:10px;margin-bottom:16px;}
.admin-stat{background:linear-gradient(135deg,#fff,#edf8f2);border:1.5px solid #b0d8c0;border-radius:14px;padding:16px;text-align:center;position:relative;overflow:hidden;}
.admin-stat::before{content:"";position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,#0d9e4a,#f0b429);}
.admin-stat-v{font-family:"Playfair Display",serif;font-size:2rem;font-weight:700;color:#071c10;line-height:1;}
.admin-stat-l{font-size:.62rem;color:#5a9070;margin-top:4px;text-transform:uppercase;letter-spacing:.5px;font-weight:600;}
.admin-section{background:#fff;border:1.5px solid #b0d8c0;border-radius:16px;overflow:hidden;margin-bottom:16px;box-shadow:0 2px 12px rgba(7,28,16,.07);}
.admin-section-hd{padding:14px 18px;border-bottom:1.5px solid #daf0e4;display:flex;align-items:center;justify-content:space-between;background:linear-gradient(135deg,#fff,#f0faf5);}
.admin-section-hd h3{font-size:.9rem;font-weight:800;color:#071c10;}
.tbl{width:100%;border-collapse:collapse;}
.tbl th{background:linear-gradient(135deg,#d8f0e4,#c0e8d0);padding:10px 14px;font-size:.68rem;font-weight:800;color:#1a6a3a;text-align:left;text-transform:uppercase;letter-spacing:.6px;}
.tbl td{padding:11px 14px;border-bottom:1px solid #daf0e4;font-size:.82rem;color:#1a3828;vertical-align:middle;}
.tbl tr:last-child td{border-bottom:none;}
.tbl tr:hover td{background:#f0faf5;}
.pill{display:inline-block;padding:3px 9px;border-radius:20px;font-size:.64rem;font-weight:800;text-transform:uppercase;}
.pill-p{background:#fff0cc;color:#b86a00;border:1px solid #f0b429;}
.pill-a{background:#c8f0dc;color:#0d7a3a;border:1px solid #5ac888;}
.pill-r{background:#ffe0e0;color:#b01818;border:1px solid #f09090;}
.btn-a{background:linear-gradient(135deg,#0d6e3a,#14a050);color:#fff;border:none;padding:5px 12px;border-radius:7px;cursor:pointer;font-size:.74rem;font-weight:700;}
.btn-r{background:linear-gradient(135deg,#8a1010,#c82020);color:#fff;border:none;padding:5px 12px;border-radius:7px;cursor:pointer;font-size:.74rem;font-weight:700;}
.refresh-btn{background:linear-gradient(135deg,#071c10,#0d6e3a);color:var(--gold2);border:none;padding:8px 14px;border-radius:9px;cursor:pointer;font-size:.76rem;font-weight:800;transition:.2s;}
.refresh-btn:hover{transform:translateY(-1px);}
@media(max-width:600px){.tbl{font-size:.74rem;}.tbl td,.tbl th{padding:8px 10px;}}

/* MISC */
#toast{position:fixed;bottom:82px;left:50%;transform:translateX(-50%) translateY(14px);background:linear-gradient(135deg,#071c10,#0d6e3a);color:#fff;padding:10px 22px;border-radius:22px;font-size:.8rem;font-weight:600;z-index:9999;opacity:0;transition:.3s;pointer-events:none;white-space:nowrap;max-width:90vw;box-shadow:0 8px 30px rgba(0,0,0,.3);}
#toast.on{opacity:1;transform:translateX(-50%) translateY(0);}
.km-cycle-card{background:var(--surf);border:1.5px solid var(--bdr);border-radius:16px;padding:16px;margin-bottom:12px;position:relative;overflow:hidden;}
.km-cycle-card::before{content:"";position:absolute;top:0;left:0;right:0;height:3px;background:linear-gradient(90deg,#f0b429,var(--g4));border-radius:3px 3px 0 0;}
.km-nums{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:10px;}
.km-num{background:var(--bg2);border-radius:10px;padding:10px 8px;text-align:center;}
.km-num-v{font-family:"Playfair Display",serif;font-size:1.4rem;font-weight:700;color:var(--txt);line-height:1;}
.km-num-l{font-size:.56rem;color:var(--txt4);margin-top:2px;}
/* Admin free ref link section */
.admin-free-ref{background:linear-gradient(135deg,#071c10,#0d6e3a);border-radius:14px;padding:16px;margin-bottom:14px;color:#fff;}
.admin-free-ref h4{font-size:.9rem;color:var(--gold2);margin-bottom:8px;}
.admin-ref-code{font-family:"Playfair Display",serif;font-size:1.6rem;color:var(--gold2);letter-spacing:3px;font-weight:700;}
.admin-inp{width:100%;padding:10px 12px;border:1.5px solid #b0d8c0;border-radius:9px;font-size:.86rem;outline:none;background:#fff;color:#071c10;font-family:"DM Sans",sans-serif;margin-bottom:8px;}
</style>
</head>
<body>

<!-- SPLASH -->
<div id="splash">
  <div class="sp-logo">🌿</div>
  <div class="sp-title">Health is <span>Wealth</span></div>
  <div class="sp-sub">Walk · Earn · Win</div>
  <div class="sp-bar"><div class="sp-fill"></div></div>
</div>

<!-- AUTH SCREEN -->
<div id="authScreen">
  <div class="auth-box">
    <div class="auth-logo">
      <div class="auth-ico">🌿</div>
      <h1>Health is <span>Wealth</span></h1>
      <p>Walk · Earn · Win</p>
    </div>
    <div class="auth-tabs">
      <button class="auth-tab on" id="tabLogin" onclick="switchTab('login')">Sign In</button>
      <button class="auth-tab" id="tabSignup" onclick="switchTab('signup')">Register</button>
    </div>
    <!-- Login Form -->
    <div id="formLogin">
      <input class="ai" type="email" id="loginEmail" placeholder="Email address" autocomplete="email"/>
      <div class="pw-row">
        <input class="ai" type="password" id="loginPass" placeholder="Password" autocomplete="current-password"/>
        <button type="button" class="pw-eye" onclick="togglePw('loginPass',this)">👁</button>
      </div>
      <div class="auth-forgot"><button onclick="forgotPw()">Forgot password?</button></div>
      <button class="btn-gold" id="btnLogin" onclick="doLogin()">Sign In →</button>
      <div class="auth-err" id="loginErr"></div>
    </div>
    <!-- Signup Form -->
    <div id="formSignup" style="display:none">
      <input class="ai" type="text" id="signupName" placeholder="Full name" autocomplete="name"/>
      <input class="ai" type="email" id="signupEmail" placeholder="Email address" autocomplete="email"/>
      <div class="pw-row">
        <input class="ai" type="password" id="signupPass" placeholder="Password (min 8 chars)" autocomplete="new-password"/>
        <button type="button" class="pw-eye" onclick="togglePw('signupPass',this)">👁</button>
      </div>
      <input class="ai" type="text" id="signupRef" placeholder="Referral code (optional)"/>
      <button class="btn-gold" id="btnSignup" onclick="doSignup()">Create Account →</button>
      <div class="auth-err" id="signupErr"></div>
      <div class="auth-ok" id="signupOk"></div>
    </div>
    <div class="auth-admin-link">
      <button onclick="goAdmin()">⚙ Admin Panel</button>
    </div>
  </div>
</div>

<!-- USER APP -->
<div id="userApp">
  <div class="topbar">
    <div class="tb-brand" onclick="goPage('home')">
      <span class="tb-ico">🌿</span>
      <span class="tb-name">Health is <span>Wealth</span></span>
    </div>
    <div class="tb-right">
      <button class="tb-pill" id="tbWallet" onclick="goPage('wallet')">₹0</button>
      <button class="tb-btn" onclick="toggleTheme()">🌙</button>
      <button class="tb-logout" onclick="doLogout()">Exit</button>
    </div>
  </div>

  <!-- HOME PAGE -->
  <div id="pgHome" class="pg on">
    <div class="wrap">
      <div class="hero-banner">
        <div class="hero-greet">Good day,</div>
        <div class="hero-name" id="heroName">Welcome, <span>Friend</span> 👋</div>
        <div class="hero-stats">
          <div class="hero-stat"><div class="hero-stat-v" id="hKm">0.00</div><div class="hero-stat-l">KM Today</div></div>
          <div class="hero-stat"><div class="hero-stat-v" id="hEarned">₹0</div><div class="hero-stat-l">Total Earned</div></div>
          <div class="hero-stat"><div class="hero-stat-v" id="hRefs">0</div><div class="hero-stat-l">Referrals</div></div>
        </div>
      </div>
      <div class="stat-grid">
        <div class="stat-card sc-walk">
          <div class="stat-v" id="statKm">0.00</div>
          <div class="stat-l">KM Walked</div>
          <div class="stat-sub" id="statKmSub">Cap: 300 KM</div>
          <div class="progress-bar"><div class="pb-fill pb-walk" id="pbKm" style="width:0%"></div></div>
        </div>
        <div class="stat-card sc-earn">
          <div class="stat-v" id="statWallet">₹0</div>
          <div class="stat-l">Wallet Balance</div>
          <div class="stat-sub">₹1 per KM</div>
          <div class="progress-bar"><div class="pb-fill pb-earn" id="pbWallet" style="width:0%"></div></div>
        </div>
        <div class="stat-card sc-ref">
          <div class="stat-v" id="statRefs">0</div>
          <div class="stat-l">Paid Referrals</div>
          <div class="stat-sub">₹300 each</div>
        </div>
        <div class="stat-card sc-wallet">
          <div class="stat-v" id="statTotalEarn">₹0</div>
          <div class="stat-l">Total Earned</div>
          <div class="stat-sub">Walk + Referrals</div>
        </div>
      </div>

      <div class="sect-hd">🏆 Prize Structure</div>
      <div class="prize-banner">
        <div class="prize-title">🎁 All-Time Referral Champions</div>
        <div class="prize-sub">Top referrers win massive cash prizes — no time limit, ever!</div>
        <div class="prize-list">
          <div class="prize-item">
            <div class="prize-rank pr-1">🥇</div>
            <div class="prize-info">
              <div class="prize-name">Top Referrer</div>
              <div class="prize-who">Most paid referrals overall</div>
            </div>
            <div><div class="prize-amt">₹1 Cr</div><div style="font-size:.62rem;color:rgba(255,255,255,.4)">1,00,00,000</div></div>
          </div>
          <div class="prize-item">
            <div class="prize-rank pr-2">🥈</div>
            <div class="prize-info">
              <div class="prize-name">2nd Place</div>
              <div class="prize-who">2nd most paid referrals</div>
            </div>
            <div><div class="prize-amt">₹75 L</div><div style="font-size:.62rem;color:rgba(255,255,255,.4)">75,00,000</div></div>
          </div>
          <div class="prize-item">
            <div class="prize-rank pr-3">🥉</div>
            <div class="prize-info">
              <div class="prize-name">3rd Place</div>
              <div class="prize-who">3rd most paid referrals</div>
            </div>
            <div><div class="prize-amt">₹50 L</div><div style="font-size:.62rem;color:rgba(255,255,255,.4)">50,00,000</div></div>
          </div>
        </div>
      </div>

      <div class="sect-hd">📋 How It Works</div>
      <div class="card">
        <div class="how-step">
          <div class="how-num">1</div>
          <div class="how-body">
            <div class="how-title">Pay ₹1,999 to Unlock</div>
            <div class="how-desc">One-time payment unlocks GPS Walk Earn + Referral Rewards forever.</div>
            <div class="how-badge">₹1,999 one-time</div>
          </div>
        </div>
        <div class="how-step">
          <div class="how-num">2</div>
          <div class="how-body">
            <div class="how-title">Walk & Earn ₹1 per KM</div>
            <div class="how-desc">GPS tracks your outdoor walk. Earn ₹1 for every kilometre walked. Cap: 300 KM per cycle.</div>
            <div class="how-badge">₹1/KM · 300 KM cap</div>
          </div>
        </div>
        <div class="how-step">
          <div class="how-num">3</div>
          <div class="how-body">
            <div class="how-title">Refer Friends & Earn ₹300</div>
            <div class="how-desc">Share your referral code. When your friend pays ₹1,999 and joins, you instantly get ₹300 cash + your 300 KM cap resets.</div>
            <div class="how-badge">₹300 + 300 KM cap reset</div>
          </div>
        </div>
        <div class="how-step">
          <div class="how-num">4</div>
          <div class="how-body">
            <div class="how-title">Win Big Cash Prizes!</div>
            <div class="how-desc">The more you refer, the higher you climb. Top 3 all-time referrers win ₹1 Crore, ₹75 Lakhs, and ₹50 Lakhs!</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- EARN PAGE -->
  <div id="pgEarn" class="pg">
    <div class="wrap">
      <div class="sect-hd">🛰️ GPS Walk & Earn</div>
      <div id="gpsLockedContent">
        <div class="gps-locked">
          <div style="font-size:2.5rem;margin-bottom:8px">🔒</div>
          <div style="font-weight:700;color:var(--txt2);margin-bottom:4px">GPS Earn Locked</div>
          <div style="font-size:.78rem;color:var(--txt4);margin-bottom:14px">Pay ₹1,999 once to unlock walking income.</div>
          <button class="btn btn-primary btn-full" onclick="openPayment()">💳 Unlock Now — ₹1,999</button>
        </div>
      </div>
      <div id="gpsPaidContent" style="display:none">
        <div class="gps-card">
          <div class="gps-status-row">
            <div class="gps-dot off" id="gpsDot"></div>
            <span class="gps-status-txt" id="gpsStatusTxt">Tap Start to begin tracking</span>
          </div>
          <div class="gps-metrics">
            <div class="gps-metric"><div class="gps-metric-v" id="gpsKm">0.00</div><div class="gps-metric-l">KM Today</div></div>
            <div class="gps-metric"><div class="gps-metric-v" id="gpsSpeed">0.0</div><div class="gps-metric-l">km/h</div></div>
            <div class="gps-metric"><div class="gps-metric-v" id="gpsEarned">₹0</div><div class="gps-metric-l">Session Earn</div></div>
          </div>
          <div class="gps-prog-wrap"><div class="gps-prog-fill" id="gpsProgFill" style="width:0%"></div></div>
          <div class="gps-cap-txt" id="gpsCapTxt">Cap: 300 KM · Walked: 0 KM</div>
          <div class="gps-btns">
            <button class="gps-btn gps-start" id="gpsBtnStart" onclick="startGPS()">📍 Start Walk</button>
            <button class="gps-btn gps-pause" id="gpsBtnPause" onclick="pauseGPS()" style="display:none">⏸ Pause</button>
            <button class="gps-btn gps-stop" id="gpsBtnStop" onclick="stopGPS()" style="display:none">⏹ Stop</button>
          </div>
        </div>
        <div class="km-cycle-card">
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px">
            <div style="font-size:.7rem;color:var(--txt4);text-transform:uppercase;letter-spacing:1px">Walking Cycle</div>
          </div>
          <div class="km-nums">
            <div class="km-num"><div class="km-num-v" id="cycleWalked">0</div><div class="km-num-l">KM Walked</div></div>
            <div class="km-num"><div class="km-num-v" id="cycleRemain">300</div><div class="km-num-l">KM Left</div></div>
            <div class="km-num"><div class="km-num-v" id="cycleEarned">₹0</div><div class="km-num-l">Earned</div></div>
          </div>
          <div class="progress-bar"><div class="pb-fill pb-walk" id="cycleProg" style="width:0%"></div></div>
          <div style="font-size:.68rem;color:var(--txt4);margin-top:6px;text-align:center" id="cycleCapLbl">Cap: 300 KM — refer friends to reset!</div>
        </div>
      </div>

      <div class="sect-hd">👥 Refer & Earn ₹300</div>
      <div id="refLockedContent">
        <div class="gps-locked">
          <div style="font-size:2rem;margin-bottom:8px">🔒</div>
          <div style="font-weight:700;color:var(--txt2);margin-bottom:4px">Referral Locked</div>
          <div style="font-size:.78rem;color:var(--txt4)">Pay ₹1,999 to unlock referral rewards.</div>
        </div>
      </div>
      <div id="refPaidContent" style="display:none">
        <div class="ref-box">
          <div class="ref-reward">
            <div class="ref-reward-amt">₹300</div>
            <div class="ref-reward-info">
              <div class="ref-reward-title">Per Paid Referral</div>
              <div class="ref-reward-sub">You also get +300 KM cap reset every time a referral pays ₹1,999!</div>
            </div>
          </div>
          <div class="ref-code-box">
            <div class="ref-code-lbl">Your Referral Code</div>
            <div class="ref-code-val" id="refCode">——</div>
            <div class="ref-url" id="refUrl"></div>
          </div>
          <div class="ref-btns">
            <button class="ref-btn rb-copy" onclick="copyRef()">📋 Copy</button>
            <button class="ref-btn rb-wa" onclick="shareWA()">💬 WhatsApp</button>
            <button class="ref-btn rb-tg" onclick="shareTG()">✈️ Telegram</button>
          </div>
        </div>
        <div class="card">
          <div class="card-hd">Your Referrals</div>
          <div id="recruitList"><div class="empty"><div class="empty-ico">👥</div><div class="empty-txt">No paid referrals yet. Share your code!</div></div></div>
        </div>
      </div>
    </div>
  </div>

  <!-- WALLET PAGE -->
  <div id="pgWallet" class="pg">
    <div class="wrap">
      <div class="wallet-hero">
        <div class="wallet-lbl">Available Balance</div>
        <div class="wallet-amt"><span>₹</span><span id="walletAmt">0</span></div>
        <div class="wallet-sub" id="walletSub">Walk more to earn more</div>
      </div>
      <div id="payStatus"></div>

      <div class="card" id="wdFormCard">
        <div class="card-hd">💸 Request Withdrawal</div>

        <!-- PENDING BLOCK: shown when user has pending withdrawal -->
        <div id="wdPendingBlock" style="display:none">
          <div style="background:var(--amber2);border:1.5px solid rgba(184,106,0,.25);border-radius:12px;padding:14px;margin-bottom:4px">
            <div style="font-size:.88rem;font-weight:700;color:var(--amber);margin-bottom:4px">⏳ Withdrawal Pending</div>
            <div style="font-size:.76rem;color:var(--txt3);margin-bottom:10px;line-height:1.5" id="wdPendingInfo">Your withdrawal request is under admin review. You cannot submit another until this is resolved or cancelled.</div>
            <button class="btn btn-red btn-full" onclick="cancelWithdrawal()" id="btnCancelWD">❌ Cancel This Request</button>
          </div>
        </div>

        <!-- NEW WITHDRAWAL FORM -->
        <div id="wdNewBlock">
          <div style="font-size:.78rem;color:var(--txt4);margin-bottom:12px;line-height:1.5">⚠️ Min ₹50. One pending request at a time. Processed in 1–3 working days. Balance is deducted immediately upon request.</div>
          <input class="inp" type="number" id="withdrawAmt" placeholder="Amount in ₹ (min ₹50)" min="50"/>

          <div style="font-size:.76rem;font-weight:700;color:var(--txt2);margin-bottom:6px">Choose payment method:</div>
          <div style="display:flex;gap:6px;margin-bottom:12px">
            <button class="wd-method-btn on" id="wdTabUpi" onclick="switchWDMethod('upi')">📱 UPI</button>
            <button class="wd-method-btn" id="wdTabBank" onclick="switchWDMethod('bank')">🏦 Bank Account</button>
          </div>

          <!-- UPI FIELDS -->
          <div id="wdUpiFields">
            <input class="inp" type="text" id="withdrawUPI" placeholder="UPI ID / GPay / PhonePe (e.g. 9876543210@ybl)"/>
            <div style="font-size:.68rem;color:var(--txt4);margin-top:-6px;margin-bottom:10px">Works with GPay, PhonePe, Paytm, BHIM, or any UPI ID</div>
          </div>

          <!-- BANK FIELDS -->
          <div id="wdBankFields" style="display:none">
            <input class="inp" type="text" id="withdrawAccName" placeholder="Account Holder Full Name"/>
            <input class="inp" type="text" id="withdrawAccNo" placeholder="Account Number"/>
            <input class="inp" type="text" id="withdrawAccNo2" placeholder="Re-enter Account Number (confirm)"/>
            <input class="inp" type="text" id="withdrawIFSC" placeholder="IFSC Code (e.g. SBIN0001234)" oninput="this.value=this.value.toUpperCase()"/>
            <input class="inp" type="text" id="withdrawBankName" placeholder="Bank Name (e.g. SBI, HDFC, ICICI)"/>
          </div>

          <button class="btn btn-primary btn-full" onclick="requestWithdraw()" id="btnRequestWD">💸 Submit Withdrawal Request</button>
        </div>
      </div>

      <div class="card">
        <div class="card-hd">Transaction History</div>
        <div id="txnList"><div class="empty"><div class="empty-ico">📭</div><div class="empty-txt">No transactions yet</div></div></div>
      </div>
    </div>
  </div>


  <!-- PROFILE PAGE -->
  <div id="pgProfile" class="pg">
    <div class="wrap">
      <div class="card" style="text-align:center;padding:24px">
        <div class="profile-avatar" id="profileAv">👤</div>
        <div style="font-family:'Playfair Display',serif;font-size:1.2rem;font-weight:700;color:var(--txt)" id="profileName">—</div>
        <div style="font-size:.78rem;color:var(--txt4);margin-top:2px" id="profileEmail">—</div>
        <div id="profileBadge" style="margin-top:8px"></div>
      </div>
      <div class="card">
        <div class="card-hd">Account Details</div>
        <div style="display:flex;justify-content:space-between;padding:8px 0;border-bottom:1px solid var(--bdr);font-size:.84rem">
          <span style="color:var(--txt4)">Referral Code</span>
          <strong id="profRefCode" style="color:var(--txt2);font-family:monospace">—</strong>
        </div>
        <div style="display:flex;justify-content:space-between;padding:8px 0;border-bottom:1px solid var(--bdr);font-size:.84rem">
          <span style="color:var(--txt4)">Total KM Walked</span>
          <strong id="profTotalKm" style="color:var(--txt2)">0 km</strong>
        </div>
        <div style="display:flex;justify-content:space-between;padding:8px 0;border-bottom:1px solid var(--bdr);font-size:.84rem">
          <span style="color:var(--txt4)">Paid Referrals</span>
          <strong id="profRefs" style="color:var(--txt2)">0</strong>
        </div>
        <div style="display:flex;justify-content:space-between;padding:8px 0;font-size:.84rem">
          <span style="color:var(--txt4)">KM Cap Remaining</span>
          <strong id="profCapLeft" style="color:var(--green)">300 km</strong>
        </div>
      </div>
      <div class="card">
        <div class="card-hd">Payment</div>
        <div id="profilePayStatus" style="margin-bottom:12px"></div>
        <button class="btn btn-primary btn-full" onclick="openPayment()" id="profilePayBtn" style="display:none">💳 Pay ₹1,999 to Unlock Earning</button>
      </div>
      <button class="btn btn-red btn-full" onclick="doLogout()" style="margin-top:4px">Sign Out</button>
    </div>
  </div>
</div>

<!-- BOTTOM NAV -->
<div class="bnav" id="bnav">
  <button class="bn-item on" onclick="goPage('home')" id="bn-home"><span class="bn-ico">🏠</span><span class="bn-lbl">Home</span></button>
  <button class="bn-item" onclick="goPage('earn')" id="bn-earn"><span class="bn-ico">🏃</span><span class="bn-lbl">Earn</span></button>
  <button class="bn-item" onclick="goPage('wallet')" id="bn-wallet"><span class="bn-ico">💳</span><span class="bn-lbl">Wallet</span></button>
  <button class="bn-item" onclick="goPage('profile')" id="bn-profile"><span class="bn-ico">👤</span><span class="bn-lbl">Profile</span></button>
</div>

<!-- UPI PAYMENT SHEET -->
<div class="upi-sheet" id="upiSheet" onclick="closePaymentBg(event)">
  <div class="upi-content">
    <div class="upi-handle"></div>
    <h3 style="font-family:'Playfair Display',serif;font-size:1.3rem;font-weight:700;color:var(--txt);margin-bottom:16px">💳 Unlock Walking Income</h3>
    <div class="upi-amount">
      <div style="font-size:.66rem;color:rgba(255,255,255,.4);letter-spacing:2px;text-transform:uppercase">One-Time Payment</div>
      <div style="font-family:'Playfair Display',serif;font-size:2.2rem;font-weight:700;color:var(--gold2)">₹1,999</div>
      <div style="font-size:.7rem;color:rgba(255,255,255,.35)">Unlocks GPS Earn + Referral Rewards forever</div>
    </div>
    <div class="upi-step">
      <div class="upi-step-num">1</div>
      <div style="flex:1">
        <div style="font-size:.84rem;font-weight:700;color:var(--txt2);margin-bottom:6px">Pay ₹1,999 to this UPI ID</div>
        <div class="upi-id-box">bonagirispinoj-1@oksbi</div>
        <button class="btn btn-outline" style="font-size:.72rem;padding:5px 10px;margin-top:4px" onclick="copyUPI()">📋 Copy UPI ID</button>
      </div>
    </div>
    <div class="upi-step">
      <div class="upi-step-num">2</div>
      <div style="font-size:.84rem;color:var(--txt3)">Or pay directly via your UPI app:</div>
    </div>
    <button class="upi-app-btn upi-gpay" onclick="openUPI('gpay')">🔵 Google Pay</button>
    <button class="upi-app-btn upi-phone" onclick="openUPI('phonepe')">💜 PhonePe</button>
    <button class="upi-app-btn upi-bhim" onclick="openUPI('bhim')">🔷 BHIM UPI</button>
    <button class="upi-app-btn upi-other" onclick="openUPI('other')">📲 Any UPI App</button>
    <div class="upi-step" style="margin-top:12px">
      <div class="upi-step-num">3</div>
      <div style="width:100%">
        <div style="font-size:.84rem;font-weight:700;color:var(--txt2);margin-bottom:4px">Enter Transaction / UTR Number</div>
        <div style="font-size:.72rem;color:var(--txt4);margin-bottom:8px">After paying, find the 12-digit UTR/Transaction ID in your UPI app and enter below.</div>
        <input class="upi-inp" id="txnId" placeholder="Enter 12-digit UTR / Transaction ID" maxlength="30"/>
        <div id="txnErr" style="color:var(--red);font-size:.74rem;margin-top:4px;display:none"></div>
      </div>
    </div>
    <button class="btn btn-primary btn-full" style="margin-top:8px" id="submitPayBtn" onclick="submitPayment()">✅ Submit Payment for Verification</button>
    <button class="btn btn-outline btn-full" style="margin-top:8px" onclick="closePayment()">Cancel</button>
  </div>
</div>

<!-- SUCCESS MODAL -->
<div class="modal-bg" id="successModal">
  <div class="modal-box">
    <div class="modal-ico" id="modalIco">✅</div>
    <h3 style="font-size:1.3rem;margin-bottom:8px" id="modalTitle">Done!</h3>
    <p style="font-size:.84rem;color:var(--txt3);line-height:1.6;margin-bottom:20px;white-space:pre-wrap" id="modalMsg">Your request was submitted.</p>
    <button class="btn btn-primary btn-full" onclick="closeModal()">Continue</button>
  </div>
</div>

<!-- TOAST -->
<div id="toast"></div>

<!-- ADMIN PANEL -->
<div id="adminApp">
  <div class="admin-hd">
    <h1>🌿 Health is <span>Wealth</span> — Admin</h1>
    <div style="display:flex;gap:8px;align-items:center">
      <span style="background:rgba(240,180,41,.2);border:1px solid rgba(240,180,41,.3);color:var(--gold2);padding:4px 10px;border-radius:12px;font-size:.7rem;font-weight:700" id="adminBadge">Admin</span>
      <button onclick="adminLogout()" style="background:none;border:1px solid rgba(255,255,255,.2);color:rgba(255,255,255,.6);padding:5px 10px;border-radius:7px;cursor:pointer;font-size:.72rem;font-family:'DM Sans',sans-serif">Sign Out</button>
    </div>
  </div>

  <!-- ADMIN LOGIN -->
  <div id="adminLoginBox" style="min-height:85vh;display:flex;align-items:center;justify-content:center;background:linear-gradient(160deg,#040e08,#071c10,#0a3018);padding:20px">
    <div style="background:rgba(255,255,255,.07);border:1px solid rgba(255,255,255,.14);border-radius:24px;padding:34px 26px;width:100%;max-width:370px;box-shadow:0 32px 80px rgba(0,0,0,.6);backdrop-filter:blur(20px)">
      <div style="text-align:center;margin-bottom:24px">
        <div style="font-size:3rem;filter:drop-shadow(0 0 16px rgba(26,179,88,.5));margin-bottom:10px">🌿</div>
        <h2 style="font-family:'Playfair Display',serif;font-size:1.5rem;font-weight:700;color:#fff;margin-bottom:4px">Admin Panel</h2>
        <p style="font-size:.7rem;color:rgba(255,255,255,.3);letter-spacing:2px;text-transform:uppercase">Health is Wealth Management</p>
      </div>
      <input style="width:100%;padding:13px 14px;border:1px solid rgba(255,255,255,.15);border-radius:11px;font-size:.9rem;margin-bottom:10px;outline:none;background:rgba(255,255,255,.08);color:#fff;font-family:'DM Sans',sans-serif" type="email" id="adminEmail" placeholder="Admin Email"/>
      <div style="position:relative;margin-bottom:14px">
        <input style="width:100%;padding:13px 44px 13px 14px;border:1px solid rgba(255,255,255,.15);border-radius:11px;font-size:.9rem;outline:none;background:rgba(255,255,255,.08);color:#fff;font-family:'DM Sans',sans-serif" type="password" id="adminPass" placeholder="Admin Password"/>
        <button type="button" onclick="togglePw('adminPass',this)" style="position:absolute;right:13px;top:50%;transform:translateY(-50%);background:none;border:none;color:rgba(255,255,255,.4);cursor:pointer;font-size:1rem">👁</button>
      </div>
      <button onclick="adminLogin()" id="adminLoginBtn" style="width:100%;padding:14px;border:none;border-radius:11px;background:linear-gradient(135deg,#8a6210,#f0b429,#fad369);color:#071c10;font-weight:800;font-size:.92rem;cursor:pointer;font-family:'DM Sans',sans-serif;box-shadow:0 6px 24px rgba(240,180,41,.45)">Sign In to Admin</button>
      <div style="color:#ff9090;font-size:.74rem;text-align:center;margin-top:8px;min-height:18px" id="adminErr"></div>
      <div style="margin-top:16px;text-align:center"><button onclick="showAuth()" style="background:none;border:none;color:rgba(255,255,255,.28);font-size:.76rem;cursor:pointer;font-family:'DM Sans',sans-serif">← Back to App</button></div>
    </div>
  </div>

  <!-- ADMIN DASHBOARD -->
  <div id="adminDash" style="display:none">
    <div class="admin-tabs">
      <button class="admin-tab on" onclick="adminTab(this,'aDash')">📊 Dashboard</button>
      <button class="admin-tab" onclick="adminTab(this,'aUsers')">👥 Users</button>
      <button class="admin-tab" onclick="adminTab(this,'aLeaderboard')">🏆 Leaderboard</button>
      <button class="admin-tab" onclick="adminTab(this,'aWD')">💸 Withdrawals</button>
      <button class="admin-tab" onclick="adminTab(this,'aPay')">💳 Payments</button>
      <button class="admin-tab" onclick="adminTab(this,'aSettings')">⚙️ Settings</button>
    </div>
    <div class="admin-content">

      <!-- DASHBOARD TAB -->
      <div id="aDash">
        <div class="admin-stats">
          <div class="admin-stat"><div class="admin-stat-v" id="sTotalUsers">—</div><div class="admin-stat-l">Total Users</div></div>
          <div class="admin-stat"><div class="admin-stat-v" id="sPaidUsers">—</div><div class="admin-stat-l">Paid Users</div></div>
          <div class="admin-stat"><div class="admin-stat-v" id="sPendingWD">—</div><div class="admin-stat-l">Pending WD</div></div>
          <div class="admin-stat"><div class="admin-stat-v" id="sPendingPay">—</div><div class="admin-stat-l">Pending Pay</div></div>
          <div class="admin-stat"><div class="admin-stat-v" id="sTotalKm">—</div><div class="admin-stat-l">Total KM</div></div>
          <div class="admin-stat"><div class="admin-stat-v" id="sTotalBal">—</div><div class="admin-stat-l">Total ₹ Owed</div></div>
        </div>

        <!-- Admin Free Referral Link -->
        <div class="admin-section">
          <div class="admin-section-hd"><h3>🔗 Admin Free Referral Link</h3><button class="refresh-btn" onclick="loadAdminStats()">🔄 Refresh</button></div>
          <div style="padding:16px">
            <div class="admin-free-ref">
              <h4>Your Special Admin Referral Code</h4>
              <div class="admin-ref-code" id="adminRefCodeDisplay">—</div>
              <div style="font-size:.72rem;color:rgba(255,255,255,.5);margin-top:4px" id="adminRefUrlDisplay"></div>
              <div style="display:flex;gap:8px;margin-top:12px;flex-wrap:wrap">
                <button class="btn btn-outline" style="font-size:.74rem;padding:6px 12px;background:rgba(255,255,255,.1);color:#fff;border-color:rgba(255,255,255,.2)" onclick="copyAdminRef()">📋 Copy Link</button>
                <button class="btn btn-outline" style="font-size:.74rem;padding:6px 12px;background:#25d366;color:#fff;border:none" onclick="shareAdminRefWA()">💬 Share on WhatsApp</button>
                <button class="btn btn-outline" style="font-size:.74rem;padding:6px 12px;background:rgba(255,255,255,.1);color:#fff;border-color:rgba(255,255,255,.2)" onclick="generateNewAdminRef()">🔄 Generate New Code</button>
              </div>
            </div>
            <div style="background:#fff0cc;border:1px solid #f0b429;border-radius:10px;padding:12px;font-size:.78rem;color:#8a6210">
              <strong>ℹ️ Admin Referral:</strong> When someone joins using your admin referral code, they are pre-approved without needing to pay ₹1,999. Use this to grant free access to trusted users.
            </div>
          </div>
        </div>

        <div class="admin-section">
          <div class="admin-section-hd"><h3>Admin Guide</h3></div>
          <div style="padding:16px;font-size:.84rem;color:#2a6040;line-height:2">
            <p>💳 Verify UPI payments → Payments tab (approve = unlock GPS Earn)</p>
            <p>💸 Approve/Reject withdrawal requests → Withdrawals tab</p>
            <p>👥 View & manage all users → Users tab</p>
            <p>🏆 Leaderboard & Prize standings → Leaderboard tab</p>
            <p>⚙️ Modify app settings → Settings tab</p>
            <p>📌 Admin email: <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="fe9c91909f99978c978d8e97909194be99939f9792d09d9193">[email&#160;protected]</a></p>
            <p>📱 WhatsApp: +91 9110563922</p>
          </div>
        </div>
        <div class="admin-section">
          <div class="admin-section-hd"><h3>🔥 Firestore Security Rules</h3></div>
          <div style="padding:14px;font-size:.78rem;color:#2a6040">
            Deploy these rules in Firebase Console → Firestore → Rules:<br>
            <pre style="background:#f0faf5;border-radius:8px;padding:10px;margin-top:8px;font-size:.7rem;overflow-x:auto;white-space:pre-wrap;color:#071c10">rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    function isAdmin() {
      return request.auth != null &&
             request.auth.token.email == '<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="74161b1a15131d061d07041d1a1b1e341319151d185a171b19">[email&#160;protected]</a>';
    }
    match /users/{uid} {
      allow read: if request.auth.uid == uid || isAdmin();
      allow write: if request.auth.uid == uid || isAdmin();
    }
    match /referrals/{id} { allow read, write: if request.auth != null; }
    match /withdrawals/{id} { allow read, write: if request.auth != null; }
    match /payments/{id} { allow read, write: if request.auth != null; }
    match /km_logs/{id} { allow read, write: if request.auth != null; }
    match /app_settings/{id} { allow read: if request.auth != null; allow write: if isAdmin(); }
    match /free_referrals/{id} { allow read, write: if request.auth != null; }
  }
}</pre>
          </div>
        </div>
      </div>

      <!-- USERS TAB -->
      <div id="aUsers" style="display:none">
        <div class="admin-section">
          <div class="admin-section-hd"><h3>👥 All Users</h3><button class="refresh-btn" onclick="loadAdminUsers()">🔄 Refresh</button></div>
          <!-- Grant Free Access -->
          <div style="padding:14px;border-bottom:1px solid #daf0e4;background:#f8fff8">
            <div style="font-size:.78rem;font-weight:700;color:#071c10;margin-bottom:8px">⚡ Grant Free Access to User (by email)</div>
            <div style="display:flex;gap:8px;flex-wrap:wrap">
              <input class="admin-inp" style="flex:1;min-width:200px;margin-bottom:0" type="email" id="grantEmail" placeholder="user@email.com"/>
              <button class="btn-a" onclick="grantFreeAccess()">✅ Grant Free Access</button>
            </div>
            <div style="font-size:.7rem;color:#5a9070;margin-top:6px">This will activate GPS Earn + Referral for the user without requiring payment.</div>
          </div>
          <div style="overflow-x:auto">
            <table class="tbl">
              <thead><tr><th>#</th><th>Name / Email</th><th>Ref Code</th><th>Balance</th><th>KM Walked</th><th>Paid</th><th>Refs</th><th>Joined</th><th>Actions</th></tr></thead>
              <tbody id="usersBody"><tr><td colspan="9" style="text-align:center;padding:24px;color:#5a9070">Click Refresh to load</td></tr></tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- LEADERBOARD TAB -->
      <div id="aLeaderboard" style="display:none">
        <div class="admin-section">
          <div class="admin-section-hd"><h3>🏆 Prize Leaderboard — All Time</h3><button class="refresh-btn" onclick="loadLeaderboard()">🔄 Refresh</button></div>
          <div style="padding:14px">
            <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:10px;margin-bottom:16px" id="prizePodium">
              <div style="background:linear-gradient(135deg,#FFD700,#FFA500);border-radius:14px;padding:16px;text-align:center;box-shadow:0 6px 20px rgba(255,180,0,.3)">
                <div style="font-size:2.2rem">🥇</div>
                <div style="font-weight:800;color:#fff;font-size:1.1rem;margin-top:4px">₹1 CRORE</div>
                <div id="prize1Name" style="font-size:.76rem;color:rgba(255,255,255,.9);margin-top:4px;font-weight:700">—</div>
                <div id="prize1refs" style="font-size:.68rem;color:rgba(255,255,255,.7);margin-top:2px">— referrals</div>
              </div>
              <div style="background:linear-gradient(135deg,#C0C0C0,#A0A0A0);border-radius:14px;padding:16px;text-align:center;box-shadow:0 6px 20px rgba(160,160,160,.3)">
                <div style="font-size:2.2rem">🥈</div>
                <div style="font-weight:800;color:#fff;font-size:1.1rem;margin-top:4px">₹75 LAKHS</div>
                <div id="prize2Name" style="font-size:.76rem;color:rgba(255,255,255,.9);margin-top:4px;font-weight:700">—</div>
                <div id="prize2refs" style="font-size:.68rem;color:rgba(255,255,255,.7);margin-top:2px">— referrals</div>
              </div>
              <div style="background:linear-gradient(135deg,#CD7F32,#A0520A);border-radius:14px;padding:16px;text-align:center;box-shadow:0 6px 20px rgba(160,90,10,.3)">
                <div style="font-size:2.2rem">🥉</div>
                <div style="font-weight:800;color:#fff;font-size:1.1rem;margin-top:4px">₹50 LAKHS</div>
                <div id="prize3Name" style="font-size:.76rem;color:rgba(255,255,255,.9);margin-top:4px;font-weight:700">—</div>
                <div id="prize3refs" style="font-size:.68rem;color:rgba(255,255,255,.7);margin-top:2px">— referrals</div>
              </div>
            </div>
          </div>
          <div style="overflow-x:auto">
            <table class="tbl">
              <thead><tr><th>Rank</th><th>Name</th><th>Referrals</th><th>KM Walked</th><th>Balance</th><th>Prize</th></tr></thead>
              <tbody id="lbBody"><tr><td colspan="6" style="text-align:center;padding:24px;color:#5a9070">Click Refresh to load</td></tr></tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- WITHDRAWALS TAB -->
      <div id="aWD" style="display:none">
        <div class="admin-section">
          <div class="admin-section-hd"><h3>💸 Withdrawal Requests</h3><button class="refresh-btn" onclick="loadAdminWD()">🔄 Refresh</button></div>
          <div style="padding:10px 16px;background:#fffbe6;border-bottom:1px solid #f0d060;font-size:.78rem;color:#7a5800">
            <strong>ℹ️ Workflow:</strong> Transfer money manually to user's UPI/Bank → Then click ✅ Approve. ❌ Reject refunds balance to user wallet automatically.
          </div>
          <div style="overflow-x:auto">
            <table class="tbl">
              <thead><tr><th>User</th><th>Amount</th><th>Payment Details</th><th>Status</th><th>Date</th><th>Action</th></tr></thead>
              <tbody id="wdBody"><tr><td colspan="6" style="text-align:center;padding:24px;color:#5a9070">Switch to this tab to load</td></tr></tbody>
            </table>
          </div>
        </div>
      </div>
      <!-- PAYMENTS TAB -->
      <div id="aPay" style="display:none">
        <div class="admin-section">
          <div class="admin-section-hd"><h3>💳 Payment Verifications</h3><button class="refresh-btn" onclick="loadAdminPay()">🔄 Refresh</button></div>
          <div style="overflow-x:auto">
            <table class="tbl">
              <thead><tr><th>User</th><th>Amount</th><th>UTR / Txn ID</th><th>Status</th><th>Date</th><th>Action</th></tr></thead>
              <tbody id="payBody"><tr><td colspan="6" style="text-align:center;padding:24px;color:#5a9070">Loading...</td></tr></tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- SETTINGS TAB -->
      <div id="aSettings" style="display:none">
        <div class="admin-section">
          <div class="admin-section-hd"><h3>⚙️ App Settings</h3><button class="refresh-btn" onclick="loadSettings()">🔄 Load</button></div>
          <div style="padding:16px">
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px">
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">Entry Price (₹)</label>
                <input class="admin-inp" type="number" id="setPrice" placeholder="1999"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">Referral Reward (₹)</label>
                <input class="admin-inp" type="number" id="setRefReward" placeholder="300"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">KM Rate (₹/km)</label>
                <input class="admin-inp" type="number" id="setKmRate" placeholder="1" step="0.1"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">KM Cap per Cycle</label>
                <input class="admin-inp" type="number" id="setKmCap" placeholder="300"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">1st Prize (₹)</label>
                <input class="admin-inp" type="number" id="setPrize1" placeholder="10000000"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">2nd Prize (₹)</label>
                <input class="admin-inp" type="number" id="setPrize2" placeholder="7500000"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">3rd Prize (₹)</label>
                <input class="admin-inp" type="number" id="setPrize3" placeholder="5000000"/>
              </div>
              <div>
                <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">Min Withdrawal (₹)</label>
                <input class="admin-inp" type="number" id="setMinWD" placeholder="50"/>
              </div>
            </div>
            <div style="margin-bottom:12px">
              <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">UPI ID</label>
              <input class="admin-inp" type="text" id="setUpiId" placeholder="bonagirispinoj-1@oksbi"/>
            </div>
            <div style="margin-bottom:12px">
              <label style="font-size:.76rem;font-weight:700;color:#2a6040;display:block;margin-bottom:4px">App Announcement (shown on home screen)</label>
              <textarea class="admin-inp" id="setAnnouncement" rows="3" placeholder="Optional announcement for all users..." style="resize:vertical"></textarea>
            </div>
            <button class="btn-a" style="width:100%;padding:13px;font-size:.88rem;border-radius:10px" onclick="saveSettings()">💾 Save All Settings</button>
          </div>
        </div>

        <!-- Broadcast Message -->
        <div class="admin-section">
          <div class="admin-section-hd"><h3>📢 Broadcast WhatsApp Message</h3></div>
          <div style="padding:16px">
            <div style="font-size:.78rem;color:#5a9070;margin-bottom:10px">Send a message to your WhatsApp number for record keeping / user communication.</div>
            <textarea class="admin-inp" id="broadcastMsg" rows="4" placeholder="Type your message here..." style="resize:vertical"></textarea>
            <button class="btn-a" style="margin-top:8px" onclick="sendBroadcast()">💬 Send to WhatsApp</button>
          </div>
        </div>

        <!-- Fake UTR Protection Info -->
        <div class="admin-section">
          <div class="admin-section-hd"><h3>🛡️ Anti-Fraud Protection</h3></div>
          <div style="padding:16px;font-size:.8rem;color:#2a6040;line-height:1.8">
            <p>✅ <strong>Fake UTR Protection:</strong> UTR/Transaction IDs are stored and checked for duplicates. Submitting the same UTR twice is blocked automatically.</p>
            <p>✅ <strong>Manual Verification:</strong> All payments require admin approval before GPS Earn activates. UTR is sent to admin WhatsApp (9110563922) for cross-verification.</p>
            <p>✅ <strong>Fake Referral Protection:</strong> Referral credit is only given AFTER admin approves the referred user's payment. Self-referral is blocked.</p>
            <p>✅ <strong>Rate Limiting:</strong> Max 5 login attempts then 15-minute lockout to prevent brute force.</p>
            <p>✅ <strong>Admin-Only Approval:</strong> No automatic activation. Every payment must be manually approved by admin.</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</div>

<script>
// ═══════════════════════════════════════════════
//  HEALTH IS WEALTH v3.0
//  Admin: bonagirispinoj@gmail.com
//  UPI: bonagirispinoj-1@oksbi
//  WhatsApp: 9110563922
// ═══════════════════════════════════════════════

const FB_CONFIG = {
  apiKey: "AIzaSyBIebPXVjzVBZl380ZCELg2LUSCLr-6QrE",
  authDomain: "neurocoin-ntc.firebaseapp.com",
  databaseURL: "https://neurocoin-ntc-default-rtdb.firebaseio.com",
  projectId: "neurocoin-ntc",
  storageBucket: "neurocoin-ntc.firebasestorage.app",
  messagingSenderId: "1040732555800",
  appId: "1:1040732555800:web:55e63b5ff81781c6b1916d",
  measurementId: "G-6X8T0YQTQ3"
};

const ADMIN_EMAIL = "bonagirispinoj@gmail.com";
const ADMIN_WA    = "919110563922";
let UPI_ID    = "bonagirispinoj-1@oksbi";
let BOOK_PRICE= 1999;
let REF_REWARD= 300;
let KM_RATE   = 1;
let KM_CAP    = 300;
let MIN_WD    = 50;
let PRIZE_1   = 10000000;
let PRIZE_2   = 7500000;
let PRIZE_3   = 5000000;

let db, auth, currentUser = null;
try {
  firebase.initializeApp(FB_CONFIG);
  auth = firebase.auth();
  db   = firebase.firestore();
  db.settings({ merge: true });
  db.enablePersistence({ synchronizeTabs: true }).catch(() => {});
} catch(e) { console.error("Firebase init:", e); }

let S = {
  wallet: 0, totalKm: 0, todayKm: 0, kmLimit: 300,
  referralCount: 0, bookPurchased: false, bookStatus: "none",
  refCode: "", pendingWDId: null
};

// Rate limiting
const AUTH_MAX = 5, AUTH_LOCK = 15 * 60 * 1000;
let authAtt;
try { authAtt = JSON.parse(sessionStorage.getItem("__aa") || '{"c":0,"t":0}'); }
catch(e) { authAtt = {c:0,t:0}; }
function checkRL() {
  if (authAtt.c >= AUTH_MAX) {
    const e = Date.now() - authAtt.t;
    if (e < AUTH_LOCK) return `Too many attempts. Wait ${Math.ceil((AUTH_LOCK-e)/60000)} min.`;
    authAtt = {c:0,t:0};
  }
  return null;
}
function recordAtt(ok) {
  if (ok) { authAtt = {c:0,t:0}; }
  else { authAtt.c++; authAtt.t = Date.now(); }
  try { sessionStorage.setItem("__aa", JSON.stringify(authAtt)); } catch(e) {}
}

// Helpers
function esc(s) {
  if (typeof s !== "string") return "";
  return s.replace(/[<>&'"]/g, c => ({"<":"&lt;",">":"&gt;","&":"&amp;","'":"&#39;",'"':'&quot;'}[c]));
}
function san(s) {
  if (typeof s !== "string") return "";
  return s.replace(/[<>'"&]/g,"").trim().slice(0,300);
}
function isEmail(e) { return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(e); }
function genRef(name, uid) {
  const n = name.replace(/[^a-zA-Z]/g,"").toUpperCase().slice(0,4).padEnd(4,"X");
  return n + uid.slice(-4).toUpperCase();
}
function formatLakh(n) {
  if (n >= 10000000) return "₹" + (n/10000000).toFixed(0) + " Cr";
  if (n >= 100000)   return "₹" + (n/100000).toFixed(0) + " L";
  return "₹" + n.toLocaleString("en-IN");
}

// ── Toast ────────────────────────────────────────────────────────────────────
let toastTimer = null;
function showToast(msg, dur) {
  dur = dur || 2800;
  const t = document.getElementById("toast");
  if (!t) return;
  t.textContent = msg;
  t.classList.add("on");
  if (toastTimer) clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove("on"), dur);
}

function authErr(code) {
  const map = {
    "auth/user-not-found":"No account found with this email.",
    "auth/wrong-password":"Incorrect password.",
    "auth/invalid-credential":"Invalid email or password.",
    "auth/email-already-in-use":"Email already registered.",
    "auth/weak-password":"Password must be at least 8 characters.",
    "auth/invalid-email":"Please enter a valid email address.",
    "auth/too-many-requests":"Too many attempts. Please wait.",
    "auth/network-request-failed":"No internet connection.",
    "auth/operation-not-allowed":"Email sign-in is not enabled."
  };
  return map[code] || "Something went wrong. Try again.";
}

// ── Screen management ─────────────────────────────────────────────────────────
function showAuth() {
  document.getElementById("authScreen").style.display="flex";
  document.getElementById("userApp").style.display="none";
  document.getElementById("bnav").style.display="none";
  document.getElementById("adminApp").style.display="none";
}
function showUserApp() {
  document.getElementById("authScreen").style.display="none";
  document.getElementById("userApp").style.display="block";
  document.getElementById("bnav").style.display="flex";
  document.getElementById("adminApp").style.display="none";
}
function showAdminPanel() {
  document.getElementById("authScreen").style.display="none";
  document.getElementById("userApp").style.display="none";
  document.getElementById("bnav").style.display="none";
  document.getElementById("adminApp").style.display="block";
  document.getElementById("adminLoginBox").style.display="flex";
  document.getElementById("adminDash").style.display="none";
}
function goAdmin() { window.location.hash="#admin"; showAdminPanel(); }

function togglePw(id, btn) {
  const el = document.getElementById(id);
  if (!el) return;
  const show = el.type === "password";
  el.type = show ? "text" : "password";
  if (btn) btn.textContent = show ? "🙈" : "👁";
}
function toggleTheme() {
  const t = document.documentElement.getAttribute("data-theme")==="dark" ? "light" : "dark";
  document.documentElement.setAttribute("data-theme", t);
  document.querySelector(".tb-btn").textContent = t==="dark" ? "☀️" : "🌙";
  try { localStorage.setItem("__theme", t); } catch(e) {}
}
function goPage(page) {
  document.querySelectorAll(".pg").forEach(p => p.classList.remove("on"));
  document.querySelectorAll(".bn-item").forEach(b => b.classList.remove("on"));
  const pg = document.getElementById("pg" + page.charAt(0).toUpperCase() + page.slice(1));
  const bn = document.getElementById("bn-" + page);
  if (pg) pg.classList.add("on");
  if (bn) bn.classList.add("on");
  window.scrollTo({top:0,behavior:"smooth"});
  if (page === "wallet") { loadTxns(); checkPendingWD(); }
  if (page === "earn")   { loadReferrals(); }
}
function switchTab(tab) {
  ["login","signup"].forEach(t => {
    document.getElementById("tab"+t.charAt(0).toUpperCase()+t.slice(1)).classList.toggle("on", t===tab);
    document.getElementById("form"+t.charAt(0).toUpperCase()+t.slice(1)).style.display = t===tab?"block":"none";
  });
  ["loginErr","signupErr","signupOk"].forEach(id => { const el=document.getElementById(id); if(el)el.textContent=""; });
}

// ── App settings ──────────────────────────────────────────────────────────────
async function loadAppSettings() {
  try {
    const doc = await db.collection("app_settings").doc("config").get();
    if (doc.exists) {
      const d = doc.data();
      if (d.bookPrice) BOOK_PRICE = d.bookPrice;
      if (d.refReward) REF_REWARD  = d.refReward;
      if (d.kmRate)    KM_RATE     = d.kmRate;
      if (d.kmCap)     KM_CAP      = d.kmCap;
      if (d.minWD)     MIN_WD      = d.minWD;
      if (d.upiId)     UPI_ID      = d.upiId;
      if (d.prize1)    PRIZE_1     = d.prize1;
      if (d.prize2)    PRIZE_2     = d.prize2;
      if (d.prize3)    PRIZE_3     = d.prize3;
    }
  } catch(e) { console.error("loadAppSettings:", e); }
}

// ── Login ─────────────────────────────────────────────────────────────────────
async function doLogin() {
  const btn = document.getElementById("btnLogin");
  const errEl = document.getElementById("loginErr");
  errEl.textContent = "";
  const rl = checkRL(); if (rl) { errEl.textContent = rl; return; }
  const email = document.getElementById("loginEmail").value.trim().toLowerCase();
  const pass  = document.getElementById("loginPass").value;
  if (!isEmail(email)) { errEl.textContent="Enter a valid email."; return; }
  if (!pass) { errEl.textContent="Password is required."; return; }
  btn.disabled=true; btn.textContent="Signing in...";
  try {
    await auth.signInWithEmailAndPassword(email, pass);
    recordAtt(true);
  } catch(e) {
    recordAtt(false);
    errEl.textContent = authErr(e.code);
  } finally { btn.disabled=false; btn.textContent="Sign In →"; }
}

// ── Signup ────────────────────────────────────────────────────────────────────
async function doSignup() {
  const btn   = document.getElementById("btnSignup");
  const errEl = document.getElementById("signupErr");
  const okEl  = document.getElementById("signupOk");
  errEl.textContent=""; okEl.textContent="";
  const name  = san(document.getElementById("signupName").value).trim();
  const email = document.getElementById("signupEmail").value.trim().toLowerCase();
  const pass  = document.getElementById("signupPass").value;
  const ref   = san(document.getElementById("signupRef").value).toUpperCase().trim();
  if (!name || name.length < 2) { errEl.textContent="Enter your full name."; return; }
  if (!isEmail(email))          { errEl.textContent="Enter a valid email."; return; }
  if (!pass || pass.length < 8) { errEl.textContent="Password must be 8+ characters."; return; }

  // Check free referral
  let isFreeRef = false;
  if (ref) {
    try {
      const freeSnap = await db.collection("free_referrals").where("code","==",ref).where("active","==",true).limit(1).get();
      if (!freeSnap.empty) isFreeRef = true;
    } catch(e) {}
  }

  btn.disabled=true; btn.textContent="Creating account...";
  try {
    const cred = await auth.createUserWithEmailAndPassword(email, pass);
    await cred.user.updateProfile({ displayName: name });
    const myRef = genRef(name, cred.user.uid);
    await db.collection("users").doc(cred.user.uid).set({
      name, email, refCode: myRef, referredBy: ref || null,
      joinedAt: firebase.firestore.FieldValue.serverTimestamp(),
      wallet: 0, balance: 0,
      bookPurchased: isFreeRef,
      bookStatus: isFreeRef ? "approved" : "none",
      totalKm: 0, todayKm: 0, kmLimit: KM_CAP,
      referralCount: 0, isAdminGranted: isFreeRef
    });
    if (ref && !isFreeRef) await creditRef(ref, cred.user.uid);
    okEl.textContent = isFreeRef
      ? "✅ Account created with FREE access! Welcome!"
      : "✅ Account created! Welcome to Health is Wealth!";
    recordAtt(true);
  } catch(e) {
    recordAtt(false);
    errEl.textContent = authErr(e.code);
  } finally { btn.disabled=false; btn.textContent="Create Account →"; }
}

async function creditRef(refCode, newUid) {
  try {
    const snap = await db.collection("users").where("refCode","==",refCode).limit(1).get();
    if (snap.empty) return;
    const refDoc = snap.docs[0];
    if (refDoc.id === newUid) return;
    if (!refDoc.data().bookPurchased) return;
    await db.collection("referrals").add({
      referrerId: refDoc.id, referredId: newUid,
      status: "pending", amount: 0,
      createdAt: firebase.firestore.FieldValue.serverTimestamp()
    });
  } catch(e) { console.error("creditRef:", e); }
}

async function forgotPw() {
  const email = document.getElementById("loginEmail").value.trim();
  if (!isEmail(email)) { document.getElementById("loginErr").textContent="Enter your email first."; return; }
  try {
    await auth.sendPasswordResetEmail(email);
    const el = document.getElementById("loginErr");
    el.style.color="#6ee7b7";
    el.textContent="✅ Reset link sent to your email!";
  } catch(e) { document.getElementById("loginErr").textContent=authErr(e.code); }
}

async function doLogout() {
  if (!confirm("Sign out?")) return;
  if (gpsRunning) await stopGPS();
  await auth.signOut();
  currentUser = null;
  S = { wallet:0, totalKm:0, todayKm:0, kmLimit:300, referralCount:0, bookPurchased:false, bookStatus:"none", refCode:"", pendingWDId:null };
}

// ── Load User ─────────────────────────────────────────────────────────────────
async function loadUser(user) {
  showUserApp();
  currentUser = user;
  await loadAppSettings();
  try {
    const doc = await db.collection("users").doc(user.uid).get();
    if (doc.exists) {
      const d = doc.data();
      S.wallet        = d.wallet || d.balance || 0;
      S.refCode       = d.refCode || genRef(user.displayName||"USER", user.uid);
      S.bookPurchased = d.bookPurchased || false;
      S.bookStatus    = d.bookStatus || "none";
      S.totalKm       = d.totalKm || 0;
      S.todayKm       = d.todayKm || 0;
      S.kmLimit       = d.kmLimit || KM_CAP;
      S.referralCount = d.referralCount || 0;
    } else {
      const myRef = genRef(user.displayName||"USER", user.uid);
      await db.collection("users").doc(user.uid).set({
        name: user.displayName||"User", email: user.email||"",
        refCode: myRef, referredBy: null,
        joinedAt: firebase.firestore.FieldValue.serverTimestamp(),
        wallet:0, balance:0, bookPurchased:false, bookStatus:"none",
        totalKm:0, todayKm:0, kmLimit:KM_CAP, referralCount:0
      });
      S.refCode = myRef;
    }
  } catch(e) { console.error("loadUser:", e); }

  // Real-time listener
  db.collection("users").doc(user.uid).onSnapshot(snap => {
    if (!snap || !snap.exists) return;
    const d = snap.data();
    S.wallet        = d.wallet || d.balance || 0;
    S.totalKm       = d.totalKm || 0;
    S.todayKm       = d.todayKm || 0;
    S.kmLimit       = d.kmLimit || KM_CAP;
    S.bookPurchased = d.bookPurchased || false;
    S.bookStatus    = d.bookStatus || "none";
    S.referralCount = d.referralCount || 0;
    renderUI(user);
  });
  renderUI(user);
}

function renderUI(user) {
  const name = san(user.displayName || "Friend");
  document.getElementById("heroName").innerHTML = `Welcome, <span>${name}</span> 👋`;
  document.getElementById("hKm").textContent       = S.todayKm.toFixed(2);
  document.getElementById("hEarned").textContent   = "₹" + S.wallet.toFixed(0);
  document.getElementById("hRefs").textContent     = S.referralCount;
  document.getElementById("statKm").textContent    = S.todayKm.toFixed(2);
  document.getElementById("statKmSub").textContent = `Cap: ${S.kmLimit} KM`;
  document.getElementById("pbKm").style.width      = Math.min(100,(S.totalKm/S.kmLimit)*100)+"%";
  document.getElementById("statWallet").textContent= "₹"+S.wallet.toFixed(0);
  document.getElementById("pbWallet").style.width  = Math.min(100,(S.wallet/REF_REWARD)*100)+"%";
  document.getElementById("statRefs").textContent  = S.referralCount;
  document.getElementById("statTotalEarn").textContent = "₹"+S.wallet.toFixed(0);
  document.getElementById("walletAmt").textContent = S.wallet.toFixed(0);
  document.getElementById("tbWallet").textContent  = "₹"+S.wallet.toFixed(0);
  document.getElementById("walletSub").textContent = S.bookPurchased
    ? `${S.totalKm.toFixed(1)} km total · ${S.referralCount} referrals`
    : `Pay ₹${BOOK_PRICE} to start earning`;
  document.getElementById("profileName").textContent  = name;
  document.getElementById("profileEmail").textContent = san(user.email||"");
  document.getElementById("profRefCode").textContent  = S.refCode;
  document.getElementById("profTotalKm").textContent  = S.totalKm.toFixed(1)+" km";
  document.getElementById("profRefs").textContent     = S.referralCount;
  document.getElementById("profCapLeft").textContent  = Math.max(0, S.kmLimit-S.totalKm).toFixed(1)+" km";
  document.getElementById("profileBadge").innerHTML = S.bookPurchased
    ? '<span class="profile-badge badge-paid">✅ GPS Earn Active</span>'
    : `<span class="profile-badge badge-free">🔒 Pay ₹${BOOK_PRICE} to Unlock</span>`;
  const payBtn = document.getElementById("profilePayBtn");
  if (payBtn) payBtn.style.display = S.bookPurchased ? "none" : "flex";

  // Referral section - auto-show if bookPurchased
  const refCode = document.getElementById("refCode");
  const refUrl  = document.getElementById("refUrl");
  if (refCode) refCode.textContent = S.refCode;
  if (refUrl)  refUrl.textContent  = window.location.origin + window.location.pathname + "?ref=" + S.refCode;

  document.getElementById("gpsPaidContent").style.display   = S.bookPurchased ? "block" : "none";
  document.getElementById("gpsLockedContent").style.display = S.bookPurchased ? "none"  : "block";
  document.getElementById("refPaidContent").style.display   = S.bookPurchased ? "block" : "none";
  document.getElementById("refLockedContent").style.display = S.bookPurchased ? "none"  : "block";
  updateCycleUI();
  updatePayStatus();
}

function updatePayStatus() {
  const el  = document.getElementById("payStatus");
  const pEl = document.getElementById("profilePayStatus");
  let html = "";
  if (S.bookPurchased) {
    html = `<div style="background:var(--green2);border:1.5px solid rgba(13,158,74,.25);border-radius:10px;padding:12px;text-align:center;margin-bottom:12px">
      <div style="font-size:1.2rem;margin-bottom:4px">✅</div>
      <div style="font-weight:700;color:var(--green);font-size:.88rem">GPS Earn Activated!</div>
      <div style="font-size:.74rem;color:var(--txt3);margin-top:2px">Walk to earn ₹${KM_RATE}/KM + ₹${REF_REWARD}/referral</div>
    </div>`;
  } else if (S.bookStatus === "pending") {
    html = `<div style="background:var(--amber2);border:1.5px solid rgba(184,106,0,.2);border-radius:10px;padding:12px;text-align:center;margin-bottom:12px">
      <div style="font-size:1.2rem;margin-bottom:4px">⏳</div>
      <div style="font-weight:700;color:var(--amber);font-size:.88rem">Payment Under Review</div>
      <div style="font-size:.74rem;color:var(--txt3);margin-top:2px">Admin will verify your payment within 24 hours.</div>
    </div>`;
  } else {
    if (pEl) pEl.innerHTML = `<div style="font-size:.8rem;color:var(--txt4);margin-bottom:8px">Pay ₹${BOOK_PRICE} once to unlock GPS Walk Earn and Referral Rewards.</div>`;
    if (el)  el.innerHTML = "";
    return;
  }
  if (el)  el.innerHTML = html;
  if (pEl) pEl.innerHTML = html;
}

function updateCycleUI() {
  const walked   = S.totalKm || 0;
  const cap      = S.kmLimit || KM_CAP;
  const remain   = Math.max(0, cap - walked);
  const pct      = Math.min(100, (walked / cap) * 100);
  const cw=document.getElementById("cycleWalked"), cr=document.getElementById("cycleRemain"),
        ce=document.getElementById("cycleEarned"), cp=document.getElementById("cycleProg"),
        cl=document.getElementById("cycleCapLbl"), gpt=document.getElementById("gpsCapTxt");
  if (cw) cw.textContent = walked.toFixed(1);
  if (cr) cr.textContent = remain.toFixed(1);
  if (ce) ce.textContent = "₹" + Math.floor(walked * KM_RATE);
  if (cp) cp.style.width = pct + "%";
  if (cl) cl.textContent = `Cap: ${cap} KM — refer to reset!`;
  if (gpt) gpt.textContent = `Cap: ${cap} KM · Walked: ${walked.toFixed(1)} KM`;
}

// ── GPS Walk ──────────────────────────────────────────────────────────────────
let gpsWatchId=null, gpsLastPos=null, gpsSessionKm=0, gpsPendingKm=0;
let gpsRunning=false, gpsPaused=false;

function haversineKm(lat1,lon1,lat2,lon2) {
  const R=6371, dLat=(lat2-lat1)*Math.PI/180, dLon=(lon2-lon1)*Math.PI/180;
  const a = Math.sin(dLat/2)**2 + Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLon/2)**2;
  return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
}

function startGPS() {
  if (!S.bookPurchased) { showToast("⚠ Pay ₹"+BOOK_PRICE+" first to unlock GPS Earn"); return; }
  if (!navigator.geolocation) { showToast("⚠ GPS not supported on this device"); return; }
  if ((S.kmLimit - S.totalKm) <= 0) { showToast("⚠ KM cap reached! Refer friends to reset."); return; }
  gpsRunning=true; gpsPaused=false; gpsSessionKm=0; gpsPendingKm=0; gpsLastPos=null;
  setGPSUI("tracking");
  showToast("📍 GPS started! Walk to earn ₹"+KM_RATE+"/KM", 3000);
  gpsWatchId = navigator.geolocation.watchPosition(
    pos => {
      if (!gpsRunning || gpsPaused) return;
      const {latitude:lat, longitude:lon, speed, accuracy} = pos.coords;
      if (accuracy > 30) return;
      const spd = speed != null ? speed * 3.6 : 0;
      const speedEl = document.getElementById("gpsSpeed");
      if (speedEl) speedEl.textContent = spd.toFixed(1);
      if (speed !== null && (speed < 0.14 || speed > 2.2)) { gpsLastPos=null; return; }
      if (gpsLastPos) {
        const dist = haversineKm(gpsLastPos.lat, gpsLastPos.lon, lat, lon);
        if (dist > 0.003 && dist < 0.2) {
          gpsSessionKm += dist; gpsPendingKm += dist;
          updateGPSDisplay();
          if (gpsPendingKm >= 0.1) { saveGPSChunk(gpsPendingKm); gpsPendingKm=0; }
        }
      }
      gpsLastPos = {lat, lon};
    },
    err => {
      if (err.code===1) showToast("⚠ Location denied. Enable GPS in settings.");
      else if (err.code===2) showToast("⚠ GPS unavailable. Go outdoors.");
      else showToast("⚠ GPS error. Try again.");
      stopGPS();
    },
    {enableHighAccuracy:true, maximumAge:3000, timeout:15000}
  );
}

function pauseGPS() {
  gpsPaused = !gpsPaused;
  const btn=document.getElementById("gpsBtnPause"), dot=document.getElementById("gpsDot"), txt=document.getElementById("gpsStatusTxt");
  if (btn) btn.textContent = gpsPaused ? "▶ Resume" : "⏸ Pause";
  if (dot) dot.className  = "gps-dot " + (gpsPaused ? "paused" : "live");
  if (txt) txt.textContent = gpsPaused ? "Walk paused" : "Tracking your walk...";
}

async function stopGPS() {
  if (gpsWatchId !== null) { navigator.geolocation.clearWatch(gpsWatchId); gpsWatchId=null; }
  gpsRunning=false; gpsPaused=false;
  if (gpsPendingKm > 0.01) { await saveGPSChunk(gpsPendingKm); gpsPendingKm=0; }
  setGPSUI("idle");
  showToast(`✅ ${gpsSessionKm.toFixed(2)} KM saved · ₹${(gpsSessionKm*KM_RATE).toFixed(0)} earned`);
  gpsSessionKm=0; gpsLastPos=null;
}

function setGPSUI(mode) {
  const dot=document.getElementById("gpsDot"), txt=document.getElementById("gpsStatusTxt"),
        start=document.getElementById("gpsBtnStart"), pause=document.getElementById("gpsBtnPause"),
        stop=document.getElementById("gpsBtnStop");
  if (mode==="tracking") {
    if(dot)dot.className="gps-dot live"; if(txt)txt.textContent="Tracking your walk...";
    if(start)start.style.display="none"; if(pause)pause.style.display="flex"; if(stop)stop.style.display="flex";
  } else {
    if(dot)dot.className="gps-dot off"; if(txt)txt.textContent="Tap Start to begin tracking";
    if(start)start.style.display="flex"; if(pause)pause.style.display="none"; if(stop)stop.style.display="none";
  }
}

function updateGPSDisplay() {
  const total = S.todayKm + gpsSessionKm;
  const kmEl=document.getElementById("gpsKm"), earnEl=document.getElementById("gpsEarned"),
        progEl=document.getElementById("gpsProgFill");
  if(kmEl)   kmEl.textContent   = total.toFixed(2);
  if(earnEl) earnEl.textContent = "₹"+(gpsSessionKm*KM_RATE).toFixed(2);
  const pct = Math.min(100,((S.totalKm+gpsSessionKm)/S.kmLimit)*100);
  if(progEl) progEl.style.width = pct+"%";
}

async function saveGPSChunk(km) {
  if (!currentUser || km < 0.001) return;
  const ref = db.collection("users").doc(currentUser.uid);
  try {
    const doc = await ref.get();
    if (!doc || !doc.exists) return;
    const d = doc.data();
    const remaining = (d.kmLimit||KM_CAP) - (d.totalKm||0);
    if (remaining <= 0) { stopGPS(); showToast("⚠ KM cap reached!"); return; }
    const allowed = Math.min(km, remaining);
    const earn = parseFloat((allowed * KM_RATE).toFixed(2));
    await ref.update({
      totalKm: firebase.firestore.FieldValue.increment(allowed),
      todayKm: firebase.firestore.FieldValue.increment(allowed),
      wallet:  firebase.firestore.FieldValue.increment(earn),
      balance: firebase.firestore.FieldValue.increment(earn)
    });
    await db.collection("km_logs").add({
      uid: currentUser.uid, km: allowed, earned: earn,
      date: new Date().toISOString().slice(0,10),
      ts: firebase.firestore.FieldValue.serverTimestamp()
    });
  } catch(e) { console.error("saveGPS:", e); }
}

// ── Referral ──────────────────────────────────────────────────────────────────
function copyRef() {
  const url = window.location.origin + window.location.pathname + "?ref=" + S.refCode;
  if (navigator.clipboard) navigator.clipboard.writeText(url).then(()=>showToast("📋 Referral link copied!"));
  else showToast("📋 Code: " + S.refCode);
}
function shareWA() {
  const url = window.location.origin + window.location.pathname + "?ref=" + S.refCode;
  window.open("https://wa.me/?text=" + encodeURIComponent("🌿 Join Health is Wealth! Walk daily & earn real money! I earned ₹"+S.wallet.toFixed(0)+" so far! Join using my link: " + url));
}
function shareTG() {
  const url = window.location.origin + window.location.pathname + "?ref=" + S.refCode;
  window.open("https://t.me/share/url?url=" + encodeURIComponent(url) + "&text=" + encodeURIComponent("🌿 Join Health is Wealth — Walk & Earn real money!"));
}

async function loadReferrals() {
  if (!currentUser) return;
  const list = document.getElementById("recruitList");
  if (!list) return;
  try {
    const snap = await db.collection("referrals").where("referrerId","==",currentUser.uid).orderBy("createdAt","desc").limit(20).get();
    if (!snap || snap.empty) { list.innerHTML='<div class="empty"><div class="empty-ico">👥</div><div class="empty-txt">No paid referrals yet. Share your code!</div></div>'; return; }
    list.innerHTML = snap.docs.map(d => {
      const r = d.data();
      const stat = r.status==="paid"
        ? `<span class="rb-paid">✅ ₹${REF_REWARD}</span>`
        : '<span class="rb-pending">⏳ Pending</span>';
      return `<div class="recruit-item"><div class="recruit-av">${esc(r.referredName||"U").slice(0,1)}</div><div style="flex:1"><div style="font-weight:600;font-size:.86rem;color:var(--txt2)">${esc(r.referredName||"User")}</div><div style="font-size:.72rem;color:var(--txt4)">Direct Referral · +₹${REF_REWARD} + ${KM_CAP}KM reset</div></div>${stat}</div>`;
    }).join("");
  } catch(e) { console.error("loadReferrals:", e); }
}

// ── Payment (UPI unlock) ──────────────────────────────────────────────────────
function openPayment()  { document.getElementById("upiSheet").classList.add("on"); }
function closePayment() {
  document.getElementById("upiSheet").classList.remove("on");
  const txn = document.getElementById("txnId"); if (txn) txn.value = "";
  const err = document.getElementById("txnErr"); if (err) { err.textContent=""; err.style.display="none"; }
}
function closePaymentBg(e) { if(e.target===document.getElementById("upiSheet")) closePayment(); }
function copyUPI() {
  if (navigator.clipboard) navigator.clipboard.writeText(UPI_ID).then(()=>showToast("📋 Copied: "+UPI_ID));
  else showToast("UPI: " + UPI_ID);
}
function openUPI(app) {
  const pa=UPI_ID, pn="Health+is+Wealth", am=BOOK_PRICE, tn="Unlock+GPS+Earn";
  const upiUrl = `upi://pay?pa=${pa}&pn=${pn}&am=${am}&cu=INR&tn=${tn}`;
  const pkgs = {gpay:"com.google.android.apps.nbu.paisa.user", phonepe:"com.phonepe.app", bhim:"in.org.npci.upiapp"};
  if (pkgs[app]) {
    window.location.href = `intent://pay?pa=${pa}&pn=${pn}&am=${am}&cu=INR&tn=${tn}#Intent;scheme=upi;package=${pkgs[app]};end`;
    setTimeout(()=>window.open(upiUrl,"_blank"), 600);
  } else {
    window.location.href = upiUrl;
    setTimeout(()=>{ if(navigator.clipboard) navigator.clipboard.writeText(UPI_ID).then(()=>showToast("📋 UPI ID copied")); }, 1000);
  }
}

// ── SUBMIT PAYMENT — ₹1999 AUTO-ACTIVATES referral link (no UTR needed) ──────
async function submitPayment() {
  const txnInput = document.getElementById("txnId");
  const txn      = san(txnInput.value).trim();
  const errEl    = document.getElementById("txnErr");
  const btn      = document.getElementById("submitPayBtn");
  errEl.textContent=""; errEl.style.display="none";

  if (!txn) { errEl.textContent="⚠️ Please enter your UTR / Transaction ID."; errEl.style.display="block"; txnInput.focus(); return; }
  if (txn.length < 6) { errEl.textContent="⚠️ UTR must be at least 6 characters."; errEl.style.display="block"; txnInput.focus(); return; }
  if (!currentUser) { showToast("⚠ Please login again."); return; }
  if (S.bookPurchased) { showToast("✅ Already activated!"); closePayment(); return; }
  if (S.bookStatus === "pending") { showToast("⏳ Payment already under review."); return; }

  btn.disabled=true; btn.textContent="Submitting...";
  try {
    // Duplicate UTR check
    const dupCheck = await db.collection("payments").where("txnId","==",txn).limit(1).get();
    if (!dupCheck.empty) {
      errEl.textContent="❌ This UTR has already been submitted. Enter a different UTR.";
      errEl.style.display="block";
      btn.disabled=false; btn.textContent="✅ Submit Payment for Verification";
      return;
    }

    const payRef = await db.collection("payments").add({
      userId: currentUser.uid, name: currentUser.displayName||"User",
      email: currentUser.email||"", txnId: txn, amount: BOOK_PRICE,
      status: "pending", submittedAt: firebase.firestore.FieldValue.serverTimestamp()
    });
    await db.collection("users").doc(currentUser.uid).update({ bookStatus: "pending" });

    // Auto-activate referral link immediately for ₹1999 payers
    // (They get referral link right away; GPS earn activates after admin approval)
    await db.collection("users").doc(currentUser.uid).update({
      referralLinkUnlocked: true // referral link shown immediately
    });

    // WhatsApp admin notification
    const waMsg = encodeURIComponent(
      `🌿 *Health is Wealth — New Payment*

` +
      `👤 *Name:* ${currentUser.displayName||"User"}
` +
      `📧 *Email:* ${currentUser.email||"N/A"}
` +
      `💳 *UTR:* ${txn}
💰 *Amount:* ₹${BOOK_PRICE}
` +
      `📅 *Time:* ${new Date().toLocaleString("en-IN")}

` +
      `✅ Approve in Admin Panel:
${window.location.origin}${window.location.pathname}#admin

Payment ID: ${payRef.id}`
    );
    closePayment();
    S.bookStatus = "pending";
    updatePayStatus();
    document.getElementById("modalIco").textContent  = "🎉";
    document.getElementById("modalTitle").textContent = "Payment Submitted!";
    document.getElementById("modalMsg").textContent   = `UTR: ${txn}\n\n✅ Your referral link is now active! Start sharing and earn ₹300/referral immediately.\n\nGPS Walk Earn will activate within 24 hours after admin verifies your payment.`;
    document.getElementById("successModal").classList.add("on");
    setTimeout(()=>window.open(`https://wa.me/${ADMIN_WA}?text=${waMsg}`,"_blank"), 1500);
  } catch(e) {
    errEl.textContent="⚠️ Submission failed: "+e.message+". Try again.";
    errEl.style.display="block";
  } finally { btn.disabled=false; btn.textContent="✅ Submit Payment for Verification"; }
}

function closeModal() { document.getElementById("successModal").classList.remove("on"); }

// ── Withdrawal method toggle ──────────────────────────────────────────────────
let currentWDMethod = "upi";
function switchWDMethod(method) {
  currentWDMethod = method;
  document.getElementById("wdUpiFields").style.display  = method==="upi"  ? "block" : "none";
  document.getElementById("wdBankFields").style.display = method==="bank" ? "block" : "none";
  document.getElementById("wdTabUpi").classList.toggle("on",  method==="upi");
  document.getElementById("wdTabBank").classList.toggle("on", method==="bank");
}

// ── Check pending withdrawal & show/hide blocks ───────────────────────────────
async function checkPendingWD() {
  if (!currentUser) return;
  try {
    const snap = await db.collection("withdrawals")
      .where("uid","==",currentUser.uid)
      .where("status","==","pending")
      .limit(1).get();
    const newBlock  = document.getElementById("wdNewBlock");
    const pendBlock = document.getElementById("wdPendingBlock");
    const pendInfo  = document.getElementById("wdPendingInfo");
    if (!snap.empty) {
      const d = snap.docs[0].data();
      S.pendingWDId = snap.docs[0].id;
      if (pendBlock) pendBlock.style.display="block";
      if (newBlock)  newBlock.style.display="none";
      if (pendInfo)  pendInfo.textContent = `₹${d.amount} withdrawal request is pending admin approval. You can cancel and re-submit if needed.`;
    } else {
      S.pendingWDId = null;
      if (pendBlock) pendBlock.style.display="none";
      if (newBlock)  newBlock.style.display="block";
    }
  } catch(e) { console.error("checkPendingWD:", e); }
}

// ── Cancel withdrawal ─────────────────────────────────────────────────────────
async function cancelWithdrawal() {
  if (!S.pendingWDId || !currentUser) return;
  if (!confirm("Cancel this withdrawal request? Your balance will be refunded.")) return;
  try {
    const doc = await db.collection("withdrawals").doc(S.pendingWDId).get();
    if (!doc.exists || doc.data().status !== "pending") { showToast("⚠ Request not found or already processed."); return; }
    const amt = doc.data().amount || 0;
    await db.collection("withdrawals").doc(S.pendingWDId).update({
      status: "cancelled", cancelledAt: firebase.firestore.FieldValue.serverTimestamp()
    });
    await db.collection("users").doc(currentUser.uid).update({
      wallet:  firebase.firestore.FieldValue.increment(amt),
      balance: firebase.firestore.FieldValue.increment(amt)
    });
    S.pendingWDId = null;
    showToast("✅ Withdrawal cancelled. ₹"+amt+" refunded to wallet.");
    checkPendingWD();
    loadTxns();
  } catch(e) { showToast("⚠ Cancel failed: "+e.message); }
}

// ── Request withdrawal ────────────────────────────────────────────────────────
async function requestWithdraw() {
  if (!currentUser) return;
  if (!S.bookPurchased) { showToast("⚠ Activate your account first."); return; }

  // Anti-cheat: only one pending at a time
  const pending = await db.collection("withdrawals").where("uid","==",currentUser.uid).where("status","==","pending").limit(1).get();
  if (!pending.empty) { showToast("⚠ You already have a pending request. Cancel it first."); return; }

  const amt = parseFloat(document.getElementById("withdrawAmt").value);
  if (!amt || amt < MIN_WD) { showToast(`⚠ Minimum withdrawal is ₹${MIN_WD}`); return; }
  if (S.wallet < amt) { showToast(`⚠ Insufficient balance. Available: ₹${S.wallet.toFixed(0)}`); return; }

  let paymentDetail = {};
  if (currentWDMethod === "upi") {
    const upi = san(document.getElementById("withdrawUPI").value).trim();
    if (!upi || upi.length < 5) { showToast("⚠ Enter a valid UPI ID / GPay / PhonePe number"); return; }
    paymentDetail = { method: "upi", upiId: upi };
  } else {
    const accName = san(document.getElementById("withdrawAccName").value).trim();
    const accNo   = san(document.getElementById("withdrawAccNo").value).trim();
    const accNo2  = san(document.getElementById("withdrawAccNo2").value).trim();
    const ifsc    = san(document.getElementById("withdrawIFSC").value).trim().toUpperCase();
    const bank    = san(document.getElementById("withdrawBankName").value).trim();
    if (!accName) { showToast("⚠ Enter account holder name"); return; }
    if (!accNo || accNo.length < 8) { showToast("⚠ Enter valid account number"); return; }
    if (accNo !== accNo2) { showToast("⚠ Account numbers do not match"); return; }
    if (!ifsc || ifsc.length !== 11) { showToast("⚠ Enter valid 11-character IFSC code"); return; }
    if (!bank) { showToast("⚠ Enter bank name"); return; }
    paymentDetail = { method: "bank", accName, accNo, ifsc, bankName: bank };
  }

  if (!confirm(`Request withdrawal of ₹${amt}?
This will deduct from your wallet immediately.`)) return;

  const btn = document.getElementById("btnRequestWD");
  btn.disabled=true; btn.textContent="Submitting...";
  try {
    await db.collection("withdrawals").add({
      uid: currentUser.uid, name: currentUser.displayName||"User",
      email: currentUser.email||"", amount: amt,
      ...paymentDetail,
      upiId: paymentDetail.upiId || "Bank Transfer", // for backward compat in admin
      status: "pending", time: firebase.firestore.FieldValue.serverTimestamp()
    });
    await db.collection("users").doc(currentUser.uid).update({
      wallet:  firebase.firestore.FieldValue.increment(-amt),
      balance: firebase.firestore.FieldValue.increment(-amt)
    });
    // Clear form
    document.getElementById("withdrawAmt").value = "";
    if (currentWDMethod==="upi") document.getElementById("withdrawUPI").value="";
    else {
      ["withdrawAccName","withdrawAccNo","withdrawAccNo2","withdrawIFSC","withdrawBankName"].forEach(id=>{
        const el=document.getElementById(id); if(el) el.value="";
      });
    }
    showToast(`✅ Withdrawal ₹${amt} requested! Admin will process in 1-3 days.`);
    checkPendingWD();
    loadTxns();
  } catch(e) { showToast("⚠ Request failed: "+e.message); }
  finally { btn.disabled=false; btn.textContent="💸 Submit Withdrawal Request"; }
}

// ── Transaction history ───────────────────────────────────────────────────────
async function loadTxns() {
  if (!currentUser) return;
  const list = document.getElementById("txnList");
  if (!list) return;
  try {
    const [p, w] = await Promise.all([
      db.collection("payments").where("userId","==",currentUser.uid).orderBy("submittedAt","desc").limit(10).get().catch(()=>({docs:[]})),
      db.collection("withdrawals").where("uid","==",currentUser.uid).orderBy("time","desc").limit(10).get().catch(()=>({docs:[]}))
    ]);
    const items = [];
    p.docs.forEach(d => {
      const t=d.data();
      items.push({type:"pay", desc:`Payment — Unlock GPS Earn (UTR: ${t.txnId||"—"})`, amt:t.amount||BOOK_PRICE, status:t.status, date:t.submittedAt?.toDate()});
    });
    w.docs.forEach(d => {
      const t=d.data();
      const dest = t.method==="bank" ? `Bank (${t.ifsc||""})` : (t.upiId||"UPI");
      items.push({type:"wd", desc:`Withdrawal → ${dest}`, amt:t.amount||0, status:t.status, date:t.time?.toDate()});
    });
    if (!items.length) { list.innerHTML='<div class="empty"><div class="empty-ico">📭</div><div class="empty-txt">No transactions yet</div></div>'; return; }
    items.sort((a,b) => (b.date||0)-(a.date||0));
    const sc = s => s==="approved"||s==="paid"?"var(--green)":s==="rejected"||s==="cancelled"?"var(--red)":"var(--amber)";
    list.innerHTML = items.map(t => `
      <div class="txn-item">
        <div class="txn-ico ${t.type==="wd"?"txn-ico-out":"txn-ico-in"}">${t.type==="wd"?"💸":"💰"}</div>
        <div class="txn-desc">${esc(t.desc)}<div class="txn-status" style="color:${sc(t.status)}">${t.status}</div></div>
        <span class="${t.type==="wd"?"txn-out":"txn-in"}">${t.type==="wd"?"-":""}₹${t.amt}</span>
      </div>`).join("");
  } catch(e) { console.error("loadTxns:", e); }
}

// ── App init ──────────────────────────────────────────────────────────────────
(function checkUrlRef() {
  const urlParams = new URLSearchParams(window.location.search);
  const ref = urlParams.get("ref");
  if (ref) { try { sessionStorage.setItem("__pendingRef", ref.toUpperCase()); } catch(e) {} }
})();

window.addEventListener("load", () => {
  try {
    const t = localStorage.getItem("__theme");
    if (t) { document.documentElement.setAttribute("data-theme",t); if(t==="dark"){const b=document.querySelector(".tb-btn");if(b)b.textContent="☀️";} }
  } catch(e) {}
  const splash = document.getElementById("splash");
  setTimeout(()=>{ if(splash) splash.classList.add("gone"); }, 2600);

  // Auto-fill ref code from URL
  try {
    const pendRef = sessionStorage.getItem("__pendingRef");
    if (pendRef) {
      const refInput = document.getElementById("signupRef");
      if (refInput) { refInput.value = pendRef; switchTab("signup"); }
      sessionStorage.removeItem("__pendingRef");
    }
  } catch(e) {}

  if (window.location.hash==="#admin") { showAdminPanel(); return; }
  if (!auth) { setTimeout(showAuth, 2700); return; }
  let resolved=false;
  const safeTimer = setTimeout(()=>{ if(!resolved){resolved=true;showAuth();} }, 7000);
  auth.onAuthStateChanged(user => {
    if (!resolved) { resolved=true; clearTimeout(safeTimer); }
    if (user) { currentUser=user; loadUser(user); }
    else { currentUser=null; showAuth(); }
  });
});

window.addEventListener("hashchange", ()=>{
  if (window.location.hash==="#admin") showAdminPanel();
  else if (currentUser) showUserApp();
  else showAuth();
});

// ══════════════════════════════════════════════════════════════════════════════
//  ADMIN PANEL
// ══════════════════════════════════════════════════════════════════════════════
let adminRefCode = null;

async function adminLogin() {
  const email = document.getElementById("adminEmail").value.trim().toLowerCase();
  const pass  = document.getElementById("adminPass").value;
  const err   = document.getElementById("adminErr");
  const btn   = document.getElementById("adminLoginBtn");
  err.textContent = "";
  if (!email || !pass) { err.textContent="Enter email and password."; return; }
  if (email !== ADMIN_EMAIL.toLowerCase()) { err.textContent="Access denied. Not admin."; return; }
  if (btn) { btn.disabled=true; btn.textContent="Signing in..."; }
  try {
    const cred = await auth.signInWithEmailAndPassword(email, pass);
    if (cred.user.email.toLowerCase() !== ADMIN_EMAIL.toLowerCase()) { await auth.signOut(); err.textContent="Access denied."; return; }
    document.getElementById("adminBadge").textContent = cred.user.email.split("@")[0];
    document.getElementById("adminLoginBox").style.display="none";
    document.getElementById("adminDash").style.display="block";
    loadAdminStats();
    loadAdminWD();
    loadAdminRefCode();
  } catch(e) { err.textContent=authErr(e.code); }
  finally { if(btn){btn.disabled=false;btn.textContent="Sign In to Admin";} }
}

function adminLogout() {
  auth.signOut().then(()=>{
    const lb=document.getElementById("adminLoginBox"), da=document.getElementById("adminDash");
    if(lb)lb.style.display="flex"; if(da)da.style.display="none";
    const ae=document.getElementById("adminEmail"), ap=document.getElementById("adminPass");
    if(ae)ae.value=""; if(ap)ap.value="";
    window.location.hash=""; showAuth();
  }).catch(()=>{ window.location.hash=""; showAuth(); });
}

function adminTab(btn, tabId) {
  document.querySelectorAll(".admin-tab").forEach(t=>t.classList.remove("on"));
  btn.classList.add("on");
  ["aDash","aUsers","aLeaderboard","aWD","aPay","aSettings"].forEach(id=>{
    const e=document.getElementById(id); if(e)e.style.display="none";
  });
  const el = document.getElementById(tabId);
  if (el) el.style.display="block";
  if (tabId==="aWD")          loadAdminWD();
  else if (tabId==="aPay")    loadAdminPay();
  else if (tabId==="aUsers")  loadAdminUsers();
  else if (tabId==="aLeaderboard") loadLeaderboard();
  else if (tabId==="aSettings")    loadSettings();
  else loadAdminStats();
}

async function loadAdminStats() {
  try {
    const [users, wd, pay] = await Promise.all([
      db.collection("users").get().catch(()=>({size:0,docs:[]})),
      db.collection("withdrawals").where("status","==","pending").get().catch(()=>({size:0,docs:[]})),
      db.collection("payments").where("status","==","pending").get().catch(()=>({size:0,docs:[]}))
    ]);
    document.getElementById("sTotalUsers").textContent = users.size||0;
    const paidCount = (users.docs||[]).filter(d=>d.data().bookPurchased).length;
    document.getElementById("sPaidUsers").textContent  = paidCount;
    document.getElementById("sPendingWD").textContent  = wd.size||0;
    document.getElementById("sPendingPay").textContent = pay.size||0;
    let totalKm=0, totalBal=0;
    (users.docs||[]).forEach(d=>{ const dd=d.data(); totalKm+=dd.totalKm||0; totalBal+=dd.wallet||dd.balance||0; });
    document.getElementById("sTotalKm").textContent  = totalKm.toFixed(1);
    document.getElementById("sTotalBal").textContent = "₹"+totalBal.toFixed(0);
  } catch(e) { console.error("adminStats:", e); }
}

// ── Admin: Load all users ─────────────────────────────────────────────────────
async function loadAdminUsers() {
  const tbody = document.getElementById("usersBody");
  tbody.innerHTML='<tr><td colspan="10" style="text-align:center;padding:20px;color:#5a9070">Loading...</td></tr>';
  try {
    const snap = await db.collection("users").orderBy("joinedAt","desc").limit(200).get();
    if (snap.empty) { tbody.innerHTML='<tr><td colspan="10" style="text-align:center;padding:20px;color:#5a9070">No users yet.</td></tr>'; return; }

    // Get pending withdrawal amounts per user
    const wdSnap = await db.collection("withdrawals").where("status","==","pending").get();
    const wdMap = {};
    wdSnap.docs.forEach(d=>{ const dd=d.data(); wdMap[dd.uid] = (wdMap[dd.uid]||0) + (dd.amount||0); });

    tbody.innerHTML = snap.docs.map((doc,i) => {
      const d = doc.data();
      const joined = d.joinedAt?.toDate()?.toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"2-digit"})||"—";
      const capPct = Math.min(100,((d.totalKm||0)/Math.max(1,d.kmLimit||300))*100);
      const pendWD = wdMap[doc.id] || 0;
      return `<tr>
        <td style="font-weight:800;color:#5a9070;font-size:.8rem">#${i+1}</td>
        <td>
          <strong>${esc(d.name||"—")}</strong>
          <div style="font-size:.68rem;color:#5a9070">${esc(d.email||"—")}</div>
        </td>
        <td style="font-family:monospace;font-weight:700;font-size:.8rem">${esc(d.refCode||"—")}</td>
        <td><strong style="color:var(--green)">₹${(d.wallet||d.balance||0).toFixed(0)}</strong>
          ${pendWD>0?`<div style="font-size:.64rem;color:var(--amber)">⏳ ₹${pendWD} pending WD</div>`:""}
        </td>
        <td>
          <div style="font-weight:700">${(d.totalKm||0).toFixed(1)} km</div>
          <div style="background:#daf0e4;border-radius:4px;height:5px;margin-top:3px;overflow:hidden">
            <div style="height:100%;background:linear-gradient(90deg,#0d9e4a,#f0b429);width:${capPct.toFixed(0)}%;border-radius:4px"></div>
          </div>
        </td>
        <td><span class="pill ${d.bookPurchased?"pill-a":"pill-p"}">${d.bookPurchased?(d.isAdminGranted?"🎁 Free":"✅ Active"):"🔒 Locked"}</span></td>
        <td style="font-weight:700;text-align:center">${d.referralCount||0}</td>
        <td style="font-size:.72rem">${joined}</td>
        <td>
          <div style="display:flex;gap:4px;flex-wrap:wrap">
            <button class="btn-a" style="font-size:.64rem;padding:4px 8px" onclick="adminGrantCap('${doc.id}',${d.totalKm||0})">+KM</button>
            ${!d.bookPurchased?`<button class="btn-a" style="font-size:.64rem;padding:4px 8px;background:linear-gradient(135deg,#6820d0,#a050f8)" onclick="adminGrantFreeById('${doc.id}','${esc(d.name||"")}')">🎁</button>`:""}
            <button class="btn-r" style="font-size:.64rem;padding:4px 8px" onclick="adminEditBalance('${doc.id}','${esc(d.name||"")}',${d.wallet||d.balance||0})">₹</button>
            <button class="btn-danger" style="font-size:.64rem;padding:4px 8px" onclick="adminRemoveUser('${doc.id}','${esc(d.name||"")}','${esc(d.email||"")}')">🗑</button>
          </div>
        </td>
      </tr>`;
    }).join("");
  } catch(e) {
    tbody.innerHTML=`<tr><td colspan="10" style="text-align:center;padding:20px;color:var(--red)">Error: ${esc(e.message)}</td></tr>`;
  }
}

// ── Admin: Remove user ────────────────────────────────────────────────────────
async function adminRemoveUser(uid, name, email) {
  if (!confirm(`⚠️ REMOVE USER?\n\nName: ${name}\nEmail: ${email}\n\nThis will permanently delete this user's data. This cannot be undone.`)) return;
  const confirm2 = prompt(`Type REMOVE to confirm deletion of ${name}:`);
  if (confirm2 !== "REMOVE") { showToast("❌ Cancelled. You must type REMOVE exactly."); return; }
  try {
    // Delete user documents from all collections
    await db.collection("users").doc(uid).delete();
    const [wdSnap, paySnap, refSnap, kmSnap] = await Promise.all([
      db.collection("withdrawals").where("uid","==",uid).get().catch(()=>({docs:[]})),
      db.collection("payments").where("userId","==",uid).get().catch(()=>({docs:[]})),
      db.collection("referrals").where("referrerId","==",uid).get().catch(()=>({docs:[]})),
      db.collection("km_logs").where("uid","==",uid).limit(50).get().catch(()=>({docs:[]}))
    ]);
    const batch = db.batch();
    [...wdSnap.docs, ...paySnap.docs, ...refSnap.docs, ...kmSnap.docs].forEach(d => batch.delete(d.ref));
    await batch.commit();
    showToast(`✅ User ${name} removed successfully.`);
    loadAdminUsers();
  } catch(e) { showToast("⚠ Remove failed: "+e.message); }
}

async function adminGrantCap(uid, currentTotalKm) {
  if (!confirm(`Grant +${KM_CAP} KM cap to this user?`)) return;
  try {
    await db.collection("users").doc(uid).update({kmLimit:(currentTotalKm||0)+KM_CAP});
    showToast(`✅ +${KM_CAP} KM cap granted!`);
    loadAdminUsers();
  } catch(e) { showToast("⚠ Failed: "+e.message); }
}

async function adminGrantFreeById(uid, name) {
  if (!confirm(`Grant FREE GPS Earn access to ${name}?`)) return;
  try {
    await db.collection("users").doc(uid).update({
      bookPurchased:true, bookStatus:"approved", isAdminGranted:true,
      referralLinkUnlocked:true, grantedAt:firebase.firestore.FieldValue.serverTimestamp()
    });
    showToast("✅ Free access granted to "+name);
    loadAdminUsers();
  } catch(e) { showToast("⚠ Failed: "+e.message); }
}

async function adminEditBalance(uid, name, currentBal) {
  const newBal = prompt(`Edit wallet balance for ${name}\nCurrent: ₹${currentBal}\n\nEnter new balance:`, currentBal);
  if (newBal === null) return;
  const n = parseFloat(newBal);
  if (isNaN(n) || n < 0) { showToast("⚠ Invalid amount"); return; }
  if (!confirm(`Set ₹${n} balance for ${name}?`)) return;
  try {
    await db.collection("users").doc(uid).update({wallet:n, balance:n});
    showToast(`✅ Balance updated to ₹${n}`);
    loadAdminUsers();
  } catch(e) { showToast("⚠ Failed: "+e.message); }
}

async function grantFreeAccess() {
  const emailInput = document.getElementById("grantEmail");
  const email = san(emailInput.value).trim().toLowerCase();
  if (!isEmail(email)) { showToast("⚠ Enter valid email"); return; }
  try {
    const snap = await db.collection("users").where("email","==",email).limit(1).get();
    if (snap.empty) { showToast("⚠ User not found with that email"); return; }
    const uid = snap.docs[0].id;
    await db.collection("users").doc(uid).update({
      bookPurchased:true, bookStatus:"approved", isAdminGranted:true,
      referralLinkUnlocked:true, grantedAt:firebase.firestore.FieldValue.serverTimestamp()
    });
    showToast("✅ Free access granted to "+email);
    emailInput.value="";
    loadAdminUsers();
  } catch(e) { showToast("⚠ Failed: "+e.message); }
}

// ── Admin: Withdrawals ────────────────────────────────────────────────────────
let wdUnsub = null;
function loadAdminWD() {
  if (wdUnsub) wdUnsub();
  const tbody = document.getElementById("wdBody");
  tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">Loading...</td></tr>';
  wdUnsub = db.collection("withdrawals").orderBy("time","desc").limit(100).onSnapshot(snap => {
    if (!snap || snap.empty) {
      tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">No withdrawal requests yet.</td></tr>';
      return;
    }
    tbody.innerHTML = snap.docs.map(doc => {
      const d = doc.data();
      const date = d.time?.toDate()?.toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"numeric"})||"—";
      const pcls = d.status==="approved"?"pill-a":d.status==="rejected"?"pill-r":d.status==="cancelled"?"pill-r":"pill-p";

      // Build payment detail box
      let payDetail = "";
      if (d.method === "bank") {
        payDetail = `<div class="wd-detail-box">
          🏦 <strong>Bank Transfer</strong><br>
          Name: <strong>${esc(d.accName||"—")}</strong><br>
          A/c: <strong>${esc(d.accNo||"—")}</strong><br>
          IFSC: <strong>${esc(d.ifsc||"—")}</strong><br>
          Bank: ${esc(d.bankName||"—")}
        </div>`;
      } else {
        payDetail = `<div class="wd-detail-box">
          📱 <strong>UPI / GPay / PhonePe</strong><br>
          ID: <strong style="color:#0d6e3a;font-size:.84rem">${esc(d.upiId||"—")}</strong>
        </div>`;
      }

      const btns = d.status==="pending"
        ? `<div style="display:flex;gap:6px;flex-wrap:wrap">
            <button class="btn-a" onclick="aApproveWD('${doc.id}','${d.uid||""}','${esc(d.upiId||"")}',${d.amount||0},'${esc(d.name||"")}')">✅ Approve</button>
            <button class="btn-r" onclick="aRejectWD('${doc.id}','${d.uid}',${d.amount||0})">❌ Reject</button>
           </div>`
        : `<span style="font-size:.72rem;color:#5a9070">${d.status}</span>`;

      return `<tr>
        <td><strong>${esc(d.name||"—")}</strong><br><small style="color:#5a9070">${esc(d.email||"")}</small></td>
        <td><strong style="color:var(--green);font-size:1rem">₹${d.amount||0}</strong></td>
        <td>${payDetail}</td>
        <td><span class="pill ${pcls}">${d.status}</span></td>
        <td style="font-size:.72rem">${date}</td>
        <td>${btns}</td>
      </tr>`;
    }).join("");
  });
}

async function aApproveWD(id, uid, upiId, amount, name) {
  if (!confirm(`✅ Confirm you have ALREADY transferred ₹${amount} to:\n${upiId}\n\nApprove this withdrawal?`)) return;
  try {
    await db.collection("withdrawals").doc(id).update({
      status:"approved", approvedAt:firebase.firestore.FieldValue.serverTimestamp()
    });
    showToast("✅ Withdrawal approved!");
    const msg = encodeURIComponent(`✅ Health is Wealth

Withdrawal APPROVED!
👤 ${name}
💰 ₹${amount}
📲 ${upiId}

Money has been transferred.`);
    window.open(`https://wa.me/${ADMIN_WA}?text=${msg}`,"_blank");
  } catch(e) { showToast("⚠ Error: "+e.message); }
}

async function aRejectWD(id, uid, amt) {
  if (!confirm(`Reject & refund ₹${amt} to user's wallet?`)) return;
  try {
    if (uid) await db.collection("users").doc(uid).update({
      wallet:  firebase.firestore.FieldValue.increment(amt),
      balance: firebase.firestore.FieldValue.increment(amt)
    });
    await db.collection("withdrawals").doc(id).update({
      status:"rejected", rejectedAt:firebase.firestore.FieldValue.serverTimestamp()
    });
    showToast(`❌ Rejected. ₹${amt} refunded to user.`);
  } catch(e) { showToast("⚠ Error: "+e.message); }
}

// ── Admin: Payments ───────────────────────────────────────────────────────────
let payUnsub = null;
function loadAdminPay() {
  if (payUnsub) payUnsub();
  const tbody = document.getElementById("payBody");
  tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">Loading...</td></tr>';
  payUnsub = db.collection("payments").orderBy("submittedAt","desc").limit(100).onSnapshot(snap => {
    if (!snap || snap.empty) {
      tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">No payments yet.</td></tr>';
      return;
    }
    tbody.innerHTML = snap.docs.map(doc => {
      const d = doc.data();
      const date = d.submittedAt?.toDate()?.toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"numeric"})||"—";
      const pcls = d.status==="approved"||d.status==="paid"?"pill-a":d.status==="rejected"?"pill-r":"pill-p";
      const btns = d.status==="pending"
        ? `<div style="display:flex;gap:6px;flex-wrap:wrap">
            <button class="btn-a" onclick="aApprovePay('${doc.id}','${d.userId}')">✅ Approve</button>
            <button class="btn-r" onclick="aRejectPay('${doc.id}','${d.userId}')">❌ Reject</button>
           </div>`
        : `<span style="font-size:.72rem;color:#5a9070">${d.status}</span>`;
      return `<tr>
        <td style="font-size:.72rem"><strong>${esc(d.name||"—")}</strong><br><span style="color:#5a9070">${esc(d.email||"")}</span></td>
        <td><strong style="color:var(--green)">₹${d.amount||BOOK_PRICE}</strong></td>
        <td style="font-size:.72rem;font-family:monospace;font-weight:700">${esc(d.txnId||"—")}</td>
        <td><span class="pill ${pcls}">${d.status}</span></td>
        <td style="font-size:.72rem">${date}</td>
        <td>${btns}</td>
      </tr>`;
    }).join("");
  });
}

async function aApprovePay(id, userId) {
  if (!confirm("Approve payment and fully activate GPS Earn + Referral for this user?")) return;
  try {
    await db.collection("payments").doc(id).update({status:"approved", approvedAt:firebase.firestore.FieldValue.serverTimestamp()});
    await db.collection("users").doc(userId).update({
      bookPurchased:true, bookStatus:"approved", referralLinkUnlocked:true
    });
    // Credit referrer
    const userDoc  = await db.collection("users").doc(userId).get();
    const userData = userDoc.data();
    if (userData && userData.referredBy) {
      const refSnap = await db.collection("users").where("refCode","==",userData.referredBy).limit(1).get();
      if (!refSnap.empty) {
        const referrerDoc  = refSnap.docs[0];
        const referrerData = referrerDoc.data();
        if (referrerData.bookPurchased && referrerDoc.id !== userId) {
          const newKmLimit = (referrerData.totalKm||0) + KM_CAP;
          await db.collection("users").doc(referrerDoc.id).update({
            referralCount: firebase.firestore.FieldValue.increment(1),
            wallet:  firebase.firestore.FieldValue.increment(REF_REWARD),
            balance: firebase.firestore.FieldValue.increment(REF_REWARD),
            kmLimit: newKmLimit
          });
          const refRec = await db.collection("referrals").where("referrerId","==",referrerDoc.id).where("referredId","==",userId).limit(1).get();
          if (!refRec.empty) {
            await refRec.docs[0].ref.update({
              status:"paid", amount:REF_REWARD,
              paidAt:firebase.firestore.FieldValue.serverTimestamp(),
              referredName:userData.name||"User"
            });
          }
          await db.collection("km_logs").add({
            uid:referrerDoc.id, km:0, earned:REF_REWARD, type:"referral_bonus",
            referredUserId:userId, date:new Date().toISOString().slice(0,10),
            ts:firebase.firestore.FieldValue.serverTimestamp()
          });
        }
      }
    }
    showToast("✅ Payment approved! GPS Earn + Referral activated.");
  } catch(e) { showToast("⚠ Error: "+e.message); }
}

async function aRejectPay(id, userId) {
  if (!confirm("Reject this payment?")) return;
  try {
    await db.collection("payments").doc(id).update({status:"rejected", rejectedAt:firebase.firestore.FieldValue.serverTimestamp()});
    if (userId) await db.collection("users").doc(userId).update({bookStatus:"rejected"});
    showToast("❌ Payment rejected.");
  } catch(e) { showToast("⚠ Error: "+e.message); }
}

// ── Admin: Leaderboard ────────────────────────────────────────────────────────
async function loadLeaderboard() {
  const tbody = document.getElementById("lbBody");
  tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">Loading...</td></tr>';
  try {
    const snap = await db.collection("users").where("bookPurchased","==",true).orderBy("referralCount","desc").limit(50).get();
    if (snap.empty) { tbody.innerHTML='<tr><td colspan="6" style="text-align:center;padding:20px;color:#5a9070">No active users yet.</td></tr>'; return; }
    const medals=["🥇","🥈","🥉"], prizes=[PRIZE_1,PRIZE_2,PRIZE_3];
    const top = snap.docs.slice(0,3).map(d=>d.data());
    ["prize1","prize2","prize3"].forEach((id,i)=>{
      const ne=document.getElementById(id+"Name"), re=document.getElementById(id+"refs");
      if(top[i]){if(ne)ne.textContent=esc(top[i].name||"—");if(re)re.textContent=(top[i].referralCount||0)+" referrals";}
    });
    tbody.innerHTML = snap.docs.map((doc,i)=>{
      const d=doc.data();
      const prizeLabel = i<3 ? formatLakh(prizes[i]) : "—";
      return `<tr style="${i<3?"background:linear-gradient(90deg,rgba(240,180,41,.06),transparent)":""}">
        <td style="font-size:1.1rem;font-weight:800;text-align:center">${medals[i]||"#"+(i+1)}</td>
        <td><strong style="color:#071c10">${esc(d.name||"—")}</strong><div style="font-size:.68rem;color:#5a9070">${esc(d.email||"")}</div></td>
        <td><strong style="color:var(--green);font-size:1rem">${d.referralCount||0}</strong></td>
        <td>${(d.totalKm||0).toFixed(1)} km</td>
        <td><strong style="color:var(--green)">₹${(d.wallet||d.balance||0).toFixed(0)}</strong></td>
        <td><strong style="color:#6820d0;font-size:.88rem">${prizeLabel}</strong></td>
      </tr>`;
    }).join("");
  } catch(e) {
    tbody.innerHTML=`<tr><td colspan="6" style="text-align:center;padding:20px;color:var(--red)">Error: ${esc(e.message)}</td></tr>`;
  }
}

// ── Admin: Free Referral code ─────────────────────────────────────────────────
async function loadAdminRefCode() {
  try {
    const snap = await db.collection("free_referrals").where("isAdmin","==",true).where("active","==",true).limit(1).get();
    if (!snap.empty) {
      adminRefCode = snap.docs[0].data().code;
    } else {
      adminRefCode = "ADMIN" + Math.random().toString(36).slice(-5).toUpperCase();
      await db.collection("free_referrals").add({
        code:adminRefCode, isAdmin:true, active:true, createdAt:firebase.firestore.FieldValue.serverTimestamp()
      });
    }
    const display=document.getElementById("adminRefCodeDisplay"), urlDisplay=document.getElementById("adminRefUrlDisplay");
    if(display)  display.textContent    = adminRefCode;
    if(urlDisplay) urlDisplay.textContent = `${window.location.origin}${window.location.pathname}?ref=${adminRefCode}`;
  } catch(e) { console.error("loadAdminRefCode:", e); }
}
function copyAdminRef() {
  if (!adminRefCode) return;
  const url = `${window.location.origin}${window.location.pathname}?ref=${adminRefCode}`;
  if (navigator.clipboard) navigator.clipboard.writeText(url).then(()=>showToast("📋 Admin referral link copied!"));
  else showToast("Code: "+adminRefCode);
}
function shareAdminRefWA() {
  if (!adminRefCode) return;
  const url = `${window.location.origin}${window.location.pathname}?ref=${adminRefCode}`;
  window.open("https://wa.me/?text=" + encodeURIComponent("🌿 Join Health is Wealth FREE using admin code!

🔗 "+url+"

Use this link to get FREE access!"),"_blank");
}
async function generateNewAdminRef() {
  if (!confirm("Generate a new admin referral code? Old code will stop working.")) return;
  try {
    const snap = await db.collection("free_referrals").where("isAdmin","==",true).where("active","==",true).get();
    const batch = db.batch();
    snap.docs.forEach(d => batch.update(d.ref,{active:false}));
    await batch.commit();
    adminRefCode = "ADMIN" + Math.random().toString(36).slice(-5).toUpperCase();
    await db.collection("free_referrals").add({code:adminRefCode,isAdmin:true,active:true,createdAt:firebase.firestore.FieldValue.serverTimestamp()});
    const display=document.getElementById("adminRefCodeDisplay"), urlDisplay=document.getElementById("adminRefUrlDisplay");
    if(display)  display.textContent    = adminRefCode;
    if(urlDisplay) urlDisplay.textContent = `${window.location.origin}${window.location.pathname}?ref=${adminRefCode}`;
    showToast("✅ New admin referral code generated!");
  } catch(e) { showToast("⚠ Failed: "+e.message); }
}

// ── Admin: Settings ───────────────────────────────────────────────────────────
async function loadSettings() {
  try {
    const doc = await db.collection("app_settings").doc("config").get();
    if (doc.exists) {
      const d=doc.data();
      if(d.bookPrice) document.getElementById("setPrice").value=d.bookPrice;
      if(d.refReward) document.getElementById("setRefReward").value=d.refReward;
      if(d.kmRate)    document.getElementById("setKmRate").value=d.kmRate;
      if(d.kmCap)     document.getElementById("setKmCap").value=d.kmCap;
      if(d.minWD)     document.getElementById("setMinWD").value=d.minWD;
      if(d.upiId)     document.getElementById("setUpiId").value=d.upiId;
      if(d.prize1)    document.getElementById("setPrize1").value=d.prize1;
      if(d.prize2)    document.getElementById("setPrize2").value=d.prize2;
      if(d.prize3)    document.getElementById("setPrize3").value=d.prize3;
      if(d.announcement) document.getElementById("setAnnouncement").value=d.announcement;
    }
    showToast("✅ Settings loaded.");
  } catch(e) { showToast("⚠ Failed to load: "+e.message); }
}
async function saveSettings() {
  const price=parseFloat(document.getElementById("setPrice").value)||BOOK_PRICE;
  const refR=parseFloat(document.getElementById("setRefReward").value)||REF_REWARD;
  const kmR=parseFloat(document.getElementById("setKmRate").value)||KM_RATE;
  const kmC=parseInt(document.getElementById("setKmCap").value)||KM_CAP;
  const minW=parseFloat(document.getElementById("setMinWD").value)||MIN_WD;
  const upi=document.getElementById("setUpiId").value.trim()||UPI_ID;
  const p1=parseFloat(document.getElementById("setPrize1").value)||PRIZE_1;
  const p2=parseFloat(document.getElementById("setPrize2").value)||PRIZE_2;
  const p3=parseFloat(document.getElementById("setPrize3").value)||PRIZE_3;
  const ann=document.getElementById("setAnnouncement").value.trim();
  try {
    await db.collection("app_settings").doc("config").set({
      bookPrice:price,refReward:refR,kmRate:kmR,kmCap:kmC,
      minWD:minW,upiId:upi,prize1:p1,prize2:p2,prize3:p3,
      announcement:ann,updatedAt:firebase.firestore.FieldValue.serverTimestamp()
    },{merge:true});
    BOOK_PRICE=price;REF_REWARD=refR;KM_RATE=kmR;KM_CAP=kmC;
    MIN_WD=minW;UPI_ID=upi;PRIZE_1=p1;PRIZE_2=p2;PRIZE_3=p3;
    showToast("✅ Settings saved!");
  } catch(e) { showToast("⚠ Save failed: "+e.message); }
}
function sendBroadcast() {
  const msg = document.getElementById("broadcastMsg").value.trim();
  if (!msg) { showToast("⚠ Enter a message first"); return; }
  window.open(`https://wa.me/${ADMIN_WA}?text=${encodeURIComponent(msg)}`,"_blank");
}
</script>
</body>
</html>
