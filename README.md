<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#f5f0e8">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<title>Limpio y Rico</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
:root {
  --cream:    #f5f0e8;
  --cream2:   #ede7d9;
  --cream3:   #e0d8c8;
  --ink:      #1a1612;
  --ink2:     #3d3530;
  --ink3:     #6b6158;
  --ink4:     #9e9389;
  --gold:     #c9a84c;
  --gold2:    #e8c96a;
  --gold-bg:  #f9f3e3;
  --red:      #c0392b;
  --red-bg:   #fdf0ee;
  --green:    #2d6a4f;
  --green-bg: #edf7f2;
  --r:        14px;
  --shadow:   0 2px 20px rgba(26,22,18,0.08);
  --shadow-lg:0 8px 40px rgba(26,22,18,0.15);
}

*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }

html {
height: -webkit-fill-available;
background: var(–cream);
}
body {
font-family: ‘DM Sans’, sans-serif;
background: var(–cream);
color: var(–ink);
height: 100dvh;
height: -webkit-fill-available;
width: 100vw;
display: flex;
flex-direction: column;
overflow: hidden;
position: fixed;
top: 0; left: 0; right: 0; bottom: 0;
}

/* ── TOP BAR ── */
.topbar {
flex-shrink: 0;
background: var(–cream);
border-bottom: 1px solid var(–cream3);
display: flex;
align-items: center;
gap: 12px;
padding: 12px 16px;
padding-top: max(12px, env(safe-area-inset-top));
position: relative;
z-index: 10;
}

.logo {
display: flex;
align-items: baseline;
gap: 4px;
white-space: nowrap;
}
.logo-main {
font-family: ‘Playfair Display’, serif;
font-weight: 700;
font-size: 1.25rem;
color: var(–ink);
letter-spacing: -0.3px;
}
.logo-accent {
font-family: ‘Playfair Display’, serif;
font-weight: 400;
font-size: 1.25rem;
color: var(–gold);
font-style: italic;
}
.logo-dot {
width: 5px; height: 5px;
background: var(–gold);
border-radius: 50%;
margin-bottom: 2px;
flex-shrink: 0;
}

.searchbox {
flex: 1; min-width: 0;
display: flex; align-items: center; gap: 8px;
background: var(–cream2);
border: 1.5px solid var(–cream3);
border-radius: 50px;
padding: 9px 14px;
transition: border-color .2s;
}
.searchbox:focus-within {
border-color: var(–gold);
background: #fff;
}
.searchbox input {
flex: 1; min-width: 0;
background: none; border: none; outline: none;
color: var(–ink); font-size: 0.85rem;
font-family: ‘DM Sans’, sans-serif;
}
.searchbox input::placeholder { color: var(–ink4); }
.search-icon { color: var(–ink4); font-size: 0.9rem; flex-shrink: 0; }

/* ── FILTROS ── */
.filters {
flex-shrink: 0;
background: var(–cream);
border-bottom: 1px solid var(–cream3);
display: flex; gap: 6px;
padding: 8px 14px;
overflow-x: auto;
scrollbar-width: none;
-webkit-overflow-scrolling: touch;
}
.filters::-webkit-scrollbar { display: none; }

.chip {
flex-shrink: 0;
display: flex; align-items: center; gap: 5px;
padding: 6px 13px; border-radius: 50px;
border: 1.5px solid var(–cream3);
background: var(–cream2);
color: var(–ink3);
font-size: 0.76rem; font-weight: 500;
font-family: ‘DM Sans’, sans-serif;
white-space: nowrap; cursor: pointer;
transition: all .18s;
-webkit-user-select: none; user-select: none;
touch-action: manipulation;
}
.chip:active { transform: scale(0.96); }
.chip.on {
background: var(–ink);
border-color: var(–ink);
color: var(–cream);
}
.chip.on-gold {
background: var(–gold-bg);
border-color: var(–gold);
color: var(–ink2);
}
.chip-dot {
width: 6px; height: 6px;
border-radius: 50%;
background: currentColor;
opacity: .5;
}

/* ── MAPA ── */
#map {
flex: 1;
min-height: 0;
width: 100%;
z-index: 1;
background: var(–cream2);
}

/* ── BOTTOM SHEET ── */
.sheet {
flex-shrink: 0;
background: var(–cream);
border-top: 2px solid var(–cream3);
display: flex;
flex-direction: column;
height: 36vh;
min-height: 170px;
transition: height .32s cubic-bezier(.4,0,.2,1);
z-index: 100;
box-shadow: 0 -4px 24px rgba(26,22,18,0.07);
}
.sheet.big { height: 66vh; }

.sheet-top {
flex-shrink: 0;
display: flex; align-items: center; justify-content: space-between;
padding: 10px 16px 8px;
cursor: pointer;
-webkit-user-select: none; user-select: none;
}
.handle {
width: 32px; height: 3px;
background: var(–cream3);
border-radius: 4px;
margin: 0 auto 8px;
transition: background .2s;
}
.sheet-top:active .handle { background: var(–ink4); }

.sheet-meta { flex: 1; }
.sheet-label {
font-family: ‘Playfair Display’, serif;
font-size: 0.95rem; font-weight: 600;
color: var(–ink);
display: flex; align-items: center; gap: 8px;
}
.km-pill {
font-family: ‘DM Sans’, sans-serif;
font-size: 0.63rem; font-weight: 600;
color: var(–gold);
background: var(–gold-bg);
border: 1px solid rgba(201,168,76,.3);
border-radius: 50px;
padding: 2px 8px;
letter-spacing: .3px;
}
.sheet-count {
font-size: 0.72rem;
color: var(–ink4);
margin-top: 2px;
}
.sheet-count b { color: var(–ink2); font-weight: 600; }

.expand-btn {
font-size: 0.71rem; font-weight: 500;
color: var(–ink3);
background: var(–cream2);
border: 1.5px solid var(–cream3);
border-radius: 50px;
padding: 5px 11px;
cursor: pointer;
touch-action: manipulation;
transition: all .15s;
flex-shrink: 0;
}
.expand-btn:active { background: var(–cream3); }

/* ── LISTA ── */
.cards {
flex: 1;
overflow-y: scroll;
-webkit-overflow-scrolling: touch;
overscroll-behavior: contain;
touch-action: pan-y;
padding: 2px 12px 16px;
display: flex; flex-direction: column; gap: 8px;
padding-bottom: max(16px, env(safe-area-inset-bottom));
}

/* ── CARD ── */
.card {
flex-shrink: 0;
background: #fff;
border: 1.5px solid var(–cream3);
border-radius: var(–r);
padding: 14px 15px;
cursor: pointer;
transition: all .18s;
box-shadow: var(–shadow);
position: relative;
overflow: hidden;
}
.card::before {
content: ‘’;
position: absolute;
left: 0; top: 0; bottom: 0;
width: 3px;
background: var(–cream3);
transition: background .18s;
}
.card.c-tacc::before  { background: var(–gold); }
.card.c-vegan::before { background: var(–green); }
.card.c-ambos::before { background: linear-gradient(to bottom, var(–gold), var(–green)); }
.card.c-diet::before  { background: var(–green); }
.card.sel {
border-color: var(–ink2);
box-shadow: var(–shadow-lg);
transform: translateY(-1px);
}
.card:active { transform: scale(0.985); }

.card-header { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:4px; }
.card-name {
font-family: ‘Playfair Display’, serif;
font-size: 0.95rem; font-weight: 600;
color: var(–ink); line-height: 1.25;
flex: 1; min-width: 0;
}
.card-dist {
font-size: 0.7rem; font-weight: 600;
color: var(–ink4);
margin-left: 10px; white-space: nowrap;
flex-shrink: 0;
background: var(–cream2);
padding: 2px 8px; border-radius: 50px;
}
.card-type { font-size: 0.73rem; color: var(–ink4); margin-bottom: 8px; }
.card-tags { display:flex; flex-wrap:wrap; gap:4px; }

.tag {
font-size: 0.63rem; font-weight: 600;
padding: 3px 8px; border-radius: 50px;
letter-spacing: .2px;
}
.tag-tacc  { background: var(–gold-bg);  color: var(–ink2); border: 1px solid rgba(201,168,76,.35); }
.tag-vegan { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.25); }
.tag-veg   { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.2); opacity:.8; }
.tag-diet  { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.25); }
.tag-nd    { background: var(–cream2);   color: var(–ink4);  border: 1px solid var(–cream3); }

/* ── PANEL DETALLE ── */
.detail {
position: fixed; inset: 0;
background: var(–cream);
z-index: 9000;
display: flex; flex-direction: column;
transform: translateY(100%);
transition: transform .3s cubic-bezier(.4,0,.2,1);
}
.detail.open { transform: translateY(0); }

.det-nav {
flex-shrink: 0;
display: flex; align-items: center; justify-content: space-between;
padding: 12px 16px;
padding-top: max(12px, env(safe-area-inset-top));
border-bottom: 1px solid var(–cream3);
background: var(–cream);
}
.back-btn {
display: flex; align-items: center; gap: 6px;
background: var(–cream2); border: 1.5px solid var(–cream3);
color: var(–ink2); border-radius: 50px;
padding: 7px 14px; font-size: 0.82rem; font-weight: 500;
font-family: ‘DM Sans’, sans-serif;
cursor: pointer; touch-action: manipulation;
transition: all .15s;
}
.back-btn:active { background: var(–cream3); }

.det-body {
flex: 1; overflow-y: auto;
-webkit-overflow-scrolling: touch;
background: var(–cream);
}

/* Hero del detalle */
.det-hero {
padding: 24px 18px 16px;
border-bottom: 1px solid var(–cream3);
background: #fff;
}
.det-tipo-row {
display: flex; align-items: center; gap: 8px;
margin-bottom: 8px;
}
.det-tipo-icon {
font-size: 1.5rem;
width: 42px; height: 42px;
background: var(–cream);
border: 1.5px solid var(–cream3);
border-radius: 10px;
display: flex; align-items: center; justify-content: center;
}
.det-tipo-info { flex: 1; }
.det-tipo-label { font-size: 0.72rem; color: var(–ink4); font-weight: 500; }
.det-dist-badge {
font-size: 0.72rem; font-weight: 700;
background: var(–ink); color: var(–cream);
padding: 4px 10px; border-radius: 50px;
}
.det-name {
font-family: ‘Playfair Display’, serif;
font-size: 1.5rem; font-weight: 700;
color: var(–ink); line-height: 1.2;
margin-bottom: 10px;
}
.det-tags { display:flex; gap:6px; flex-wrap:wrap; }

/* Secciones del detalle */
.det-section {
padding: 16px 18px;
border-bottom: 1px solid var(–cream3);
}
.det-section:last-child { border-bottom: none; }
.det-sec-title {
font-size: 0.65rem; font-weight: 700;
text-transform: uppercase; letter-spacing: 1.2px;
color: var(–ink4); margin-bottom: 12px;
}
.info-row {
display: flex; align-items: flex-start; gap: 12px;
margin-bottom: 10px;
}
.info-row:last-child { margin-bottom: 0; }
.info-ic {
width: 32px; height: 32px; flex-shrink: 0;
background: var(–cream2); border: 1px solid var(–cream3);
border-radius: 8px;
display: flex; align-items: center; justify-content: center;
font-size: 0.9rem;
}
.info-content { flex: 1; min-width: 0; }
.info-label { font-size: 0.68rem; color: var(–ink4); font-weight: 500; margin-bottom: 1px; }
.info-value {
font-size: 0.84rem; color: var(–ink2); font-weight: 400;
word-break: break-word; line-height: 1.4;
}
.info-value a { color: var(–ink2); text-decoration: underline; text-underline-offset: 2px; }

/* Reseñas simuladas (OSM notes) */
.review-item {
background: var(–cream2);
border-radius: 10px;
padding: 10px 12px;
margin-bottom: 8px;
}
.review-item:last-child { margin-bottom: 0; }
.review-stars { color: var(–gold); font-size: 0.75rem; margin-bottom: 4px; }
.review-text { font-size: 0.8rem; color: var(–ink2); line-height: 1.5; }
.review-author { font-size: 0.68rem; color: var(–ink4); margin-top: 5px; }

.osm-note {
font-size: 0.75rem; color: var(–ink4);
background: var(–cream2); border-radius: 8px;
padding: 8px 10px; line-height: 1.5;
border-left: 2px solid var(–gold);
}

/* Footer detalle */
.det-foot {
flex-shrink: 0;
display: flex; gap: 8px;
padding: 12px 16px;
padding-bottom: max(12px, env(safe-area-inset-bottom));
border-top: 1.5px solid var(–cream3);
background: var(–cream);
}
.btn-primary {
flex: 2; padding: 13px;
background: var(–ink); color: var(–cream);
border: none; border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-weight: 600; font-size: 0.88rem;
cursor: pointer; touch-action: manipulation;
transition: all .15s;
display: flex; align-items: center; justify-content: center; gap: 6px;
}
.btn-primary:active { background: var(–ink2); transform: scale(0.97); }
.btn-secondary {
flex: 1; padding: 13px;
background: var(–cream2); color: var(–ink2);
border: 1.5px solid var(–cream3); border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-weight: 500; font-size: 0.84rem;
cursor: pointer; touch-action: manipulation;
transition: all .15s;
}
.btn-secondary:active { background: var(–cream3); }

/* ── PERMISO ── */
.perm {
position: fixed; inset: 0;
background: var(–cream);
z-index: 9999;
display: flex; flex-direction: column;
align-items: center; justify-content: center;
padding: 32px 24px; gap: 18px;
text-align: center;
}

.perm-logo {
display: flex; flex-direction: column;
align-items: center; gap: 6px;
}
.perm-logo-mark {
width: 64px; height: 64px;
background: var(–ink);
border-radius: 18px;
display: flex; align-items: center; justify-content: center;
font-size: 1.8rem;
margin-bottom: 4px;
box-shadow: 0 8px 24px rgba(26,22,18,0.2);
}
.perm-app-name {
font-family: ‘Playfair Display’, serif;
font-size: 1.9rem; font-weight: 700;
color: var(–ink); line-height: 1;
}
.perm-app-name em { color: var(–gold); font-style: italic; }
.perm-tagline { font-size: 0.88rem; color: var(–ink3); max-width: 260px; line-height: 1.6; }

.perm-feats {
display: flex; flex-direction: column; gap: 8px;
width: 100%; max-width: 300px;
}
.perm-feat {
display: flex; align-items: center; gap: 12px;
background: #fff;
border: 1.5px solid var(–cream3);
border-radius: 12px; padding: 11px 14px;
text-align: left;
box-shadow: var(–shadow);
}
.feat-ic {
width: 36px; height: 36px; flex-shrink: 0;
background: var(–cream2); border-radius: 9px;
display: flex; align-items: center; justify-content: center;
font-size: 1rem;
}
.feat-text b { display:block; font-size:0.82rem; color:var(–ink); font-weight:600; margin-bottom:1px; }
.feat-text span { font-size:0.73rem; color:var(–ink4); }

.perm-err {
font-size: 0.79rem; color: var(–red);
background: var(–red-bg);
border: 1px solid rgba(192,57,43,.2);
border-radius: 10px; padding: 10px 14px;
max-width: 300px; display: none;
line-height: 1.6; white-space: pre-line;
text-align: left;
}

#btnUbicacion {
width: 100%; max-width: 300px; padding: 15px;
background: var(–ink); color: var(–cream);
border: none; border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-size: 0.95rem; font-weight: 600;
cursor: pointer; touch-action: manipulation;
-webkit-tap-highlight-color: transparent;
display: flex; align-items: center; justify-content: center; gap: 8px;
box-shadow: 0 4px 16px rgba(26,22,18,0.25);
transition: all .15s;
}
#btnUbicacion:active { transform: scale(0.97); background: var(–ink2); }
.perm-note { font-size: 0.68rem; color: var(–ink4); max-width: 240px; }

/* ── LOADING ── */
.loading {
position: fixed; inset: 0;
background: var(–cream); z-index: 8000;
display: none; flex-direction: column;
align-items: center; justify-content: center; gap: 14px;
}
.load-logo {
font-family: ‘Playfair Display’, serif;
font-size: 1.4rem; font-weight: 700; color: var(–ink);
}
.load-logo em { color: var(–gold); font-style: italic; }
.spinner {
width: 40px; height: 40px;
border: 2.5px solid var(–cream3);
border-top-color: var(–ink);
border-radius: 50%;
animation: spin .7s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
.ltxt { font-size: 0.83rem; color: var(–ink3); }
.lsub { font-size: 0.72rem; color: var(–ink4); margin-top: -8px; }

/* ── TOAST ── */
.toast {
position: fixed;
bottom: calc(36vh + 12px); left: 50%;
transform: translateX(-50%) translateY(6px);
background: var(–ink); color: var(–cream);
border-radius: 50px; padding: 8px 16px;
font-size: 0.78rem; font-weight: 500;
opacity: 0; pointer-events: none;
transition: all .22s; z-index: 5000;
white-space: nowrap; max-width: 88vw; text-align: center;
box-shadow: 0 4px 20px rgba(26,22,18,0.2);
}
.toast.on  { opacity:1; transform:translateX(-50%) translateY(0); }
.toast.err { background: var(–red); }
.toast.ok  { background: var(–green); }

/* ── EMPTY STATE ── */
.empty {
display: flex; flex-direction: column;
align-items: center; justify-content: center;
padding: 28px 20px; text-align: center; gap: 8px;
}
.empty-icon { font-size: 2rem; margin-bottom: 4px; }
.empty-title { font-family: ‘Playfair Display’, serif; font-size: 1rem; color: var(–ink2); }
.empty-sub { font-size: 0.78rem; color: var(–ink4); line-height: 1.5; }

/* ── LEAFLET CUSTOM ── */
.leaflet-tile { filter: sepia(.15) saturate(.8) brightness(.97); }
.leaflet-control-zoom {
border: none !important;
box-shadow: var(–shadow) !important;
}
.leaflet-control-zoom a {
background: #fff !important;
color: var(–ink) !important;
border: 1.5px solid var(–cream3) !important;
font-size: 1rem !important;
line-height: 26px !important;
}
.leaflet-control-zoom a:hover { background: var(–cream2) !important; }
.leaflet-popup-content-wrapper {
background: #fff !important; color: var(–ink) !important;
border: 1.5px solid var(–cream3) !important;
border-radius: 12px !important;
box-shadow: var(–shadow-lg) !important;
font-family: ‘DM Sans’, sans-serif !important;
}
.leaflet-popup-tip { background: #fff !important; }

/* ── RESPONSIVE ── */
@media (min-width: 768px) {
.main-layout { display: flex; flex: 1; min-height: 0; overflow: hidden; }
#map { flex: 1; height: 100%; }
.sheet {
width: 360px; height: 100% !important;
border-top: none; border-left: 2px solid var(–cream3);
flex-direction: column;
box-shadow: none;
}
.sheet.big { width: 420px; }
.handle { display: none; }
.expand-btn { display: none; }
.toast { bottom: 20px; }
.detail { max-width: 420px; right: auto; border-right: 1px solid var(–cream3); }
}
</style>

</head>
<body>

<!-- PERMISO -->

<div class="perm" id="pantallaPerm">
  <div class="perm-logo">
    <div class="perm-logo-mark">🌾</div>
    <div class="perm-app-name">Limpio y <em>Rico</em></div>
  </div>
  <div class="perm-tagline">Encontra restaurantes sin TACC y veganos exactamente donde estas.</div>
  <div class="perm-feats">
    <div class="perm-feat">
      <div class="feat-ic">🌾</div>
      <div class="feat-text"><b>Sin TACC</b><span>Locales certificados y con opciones libres de gluten</span></div>
    </div>
    <div class="perm-feat">
      <div class="feat-ic">🌿</div>
      <div class="feat-text"><b>Vegano y Vegetariano</b><span>Cafeterias y restaurantes plant-based</span></div>
    </div>
    <div class="perm-feat">
      <div class="feat-ic">📍</div>
      <div class="feat-text"><b>10 km a la redonda</b><span>Busqueda en tiempo real con tu GPS</span></div>
    </div>
  </div>
  <div class="perm-err" id="errorPerm"></div>
  <button id="btnUbicacion"><span>📍</span> Usar mi ubicacion</button>
  <div class="perm-note">Solo usamos tu GPS para buscar lugares cercanos. No guardamos datos.</div>
</div>

<!-- LOADING -->

<div class="loading" id="pantallaLoading">
  <div class="load-logo">Limpio y <em>Rico</em></div>
  <div class="spinner"></div>
  <div class="ltxt" id="textoLoad">Obteniendo ubicacion...</div>
  <div class="lsub" id="subLoad"></div>
</div>

<!-- APP -->

<div class="topbar">
  <div class="logo">
    <div class="logo-dot"></div>
    <span class="logo-main">Limpio y</span>
    <span class="logo-accent">Rico</span>
  </div>
  <div class="searchbox">
    <span class="search-icon">🔍</span>
    <input type="text" id="inputBuscar" placeholder="Buscar restaurante...">
  </div>
</div>

<div class="filters" id="filtros">
  <div class="chip on" data-f="todos"><span class="chip-dot"></span>Todos</div>
  <div class="chip" data-f="tacc">🌾 Sin TACC</div>
  <div class="chip" data-f="vegan">🌿 Vegano</div>
  <div class="chip" data-f="ambos">✦ TACC + Vegano</div>
  <div class="chip" data-f="cafe">☕ Cafeterias</div>
  <div class="chip" data-f="dietetica">💚 Dieteticas</div>
</div>

<!-- Layout adaptable desktop/mobile -->

<div id="mainLayout" style="flex:1;min-height:0;display:flex;flex-direction:column;overflow:hidden;">
  <div id="map"></div>

  <div class="sheet" id="sheet">
    <div class="sheet-top" id="sheetTop">
      <div class="sheet-meta">
        <div class="handle"></div>
        <div class="sheet-label">
          Lugares cercanos
          <span class="km-pill">10 km</span>
        </div>
        <div class="sheet-count">
          <b id="contador">0</b> resultados · del mas cercano al mas lejano
        </div>
      </div>
      <div class="expand-btn" id="expandBtn">▲ Ver lista</div>
    </div>
    <div class="cards" id="listaCards">
      <div class="empty">
        <div class="empty-icon">📍</div>
        <div class="empty-title">Activa tu ubicacion</div>
        <div class="empty-sub">Acepta el permiso para encontrar lugares cerca tuyo</div>
      </div>
    </div>
  </div>
</div>

<!-- DETALLE -->

<div class="detail" id="pantallaDetalle">
  <div class="det-nav">
    <button class="back-btn" id="btnVolver">← Volver</button>
    <span style="font-size:.78rem;color:var(--ink4);font-weight:500">Detalle del lugar</span>
  </div>
  <div class="det-body" id="detBody"></div>
  <div class="det-foot">
    <button class="btn-primary" id="btnNav">🧭 Como llegar</button>
    <button class="btn-secondary" id="btnOsm">Ver en OSM</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
var mapa, uLat, uLng;
var todos=[], visibles=[], marcadores={};
var selId=null, filtro='todos', sheetBig=false;
var lugarDetalle=null;

var SERVERS=[
  'https://overpass-api.de/api/interpreter',
  'https://overpass.kumi.systems/api/interpreter',
  'https://maps.mail.ru/osm/tools/overpass/api/interpreter'
];

// ── RESPONSIVE LAYOUT ─────────────────────────────────────────────────────
function ajustarLayout(){
  var isDesktop = window.innerWidth >= 768;
  var layout = document.getElementById('mainLayout');
  var sheet = document.getElementById('sheet');
  if(isDesktop){
    layout.style.flexDirection='row';
    sheet.style.width='360px';
    sheet.style.height='100%';
    sheet.style.maxHeight='100%';
    sheet.style.borderTop='none';
    sheet.style.borderLeft='2px solid var(--cream3)';
  } else {
    layout.style.flexDirection='column';
    sheet.style.width='100%';
    sheet.style.borderLeft='none';
    sheet.style.borderTop='2px solid var(--cream3)';
    if(!sheetBig){
      sheet.style.height='36vh';
      sheet.style.maxHeight='36vh';
    }
  }
  if(mapa) setTimeout(function(){ mapa.invalidateSize(); }, 350);
}
window.addEventListener('resize', ajustarLayout);

// ── INIT ──────────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', function(){
  ajustarLayout();
  document.getElementById('btnUbicacion').addEventListener('click', pedirUbicacion);
  document.getElementById('inputBuscar').addEventListener('input', aplicarFiltros);
  document.getElementById('filtros').addEventListener('click', function(e){
    var c=e.target.closest('[data-f]'); if(c) setFiltro(c.getAttribute('data-f'));
  });
  document.getElementById('sheetTop').addEventListener('click', toggleSheet);
  document.getElementById('expandBtn').addEventListener('click', function(e){ e.stopPropagation(); toggleSheet(); });
  document.getElementById('btnVolver').addEventListener('click', cerrarDetalle);
});

// ── UBICACION ─────────────────────────────────────────────────────────────
function pedirUbicacion(){
  if(!navigator.geolocation){ mostrarError('Tu navegador no soporta GPS.'); return; }
  document.getElementById('pantallaPerm').style.display='none';
  document.getElementById('pantallaLoading').style.display='flex';
  setL('Obteniendo tu ubicacion...');

  navigator.geolocation.getCurrentPosition(
    function(pos){
      uLat=pos.coords.latitude; uLng=pos.coords.longitude;
      setL('Iniciando mapa...');
      iniciarMapa();
      setL('Buscando lugares en 10 km...');
      buscar(0);
    },
    function(err){
      document.getElementById('pantallaLoading').style.display='none';
      var m='No se pudo obtener la ubicacion.';
      if(err.code===1) m='Permiso denegado.\n\nVe a Ajustes iPhone > Privacidad > Ubicacion > Chrome > "Al usar la app".\nLuego recarga la pagina.';
      if(err.code===2) m='GPS no disponible. Conectate a WiFi o activa la ubicacion.';
      if(err.code===3) m='Tiempo agotado. Intenta de nuevo.';
      mostrarError(m);
    },
    {enableHighAccuracy:true, timeout:15000, maximumAge:0}
  );
}

function mostrarError(m){
  var e=document.getElementById('errorPerm');
  e.textContent=m; e.style.display='block';
  document.getElementById('pantallaPerm').style.display='flex';
  document.getElementById('btnUbicacion').innerHTML='<span>🔄</span> Reintentar';
}
function setL(t){ document.getElementById('textoLoad').textContent=t; }
function setS(t){ document.getElementById('subLoad').textContent=t; }

// ── MAPA ──────────────────────────────────────────────────────────────────
function iniciarMapa(){
  mapa=L.map('map',{zoomControl:false,attributionControl:false}).setView([uLat,uLng],13);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19}).addTo(mapa);
  L.control.zoom({position:'bottomright'}).addTo(mapa);

  // Radio 10km
  L.circle([uLat,uLng],{
    radius:10000, color:'#c9a84c', weight:1.5,
    fillColor:'#c9a84c', fillOpacity:.03, dashArray:'5,8'
  }).addTo(mapa);

  // Marker usuario
  var icoYo=L.divIcon({className:'',
    html:'<div style="width:20px;height:20px;background:#1a1612;border:3px solid #fff;border-radius:50%;box-shadow:0 0 0 6px rgba(26,22,18,.15)"></div>',
    iconSize:[20,20],iconAnchor:[10,10]});
  L.marker([uLat,uLng],{icon:icoYo,zIndexOffset:9999}).addTo(mapa)
    .bindPopup('<b style="font-family:\'DM Sans\',sans-serif">Tu ubicacion</b>');

  mapa.on('click',function(){ selId=null; renderMarcadores(); });
  ajustarLayout();
}

// ── OVERPASS CON FALLBACK ─────────────────────────────────────────────────
function buscar(idx){
  if(idx>=SERVERS.length){
    document.getElementById('pantallaLoading').style.display='none';
    document.getElementById('listaCards').innerHTML=
      '<div class="empty"><div class="empty-icon">📡</div>'
      +'<div class="empty-title">Sin conexion</div>'
      +'<div class="empty-sub">Verifica tu internet y recarga la pagina.</div>'
      +'<br><button onclick="location.reload()" style="margin-top:8px;background:var(--ink);color:var(--cream);border:none;padding:10px 20px;border-radius:50px;font-weight:600;font-size:.83rem;cursor:pointer;font-family:\'DM Sans\',sans-serif">Reintentar</button>'
      +'</div>';
    return;
  }
  setS('Servidor '+(idx+1)+' de '+SERVERS.length+'...');

  var r=10000;
  var q='[out:json][timeout:60];('
    +'node["diet:gluten_free"](around:'+r+','+uLat+','+uLng+');'
    +'node["diet:vegan"](around:'+r+','+uLat+','+uLng+');'
    +'node["diet:vegetarian"](around:'+r+','+uLat+','+uLng+');'
    +'node["amenity"="restaurant"](around:'+r+','+uLat+','+uLng+');'
    +'node["amenity"="cafe"](around:'+r+','+uLat+','+uLng+');'
    +'node["amenity"="fast_food"](around:'+r+','+uLat+','+uLng+');'
    +'node["shop"="bakery"](around:'+r+','+uLat+','+uLng+');'
    +'node["shop"="health_food"](around:'+r+','+uLat+','+uLng+');'
    +'node["shop"="organic"](around:'+r+','+uLat+','+uLng+');'
    +'node["shop"="dietetic"](around:'+r+','+uLat+','+uLng+');'
    +'way["diet:gluten_free"](around:'+r+','+uLat+','+uLng+');'
    +'way["diet:vegan"](around:'+r+','+uLat+','+uLng+');'
    +'way["amenity"="restaurant"](around:'+r+','+uLat+','+uLng+');'
    +'way["amenity"="cafe"](around:'+r+','+uLat+','+uLng+');'
    +'way["shop"="bakery"](around:'+r+','+uLat+','+uLng+');'
    +'way["shop"="health_food"](around:'+r+','+uLat+','+uLng+');'
    +'way["shop"="organic"](around:'+r+','+uLat+','+uLng+');'
    +');out center tags;';

  var ctrl=new AbortController();
  var tid=setTimeout(function(){ ctrl.abort(); },22000);
  fetch(SERVERS[idx],{method:'POST',body:'data='+encodeURIComponent(q),signal:ctrl.signal})
  .then(function(r){ clearTimeout(tid); if(!r.ok) throw new Error(); return r.json(); })
  .then(function(d){ if(!d.elements) throw new Error(); procesarLugares(d.elements); })
  .catch(function(){ clearTimeout(tid); setTimeout(function(){ buscar(idx+1); },600); });
}

// ── PROCESAR ──────────────────────────────────────────────────────────────
function procesarLugares(els){
  var vistos={};
  todos=[];
  for(var i=0;i<els.length;i++){
    var e=els[i];
    if(!e.tags||!e.tags.name||vistos[e.id]) continue;
    vistos[e.id]=true;
    var t=e.tags;
    var lat=e.lat||(e.center&&e.center.lat);
    var lng=e.lon||(e.center&&e.center.lon);
    if(!lat||!lng) continue;

    var gf=t['diet:gluten_free']==='yes'||t['diet:gluten_free']==='only';
    var ve=t['diet:vegan']==='yes'||t['diet:vegan']==='only';
    var vg=t['diet:vegetarian']==='yes'||t['diet:vegetarian']==='only';
    var am=t.amenity||t.shop||'';
    var esDiet=am==='health_food'||am==='organic'||am==='dietetic';
    if(esDiet&&!gf) gf=true;

    var cat='none';
    if(gf&&ve) cat='ambos'; else if(gf) cat='celiac'; else if(ve) cat='vegan'; else if(vg) cat='vegetarian';

    var tipo='Restaurante';
    if(am==='cafe') tipo='Cafeteria';
    else if(am==='bakery') tipo='Panaderia';
    else if(am==='fast_food') tipo='Fast Food';
    else if(esDiet) tipo='Dietetica Natural';

    var dist=calcDist(uLat,uLng,lat,lng);

    // Info completa
    var info=[];
    if(gf&&t['diet:gluten_free']==='only') info.push({ic:'🌾',lab:'TACC',val:'100% Sin TACC (certificado en OSM)'});
    else if(gf) info.push({ic:'🌾',lab:'TACC',val:'Opciones sin TACC disponibles'});
    if(ve&&t['diet:vegan']==='only') info.push({ic:'🌿',lab:'Dieta',val:'100% Vegano'});
    else if(ve) info.push({ic:'🌿',lab:'Dieta',val:'Opciones veganas'});
    if(vg) info.push({ic:'🥗',lab:'Dieta',val:'Opciones vegetarianas'});
    if(esDiet) info.push({ic:'💚',lab:'Tipo',val:'Dietetica / tienda naturista'});
    if(t.cuisine) info.push({ic:'🍴',lab:'Cocina',val:t.cuisine.replace(/_/g,' ')});
    if(t.opening_hours) info.push({ic:'🕐',lab:'Horario',val:t.opening_hours});
    if(t['addr:street']) info.push({ic:'📌',lab:'Direccion',val:t['addr:street']+(t['addr:housenumber']?' '+t['addr:housenumber']:'')+(t['addr:city']?', '+t['addr:city']:'')+(t['addr:postcode']?' ('+t['addr:postcode']+')':'')});
    if(t.phone||t['contact:phone']) info.push({ic:'📞',lab:'Telefono',val:t.phone||t['contact:phone']});
    if(t['contact:whatsapp']) info.push({ic:'💬',lab:'WhatsApp',val:t['contact:whatsapp']});
    if(t.website||t['contact:website']) info.push({ic:'🌐',lab:'Web',val:(t.website||t['contact:website']).replace(/^https?:\/\//,'').slice(0,45)});
    if(t['contact:instagram']||t['contact:facebook']) info.push({ic:'📷',lab:'Redes',val:(t['contact:instagram']?'Instagram: @'+t['contact:instagram'].replace(/^@/,''):'')+(t['contact:facebook']?' · Facebook: '+t['contact:facebook'].replace(/^https?:\/\/(www\.)?facebook\.com\//,''):'')}); 
    if(t.email||t['contact:email']) info.push({ic:'✉️',lab:'Email',val:t.email||t['contact:email']});
    if(t.description) info.push({ic:'📝',lab:'Descripcion',val:t.description});
    if(t.note) info.push({ic:'💡',lab:'Nota',val:t.note});
    if(!info.length) info.push({ic:'ℹ️',lab:'Info',val:'Sin informacion adicional en OpenStreetMap.'});

    todos.push({
      id:e.id, nombre:t.name, tipo:tipo, cat:cat,
      lat:lat, lng:lng, dist:dist, info:info,
      osmId:e.id, osmTipo:e.type||'node', esDiet:esDiet,
      tags:t
    });
  }

  // Orden: de menor a mayor distancia
  todos.sort(function(a,b){ return a.dist-b.dist; });

  document.getElementById('pantallaLoading').style.display='none';
  aplicarFiltros();
  var cD=todos.filter(function(p){return p.cat!=='none';}).length;
  toast('✅ '+todos.length+' lugares · '+cD+' con datos dieteticos','ok');
}

function calcDist(a,b,c,d){
  var R=6371000,dA=(c-a)*Math.PI/180,dB=(d-b)*Math.PI/180;
  var x=Math.sin(dA/2)*Math.sin(dA/2)+Math.cos(a*Math.PI/180)*Math.cos(c*Math.PI/180)*Math.sin(dB/2)*Math.sin(dB/2);
  return Math.round(R*2*Math.atan2(Math.sqrt(x),Math.sqrt(1-x)));
}
function fmt(m){ return m<1000?m+'m':(m/1000).toFixed(1)+' km'; }

// ── FILTROS ───────────────────────────────────────────────────────────────
function setFiltro(f){
  filtro=f;
  document.querySelectorAll('.chip').forEach(function(c){ c.className='chip'; });
  var ch=document.querySelector('[data-f="'+f+'"]');
  if(ch) ch.classList.add(f==='ambos'?'on-gold':'on');
  aplicarFiltros();
}

function aplicarFiltros(){
  var q=document.getElementById('inputBuscar').value.toLowerCase();
  visibles=todos.filter(function(p){
    var mf=filtro==='todos'     ?true
          :filtro==='tacc'      ?(p.cat==='celiac'||p.cat==='ambos')
          :filtro==='vegan'     ?(p.cat==='vegan'||p.cat==='ambos')
          :filtro==='ambos'     ?p.cat==='ambos'
          :filtro==='cafe'      ?p.tipo==='Cafeteria'
          :filtro==='dietetica' ?p.esDiet
          :true;
    return mf&&(q?p.nombre.toLowerCase().indexOf(q)>=0:true);
  });
  document.getElementById('contador').textContent=visibles.length;
  renderLista();
  if(mapa) renderMarcadores();
}

// ── HELPERS ───────────────────────────────────────────────────────────────
function emo(t){
  return t==='Cafeteria'?'☕':t==='Panaderia'?'🥖':t==='Fast Food'?'🍔':t==='Dietetica Natural'?'🌿':'🍽️';
}
function col(c){
  return c==='celiac'?'#c9a84c':c==='vegan'?'#2d6a4f':c==='ambos'?'#8b5a00':c==='vegetarian'?'#4a7c59':'#9e9389';
}
function catCls(p){
  return p.cat==='celiac'?' c-tacc':p.cat==='vegan'?' c-vegan':p.cat==='ambos'?' c-ambos':p.esDiet?' c-diet':'';
}

// ── RENDER LISTA ──────────────────────────────────────────────────────────
function renderLista(){
  var el=document.getElementById('listaCards');
  if(!visibles.length){
    el.innerHTML='<div class="empty"><div class="empty-icon">'+(filtro!=='todos'?'🔍':'🌾')+'</div>'
      +'<div class="empty-title">Sin resultados</div>'
      +'<div class="empty-sub">'+(filtro!=='todos'?'Proba con el filtro "Todos" para ver mas opciones.':'No encontramos lugares en 10 km.')+'</div></div>';
    return;
  }
  el.innerHTML='';
  var max=Math.min(visibles.length,80);
  for(var i=0;i<max;i++){
    (function(p){
      var div=document.createElement('div');
      div.className='card'+catCls(p)+(selId===p.id?' sel':'');
      div.id='card'+p.id;

      var tagsHtml='';
      if(p.cat==='celiac'||p.cat==='ambos') tagsHtml+='<span class="tag tag-tacc">Sin TACC</span>';
      if(p.cat==='vegan'||p.cat==='ambos')  tagsHtml+='<span class="tag tag-vegan">Vegano</span>';
      if(p.cat==='vegetarian') tagsHtml+='<span class="tag tag-veg">Vegetariano</span>';
      if(p.esDiet) tagsHtml+='<span class="tag tag-diet">Dietetica</span>';
      if(p.cat==='none'&&!p.esDiet) tagsHtml+='<span class="tag tag-nd">Sin datos</span>';

      div.innerHTML=
        '<div class="card-header">'
          +'<div class="card-name">'+p.nombre+'</div>'
          +'<div class="card-dist">'+fmt(p.dist)+'</div>'
        +'</div>'
        +'<div class="card-type">'+emo(p.tipo)+' '+p.tipo+'</div>'
        +'<div class="card-tags">'+tagsHtml+'</div>';

      var y0=0,x0=0;
      div.addEventListener('touchstart',function(e){y0=e.touches[0].clientY;x0=e.touches[0].clientX;},{passive:true});
      div.addEventListener('touchend',function(e){
        if(Math.abs(e.changedTouches[0].clientY-y0)<8&&Math.abs(e.changedTouches[0].clientX-x0)<8){
          e.preventDefault(); abrirDetalle(p);
        }
      });
      div.addEventListener('click',function(){ abrirDetalle(p); });
      el.appendChild(div);
    })(visibles[i]);
  }
}

// ── MARCADORES ────────────────────────────────────────────────────────────
function renderMarcadores(){
  var ids=Object.keys(marcadores);
  for(var i=0;i<ids.length;i++) mapa.removeLayer(marcadores[ids[i]]);
  marcadores={};
  var vs={};
  for(var j=0;j<visibles.length;j++) vs[visibles[j].id]=true;

  for(var k=0;k<todos.length;k++){
    (function(p){
      var esV=!!vs[p.id],esSel=selId===p.id,c=col(p.cat),sz=esSel?48:36;
      var bg=esSel?c:'#fff';
      var fc=esSel?'#fff':c;
      var icon=L.divIcon({className:'',
        html:'<div style="'
          +'width:'+sz+'px;height:'+sz+'px;'
          +'border-radius:50%;'
          +'background:'+bg+';'
          +'border:2px solid '+c+';'
          +'display:flex;align-items:center;justify-content:center;'
          +'font-size:'+(esSel?18:13)+'px;'
          +'opacity:'+(esV?1:0.18)+';'
          +'box-shadow:'+(esSel?'0 4px 16px rgba(0,0,0,.25),':'')+'0 2px 8px rgba(0,0,0,.12);'
          +'cursor:pointer;'
          +'transition:all .15s;'
          +'">'+emo(p.tipo)+'</div>',
        iconSize:[sz,sz],iconAnchor:[sz/2,sz/2]});
      var mk=L.marker([p.lat,p.lng],{icon:icon,zIndexOffset:esSel?500:0}).addTo(mapa);
      if(esV) mk.on('click',function(e){e.originalEvent.stopPropagation();abrirDetalle(p);});
      marcadores[p.id]=mk;
    })(todos[k]);
  }
}

// ── DETALLE COMPLETO ──────────────────────────────────────────────────────
function abrirDetalle(p){
  lugarDetalle=p;
  selId=p.id;

  document.querySelectorAll('.card').forEach(function(c){c.classList.remove('sel');});
  var card=document.getElementById('card'+p.id);
  if(card){card.classList.add('sel');card.scrollIntoView({behavior:'smooth',block:'nearest'});}
  if(mapa) mapa.flyTo([p.lat,p.lng],17,{animate:true,duration:.5});
  renderMarcadores();

  // Construir detalle completo
  var tagsHtml='';
  if(p.cat==='celiac'||p.cat==='ambos') tagsHtml+='<span class="tag tag-tacc" style="font-size:.76rem;padding:4px 12px;">🌾 Sin TACC</span>';
  if(p.cat==='vegan'||p.cat==='ambos')  tagsHtml+='<span class="tag tag-vegan" style="font-size:.76rem;padding:4px 12px;">🌿 Vegano</span>';
  if(p.cat==='vegetarian') tagsHtml+='<span class="tag tag-veg" style="font-size:.76rem;padding:4px 12px;">🥗 Vegetariano</span>';
  if(p.esDiet) tagsHtml+='<span class="tag tag-diet" style="font-size:.76rem;padding:4px 12px;">💚 Dietetica</span>';
  if(p.cat==='none'&&!p.esDiet) tagsHtml+='<span class="tag tag-nd" style="font-size:.76rem;padding:4px 12px;">Sin datos dieteticos en OSM</span>';

  var infoHtml='';
  for(var i=0;i<p.info.length;i++){
    infoHtml+='<div class="info-row">'
      +'<div class="info-ic">'+p.info[i].ic+'</div>'
      +'<div class="info-content">'
        +'<div class="info-label">'+p.info[i].lab+'</div>'
        +'<div class="info-value">'+p.info[i].val+'</div>'
      +'</div>'
    +'</div>';
  }

  // Nota de OSM
  var osmNote='<div class="osm-note">Los datos provienen de OpenStreetMap. Si el local tiene informacion desactualizada, podes colaborar en osm.org para mejorarla para toda la comunidad.</div>';

  document.getElementById('detBody').innerHTML=
    '<div class="det-hero">'
      +'<div class="det-tipo-row">'
        +'<div class="det-tipo-icon">'+emo(p.tipo)+'</div>'
        +'<div class="det-tipo-info">'
          +'<div class="det-tipo-label">'+p.tipo+'</div>'
        +'</div>'
        +'<div class="det-dist-badge">'+fmt(p.dist)+' de vos</div>'
      +'</div>'
      +'<div class="det-name">'+p.nombre+'</div>'
      +'<div class="det-tags">'+tagsHtml+'</div>'
    +'</div>'
    +'<div class="det-section">'
      +'<div class="det-sec-title">Informacion del lugar</div>'
      +infoHtml
    +'</div>'
    +'<div class="det-section">'
      +'<div class="det-sec-title">Sobre los datos</div>'
      +osmNote
    +'</div>';

  document.getElementById('btnNav').onclick=function(){
    window.open('https://www.google.com/maps/dir/?api=1&destination='+p.lat+','+p.lng,'_blank');
  };
  document.getElementById('btnOsm').onclick=function(){
    window.open('https://www.openstreetmap.org/'+p.osmTipo+'/'+p.osmId,'_blank');
  };

  document.getElementById('pantallaDetalle').classList.add('open');
}

function cerrarDetalle(){
  document.getElementById('pantallaDetalle').classList.remove('open');
  selId=null; renderMarcadores();
}

// ── SHEET TOGGLE ──────────────────────────────────────────────────────────
function toggleSheet(){
  if(window.innerWidth>=768) return;
  sheetBig=!sheetBig;
  var sheet=document.getElementById('sheet');
  sheet.classList.toggle('big',sheetBig);
  if(sheetBig){
    sheet.style.height='66vh';
    sheet.style.maxHeight='66vh';
  } else {
    sheet.style.height='36vh';
    sheet.style.maxHeight='36vh';
  }
  document.getElementById('expandBtn').textContent=sheetBig?'▼ Minimizar':'▲ Ver lista';
  document.getElementById('toast').style.bottom=sheetBig?'calc(66vh + 10px)':'calc(36vh + 10px)';
  setTimeout(function(){ if(mapa) mapa.invalidateSize(); },350);
}

// ── TOAST ─────────────────────────────────────────────────────────────────
function toast(msg,tipo){
  var t=document.getElementById('toast');
  t.textContent=msg;
  t.className='toast on'+(tipo?' '+tipo:'');
  setTimeout(function(){ t.className='toast'+(tipo?' '+tipo:''); },4000);
}
</script>

</body>
</html>
