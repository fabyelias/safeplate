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
html, body { width:100%; height:100%; overflow:hidden; background:#0d1117; color:#e6edf3; font-family:-apple-system,BlinkMacSystemFont,sans-serif; }
body { display:flex; flex-direction:column; height:100dvh; }

.topbar { background:#161b22; border-bottom:1px solid #30363d; padding:12px 14px; padding-top:max(12px,env(safe-area-inset-top)); display:flex; align-items:center; gap:10px; flex-shrink:0; }
.logo { font-weight:800; font-size:1.15rem; white-space:nowrap; }
.logo span { color:#39d353; }
.searchbox { flex:1; min-width:0; display:flex; align-items:center; background:#1c2330; border:1px solid #30363d; border-radius:50px; padding:8px 13px; gap:6px; }
.searchbox input { background:none; border:none; outline:none; color:#e6edf3; font-size:0.85rem; width:100%; min-width:0; }
.searchbox input::placeholder { color:#8b949e; }

.filters { background:#161b22; border-bottom:1px solid #30363d; padding:8px 12px; display:flex; gap:6px; overflow-x:auto; flex-shrink:0; scrollbar-width:none; }
.filters::-webkit-scrollbar { display:none; }
.chip { padding:5px 13px; border-radius:50px; border:1.5px solid #30363d; background:#1c2330; color:#8b949e; font-size:0.78rem; font-weight:500; cursor:pointer; white-space:nowrap; flex-shrink:0; -webkit-user-select:none; user-select:none; touch-action:manipulation; }
.chip.ag { background:#1a4a2e; border-color:#39d353; color:#39d353; }
.chip.at { background:#003d32; border-color:#00c9a7; color:#00c9a7; }
.chip.aa { background:#3d2c00; border-color:#f0a500; color:#f0a500; }

#map { flex:1; min-height:0; z-index:1; }

.sheet { background:#161b22; border-top:1px solid #30363d; flex-shrink:0; height:36vh; display:flex; flex-direction:column; transition:height 0.3s ease; z-index:100; }
.sheet.big { height:65vh; }
.sheet-top { padding:10px 14px 8px; display:flex; align-items:center; justify-content:space-between; flex-shrink:0; cursor:pointer; }
.handle { width:32px; height:4px; background:#30363d; border-radius:4px; margin:0 auto 7px; }
.sheet-label { font-size:0.85rem; font-weight:600; }
.sheet-sub { font-size:0.72rem; color:#8b949e; margin-top:1px; }
.sheet-sub b { color:#39d353; }
.toggle-btn { font-size:0.72rem; color:#8b949e; background:#1c2330; border:1px solid #30363d; border-radius:50px; padding:4px 10px; cursor:pointer; touch-action:manipulation; flex-shrink:0; }

/* Radio indicator */
.radio-badge { font-size:0.68rem; color:#7ab8f5; background:#1e2d3d; border:1px solid rgba(122,184,245,0.3); border-radius:50px; padding:2px 8px; margin-left:6px; }

.cards { flex:1; overflow-y:scroll; -webkit-overflow-scrolling:touch; overscroll-behavior:contain; padding:4px 10px 12px; display:flex; flex-direction:column; gap:8px; touch-action:pan-y; }

.card { background:#1c2330; border:1px solid #30363d; border-left:3px solid #39d353; border-radius:12px; padding:12px 14px; cursor:pointer; flex-shrink:0; -webkit-tap-highlight-color:rgba(57,211,83,0.1); touch-action:manipulation; }
.card:active { background:rgba(57,211,83,0.06); }
.card.cv { border-left-color:#00c9a7; }
.card.ca { border-left-color:#f0a500; }
.card.cn { border-left-color:#8b949e; }
.card.sel { background:rgba(57,211,83,0.08); }

.card-top { display:flex; justify-content:space-between; margin-bottom:3px; }
.card-name { font-weight:700; font-size:0.88rem; flex:1; min-width:0; }
.card-dist { font-size:0.72rem; color:#8b949e; margin-left:8px; white-space:nowrap; }
.card-type { font-size:0.75rem; color:#8b949e; margin-bottom:6px; }
.tags { display:flex; flex-wrap:wrap; gap:4px; }
.tag { font-size:0.65rem; font-weight:600; padding:2px 7px; border-radius:50px; }
.tg  { background:#1a4a2e; color:#39d353; border:1px solid rgba(57,211,83,0.3); }
.tv  { background:#003d32; color:#00c9a7; border:1px solid rgba(0,201,167,0.3); }
.tvg { background:#1e2d3d; color:#7ab8f5; border:1px solid rgba(122,184,245,0.3); }
.tn  { background:#2a1e1e; color:#f08080; border:1px solid rgba(240,128,128,0.3); }
.tt  { background:#2d1e3d; color:#c084fc; border:1px solid rgba(192,132,252,0.3); }

/* DETALLE */
.detail { position:fixed; inset:0; background:#0d1117; z-index:9000; display:flex; flex-direction:column; transform:translateY(100%); transition:transform 0.3s ease; }
.detail.open { transform:translateY(0); }
.det-head { padding:14px 16px; padding-top:max(14px,env(safe-area-inset-top)); border-bottom:1px solid #30363d; display:flex; align-items:center; justify-content:space-between; flex-shrink:0; }
.back-btn { background:#1c2330; border:1px solid #30363d; color:#8b949e; border-radius:50px; padding:8px 16px; font-size:0.83rem; cursor:pointer; touch-action:manipulation; }
.det-body { flex:1; overflow-y:auto; padding:18px; -webkit-overflow-scrolling:touch; }
.det-name { font-size:1.3rem; font-weight:800; margin-bottom:4px; }
.det-sub  { font-size:0.83rem; color:#8b949e; margin-bottom:12px; }
.det-tags { display:flex; gap:6px; flex-wrap:wrap; margin-bottom:14px; }
.det-sec  { font-size:0.7rem; font-weight:700; text-transform:uppercase; letter-spacing:1px; color:#8b949e; margin-bottom:8px; margin-top:14px; }
.det-line { font-size:0.85rem; color:#c9d1d9; margin-bottom:8px; line-height:1.5; display:flex; align-items:flex-start; gap:8px; }
.det-line span { flex-shrink:0; }
.det-foot { padding:14px 16px; padding-bottom:max(14px,env(safe-area-inset-bottom)); border-top:1px solid #30363d; display:flex; gap:10px; flex-shrink:0; }
.btn-p { flex:1; padding:13px; background:#39d353; color:#000; border:none; border-radius:50px; font-weight:700; font-size:0.88rem; cursor:pointer; touch-action:manipulation; }
.btn-s { flex:1; padding:13px; background:#1c2330; color:#e6edf3; border:1.5px solid #30363d; border-radius:50px; font-weight:700; font-size:0.88rem; cursor:pointer; touch-action:manipulation; }

/* PERMISO */
.perm { position:fixed; inset:0; background:#0d1117; z-index:9999; display:flex; flex-direction:column; align-items:center; justify-content:center; padding:32px 24px; text-align:center; gap:16px; }
.perm-icon { font-size:3.5rem; line-height:1; }
.perm-title { font-size:1.8rem; font-weight:800; }
.perm-title span { color:#39d353; }
.perm-desc { font-size:0.9rem; color:#8b949e; max-width:300px; line-height:1.6; }
.perm-features { display:flex; flex-direction:column; gap:8px; max-width:300px; }
.perm-feat { display:flex; align-items:center; gap:10px; background:#161b22; border:1px solid #30363d; border-radius:12px; padding:10px 14px; font-size:0.82rem; color:#8b949e; text-align:left; }
.perm-feat b { color:#e6edf3; }
.perm-error { font-size:0.8rem; color:#f08080; background:rgba(240,80,80,0.1); border:1px solid rgba(240,80,80,0.3); border-radius:10px; padding:10px 14px; max-width:300px; display:none; line-height:1.5; }
#btnUbicacion { width:100%; max-width:300px; padding:16px; background:#39d353; color:#000; border:none; border-radius:50px; font-size:1rem; font-weight:700; cursor:pointer; touch-action:manipulation; -webkit-tap-highlight-color:transparent; }
.perm-note { font-size:0.72rem; color:#8b949e; max-width:260px; }

/* LOADING */
.loading { position:fixed; inset:0; background:#0d1117; z-index:8000; display:none; flex-direction:column; align-items:center; justify-content:center; gap:14px; }
.spinner { width:44px; height:44px; border:3px solid #30363d; border-top-color:#39d353; border-radius:50%; animation:spin 0.8s linear infinite; }
@keyframes spin { to { transform:rotate(360deg); } }
.load-txt { font-size:0.85rem; color:#8b949e; }
.load-sub { font-size:0.75rem; color:#30363d; margin-top:-8px; }

/* TOAST */
.toast { position:fixed; bottom:calc(36vh + 12px); left:50%; transform:translateX(-50%) translateY(8px); background:#161b22; border:1px solid #39d353; border-radius:50px; padding:7px 16px; font-size:0.8rem; color:#39d353; opacity:0; pointer-events:none; transition:all 0.25s; z-index:5000; white-space:nowrap; max-width:92vw; text-align:center; }
.toast.on { opacity:1; transform:translateX(-50%) translateY(0); }
.toast.err { border-color:#f0a500; color:#f0a500; }

/* Leaflet */
.leaflet-tile { filter:brightness(0.72) saturate(0.6); }
.leaflet-control-zoom a { background:#161b22!important; color:#e6edf3!important; border-color:#30363d!important; }
.leaflet-popup-content-wrapper { background:#1c2330!important; color:#e6edf3!important; border:1px solid #30363d!important; border-radius:12px!important; font-family:-apple-system,sans-serif!important; }
.leaflet-popup-tip { background:#1c2330!important; }

/* Circulo de radio */
.leaflet-interactive.radio-circle { pointer-events:none; }
</style>

</head>
<body>

<!-- PERMISO -->

<div class="perm" id="pantallaPerm">
  <div class="perm-icon">🍽️</div>
  <div class="perm-title">Safe<span>Plate</span></div>
  <div class="perm-desc">Tu app para comer sin TACC y vegano en Argentina.</div>
  <div class="perm-features">
    <div class="perm-feat">🌾 <div><b>Sin TACC / Celiacos</b><br>Lugares con opciones libres de gluten</div></div>
    <div class="perm-feat">🌿 <div><b>Vegano y Vegetariano</b><br>Restaurants y cafeterias plant-based</div></div>
    <div class="perm-feat">📍 <div><b>10 km a la redonda</b><br>Busca en tiempo real donde estas</div></div>
  </div>
  <div class="perm-error" id="errorPerm"></div>
  <button id="btnUbicacion">📍 Usar mi ubicacion</button>
  <div class="perm-note">Solo usamos tu GPS para buscar. No guardamos nada.</div>
</div>

<!-- LOADING -->

<div class="loading" id="pantallaLoading">
  <div style="font-size:1.4rem;font-weight:800;">Safe<span style="color:#39d353">Plate</span></div>
  <div class="spinner"></div>
  <div class="load-txt" id="textoLoading">Obteniendo ubicacion...</div>
  <div class="load-sub" id="subtextoLoading"></div>
</div>

<!-- APP -->

<div class="topbar">
  <div class="logo">Safe<span>Plate</span></div>
  <div class="searchbox">
    <span style="color:#8b949e;flex-shrink:0">🔍</span>
    <input type="text" id="inputBuscar" placeholder="Buscar restaurante...">
  </div>
</div>

<div class="filters" id="filtros">
  <div class="chip ag" data-filtro="todos">Todos</div>
  <div class="chip"    data-filtro="sinTacc">Sin TACC</div>
  <div class="chip"    data-filtro="vegan">Vegano</div>
  <div class="chip"    data-filtro="ambos">TACC + Vegano</div>
  <div class="chip"    data-filtro="cafe">Cafeterias</div>
</div>

<div id="map"></div>

<div class="sheet" id="sheet">
  <div class="sheet-top" id="sheetTop">
    <div>
      <div class="handle"></div>
      <div class="sheet-label">Lugares cercanos <span class="radio-badge">10 km</span></div>
      <div class="sheet-sub"><b id="contador">0</b> resultados encontrados</div>
    </div>
    <div class="toggle-btn" id="toggleBtn">Ver lista</div>
  </div>
  <div class="cards" id="listaCards">
    <div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:20px;">
      Acepta el permiso de ubicacion para comenzar
    </div>
  </div>
</div>

<!-- DETALLE -->

<div class="detail" id="pantallaDetalle">
  <div class="det-head">
    <button class="back-btn" id="btnVolver">← Volver</button>
    <span style="font-size:0.82rem;color:#8b949e;font-weight:600;">Detalle del lugar</span>
  </div>
  <div class="det-body">
    <div class="det-name" id="dNombre"></div>
    <div class="det-sub"  id="dTipo"></div>
    <div class="det-tags" id="dTags"></div>
    <div class="det-sec">Informacion</div>
    <div id="dInfo"></div>
  </div>
  <div class="det-foot">
    <button class="btn-p" id="btnNavegar">Como llegar</button>
    <button class="btn-s" id="btnOSM">Ver en OSM</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
var mapa, uLat, uLng, circuloRadio;
var todos = [], visibles = [], marcadores = {};
var selId = null, filtro = 'todos', sheetAbierto = false;

// Servidores Overpass en orden de prioridad (fallback automatico)
var OVERPASS_SERVERS = [
  'https://overpass-api.de/api/interpreter',
  'https://overpass.kumi.systems/api/interpreter',
  'https://maps.mail.ru/osm/tools/overpass/api/interpreter'
];

// ── INIT ──────────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', function() {
  document.getElementById('btnUbicacion').addEventListener('click', pedirUbicacion);
  document.getElementById('inputBuscar').addEventListener('input', aplicarFiltros);
  document.getElementById('filtros').addEventListener('click', function(e) {
    var chip = e.target.closest('[data-filtro]');
    if (chip) setFiltro(chip.getAttribute('data-filtro'));
  });
  document.getElementById('sheetTop').addEventListener('click', toggleSheet);
  document.getElementById('toggleBtn').addEventListener('click', function(e) { e.stopPropagation(); toggleSheet(); });
  document.getElementById('btnVolver').addEventListener('click', cerrarDetalle);
});

// ── UBICACION ─────────────────────────────────────────────────────────────
function pedirUbicacion() {
  if (!navigator.geolocation) { mostrarError('Tu navegador no soporta GPS. Usa Chrome o Safari.'); return; }
  document.getElementById('pantallaPerm').style.display = 'none';
  document.getElementById('pantallaLoading').style.display = 'flex';
  setTexto('Obteniendo tu ubicacion...');

  navigator.geolocation.getCurrentPosition(
    function(pos) {
      uLat = pos.coords.latitude;
      uLng = pos.coords.longitude;
      setTexto('Iniciando mapa...');
      iniciarMapa();
      setTexto('Buscando lugares en 10 km...');
      setSubtexto('Conectando con OpenStreetMap...');
      buscarConFallback(0);
    },
    function(err) {
      document.getElementById('pantallaLoading').style.display = 'none';
      var msg = 'No se pudo obtener la ubicacion.';
      if (err.code === 1) msg = 'Permiso denegado.\n\nVe a: Ajustes iPhone > Privacidad > Localizacion > Chrome > "Al usar la app".\n\nLuego recarga la pagina.';
      if (err.code === 2) msg = 'GPS no disponible. Conectate a WiFi o activa la ubicacion.';
      if (err.code === 3) msg = 'Tiempo agotado. Intenta de nuevo en un lugar con mejor senal.';
      mostrarError(msg);
    },
    { enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
  );
}

function mostrarError(msg) {
  var e = document.getElementById('errorPerm');
  e.textContent = msg; e.style.display = 'block';
  document.getElementById('pantallaPerm').style.display = 'flex';
  document.getElementById('btnUbicacion').textContent = 'Reintentar';
}
function setTexto(t) { document.getElementById('textoLoading').textContent = t; }
function setSubtexto(t) { document.getElementById('subtextoLoading').textContent = t; }

// ── MAPA ──────────────────────────────────────────────────────────────────
function iniciarMapa() {
  mapa = L.map('map', { zoomControl:false, attributionControl:false }).setView([uLat, uLng], 13);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom:19 }).addTo(mapa);
  L.control.zoom({ position:'bottomright' }).addTo(mapa);

  // Circulo de 10km
  circuloRadio = L.circle([uLat, uLng], {
    radius: 10000,
    color: '#39d353', weight: 1.5,
    fillColor: '#39d353', fillOpacity: 0.04,
    dashArray: '6,6', className:'radio-circle'
  }).addTo(mapa);

  // Tu ubicacion
  var iconoYo = L.divIcon({
    className:'',
    html:'<div style="width:22px;height:22px;background:#7ab8f5;border:3px solid white;border-radius:50%;box-shadow:0 0 0 8px rgba(122,184,245,0.2)"></div>',
    iconSize:[22,22], iconAnchor:[11,11]
  });
  L.marker([uLat, uLng], { icon:iconoYo, zIndexOffset:9999 })
    .addTo(mapa).bindPopup('<b style="color:#7ab8f5">Tu ubicacion</b>');

  mapa.on('click', function() { selId=null; renderMarcadores(); });
}

// ── OVERPASS CON FALLBACK ─────────────────────────────────────────────────
function buscarConFallback(intento) {
  if (intento >= OVERPASS_SERVERS.length) {
    // Todos los servidores fallaron
    document.getElementById('pantallaLoading').style.display = 'none';
    document.getElementById('listaCards').innerHTML =
      '<div style="text-align:center;color:#f08080;font-size:0.83rem;padding:24px;line-height:1.7;">'
      + 'No se pudo conectar con OpenStreetMap.<br>'
      + 'Verifica que tengas internet activo<br>y vuelve a intentarlo.'
      + '<br><br><button onclick="location.reload()" style="background:#39d353;color:#000;border:none;padding:10px 20px;border-radius:50px;font-weight:700;font-size:0.85rem;cursor:pointer;">Reintentar</button>'
      + '</div>';
    toast('Sin respuesta de servidores. Reintenta.', true);
    return;
  }

  var servidor = OVERPASS_SERVERS[intento];
  setSubtexto('Servidor ' + (intento+1) + ' de ' + OVERPASS_SERVERS.length + '...');

  // Radio 10km — query amplia para Argentina
  // Busca: restaurantes, cafes, panaderias, dietéticas, tiendas naturistas
  // + específicamente los que tienen tags de dieta
  var r = 10000;
  var q = '[out:json][timeout:60];('

    // Lugares con gluten_free o vegan explícito (máxima prioridad)
    + 'node["diet:gluten_free"](around:'+r+','+uLat+','+uLng+');'
    + 'node["diet:vegan"](around:'+r+','+uLat+','+uLng+');'
    + 'node["diet:vegetarian"](around:'+r+','+uLat+','+uLng+');'
    + 'way["diet:gluten_free"](around:'+r+','+uLat+','+uLng+');'
    + 'way["diet:vegan"](around:'+r+','+uLat+','+uLng+');'

    // Todos los restaurantes y cafeterias del area
    + 'node["amenity"="restaurant"](around:'+r+','+uLat+','+uLng+');'
    + 'node["amenity"="cafe"](around:'+r+','+uLat+','+uLng+');'
    + 'node["amenity"="fast_food"](around:'+r+','+uLat+','+uLng+');'
    + 'node["shop"="bakery"](around:'+r+','+uLat+','+uLng+');'
    + 'node["shop"="health_food"](around:'+r+','+uLat+','+uLng+');'
    + 'node["shop"="organic"](around:'+r+','+uLat+','+uLng+');'
    + 'node["shop"="dietetic"](around:'+r+','+uLat+','+uLng+');'

    // Ways (polígonos, locales más grandes)
    + 'way["amenity"="restaurant"](around:'+r+','+uLat+','+uLng+');'
    + 'way["amenity"="cafe"](around:'+r+','+uLat+','+uLng+');'
    + 'way["amenity"="fast_food"](around:'+r+','+uLat+','+uLng+');'
    + 'way["shop"="bakery"](around:'+r+','+uLat+','+uLng+');'
    + 'way["shop"="health_food"](around:'+r+','+uLat+','+uLng+');'
    + 'way["shop"="organic"](around:'+r+','+uLat+','+uLng+');'

    + ');out center tags;';

  var controller = new AbortController();
  var timeout = setTimeout(function() { controller.abort(); }, 20000);

  fetch(servidor, {
    method: 'POST',
    body: 'data=' + encodeURIComponent(q),
    signal: controller.signal
  })
  .then(function(res) {
    clearTimeout(timeout);
    if (!res.ok) throw new Error('HTTP ' + res.status);
    return res.json();
  })
  .then(function(data) {
    if (!data.elements) throw new Error('Sin datos');
    procesarLugares(data.elements);
  })
  .catch(function(err) {
    clearTimeout(timeout);
    // Intenta con el siguiente servidor
    setSubtexto('Servidor ' + (intento+1) + ' no responde, probando otro...');
    setTimeout(function() { buscarConFallback(intento + 1); }, 800);
  });
}

// ── PROCESAR ──────────────────────────────────────────────────────────────
function procesarLugares(els) {
  var vistos = {};
  todos = [];

  for (var i = 0; i < els.length; i++) {
    var e = els[i];
    if (!e.tags || !e.tags.name) continue;
    if (vistos[e.id]) continue;
    vistos[e.id] = true;

    var t = e.tags;
    var lat = e.lat || (e.center && e.center.lat);
    var lng = e.lon || (e.center && e.center.lon);
    if (!lat || !lng) continue;

    // Deteccion de dieta
    var gf = t['diet:gluten_free']==='yes' || t['diet:gluten_free']==='only';
    var ve = t['diet:vegan']==='yes' || t['diet:vegan']==='only';
    var vg = t['diet:vegetarian']==='yes' || t['diet:vegetarian']==='only';

    // Deteccion por tipo de local (organico, dietetica = probablemente sin tacc)
    var am = t.amenity || t.shop || '';
    var esDietetica = am==='health_food' || am==='organic' || am==='dietetic';
    if (esDietetica && !gf) gf = true; // dieteticas se marcan como sin tacc probable

    var cat = 'none';
    if (gf && ve) cat='ambos';
    else if (gf)  cat='celiac';
    else if (ve)  cat='vegan';
    else if (vg)  cat='vegetarian';

    var tipo = 'Restaurante';
    if (am==='cafe')          tipo='Cafeteria';
    else if (am==='bakery')   tipo='Panaderia';
    else if (am==='fast_food') tipo='Fast Food';
    else if (am==='health_food'||am==='organic'||am==='dietetic') tipo='Dietetica / Natural';

    var dist = calcDist(uLat, uLng, lat, lng);

    // Info detallada
    var info = [];
    if (t['diet:gluten_free']==='only')    info.push({ ic:'🌾', txt:'100% Sin TACC (certificado en OSM)' });
    else if (t['diet:gluten_free']==='yes') info.push({ ic:'🌾', txt:'Opciones sin TACC disponibles' });
    if (t['diet:vegan']==='only')          info.push({ ic:'🌿', txt:'100% Vegano' });
    else if (t['diet:vegan']==='yes')      info.push({ ic:'🌿', txt:'Opciones veganas disponibles' });
    if (vg) info.push({ ic:'🥗', txt:'Opciones vegetarianas' });
    if (esDietetica) info.push({ ic:'💚', txt:'Dietetica / tienda naturista' });
    if (t.cuisine)        info.push({ ic:'🍴', txt:'Cocina: '+t.cuisine.replace(/_/g,' ') });
    if (t.opening_hours)  info.push({ ic:'🕐', txt:'Horario: '+t.opening_hours });
    if (t['addr:street'])  info.push({ ic:'📌', txt:t['addr:street']+(t['addr:housenumber']?' '+t['addr:housenumber']:'')+(t['addr:city']?', '+t['addr:city']:'') });
    if (t.phone)   info.push({ ic:'📞', txt:t.phone });
    if (t.website) info.push({ ic:'🌐', txt:t.website.replace(/^https?:\/\//,'').slice(0,40) });
    if (t['contact:instagram']) info.push({ ic:'📷', txt:'@'+t['contact:instagram'].replace(/^@/,'') });
    if (!info.length) info.push({ ic:'ℹ️', txt:'Sin informacion adicional en OpenStreetMap.' });

    todos.push({
      id:e.id, nombre:t.name, tipo:tipo, cat:cat,
      lat:lat, lng:lng, dist:dist, info:info,
      osmId:e.id, osmTipo:e.type||'node',
      esDietetica:esDietetica
    });
  }

  todos.sort(function(a, b) {
    // Primero los que tienen datos dieteticos, luego por distancia
    var scoreA = (a.cat!=='none'?0:1)*100000 + a.dist;
    var scoreB = (b.cat!=='none'?0:1)*100000 + b.dist;
    return scoreA - scoreB;
  });

  document.getElementById('pantallaLoading').style.display = 'none';
  aplicarFiltros();

  var conDieta = todos.filter(function(p){ return p.cat!=='none'; }).length;
  toast('✅ '+todos.length+' lugares · '+conDieta+' con info dietetica');
}

function calcDist(a,b,c,d){
  var R=6371000,dA=(c-a)*Math.PI/180,dB=(d-b)*Math.PI/180;
  var x=Math.sin(dA/2)*Math.sin(dA/2)+Math.cos(a*Math.PI/180)*Math.cos(c*Math.PI/180)*Math.sin(dB/2)*Math.sin(dB/2);
  return Math.round(R*2*Math.atan2(Math.sqrt(x),Math.sqrt(1-x)));
}
function fmt(m){ return m<1000?m+'m':(m/1000).toFixed(1)+'km'; }

// ── FILTROS ───────────────────────────────────────────────────────────────
function setFiltro(f) {
  filtro = f;
  document.querySelectorAll('.chip').forEach(function(c){ c.className='chip'; });
  var cl = f==='vegan'?'at':f==='ambos'?'aa':'ag';
  var chip = document.querySelector('[data-filtro="'+f+'"]');
  if (chip) chip.classList.add(cl);
  aplicarFiltros();
}

function aplicarFiltros() {
  var q = document.getElementById('inputBuscar').value.toLowerCase();
  visibles = todos.filter(function(p) {
    var mf = filtro==='todos'    ? true
           : filtro==='sinTacc'  ? (p.cat==='celiac'||p.cat==='ambos')
           : filtro==='vegan'    ? (p.cat==='vegan'||p.cat==='ambos')
           : filtro==='ambos'    ? p.cat==='ambos'
           : filtro==='cafe'     ? p.tipo==='Cafeteria'
           : true;
    return mf && (q ? p.nombre.toLowerCase().indexOf(q)>=0 : true);
  });
  document.getElementById('contador').textContent = visibles.length;
  renderLista();
  if (mapa) renderMarcadores();
}

// ── HELPERS ───────────────────────────────────────────────────────────────
function emo(t){
  if(t==='Cafeteria') return '☕';
  if(t==='Panaderia') return '🥖';
  if(t==='Fast Food') return '🍔';
  if(t==='Dietetica / Natural') return '💚';
  return '🍽️';
}
function col(c){
  if(c==='celiac') return '#39d353';
  if(c==='vegan')  return '#00c9a7';
  if(c==='ambos')  return '#f0a500';
  return '#8b949e';
}

// ── LISTA ─────────────────────────────────────────────────────────────────
function renderLista() {
  var el = document.getElementById('listaCards');
  if (!visibles.length) {
    var msg = filtro!=='todos'
      ? 'Sin resultados para este filtro en 10 km.<br>Proba con "Todos".'
      : 'Sin lugares encontrados en 10 km.';
    el.innerHTML = '<div style="text-align:center;color:#8b949e;font-size:0.83rem;padding:24px;line-height:1.6;">'+msg+'</div>';
    return;
  }
  el.innerHTML = '';
  var max = Math.min(visibles.length, 80);
  for (var i = 0; i < max; i++) {
    (function(p) {
      var div = document.createElement('div');
      div.className = 'card'+(p.cat==='vegan'?' cv':p.cat==='ambos'?' ca':p.cat==='none'?' cn':'')+(selId===p.id?' sel':'');
      div.id = 'card'+p.id;
      div.innerHTML =
        '<div class="card-top">'
          +'<div class="card-name">'+p.nombre+'</div>'
          +'<div class="card-dist">'+fmt(p.dist)+'</div>'
        +'</div>'
        +'<div class="card-type">'+emo(p.tipo)+' '+p.tipo+'</div>'
        +'<div class="tags">'
          +(p.cat==='celiac'||p.cat==='ambos'?'<span class="tag tg">Sin TACC</span>':'')
          +(p.cat==='vegan' ||p.cat==='ambos'?'<span class="tag tv">Vegano</span>':'')
          +(p.cat==='vegetarian'?'<span class="tag tvg">Vegetariano</span>':'')
          +(p.esDietetica?'<span class="tag tt">Dietetica</span>':'')
          +(p.cat==='none'&&!p.esDietetica?'<span class="tag tn">Sin datos</span>':'')
        +'</div>';

      // Tap vs scroll detection
      var ty0=0, tx0=0;
      div.addEventListener('touchstart', function(e){ ty0=e.touches[0].clientY; tx0=e.touches[0].clientX; }, {passive:true});
      div.addEventListener('touchend', function(e){
        var dy=Math.abs(e.changedTouches[0].clientY-ty0);
        var dx=Math.abs(e.changedTouches[0].clientX-tx0);
        if(dy<10&&dx<10){ e.preventDefault(); abrirDetalle(p); }
      });
      div.addEventListener('click', function(){ abrirDetalle(p); });

      el.appendChild(div);
    })(visibles[i]);
  }
}

// ── MARCADORES ────────────────────────────────────────────────────────────
function renderMarcadores() {
  var ids = Object.keys(marcadores);
  for(var i=0;i<ids.length;i++) mapa.removeLayer(marcadores[ids[i]]);
  marcadores={};
  var vs={};
  for(var j=0;j<visibles.length;j++) vs[visibles[j].id]=true;

  for(var k=0;k<todos.length;k++){
    (function(p){
      var esV=!!vs[p.id], esSel=selId===p.id, c=col(p.cat), sz=esSel?48:36;
      var icon=L.divIcon({className:'',
        html:'<div style="width:'+sz+'px;height:'+sz+'px;border-radius:50%;'
          +'background:'+c+'22;border:2.5px solid '+c+';'
          +'display:flex;align-items:center;justify-content:center;'
          +'font-size:'+(esSel?19:14)+'px;opacity:'+(esV?1:0.12)+';'
          +'box-shadow:'+(esSel?'0 0 0 5px '+c+'33,':'')+'0 3px 12px rgba(0,0,0,0.6);'
          +'cursor:pointer;">'+emo(p.tipo)+'</div>',
        iconSize:[sz,sz],iconAnchor:[sz/2,sz/2]
      });
      var mk=L.marker([p.lat,p.lng],{icon:icon,zIndexOffset:esSel?500:0}).addTo(mapa);
      if(esV) mk.on('click',function(e){e.originalEvent.stopPropagation();abrirDetalle(p);});
      marcadores[p.id]=mk;
    })(todos[k]);
  }
}

// ── DETALLE ───────────────────────────────────────────────────────────────
function abrirDetalle(p) {
  selId=p.id;
  document.querySelectorAll('.card').forEach(function(c){c.classList.remove('sel');});
  var card=document.getElementById('card'+p.id);
  if(card){card.classList.add('sel');card.scrollIntoView({behavior:'smooth',block:'nearest'});}
  if(mapa) mapa.flyTo([p.lat,p.lng],17,{animate:true,duration:0.6});
  renderMarcadores();

  document.getElementById('dNombre').textContent=p.nombre;
  document.getElementById('dTipo').textContent=emo(p.tipo)+' '+p.tipo+'  ·  '+fmt(p.dist)+' de vos';

  var bt=document.getElementById('dTags'); bt.innerHTML='';
  if(p.cat==='celiac'||p.cat==='ambos') bt.innerHTML+='<span class="tag tg" style="font-size:0.78rem;padding:4px 12px;">🌾 Sin TACC</span>';
  if(p.cat==='vegan' ||p.cat==='ambos') bt.innerHTML+='<span class="tag tv" style="font-size:0.78rem;padding:4px 12px;">🌿 Vegano</span>';
  if(p.cat==='vegetarian') bt.innerHTML+='<span class="tag tvg" style="font-size:0.78rem;padding:4px 12px;">🥗 Vegetariano</span>';
  if(p.esDietetica) bt.innerHTML+='<span class="tag tt" style="font-size:0.78rem;padding:4px 12px;">💚 Dietetica</span>';
  if(p.cat==='none'&&!p.esDietetica) bt.innerHTML+='<span class="tag tn" style="font-size:0.78rem;padding:4px 12px;">Sin datos dieteticos en OSM</span>';

  var di=document.getElementById('dInfo'); di.innerHTML='';
  for(var i=0;i<p.info.length;i++){
    var d=document.createElement('div'); d.className='det-line';
    d.innerHTML='<span>'+p.info[i].ic+'</span><span>'+p.info[i].txt+'</span>';
    di.appendChild(d);
  }

  document.getElementById('btnNavegar').onclick=function(){
    window.open('https://www.google.com/maps/dir/?api=1&destination='+p.lat+','+p.lng,'_blank');
  };
  document.getElementById('btnOSM').onclick=function(){
    window.open('https://www.openstreetmap.org/'+p.osmTipo+'/'+p.osmId,'_blank');
  };
  document.getElementById('pantallaDetalle').classList.add('open');
}

function cerrarDetalle(){
  document.getElementById('pantallaDetalle').classList.remove('open');
  selId=null; renderMarcadores();
}

function toggleSheet(){
  sheetAbierto=!sheetAbierto;
  document.getElementById('sheet').classList.toggle('big',sheetAbierto);
  document.getElementById('toggleBtn').textContent=sheetAbierto?'Minimizar':'Ver lista';
  document.getElementById('toast').style.bottom=sheetAbierto?'calc(65vh + 12px)':'calc(36vh + 12px)';
}

function toast(msg, esError){
  var t=document.getElementById('toast');
  t.textContent=msg;
  t.className='toast on'+(esError?' err':'');
  setTimeout(function(){t.className='toast'+(esError?' err':'');},4000);
}
</script>

</body>
</html>
