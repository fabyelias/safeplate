<!DOCTYPE html>

<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#0d1117">
<title>SafePlate</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
* { margin:0; padding:0; box-sizing:border-box; }
body {
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  background: #0d1117;
  color: #e6edf3;
  height: 100dvh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.topbar {
  background: #161b22;
  border-bottom: 1px solid #30363d;
  padding: 12px 16px;
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
}
.logo { font-weight: 800; font-size: 1.2rem; white-space: nowrap; }
.logo span { color: #39d353; }
.searchbox {
  flex: 1;
  display: flex;
  align-items: center;
  background: #1c2330;
  border: 1px solid #30363d;
  border-radius: 50px;
  padding: 8px 14px;
  gap: 6px;
}
.searchbox input {
  background: none; border: none; outline: none;
  color: #e6edf3; font-size: 0.87rem; width: 100%;
}
.searchbox input::placeholder { color: #8b949e; }
.filters {
  background: #161b22;
  border-bottom: 1px solid #30363d;
  padding: 8px 12px;
  display: flex;
  gap: 6px;
  overflow-x: auto;
  flex-shrink: 0;
  scrollbar-width: none;
}
.filters::-webkit-scrollbar { display: none; }
.chip {
  padding: 5px 13px;
  border-radius: 50px;
  border: 1.5px solid #30363d;
  background: #1c2330;
  color: #8b949e;
  font-size: 0.78rem;
  font-weight: 500;
  cursor: pointer;
  white-space: nowrap;
  flex-shrink: 0;
  transition: all 0.15s;
  -webkit-user-select: none;
  user-select: none;
}
.chip.ag { background:#1a4a2e; border-color:#39d353; color:#39d353; }
.chip.at { background:#003d32; border-color:#00c9a7; color:#00c9a7; }
.chip.aa { background:#3d2c00; border-color:#f0a500; color:#f0a500; }
#map { flex: 1; min-height: 0; z-index: 1; }
.sheet {
  background: #161b22;
  border-top: 1px solid #30363d;
  flex-shrink: 0;
  height: 35vh;
  display: flex;
  flex-direction: column;
  transition: height 0.3s ease;
  z-index: 100;
}
.sheet.big { height: 65vh; }
.sheet-top {
  padding: 10px 16px 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
  flex-shrink: 0;
}
.handle { width:36px; height:4px; background:#30363d; border-radius:4px; margin:0 auto 8px; }
.sheet-label { font-size:0.85rem; font-weight:600; }
.sheet-sub { font-size:0.72rem; color:#8b949e; margin-top:1px; }
.sheet-sub b { color:#39d353; }
.toggle-btn {
  font-size:0.72rem; color:#8b949e;
  background:#1c2330; border:1px solid #30363d;
  border-radius:50px; padding:4px 10px; cursor:pointer;
}
.cards {
  flex: 1; overflow-y: auto;
  padding: 0 10px 12px;
  display: flex; flex-direction: column; gap: 8px;
  -webkit-overflow-scrolling: touch;
}
.card {
  background: #1c2330;
  border: 1px solid #30363d;
  border-left: 3px solid #39d353;
  border-radius: 12px;
  padding: 12px 14px;
  cursor: pointer;
  flex-shrink: 0;
}
.card.cv { border-left-color: #00c9a7; }
.card.ca { border-left-color: #f0a500; }
.card.cn { border-left-color: #8b949e; }
.card.sel { background: rgba(57,211,83,0.08); }
.card-top { display:flex; justify-content:space-between; margin-bottom:3px; }
.card-name { font-weight:700; font-size:0.88rem; }
.card-dist { font-size:0.72rem; color:#8b949e; }
.card-type { font-size:0.75rem; color:#8b949e; margin-bottom:6px; }
.tags { display:flex; flex-wrap:wrap; gap:4px; }
.tag { font-size:0.65rem; font-weight:600; padding:2px 7px; border-radius:50px; }
.tg { background:#1a4a2e; color:#39d353; border:1px solid rgba(57,211,83,0.3); }
.tv { background:#003d32; color:#00c9a7; border:1px solid rgba(0,201,167,0.3); }
.tvg{ background:#1e2d3d; color:#7ab8f5; border:1px solid rgba(122,184,245,0.3); }
.tn { background:#2a1e1e; color:#f08080; border:1px solid rgba(240,128,128,0.3); }

/* DETAIL PANEL */
.detail {
position:fixed; inset:0;
background:#0d1117; z-index:9000;
display:flex; flex-direction:column;
transform:translateY(100%);
transition:transform 0.3s ease;
}
.detail.open { transform:translateY(0); }
.det-head {
padding:16px; border-bottom:1px solid #30363d;
display:flex; align-items:center; justify-content:space-between;
flex-shrink:0;
}
.back-btn {
background:#1c2330; border:1px solid #30363d;
color:#8b949e; border-radius:50px;
padding:7px 14px; font-size:0.83rem; cursor:pointer;
}
.det-body { flex:1; overflow-y:auto; padding:18px; -webkit-overflow-scrolling:touch; }
.det-name { font-size:1.35rem; font-weight:800; margin-bottom:4px; }
.det-sub  { font-size:0.83rem; color:#8b949e; margin-bottom:12px; }
.det-tags { display:flex; gap:6px; flex-wrap:wrap; margin-bottom:14px; }
.det-sec {
font-size:0.72rem; font-weight:700;
text-transform:uppercase; letter-spacing:1px;
color:#8b949e; margin-bottom:8px; margin-top:14px;
}
.det-line { font-size:0.85rem; color:#8b949e; margin-bottom:6px; line-height:1.5; }
.det-foot {
padding:14px 16px; border-top:1px solid #30363d;
display:flex; gap:10px; flex-shrink:0;
}
.btn-p {
flex:1; padding:13px; background:#39d353; color:#000;
border:none; border-radius:50px; font-weight:700; font-size:0.88rem; cursor:pointer;
}
.btn-s {
flex:1; padding:13px; background:#1c2330; color:#e6edf3;
border:1.5px solid #30363d; border-radius:50px; font-weight:700; font-size:0.88rem; cursor:pointer;
}

/* PERMISSION SCREEN */
.perm {
position:fixed; inset:0; background:#0d1117; z-index:9999;
display:flex; flex-direction:column;
align-items:center; justify-content:center;
padding:32px 24px; text-align:center; gap:16px;
}
.perm-icon { font-size:3.5rem; line-height:1; }
.perm-title { font-size:1.8rem; font-weight:800; }
.perm-title span { color:#39d353; }
.perm-desc { font-size:0.9rem; color:#8b949e; max-width:300px; line-height:1.6; }
.perm-error {
font-size:0.8rem; color:#f08080;
background:rgba(240,80,80,0.1);
border:1px solid rgba(240,80,80,0.3);
border-radius:10px; padding:10px 14px;
max-width:300px; display:none;
}
#btnUbicacion {
width:100%; max-width:300px; padding:15px;
background:#39d353; color:#000;
border:none; border-radius:50px;
font-size:1rem; font-weight:700;
cursor:pointer; touch-action:manipulation;
-webkit-tap-highlight-color: transparent;
}
.perm-note { font-size:0.72rem; color:#8b949e; max-width:260px; }

/* LOADING */
.loading {
position:fixed; inset:0; background:#0d1117; z-index:8000;
display:none; flex-direction:column;
align-items:center; justify-content:center; gap:14px;
}
.spinner {
width:44px; height:44px;
border:3px solid #30363d; border-top-color:#39d353;
border-radius:50%; animation:spin 0.8s linear infinite;
}
@keyframes spin { to { transform:rotate(360deg); } }
.load-txt { font-size:0.85rem; color:#8b949e; }

/* TOAST */
.toast {
position:fixed; bottom:calc(35vh + 14px); left:50%;
transform:translateX(-50%) translateY(8px);
background:#161b22; border:1px solid #39d353;
border-radius:50px; padding:7px 16px;
font-size:0.8rem; color:#39d353;
opacity:0; pointer-events:none;
transition:all 0.25s; z-index:5000; white-space:nowrap;
}
.toast.on { opacity:1; transform:translateX(-50%) translateY(0); }

.leaflet-tile { filter:brightness(0.7) saturate(0.6); }
.leaflet-control-zoom a { background:#161b22!important; color:#e6edf3!important; border-color:#30363d!important; }
.leaflet-popup-content-wrapper { background:#1c2330!important; color:#e6edf3!important; border:1px solid #30363d!important; border-radius:12px!important; }
.leaflet-popup-tip { background:#1c2330!important; }
</style>

</head>
<body>

<!-- PERMISSION -->

<div class="perm" id="pantallaPerm">
  <div class="perm-icon">🍽️</div>
  <div class="perm-title">Safe<span>Plate</span></div>
  <div class="perm-desc">Encontramos restaurantes veganos y sin gluten exactamente donde estas.</div>
  <div class="perm-error" id="errorPerm"></div>
  <button id="btnUbicacion">📍 Usar mi ubicacion</button>
  <div class="perm-note">Solo usamos tu GPS para buscar lugares cercanos. No guardamos nada.</div>
</div>

<!-- LOADING -->

<div class="loading" id="pantallaLoading">
  <div style="font-size:1.4rem;font-weight:800;">Safe<span style="color:#39d353">Plate</span></div>
  <div class="spinner"></div>
  <div class="load-txt" id="textoLoading">Obteniendo ubicacion...</div>
</div>

<!-- APP -->

<div class="topbar">
  <div class="logo">Safe<span>Plate</span></div>
  <div class="searchbox">
    <span style="color:#8b949e">🔍</span>
    <input type="text" id="inputBuscar" placeholder="Buscar...">
  </div>
</div>

<div class="filters" id="filtros">
  <div class="chip ag" data-filtro="todos">Todos</div>
  <div class="chip"    data-filtro="celiac">Sin Gluten</div>
  <div class="chip"    data-filtro="vegan">Vegano</div>
  <div class="chip"    data-filtro="ambos">SG + Vegano</div>
  <div class="chip"    data-filtro="cafe">Cafeterias</div>
</div>

<div id="map"></div>

<div class="sheet" id="sheet">
  <div class="sheet-top" id="sheetTop">
    <div>
      <div class="handle"></div>
      <div class="sheet-label">Lugares cercanos</div>
      <div class="sheet-sub"><b id="contador">0</b> resultados</div>
    </div>
    <div class="toggle-btn" id="toggleBtn">Ver lista</div>
  </div>
  <div class="cards" id="listaCards">
    <div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:20px;">
      Acepta el permiso de ubicacion para comenzar
    </div>
  </div>
</div>

<!-- DETAIL -->

<div class="detail" id="pantallaDetalle">
  <div class="det-head">
    <button class="back-btn" id="btnVolver">← Volver</button>
    <span style="font-size:0.82rem;color:#8b949e;font-weight:600;">Detalle</span>
  </div>
  <div class="det-body">
    <div class="det-name" id="dNombre"></div>
    <div class="det-sub"  id="dTipo"></div>
    <div class="det-tags" id="dTags"></div>
    <div class="det-sec">Informacion del lugar</div>
    <div id="dInfo"></div>
  </div>
  <div class="det-foot">
    <button class="btn-p" id="btnNavegar">Como llegar</button>
    <button class="btn-s" id="btnOSM">Ver en mapa</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ── VARIABLES ─────────────────────────────────────────────────────────────
var mapa, uLat, uLng;
var todos = [], visibles = [], marcadores = {};
var selId = null, filtro = 'todos', sheetAbierto = false;
var lugarActual = null;

// ── EVENTOS — sin onclick en HTML, resistente al traductor ────────────────
document.addEventListener('DOMContentLoaded', function() {

  // Boton ubicacion — usa tanto click como touchend para maxima compatibilidad
  var btn = document.getElementById('btnUbicacion');
  btn.addEventListener('touchend', function(e) {
    e.preventDefault();
    pedirUbicacion();
  });
  btn.addEventListener('click', function(e) {
    e.preventDefault();
    pedirUbicacion();
  });

  // Buscar
  document.getElementById('inputBuscar').addEventListener('input', aplicarFiltros);

  // Chips de filtro
  document.getElementById('filtros').addEventListener('click', function(e) {
    var chip = e.target.closest('[data-filtro]');
    if (chip) setFiltro(chip.getAttribute('data-filtro'));
  });

  // Sheet toggle
  document.getElementById('sheetTop').addEventListener('click', toggleSheet);

  // Volver desde detalle
  document.getElementById('btnVolver').addEventListener('click', cerrarDetalle);
  document.getElementById('btnVolver').addEventListener('touchend', function(e) {
    e.preventDefault(); cerrarDetalle();
  });

});

// ── UBICACION ─────────────────────────────────────────────────────────────
function pedirUbicacion() {
  if (!navigator.geolocation) {
    mostrarError('Tu navegador no soporta GPS. Proba con Chrome o Safari.');
    return;
  }
  document.getElementById('pantallaPerm').style.display = 'none';
  var ls = document.getElementById('pantallaLoading');
  ls.style.display = 'flex';
  setTexto('Obteniendo tu ubicacion...');

  navigator.geolocation.getCurrentPosition(
    function(pos) {
      uLat = pos.coords.latitude;
      uLng = pos.coords.longitude;
      setTexto('Iniciando mapa...');
      iniciarMapa();
      setTexto('Buscando lugares cercanos...');
      buscarLugares();
    },
    function(err) {
      ls.style.display = 'none';
      var msg = 'No se pudo obtener la ubicacion.';
      if (err.code === 1) msg = 'Permiso denegado. Ve a Ajustes > Chrome > Ubicacion y activala, luego recarga la pagina.';
      if (err.code === 2) msg = 'Ubicacion no disponible. Activa el GPS o conectate a WiFi.';
      if (err.code === 3) msg = 'Tiempo agotado. Intenta de nuevo.';
      mostrarError(msg);
    },
    { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
  );
}

function mostrarError(msg) {
  var e = document.getElementById('errorPerm');
  e.textContent = msg;
  e.style.display = 'block';
  document.getElementById('pantallaPerm').style.display = 'flex';
  document.getElementById('btnUbicacion').textContent = 'Reintentar';
}

function setTexto(t) { document.getElementById('textoLoading').textContent = t; }

// ── MAPA ──────────────────────────────────────────────────────────────────
function iniciarMapa() {
  mapa = L.map('map', { zoomControl: false, attributionControl: false })
    .setView([uLat, uLng], 15);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(mapa);
  L.control.zoom({ position: 'bottomright' }).addTo(mapa);

  var iconoYo = L.divIcon({
    className: '',
    html: '<div style="width:20px;height:20px;background:#7ab8f5;border:3px solid white;border-radius:50%;box-shadow:0 0 0 8px rgba(122,184,245,0.2)"></div>',
    iconSize: [20, 20], iconAnchor: [10, 10]
  });

  L.marker([uLat, uLng], { icon: iconoYo, zIndexOffset: 9999 })
    .addTo(mapa).bindPopup('<b style="color:#7ab8f5">Tu ubicacion</b>');

  mapa.on('click', function() { selId = null; renderMarcadores(); });
}

// ── OVERPASS API ──────────────────────────────────────────────────────────
function buscarLugares() {
  var r = 1500;
  var q = '[out:json][timeout:25];('
    + 'node["amenity"="restaurant"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'node["amenity"="cafe"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'node["amenity"="fast_food"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'node["shop"="bakery"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'node["shop"="health_food"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="restaurant"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="cafe"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="fast_food"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + 'way["shop"="bakery"](around:' + r + ',' + uLat + ',' + uLng + ');'
    + ');out center tags;';

  fetch('https://overpass-api.de/api/interpreter', {
    method: 'POST',
    body: 'data=' + encodeURIComponent(q)
  })
  .then(function(res) { if (!res.ok) throw new Error(); return res.json(); })
  .then(function(data) { procesarLugares(data.elements); })
  .catch(function() {
    document.getElementById('pantallaLoading').style.display = 'none';
    document.getElementById('listaCards').innerHTML =
      '<div style="text-align:center;color:#f08080;font-size:0.83rem;padding:20px;">Error al cargar lugares. Verifica tu conexion a internet.</div>';
    toast('Sin conexion. Verifica internet.');
  });
}

// ── PROCESAR DATOS ────────────────────────────────────────────────────────
function procesarLugares(els) {
  todos = [];
  for (var i = 0; i < els.length; i++) {
    var e = els[i];
    if (!e.tags || !e.tags.name) continue;
    var t = e.tags;
    var lat = e.lat || (e.center && e.center.lat);
    var lng = e.lon || (e.center && e.center.lon);
    if (!lat || !lng) continue;

    var gf = t['diet:gluten_free'] === 'yes' || t['diet:gluten_free'] === 'only';
    var ve = t['diet:vegan'] === 'yes' || t['diet:vegan'] === 'only';
    var vg = t['diet:vegetarian'] === 'yes' || t['diet:vegetarian'] === 'only';
    var cat = 'none';
    if (gf && ve) cat = 'ambos';
    else if (gf)  cat = 'celiac';
    else if (ve)  cat = 'vegan';
    else if (vg)  cat = 'vegetarian';

    var am = t.amenity || t.shop || '';
    var tipo = 'Restaurante';
    if (am === 'cafe')          tipo = 'Cafeteria';
    else if (am === 'bakery')   tipo = 'Panaderia';
    else if (am === 'fast_food') tipo = 'Fast Food';
    else if (am === 'health_food') tipo = 'Tienda Saludable';

    var dist = calcDist(uLat, uLng, lat, lng);
    var info = [];
    if (t['diet:vegan'] === 'only')         info.push('100% Vegano');
    else if (t['diet:vegan'] === 'yes')     info.push('Opciones veganas disponibles');
    if (t['diet:gluten_free'] === 'only')   info.push('100% Sin gluten');
    else if (t['diet:gluten_free'] === 'yes') info.push('Opciones sin gluten disponibles');
    if (vg) info.push('Opciones vegetarianas');
    if (t.cuisine)        info.push('Cocina: ' + t.cuisine.replace(/_/g, ' '));
    if (t.opening_hours)  info.push('Horario: ' + t.opening_hours);
    if (t['addr:street']) info.push('Direccion: ' + t['addr:street'] + (t['addr:housenumber'] ? ' ' + t['addr:housenumber'] : ''));
    if (t.phone)    info.push('Tel: ' + t.phone);
    if (t.website)  info.push('Web: ' + t.website.replace(/^https?:\/\//, '').slice(0, 35));
    if (!info.length) info.push('Sin informacion adicional.');

    todos.push({ id: e.id, nombre: t.name, tipo: tipo, cat: cat, lat: lat, lng: lng, dist: dist, info: info, osmId: e.id, osmTipo: e.type || 'node' });
  }
  todos.sort(function(a, b) { return a.dist - b.dist; });
  document.getElementById('pantallaLoading').style.display = 'none';
  aplicarFiltros();
  toast('✅ ' + todos.length + ' lugares encontrados');
}

function calcDist(a, b, c, d) {
  var R = 6371000, dA = (c-a)*Math.PI/180, dB = (d-b)*Math.PI/180;
  var x = Math.sin(dA/2)*Math.sin(dA/2) + Math.cos(a*Math.PI/180)*Math.cos(c*Math.PI/180)*Math.sin(dB/2)*Math.sin(dB/2);
  return Math.round(R * 2 * Math.atan2(Math.sqrt(x), Math.sqrt(1-x)));
}
function fmt(m) { return m < 1000 ? m + 'm' : (m/1000).toFixed(1) + 'km'; }

// ── FILTROS ───────────────────────────────────────────────────────────────
function setFiltro(f) {
  filtro = f;
  var chips = document.querySelectorAll('.chip');
  for (var i = 0; i < chips.length; i++) {
    chips[i].className = 'chip';
  }
  var cl = f === 'vegan' ? 'at' : f === 'ambos' ? 'aa' : 'ag';
  var chip = document.querySelector('[data-filtro="' + f + '"]');
  if (chip) chip.classList.add(cl);
  aplicarFiltros();
}

function aplicarFiltros() {
  var q = document.getElementById('inputBuscar').value.toLowerCase();
  visibles = todos.filter(function(p) {
    var mf = filtro === 'todos'  ? true
           : filtro === 'celiac' ? (p.cat === 'celiac' || p.cat === 'ambos')
           : filtro === 'vegan'  ? (p.cat === 'vegan'  || p.cat === 'ambos')
           : filtro === 'ambos'  ? p.cat === 'ambos'
           : filtro === 'cafe'   ? p.tipo === 'Cafeteria'
           : true;
    return mf && (q ? p.nombre.toLowerCase().indexOf(q) >= 0 : true);
  });
  document.getElementById('contador').textContent = visibles.length;
  renderLista();
  if (mapa) renderMarcadores();
}

// ── LISTA ─────────────────────────────────────────────────────────────────
function emo(t) { return t==='Cafeteria'?'☕':t==='Panaderia'?'🥖':t==='Fast Food'?'🍔':'🍽️'; }
function col(c) { return c==='celiac'?'#39d353':c==='vegan'?'#00c9a7':c==='ambos'?'#f0a500':'#8b949e'; }

function renderLista() {
  var el = document.getElementById('listaCards');
  if (!visibles.length) {
    el.innerHTML = '<div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:20px;">Sin resultados' + (filtro !== 'todos' ? ' para este filtro.' : '.') + '</div>';
    return;
  }
  var frag = document.createDocumentFragment();
  var max = Math.min(visibles.length, 60);
  for (var i = 0; i < max; i++) {
    (function(p) {
      var div = document.createElement('div');
      var cls = 'card' + (p.cat==='vegan'?' cv':p.cat==='ambos'?' ca':p.cat==='none'?' cn':'') + (selId===p.id?' sel':'');
      div.className = cls;
      div.id = 'card' + p.id;
      div.innerHTML = '<div class="card-top"><div class="card-name">' + p.nombre + '</div><div class="card-dist">' + fmt(p.dist) + '</div></div>'
        + '<div class="card-type">' + emo(p.tipo) + ' ' + p.tipo + '</div>'
        + '<div class="tags">'
        + (p.cat==='celiac'||p.cat==='ambos' ? '<span class="tag tg">Sin Gluten</span>' : '')
        + (p.cat==='vegan' ||p.cat==='ambos' ? '<span class="tag tv">Vegano</span>' : '')
        + (p.cat==='vegetarian' ? '<span class="tag tvg">Vegetariano</span>' : '')
        + (p.cat==='none' ? '<span class="tag tn">Sin datos</span>' : '')
        + '</div>';
      div.addEventListener('click', function() { abrirDetalle(p); });
      div.addEventListener('touchend', function(e) { e.preventDefault(); abrirDetalle(p); });
      frag.appendChild(div);
    })(visibles[i]);
  }
  el.innerHTML = '';
  el.appendChild(frag);
}

// ── MARCADORES ────────────────────────────────────────────────────────────
function renderMarcadores() {
  var ids = Object.keys(marcadores);
  for (var i = 0; i < ids.length; i++) mapa.removeLayer(marcadores[ids[i]]);
  marcadores = {};

  var vsSet = {};
  for (var j = 0; j < visibles.length; j++) vsSet[visibles[j].id] = true;

  for (var k = 0; k < todos.length; k++) {
    (function(p) {
      var esV = !!vsSet[p.id];
      var esSel = selId === p.id;
      var c = col(p.cat);
      var sz = esSel ? 46 : 34;
      var icon = L.divIcon({
        className: '',
        html: '<div style="width:' + sz + 'px;height:' + sz + 'px;border-radius:50%;'
          + 'background:' + c + '25;border:2.5px solid ' + c + ';'
          + 'display:flex;align-items:center;justify-content:center;'
          + 'font-size:' + (esSel?18:13) + 'px;opacity:' + (esV?1:0.15) + ';'
          + 'box-shadow:0 3px 10px rgba(0,0,0,0.5);cursor:pointer;">'
          + emo(p.tipo) + '</div>',
        iconSize: [sz, sz], iconAnchor: [sz/2, sz/2]
      });
      var mk = L.marker([p.lat, p.lng], { icon: icon, zIndexOffset: esSel ? 100 : 0 }).addTo(mapa);
      if (esV) mk.on('click', function(e) { e.originalEvent.stopPropagation(); abrirDetalle(p); });
      marcadores[p.id] = mk;
    })(todos[k]);
  }
}

// ── DETALLE ───────────────────────────────────────────────────────────────
function abrirDetalle(p) {
  lugarActual = p;
  selId = p.id;

  var cards = document.querySelectorAll('.card');
  for (var i = 0; i < cards.length; i++) cards[i].classList.remove('sel');
  var c = document.getElementById('card' + p.id);
  if (c) { c.classList.add('sel'); c.scrollIntoView({ behavior: 'smooth', block: 'nearest' }); }

  if (mapa) mapa.flyTo([p.lat, p.lng], 17, { animate: true, duration: 0.6 });
  renderMarcadores();

  document.getElementById('dNombre').textContent = p.nombre;
  document.getElementById('dTipo').textContent = emo(p.tipo) + ' ' + p.tipo + '  ·  ' + fmt(p.dist);

  var bt = document.getElementById('dTags');
  bt.innerHTML = '';
  if (p.cat==='celiac'||p.cat==='ambos') bt.innerHTML += '<span class="tag tg" style="font-size:0.78rem;padding:4px 11px;">Sin Gluten</span>';
  if (p.cat==='vegan' ||p.cat==='ambos') bt.innerHTML += '<span class="tag tv" style="font-size:0.78rem;padding:4px 11px;">Vegano</span>';
  if (p.cat==='vegetarian') bt.innerHTML += '<span class="tag tvg" style="font-size:0.78rem;padding:4px 11px;">Vegetariano</span>';
  if (p.cat==='none') bt.innerHTML += '<span class="tag tn" style="font-size:0.78rem;padding:4px 11px;">Sin datos dieteticos</span>';

  var di = document.getElementById('dInfo');
  di.innerHTML = '';
  for (var i = 0; i < p.info.length; i++) {
    var d = document.createElement('div');
    d.className = 'det-line';
    d.textContent = p.info[i];
    di.appendChild(d);
  }

  document.getElementById('btnNavegar').onclick = function() {
    window.open('https://www.google.com/maps/dir/?api=1&destination=' + p.lat + ',' + p.lng, '_blank');
  };
  document.getElementById('btnOSM').onclick = function() {
    window.open('https://www.openstreetmap.org/' + p.osmTipo + '/' + p.osmId, '_blank');
  };

  document.getElementById('pantallaDetalle').classList.add('open');
}

function cerrarDetalle() {
  document.getElementById('pantallaDetalle').classList.remove('open');
  selId = null;
  renderMarcadores();
}

// ── SHEET ─────────────────────────────────────────────────────────────────
function toggleSheet() {
  sheetAbierto = !sheetAbierto;
  document.getElementById('sheet').classList.toggle('big', sheetAbierto);
  document.getElementById('toggleBtn').textContent = sheetAbierto ? 'Minimizar' : 'Ver lista';
  document.getElementById('toast').style.bottom = sheetAbierto ? 'calc(65vh + 14px)' : 'calc(35vh + 14px)';
}

// ── TOAST ─────────────────────────────────────────────────────────────────
function toast(msg) {
  var t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('on');
  setTimeout(function() { t.classList.remove('on'); }, 3000);
}
</script>

</body>
</html>
