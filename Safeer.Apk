<!DOCTYPE html>
<html lang="id">
<head>
  <script type="module" src="camera.js"></script>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SAFEER — Pusat Kendali Keselamatan Pengemudi</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
/* ==========================================================================
   SAFEER — DESIGN SYSTEM
   Konsep visual: instrumen dasbor malam hari (night cockpit instrumentation).
   Palet dan tipografi diturunkan dari gugus kendali kendaraan sungguhan —
   layar telemetri gelap dengan sinyal sian sebagai status "terjaga",
   amber sebagai "waspada", dan merah sebagai "bahaya".
   ========================================================================== */

:root{
  /* --- warna inti (named tokens) --- */
  --ink-950: #070B14;      /* latar utama - malam gelap */
  --ink-900: #0B1220;      /* latar sekunder */
  --ink-850: #0F1830;      /* panel dasbor */
  --ink-800: #131C36;      /* kartu */
  --ink-700: #1B2745;      /* kartu hover / border kuat */
  --ink-600: #263258;      /* border halus */
  --ink-500: #3A4A78;      /* teks pudar */

  --signal-cyan: #2FE0C9;     /* status terjaga / normal */
  --signal-cyan-dim: #17847A;
  --signal-amber: #F5A623;    /* status waspada / caution */
  --signal-amber-dim: #8A5E12;
  --signal-red: #FF4D5E;      /* status bahaya / darurat */
  --signal-red-dim: #8C1F29;
  --signal-violet: #8B7CF6;   /* aksen data sekunder */

  --text-hi: #EAF0FB;
  --text-mid: #A9B7D6;
  --text-low: #6C7A9C;

  --radius-sm: 8px;
  --radius-md: 14px;
  --radius-lg: 22px;
  --radius-pill: 999px;

  --font-display: 'Space Grotesk', sans-serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --shadow-panel: 0 20px 60px -20px rgba(0,0,0,0.6);
  --shadow-glow-cyan: 0 0 40px -8px rgba(47,224,201,0.35);
  --shadow-glow-amber: 0 0 40px -8px rgba(245,166,35,0.35);
  --shadow-glow-red: 0 0 40px -8px rgba(255,77,94,0.4);

  --ease-out: cubic-bezier(.16,1,.3,1);
}

*, *::before, *::after{ box-sizing: border-box; }
html{ scroll-behavior: smooth; }
body{
  margin:0;
  min-height:100vh;
  background:
    radial-gradient(ellipse 1200px 800px at 15% -10%, rgba(47,224,201,0.07), transparent 60%),
    radial-gradient(ellipse 900px 700px at 100% 0%, rgba(139,124,246,0.06), transparent 55%),
    var(--ink-950);
  color: var(--text-hi);
  font-family: var(--font-body);
  -webkit-font-smoothing: antialiased;
  overflow-x:hidden;
}

::selection{ background: var(--signal-cyan); color:#04120F; }

::-webkit-scrollbar{ width:10px; height:10px; }
::-webkit-scrollbar-track{ background: transparent; }
::-webkit-scrollbar-thumb{ background: var(--ink-600); border-radius: 10px; border:2px solid transparent; background-clip: padding-box;}
::-webkit-scrollbar-thumb:hover{ background: var(--ink-500); background-clip: padding-box; }

a{ color:inherit; text-decoration:none; }
button{ font-family: inherit; cursor:pointer; }
ul{ list-style:none; margin:0; padding:0; }
h1,h2,h3,h4,h5{ margin:0; font-family: var(--font-display); letter-spacing:-0.01em; }
p{ margin:0; }
svg{ display:block; }

:focus-visible{
  outline: 2px solid var(--signal-cyan);
  outline-offset: 3px;
  border-radius: 4px;
}

.mono{ font-family: var(--font-mono); }
.visually-hidden{
  position:absolute !important; width:1px;height:1px;
  padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);
  white-space:nowrap;border:0;
}

@media (prefers-reduced-motion: reduce){
  *{ animation-duration: 0.001ms !important; animation-iteration-count: 1 !important; transition-duration: 0.001ms !important; scroll-behavior: auto !important; }
}

/* ==========================================================================
   TIPOGRAFI UTILITAS
   ========================================================================== */
.eyebrow{
  font-family: var(--font-mono);
  font-size: 11px;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--signal-cyan);
  display:flex; align-items:center; gap:8px;
}
.eyebrow::before{
  content:"";
  width:6px; height:6px; border-radius:50%;
  background: var(--signal-cyan);
  box-shadow: 0 0 10px 2px rgba(47,224,201,0.7);
}
.page-title{ font-size: 28px; font-weight:700; color:var(--text-hi); }
.section-title{ font-size: 18px; font-weight:600; color:var(--text-hi); }
.muted{ color: var(--text-mid); }
.faint{ color: var(--text-low); }
.text-sm{ font-size: 13px; }
.text-xs{ font-size: 11px; }

/* ==========================================================================
   SPLASH / AUTENTIKASI
   ========================================================================== */
#splash-screen{
  position: fixed; inset:0; z-index: 300;
  display:flex; align-items:center; justify-content:center;
  background:
    radial-gradient(ellipse 1000px 700px at 50% 0%, rgba(47,224,201,0.10), transparent 60%),
    var(--ink-950);
  transition: opacity .6s var(--ease-out), visibility .6s var(--ease-out);
}
#splash-screen.hidden{ opacity:0; visibility:hidden; pointer-events:none; }

.auth-shell{
  width:100%; max-width: 420px; padding: 20px;
  display:flex; flex-direction:column; align-items:center; gap:28px;
}
.brand-mark{
  display:flex; align-items:center; gap:12px;
}
.brand-mark .logo-badge{
  width:52px;height:52px;border-radius:16px;
  background: linear-gradient(155deg, var(--signal-cyan), var(--signal-cyan-dim));
  display:flex;align-items:center;justify-content:center;
  box-shadow: var(--shadow-glow-cyan);
}
.brand-mark .logo-badge svg{ width:28px;height:28px; }
.brand-mark .brand-text .name{ font-family: var(--font-display); font-weight:700; font-size:22px; letter-spacing:0.02em; }
.brand-mark .brand-text .tagline{ font-size:11px; color: var(--text-mid); letter-spacing:0.08em; text-transform:uppercase; }

.radar-loader{
  width: 140px; height:140px; position:relative; margin: 6px 0 4px;
}
.radar-loader svg{ width:100%; height:100%; }
.radar-loader .sweep{
  transform-origin: 70px 70px;
  animation: radar-spin 2.4s linear infinite;
}
@keyframes radar-spin{ to{ transform: rotate(360deg);} }
.radar-loader .ring{ stroke: var(--ink-600); }
.radar-loader .blip{ animation: blip-pulse 1.8s var(--ease-out) infinite; }
@keyframes blip-pulse{
  0%,100%{ opacity:.35; r:2.4; }
  50%{ opacity:1; r:4; }
}

.auth-card{
  width:100%;
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700);
  border-radius: var(--radius-lg);
  padding: 28px 26px;
  box-shadow: var(--shadow-panel);
}
.auth-card h1{ font-size:20px; margin-bottom:6px; }
.auth-card .sub{ color:var(--text-mid); font-size:13px; margin-bottom:22px; }

.field{ margin-bottom:16px; }
.field label{
  display:block; font-size:12px; color: var(--text-mid);
  margin-bottom:7px; font-weight:600; letter-spacing:0.02em;
}
.field input, .field select{
  width:100%;
  background: var(--ink-900);
  border:1px solid var(--ink-600);
  color: var(--text-hi);
  padding: 12px 14px;
  border-radius: var(--radius-sm);
  font-size: 14px;
  font-family: inherit;
  transition: border-color .2s, box-shadow .2s;
}
.field input:focus, .field select:focus{
  border-color: var(--signal-cyan);
  box-shadow: 0 0 0 3px rgba(47,224,201,0.15);
  outline:none;
}
.field input::placeholder{ color: var(--text-low); }

.role-toggle{
  display:flex; gap:8px; margin-bottom:20px;
  background: var(--ink-900); padding:4px; border-radius: var(--radius-pill);
  border:1px solid var(--ink-600);
}
.role-toggle button{
  flex:1; border:none; background:transparent; color: var(--text-mid);
  padding: 9px 10px; border-radius: var(--radius-pill); font-size:12.5px; font-weight:600;
  transition: all .2s var(--ease-out);
}
.role-toggle button.active{
  background: linear-gradient(135deg, var(--signal-cyan), var(--signal-cyan-dim));
  color: #04140F;
}

.btn{
  display:inline-flex; align-items:center; justify-content:center; gap:8px;
  border:none; border-radius: var(--radius-sm);
  padding: 12px 18px; font-weight:600; font-size:14px;
  transition: transform .15s var(--ease-out), box-shadow .2s, background .2s, opacity .2s;
  white-space:nowrap;
}
.btn:active{ transform: translateY(1px) scale(.99); }
.btn-primary{
  background: linear-gradient(135deg, var(--signal-cyan), var(--signal-cyan-dim));
  color:#04140F;
  box-shadow: var(--shadow-glow-cyan);
  width:100%;
}
.btn-primary:hover{ filter: brightness(1.08); }
.btn-ghost{
  background: var(--ink-800); color: var(--text-hi); border:1px solid var(--ink-600);
}
.btn-ghost:hover{ background: var(--ink-700); }
.btn-outline-danger{
  background:transparent; border:1px solid var(--signal-red); color: var(--signal-red);
}
.btn-outline-danger:hover{ background: rgba(255,77,94,0.1); }
.btn-sm{ padding: 8px 12px; font-size:12.5px; border-radius:8px; }
.btn-icon{ width:36px; height:36px; padding:0; border-radius:10px; }

.auth-foot{
  margin-top:16px; text-align:center; font-size:12.5px; color: var(--text-low);
}
.auth-foot button{ background:none;border:none;color:var(--signal-cyan); font-weight:600; }

.auth-demo-note{
  margin-top: 18px; font-size: 11.5px; color: var(--text-low); text-align:center;
  border-top: 1px dashed var(--ink-600); padding-top:14px; line-height:1.6;
}

/* ==========================================================================
   APP SHELL — SIDEBAR + TOPBAR + LAYOUT
   ========================================================================== */
#app-shell{
  display:none;
  grid-template-columns: 264px 1fr;
  min-height: 100vh;
}
#app-shell.active{ display:grid; }

.sidebar{
  background: linear-gradient(180deg, var(--ink-900), var(--ink-950));
  border-right: 1px solid var(--ink-700);
  padding: 22px 16px;
  display:flex; flex-direction:column;
  position: sticky; top:0; height:100vh;
}
.sidebar .brand-mark{ padding: 4px 8px 22px; }
.sidebar .brand-mark .logo-badge{ width:40px;height:40px;border-radius:12px; }
.sidebar .brand-mark .logo-badge svg{ width:22px;height:22px; }
.sidebar .brand-mark .name{ font-size:17px; }

.nav-group{ margin-bottom: 18px; }
.nav-group .nav-label{
  font-size:10.5px; letter-spacing:0.14em; text-transform:uppercase;
  color: var(--text-low); padding: 0 12px; margin-bottom:8px; font-weight:700;
}
.nav-item{
  display:flex; align-items:center; gap:12px;
  padding: 10px 12px; border-radius: 10px;
  color: var(--text-mid); font-size:13.5px; font-weight:600;
  border:none; background:transparent; width:100%; text-align:left;
  transition: background .18s, color .18s;
  position:relative;
}
.nav-item svg{ width:18px;height:18px; flex-shrink:0; opacity:.85; }
.nav-item:hover{ background: var(--ink-800); color: var(--text-hi); }
.nav-item.active{
  background: linear-gradient(135deg, rgba(47,224,201,0.16), rgba(47,224,201,0.03));
  color: var(--signal-cyan);
  box-shadow: inset 0 0 0 1px rgba(47,224,201,0.25);
}
.nav-item.active svg{ opacity:1; }
.nav-item .nav-badge{
  margin-left:auto; background: var(--signal-red); color:#fff;
  font-size:10px; font-weight:700; padding:2px 7px; border-radius: var(--radius-pill);
}

.sidebar-foot{
  margin-top:auto; padding-top:16px; border-top:1px solid var(--ink-700);
}
.mini-profile{
  display:flex; align-items:center; gap:10px; padding:8px;
  border-radius:12px; transition: background .2s;
}
.mini-profile:hover{ background: var(--ink-800); }
.mini-profile .avatar{
  width:36px;height:36px;border-radius:10px;
  background: linear-gradient(135deg, var(--signal-violet), #5A4CC4);
  display:flex;align-items:center;justify-content:center;
  font-weight:700; font-size:13px; font-family: var(--font-display);
}
.mini-profile .who{ line-height:1.3; }
.mini-profile .who .n{ font-size:13px; font-weight:600; }
.mini-profile .who .r{ font-size:11px; color: var(--text-low); }

.main-col{ display:flex; flex-direction:column; min-width:0; }

.topbar{
  display:flex; align-items:center; justify-content:space-between;
  padding: 16px 28px; border-bottom:1px solid var(--ink-700);
  position: sticky; top:0; z-index:20;
  background: rgba(7,11,20,0.82); backdrop-filter: blur(14px);
}
.topbar-left{ display:flex; flex-direction:column; gap:2px; }
.topbar-left .crumb{ font-size:11px; color: var(--text-low); letter-spacing:.05em; text-transform:uppercase; }
.topbar-left .title{ font-family: var(--font-display); font-size:19px; font-weight:700; }
.topbar-right{ display:flex; align-items:center; gap:12px; }

.device-status-pill{
  display:flex; align-items:center; gap:8px;
  background: var(--ink-800); border:1px solid var(--ink-600);
  padding: 7px 12px; border-radius: var(--radius-pill);
  font-size:12px; color: var(--text-mid);
}
.device-status-pill .dot{
  width:8px;height:8px;border-radius:50%; background: var(--signal-cyan);
  box-shadow: 0 0 8px 1px rgba(47,224,201,0.8);
  animation: pulse-dot 2s ease-in-out infinite;
}
@keyframes pulse-dot{ 0%,100%{opacity:1;} 50%{opacity:.4;} }

.icon-btn{
  width:38px;height:38px;border-radius:10px;
  background: var(--ink-800); border:1px solid var(--ink-600);
  display:flex;align-items:center;justify-content:center;
  color: var(--text-mid); position:relative;
  transition: background .2s, color .2s;
}
.icon-btn:hover{ background: var(--ink-700); color: var(--text-hi); }
.icon-btn svg{ width:18px;height:18px; }
.icon-btn .ping{
  position:absolute; top:-3px; right:-3px;
  width:10px;height:10px;border-radius:50%;
  background: var(--signal-red); border:2px solid var(--ink-950);
}

.page{ display:none; padding: 26px 28px 60px; animation: page-in .4s var(--ease-out); }
.page.active{ display:block; }
@keyframes page-in{ from{ opacity:0; transform: translateY(6px);} to{opacity:1; transform:translateY(0);} }

.page-head{
  display:flex; align-items:flex-end; justify-content:space-between;
  margin-bottom:22px; gap:16px; flex-wrap:wrap;
}
.page-head .sub{ color: var(--text-mid); font-size:13.5px; margin-top:6px; max-width:560px; }
.page-head-actions{ display:flex; gap:10px; }

/* ==========================================================================
   GRID + CARD PRIMITIF
   ========================================================================== */
.grid{ display:grid; gap:18px; }
.grid-cols-4{ grid-template-columns: repeat(4, 1fr); }
.grid-cols-3{ grid-template-columns: repeat(3, 1fr); }
.grid-cols-2{ grid-template-columns: repeat(2, 1fr); }
.grid-span-2{ grid-column: span 2; }
.grid-span-3{ grid-column: span 3; }

.dash-main-grid{
  display:grid;
  grid-template-columns: 380px 1fr;
  gap: 20px;
  margin-bottom:20px;
}

.panel{
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border: 1px solid var(--ink-700);
  border-radius: var(--radius-lg);
  padding: 22px;
  box-shadow: var(--shadow-panel);
}
.panel-flush{ padding:0; overflow:hidden; }
.panel-head{
  display:flex; align-items:center; justify-content:space-between;
  margin-bottom:16px;
}
.panel-head .htitle{ display:flex; align-items:center; gap:8px; font-size:14.5px; font-weight:700; }
.panel-head .htitle svg{ width:16px;height:16px; color:var(--signal-cyan); }

.stat-card{
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700); border-radius: var(--radius-md);
  padding: 18px 20px; position:relative; overflow:hidden;
}
.stat-card::after{
  content:""; position:absolute; right:-30px; top:-30px;
  width:110px;height:110px; border-radius:50%;
  background: radial-gradient(circle, rgba(47,224,201,0.10), transparent 70%);
}
.stat-card .label{ font-size:12px; color:var(--text-mid); font-weight:600; display:flex; align-items:center; gap:7px;}
.stat-card .label svg{ width:14px;height:14px; }
.stat-card .value{ font-family: var(--font-display); font-size:30px; font-weight:700; margin-top:10px; }
.stat-card .value .unit{ font-size:14px; color: var(--text-mid); font-weight:500; margin-left:4px;}
.stat-card .delta{ margin-top:8px; font-size:12px; display:flex; align-items:center; gap:5px; }
.stat-card .delta.up{ color: var(--signal-cyan); }
.stat-card .delta.down{ color: var(--signal-red); }
.stat-card .delta.flat{ color: var(--text-low); }

.badge{
  display:inline-flex; align-items:center; gap:6px;
  font-size:11.5px; font-weight:700; padding:5px 11px; border-radius: var(--radius-pill);
  letter-spacing:.02em;
}
.badge::before{ content:""; width:6px;height:6px;border-radius:50%; }
.badge-safe{ background: rgba(47,224,201,0.12); color: var(--signal-cyan); }
.badge-safe::before{ background: var(--signal-cyan); }
.badge-warn{ background: rgba(245,166,35,0.14); color: var(--signal-amber); }
.badge-warn::before{ background: var(--signal-amber); }
.badge-danger{ background: rgba(255,77,94,0.14); color: var(--signal-red); }
.badge-danger::before{ background: var(--signal-red); }
.badge-neutral{ background: var(--ink-700); color: var(--text-mid); }
.badge-neutral::before{ background: var(--text-low); }

/* ==========================================================================
   RADAR KEWASPADAAN — elemen signature aplikasi.
   Visualisasi sapuan radar melingkar merepresentasikan siklus pemindaian
   AI terhadap indikator kelelahan (mata, kepala, pandangan) secara berkala.
   ========================================================================== */
.radar-wrap{
  display:flex; flex-direction:column; align-items:center; gap:14px;
  padding: 6px 0 4px;
}
.radar-frame{
  position:relative; width:280px; height:280px;
}
.radar-frame svg{ width:100%; height:100%; }
.radar-ring{ fill:none; stroke: var(--ink-600); stroke-width:1; }
.radar-ring.mid{ stroke: var(--ink-700); stroke-dasharray:2 5; }
.radar-crosshair{ stroke: var(--ink-700); stroke-width:1; }
.radar-sweep-group{
  transform-origin: 140px 140px;
  animation: radar-spin 3.6s linear infinite;
}
.radar-sweep-gradient-fill{ opacity:.55; }
.radar-blip{ animation: blip-pulse 2.2s var(--ease-out) infinite; }
.radar-center-readout{
  position:absolute; inset:0; display:flex; flex-direction:column;
  align-items:center; justify-content:center; pointer-events:none;
}
.radar-center-readout .score{ font-family: var(--font-display); font-size:44px; font-weight:700; }
.radar-center-readout .score-label{ font-size:11px; color: var(--text-mid); text-transform:uppercase; letter-spacing:.14em; margin-top:2px;}
.radar-status-line{
  display:flex; align-items:center; gap:8px; font-size:13px; font-weight:700;
  padding: 8px 16px; border-radius: var(--radius-pill);
}
.radar-status-line.safe{ background: rgba(47,224,201,0.12); color: var(--signal-cyan); }
.radar-status-line.warn{ background: rgba(245,166,35,0.14); color: var(--signal-amber); }
.radar-status-line.danger{ background: rgba(255,77,94,0.15); color: var(--signal-red); animation: alert-flash 1.1s ease-in-out infinite; }
@keyframes alert-flash{ 0%,100%{ box-shadow: 0 0 0 0 rgba(255,77,94,0.4);} 50%{ box-shadow: 0 0 0 8px rgba(255,77,94,0);} }

.rec-banner{
  margin-top: 16px; display:flex; align-items:flex-start; gap:12px;
  padding: 14px 16px; border-radius: var(--radius-md);
  background: rgba(245,166,35,0.08); border:1px solid rgba(245,166,35,0.35);
}
.rec-banner.ok{ background: rgba(47,224,201,0.07); border-color: rgba(47,224,201,0.3); }
.rec-banner svg{ width:20px;height:20px; color: var(--signal-amber); flex-shrink:0; margin-top:1px;}
.rec-banner.ok svg{ color: var(--signal-cyan); }
.rec-banner .t{ font-size:13px; font-weight:700; margin-bottom:2px; }
.rec-banner .d{ font-size:12.5px; color: var(--text-mid); line-height:1.5; }

/* Indikator kelelahan — grid mini metrik */
.indicator-grid{ display:grid; grid-template-columns: repeat(2,1fr); gap:12px; margin-top:18px; }
.indicator-item{
  background: var(--ink-900); border:1px solid var(--ink-700); border-radius: var(--radius-md);
  padding:14px 15px;
}
.indicator-item .ihead{ display:flex; align-items:center; justify-content:space-between; margin-bottom:10px;}
.indicator-item .ihead .lbl{ font-size:12px; color:var(--text-mid); font-weight:600; display:flex; gap:7px; align-items:center;}
.indicator-item .ihead .lbl svg{ width:14px;height:14px; }
.indicator-item .ival{ font-family: var(--font-mono); font-size:20px; font-weight:700; }
.indicator-item .ival .u{ font-size:11px; color:var(--text-low); font-weight:500; }
.meter{ height:6px; border-radius:4px; background: var(--ink-700); overflow:hidden; margin-top:9px;}
.meter > span{ display:block; height:100%; border-radius:4px; transition: width .5s var(--ease-out); }
.meter.c-safe > span{ background: linear-gradient(90deg, var(--signal-cyan-dim), var(--signal-cyan)); }
.meter.c-warn > span{ background: linear-gradient(90deg, var(--signal-amber-dim), var(--signal-amber)); }
.meter.c-danger > span{ background: linear-gradient(90deg, var(--signal-red-dim), var(--signal-red)); }

/* Trip status strip */
.trip-strip{
  display:flex; align-items:center; justify-content:space-between;
  background: var(--ink-900); border:1px solid var(--ink-700);
  border-radius: var(--radius-md); padding:14px 18px; margin-bottom:18px;
  flex-wrap:wrap; gap:14px;
}
.trip-strip .ts-item{ display:flex; flex-direction:column; gap:3px; }
.trip-strip .ts-item .l{ font-size:10.5px; color:var(--text-low); text-transform:uppercase; letter-spacing:.08em; }
.trip-strip .ts-item .v{ font-family: var(--font-mono); font-size:15px; font-weight:600; }
.trip-strip .divider{ width:1px; height:30px; background: var(--ink-700); }

/* Driving score circular */
.score-ring-wrap{ position:relative; width:120px;height:120px; margin:0 auto; }
.score-ring-wrap svg{ width:100%;height:100%; transform: rotate(-90deg); }
.score-ring-track{ fill:none; stroke: var(--ink-700); stroke-width:9; }
.score-ring-value{ fill:none; stroke-width:9; stroke-linecap:round; transition: stroke-dashoffset 1s var(--ease-out); }
.score-ring-label{
  position:absolute; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center;
}
.score-ring-label .n{ font-family: var(--font-display); font-size:26px; font-weight:700; }
.score-ring-label .s{ font-size:9.5px; color:var(--text-low); text-transform:uppercase; }

/* ==========================================================================
   TABEL — riwayat perjalanan, log peringatan, daftar armada
   ========================================================================== */
.table-toolbar{
  display:flex; align-items:center; justify-content:space-between;
  padding: 16px 20px; border-bottom:1px solid var(--ink-700); flex-wrap:wrap; gap:12px;
}
.search-box{
  display:flex; align-items:center; gap:9px;
  background: var(--ink-900); border:1px solid var(--ink-600); border-radius: var(--radius-sm);
  padding: 9px 13px; min-width:230px;
}
.search-box svg{ width:15px;height:15px; color:var(--text-low); flex-shrink:0; }
.search-box input{ background:none; border:none; color:var(--text-hi); font-size:13px; width:100%; }
.search-box input:focus{ outline:none; }
.filter-chip-row{ display:flex; gap:8px; flex-wrap:wrap; }
.filter-chip{
  font-size:12px; padding:7px 13px; border-radius: var(--radius-pill);
  background: var(--ink-900); border:1px solid var(--ink-600); color: var(--text-mid); font-weight:600;
}
.filter-chip.active{ background: rgba(47,224,201,0.14); border-color: var(--signal-cyan); color: var(--signal-cyan); }

table.data-table{ width:100%; border-collapse:collapse; }
.data-table thead th{
  text-align:left; font-size:11px; text-transform:uppercase; letter-spacing:.08em;
  color: var(--text-low); font-weight:700; padding: 12px 20px; border-bottom:1px solid var(--ink-700);
  background: var(--ink-900);
}
.data-table tbody td{
  padding: 14px 20px; font-size:13px; border-bottom:1px solid var(--ink-800); color: var(--text-hi);
}
.data-table tbody tr{ transition: background .15s; }
.data-table tbody tr:hover{ background: rgba(47,224,201,0.03); }
.data-table tbody tr:last-child td{ border-bottom:none; }
.cell-strong{ font-weight:700; }
.cell-mono{ font-family: var(--font-mono); font-size:12.5px; color: var(--text-mid); }
.row-avatar-name{ display:flex; align-items:center; gap:10px; }
.row-avatar{
  width:32px;height:32px;border-radius:9px; flex-shrink:0;
  display:flex;align-items:center;justify-content:center; font-weight:700; font-size:12px;
  font-family: var(--font-display);
}
.mini-bar-track{ width:80px; height:6px; border-radius:4px; background: var(--ink-700); overflow:hidden; display:inline-block; vertical-align:middle; margin-right:8px;}
.mini-bar-fill{ height:100%; border-radius:4px; display:block; }
.table-pagination{
  display:flex; align-items:center; justify-content:space-between;
  padding: 14px 20px; border-top:1px solid var(--ink-700); font-size:12.5px; color: var(--text-mid);
}
.pg-btns{ display:flex; gap:6px; }
.pg-btn{
  width:30px;height:30px; border-radius:8px; background: var(--ink-900); border:1px solid var(--ink-600);
  color: var(--text-mid); font-size:12px; display:flex; align-items:center; justify-content:center;
}
.pg-btn.active{ background: var(--signal-cyan); color:#04140F; border-color: var(--signal-cyan); font-weight:700; }

/* ==========================================================================
   CHART CANVAS CONTAINERS
   ========================================================================== */
.chart-card{
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700); border-radius: var(--radius-lg); padding:22px;
}
.chart-legend{ display:flex; gap:16px; flex-wrap:wrap; margin-top:14px; }
.chart-legend .li{ display:flex; align-items:center; gap:7px; font-size:12px; color: var(--text-mid); }
.chart-legend .sw{ width:10px;height:10px;border-radius:3px; }
canvas{ max-width:100%; }

.donut-wrap{ position:relative; display:flex; align-items:center; justify-content:center; }
.donut-center{ position:absolute; text-align:center; }
.donut-center .n{ font-family: var(--font-display); font-size:28px; font-weight:700; }
.donut-center .s{ font-size:10.5px; color: var(--text-low); text-transform:uppercase; }

/* ==========================================================================
   LAPORAN KESELAMATAN — kartu laporan
   ========================================================================== */
.report-card{
  display:flex; gap:16px; padding:18px; border-radius: var(--radius-md);
  background: var(--ink-900); border:1px solid var(--ink-700); margin-bottom:12px;
  align-items:flex-start; transition: border-color .2s, transform .2s;
}
.report-card:hover{ border-color: var(--ink-600); transform: translateY(-1px); }
.report-icon{
  width:44px;height:44px;border-radius:12px; flex-shrink:0;
  display:flex; align-items:center; justify-content:center;
}
.report-icon.sev-low{ background: rgba(47,224,201,0.12); color:var(--signal-cyan); }
.report-icon.sev-mid{ background: rgba(245,166,35,0.14); color:var(--signal-amber); }
.report-icon.sev-high{ background: rgba(255,77,94,0.14); color:var(--signal-red); }
.report-icon svg{ width:22px;height:22px; }
.report-body{ flex:1; min-width:0; }
.report-body .rtop{ display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap; }
.report-body h4{ font-size:14.5px; font-weight:700; }
.report-body .rdate{ font-size:11.5px; color: var(--text-low); font-family: var(--font-mono); }
.report-body p{ font-size:12.5px; color: var(--text-mid); margin-top:6px; line-height:1.55; }
.report-actions{ display:flex; gap:8px; margin-top:12px; }

/* ==========================================================================
   DASHBOARD ARMADA (B2B)
   ========================================================================== */
.fleet-driver-card{
  background: var(--ink-900); border:1px solid var(--ink-700); border-radius: var(--radius-md);
  padding:16px; display:flex; flex-direction:column; gap:12px; transition: border-color .2s, transform .2s;
}
.fleet-driver-card:hover{ border-color: var(--signal-cyan); transform: translateY(-2px); }
.fleet-driver-top{ display:flex; align-items:center; gap:12px; }
.fleet-driver-top .info{ flex:1; min-width:0; }
.fleet-driver-top .info .n{ font-size:13.5px; font-weight:700; }
.fleet-driver-top .info .id{ font-size:11px; color: var(--text-low); font-family: var(--font-mono); }
.status-dot-lg{ width:11px;height:11px;border-radius:50%; flex-shrink:0; }
.status-dot-lg.safe{ background: var(--signal-cyan); box-shadow:0 0 8px 1px rgba(47,224,201,.7); }
.status-dot-lg.warn{ background: var(--signal-amber); box-shadow:0 0 8px 1px rgba(245,166,35,.7); }
.status-dot-lg.danger{ background: var(--signal-red); box-shadow:0 0 8px 1px rgba(255,77,94,.7); animation: pulse-dot 1s infinite; }
.fleet-driver-meta{ display:grid; grid-template-columns: 1fr 1fr; gap:8px; }
.fleet-driver-meta .m{ background: var(--ink-800); border-radius:8px; padding:8px 10px; }
.fleet-driver-meta .m .l{ font-size:9.5px; color:var(--text-low); text-transform:uppercase; letter-spacing:.05em; }
.fleet-driver-meta .m .v{ font-family: var(--font-mono); font-size:13px; font-weight:600; margin-top:2px; }

.fleet-map-shell{
  height: 340px; border-radius: var(--radius-lg); position:relative; overflow:hidden;
  background:
    linear-gradient(rgba(38,50,88,.55) 1px, transparent 1px) 0 0/32px 32px,
    linear-gradient(90deg, rgba(38,50,88,.55) 1px, transparent 1px) 0 0/32px 32px,
    var(--ink-900);
  border:1px solid var(--ink-700);
}
.fleet-map-pin{
  position:absolute; width:14px;height:14px;border-radius:50% 50% 50% 0;
  transform: rotate(-45deg); display:flex; align-items:center; justify-content:center;
}
.fleet-map-pin.safe{ background: var(--signal-cyan); }
.fleet-map-pin.warn{ background: var(--signal-amber); }
.fleet-map-pin.danger{ background: var(--signal-red); }
.fleet-map-note{
  position:absolute; bottom:14px; left:14px; background: rgba(7,11,20,0.75); backdrop-filter: blur(6px);
  padding:8px 12px; border-radius:10px; font-size:11px; color: var(--text-mid); border:1px solid var(--ink-700);
}

/* ==========================================================================
   NOTIFIKASI DARURAT
   ========================================================================== */
.emergency-hero{
  border-radius: var(--radius-lg); padding: 26px;
  background: linear-gradient(135deg, rgba(255,77,94,0.14), rgba(255,77,94,0.02));
  border: 1px solid rgba(255,77,94,0.35);
  display:flex; align-items:center; justify-content:space-between; gap:20px; flex-wrap:wrap;
}
.emergency-hero .l{ max-width:520px; }
.emergency-hero h2{ font-size:20px; margin-bottom:6px; }
.emergency-hero p{ color: var(--text-mid); font-size:13px; line-height:1.6; }

.contact-item{
  display:flex; align-items:center; gap:12px; padding:14px; border-radius: var(--radius-md);
  background: var(--ink-900); border:1px solid var(--ink-700); margin-bottom:10px;
}
.contact-item .avatar{
  width:38px;height:38px;border-radius:10px; background: var(--ink-700);
  display:flex;align-items:center;justify-content:center; font-weight:700; font-family: var(--font-display); font-size:13px;
}
.contact-item .info{ flex:1; }
.contact-item .info .n{ font-size:13.5px; font-weight:700; }
.contact-item .info .r{ font-size:11.5px; color: var(--text-low); }

.timeline{ position:relative; padding-left:26px; }
.timeline::before{ content:""; position:absolute; left:6px; top:6px; bottom:6px; width:1px; background: var(--ink-700); }
.timeline-item{ position:relative; margin-bottom:20px; }
.timeline-item::before{
  content:""; position:absolute; left:-26px; top:3px; width:11px;height:11px;border-radius:50%;
  border:2px solid var(--ink-950);
}
.timeline-item.danger::before{ background: var(--signal-red); }
.timeline-item.warn::before{ background: var(--signal-amber); }
.timeline-item.safe::before{ background: var(--signal-cyan); }
.timeline-item .tt{ font-size:13px; font-weight:700; }
.timeline-item .td{ font-size:12px; color: var(--text-mid); margin-top:3px; }
.timeline-item .tm{ font-size:11px; color: var(--text-low); font-family: var(--font-mono); margin-top:4px; }

/* ==========================================================================
   PENGATURAN — form & toggle
   ========================================================================== */
.settings-row{
  display:flex; align-items:center; justify-content:space-between; gap:16px;
  padding: 16px 0; border-bottom:1px solid var(--ink-800);
}
.settings-row:last-child{ border-bottom:none; }
.settings-row .l .t{ font-size:13.5px; font-weight:600; }
.settings-row .l .d{ font-size:12px; color: var(--text-mid); margin-top:3px; max-width:420px; line-height:1.5;}

.switch{ position:relative; width:44px; height:25px; flex-shrink:0; }
.switch input{ opacity:0; width:0; height:0; }
.switch .track{
  position:absolute; inset:0; background: var(--ink-700); border-radius: var(--radius-pill);
  transition: background .2s; cursor:pointer;
}
.switch .track::before{
  content:""; position:absolute; width:19px;height:19px; border-radius:50%; left:3px; top:3px;
  background: var(--text-hi); transition: transform .2s var(--ease-out);
}
.switch input:checked + .track{ background: var(--signal-cyan-dim); }
.switch input:checked + .track::before{ transform: translateX(19px); background:#04140F; }

.device-pair-card{
  display:flex; align-items:center; gap:16px; padding:18px; border-radius: var(--radius-md);
  background: var(--ink-900); border:1px solid var(--ink-700);
}
.device-pair-card .dicon{
  width:52px;height:52px;border-radius:14px; background: var(--ink-800);
  display:flex;align-items:center;justify-content:center; color: var(--signal-cyan);
}
.device-pair-card .dicon svg{ width:26px;height:26px; }

/* ==========================================================================
   MODAL & TOAST
   ========================================================================== */
.modal-overlay{
  position:fixed; inset:0; background: rgba(4,7,14,0.7); backdrop-filter: blur(4px);
  display:none; align-items:center; justify-content:center; z-index:200; padding:20px;
}
.modal-overlay.active{ display:flex; animation: fade-in .2s var(--ease-out); }
@keyframes fade-in{ from{opacity:0;} to{opacity:1;} }
.modal-box{
  width:100%; max-width:480px; background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700); border-radius: var(--radius-lg); padding:24px;
  box-shadow: var(--shadow-panel); animation: modal-in .25s var(--ease-out);
}
@keyframes modal-in{ from{ opacity:0; transform: translateY(12px) scale(.98);} to{opacity:1; transform:translateY(0) scale(1);} }
.modal-box .mhead{ display:flex; align-items:center; justify-content:space-between; margin-bottom:14px; }
.modal-box .mhead h3{ font-size:17px; }
.modal-close{ width:30px;height:30px; border-radius:8px; background: var(--ink-700); border:none; color:var(--text-mid); display:flex; align-items:center; justify-content:center;}
.modal-box .mbody{ font-size:13.5px; color: var(--text-mid); line-height:1.6; }
.modal-box .mfoot{ display:flex; justify-content:flex-end; gap:10px; margin-top:20px; }

.toast-stack{
  position:fixed; top:20px; right:20px; z-index:250; display:flex; flex-direction:column; gap:10px;
  width: 340px; max-width: calc(100vw - 40px);
}
.toast{
  display:flex; align-items:flex-start; gap:11px; padding:14px 15px;
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700); border-left:3px solid var(--signal-cyan);
  border-radius: var(--radius-md); box-shadow: var(--shadow-panel);
  animation: toast-in .3s var(--ease-out);
}
.toast.warn{ border-left-color: var(--signal-amber); }
.toast.danger{ border-left-color: var(--signal-red); }
@keyframes toast-in{ from{ opacity:0; transform: translateX(30px);} to{ opacity:1; transform:translateX(0);} }
.toast svg{ width:18px;height:18px; flex-shrink:0; margin-top:1px; color: var(--signal-cyan); }
.toast.warn svg{ color: var(--signal-amber); }
.toast.danger svg{ color: var(--signal-red); }
.toast .tt{ font-size:13px; font-weight:700; }
.toast .td{ font-size:12px; color: var(--text-mid); margin-top:2px; }
.toast .tx{ margin-left:auto; background:none; border:none; color: var(--text-low); flex-shrink:0; }

/* ==========================================================================
   RESPONSIVE
   ========================================================================== */
@media (max-width: 1180px){
  .dash-main-grid{ grid-template-columns: 1fr; }
  .grid-cols-4{ grid-template-columns: repeat(2,1fr); }
  .grid-cols-3{ grid-template-columns: repeat(2,1fr); }
}
@media (max-width: 880px){
  #app-shell{ grid-template-columns: 1fr; }
  .sidebar{ position:fixed; left:-280px; z-index:100; width:264px; transition: left .25s var(--ease-out); box-shadow: 20px 0 60px rgba(0,0,0,.5);}
  .sidebar.open{ left:0; }
  .grid-cols-4, .grid-cols-3, .grid-cols-2{ grid-template-columns: 1fr; }
  .topbar{ padding:14px 16px; }
  .page{ padding:18px 16px 50px; }
  .fleet-driver-meta{ grid-template-columns: 1fr 1fr; }
}
.menu-toggle{ display:none; }
@media (max-width: 880px){ .menu-toggle{ display:flex; } }

/* ==========================================================================
   KOMPONEN TAMBAHAN — panel edukasi, empty state, skeleton, cetak
   ========================================================================== */
.info-explainer-grid{ display:grid; grid-template-columns: repeat(5,1fr); gap:12px; margin-top:18px; }
.info-explainer-item{
  background: var(--ink-900); border:1px solid var(--ink-700); border-radius: var(--radius-md);
  padding:14px; text-align:left;
}
.info-explainer-item .iico{
  width:34px;height:34px;border-radius:9px; background: rgba(47,224,201,0.12); color: var(--signal-cyan);
  display:flex;align-items:center;justify-content:center; margin-bottom:10px;
}
.info-explainer-item .iico svg{ width:17px;height:17px; }
.info-explainer-item .it{ font-size:12.5px; font-weight:700; margin-bottom:4px; }
.info-explainer-item .id{ font-size:11.5px; color: var(--text-mid); line-height:1.5; }
@media (max-width: 1180px){ .info-explainer-grid{ grid-template-columns: repeat(2,1fr); } }

.empty-state{
  display:flex; flex-direction:column; align-items:center; justify-content:center;
  padding: 50px 20px; text-align:center; color: var(--text-mid);
}
.empty-state svg{ width:44px;height:44px; color: var(--text-low); margin-bottom:14px; }
.empty-state .et{ font-size:14px; font-weight:700; color: var(--text-hi); margin-bottom:6px; }
.empty-state .ed{ font-size:12.5px; max-width:320px; line-height:1.6; }

.skeleton{
  background: linear-gradient(90deg, var(--ink-800) 25%, var(--ink-700) 37%, var(--ink-800) 63%);
  background-size: 400% 100%;
  animation: skeleton-shimmer 1.4s ease infinite;
  border-radius: 8px;
}
@keyframes skeleton-shimmer{ 0%{ background-position: 100% 50%; } 100%{ background-position: 0 50%; } }

.dropdown-menu{
  position:absolute; top:calc(100% + 8px); right:0; min-width:190px;
  background: linear-gradient(180deg, var(--ink-800), var(--ink-850));
  border:1px solid var(--ink-700); border-radius: var(--radius-md);
  box-shadow: var(--shadow-panel); padding:6px; z-index:60; display:none;
}
.dropdown-menu.open{ display:block; animation: modal-in .18s var(--ease-out); }
.dropdown-menu button{
  width:100%; text-align:left; background:none; border:none; color: var(--text-mid);
  font-size:12.5px; padding:9px 10px; border-radius:8px; font-weight:600;
}
.dropdown-menu button:hover{ background: var(--ink-700); color: var(--text-hi); }

.kbd{
  font-family: var(--font-mono); font-size:10.5px; padding:2px 6px; border-radius:5px;
  background: var(--ink-700); border:1px solid var(--ink-600); color: var(--text-mid);
}

.form-error{ font-size:11.5px; color: var(--signal-red); margin-top:6px; display:none; }
.field.has-error input{ border-color: var(--signal-red); }
.field.has-error .form-error{ display:block; }

@media print{
  .sidebar, .topbar, .page-head-actions, .btn{ display:none !important; }
  body{ background:#fff; color:#000; }
}

/* ==========================================================================
   TOOLTIP & AKORDION BANTUAN
   ========================================================================== */
[data-tooltip]{ position:relative; }
[data-tooltip]::after{
  content: attr(data-tooltip);
  position:absolute; bottom:calc(100% + 8px); left:50%; transform: translateX(-50%);
  background: var(--ink-700); color: var(--text-hi); font-size:11px; font-weight:600;
  padding:6px 10px; border-radius:7px; white-space:nowrap;
  opacity:0; pointer-events:none; transition: opacity .15s, transform .15s;
  box-shadow: 0 8px 20px rgba(0,0,0,.4); z-index:80;
}
[data-tooltip]:hover::after{ opacity:1; transform: translateX(-50%) translateY(-2px); }

.accordion-item{
  border:1px solid var(--ink-700); border-radius: var(--radius-md); margin-bottom:10px; overflow:hidden;
  background: var(--ink-900);
}
.accordion-head{
  width:100%; display:flex; align-items:center; justify-content:space-between;
  padding:15px 16px; background:none; border:none; color: var(--text-hi); font-size:13.5px; font-weight:700;
}
.accordion-head svg{ width:16px;height:16px; color: var(--text-mid); transition: transform .25s var(--ease-out); }
.accordion-item.open .accordion-head svg{ transform: rotate(180deg); }
.accordion-body{
  max-height:0; overflow:hidden; transition: max-height .3s var(--ease-out);
}
.accordion-body-inner{ padding: 0 16px 16px; font-size:12.5px; color: var(--text-mid); line-height:1.65; }

.sparkline-row{ display:flex; align-items:center; gap:8px; margin-top:10px; }
.sparkline-row canvas{ flex-shrink:0; }
</style>
</head>
<body>

<!-- ==========================================================================
     SPLASH / LOGIN SCREEN
     ========================================================================== -->
<div id="splash-screen">
  <div class="auth-shell">
    <div class="brand-mark">
      <div class="logo-badge">
        <svg viewBox="0 0 24 24" fill="none"><path d="M12 2L4 5v6c0 5.2 3.4 9.4 8 11 4.6-1.6 8-5.8 8-11V5l-8-3z" stroke="#04140F" stroke-width="1.8" stroke-linejoin="round" fill="#04140F" fill-opacity="0.12"/><path d="M8.5 12.2l2.4 2.4 4.6-4.9" stroke="#04140F" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <div class="brand-text">
        <div class="name">SAFEER</div>
        <div class="tagline">Driver Monitoring Control Center</div>
      </div>
    </div>

    <div class="radar-loader">
      <svg viewBox="0 0 140 140">
        <circle class="ring" cx="70" cy="70" r="66" stroke-width="1"/>
        <circle class="ring" cx="70" cy="70" r="46" stroke-width="1"/>
        <circle class="ring" cx="70" cy="70" r="24" stroke-width="1"/>
        <line class="radar-crosshair" x1="70" y1="4" x2="70" y2="136"/>
        <line class="radar-crosshair" x1="4" y1="70" x2="136" y2="70"/>
        <g class="sweep">
          <path d="M70 70 L70 4 A66 66 0 0 1 116.7 23.3 Z" fill="url(#sweepGrad)" opacity="0.6"/>
        </g>
        <circle class="blip" cx="94" cy="46" r="3" fill="#2FE0C9"/>
        <circle class="blip" cx="50" cy="94" r="2.6" fill="#2FE0C9" style="animation-delay:.6s"/>
        <defs>
          <linearGradient id="sweepGrad" x1="70" y1="70" x2="116.7" y2="23.3" gradientUnits="userSpaceOnUse">
            <stop offset="0" stop-color="#2FE0C9" stop-opacity="0.55"/>
            <stop offset="1" stop-color="#2FE0C9" stop-opacity="0"/>
          </linearGradient>
        </defs>
      </svg>
    </div>

    <div class="auth-card">
      <h1>Masuk ke SAFEER</h1>
      <p class="sub">Pantau kondisi kelelahan pengemudi dan skor berkendara secara real-time.</p>

      <div class="role-toggle" id="roleToggle">
        <button class="active" data-role="driver" type="button">Pengemudi</button>
        <button data-role="fleet" type="button">Admin Armada (B2B)</button>
      </div>

      <div class="field">
        <label for="loginEmail">Alamat email</label>
        <input id="loginEmail" type="email" placeholder="nama@perusahaan.com" value="andra.pratama@safeer.id">
      </div>
      <div class="field">
        <label for="loginPass">Kata sandi</label>
        <input id="loginPass" type="password" placeholder="••••••••" value="safeer2026">
      </div>

      <button class="btn btn-primary" id="btnLogin" type="button">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none"><path d="M5 12h14M13 6l6 6-6 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
        Masuk ke Pusat Kendali
      </button>

      <div class="auth-foot">
        Belum memiliki perangkat? <button type="button" id="btnDemo">Coba mode demo</button>
      </div>
      <div class="auth-demo-note">
        Prototipe antarmuka SAFEER — seluruh data kelelahan, skor berkendara, dan riwayat perjalanan pada tampilan ini adalah data simulasi untuk keperluan demonstrasi.
      </div>
    </div>
  </div>
</div>

<!-- ==========================================================================
     TOAST STACK
     ========================================================================== -->
<div class="toast-stack" id="toastStack"></div>

<!-- ==========================================================================
     APP SHELL
     ========================================================================== -->
<div id="app-shell">
  <aside class="sidebar" id="sidebar">
    <div class="brand-mark">
      <div class="logo-badge">
        <svg viewBox="0 0 24 24" fill="none"><path d="M12 2L4 5v6c0 5.2 3.4 9.4 8 11 4.6-1.6 8-5.8 8-11V5l-8-3z" stroke="#04140F" stroke-width="1.8" stroke-linejoin="round" fill="#04140F" fill-opacity="0.12"/><path d="M8.5 12.2l2.4 2.4 4.6-4.9" stroke="#04140F" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <div class="brand-text">
        <div class="name">SAFEER</div>
        <div class="tagline">Control Center</div>
      </div>
    </div>

    <div class="nav-group">
      <div class="nav-label">Pemantauan</div>
      <ul>
        <li><button class="nav-item active" data-page="dashboard" type="button">
          <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.8"/><path d="M12 7v5l3.2 2" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
          Dasbor Real-time
        </button></li>
        <li><button class="nav-item" data-page="history" type="button">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 6h16M4 12h16M4 18h10" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
          Riwayat Perjalanan
        </button></li>
        <li><button class="nav-item" data-page="analytics" type="button">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 20V10M11 20V4M18 20v-7" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
          Statistik &amp; Performa
        </button></li>
        <li><button class="nav-item" data-page="reports" type="button">
          <svg viewBox="0 0 24 24" fill="none"><path d="M7 3h8l4 4v14H7z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/><path d="M9 12h6M9 16h6" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
          Laporan Keselamatan
        </button></li>
      </ul>
    </div>

    <div class="nav-group">
      <div class="nav-label">Armada &amp; Darurat</div>
      <ul>
        <li><button class="nav-item" data-page="fleet" type="button">
          <svg viewBox="0 0 24 24" fill="none"><rect x="3" y="7" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.8"/><rect x="14" y="7" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.8"/><rect x="8.5" y="15" width="7" height="5" rx="1.5" stroke="currentColor" stroke-width="1.8"/></svg>
          Dasbor Armada (B2B)
        </button></li>
        <li><button class="nav-item" data-page="emergency" type="button">
          <svg viewBox="0 0 24 24" fill="none"><path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/><path d="M12 10v4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/><circle cx="12" cy="17" r="0.9" fill="currentColor"/></svg>
          Notifikasi Darurat
          <span class="nav-badge" id="emergencyBadge">2</span>
        </button></li>
      </ul>
    </div>

    <div class="nav-group">
      <div class="nav-label">Akun</div>
      <ul>
        <li><button class="nav-item" data-page="settings" type="button">
          <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.8"/><path d="M19.4 13a7.6 7.6 0 000-2l2-1.5-2-3.4-2.3.9a7.6 7.6 0 00-1.7-1L15 3.5h-4l-.4 2.5a7.6 7.6 0 00-1.7 1l-2.3-.9-2 3.4L6.6 11a7.6 7.6 0 000 2l-2 1.5 2 3.4 2.3-.9a7.6 7.6 0 001.7 1l.4 2.5h4l.4-2.5a7.6 7.6 0 001.7-1l2.3.9 2-3.4-2-1.5z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/></svg>
          Pengaturan
        </button></li>
      </ul>
    </div>

    <div class="sidebar-foot">
      <button class="mini-profile" id="btnLogout" type="button" style="width:100%; border:none; background:none; cursor:pointer;">
        <div class="avatar" id="profileAvatar">AP</div>
        <div class="who" style="text-align:left;">
          <div class="n" id="profileName">Andra Pratama</div>
          <div class="r" id="profileRole">Pengemudi Aktif</div>
        </div>
      </button>
    </div>
  </aside>

  <div class="main-col">
    <header class="topbar">
      <div class="topbar-left">
        <button class="icon-btn menu-toggle" id="menuToggle" type="button" style="margin-right:10px;">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 7h16M4 12h16M4 17h16" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
        </button>
        <span class="crumb" id="pageCrumb">Pemantauan / Real-time</span>
        <span class="title" id="pageTopTitle">Dasbor Real-time</span>
      </div>
      <div class="topbar-right">
        <div class="device-status-pill">
          <span class="dot"></span>
          Perangkat AI DMS-04 · Terhubung
        </div>
        <button class="icon-btn" type="button" title="Notifikasi">
          <svg viewBox="0 0 24 24" fill="none"><path d="M6 9a6 6 0 1112 0c0 4 1.5 5.5 2 6.5H4C4.5 14.5 6 13 6 9z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/><path d="M10 19a2 2 0 004 0" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg>
          <span class="ping"></span>
        </button>
        <button class="icon-btn" type="button" title="Bantuan">
          <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/><path d="M9.5 9.3a2.5 2.5 0 114 2c-.9.6-1.5 1.1-1.5 2.2" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/><circle cx="12" cy="17" r="0.8" fill="currentColor"/></svg>
        </button>
      </div>
    </header>

    <main id="pageContainer">

      <!-- ==================================================================
           PAGE: DASHBOARD REAL-TIME
           ================================================================== -->
           
      <section class="page active" id="page-dashboard">
        <div class="page-head">
          <div>
            <div class="eyebrow">Pemindaian Aktif</div>
            <h1 class="page-title">Dasbor Pemantauan Real-time</h1>
            <p class="sub">Analisis kondisi kelelahan pengemudi diperbarui setiap detik oleh perangkat AI Driver Monitoring System yang terpasang pada dasbor kendaraan.</p>
          </div>
          <div class="page-head-actions">
            <button class="btn btn-ghost btn-sm" id="btnToggleTrip" type="button">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M5 4l14 8-14 8V4z" fill="currentColor"/></svg>
              Mulai Perjalanan
            </button>
            <button class="btn btn-outline-danger btn-sm" id="btnSimAlert" type="button">Simulasikan Kelelahan</button>
          </div>
        </div>
        <div class="camera-container">
          <video id="video" autoplay playsinline></video>
          <canvas id="canvas"></canvas>
        </div>
        <div class="trip-strip">
          <div class="ts-item"><span class="l">Status Perjalanan</span><span class="v" id="tripStatusVal">Belum Dimulai</span></div>
          <div class="divider"></div>
          <div class="ts-item"><span class="l">Durasi</span><span class="v mono" id="tripDuration">00:00:00</span></div>
          <div class="divider"></div>
          <div class="ts-item"><span class="l">Jarak Tempuh</span><span class="v mono" id="tripDistance">0.0 km</span></div>
          <div class="divider"></div>
          <div class="ts-item"><span class="l">Kecepatan</span><span class="v mono" id="tripSpeed">0 km/j</span></div>
          <div class="divider"></div>
          <div class="ts-item"><span class="l">Lokasi Terakhir</span><span class="v" id="tripLoc">Surabaya, Jawa Timur</span></div>
        </div>

        <div class="dash-main-grid">
          <div class="panel">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.8"/></svg>
                Radar Kewaspadaan
              </div>
              <span class="badge badge-safe" id="scanFreqBadge">Pindai 1×/dtk</span>
            </div>

            <div class="radar-wrap">
              <div class="radar-frame">
                <svg viewBox="0 0 280 280">
                  <circle class="radar-ring" cx="140" cy="140" r="132"/>
                  <circle class="radar-ring mid" cx="140" cy="140" r="98"/>
                  <circle class="radar-ring mid" cx="140" cy="140" r="64"/>
                  <circle class="radar-ring mid" cx="140" cy="140" r="30"/>
                  <line class="radar-crosshair" x1="140" y1="8" x2="140" y2="272"/>
                  <line class="radar-crosshair" x1="8" y1="140" x2="272" y2="140"/>
                  <g class="radar-sweep-group">
                    <path d="M140 140 L140 8 A132 132 0 0 1 224 44 Z" fill="url(#sweepGradMain)" class="radar-sweep-gradient-fill"/>
                  </g>
                  <circle class="radar-blip" cx="176" cy="76" r="3.4" fill="#2FE0C9"/>
                  <circle class="radar-blip" cx="98" cy="188" r="2.8" fill="#2FE0C9" style="animation-delay:.5s"/>
                  <circle class="radar-blip" cx="200" cy="176" r="2.4" fill="#F5A623" style="animation-delay:1s"/>
                  <defs>
                    <linearGradient id="sweepGradMain" x1="140" y1="140" x2="224" y2="44" gradientUnits="userSpaceOnUse">
                      <stop offset="0" stop-color="#2FE0C9" stop-opacity="0.5"/>
                      <stop offset="1" stop-color="#2FE0C9" stop-opacity="0"/>
                    </linearGradient>
                  </defs>
                </svg>
                <div class="radar-center-readout">
                  <div class="score" id="alertnessScore">92</div>
                  <div class="score-label">Skor Kewaspadaan</div>
                </div>
              </div>
              <div class="radar-status-line safe" id="alertnessStatusLine">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M20 6L9 17l-5-5" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg>
                Pengemudi dalam kondisi terjaga
              </div>
            </div>

            <div class="rec-banner ok" id="recBanner">
              <svg viewBox="0 0 24 24" fill="none"><path d="M12 8v5M12 16h.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg>
              <div>
                <div class="t" id="recTitle">Kondisi baik, lanjutkan perjalanan</div>
                <div class="d" id="recDesc">Tidak ada indikasi kelelahan terdeteksi dalam 10 menit terakhir. Pertahankan pola istirahat setiap 2 jam.</div>
              </div>
            </div>

            <div class="panel-head" style="margin-top:22px;">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/><path d="M12 8v4l3 2" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg>
                Cara Kerja Deteksi AI
              </div>
            </div>
            <div class="info-explainer-grid" style="grid-template-columns:1fr;">
              <div class="info-explainer-item">
                <div class="iico"><svg viewBox="0 0 24 24" fill="none"><path d="M2 12s3.5-6 10-6 10 6 10 6-3.5 6-10 6-10-6-10-6z" stroke="currentColor" stroke-width="1.6"/></svg></div>
                <div class="it">Frekuensi Kedipan</div>
                <div class="id">Menghitung jumlah kedipan mata per menit. Penurunan tajam menandakan mata mulai berat.</div>
              </div>
              <div class="info-explainer-item">
                <div class="iico"><svg viewBox="0 0 24 24" fill="none"><path d="M12 3v3M12 18v3M3 12h3M18 12h3" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg></div>
                <div class="it">Durasi Mata Tertutup</div>
                <div class="id">Mengukur lamanya mata tertutup pada tiap kedipan untuk mendeteksi microsleep.</div>
              </div>
              <div class="info-explainer-item">
                <div class="iico"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg></div>
                <div class="it">Aktivitas Menguap</div>
                <div class="id">Mendeteksi pola bukaan mulut yang khas dari gerakan menguap berulang.</div>
              </div>
              <div class="info-explainer-item">
                <div class="iico"><svg viewBox="0 0 24 24" fill="none"><rect x="6" y="4" width="12" height="16" rx="4" stroke="currentColor" stroke-width="1.6"/></svg></div>
                <div class="it">Posisi Kepala</div>
                <div class="id">Melacak kemiringan dan gerakan kepala untuk mendeteksi tanda mengantuk atau menunduk.</div>
              </div>
              <div class="info-explainer-item">
                <div class="iico"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/></svg></div>
                <div class="it">Arah Pandangan</div>
                <div class="id">Memastikan fokus pandangan tetap tertuju ke jalan, bukan ke perangkat lain di kabin.</div>
              </div>
            </div>
          </div>

          <div style="display:flex; flex-direction:column; gap:20px;">
            <div class="panel">
              <div class="panel-head">
                <div class="htitle">
                  <svg viewBox="0 0 24 24" fill="none"><path d="M2 12s3.5-6 10-6 10 6 10 6-3.5 6-10 6-10-6-10-6z" stroke="currentColor" stroke-width="1.8"/><circle cx="12" cy="12" r="2.6" stroke="currentColor" stroke-width="1.8"/></svg>
                  Indikator Kelelahan AI
                </div>
                <span class="badge badge-neutral">5 indikator</span>
              </div>

              <div class="indicator-grid">
                <div class="indicator-item">
                  <div class="ihead">
                    <span class="lbl"><svg viewBox="0 0 24 24" fill="none"><path d="M2 12s3.5-6 10-6 10 6 10 6-3.5 6-10 6-10-6-10-6z" stroke="currentColor" stroke-width="1.6"/></svg>Frekuensi Kedipan</span>
                  </div>
                  <div class="ival" id="metricBlink">16 <span class="u">kedip/mnt</span></div>
                  <div class="meter c-safe"><span id="meterBlink" style="width:78%"></span></div>
                </div>
                <div class="indicator-item">
                  <div class="ihead">
                    <span class="lbl"><svg viewBox="0 0 24 24" fill="none"><path d="M12 3v3M12 18v3M3 12h3M18 12h3" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg>Durasi Mata Tertutup</span>
                  </div>
                  <div class="ival" id="metricEyeClose">0.21 <span class="u">detik</span></div>
                  <div class="meter c-safe"><span id="meterEyeClose" style="width:22%"></span></div>
                </div>
                <div class="indicator-item">
                  <div class="ihead">
                    <span class="lbl"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg>Aktivitas Menguap</span>
                  </div>
                  <div class="ival" id="metricYawn">1 <span class="u">/ 10 mnt</span></div>
                  <div class="meter c-safe"><span id="meterYawn" style="width:15%"></span></div>
                </div>
                <div class="indicator-item">
                  <div class="ihead">
                    <span class="lbl"><svg viewBox="0 0 24 24" fill="none"><rect x="6" y="4" width="12" height="16" rx="4" stroke="currentColor" stroke-width="1.6"/></svg>Posisi Kepala</span>
                  </div>
                  <div class="ival" id="metricHead">Stabil</div>
                  <div class="meter c-safe"><span id="meterHead" style="width:88%"></span></div>
                </div>
                <div class="indicator-item grid-span-2" style="grid-column:span 2;">
                  <div class="ihead">
                    <span class="lbl"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/><path d="M2 12s3.5-6 10-6 10 6 10 6-3.5 6-10 6-10-6-10-6z" stroke="currentColor" stroke-width="1.6"/></svg>Arah Pandangan (Gaze Direction)</span>
                  </div>
                  <div class="ival" id="metricGaze">Fokus ke Jalan</div>
                  <div class="meter c-safe"><span id="meterGaze" style="width:91%"></span></div>
                </div>
              </div>
            </div>

            <div class="grid grid-cols-2">
              <div class="panel">
                <div class="panel-head" style="margin-bottom:8px;">
                  <div class="htitle">
                    <svg viewBox="0 0 24 24" fill="none"><path d="M12 2v20M4 6l16 12M20 6L4 18" stroke="currentColor" stroke-width="1.4"/></svg>
                    Skor Berkendara
                  </div>
                </div>
                <div class="score-ring-wrap">
                  <svg viewBox="0 0 120 120">
                    <circle class="score-ring-track" cx="60" cy="60" r="50"/>
                    <circle class="score-ring-value" id="scoreRingCircle" cx="60" cy="60" r="50" stroke="#2FE0C9" stroke-dasharray="314" stroke-dashoffset="40"/>
                  </svg>
                  <div class="score-ring-label">
                    <div class="n" id="drivingScoreVal">87</div>
                    <div class="s">dari 100</div>
                  </div>
                </div>
                <p class="text-sm muted" style="text-align:center; margin-top:12px;">Dihitung dari rata-rata kewaspadaan sepanjang perjalanan aktif.</p>
              </div>
              <div class="panel">
                <div class="panel-head" style="margin-bottom:8px;">
                  <div class="htitle">
                    <svg viewBox="0 0 24 24" fill="none"><path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/></svg>
                    Rekomendasi Istirahat
                  </div>
                </div>
                <p class="text-sm muted" style="line-height:1.6;">Waktu berkendara berkelanjutan saat ini:</p>
                <div class="mono" style="font-size:26px;font-weight:700;margin:8px 0;" id="continuousDriveTime">01:42:00</div>
                <p class="text-sm" style="color:var(--signal-amber);" id="restSuggestion">Disarankan istirahat dalam 18 menit ke depan.</p>
                <button class="btn btn-ghost btn-sm" style="width:100%; margin-top:12px;" type="button" id="btnFindRest">Cari Titik Istirahat Terdekat</button>
              </div>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-4">
          <div class="stat-card">
            <div class="label"><svg viewBox="0 0 24 24" fill="none"><path d="M4 19V5M4 5l6 5-6 5" stroke="currentColor" stroke-width="1.6"/></svg>Total Perjalanan Bulan Ini</div>
            <div class="value">24<span class="unit">kali</span></div>
            <div class="delta up">▲ 12% dari bulan lalu</div>
            <div class="sparkline-row"><canvas class="spark" id="sparkTrips" width="140" height="34"></canvas></div>
          </div>
          <div class="stat-card">
            <div class="label"><svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg>Rata-rata Kewaspadaan</div>
            <div class="value">89<span class="unit">/100</span></div>
            <div class="delta up">▲ 4 poin</div>
            <div class="sparkline-row"><canvas class="spark" id="sparkAlertness" width="140" height="34"></canvas></div>
          </div>
          <div class="stat-card">
            <div class="label"><svg viewBox="0 0 24 24" fill="none"><path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.6"/></svg>Peringatan Kelelahan</div>
            <div class="value">3<span class="unit">kejadian</span></div>
            <div class="delta down">▼ 2 dari bulan lalu</div>
            <div class="sparkline-row"><canvas class="spark" id="sparkWarnings" width="140" height="34"></canvas></div>
          </div>
          <div class="stat-card">
            <div class="label"><svg viewBox="0 0 24 24" fill="none"><path d="M4 6h16M4 12h16M4 18h10" stroke="currentColor" stroke-width="1.6"/></svg>Total Jarak Tempuh</div>
            <div class="value">1.240<span class="unit">km</span></div>
            <div class="delta flat">— stabil</div>
            <div class="sparkline-row"><canvas class="spark" id="sparkDistance" width="140" height="34"></canvas></div>
          </div>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: RIWAYAT PERJALANAN
           ================================================================== -->
      <section class="page" id="page-history">
        <div class="page-head">
          <div>
            <div class="eyebrow">Arsip Perjalanan</div>
            <h1 class="page-title">Riwayat Perjalanan</h1>
            <p class="sub">Seluruh perjalanan tercatat otomatis lengkap dengan rata-rata kewaspadaan, skor berkendara, dan status keselamatan.</p>
          </div>
          <div class="page-head-actions">
            <button class="btn btn-ghost btn-sm" type="button">
              <svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M12 3v13M7 11l5 5 5-5M5 21h14" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              Ekspor Laporan
            </button>
          </div>
        </div>

        <div class="grid grid-cols-4" style="margin-bottom:20px;">
          <div class="stat-card"><div class="label">Perjalanan Tercatat</div><div class="value" id="histTotalTrips">24</div></div>
          <div class="stat-card"><div class="label">Total Durasi</div><div class="value">58<span class="unit">jam</span></div></div>
          <div class="stat-card"><div class="label">Skor Rata-rata</div><div class="value">86<span class="unit">/100</span></div></div>
          <div class="stat-card"><div class="label">Perjalanan Aman</div><div class="value">21<span class="unit">/24</span></div></div>
        </div>

        <div class="panel panel-flush">
          <div class="table-toolbar">
            <div class="search-box">
              <svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="7" stroke="currentColor" stroke-width="1.8"/><path d="M21 21l-4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
              <input type="text" id="historySearch" placeholder="Cari berdasarkan rute atau tanggal…">
            </div>
            <div class="filter-chip-row" id="historyFilters">
              <button class="filter-chip active" data-filter="all" type="button">Semua</button>
              <button class="filter-chip" data-filter="safe" type="button">Aman</button>
              <button class="filter-chip" data-filter="warn" type="button">Waspada</button>
              <button class="filter-chip" data-filter="danger" type="button">Berisiko</button>
            </div>
          </div>
          <div style="overflow-x:auto;">
            <table class="data-table">
              <thead>
                <tr>
                  <th>Tanggal &amp; Rute</th>
                  <th>Durasi</th>
                  <th>Jarak</th>
                  <th>Kewaspadaan</th>
                  <th>Skor Berkendara</th>
                  <th>Status</th>
                  <th></th>
                </tr>
              </thead>
              <tbody id="historyTableBody"></tbody>
            </table>
          </div>
          <div class="table-pagination">
            <span id="historyPageInfo">Menampilkan 1–8 dari 24 perjalanan</span>
            <div class="pg-btns">
              <button class="pg-btn" type="button">‹</button>
              <button class="pg-btn active" type="button">1</button>
              <button class="pg-btn" type="button">2</button>
              <button class="pg-btn" type="button">3</button>
              <button class="pg-btn" type="button">›</button>
            </div>
          </div>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: STATISTIK & PERFORMA
           ================================================================== -->
      <section class="page" id="page-analytics">
        <div class="page-head">
          <div>
            <div class="eyebrow">Analitik Kewaspadaan</div>
            <h1 class="page-title">Statistik &amp; Performa Berkendara</h1>
            <p class="sub">Pola kelelahan dan skor berkendara dirangkum dari seluruh data perjalanan untuk membantu Anda memahami kebiasaan mengemudi.</p>
          </div>
          <div class="filter-chip-row">
            <button class="filter-chip active" type="button">7 Hari</button>
            <button class="filter-chip" type="button">30 Hari</button>
            <button class="filter-chip" type="button">90 Hari</button>
          </div>
        </div>

        <div class="grid grid-cols-2" style="margin-bottom:20px;">
          <div class="chart-card grid-span-2" style="grid-column: span 2;">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><path d="M4 20V10M11 20V4M18 20v-7" stroke="currentColor" stroke-width="1.8"/></svg>
                Tren Skor Kewaspadaan Mingguan
              </div>
              <span class="badge badge-safe">Rata-rata 88.4</span>
            </div>
            <canvas id="chartAlertnessTrend" height="220"></canvas>
            <div class="chart-legend">
              <div class="li"><span class="sw" style="background:#2FE0C9;"></span>Skor Kewaspadaan</div>
              <div class="li"><span class="sw" style="background:#8B7CF6;"></span>Skor Berkendara</div>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-2">
          <div class="chart-card">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><rect x="4" y="10" width="4" height="10" stroke="currentColor" stroke-width="1.6"/><rect x="10" y="5" width="4" height="15" stroke="currentColor" stroke-width="1.6"/><rect x="16" y="13" width="4" height="7" stroke="currentColor" stroke-width="1.6"/></svg>
                Kejadian Kelelahan berdasarkan Jenis
              </div>
            </div>
            <canvas id="chartFatigueEvents" height="240"></canvas>
            <div class="chart-legend">
              <div class="li"><span class="sw" style="background:#F5A623;"></span>Menguap Berlebih</div>
              <div class="li"><span class="sw" style="background:#FF4D5E;"></span>Mata Tertutup Lama</div>
              <div class="li"><span class="sw" style="background:#8B7CF6;"></span>Kepala Menunduk</div>
              <div class="li"><span class="sw" style="background:#3A4A78;"></span>Pandangan Teralih</div>
            </div>
          </div>

          <div class="chart-card">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="8" stroke="currentColor" stroke-width="1.6"/></svg>
                Distribusi Skor Berkendara
              </div>
            </div>
            <div class="donut-wrap">
              <canvas id="chartScoreDonut" width="220" height="220"></canvas>
              <div class="donut-center">
                <div class="n">86</div>
                <div class="s">Rata-rata</div>
              </div>
            </div>
            <div class="chart-legend" style="justify-content:center;">
              <div class="li"><span class="sw" style="background:#2FE0C9;"></span>Sangat Baik (90–100)</div>
              <div class="li"><span class="sw" style="background:#8B7CF6;"></span>Baik (75–89)</div>
              <div class="li"><span class="sw" style="background:#F5A623;"></span>Cukup (60–74)</div>
              <div class="li"><span class="sw" style="background:#FF4D5E;"></span>Perlu Perhatian (&lt;60)</div>
            </div>
          </div>
        </div>

        <div class="panel" style="margin-top:20px;">
          <div class="panel-head">
            <div class="htitle">
              <svg viewBox="0 0 24 24" fill="none"><path d="M4 12h4l3-8 4 16 3-8h4" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/></svg>
              Pola Jam Berisiko
            </div>
            <span class="badge badge-warn">Puncak 13.00–15.00</span>
          </div>
          <canvas id="chartHourlyRisk" height="140"></canvas>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: LAPORAN KESELAMATAN
           ================================================================== -->
      <section class="page" id="page-reports">
        <div class="page-head">
          <div>
            <div class="eyebrow">Dokumentasi Otomatis</div>
            <h1 class="page-title">Laporan Keselamatan</h1>
            <p class="sub">Laporan tersimpan otomatis setiap kali sistem AI mendeteksi indikasi kelelahan atau perilaku berkendara berisiko.</p>
          </div>
          <div class="page-head-actions">
            <button class="btn btn-ghost btn-sm" type="button">Unduh Semua (PDF)</button>
          </div>
        </div>

        <div class="grid grid-cols-3" style="margin-bottom:20px;">
          <div class="stat-card"><div class="label">Laporan Bulan Ini</div><div class="value">9</div></div>
          <div class="stat-card"><div class="label">Tingkat Risiko Tinggi</div><div class="value" style="color:var(--signal-red);">2</div></div>
          <div class="stat-card"><div class="label">Waktu Respons Rata-rata</div><div class="value">4.2<span class="unit">dtk</span></div></div>
        </div>

        <div class="filter-chip-row" id="reportFilters" style="margin-bottom:16px;">
          <button class="filter-chip active" data-sev="all" type="button">Semua Tingkat</button>
          <button class="filter-chip" data-sev="sev-high" type="button">Risiko Tinggi</button>
          <button class="filter-chip" data-sev="sev-mid" type="button">Risiko Sedang</button>
          <button class="filter-chip" data-sev="sev-low" type="button">Informasi</button>
        </div>

        <div id="reportsList"></div>

        <div class="panel" style="margin-top:20px;">
          <div class="panel-head"><div class="htitle">Legenda Tingkat Risiko</div></div>
          <div class="grid grid-cols-3">
            <div style="display:flex; gap:10px; align-items:flex-start;">
              <span class="badge badge-safe">Aman</span>
              <span class="text-xs muted">Kewaspadaan di atas 80/100, tidak diperlukan tindakan tambahan.</span>
            </div>
            <div style="display:flex; gap:10px; align-items:flex-start;">
              <span class="badge badge-warn">Waspada</span>
              <span class="text-xs muted">Kewaspadaan 55–79/100, disarankan meningkatkan frekuensi istirahat.</span>
            </div>
            <div style="display:flex; gap:10px; align-items:flex-start;">
              <span class="badge badge-danger">Berisiko</span>
              <span class="text-xs muted">Kewaspadaan di bawah 55/100, notifikasi darurat dikirim otomatis.</span>
            </div>
          </div>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: DASBOR ARMADA (B2B)
           ================================================================== -->
      <section class="page" id="page-fleet">
        <div class="page-head">
          <div>
            <div class="eyebrow">Mode Perusahaan · B2B</div>
            <h1 class="page-title">Dasbor Pemantauan Armada</h1>
            <p class="sub">Pantau kondisi seluruh pengemudi secara bersamaan sebagai dasar evaluasi kinerja dan pengambilan keputusan operasional.</p>
          </div>
          <div class="page-head-actions">
            <button class="btn btn-ghost btn-sm" type="button">Kelola Pengemudi</button>
            <button class="btn btn-primary btn-sm" style="width:auto;" type="button">Unduh Laporan Armada</button>
          </div>
        </div>

        <div class="grid grid-cols-4" style="margin-bottom:20px;">
          <div class="stat-card">
            <div class="label"><svg viewBox="0 0 24 24" fill="none"><rect x="3" y="7" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.6"/><rect x="14" y="7" width="7" height="7" rx="1.5" stroke="currentColor" stroke-width="1.6"/></svg>Total Armada</div>
            <div class="value">18<span class="unit">unit</span></div>
          </div>
          <div class="stat-card">
            <div class="label" style="color:var(--signal-cyan);">● Kondisi Aman</div>
            <div class="value">14</div>
          </div>
          <div class="stat-card">
            <div class="label" style="color:var(--signal-amber);">● Perlu Diawasi</div>
            <div class="value">3</div>
          </div>
          <div class="stat-card">
            <div class="label" style="color:var(--signal-red);">● Status Darurat</div>
            <div class="value">1</div>
          </div>
        </div>

        <div class="grid" style="grid-template-columns: 1fr 380px; margin-bottom:20px;">
          <div class="panel">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><path d="M12 21s7-6.5 7-12a7 7 0 10-14 0c0 5.5 7 12 7 12z" stroke="currentColor" stroke-width="1.6"/><circle cx="12" cy="9" r="2.4" stroke="currentColor" stroke-width="1.6"/></svg>
                Peta Sebaran Armada Real-time
              </div>
              <span class="badge badge-safe">18 unit terhubung</span>
            </div>
            <div class="fleet-map-shell" id="fleetMap"></div>
          </div>
          <div class="panel">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.6"/></svg>
                Log Peringatan Terbaru
              </div>
            </div>
            <div class="timeline" id="fleetAlertTimeline"></div>
          </div>
        </div>

        <div class="panel">
          <div class="panel-head">
            <div class="htitle">
              <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="8" r="3.4" stroke="currentColor" stroke-width="1.6"/><path d="M4 20c0-3.9 3.6-6 8-6s8 2.1 8 6" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg>
              Status Pengemudi
            </div>
            <div class="search-box" style="min-width:200px;">
              <svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="7" stroke="currentColor" stroke-width="1.8"/><path d="M21 21l-4-4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/></svg>
              <input type="text" id="fleetSearch" placeholder="Cari nama pengemudi…">
            </div>
            <div class="filter-chip-row" id="fleetStatusFilters">
              <button class="filter-chip active" data-status="all" type="button">Semua</button>
              <button class="filter-chip" data-status="safe" type="button">Aman</button>
              <button class="filter-chip" data-status="warn" type="button">Perlu Perhatian</button>
              <button class="filter-chip" data-status="danger" type="button">Darurat</button>
            </div>
          </div>
          <div class="grid grid-cols-3" id="fleetDriverGrid"></div>
          <div class="empty-state" id="fleetEmptyState" style="display:none;">
            <svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="7" stroke="currentColor" stroke-width="1.6"/><path d="M21 21l-4-4" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/></svg>
            <div class="et">Tidak ada pengemudi ditemukan</div>
            <div class="ed">Coba ubah kata kunci pencarian atau pilih filter status yang berbeda.</div>
          </div>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: NOTIFIKASI DARURAT
           ================================================================== -->
      <section class="page" id="page-emergency">
        <div class="page-head">
          <div>
            <div class="eyebrow">Respons Darurat</div>
            <h1 class="page-title">Notifikasi Darurat</h1>
            <p class="sub">Sistem akan mengirim notifikasi otomatis kepada kontak darurat atau administrator armada saat kelelahan tingkat tinggi terdeteksi.</p>
          </div>
        </div>

        <div class="emergency-hero" style="margin-bottom:22px;">
          <div class="l">
            <h2>Sistem Peringatan Darurat Aktif</h2>
            <p>Ambang batas deteksi disetel pada tingkat kewaspadaan di bawah 45/100 atau durasi mata tertutup melebihi 1.5 detik. Notifikasi akan dikirim ke kontak darurat dan administrator armada secara bersamaan.</p>
          </div>
          <button class="btn btn-outline-danger" id="btnTestAlert" type="button">Uji Kirim Notifikasi</button>
        </div>

        <div class="panel" style="margin:20px 0;">
          <div class="panel-head"><div class="htitle">Alur Eskalasi Bertingkat</div></div>
          <div class="grid grid-cols-3">
            <div style="display:flex; gap:12px; align-items:flex-start;">
              <span class="badge badge-safe" style="flex-shrink:0;">1</span>
              <div>
                <div class="text-sm" style="font-weight:700;">Peringatan di Kabin</div>
                <div class="text-xs muted">Getaran kursi dan suara peringatan aktif seketika saat kelelahan terdeteksi.</div>
              </div>
            </div>
            <div style="display:flex; gap:12px; align-items:flex-start;">
              <span class="badge badge-warn" style="flex-shrink:0;">2</span>
              <div>
                <div class="text-sm" style="font-weight:700;">Konfirmasi Pengemudi</div>
                <div class="text-xs muted">Pengemudi diberi 8 detik untuk merespons sebelum eskalasi berlanjut.</div>
              </div>
            </div>
            <div style="display:flex; gap:12px; align-items:flex-start;">
              <span class="badge badge-danger" style="flex-shrink:0;">3</span>
              <div>
                <div class="text-sm" style="font-weight:700;">Notifikasi Eksternal</div>
                <div class="text-xs muted">Kontak darurat dan administrator armada menerima notifikasi otomatis.</div>
              </div>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-2">
          <div class="panel">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><path d="M4 6l8 6 8-6M4 6h16v12H4V6z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/></svg>
                Kontak Darurat
              </div>
              <button class="btn btn-ghost btn-sm" type="button">+ Tambah</button>
            </div>
            <div id="emergencyContactsList"></div>
          </div>

          <div class="panel">
            <div class="panel-head">
              <div class="htitle">
                <svg viewBox="0 0 24 24" fill="none"><path d="M12 8v5M12 16h.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg>
                Riwayat Notifikasi
              </div>
            </div>
            <div class="timeline" id="emergencyHistoryTimeline"></div>
          </div>
        </div>

        <div class="panel" style="margin-top:20px;">
          <div class="panel-head">
            <div class="htitle">
              <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/></svg>
              Pengaturan Eskalasi
            </div>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Kirim notifikasi ke kontak darurat</div><div class="d">Notifikasi dikirim otomatis melalui SMS dan panggilan saat kelelahan tingkat tinggi terdeteksi.</div></div>
            <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Kirim notifikasi ke administrator armada</div><div class="d">Berlaku untuk akun yang terhubung dengan perusahaan atau armada.</div></div>
            <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Aktifkan peringatan getar dan suara di kabin</div><div class="d">Memberi peringatan langsung kepada pengemudi sebelum notifikasi eksternal dikirim.</div></div>
            <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Rekomendasi berhenti otomatis</div><div class="d">Menyarankan titik istirahat terdekat saat tingkat kelelahan tinggi terdeteksi berulang.</div></div>
            <label class="switch"><input type="checkbox"><span class="track"></span></label>
          </div>
        </div>
      </section>

      <!-- ==================================================================
           PAGE: PENGATURAN
           ================================================================== -->
      <section class="page" id="page-settings">
        <div class="page-head">
          <div>
            <div class="eyebrow">Akun &amp; Perangkat</div>
            <h1 class="page-title">Pengaturan</h1>
            <p class="sub">Kelola profil, perangkat AI Driver Monitoring System, serta preferensi notifikasi dan privasi data Anda.</p>
          </div>
        </div>

        <div class="grid grid-cols-2">
          <div class="panel">
            <div class="panel-head"><div class="htitle">Profil Pengguna</div></div>
            <div class="field"><label>Nama Lengkap</label><input type="text" value="Andra Pratama"></div>
            <div class="field"><label>Nomor SIM</label><input type="text" value="351209XXXXXXXXX"></div>
            <div class="field"><label>Alamat Email</label><input type="email" value="andra.pratama@safeer.id"></div>
            <div class="field"><label>Nomor Telepon</label><input type="text" value="+62 812-3456-7890"></div>
            <button class="btn btn-primary" style="width:auto;" type="button">Simpan Perubahan</button>
          </div>

          <div style="display:flex; flex-direction:column; gap:20px;">
            <div class="panel">
              <div class="panel-head"><div class="htitle">Perangkat Terhubung</div></div>
              <div class="device-pair-card">
                <div class="dicon">
                  <svg viewBox="0 0 24 24" fill="none"><rect x="4" y="4" width="16" height="16" rx="3" stroke="currentColor" stroke-width="1.6"/><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/></svg>
                </div>
                <div style="flex:1;">
                  <div style="font-weight:700; font-size:13.5px;">Kamera AI DMS-04</div>
                  <div class="text-xs muted mono">SN: SFR-2026-88213 · Firmware v3.2.1</div>
                </div>
                <span class="badge badge-safe">Aktif</span>
              </div>
              <button class="btn btn-ghost btn-sm" style="width:100%; margin-top:14px;" type="button">+ Pasangkan Perangkat Baru</button>
            </div>

            <div class="panel">
              <div class="panel-head"><div class="htitle">Preferensi Notifikasi</div></div>
              <div class="settings-row">
                <div class="l"><div class="t">Ringkasan perjalanan harian</div><div class="d">Kirim ringkasan kewaspadaan dan skor berkendara setiap akhir hari.</div></div>
                <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
              </div>
              <div class="settings-row">
                <div class="l"><div class="t">Peringatan kelelahan langsung</div><div class="d">Notifikasi seketika saat AI mendeteksi tanda kelelahan.</div></div>
                <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
              </div>
              <div class="settings-row">
                <div class="l"><div class="t">Tips keselamatan mingguan</div><div class="d">Rekomendasi berdasarkan pola berkendara minggu sebelumnya.</div></div>
                <label class="switch"><input type="checkbox"><span class="track"></span></label>
              </div>
            </div>
          </div>
        </div>

        <div class="panel" style="margin-top:20px;">
          <div class="panel-head">
            <div class="htitle">
              <svg viewBox="0 0 24 24" fill="none"><path d="M12 3l8 4v5c0 5-3.4 8.6-8 9.9C7.4 20.6 4 17 4 12V7l8-4z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/></svg>
              Privasi &amp; Penyimpanan Data
            </div>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Penyimpanan cloud terenkripsi</div><div class="d">Seluruh data kelelahan dan riwayat perjalanan disimpan dengan enkripsi end-to-end.</div></div>
            <label class="switch"><input type="checkbox" checked disabled><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Bagikan data ke administrator armada</div><div class="d">Berlaku hanya jika akun terdaftar dalam armada perusahaan.</div></div>
            <label class="switch"><input type="checkbox" checked><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Simpan rekaman video indikator wajah</div><div class="d">Video mentah tidak disimpan secara default — hanya metrik hasil analisis yang direkam.</div></div>
            <label class="switch"><input type="checkbox"><span class="track"></span></label>
          </div>
          <div class="settings-row">
            <div class="l"><div class="t">Hapus seluruh data perjalanan</div><div class="d">Tindakan ini permanen dan tidak dapat dibatalkan.</div></div>
            <button class="btn btn-outline-danger btn-sm" type="button">Hapus Data</button>
          </div>
        </div>

        <div class="panel" style="margin-top:20px;">
          <div class="panel-head">
            <div class="htitle">
              <svg viewBox="0 0 24 24" fill="none"><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/><path d="M9.5 9.3a2.5 2.5 0 114 2c-.9.6-1.5 1.1-1.5 2.2" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/><circle cx="12" cy="17" r="0.8" fill="currentColor"/></svg>
              Pertanyaan Umum
            </div>
          </div>
          <div id="faqAccordion">
            <div class="accordion-item">
              <button class="accordion-head" type="button">
                Bagaimana AI mendeteksi kelelahan tanpa menyimpan video wajah saya?
                <svg viewBox="0 0 24 24" fill="none"><path d="M6 9l6 6 6-6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </button>
              <div class="accordion-body"><div class="accordion-body-inner">Perangkat memproses citra wajah secara langsung di perangkat (on-device) untuk mengekstrak metrik seperti frekuensi kedipan dan posisi kepala. Hanya hasil metrik yang dikirim ke cloud, bukan rekaman video mentah, kecuali fitur penyimpanan video diaktifkan secara manual.</div></div>
            </div>
            <div class="accordion-item">
              <button class="accordion-head" type="button">
                Apa yang terjadi saat status darurat terdeteksi?
                <svg viewBox="0 0 24 24" fill="none"><path d="M6 9l6 6 6-6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </button>
              <div class="accordion-body"><div class="accordion-body-inner">Sistem akan memberikan peringatan getar dan suara di kabin terlebih dahulu, kemudian mengirim notifikasi ke kontak darurat dan administrator armada apabila kondisi tidak membaik dalam beberapa detik.</div></div>
            </div>
            <div class="accordion-item">
              <button class="accordion-head" type="button">
                Bagaimana Skor Berkendara dihitung?
                <svg viewBox="0 0 24 24" fill="none"><path d="M6 9l6 6 6-6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </button>
              <div class="accordion-body"><div class="accordion-body-inner">Skor merupakan rata-rata tertimbang dari tingkat kewaspadaan sepanjang perjalanan, memperhitungkan frekuensi kejadian kelelahan dan konsistensi fokus terhadap jalan.</div></div>
            </div>
          </div>
        </div>
      </section>

    </main>
  </div>
</div>

<!-- ==========================================================================
     MODAL: DETAIL PERJALANAN
     ========================================================================== -->
<div class="modal-overlay" id="tripModal">
  <div class="modal-box">
    <div class="mhead">
      <h3 id="tripModalTitle">Detail Perjalanan</h3>
      <button class="modal-close" data-close-modal="tripModal" type="button">✕</button>
    </div>
    <div class="mbody" id="tripModalBody"></div>
    <div class="mfoot">
      <button class="btn btn-ghost btn-sm" data-close-modal="tripModal" type="button">Tutup</button>
      <button class="btn btn-primary btn-sm" style="width:auto;" type="button">Unduh Laporan</button>
    </div>
  </div>
</div>

<!-- ==========================================================================
     MODAL: PANDUAN CEPAT (ONBOARDING)
     ========================================================================== -->
<div class="modal-overlay" id="onboardingModal">
  <div class="modal-box">
    <div class="mhead">
      <h3>Selamat Datang di SAFEER</h3>
      <button class="modal-close" data-close-modal="onboardingModal" type="button">✕</button>
    </div>
    <div class="mbody">
      <p style="margin-bottom:14px;">Berikut tiga hal yang bisa langsung Anda coba di pusat kendali ini:</p>
      <div style="display:flex; gap:12px; margin-bottom:12px; align-items:flex-start;">
        <span class="badge badge-safe" style="flex-shrink:0;">1</span>
        <span>Tekan <strong>Mulai Perjalanan</strong> di Dasbor Real-time untuk melihat simulasi pemantauan aktif.</span>
      </div>
      <div style="display:flex; gap:12px; margin-bottom:12px; align-items:flex-start;">
        <span class="badge badge-warn" style="flex-shrink:0;">2</span>
        <span>Tekan <strong>Simulasikan Kelelahan</strong> untuk melihat bagaimana sistem merespons kondisi berisiko.</span>
      </div>
      <div style="display:flex; gap:12px; align-items:flex-start;">
        <span class="badge badge-neutral" style="flex-shrink:0;">3</span>
        <span>Jelajahi <strong>Dasbor Armada</strong> untuk melihat tampilan sisi administrator perusahaan (B2B).</span>
      </div>
    </div>
    <div class="mfoot">
      <button class="btn btn-primary" style="width:auto;" data-close-modal="onboardingModal" type="button">Mengerti, Mulai Jelajahi</button>
    </div>
  </div>
</div>

<!-- ==========================================================================
     MODAL: KONFIRMASI DARURAT
     ========================================================================== -->
<div class="modal-overlay" id="emergencyModal">
  <div class="modal-box">
    <div class="mhead">
      <h3>Kirim Notifikasi Uji Coba?</h3>
      <button class="modal-close" data-close-modal="emergencyModal" type="button">✕</button>
    </div>
    <div class="mbody">
      Sistem akan mengirim notifikasi uji ke seluruh kontak darurat dan administrator armada yang terdaftar. Gunakan fitur ini untuk memastikan jalur komunikasi darurat berfungsi dengan baik.
    </div>
    <div class="mfoot">
      <button class="btn btn-ghost btn-sm" data-close-modal="emergencyModal" type="button">Batal</button>
      <button class="btn btn-outline-danger btn-sm" id="btnConfirmTestAlert" type="button">Kirim Sekarang</button>
    </div>
  </div>
</div>

<script>
/* ============================================================================
   SAFEER — LOGIKA APLIKASI (PROTOTIPE)
   Seluruh data pada berkas ini adalah data simulasi (mock data) yang
   dibangkitkan di sisi klien untuk keperluan demonstrasi antarmuka.

   DAFTAR ISI:
     0.  State global & metadata halaman
     1.  Data simulasi (nama, rute, kejadian, pengemudi armada, laporan)
     2.  Autentikasi / layar splash
     3.  Navigasi antar halaman (SPA sederhana berbasis show/hide)
     4.  Sistem notifikasi toast
     5.  Modal helper (buka/tutup, ESC untuk menutup)
     6.  Simulasi pemantauan real-time (radar kewaspadaan & indikator AI)
     7.  Render tabel riwayat perjalanan + detail modal
     8.  Render laporan keselamatan + filter tingkat risiko
     9.  Render dasbor armada B2B (kartu pengemudi, peta, log peringatan)
     10. Render notifikasi darurat (kontak, riwayat, eskalasi)
     11. Mesin grafik kustom berbasis <canvas> (tanpa pustaka eksternal)
     12. Komponen akordion FAQ
     13. Inisialisasi aplikasi
   ============================================================================ */
'use strict';

/* ----------------------------------------------------------------------
   0. STATE GLOBAL
   ---------------------------------------------------------------------- */
const SafeerState = {
  role: 'driver',
  tripActive: false,
  tripStartedAt: null,
  tripTimerHandle: null,
  simTimerHandle: null,
  radarTimerHandle: null,
  distanceKm: 0,
  currentAlertness: 92,
  currentDrivingScore: 87,
  continuousDriveSeconds: 6120, // 01:42:00 awal, demo
  fatigueSimActive: false,
  historyFilter: 'all',
  blinkCount:0,
  eyeClosed:false,
  eyeClosedDuration:0,
  cameraReady:false,
  headPose:"Normal",
  yawnCount:0,
};

const PAGE_META = {
  dashboard:  { crumb: 'Pemantauan / Real-time',   title: 'Dasbor Real-time' },
  history:    { crumb: 'Pemantauan / Riwayat',      title: 'Riwayat Perjalanan' },
  analytics:  { crumb: 'Pemantauan / Statistik',     title: 'Statistik & Performa' },
  reports:    { crumb: 'Pemantauan / Laporan',       title: 'Laporan Keselamatan' },
  fleet:      { crumb: 'Armada / B2B',                title: 'Dasbor Armada' },
  emergency:  { crumb: 'Armada / Darurat',            title: 'Notifikasi Darurat' },
  settings:   { crumb: 'Akun / Preferensi',           title: 'Pengaturan' },
};

/* ----------------------------------------------------------------------
   1. DATA SIMULASI — nama, rute, kejadian
   ---------------------------------------------------------------------- */
const DRIVER_NAMES = [
  'Andra Pratama','Bagus Wicaksono','Citra Ayu Lestari','Dimas Setiawan',
  'Eka Rahmawati','Farhan Maulana','Gita Puspitasari','Hendra Kurniawan',
  'Intan Permatasari','Joko Susilo','Kartika Dewi','Lukman Hakim',
  'Mira Anggraini','Nanda Saputra','Oki Ramadhan','Putri Wulandari',
  'Rizky Firmansyah','Sari Indah Wahyuni'
];

const ROUTE_NAMES = [
  'Surabaya → Sidoarjo via Tol Waru',
  'Gresik → Surabaya via Jalur Pantura',
  'Malang → Surabaya via Tol Pandaan',
  'Surabaya → Mojokerto via Tol Krian',
  'Bangil → Pasuruan via Jalan Raya',
  'Surabaya → Lamongan via Tol Manyar',
  'Sidoarjo → Surabaya via Jalan Ahmad Yani',
  'Surabaya → Gresik Kawasan Industri',
  'Pasuruan → Malang via Tol Gempol',
  'Surabaya Pusat → Bandara Juanda',
];

const FATIGUE_EVENT_TYPES = [
  'Menguap berulang terdeteksi',
  'Durasi mata tertutup melebihi ambang batas',
  'Kepala menunduk lebih dari 3 detik',
  'Pandangan teralih dari jalan',
  'Frekuensi kedipan menurun drastis',
];

function pick(arr){ return arr[Math.floor(Math.random()*arr.length)]; }
function randRange(min,max){ return Math.random()*(max-min)+min; }
function randInt(min,max){ return Math.floor(randRange(min,max+1)); }
function pad2(n){ return n.toString().padStart(2,'0'); }
function fmtHMS(totalSeconds){
  const h = Math.floor(totalSeconds/3600);
  const m = Math.floor((totalSeconds%3600)/60);
  const s = Math.floor(totalSeconds%60);
  return `${pad2(h)}:${pad2(m)}:${pad2(s)}`;
}
function initials(name){
  return name.split(' ').slice(0,2).map(w=>w[0]).join('').toUpperCase();
}
function avatarColorFor(name){
  const palette = ['#2FE0C9','#8B7CF6','#F5A623','#5DA9FF','#FF7A9E'];
  let sum = 0; for(let i=0;i<name.length;i++) sum += name.charCodeAt(i);
  return palette[sum % palette.length];
}

/* Bangun daftar 24 perjalanan riwayat (mock) */
function generateTripHistory(count){
  const trips = [];
  const now = new Date(2026, 7, 5, 8, 0, 0);
  for(let i=0;i<count;i++){
    const date = new Date(now.getTime() - i*1000*60*60*(20+randInt(0,10)));
    const durationMin = randInt(18,95);
    const distance = +(durationMin * randRange(0.5,0.9)).toFixed(1);
    const alertness = randInt(58,99);
    const score = Math.max(40, Math.min(100, alertness - randInt(-4,10)));
    let status = 'safe';
    if(alertness < 65 || score < 65) status = 'danger';
    else if(alertness < 80 || score < 78) status = 'warn';
    trips.push({
      id: 'TRP-' + (2026000 + count - i),
      date,
      route: pick(ROUTE_NAMES),
      durationMin,
      distance,
      alertness,
      score,
      status,
    });
  }
  return trips;
}
const tripHistoryData = generateTripHistory(24);

/* Bangun daftar pengemudi armada (mock, untuk mode B2B) */
function generateFleetDrivers(count){
  const drivers = [];
  for(let i=0;i<count;i++){
    const name = DRIVER_NAMES[i % DRIVER_NAMES.length];
    const roll = Math.random();
    let status = 'safe';
    if(roll > 0.94) status = 'danger';
    else if(roll > 0.78) status = 'warn';
    drivers.push({
      id: 'DRV-' + (1000+i),
      name,
      status,
      vehicle: 'B ' + randInt(1000,9999) + ' ' + String.fromCharCode(65+randInt(0,25)) + String.fromCharCode(65+randInt(0,25)),
      alertness: status==='danger' ? randInt(30,48) : status==='warn' ? randInt(55,74) : randInt(80,99),
      speed: randInt(0,92),
      route: pick(ROUTE_NAMES),
      lastUpdate: `${randInt(0,4)} mnt lalu`,
    });
  }
  return drivers;
}
const fleetDriversData = generateFleetDrivers(9);

/* Log peringatan armada (mock) */
function generateFleetAlertLog(){
  const items = [];
  const severities = ['danger','warn','safe'];
  for(let i=0;i<6;i++){
    const sev = i===0 ? 'danger' : pick(severities);
    items.push({
      sev,
      title: sev==='danger' ? 'Kelelahan tingkat tinggi — perlu tindakan segera'
            : sev==='warn' ? 'Indikasi kelelahan ringan terdeteksi'
            : 'Perjalanan selesai dengan aman',
      desc: `${pick(DRIVER_NAMES)} · ${pick(ROUTE_NAMES)}`,
      time: `${randInt(0,58)} menit lalu`,
    });
  }
  return items;
}
const fleetAlertLog = generateFleetAlertLog();

/* Laporan keselamatan (mock) */
function generateSafetyReports(){
  const sevList = ['sev-high','sev-mid','sev-low'];
  const titles = {
    'sev-high': ['Kelelahan Tingkat Tinggi Terdeteksi', 'Mata Tertutup Melebihi 2 Detik'],
    'sev-mid': ['Frekuensi Menguap Meningkat', 'Kepala Menunduk Berulang'],
    'sev-low': ['Perjalanan Selesai dengan Skor Baik', 'Pola Kedipan Normal Sepanjang Perjalanan'],
  };
  const reports = [];
  for(let i=0;i<9;i++){
    const sev = sevList[i % 3];
    reports.push({
      sev,
      title: pick(titles[sev]),
      date: `${randInt(1,4)} Agustus 2026 · ${pad2(randInt(6,21))}:${pad2(randInt(0,59))}`,
      desc: 'Terdeteksi pada rute ' + pick(ROUTE_NAMES) + '. Sistem AI merekomendasikan ' +
            (sev==='sev-high' ? 'berhenti segera di titik istirahat terdekat.' :
             sev==='sev-mid' ? 'peningkatan frekuensi istirahat pada perjalanan berikutnya.' :
             'mempertahankan pola berkendara saat ini.'),
    });
  }
  return reports;
}
const safetyReportsData = generateSafetyReports();

/* ----------------------------------------------------------------------
   2. AUTENTIKASI / SPLASH
   ---------------------------------------------------------------------- */
function initAuth(){
  const roleButtons = document.querySelectorAll('#roleToggle button');
  roleButtons.forEach(btn=>{
    btn.addEventListener('click', ()=>{
      roleButtons.forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      SafeerState.role = btn.dataset.role;
    });
  });

  document.getElementById('btnLogin').addEventListener('click', ()=> attemptLogin());
  document.getElementById('btnDemo').addEventListener('click', ()=> enterApp());

  document.getElementById('loginEmail').addEventListener('keydown', (e)=>{
    if(e.key === 'Enter') attemptLogin();
  });
  document.getElementById('loginPass').addEventListener('keydown', (e)=>{
    if(e.key === 'Enter') attemptLogin();
  });
}

/* Validasi ringan sebelum masuk — memastikan prototipe terasa seperti
   aplikasi sungguhan tanpa memerlukan backend autentikasi nyata. */
function attemptLogin(){
  const emailField = document.getElementById('loginEmail');
  const passField = document.getElementById('loginPass');
  const emailWrap = emailField.closest('.field');
  const passWrap = passField.closest('.field');

  emailWrap.classList.remove('has-error');
  passWrap.classList.remove('has-error');

  const emailValid = /\S+@\S+\.\S+/.test(emailField.value.trim());
  const passValid = passField.value.trim().length >= 6;

  if(!emailValid){
    emailWrap.classList.add('has-error');
    ensureErrorMessage(emailWrap, 'Masukkan alamat email yang valid.');
  }
  if(!passValid){
    passWrap.classList.add('has-error');
    ensureErrorMessage(passWrap, 'Kata sandi minimal 6 karakter.');
  }
  if(!emailValid || !passValid) return;

  enterApp();
}
function ensureErrorMessage(wrap, message){
  let msg = wrap.querySelector('.form-error');
  if(!msg){
    msg = document.createElement('div');
    msg.className = 'form-error';
    wrap.appendChild(msg);
  }
  msg.textContent = message;
}

function enterApp(){
  document.getElementById('splash-screen').classList.add('hidden');
  document.getElementById('app-shell').classList.add('active');
  applyRoleToProfile();
  if(SafeerState.role === 'fleet'){
    navigateTo('fleet');
  } else {
    navigateTo('dashboard');
  }
  showToast('safe','Berhasil masuk','Perangkat AI DMS-04 terhubung dan siap memantau.');
  setTimeout(()=> openModal('onboardingModal'), 700);
}

function applyRoleToProfile(){
  const isFleet = SafeerState.role === 'fleet';
  document.getElementById('profileName').textContent = isFleet ? 'Wulan Setiadi' : 'Andra Pratama';
  document.getElementById('profileRole').textContent = isFleet ? 'Administrator Armada' : 'Pengemudi Aktif';
  document.getElementById('profileAvatar').textContent = isFleet ? 'WS' : 'AP';
}

/* ----------------------------------------------------------------------
   3. NAVIGASI ANTAR HALAMAN
   ---------------------------------------------------------------------- */
function navigateTo(pageId){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  const target = document.getElementById('page-'+pageId);
  if(target) target.classList.add('active');

  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  const navBtn = document.querySelector(`.nav-item[data-page="${pageId}"]`);
  if(navBtn) navBtn.classList.add('active');

  const meta = PAGE_META[pageId];
  if(meta){
    document.getElementById('pageCrumb').textContent = meta.crumb;
    document.getElementById('pageTopTitle').textContent = meta.title;
  }

  if(pageId === 'analytics') requestAnimationFrame(renderAllCharts);
  if(window.innerWidth <= 880) document.getElementById('sidebar').classList.remove('open');
}

function initNavigation(){
  document.querySelectorAll('.nav-item[data-page]').forEach(btn=>{
    btn.addEventListener('click', ()=> navigateTo(btn.dataset.page));
  });
  document.getElementById('menuToggle').addEventListener('click', ()=>{
    document.getElementById('sidebar').classList.toggle('open');
  });
  document.getElementById('btnLogout').addEventListener('click', (e)=>{
    e.preventDefault();
    document.getElementById('app-shell').classList.remove('active');
    document.getElementById('splash-screen').classList.remove('hidden');
    stopTripSimulation();
  });
}

/* ----------------------------------------------------------------------
   4. TOAST NOTIFICATION
   ---------------------------------------------------------------------- */
function showToast(kind, title, desc){
  const stack = document.getElementById('toastStack');
  const el = document.createElement('div');
  el.className = 'toast ' + (kind === 'safe' ? '' : kind);
  const iconPath = kind === 'danger'
    ? '<path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/><path d="M12 10v4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/>'
    : kind === 'warn'
    ? '<circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/><path d="M12 8v5M12 16h.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>'
    : '<path d="M20 6L9 17l-5-5" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>';
  el.innerHTML = `
    <svg viewBox="0 0 24 24" fill="none">${iconPath}</svg>
    <div>
      <div class="tt">${title}</div>
      <div class="td">${desc}</div>
    </div>
    <button class="tx" type="button" aria-label="Tutup notifikasi">✕</button>
  `;
  el.querySelector('.tx').addEventListener('click', ()=> el.remove());
  stack.appendChild(el);
  setTimeout(()=>{ if(el.parentNode) el.remove(); }, 6000);
}

/* ----------------------------------------------------------------------
   5. MODAL HELPERS
   ---------------------------------------------------------------------- */
function openModal(id){ document.getElementById(id).classList.add('active'); }
function closeModal(id){ document.getElementById(id).classList.remove('active'); }
function initModals(){
  document.querySelectorAll('[data-close-modal]').forEach(btn=>{
    btn.addEventListener('click', ()=> closeModal(btn.dataset.closeModal));
  });
  document.querySelectorAll('.modal-overlay').forEach(ov=>{
    ov.addEventListener('click', (e)=>{ if(e.target === ov) ov.classList.remove('active'); });
  });
  document.addEventListener('keydown', (e)=>{
    if(e.key === 'Escape'){
      document.querySelectorAll('.modal-overlay.active').forEach(ov=> ov.classList.remove('active'));
    }
  });
}

/* ----------------------------------------------------------------------
   6. SIMULASI PEMANTAUAN REAL-TIME (DASBOR UTAMA)
   ---------------------------------------------------------------------- */
function startTripSimulation(){
  SafeerState.tripActive = true;
  SafeerState.tripStartedAt = Date.now();
  document.getElementById('tripStatusVal').textContent = 'Sedang Berlangsung';
  document.getElementById('btnToggleTrip').innerHTML = `
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none"><rect x="6" y="6" width="12" height="12" rx="2" fill="currentColor"/></svg>
    Akhiri Perjalanan`;

  SafeerState.tripTimerHandle = setInterval(()=>{
    const elapsedSec = Math.floor((Date.now() - SafeerState.tripStartedAt)/1000);
    document.getElementById('tripDuration').textContent = fmtHMS(elapsedSec);
    SafeerState.distanceKm += randRange(0.006,0.014);
    document.getElementById('tripDistance').textContent = SafeerState.distanceKm.toFixed(1) + ' km';
    document.getElementById('tripSpeed').textContent = randInt(38,78) + ' km/j';

    SafeerState.continuousDriveSeconds += 1;
    document.getElementById('continuousDriveTime').textContent = fmtHMS(SafeerState.continuousDriveSeconds);
    updateRestSuggestion();
  }, 1000);

  showToast('safe','Perjalanan dimulai','Pemantauan kelelahan aktif. Berkendara aman!');
}

function stopTripSimulation(){
  SafeerState.tripActive = false;
  clearInterval(SafeerState.tripTimerHandle);
  document.getElementById('tripStatusVal').textContent = 'Belum Dimulai';
  document.getElementById('btnToggleTrip').innerHTML = `
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M5 4l14 8-14 8V4z" fill="currentColor"/></svg>
    Mulai Perjalanan`;
}

function updateRestSuggestion(){
  const mins = Math.floor(SafeerState.continuousDriveSeconds/60);
  const el = document.getElementById('restSuggestion');
  if(mins >= 120){
    el.textContent = 'Waktu istirahat! Anda telah berkendara lebih dari 2 jam berturut-turut.';
    el.style.color = 'var(--signal-red)';
  } else if(mins >= 90){
    const remaining = 120 - mins;
    el.textContent = `Disarankan istirahat dalam ${remaining} menit ke depan.`;
    el.style.color = 'var(--signal-amber)';
  } else {
    el.textContent = 'Kondisi berkendara masih dalam batas aman.';
    el.style.color = 'var(--signal-cyan)';
  }
}

function setMeter(id, percent, level){
  const el = document.getElementById(id);
  if(!el) return;
  el.style.width = Math.max(4,Math.min(100,percent)) + '%';
  const track = el.parentElement;
  track.classList.remove('c-safe','c-warn','c-danger');
  track.classList.add('c-'+level);
}

function levelFromScore(score){
  if(score >= 80) return 'safe';
  if(score >= 55) return 'warn';
  return 'danger';
}

/* Loop pembaruan indikator kelelahan setiap detik, mensimulasikan output AI */
function tickFatigueMetrics(){
  const forcedDrop = SafeerState.fatigueSimActive;

  let blink = forcedDrop ? randInt(4,8) : randInt(13,20);
  let eyeClose = forcedDrop ? randRange(1.1,2.1) : randRange(0.1,0.35);
  let yawn = forcedDrop ? randInt(3,6) : randInt(0,2);
  let headStable = forcedDrop ? randInt(35,55) : randInt(80,98);
  let gazeFocus = forcedDrop ? randInt(30,50) : randInt(85,99);

  document.getElementById('metricBlink').innerHTML = blink + ' <span class="u">kedip/mnt</span>';
  setMeter('meterBlink', (blink/24)*100, blink < 9 ? 'danger' : blink < 12 ? 'warn' : 'safe');

  document.getElementById('metricEyeClose').innerHTML = eyeClose.toFixed(2) + ' <span class="u">detik</span>';
  setMeter('meterEyeClose', (eyeClose/2.2)*100, eyeClose > 1.4 ? 'danger' : eyeClose > 0.6 ? 'warn' : 'safe');

  document.getElementById('metricYawn').innerHTML = yawn + ' <span class="u">/ 10 mnt</span>';
  setMeter('meterYawn', (yawn/6)*100, yawn >= 4 ? 'danger' : yawn >= 2 ? 'warn' : 'safe');

  const headLabel = headStable > 75 ? 'Stabil' : headStable > 50 ? 'Sedikit Menunduk' : 'Menunduk Berulang';
  document.getElementById('metricHead').textContent = headLabel;
  setMeter('meterHead', headStable, headStable > 75 ? 'safe' : headStable > 50 ? 'warn' : 'danger');

  const gazeLabel = gazeFocus > 80 ? 'Fokus ke Jalan' : gazeFocus > 55 ? 'Sesekali Teralih' : 'Sering Teralih';
  document.getElementById('metricGaze').textContent = gazeLabel;
  setMeter('meterGaze', gazeFocus, gazeFocus > 80 ? 'safe' : gazeFocus > 55 ? 'warn' : 'danger');

  const composite = Math.round((
    (blink/22)*100*0.2 +
    (100 - (eyeClose/2.2)*100)*0.25 +
    (100 - (yawn/6)*100)*0.15 +
    headStable*0.2 +
    gazeFocus*0.2
  ));
  const clamped = Math.max(15, Math.min(99, composite));
  SafeerState.currentAlertness = clamped;
  updateAlertnessDisplay(clamped);
}

function updateAlertnessDisplay(score){
  document.getElementById('alertnessScore').textContent = score;
  const level = levelFromScore(score);
  const statusLine = document.getElementById('alertnessStatusLine');
  const recBanner = document.getElementById('recBanner');
  const recTitle = document.getElementById('recTitle');
  const recDesc = document.getElementById('recDesc');

  statusLine.classList.remove('safe','warn','danger');
  statusLine.classList.add(level);
  recBanner.classList.remove('ok');

  if(level === 'safe'){
    statusLine.innerHTML = `<svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M20 6L9 17l-5-5" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"/></svg> Pengemudi dalam kondisi terjaga`;
    recBanner.classList.add('ok');
    recTitle.textContent = 'Kondisi baik, lanjutkan perjalanan';
    recDesc.textContent = 'Tidak ada indikasi kelelahan terdeteksi. Pertahankan pola istirahat setiap 2 jam.';
  } else if(level === 'warn'){
    statusLine.innerHTML = `<svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M12 8v5M12 16h.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/><circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/></svg> Indikasi kelelahan ringan terdeteksi`;
    recTitle.textContent = 'Tingkatkan kewaspadaan';
    recDesc.textContent = 'Beberapa indikator menunjukkan penurunan kewaspadaan. Pertimbangkan istirahat singkat dalam 15 menit.';
  } else {
    statusLine.innerHTML = `<svg width="14" height="14" viewBox="0 0 24 24" fill="none"><path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/></svg> Kelelahan tingkat tinggi terdeteksi!`;
    recTitle.textContent = 'Segera berhenti dan beristirahat';
    recDesc.textContent = 'Sistem mendeteksi risiko keselamatan tinggi. Notifikasi darurat telah dikirim ke kontak terdaftar.';
  }

  // skor berkendara mengikuti tren kewaspadaan dengan sedikit redaman
  SafeerState.currentDrivingScore = Math.round(SafeerState.currentDrivingScore*0.9 + score*0.1);
  document.getElementById('drivingScoreVal').textContent = SafeerState.currentDrivingScore;
  const circumference = 314;
  const offset = circumference - (SafeerState.currentDrivingScore/100)*circumference;
  const ring = document.getElementById('scoreRingCircle');
  ring.style.strokeDashoffset = offset;
  ring.setAttribute('stroke', SafeerState.currentDrivingScore >= 80 ? '#2FE0C9' : SafeerState.currentDrivingScore >= 55 ? '#F5A623' : '#FF4D5E');
}

function initRealtimeSimulation(){
  tickFatigueMetrics();
  SafeerState.radarTimerHandle = setInterval(tickFatigueMetrics, 4000);

  document.getElementById('btnToggleTrip').addEventListener('click', ()=>{
    if(SafeerState.tripActive) stopTripSimulation(); else startTripSimulation();
  });

  document.getElementById('btnSimAlert').addEventListener('click', ()=>{
    SafeerState.fatigueSimActive = true;
    tickFatigueMetrics();
    showToast('danger','Kelelahan tingkat tinggi terdeteksi','Notifikasi darurat dikirim ke kontak terdaftar dan administrator armada.');
    document.getElementById('emergencyBadge').textContent =
      (parseInt(document.getElementById('emergencyBadge').textContent,10)+1);
    setTimeout(()=>{ SafeerState.fatigueSimActive = false; tickFatigueMetrics(); }, 8000);
  });

  document.getElementById('btnFindRest').addEventListener('click', ()=>{
    showToast('safe','Titik istirahat ditemukan','Rest Area KM 26 Tol Surabaya–Mojokerto, 4.2 km dari lokasi Anda.');
  });
}

/* ----------------------------------------------------------------------
   7. RENDER RIWAYAT PERJALANAN
   ---------------------------------------------------------------------- */
function statusBadge(status){
  if(status === 'safe') return '<span class="badge badge-safe">Aman</span>';
  if(status === 'warn') return '<span class="badge badge-warn">Waspada</span>';
  return '<span class="badge badge-danger">Berisiko</span>';
}
function barColorFor(status){
  return status === 'safe' ? '#2FE0C9' : status === 'warn' ? '#F5A623' : '#FF4D5E';
}
function formatTripDate(d){
  const bulan = ['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Agu','Sep','Okt','Nov','Des'];
  return `${d.getDate()} ${bulan[d.getMonth()]} ${d.getFullYear()} · ${pad2(d.getHours())}:${pad2(d.getMinutes())}`;
}

function renderTripHistoryTable(){
  const body = document.getElementById('historyTableBody');
  const query = (document.getElementById('historySearch').value || '').toLowerCase();
  const filtered = tripHistoryData.filter(t=>{
    const matchesFilter = SafeerState.historyFilter === 'all' || t.status === SafeerState.historyFilter;
    const matchesQuery = t.route.toLowerCase().includes(query) || formatTripDate(t.date).toLowerCase().includes(query);
    return matchesFilter && matchesQuery;
  }).slice(0,8);

  body.innerHTML = filtered.map(t => `
    <tr>
      <td>
        <div class="cell-strong">${t.route}</div>
        <div class="cell-mono">${formatTripDate(t.date)} · ${t.id}</div>
      </td>
      <td>${t.durationMin} menit</td>
      <td>${t.distance} km</td>
      <td>
        <span class="mini-bar-track"><span class="mini-bar-fill" style="width:${t.alertness}%;background:${barColorFor(t.status)};"></span></span>
        ${t.alertness}/100
      </td>
      <td class="cell-strong">${t.score}/100</td>
      <td>${statusBadge(t.status)}</td>
      <td><button class="btn btn-ghost btn-sm" data-trip-id="${t.id}">Detail</button></td>
    </tr>
  `).join('');

  document.getElementById('historyPageInfo').textContent =
    `Menampilkan ${filtered.length} dari ${tripHistoryData.length} perjalanan`;

  body.querySelectorAll('[data-trip-id]').forEach(btn=>{
    btn.addEventListener('click', ()=> openTripDetail(btn.dataset.tripId));
  });
}

function openTripDetail(tripId){
  const t = tripHistoryData.find(x=>x.id === tripId);
  if(!t) return;
  document.getElementById('tripModalTitle').textContent = t.route;
  document.getElementById('tripModalBody').innerHTML = `
    <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-bottom:14px;">
      <div><div class="text-xs faint">Tanggal</div><div class="cell-strong">${formatTripDate(t.date)}</div></div>
      <div><div class="text-xs faint">ID Perjalanan</div><div class="cell-strong mono">${t.id}</div></div>
      <div><div class="text-xs faint">Durasi</div><div class="cell-strong">${t.durationMin} menit</div></div>
      <div><div class="text-xs faint">Jarak</div><div class="cell-strong">${t.distance} km</div></div>
      <div><div class="text-xs faint">Kewaspadaan Rata-rata</div><div class="cell-strong">${t.alertness}/100</div></div>
      <div><div class="text-xs faint">Skor Berkendara</div><div class="cell-strong">${t.score}/100</div></div>
    </div>
    ${statusBadge(t.status)}
    <p style="margin-top:12px; line-height:1.6;">Selama perjalanan ini, sistem AI mencatat kondisi pengemudi secara berkala. ${
      t.status === 'danger' ? 'Terdapat beberapa indikasi kelelahan signifikan yang memicu rekomendasi istirahat.' :
      t.status === 'warn' ? 'Terdapat indikasi kelelahan ringan pada sebagian perjalanan.' :
      'Tidak ditemukan indikasi kelelahan yang signifikan sepanjang perjalanan.'
    }</p>
  `;
  openModal('tripModal');
}

function initHistoryPage(){
  renderTripHistoryTable();
  document.getElementById('historySearch').addEventListener('input', renderTripHistoryTable);
  document.querySelectorAll('#historyFilters .filter-chip').forEach(chip=>{
    chip.addEventListener('click', ()=>{
      document.querySelectorAll('#historyFilters .filter-chip').forEach(c=>c.classList.remove('active'));
      chip.classList.add('active');
      SafeerState.historyFilter = chip.dataset.filter;
      renderTripHistoryTable();
    });
  });
}

/* ----------------------------------------------------------------------
   8. RENDER LAPORAN KESELAMATAN
   ---------------------------------------------------------------------- */
function sevIcon(sev){
  if(sev === 'sev-high') return '<path d="M12 3l9 16H3z" stroke="currentColor" stroke-width="1.8" stroke-linejoin="round"/><path d="M12 10v4" stroke="currentColor" stroke-width="1.8" stroke-linecap="round"/>';
  if(sev === 'sev-mid') return '<circle cx="12" cy="12" r="9" stroke="currentColor" stroke-width="1.6"/><path d="M12 8v5M12 16h.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>';
  return '<path d="M20 6L9 17l-5-5" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>';
}
let reportSeverityFilter = 'all';
function renderSafetyReports(){
  const list = document.getElementById('reportsList');
  const filtered = reportSeverityFilter === 'all'
    ? safetyReportsData
    : safetyReportsData.filter(r => r.sev === reportSeverityFilter);

  if(filtered.length === 0){
    list.innerHTML = `
      <div class="empty-state">
        <svg viewBox="0 0 24 24" fill="none"><path d="M7 3h8l4 4v14H7z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/></svg>
        <div class="et">Belum ada laporan pada kategori ini</div>
        <div class="ed">Laporan akan muncul otomatis saat sistem AI mendeteksi kejadian pada tingkat risiko ini.</div>
      </div>`;
    return;
  }

  list.innerHTML = filtered.map(r => `
    <div class="report-card">
      <div class="report-icon ${r.sev}"><svg viewBox="0 0 24 24" fill="none">${sevIcon(r.sev)}</svg></div>
      <div class="report-body">
        <div class="rtop">
          <h4>${r.title}</h4>
          <span class="rdate">${r.date}</span>
        </div>
        <p>${r.desc}</p>
        <div class="report-actions">
          <button class="btn btn-ghost btn-sm" type="button">Lihat Detail</button>
          <button class="btn btn-ghost btn-sm" type="button">Unduh PDF</button>
        </div>
      </div>
    </div>
  `).join('');
}

function initReportFilters(){
  document.querySelectorAll('#reportFilters .filter-chip').forEach(chip=>{
    chip.addEventListener('click', ()=>{
      document.querySelectorAll('#reportFilters .filter-chip').forEach(c=>c.classList.remove('active'));
      chip.classList.add('active');
      reportSeverityFilter = chip.dataset.sev;
      renderSafetyReports();
    });
  });
}

/* ----------------------------------------------------------------------
   9. RENDER DASBOR ARMADA (B2B)
   ---------------------------------------------------------------------- */
let fleetStatusFilter = 'all';
let fleetSearchQuery = '';
function renderFleetDriverGrid(){
  const grid = document.getElementById('fleetDriverGrid');
  const emptyState = document.getElementById('fleetEmptyState');
  const filtered = fleetDriversData.filter(d=>{
    const matchesStatus = fleetStatusFilter === 'all' || d.status === fleetStatusFilter;
    const matchesQuery = d.name.toLowerCase().includes(fleetSearchQuery.toLowerCase());
    return matchesStatus && matchesQuery;
  });

  if(filtered.length === 0){
    grid.style.display = 'none';
    emptyState.style.display = 'flex';
    return;
  }
  grid.style.display = 'grid';
  emptyState.style.display = 'none';

  grid.innerHTML = filtered.map(d => `
    <div class="fleet-driver-card">
      <div class="fleet-driver-top">
        <div class="row-avatar" style="background:${avatarColorFor(d.name)}22; color:${avatarColorFor(d.name)};">${initials(d.name)}</div>
        <div class="info">
          <div class="n">${d.name}</div>
          <div class="id">${d.id} · ${d.vehicle}</div>
        </div>
        <span class="status-dot-lg ${d.status}"></span>
      </div>
      <div class="fleet-driver-meta">
        <div class="m"><div class="l">Kewaspadaan</div><div class="v">${d.alertness}/100</div></div>
        <div class="m"><div class="l">Kecepatan</div><div class="v">${d.speed} km/j</div></div>
        <div class="m" style="grid-column:span 2;"><div class="l">Rute Aktif</div><div class="v" style="font-size:11.5px; font-weight:600;">${d.route}</div></div>
      </div>
      <div class="text-xs faint">Diperbarui ${d.lastUpdate}</div>
    </div>
  `).join('');
}

function renderFleetAlertTimeline(){
  const el = document.getElementById('fleetAlertTimeline');
  el.innerHTML = fleetAlertLog.map(a => `
    <div class="timeline-item ${a.sev}">
      <div class="tt">${a.title}</div>
      <div class="td">${a.desc}</div>
      <div class="tm">${a.time}</div>
    </div>
  `).join('');
}

function renderFleetMap(){
  const map = document.getElementById('fleetMap');
  map.innerHTML = '';
  fleetDriversData.forEach(d => {
    const pin = document.createElement('div');
    pin.className = 'fleet-map-pin ' + d.status;
    pin.style.left = randInt(6,90) + '%';
    pin.style.top = randInt(10,84) + '%';
    pin.title = d.name;
    map.appendChild(pin);
  });
  const note = document.createElement('div');
  note.className = 'fleet-map-note';
  note.textContent = 'Visualisasi lokasi berbasis simulasi — integrasi GPS aktual tersedia melalui perangkat AI DMS.';
  map.appendChild(note);
}

function initFleetPage(){
  renderFleetDriverGrid();
  renderFleetAlertTimeline();
  renderFleetMap();

  document.getElementById('fleetSearch').addEventListener('input', (e)=>{
    fleetSearchQuery = e.target.value;
    renderFleetDriverGrid();
  });
  document.querySelectorAll('#fleetStatusFilters .filter-chip').forEach(chip=>{
    chip.addEventListener('click', ()=>{
      document.querySelectorAll('#fleetStatusFilters .filter-chip').forEach(c=>c.classList.remove('active'));
      chip.classList.add('active');
      fleetStatusFilter = chip.dataset.status;
      renderFleetDriverGrid();
    });
  });
}

/* ----------------------------------------------------------------------
   10. RENDER NOTIFIKASI DARURAT
   ---------------------------------------------------------------------- */
const emergencyContacts = [
  { name:'Rina Kusuma Wardani', role:'Kontak Keluarga · Istri' },
  { name:'Budi Santoso', role:'Kontak Keluarga · Ayah' },
  { name:'Pos Pengawasan Armada', role:'Administrator Armada CV Avijaya' },
];
const emergencyHistory = [
  { sev:'danger', title:'Notifikasi darurat terkirim', desc:'Kelelahan tingkat tinggi terdeteksi di Tol Waru KM 12', time:'Hari ini, 14:22' },
  { sev:'warn', title:'Peringatan kelelahan ringan', desc:'Frekuensi menguap meningkat pada rute Gresik–Surabaya', time:'Kemarin, 09:47' },
  { sev:'safe', title:'Uji coba notifikasi berhasil', desc:'Seluruh kontak darurat menerima notifikasi dalam 3.8 detik', time:'2 hari lalu, 18:03' },
];

function renderEmergencyContacts(){
  const el = document.getElementById('emergencyContactsList');
  el.innerHTML = emergencyContacts.map(c => `
    <div class="contact-item">
      <div class="avatar" style="color:${avatarColorFor(c.name)};">${initials(c.name)}</div>
      <div class="info">
        <div class="n">${c.name}</div>
        <div class="r">${c.role}</div>
      </div>
      <button class="btn btn-ghost btn-sm" type="button">Ubah</button>
    </div>
  `).join('');
}

function renderEmergencyHistory(){
  const el = document.getElementById('emergencyHistoryTimeline');
  el.innerHTML = emergencyHistory.map(h => `
    <div class="timeline-item ${h.sev}">
      <div class="tt">${h.title}</div>
      <div class="td">${h.desc}</div>
      <div class="tm">${h.time}</div>
    </div>
  `).join('');
}

function initEmergencyPage(){
  renderEmergencyContacts();
  renderEmergencyHistory();
  document.getElementById('btnTestAlert').addEventListener('click', ()=> openModal('emergencyModal'));
  document.getElementById('btnConfirmTestAlert').addEventListener('click', ()=>{
    closeModal('emergencyModal');
    showToast('safe','Notifikasi uji terkirim','Seluruh kontak darurat dan administrator armada telah menerima notifikasi uji coba.');
  });
}

/* ----------------------------------------------------------------------
   11. MESIN GRAFIK (CANVAS, TANPA PUSTAKA EKSTERNAL)
   ---------------------------------------------------------------------- */
function setupCanvasHiDPI(canvas){
  const ratio = window.devicePixelRatio || 1;
  const rect = canvas.getBoundingClientRect();
  const cssWidth = rect.width || canvas.parentElement.clientWidth;
  const cssHeight = canvas.height ? parseInt(canvas.getAttribute('height'),10) : 220;
  canvas.width = cssWidth * ratio;
  canvas.height = cssHeight * ratio;
  canvas.style.width = cssWidth + 'px';
  canvas.style.height = cssHeight + 'px';
  const ctx = canvas.getContext('2d');
  ctx.scale(ratio, ratio);
  return { ctx, w: cssWidth, h: cssHeight };
}

function drawRoundedRect(ctx,x,y,w,h,r){
  ctx.beginPath();
  ctx.moveTo(x+r, y);
  ctx.arcTo(x+w, y, x+w, y+h, r);
  ctx.arcTo(x+w, y+h, x, y+h, r);
  ctx.arcTo(x, y+h, x, y, r);
  ctx.arcTo(x, y, x+w, y, r);
  ctx.closePath();
}

/* Grafik garis: tren kewaspadaan mingguan */
function drawAlertnessTrendChart(){
  const canvas = document.getElementById('chartAlertnessTrend');
  if(!canvas) return;
  const { ctx, w, h } = setupCanvasHiDPI(canvas);
  ctx.clearRect(0,0,w,h);

  const days = ['Sen','Sel','Rab','Kam','Jum','Sab','Min'];
  const alertnessSeries = days.map(()=> randInt(78,96));
  const scoreSeries = days.map(()=> randInt(70,92));

  const padL = 34, padR = 12, padT = 16, padB = 28;
  const plotW = w - padL - padR;
  const plotH = h - padT - padB;
  const maxVal = 100, minVal = 50;

  // grid horizontal
  ctx.strokeStyle = 'rgba(255,255,255,0.06)';
  ctx.lineWidth = 1;
  ctx.font = '10px JetBrains Mono, monospace';
  ctx.fillStyle = 'rgba(169,183,214,0.7)';
  for(let i=0;i<=4;i++){
    const y = padT + (plotH/4)*i;
    ctx.beginPath(); ctx.moveTo(padL, y); ctx.lineTo(w-padR, y); ctx.stroke();
    const val = Math.round(maxVal - (maxVal-minVal)*(i/4));
    ctx.fillText(val, 4, y+3);
  }

  function plot(series, color, glow){
    ctx.beginPath();
    series.forEach((val,i)=>{
      const x = padL + (plotW/(series.length-1))*i;
      const y = padT + plotH - ((val-minVal)/(maxVal-minVal))*plotH;
      if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
    });
    ctx.strokeStyle = color;
    ctx.lineWidth = 2.4;
    ctx.lineJoin = 'round';
    if(glow){ ctx.shadowColor = color; ctx.shadowBlur = 8; }
    ctx.stroke();
    ctx.shadowBlur = 0;
    series.forEach((val,i)=>{
      const x = padL + (plotW/(series.length-1))*i;
      const y = padT + plotH - ((val-minVal)/(maxVal-minVal))*plotH;
      ctx.beginPath(); ctx.arc(x,y,3,0,Math.PI*2);
      ctx.fillStyle = color; ctx.fill();
    });
  }
  plot(scoreSeries, '#8B7CF6', false);
  plot(alertnessSeries, '#2FE0C9', true);

  ctx.fillStyle = 'rgba(169,183,214,0.85)';
  ctx.font = '11px Inter, sans-serif';
  days.forEach((d,i)=>{
    const x = padL + (plotW/(days.length-1))*i;
    ctx.textAlign = i===0 ? 'left' : i===days.length-1 ? 'right' : 'center';
    ctx.fillText(d, x, h-8);
  });
}

/* Grafik batang: kejadian kelelahan berdasarkan jenis */
function drawFatigueEventsChart(){
  const canvas = document.getElementById('chartFatigueEvents');
  if(!canvas) return;
  const { ctx, w, h } = setupCanvasHiDPI(canvas);
  ctx.clearRect(0,0,w,h);

  const labels = ['Menguap','Mata\nTertutup','Kepala\nMenunduk','Pandangan\nTeralih'];
  const values = [randInt(4,12), randInt(2,8), randInt(3,10), randInt(1,6)];
  const colors = ['#F5A623','#FF4D5E','#8B7CF6','#3A4A78'];

  const padL = 28, padR = 12, padT = 16, padB = 34;
  const plotW = w - padL - padR;
  const plotH = h - padT - padB;
  const maxVal = Math.max(...values) + 3;
  const barW = plotW / values.length * 0.5;
  const gap = plotW / values.length;

  ctx.strokeStyle = 'rgba(255,255,255,0.06)';
  ctx.font = '10px JetBrains Mono, monospace';
  ctx.fillStyle = 'rgba(169,183,214,0.7)';
  for(let i=0;i<=3;i++){
    const y = padT + (plotH/3)*i;
    ctx.beginPath(); ctx.moveTo(padL,y); ctx.lineTo(w-padR,y); ctx.stroke();
    ctx.fillText(Math.round(maxVal - (maxVal)*(i/3)), 2, y+3);
  }

  values.forEach((val,i)=>{
    const barH = (val/maxVal)*plotH;
    const x = padL + gap*i + (gap-barW)/2;
    const y = padT + plotH - barH;
    const grad = ctx.createLinearGradient(0,y,0,padT+plotH);
    grad.addColorStop(0, colors[i]);
    grad.addColorStop(1, colors[i]+'33');
    ctx.fillStyle = grad;
    drawRoundedRect(ctx, x, y, barW, barH, 6);
    ctx.fill();

    ctx.fillStyle = '#EAF0FB';
    ctx.font = '11px Inter, sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText(val, x+barW/2, y-6);

    ctx.fillStyle = 'rgba(169,183,214,0.85)';
    ctx.font = '10px Inter, sans-serif';
    const lines = labels[i].split('\n');
    lines.forEach((line,li)=> ctx.fillText(line, x+barW/2, h-30+li*11+8));
  });
}

/* Grafik donat: distribusi skor berkendara */
function drawScoreDonutChart(){
  const canvas = document.getElementById('chartScoreDonut');
  if(!canvas) return;
  const ratio = window.devicePixelRatio || 1;
  const size = 220;
  canvas.width = size*ratio; canvas.height = size*ratio;
  canvas.style.width = size+'px'; canvas.style.height = size+'px';
  const ctx = canvas.getContext('2d');
  ctx.scale(ratio,ratio);
  ctx.clearRect(0,0,size,size);

  const segments = [
    { val: 42, color:'#2FE0C9' },
    { val: 33, color:'#8B7CF6' },
    { val: 17, color:'#F5A623' },
    { val: 8,  color:'#FF4D5E' },
  ];
  const total = segments.reduce((a,s)=>a+s.val,0);
  const cx = size/2, cy = size/2, rOuter = 92, rInner = 60;
  let start = -Math.PI/2;

  segments.forEach(seg=>{
    const angle = (seg.val/total)*Math.PI*2;
    ctx.beginPath();
    ctx.arc(cx,cy,rOuter,start,start+angle);
    ctx.arc(cx,cy,rInner,start+angle,start,true);
    ctx.closePath();
    ctx.fillStyle = seg.color;
    ctx.fill();
    start += angle;
  });
}

/* Grafik area: pola jam berisiko sepanjang hari */
function drawHourlyRiskChart(){
  const canvas = document.getElementById('chartHourlyRisk');
  if(!canvas) return;
  const { ctx, w, h } = setupCanvasHiDPI(canvas);
  ctx.clearRect(0,0,w,h);

  const hours = ['06','08','10','12','14','16','18','20','22'];
  const risk = [12,18,22,35,58,64,40,28,15];

  const padL = 30, padR = 12, padT = 12, padB = 24;
  const plotW = w - padL - padR;
  const plotH = h - padT - padB;
  const maxVal = 70;

  ctx.beginPath();
  hours.forEach((hh,i)=>{
    const x = padL + (plotW/(hours.length-1))*i;
    const y = padT + plotH - (risk[i]/maxVal)*plotH;
    if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
  });
  ctx.lineTo(padL+plotW, padT+plotH);
  ctx.lineTo(padL, padT+plotH);
  ctx.closePath();
  const grad = ctx.createLinearGradient(0,padT,0,padT+plotH);
  grad.addColorStop(0, 'rgba(245,166,35,0.4)');
  grad.addColorStop(1, 'rgba(245,166,35,0.02)');
  ctx.fillStyle = grad;
  ctx.fill();

  ctx.beginPath();
  hours.forEach((hh,i)=>{
    const x = padL + (plotW/(hours.length-1))*i;
    const y = padT + plotH - (risk[i]/maxVal)*plotH;
    if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
  });
  ctx.strokeStyle = '#F5A623';
  ctx.lineWidth = 2.2;
  ctx.stroke();

  ctx.fillStyle = 'rgba(169,183,214,0.85)';
  ctx.font = '10.5px Inter, sans-serif';
  hours.forEach((hh,i)=>{
    const x = padL + (plotW/(hours.length-1))*i;
    ctx.textAlign = i===0 ? 'left' : i===hours.length-1 ? 'right' : 'center';
    ctx.fillText(hh+':00', x, h-6);
  });
}

function renderAllCharts(){
  drawAlertnessTrendChart();
  drawFatigueEventsChart();
  drawScoreDonutChart();
  drawHourlyRiskChart();
}

/* Sparkline mini untuk kartu ringkasan di dasbor utama */
function drawSparkline(canvasId, points, color){
  const canvas = document.getElementById(canvasId);
  if(!canvas) return;
  const ratio = window.devicePixelRatio || 1;
  const w = canvas.width, h = canvas.height;
  canvas.width = w*ratio; canvas.height = h*ratio;
  canvas.style.width = w+'px'; canvas.style.height = h+'px';
  const ctx = canvas.getContext('2d');
  ctx.scale(ratio,ratio);
  ctx.clearRect(0,0,w,h);

  const max = Math.max(...points), min = Math.min(...points);
  const range = (max-min) || 1;
  const stepX = w/(points.length-1);

  ctx.beginPath();
  points.forEach((p,i)=>{
    const x = i*stepX;
    const y = h - ((p-min)/range)*h*0.85 - 3;
    if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
  });
  ctx.strokeStyle = color;
  ctx.lineWidth = 1.8;
  ctx.lineJoin = 'round';
  ctx.stroke();

  ctx.lineTo(w,h); ctx.lineTo(0,h); ctx.closePath();
  const grad = ctx.createLinearGradient(0,0,0,h);
  grad.addColorStop(0, color+'33');
  grad.addColorStop(1, color+'00');
  ctx.fillStyle = grad;
  ctx.fill();
}

function renderDashboardSparklines(){
  drawSparkline('sparkTrips', Array.from({length:10}, ()=> randInt(1,5)), '#2FE0C9');
  drawSparkline('sparkAlertness', Array.from({length:10}, ()=> randInt(78,96)), '#2FE0C9');
  drawSparkline('sparkWarnings', Array.from({length:10}, ()=> randInt(0,4)), '#FF4D5E');
  drawSparkline('sparkDistance', Array.from({length:10}, ()=> randInt(20,70)), '#8B7CF6');
}

/* ----------------------------------------------------------------------
   12. KOMPONEN AKORDION FAQ
   ---------------------------------------------------------------------- */
function initFaqAccordion(){
  document.querySelectorAll('#faqAccordion .accordion-item').forEach(item=>{
    const head = item.querySelector('.accordion-head');
    const body = item.querySelector('.accordion-body');
    head.addEventListener('click', ()=>{
      const isOpen = item.classList.contains('open');
      document.querySelectorAll('#faqAccordion .accordion-item').forEach(other=>{
        other.classList.remove('open');
        other.querySelector('.accordion-body').style.maxHeight = null;
      });
      if(!isOpen){
        item.classList.add('open');
        body.style.maxHeight = body.scrollHeight + 'px';
      }
    });
  });
}

/* ----------------------------------------------------------------------
   13. INISIALISASI APLIKASI
   ---------------------------------------------------------------------- */
function initApp(){
  initAuth();
  initNavigation();
  initModals();
  initRealtimeSimulation();
  initHistoryPage();
  renderSafetyReports();
  initReportFilters();
  initFleetPage();
  initEmergencyPage();
  initFaqAccordion();
  renderDashboardSparklines();
  startCamera();
  startFaceDetection();

  window.addEventListener('resize', ()=>{
    const analyticsPage = document.getElementById('page-analytics');
    if(analyticsPage.classList.contains('active')) renderAllCharts();
  });
}

async function startCamera(){
  const video=document.getElementById("video");
  const stream=await navigator.mediaDevices.getUserMedia({
    video:true
  });
  video.srcObject=stream;
}
function testAlarm() {
    document.getElementById("alarm").play();
}

document.addEventListener('DOMContentLoaded', initApp);
</script>
<audio id="alarm" preload="auto">
    <source src="alarm.mp3.mpeg" type="audio/mpeg">
</audio>

</body>
</html>
