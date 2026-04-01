<!DOCTYPE html>

<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="google" content="notranslate">
<meta name="theme-color" content="#0d1117">
<title>SafePlate</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
* { margin:0; padding:0; box-sizing:border-box; }

body {
font-family: -apple-system, BlinkMacSystemFont, ‘Segoe UI’, sans-serif;
background: #0d1117;
color: #e6edf3;
height: 100dvh;
display: flex;
flex-direction: column;
overflow: hidden;
}

/* TOPBAR */
.topbar {
background: #161b22;
border-bottom: 1px solid #30363d;
padding: 12px 16px;
display: flex;
align-items: center;
gap: 10px;
flex-shrink: 0;
}
.logo {
font-weight: 800;
font-size: 1.2rem;
letter-spacing: -0.5px;
white-space: nowrap;
}
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
background: none;
border: none;
outline: none;
color: #e6edf3;
font-size: 0.87rem;
width: 100%;
}
.searchbox input::placeholder { color: #8b949e; }

/* FILTERS */
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
}
.chip.active-g { background: #1a4a2e; border-color: #39d353; color: #39d353; }
.chip.active-t { background: #003d32; border-color: #00c9a7; color: #00c9a7; }
.chip.active-a { background: #3d2c00; border-color: #f0a500; color: #f0a500; }

/* MAP */
#map {
flex: 1;
min-height: 0;
z-index: 1;
}

/* BOTTOM SHEET */
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
.handle {
width: 36px; height: 4px;
background: #30363d;
border-radius: 4px;
margin: 0 auto 8px;
}
.sheet-info { font-size: 0.85rem; font-weight: 600; }
.sheet-count { font-size: 0.73rem; color: #8b949e; margin-top: 1px; }
.sheet-count b { color: #39d353; }
.expand-btn {
font-size: 0.72rem;
color: #8b949e;
background: #1c2330;
border: 1px solid #30363d;
border-radius: 50px;
padding: 4px 10px;
cursor: pointer;
}

.cards {
flex: 1;
overflow-y: auto;
padding: 0 10px 12px;
display: flex;
flex-direction: column;
gap: 8px;
-webkit-overflow-scrolling: touch;
}

/* CARD */
.card {
background: #1c2330;
border: 1px solid #30363d;
border-left: 3px solid #39d353;
border-radius: 12px;
padding: 12px 14px;
cursor: pointer;
flex-shrink: 0;
transition: background 0.15s;
}
.card.vegan  { border-left-color: #00c9a7; }
.card.ambos  { border-left-color: #f0a500; }
.card.none   { border-left-color: #8b949e; }
.card.active { background: rgba(57,211,83,0.08); }

.card-top {
display: flex;
justify-content: space-between;
margin-bottom: 3px;
}
.card-name { font-weight: 700; font-size: 0.88rem; }
.card-dist { font-size: 0.72rem; color: #8b949e; }
.card-type { font-size: 0.75rem; color: #8b949e; margin-bottom: 6px; }
.tags { display: flex; flex-wrap: wrap; gap: 4px; }
.tag {
font-size: 0.65rem;
font-weight: 600;
padding: 2px 7px;
border-radius: 50px;
}
.tag-g { background: #1a4a2e; color: #39d353; border: 1px solid rgba(57,211,83,0.3); }
.tag-v { background: #003d32; color: #00c9a7; border: 1px solid rgba(0,201,167,0.3); }
.tag-vg { background: #1e2d3d; color: #7ab8f5; border: 1px solid rgba(122,184,245,0.3); }
.tag-n { background: #2a1e1e; color: #f08080; border: 1px solid rgba(240,128,128,0.3); }

/* DETAIL */
.detail {
position: fixed;
inset: 0;
background: #0d1117;
z-index: 9000;
display: flex;
flex-direction: column;
transform: translateY(100%);
transition: transform 0.3s ease;
}
.detail.show { transform: translateY(0); }

.detail-header {
padding: 16px;
border-bottom: 1px solid #30363d;
display: flex;
align-items: center;
justify-content: space-between;
flex-shrink: 0;
}
.back-btn {
background: #1c2330;
border: 1px solid #30363d;
color: #8b949e;
border-radius: 50px;
padding: 7px 14px;
font-size: 0.83rem;
cursor: pointer;
}
.detail-body { flex: 1; overflow-y: auto; padding: 18px; -webkit-overflow-scrolling: touch; }
.detail-name { font-size: 1.4rem; font-weight: 800; margin-bottom: 4px; }
.detail-sub { font-size: 0.83rem; color: #8b949e; margin-bottom: 14px; }
.detail-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 16px; }
.detail-section {
font-size: 0.72rem;
font-weight: 700;
text-transform: uppercase;
letter-spacing: 1px;
color: #8b949e;
margin-bottom: 8px;
margin-top: 16px;
}
.detail-line { font-size: 0.85rem; color: #8b949e; margin-bottom: 6px; line-height: 1.5; }

.detail-footer {
padding: 14px 16px;
border-top: 1px solid #30363d;
display: flex;
gap: 10px;
flex-shrink: 0;
}
.btn-primary {
flex: 1; padding: 13px;
background: #39d353; color: #000;
border: none; border-radius: 50px;
font-weight: 700; font-size: 0.88rem;
cursor: pointer;
}
.btn-secondary {
flex: 1; padding: 13px;
background: #1c2330; color: #e6edf3;
border: 1.5px solid #30363d; border-radius: 50px;
font-weight: 700; font-size: 0.88rem;
cursor: pointer;
}

/* PERMISSION SCREEN */
.perm-screen {
position: fixed;
inset: 0;
background: #0d1117;
z-index: 9999;
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
padding: 32px 24px;
text-align: center;
gap: 16px;
}
.perm-icon { font-size: 3.5rem; }
.perm-title { font-size: 1.8rem; font-weight: 800; }
.perm-title span { color: #39d353; }
.perm-desc { font-size: 0.9rem; color: #8b949e; max-width: 300px; line-height: 1.6; }
.perm-btn {
width: 100%; max-width: 300px;
padding: 15px;
background: #39d353; color: #000;
border: none; border-radius: 50px;
font-size: 1rem; font-weight: 700;
cursor: pointer;
}
.perm-error {
font-size: 0.8rem; color: #f08080;
background: rgba(240,80,80,0.1);
border: 1px solid rgba(240,80,80,0.3);
border-radius: 10px;
padding: 10px 14px;
max-width: 300px;
display: none;
}
.perm-note { font-size: 0.72rem; color: #8b949e; max-width: 260px; }

/* LOADING */
.loading-screen {
position: fixed;
inset: 0;
background: #0d1117;
z-index: 8000;
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
gap: 14px;
display: none;
}
.spinner {
width: 44px; height: 44px;
border: 3px solid #30363d;
border-top-color: #39d353;
border-radius: 50%;
animation: spin 0.8s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }
.loading-text { font-size: 0.85rem; color: #8b949e; }

/* TOAST */
.toast {
position: fixed;
bottom: calc(35vh + 14px);
left: 50%;
transform: translateX(-50%) translateY(8px);
background: #161b22;
border: 1px solid #39d353;
border-radius: 50px;
padding: 7px 16px;
font-size: 0.8rem;
color: #39d353;
opacity: 0;
pointer-events: none;
transition: all 0.25s;
z-index: 5000;
white-space: nowrap;
}
.toast.show {
opacity: 1;
transform: translateX(-50%) translateY(0);
}

/* Leaflet dark */
.leaflet-tile { filter: brightness(0.7) saturate(0.6); }
.leaflet-control-zoom a {
background: #161b22 !important;
color: #e6edf3 !important;
border-color: #30363d !important;
}
.leaflet-popup-content-wrapper {
background: #1c2330 !important;
color: #e6edf3 !important;
border: 1px solid #30363d !important;
border-radius: 12px !important;
}
.leaflet-popup-tip { background: #1c2330 !important; }
</style>

</head>
<body>

<!-- PERMISSION -->

<div class="perm-screen" id="permScreen">
  <div class="perm-icon">🍽️</div>
  <div class="perm-title">Safe<span>Plate</span></div>
  <div class="perm-desc">Encontramos restaurantes veganos y sin gluten exactamente donde estás ahora.</div>
  <div class="perm-error" id="permError"></div>
  <button class="perm-btn" id="permBtn" onclick="pedirUbicacion()">📍 Usar mi ubicación</button>
  <div class="perm-note">Solo usamos tu GPS para buscar lugares cercanos. No guardamos nada.</div>
</div>

<!-- LOADING -->

<div class="loading-screen" id="loadingScreen">
  <div style="font-size:1.4rem;font-weight:800;">Safe<span style="color:#39d353">Plate</span></div>
  <div class="spinner"></div>
  <div class="loading-text" id="loadingText">Obteniendo ubicación...</div>
</div>

<!-- APP -->

<div class="topbar">
  <div class="logo">Safe<span>Plate</span></div>
  <div class="searchbox">
    <span style="color:#8b949e">🔍</span>
    <input type="text" id="busqueda" placeholder="Buscar..." oninput="aplicarFiltros()">
  </div>
</div>

<div class="filters">
  <div class="chip active-g" id="chip-todos"  onclick="setFiltro('todos')">✦ Todos</div>
  <div class="chip"          id="chip-celiac" onclick="setFiltro('celiac')">🌾 Sin Gluten</div>
  <div class="chip"          id="chip-vegan"  onclick="setFiltro('vegan')">🌿 Vegano</div>
  <div class="chip"          id="chip-ambos"  onclick="setFiltro('ambos')">✨ SG+Vegano</div>
  <div class="chip"          id="chip-cafe"   onclick="setFiltro('cafe')">☕ Cafeterias</div>
</div>

<div id="map"></div>

<div class="sheet" id="sheet">
  <div class="sheet-top" onclick="toggleSheet()">
    <div>
      <div class="handle"></div>
      <div class="sheet-info">Lugares cercanos</div>
      <div class="sheet-count"><b id="contador">0</b> resultados</div>
    </div>
    <div class="expand-btn" id="expandBtn">▲ Ver lista</div>
  </div>
  <div class="cards" id="listaLugares">
    <div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:20px;">
      Primero acepta el permiso de ubicación
    </div>
  </div>
</div>

<!-- DETAIL -->

<div class="detail" id="detalle">
  <div class="detail-header">
    <button class="back-btn" onclick="cerrarDetalle()">← Volver</button>
    <span style="font-size:0.82rem;color:#8b949e;font-weight:600;">Detalle</span>
  </div>
  <div class="detail-body">
    <div class="detail-name"  id="d-nombre"></div>
    <div class="detail-sub"   id="d-tipo"></div>
    <div class="detail-tags"  id="d-tags"></div>
    <div class="detail-section">Informacion del lugar</div>
    <div id="d-info"></div>
  </div>
  <div class="detail-footer">
    <button class="btn-primary"   id="btnNavegar">🧭 Como llegar</button>
    <button class="btn-secondary" id="btnOSM">🗺 Ver en mapa</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
var mapa, uLat, uLng;
var todos = [], visibles = [], marcadores = {};
var seleccionado = null, filtroActual = 'todos', sheetGrande = false;

function pedirUbicacion() {
  if (!navigator.geolocation) {
    mostrarError('Tu navegador no soporta GPS.');
    return;
  }
  document.getElementById('permScreen').style.display = 'none';
  var ls = document.getElementById('loadingScreen');
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
      var msg = 'No se pudo obtener la ubicacion.';
      if (err.code === 1) msg = 'Permiso denegado. Ve a Ajustes del celular, busca Chrome o Safari, activa Ubicacion, y recarga la pagina.';
      if (err.code === 2) msg = 'Ubicacion no disponible. Verifica tu GPS o WiFi.';
      if (err.code === 3) msg = 'Tiempo agotado. Intenta de nuevo.';
      ls.style.display = 'none';
      mostrarError(msg);
    },
    { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
  );
}

function mostrarError(msg) {
  var e = document.getElementById('permError');
  e.textContent = msg;
  e.style.display = 'block';
  document.getElementById('permScreen').style.display = 'flex';
  document.getElementById('permBtn').textContent = '🔄 Reintentar';
}

function setTexto(t) {
  document.getElementById('loadingText').textContent = t;
}

function iniciarMapa() {
  mapa = L.map('map', { zoomControl: false, attributionControl: false })
    .setView([uLat, uLng], 15);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19
  }).addTo(mapa);

  L.control.zoom({ position: 'bottomright' }).addTo(mapa);

  var iconoYo = L.divIcon({
    className: '',
    html: '<div style="width:20px;height:20px;background:#7ab8f5;border:3px solid white;border-radius:50%;box-shadow:0 0 0 8px rgba(122,184,245,0.2)"></div>',
    iconSize: [20, 20],
    iconAnchor: [10, 10]
  });

  L.marker([uLat, uLng], { icon: iconoYo, zIndexOffset: 9999 })
    .addTo(mapa)
    .bindPopup('<b style="color:#7ab8f5">Tu ubicacion</b>');

  mapa.on('click', function() {
    seleccionado = null;
    renderMarcadores();
  });
}

function buscarLugares() {
  var radio = 1500;
  var query = '[out:json][timeout:25];('
    + 'node["amenity"="restaurant"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'node["amenity"="cafe"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'node["amenity"="fast_food"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'node["shop"="bakery"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'node["shop"="health_food"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="restaurant"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="cafe"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'way["amenity"="fast_food"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + 'way["shop"="bakery"](around:' + radio + ',' + uLat + ',' + uLng + ');'
    + ');out center tags;';

  fetch('https://overpass-api.de/api/interpreter', {
    method: 'POST',
    body: 'data=' + encodeURIComponent(query)
  })
  .then(function(r) {
    if (!r.ok) throw new Error('Error API');
    return r.json();
  })
  .then(function(data) {
    procesarLugares(data.elements);
  })
  .catch(function() {
    document.getElementById('loadingScreen').style.display = 'none';
    mostrarToast('Sin conexion a OpenStreetMap. Verifica internet.');
    document.getElementById('listaLugares').innerHTML =
      '<div style="text-align:center;color:#f08080;font-size:0.83rem;padding:20px;">Error al cargar. Verifica tu conexion a internet.</div>';
  });
}

function procesarLugares(elementos) {
  todos = [];
  for (var i = 0; i < elementos.length; i++) {
    var e = elementos[i];
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
    if (am === 'cafe')         tipo = 'Cafeteria';
    else if (am === 'bakery')  tipo = 'Panaderia';
    else if (am === 'fast_food') tipo = 'Fast Food';
    else if (am === 'health_food') tipo = 'Tienda Saludable';

    var dist = calcDist(uLat, uLng, lat, lng);
    var info = [];
    if (t['diet:vegan'] === 'only')        info.push('🌿 100% Vegano');
    else if (t['diet:vegan'] === 'yes')    info.push('🌿 Opciones veganas');
    if (t['diet:gluten_free'] === 'only')  info.push('🌾 100% Sin gluten');
    else if (t['diet:gluten_free'] === 'yes') info.push('🌾 Opciones sin gluten');
    if (vg) info.push('🥗 Opciones vegetarianas');
    if (t.cuisine)        info.push('🍴 ' + t.cuisine.replace(/_/g, ' '));
    if (t.opening_hours)  info.push('🕐 ' + t.opening_hours);
    if (t['addr:street']) info.push('📌 ' + t['addr:street'] + (t['addr:housenumber'] ? ' ' + t['addr:housenumber'] : ''));
    if (t.phone)    info.push('📞 ' + t.phone);
    if (t.website)  info.push('🌐 ' + t.website.replace(/^https?:\/\//, '').slice(0, 35));
    if (info.length === 0) info.push('Sin informacion adicional en OpenStreetMap.');

    todos.push({
      id: e.id, nombre: t.name, tipo: tipo, cat: cat,
      lat: lat, lng: lng, dist: dist, info: info,
      osmId: e.id, osmTipo: e.type || 'node'
    });
  }

  todos.sort(function(a, b) { return a.dist - b.dist; });
  document.getElementById('loadingScreen').style.display = 'none';
  aplicarFiltros();
  mostrarToast('✅ ' + todos.length + ' lugares encontrados');
}

function calcDist(lat1, lon1, lat2, lon2) {
  var R = 6371000;
  var dLat = (lat2 - lat1) * Math.PI / 180;
  var dLon = (lon2 - lon1) * Math.PI / 180;
  var a = Math.sin(dLat/2) * Math.sin(dLat/2) +
    Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
    Math.sin(dLon/2) * Math.sin(dLon/2);
  return Math.round(R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a)));
}

function fmtDist(m) {
  return m < 1000 ? m + 'm' : (m/1000).toFixed(1) + 'km';
}

function setFiltro(f) {
  filtroActual = f;
  var chips = document.querySelectorAll('.chip');
  for (var i = 0; i < chips.length; i++) {
    chips[i].className = 'chip';
  }
  var cls = f === 'vegan' ? 'active-t' : f === 'ambos' ? 'active-a' : 'active-g';
  var el = document.getElementById('chip-' + f);
  if (el) el.classList.add(cls);
  aplicarFiltros();
}

function aplicarFiltros() {
  var q = document.getElementById('busqueda').value.toLowerCase();
  visibles = todos.filter(function(p) {
    var mf = filtroActual === 'todos' ? true
      : filtroActual === 'celiac' ? (p.cat === 'celiac' || p.cat === 'ambos')
      : filtroActual === 'vegan'  ? (p.cat === 'vegan'  || p.cat === 'ambos')
      : filtroActual === 'ambos'  ? p.cat === 'ambos'
      : filtroActual === 'cafe'   ? p.tipo === 'Cafeteria'
      : true;
    var ms = q ? p.nombre.toLowerCase().indexOf(q) >= 0 : true;
    return mf && ms;
  });
  document.getElementById('contador').textContent = visibles.length;
  renderLista();
  if (mapa) renderMarcadores();
}

function getEmoji(tipo) {
  if (tipo === 'Cafeteria') return '☕';
  if (tipo === 'Panaderia') return '🥖';
  if (tipo === 'Fast Food') return '🍔';
  return '🍽️';
}

function getColor(cat) {
  if (cat === 'celiac') return '#39d353';
  if (cat === 'vegan')  return '#00c9a7';
  if (cat === 'ambos')  return '#f0a500';
  return '#8b949e';
}

function renderLista() {
  var el = document.getElementById('listaLugares');
  if (visibles.length === 0) {
    el.innerHTML = '<div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:20px;">Sin resultados' + (filtroActual !== 'todos' ? ' para este filtro.' : '.') + '</div>';
    return;
  }
  var html = '';
  var max = Math.min(visibles.length, 60);
  for (var i = 0; i < max; i++) {
    var p = visibles[i];
    var cardCls = p.cat === 'vegan' ? 'card vegan' : p.cat === 'ambos' ? 'card ambos' : p.cat === 'none' ? 'card none' : 'card';
    if (seleccionado === p.id) cardCls += ' active';
    html += '<div class="' + cardCls + '" id="card' + p.id + '" onclick="abrirDetalle(' + i + ')">';
    html += '<div class="card-top"><div class="card-name">' + p.nombre + '</div><div class="card-dist">' + fmtDist(p.dist) + '</div></div>';
    html += '<div class="card-type">' + getEmoji(p.tipo) + ' ' + p.tipo + '</div>';
    html += '<div class="tags">';
    if (p.cat === 'celiac' || p.cat === 'ambos') html += '<span class="tag tag-g">🌾 Sin Gluten</span>';
    if (p.cat === 'vegan'  || p.cat === 'ambos') html += '<span class="tag tag-v">🌿 Vegano</span>';
    if (p.cat === 'vegetarian') html += '<span class="tag tag-vg">🥗 Vegetariano</span>';
    if (p.cat === 'none') html += '<span class="tag tag-n">Sin datos dieteticos</span>';
    html += '</div></div>';
  }
  el.innerHTML = html;
}

function renderMarcadores() {
  var ids = Object.keys(marcadores);
  for (var i = 0; i < ids.length; i++) {
    mapa.removeLayer(marcadores[ids[i]]);
  }
  marcadores = {};

  var visSet = {};
  for (var j = 0; j < visibles.length; j++) {
    visSet[visibles[j].id] = true;
  }

  for (var k = 0; k < todos.length; k++) {
    var p = todos[k];
    var esVis = visSet[p.id] ? true : false;
    var esSel = seleccionado === p.id;
    var c = getColor(p.cat);
    var sz = esSel ? 46 : 34;
    var op = esVis ? 1 : 0.15;

    var icon = L.divIcon({
      className: '',
      html: '<div style="width:' + sz + 'px;height:' + sz + 'px;border-radius:50%;'
        + 'background:' + c + '25;border:2.5px solid ' + c + ';'
        + 'display:flex;align-items:center;justify-content:center;'
        + 'font-size:' + (esSel ? 18 : 13) + 'px;opacity:' + op + ';'
        + 'box-shadow:0 3px 10px rgba(0,0,0,0.5);cursor:pointer;">'
        + getEmoji(p.tipo) + '</div>',
      iconSize: [sz, sz],
      iconAnchor: [sz/2, sz/2]
    });

    (function(lugar) {
      var mk = L.marker([lugar.lat, lugar.lng], { icon: icon, zIndexOffset: esSel ? 100 : 0 }).addTo(mapa);
      if (esVis) {
        mk.on('click', function(e) {
          e.originalEvent.stopPropagation();
          var idx = -1;
          for (var x = 0; x < visibles.length; x++) {
            if (visibles[x].id === lugar.id) { idx = x; break; }
          }
          if (idx >= 0) abrirDetalle(idx);
        });
      }
      marcadores[lugar.id] = mk;
    })(p);
  }
}

function abrirDetalle(idx) {
  var p = visibles[idx];
  if (!p) return;
  seleccionado = p.id;

  var cards = document.querySelectorAll('.card');
  for (var i = 0; i < cards.length; i++) cards[i].classList.remove('active');
  var card = document.getElementById('card' + p.id);
  if (card) {
    card.classList.add('active');
    card.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
  }

  if (mapa) mapa.flyTo([p.lat, p.lng], 17, { animate: true, duration: 0.6 });
  renderMarcadores();

  document.getElementById('d-nombre').textContent = p.nombre;
  document.getElementById('d-tipo').textContent = getEmoji(p.tipo) + ' ' + p.tipo + '  ·  📍 ' + fmtDist(p.dist);

  var tagsEl = document.getElementById('d-tags');
  tagsEl.innerHTML = '';
  if (p.cat === 'celiac' || p.cat === 'ambos') tagsEl.innerHTML += '<span class="tag tag-g" style="font-size:0.78rem;padding:4px 11px;">🌾 Sin Gluten</span>';
  if (p.cat === 'vegan'  || p.cat === 'ambos') tagsEl.innerHTML += '<span class="tag tag-v" style="font-size:0.78rem;padding:4px 11px;">🌿 Vegano</span>';
  if (p.cat === 'vegetarian') tagsEl.innerHTML += '<span class="tag tag-vg" style="font-size:0.78rem;padding:4px 11px;">🥗 Vegetariano</span>';
  if (p.cat === 'none') tagsEl.innerHTML += '<span class="tag tag-n" style="font-size:0.78rem;padding:4px 11px;">Sin datos en OSM</span>';

  var infoEl = document.getElementById('d-info');
  infoEl.innerHTML = '';
  for (var i = 0; i < p.info.length; i++) {
    infoEl.innerHTML += '<div class="detail-line">' + p.info[i] + '</div>';
  }

  document.getElementById('btnNavegar').onclick = function() {
    window.open('https://www.google.com/maps/dir/?api=1&destination=' + p.lat + ',' + p.lng, '_blank');
  };
  document.getElementById('btnOSM').onclick = function() {
    window.open('https://www.openstreetmap.org/' + p.osmTipo +un '/' + p.osmId, '_blank');
  };

  document.getElementById('detalle').classList.add('show');
}

function cerrarDetalle() {
  document.getElementById('detalle').classList.remove('show');
  seleccionado = null;
  renderMarcadores();
}

function toggleSheet() {
  sheetGrande = !sheetGrande;
  document.getElementById('sheet').classList.toggle('big', sheetGrande);
  document.getElementById('expandBtn').textContent = sheetGrande ? '▼ Minimizar' : '▲ Ver lista';
  document.querySelector('.toast').style.bottom = sheetGrande ? 'calc(65vh + 14px)' : 'calc(35vh + 14px)';
}

function mostrarToast(msg) {
  var t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(function() { t.classList.remove('show'); }, 3000);
}
</script>

</body>
</html>
