<!DOCTYPE html>

<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#f5f0e8">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>Limpio y Rico</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,600;0,700;1,400&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
:root {
  --cream:   #f5f0e8;
  --cream2:  #ede7d9;
  --cream3:  #ddd5c2;
  --ink:     #1a1612;
  --ink2:    #3d3530;
  --ink3:    #6b6158;
  --ink4:    #9e9389;
  --gold:    #b8962e;
  --gold-bg: #fdf6e3;
  --green:   #2d6a4f;
  --green-bg:#edf7f2;
  --red:     #c0392b;
}

/* Reset absoluto */

- { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
  html, body { width:100%; height:100%; overflow:hidden; }
  body {
  font-family: ‘DM Sans’, -apple-system, sans-serif;
  background: var(–cream);
  color: var(–ink);
  }

/* ══ CAPAS FIJAS — cada pantalla ocupa todo el viewport ══ */

/* APP PRINCIPAL — siempre visible debajo */
#appShell {
position: fixed;
inset: 0;
display: flex;
flex-direction: column;
background: var(–cream);
}

/* PANTALLA PERMISO — encima del todo */
#pantallaPerm {
position: fixed;
inset: 0;
z-index: 200;
background: var(–cream);
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
padding: 28px 22px;
gap: 16px;
text-align: center;
overflow-y: auto;
}

/* PANTALLA LOADING */
#pantallaLoading {
position: fixed;
inset: 0;
z-index: 190;
background: var(–cream);
display: none;
flex-direction: column;
align-items: center;
justify-content: center;
gap: 14px;
}

/* PANTALLA DETALLE */
#pantallaDetalle {
position: fixed;
inset: 0;
z-index: 150;
background: var(–cream);
display: flex;
flex-direction: column;
transform: translateY(100%);
transition: transform .3s ease;
}
#pantallaDetalle.open { transform: translateY(0); }

/* ══ TOP BAR ══ */
.topbar {
flex-shrink: 0;
background: var(–cream);
border-bottom: 1px solid var(–cream3);
display: flex;
align-items: center;
gap: 10px;
padding: 10px 14px;
padding-top: max(10px, env(safe-area-inset-top));
}
.logo {
font-family: ‘Playfair Display’, serif;
font-size: 1.1rem;
font-weight: 700;
white-space: nowrap;
color: var(–ink);
flex-shrink: 0;
}
.logo em { color: var(–gold); font-style: italic; font-weight: 400; }

.searchbox {
flex: 1; min-width: 0;
display: flex; align-items: center; gap: 7px;
background: var(–cream2);
border: 1.5px solid var(–cream3);
border-radius: 50px;
padding: 8px 13px;
}
.searchbox input {
flex: 1; min-width: 0;
background: none; border: none; outline: none;
color: var(–ink); font-size: 0.84rem;
font-family: ‘DM Sans’, sans-serif;
}
.searchbox input::placeholder { color: var(–ink4); }

/* ══ FILTROS ══ */
.filters {
flex-shrink: 0;
display: flex; gap: 6px;
padding: 7px 12px;
background: var(–cream);
border-bottom: 1px solid var(–cream3);
overflow-x: auto;
-webkit-overflow-scrolling: touch;
scrollbar-width: none;
}
.filters::-webkit-scrollbar { display: none; }

.chip {
flex-shrink: 0;
padding: 5px 12px;
border-radius: 50px;
border: 1.5px solid var(–cream3);
background: var(–cream2);
color: var(–ink3);
font-size: 0.75rem;
font-weight: 500;
white-space: nowrap;
cursor: pointer;
touch-action: manipulation;
transition: all .15s;
-webkit-user-select: none;
}
.chip.on      { background: var(–ink);    border-color: var(–ink);  color: var(–cream); }
.chip.on-gold { background: var(–gold-bg); border-color: var(–gold); color: var(–ink2);  }

/* ══ MAPA ══ */
#map {
flex: 1;
min-height: 0;
width: 100%;
z-index: 1;
background: var(–cream2);
}

/* ══ SHEET ══ */
.sheet {
flex-shrink: 0;
background: var(–cream);
border-top: 2px solid var(–cream3);
display: flex;
flex-direction: column;
height: 36vh;
min-height: 150px;
transition: height .3s ease;
}
.sheet.big { height: 65vh; }

.sheet-top {
flex-shrink: 0;
display: flex;
align-items: center;
justify-content: space-between;
padding: 9px 14px 7px;
cursor: pointer;
}
.handle {
width: 30px; height: 3px;
background: var(–cream3);
border-radius: 3px;
margin: 0 auto 7px;
}
.sheet-label {
font-family: ‘Playfair Display’, serif;
font-size: 0.9rem;
font-weight: 600;
display: flex;
align-items: center;
gap: 7px;
}
.km-pill {
font-family: ‘DM Sans’, sans-serif;
font-size: 0.62rem;
font-weight: 600;
color: var(–gold);
background: var(–gold-bg);
border: 1px solid rgba(184,150,46,.3);
border-radius: 50px;
padding: 2px 7px;
}
.sheet-count { font-size: 0.71rem; color: var(–ink4); margin-top: 2px; }
.sheet-count b { color: var(–ink2); font-weight: 600; }

.expand-btn {
font-size: 0.7rem;
color: var(–ink3);
background: var(–cream2);
border: 1.5px solid var(–cream3);
border-radius: 50px;
padding: 4px 10px;
cursor: pointer;
touch-action: manipulation;
flex-shrink: 0;
white-space: nowrap;
}

/* ══ LISTA ══ */
.cards {
flex: 1;
overflow-y: scroll;
-webkit-overflow-scrolling: touch;
overscroll-behavior: contain;
touch-action: pan-y;
padding: 2px 10px 16px;
display: flex;
flex-direction: column;
gap: 8px;
padding-bottom: max(16px, env(safe-area-inset-bottom));
}

/* ══ CARD ══ */
.card {
flex-shrink: 0;
background: #fff;
border: 1.5px solid var(–cream3);
border-radius: 13px;
padding: 13px 14px;
cursor: pointer;
position: relative;
overflow: hidden;
box-shadow: 0 1px 8px rgba(26,22,18,.06);
}
.card::before {
content: ‘’;
position: absolute;
left: 0; top: 0; bottom: 0;
width: 3px;
background: var(–cream3);
}
.card.c-tacc::before  { background: var(–gold); }
.card.c-vegan::before { background: var(–green); }
.card.c-ambos::before { background: linear-gradient(180deg, var(–gold), var(–green)); }
.card.c-diet::before  { background: var(–green); }
.card.sel { border-color: var(–ink2); box-shadow: 0 4px 20px rgba(26,22,18,.12); }

.card-header { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:4px; }
.card-name {
font-family: ‘Playfair Display’, serif;
font-size: 0.93rem; font-weight: 600;
color: var(–ink); line-height: 1.2;
flex: 1; min-width: 0;
}
.card-dist {
font-size: 0.69rem; font-weight: 600;
color: var(–ink4);
background: var(–cream2);
padding: 2px 8px; border-radius: 50px;
margin-left: 8px; white-space: nowrap; flex-shrink: 0;
}
.card-type { font-size: 0.72rem; color: var(–ink4); margin-bottom: 7px; }
.tags { display: flex; flex-wrap: wrap; gap: 4px; }
.tag { font-size: 0.62rem; font-weight: 600; padding: 2px 8px; border-radius: 50px; }
.tag-tacc  { background: var(–gold-bg);  color: var(–ink2); border: 1px solid rgba(184,150,46,.3); }
.tag-vegan { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.25); }
.tag-veg   { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.2); }
.tag-diet  { background: var(–green-bg); color: var(–green); border: 1px solid rgba(45,106,79,.25); }
.tag-nd    { background: var(–cream2);   color: var(–ink4);  border: 1px solid var(–cream3); }

/* ══ DETALLE ══ */
.det-nav {
flex-shrink: 0;
display: flex; align-items: center; justify-content: space-between;
padding: 11px 14px;
padding-top: max(11px, env(safe-area-inset-top));
border-bottom: 1px solid var(–cream3);
background: var(–cream);
}
.back-btn {
background: var(–cream2); border: 1.5px solid var(–cream3);
color: var(–ink2); border-radius: 50px;
padding: 7px 14px; font-size: 0.82rem; font-weight: 500;
font-family: ‘DM Sans’, sans-serif;
cursor: pointer; touch-action: manipulation;
}

.det-body {
flex: 1;
overflow-y: auto;
-webkit-overflow-scrolling: touch;
}

.det-hero {
padding: 20px 16px 14px;
border-bottom: 1px solid var(–cream3);
background: #fff;
}
.det-tipo-row {
display: flex; align-items: center; gap: 10px;
margin-bottom: 10px;
}
.det-tipo-icon {
width: 40px; height: 40px;
background: var(–cream); border: 1.5px solid var(–cream3);
border-radius: 10px;
display: flex; align-items: center; justify-content: center;
font-size: 1.2rem; flex-shrink: 0;
}
.det-tipo-label { font-size: 0.71rem; color: var(–ink4); font-weight: 500; }
.det-dist-pill {
margin-left: auto;
font-size: 0.71rem; font-weight: 700;
background: var(–ink); color: var(–cream);
padding: 4px 10px; border-radius: 50px;
}
.det-name {
font-family: ‘Playfair Display’, serif;
font-size: 1.4rem; font-weight: 700;
color: var(–ink); line-height: 1.2;
margin-bottom: 10px;
}
.det-tags { display: flex; gap: 6px; flex-wrap: wrap; }

.det-section {
padding: 14px 16px;
border-bottom: 1px solid var(–cream3);
}
.det-sec-title {
font-size: 0.63rem; font-weight: 700;
text-transform: uppercase; letter-spacing: 1.2px;
color: var(–ink4); margin-bottom: 12px;
}
.info-row {
display: flex; align-items: flex-start; gap: 10px;
margin-bottom: 10px;
}
.info-row:last-child { margin-bottom: 0; }
.info-ic {
width: 30px; height: 30px; flex-shrink: 0;
background: var(–cream2); border: 1px solid var(–cream3);
border-radius: 8px;
display: flex; align-items: center; justify-content: center;
font-size: 0.85rem;
}
.info-label { font-size: 0.66rem; color: var(–ink4); font-weight: 500; margin-bottom: 1px; }
.info-value { font-size: 0.83rem; color: var(–ink2); line-height: 1.4; word-break: break-word; }

.osm-note {
font-size: 0.74rem; color: var(–ink4);
background: var(–cream2);
border-radius: 8px; padding: 9px 11px;
border-left: 2px solid var(–gold);
line-height: 1.5;
}

.det-foot {
flex-shrink: 0;
display: flex; gap: 8px;
padding: 11px 14px;
padding-bottom: max(11px, env(safe-area-inset-bottom));
border-top: 1.5px solid var(–cream3);
background: var(–cream);
}
.btn-p {
flex: 2; padding: 13px;
background: var(–ink); color: var(–cream);
border: none; border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-weight: 600; font-size: 0.87rem;
cursor: pointer; touch-action: manipulation;
}
.btn-s {
flex: 1; padding: 13px;
background: var(–cream2); color: var(–ink2);
border: 1.5px solid var(–cream3); border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-weight: 500; font-size: 0.84rem;
cursor: pointer; touch-action: manipulation;
}

/* ══ PERMISO ══ */
.perm-logo-mark {
width: 60px; height: 60px;
background: var(–ink); border-radius: 16px;
display: flex; align-items: center; justify-content: center;
font-size: 1.7rem;
box-shadow: 0 6px 20px rgba(26,22,18,.2);
margin-bottom: 4px;
}
.perm-app-name {
font-family: ‘Playfair Display’, serif;
font-size: 1.8rem; font-weight: 700; color: var(–ink);
}
.perm-app-name em { color: var(–gold); font-style: italic; font-weight: 400; }
.perm-tagline { font-size: 0.87rem; color: var(–ink3); max-width: 260px; line-height: 1.6; }

.perm-feats { display:flex; flex-direction:column; gap:8px; width:100%; max-width:310px; }
.perm-feat {
display: flex; align-items: center; gap: 12px;
background: #fff; border: 1.5px solid var(–cream3);
border-radius: 12px; padding: 11px 13px;
text-align: left;
box-shadow: 0 1px 8px rgba(26,22,18,.05);
}
.feat-ic {
width: 36px; height: 36px; flex-shrink: 0;
background: var(–cream2); border-radius: 9px;
display: flex; align-items: center; justify-content: center;
font-size: 1rem;
}
.feat-text b { display:block; font-size:0.81rem; color:var(–ink); font-weight:600; margin-bottom:1px; }
.feat-text span { font-size:0.72rem; color:var(–ink4); }

.perm-err {
font-size: 0.78rem; color: var(–red);
background: #fdf0ee;
border: 1px solid rgba(192,57,43,.2);
border-radius: 10px; padding: 10px 13px;
max-width: 310px; display: none;
line-height: 1.6; white-space: pre-line; text-align: left;
}

#btnUbicacion {
width: 100%; max-width: 310px; padding: 15px;
background: var(–ink); color: var(–cream);
border: none; border-radius: 50px;
font-family: ‘DM Sans’, sans-serif;
font-size: 0.95rem; font-weight: 600;
cursor: pointer; touch-action: manipulation;
-webkit-tap-highlight-color: transparent;
box-shadow: 0 4px 16px rgba(26,22,18,.22);
display: flex; align-items: center; justify-content: center; gap: 8px;
}
.perm-note { font-size: 0.68rem; color: var(–ink4); max-width: 250px; }

/* ══ LOADING ══ */
.load-logo {
font-family: ‘Playfair Display’, serif;
font-size: 1.35rem; font-weight: 700; color: var(–ink);
}
.load-logo em { color: var(–gold); font-style: italic; font-weight: 400; }
.spinner {
width: 38px; height: 38px;
border: 2.5px solid var(–cream3);
border-top-color: var(–ink);
border-radius: 50%;
animation: spin .7s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
.ltxt { font-size: 0.82rem; color: var(–ink3); }
.lsub { font-size: 0.71rem; color: var(–ink4); margin-top: -6px; }

/* ══ TOAST ══ */
.toast {
position: fixed;
bottom: calc(36vh + 10px);
left: 50%; transform: translateX(-50%) translateY(6px);
background: var(–ink); color: var(–cream);
border-radius: 50px; padding: 7px 15px;
font-size: 0.77rem; font-weight: 500;
opacity: 0; pointer-events: none;
transition: all .22s; z-index: 300;
white-space: nowrap; max-width: 90vw;
box-shadow: 0 4px 16px rgba(26,22,18,.18);
}
.toast.on  { opacity:1; transform:translateX(-50%) translateY(0); }
.toast.ok  { background: var(–green); }
.toast.err { background: var(–red); }

/* ══ EMPTY STATE ══ */
.empty {
display: flex; flex-direction: column;
align-items: center; padding: 24px 16px;
text-align: center; gap: 6px;
}
.empty-ic { font-size: 1.8rem; margin-bottom: 4px; }
.empty-t  { font-family: ‘Playfair Display’, serif; font-size: 0.95rem; color: var(–ink2); }
.empty-s  { font-size: 0.77rem; color: var(–ink4); line-height: 1.5; max-width: 220px; }

/* ══ LEAFLET ══ */
.leaflet-tile { filter: sepia(.12) saturate(.85) brightness(.98); }
.leaflet-control-zoom { border: none !important; box-shadow: 0 2px 12px rgba(26,22,18,.1) !important; }
.leaflet-control-zoom a { background: #fff !important; color: var(–ink) !important; border: 1.5px solid var(–cream3) !important; }
.leaflet-popup-content-wrapper { background: #fff !important; color: var(–ink) !important; border: 1.5px solid var(–cream3) !important; border-radius: 12px !important; box-shadow: 0 4px 20px rgba(26,22,18,.12) !important; font-family: ‘DM Sans’, sans-serif !important; }
.leaflet-popup-tip { background: #fff !important; }

/* ══ TABLET / DESKTOP ══ */
@media (min-width: 720px) {
#appShell { flex-direction: column; }
.main-row { display: flex; flex: 1; min-height: 0; overflow: hidden; }
#map { flex: 1; height: 100%; }
.sheet {
width: 360px; height: 100% !important;
max-height: 100% !important;
border-top: none; border-left: 2px solid var(–cream3);
}
.handle, .expand-btn { display: none; }
.toast { bottom: 20px; }
#pantallaDetalle { max-width: 400px; right: auto; border-right: 1px solid var(–cream3); }
}
</style>

</head>
<body>

<!-- ══ PANTALLA PERMISO ══ -->

<div id="pantallaPerm">
  <div class="perm-logo-mark">🌾</div>
  <div class="perm-app-name">Limpio y <em>Rico</em></div>
  <div class="perm-tagline">Encontra restaurantes sin TACC y veganos exactamente donde estas.</div>
  <div class="perm-feats">
    <div class="perm-feat">
      <div class="feat-ic">🌾</div>
      <div class="feat-text"><b>Sin TACC</b><span>Locales con opciones libres de gluten</span></div>
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

<!-- ══ PANTALLA LOADING ══ -->

<div id="pantallaLoading">
  <div class="load-logo">Limpio y <em>Rico</em></div>
  <div class="spinner"></div>
  <div class="ltxt" id="textoLoad">Obteniendo ubicacion...</div>
  <div class="lsub" id="subLoad"></div>
</div>

<!-- ══ APP SHELL ══ -->

<div id="appShell">
  <div class="topbar">
    <div class="logo">Limpio y <em>Rico</em></div>
    <div class="searchbox">
      <span style="color:var(--ink4);flex-shrink:0">🔍</span>
      <input type="text" id="inputBuscar" placeholder="Buscar restaurante...">
    </div>
  </div>

  <div class="filters" id="filtros">
    <div class="chip on"  data-f="todos">Todos</div>
    <div class="chip"     data-f="tacc">🌾 Sin TACC</div>
    <div class="chip"     data-f="vegan">🌿 Vegano</div>
    <div class="chip"     data-f="ambos">✦ TACC+Vegano</div>
    <div class="chip"     data-f="cafe">☕ Cafeterias</div>
    <div class="chip"     data-f="dietetica">💚 Dieteticas</div>
  </div>

  <!-- En mobile: columna. En desktop: fila (via JS) -->

  <div id="mainRow" style="flex:1;min-height:0;display:flex;flex-direction:column;overflow:hidden;">
    <div id="map"></div>
    <div class="sheet" id="sheet">
      <div class="sheet-top" id="sheetTop">
        <div style="flex:1;min-width:0;">
          <div class="handle"></div>
          <div class="sheet-label">Lugares cercanos <span class="km-pill">10 km</span></div>
          <div class="sheet-count"><b id="contador">0</b> resultados · del mas cercano al mas lejano</div>
        </div>
        <div class="expand-btn" id="expandBtn">▲ Ver lista</div>
      </div>
      <div class="cards" id="listaCards">
        <div class="empty">
          <div class="empty-ic">📍</div>
          <div class="empty-t">Activa tu ubicacion</div>
          <div class="empty-s">Acepta el permiso para encontrar lugares cerca tuyo</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ══ PANTALLA DETALLE ══ -->

<div id="pantallaDetalle">
  <div class="det-nav">
    <button class="back-btn" id="btnVolver">← Volver</button>
    <span style="font-size:.77rem;color:var(--ink4)">Detalle del lugar</span>
  </div>
  <div class="det-body" id="detBody"></div>
  <div class="det-foot">
    <button class="btn-p" id="btnNav">🧭 Como llegar</button>
    <button class="btn-s" id="btnOsm">Ver en OSM</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
var mapa, uLat, uLng;
var todos=[], visibles=[], marcadores={};
var selId=null, filtro='todos', sheetBig=false;

var SERVERS=[
  'https://overpass-api.de/api/interpreter',
  'https://overpass.kumi.systems/api/interpreter',
  'https://maps.mail.ru/osm/tools/overpass/api/interpreter'
];

// ── RESPONSIVE ────────────────────────────────────────────────────────────
function ajustarLayout(){
  var row=document.getElementById('mainRow');
  var sheet=document.getElementById('sheet');
  if(window.innerWidth>=720){
    row.style.flexDirection='row';
    sheet.style.cssText='width:360px;height:100%;max-height:100%;border-top:none;border-left:2px solid var(--cream3);flex-shrink:0;display:flex;flex-direction:column;';
  } else {
    row.style.flexDirection='column';
    sheet.style.cssText='width:100%;height:'+(sheetBig?'65':'36')+'vh;min-height:150px;border-left:none;border-top:2px solid var(--cream3);flex-shrink:0;display:flex;flex-direction:column;transition:height .3s ease;';
  }
  if(mapa) setTimeout(function(){ mapa.invalidateSize(); },350);
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
  var ls=document.getElementById('pantallaLoading'); ls.style.display='flex';
  setL('Obteniendo tu ubicacion...');

  navigator.geolocation.getCurrentPosition(
    function(pos){
      uLat=pos.coords.latitude; uLng=pos.coords.longitude;
      setL('Iniciando mapa...'); iniciarMapa();
      setL('Buscando lugares en 10 km...'); buscar(0);
    },
    function(err){
      document.getElementById('pantallaLoading').style.display='none';
      var m='No se pudo obtener la ubicacion.';
      if(err.code===1) m='Permiso denegado.\n\nVe a Ajustes iPhone > Privacidad > Ubicacion > Chrome > selecciona "Al usar la app".\nLuego recarga la pagina.';
      if(err.code===2) m='GPS no disponible. Conectate a WiFi o activa la ubicacion.';
      if(err.code===3) m='Tiempo agotado. Intenta de nuevo.';
      mostrarError(m);
    },
    {enableHighAccuracy:true, timeout:15000, maximumAge:0}
  );
}

function mostrarError(m){
  var e=document.getElementById('errorPerm'); e.textContent=m; e.style.display='block';
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
  L.circle([uLat,uLng],{radius:10000,color:'#b8962e',weight:1.5,fillColor:'#b8962e',fillOpacity:.03,dashArray:'5,8'}).addTo(mapa);
  var iYo=L.divIcon({className:'',html:'<div style="width:20px;height:20px;background:#1a1612;border:3px solid #fff;border-radius:50%;box-shadow:0 0 0 6px rgba(26,22,18,.15)"></div>',iconSize:[20,20],iconAnchor:[10,10]});
  L.marker([uLat,uLng],{icon:iYo,zIndexOffset:9999}).addTo(mapa).bindPopup('<b>Tu ubicacion</b>');
  mapa.on('click',function(){ selId=null; renderMarcadores(); });
  ajustarLayout();
}

// ── OVERPASS ──────────────────────────────────────────────────────────────
function buscar(idx){
  if(idx>=SERVERS.length){
    document.getElementById('pantallaLoading').style.display='none';
    document.getElementById('listaCards').innerHTML='<div class="empty"><div class="empty-ic">📡</div><div class="empty-t">Sin conexion</div><div class="empty-s">Verifica tu internet y recarga.</div><br><button onclick="location.reload()" style="background:var(--ink);color:var(--cream);border:none;padding:9px 18px;border-radius:50px;font-weight:600;font-size:.82rem;cursor:pointer;font-family:\'DM Sans\',sans-serif">Reintentar</button></div>';
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

  var ctrl=new AbortController(), tid=setTimeout(function(){ ctrl.abort(); },22000);
  fetch(SERVERS[idx],{method:'POST',body:'data='+encodeURIComponent(q),signal:ctrl.signal})
  .then(function(r){ clearTimeout(tid); if(!r.ok) throw 0; return r.json(); })
  .then(function(d){ if(!d.elements) throw 0; procesarLugares(d.elements); })
  .catch(function(){ clearTimeout(tid); setTimeout(function(){ buscar(idx+1); },600); });
}

// ── PROCESAR ──────────────────────────────────────────────────────────────
function procesarLugares(els){
  var v={};
  todos=[];
  for(var i=0;i<els.length;i++){
    var e=els[i];
    if(!e.tags||!e.tags.name||v[e.id]) continue;
    v[e.id]=true;
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
    var info=[];
    if(t['diet:gluten_free']==='only')    info.push({ic:'🌾',lab:'TACC',val:'100% Sin TACC (certificado en OSM)'});
    else if(gf&&!esDiet)                  info.push({ic:'🌾',lab:'TACC',val:'Opciones sin TACC disponibles'});
    if(t['diet:vegan']==='only')          info.push({ic:'🌿',lab:'Dieta',val:'100% Vegano'});
    else if(ve)                           info.push({ic:'🌿',lab:'Dieta',val:'Opciones veganas'});
    if(vg)                                info.push({ic:'🥗',lab:'Dieta',val:'Opciones vegetarianas'});
    if(esDiet)                            info.push({ic:'💚',lab:'Tipo',val:'Dietetica / tienda naturista'});
    if(t.cuisine)      info.push({ic:'🍴',lab:'Cocina',val:t.cuisine.replace(/_/g,' ')});
    if(t.opening_hours) info.push({ic:'🕐',lab:'Horario',val:t.opening_hours});
    var dir='';
    if(t['addr:street']) dir+=t['addr:street'];
    if(t['addr:housenumber']) dir+=' '+t['addr:housenumber'];
    if(t['addr:city']) dir+=', '+t['addr:city'];
    if(t['addr:postcode']) dir+=' ('+t['addr:postcode']+')';
    if(dir) info.push({ic:'📌',lab:'Direccion',val:dir});
    if(t.phone||t['contact:phone']) info.push({ic:'📞',lab:'Telefono',val:t.phone||t['contact:phone']});
    if(t['contact:whatsapp']) info.push({ic:'💬',lab:'WhatsApp',val:t['contact:whatsapp']});
    if(t.website||t['contact:website']) info.push({ic:'🌐',lab:'Sitio web',val:(t.website||t['contact:website']).replace(/^https?:\/\//,'').slice(0,45)});
    var redes='';
    if(t['contact:instagram']) redes+='Instagram: @'+t['contact:instagram'].replace(/^@/,'');
    if(t['contact:facebook'])  redes+=(redes?' · ':'')+'Facebook: '+t['contact:facebook'].replace(/^https?:\/\/(www\.)?facebook\.com\//,'');
    if(redes) info.push({ic:'📷',lab:'Redes sociales',val:redes});
    if(t.email||t['contact:email']) info.push({ic:'✉️',lab:'Email',val:t.email||t['contact:email']});
    if(t.description) info.push({ic:'📝',lab:'Descripcion',val:t.description});
    if(t.note) info.push({ic:'💡',lab:'Nota',val:t.note});
    if(!info.length) info.push({ic:'ℹ️',lab:'Info',val:'Sin informacion adicional en OpenStreetMap.'});

    todos.push({id:e.id,nombre:t.name,tipo:tipo,cat:cat,lat:lat,lng:lng,dist:dist,info:info,osmId:e.id,osmTipo:e.type||'node',esDiet:esDiet});
  }
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
  renderLista(); if(mapa) renderMarcadores();
}

// ── HELPERS ───────────────────────────────────────────────────────────────
function emo(t){ return t==='Cafeteria'?'☕':t==='Panaderia'?'🥖':t==='Fast Food'?'🍔':t==='Dietetica Natural'?'🌿':'🍽️'; }
function col(c){ return c==='celiac'?'#b8962e':c==='vegan'?'#2d6a4f':c==='ambos'?'#7a5c1e':c==='vegetarian'?'#3d7a5f':'#9e9389'; }
function catCls(p){ return p.cat==='celiac'?' c-tacc':p.cat==='vegan'?' c-vegan':p.cat==='ambos'?' c-ambos':p.esDiet?' c-diet':''; }

// ── LISTA ─────────────────────────────────────────────────────────────────
function renderLista(){
  var el=document.getElementById('listaCards');
  if(!visibles.length){
    el.innerHTML='<div class="empty"><div class="empty-ic">'+(filtro!=='todos'?'🔍':'🌾')+'</div><div class="empty-t">Sin resultados</div><div class="empty-s">'+(filtro!=='todos'?'Proba con el filtro "Todos".':'No encontramos lugares en 10 km.')+'</div></div>';
    return;
  }
  el.innerHTML='';
  var max=Math.min(visibles.length,80);
  for(var i=0;i<max;i++){
    (function(p){
      var div=document.createElement('div');
      div.className='card'+catCls(p)+(selId===p.id?' sel':'');
      div.id='card'+p.id;
      var th='';
      if(p.cat==='celiac'||p.cat==='ambos') th+='<span class="tag tag-tacc">Sin TACC</span>';
      if(p.cat==='vegan'||p.cat==='ambos')  th+='<span class="tag tag-vegan">Vegano</span>';
      if(p.cat==='vegetarian') th+='<span class="tag tag-veg">Vegetariano</span>';
      if(p.esDiet) th+='<span class="tag tag-diet">Dietetica</span>';
      if(p.cat==='none'&&!p.esDiet) th+='<span class="tag tag-nd">Sin datos</span>';
      div.innerHTML='<div class="card-header"><div class="card-name">'+p.nombre+'</div><div class="card-dist">'+fmt(p.dist)+'</div></div>'
        +'<div class="card-type">'+emo(p.tipo)+' '+p.tipo+'</div>'
        +'<div class="tags">'+th+'</div>';
      var y0=0,x0=0;
      div.addEventListener('touchstart',function(e){y0=e.touches[0].clientY;x0=e.touches[0].clientX;},{passive:true});
      div.addEventListener('touchend',function(e){ if(Math.abs(e.changedTouches[0].clientY-y0)<8&&Math.abs(e.changedTouches[0].clientX-x0)<8){e.preventDefault();abrirDetalle(p);} });
      div.addEventListener('click',function(){abrirDetalle(p);});
      el.appendChild(div);
    })(visibles[i]);
  }
}

// ── MARCADORES ────────────────────────────────────────────────────────────
function renderMarcadores(){
  Object.values(marcadores).forEach(function(m){ mapa.removeLayer(m); });
  marcadores={};
  var vs={};
  visibles.forEach(function(p){ vs[p.id]=true; });
  todos.forEach(function(p){
    var esV=!!vs[p.id],esSel=selId===p.id,c=col(p.cat),sz=esSel?46:34;
    var icon=L.divIcon({className:'',
      html:'<div style="width:'+sz+'px;height:'+sz+'px;border-radius:50%;background:'+(esSel?c:'#fff')+';border:2px solid '+c+';display:flex;align-items:center;justify-content:center;font-size:'+(esSel?17:13)+'px;opacity:'+(esV?1:0.15)+';box-shadow:'+(esSel?'0 4px 14px rgba(0,0,0,.2),':'')+'0 2px 8px rgba(0,0,0,.1);cursor:pointer;">'+emo(p.tipo)+'</div>',
      iconSize:[sz,sz],iconAnchor:[sz/2,sz/2]});
    var mk=L.marker([p.lat,p.lng],{icon:icon,zIndexOffset:esSel?500:0}).addTo(mapa);
    if(esV) mk.on('click',function(e){e.originalEvent.stopPropagation();abrirDetalle(p);});
    marcadores[p.id]=mk;
  });
}

// ── DETALLE ───────────────────────────────────────────────────────────────
function abrirDetalle(p){
  selId=p.id;
  document.querySelectorAll('.card').forEach(function(c){c.classList.remove('sel');});
  var card=document.getElementById('card'+p.id);
  if(card){card.classList.add('sel');card.scrollIntoView({behavior:'smooth',block:'nearest'});}
  if(mapa) mapa.flyTo([p.lat,p.lng],17,{animate:true,duration:.5});
  renderMarcadores();

  var th='';
  if(p.cat==='celiac'||p.cat==='ambos') th+='<span class="tag tag-tacc" style="font-size:.75rem;padding:4px 11px;">🌾 Sin TACC</span>';
  if(p.cat==='vegan'||p.cat==='ambos')  th+='<span class="tag tag-vegan" style="font-size:.75rem;padding:4px 11px;">🌿 Vegano</span>';
  if(p.cat==='vegetarian') th+='<span class="tag tag-veg" style="font-size:.75rem;padding:4px 11px;">🥗 Vegetariano</span>';
  if(p.esDiet) th+='<span class="tag tag-diet" style="font-size:.75rem;padding:4px 11px;">💚 Dietetica</span>';
  if(p.cat==='none'&&!p.esDiet) th+='<span class="tag tag-nd" style="font-size:.75rem;padding:4px 11px;">Sin datos dieteticos</span>';

  var ih='';
  p.info.forEach(function(row){
    ih+='<div class="info-row"><div class="info-ic">'+row.ic+'</div><div><div class="info-label">'+row.lab+'</div><div class="info-value">'+row.val+'</div></div></div>';
  });

  document.getElementById('detBody').innerHTML=
    '<div class="det-hero">'
      +'<div class="det-tipo-row">'
        +'<div class="det-tipo-icon">'+emo(p.tipo)+'</div>'
        +'<div style="flex:1"><div class="det-tipo-label">'+p.tipo+'</div></div>'
        +'<div class="det-dist-pill">'+fmt(p.dist)+' de vos</div>'
      +'</div>'
      +'<div class="det-name">'+p.nombre+'</div>'
      +'<div class="det-tags">'+th+'</div>'
    +'</div>'
    +'<div class="det-section"><div class="det-sec-title">Informacion del lugar</div>'+ih+'</div>'
    +'<div class="det-section"><div class="osm-note">Los datos provienen de OpenStreetMap. Si hay algo desactualizado podes colaborar en osm.org.</div></div>';

  document.getElementById('btnNav').onclick=function(){ window.open('https://www.google.com/maps/dir/?api=1&destination='+p.lat+','+p.lng,'_blank'); };
  document.getElementById('btnOsm').onclick=function(){ window.open('https://www.openstreetmap.org/'+p.osmTipo+'/'+p.osmId,'_blank'); };
  document.getElementById('pantallaDetalle').classList.add('open');
}

function cerrarDetalle(){
  document.getElementById('pantallaDetalle').classList.remove('open');
  selId=null; renderMarcadores();
}

// ── SHEET ─────────────────────────────────────────────────────────────────
function toggleSheet(){
  if(window.innerWidth>=720) return;
  sheetBig=!sheetBig;
  var s=document.getElementById('sheet');
  s.style.height=(sheetBig?'65':'36')+'vh';
  document.getElementById('expandBtn').textContent=sheetBig?'▼ Minimizar':'▲ Ver lista';
  document.getElementById('toast').style.bottom='calc('+(sheetBig?'65':'36')+'vh + 10px)';
  setTimeout(function(){ if(mapa) mapa.invalidateSize(); },350);
}

// ── TOAST ─────────────────────────────────────────────────────────────────
function toast(msg,tipo){
  var t=document.getElementById('toast');
  t.textContent=msg; t.className='toast on'+(tipo?' '+tipo:'');
  setTimeout(function(){ t.className='toast'+(tipo?' '+tipo:''); },4000);
}
</script>

</body>
</html>
