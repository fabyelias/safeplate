<!DOCTYPE html>

<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>SafePlate — Come sin miedo</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
:root {
  --bg:#0d1117; --surface:#161b22; --surface2:#1c2330;
  --border:#30363d; --green:#39d353; --green-soft:#1a4a2e;
  --amber:#f0a500; --amber-soft:#3d2c00;
  --teal:#00c9a7; --teal-soft:#003d32;
  --text:#e6edf3; --muted:#8b949e; --r:14px;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);
  height:100dvh;display:flex;flex-direction:column;overflow:hidden;}

/* TOP */
.topbar{display:flex;align-items:center;gap:10px;padding:11px 16px;
background:var(–surface);border-bottom:1px solid var(–border);z-index:1000;}
.logo{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.25rem;white-space:nowrap;}
.logo span{color:var(–green);}
.search-wrap{flex:1;display:flex;align-items:center;background:var(–surface2);
border:1px solid var(–border);border-radius:50px;padding:7px 14px;gap:6px;max-width:340px;}
.search-wrap input{background:none;border:none;outline:none;color:var(–text);
font-family:‘DM Sans’,sans-serif;font-size:.87rem;width:100%;}
.search-wrap input::placeholder{color:var(–muted);}
.avatar{width:33px;height:33px;border-radius:50%;
background:linear-gradient(135deg,var(–green),var(–teal));
display:flex;align-items:center;justify-content:center;
font-family:‘Syne’,sans-serif;font-weight:700;font-size:.78rem;flex-shrink:0;}

/* FILTERS */
.filters{display:flex;gap:7px;padding:8px 14px;background:var(–surface);
border-bottom:1px solid var(–border);overflow-x:auto;scrollbar-width:none;}
.filters::-webkit-scrollbar{display:none;}
.chip{display:flex;align-items:center;gap:5px;padding:5px 13px;border-radius:50px;
border:1.5px solid var(–border);background:var(–surface2);color:var(–muted);
font-size:.79rem;font-weight:500;cursor:pointer;white-space:nowrap;flex-shrink:0;
transition:all .17s;user-select:none;}
.chip:hover{border-color:var(–green);color:var(–text);}
.chip.on-g{background:var(–green-soft);border-color:var(–green);color:var(–green);}
.chip.on-t{background:var(–teal-soft);border-color:var(–teal);color:var(–teal);}
.chip.on-a{background:var(–amber-soft);border-color:var(–amber);color:var(–amber);}

/* MAIN */
.main{display:flex;flex:1;overflow:hidden;}
#map{flex:1;height:100%;z-index:1;}

/* SIDEBAR */
.sidebar{width:320px;background:var(–surface);border-left:1px solid var(–border);
display:flex;flex-direction:column;overflow:hidden;z-index:100;}
.sb-head{padding:13px 16px 9px;border-bottom:1px solid var(–border);}
.sb-title{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.92rem;}
.sb-count{font-size:.76rem;color:var(–muted);margin-top:2px;}
.sb-count b{color:var(–green);}
.list{flex:1;overflow-y:auto;padding:9px;display:flex;flex-direction:column;gap:7px;
scrollbar-width:thin;scrollbar-color:var(–border) transparent;}

/* CARD */
.card{background:var(–surface2);border:1px solid var(–border);border-radius:var(–r);
padding:11px 13px;cursor:pointer;transition:all .17s;border-left:3px solid var(–green);}
.card.v{border-left-color:var(–teal);}
.card.a{border-left-color:var(–amber);}
.card:hover{transform:translateX(2px);box-shadow:0 4px 16px rgba(0,0,0,.4);}
.card.sel{background:rgba(57,211,83,.07);border-color:var(–green);}
.c-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:4px;}
.c-name{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.88rem;line-height:1.2;}
.c-dist{font-size:.7rem;color:var(–muted);white-space:nowrap;margin-left:6px;}
.c-type{font-size:.75rem;color:var(–muted);margin-bottom:5px;}
.c-badges{display:flex;flex-wrap:wrap;gap:4px;}
.badge{font-size:.65rem;font-weight:600;padding:2px 7px;border-radius:50px;}
.bg{background:var(–green-soft);color:var(–green);border:1px solid rgba(57,211,83,.25);}
.bt{background:var(–teal-soft);color:var(–teal);border:1px solid rgba(0,201,167,.25);}
.ba{background:var(–amber-soft);color:var(–amber);border:1px solid rgba(240,165,0,.25);}
.bc{background:#1e2d3d;color:#7ab8f5;border:1px solid rgba(122,184,245,.25);}

/* PANEL */
.panel{position:fixed;bottom:-440px;left:0;right:320px;
background:var(–surface);border-top:1px solid var(–border);
border-radius:16px 16px 0 0;padding:16px 20px 20px;z-index:300;
transition:bottom .32s cubic-bezier(.34,1.56,.64,1);
box-shadow:0 -8px 40px rgba(0,0,0,.6);}
.panel.open{bottom:0;}
.handle{width:36px;height:4px;background:var(–border);border-radius:4px;
margin:0 auto 13px;cursor:pointer;}
.p-close{position:absolute;top:13px;right:16px;background:var(–surface2);
border:1px solid var(–border);width:27px;height:27px;border-radius:50%;
cursor:pointer;color:var(–muted);font-size:.82rem;
display:flex;align-items:center;justify-content:center;transition:all .15s;}
.p-close:hover{color:var(–text);}
.p-name{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.28rem;margin-bottom:3px;}
.p-meta{font-size:.8rem;color:var(–muted);margin-bottom:9px;display:flex;flex-wrap:wrap;gap:7px;}
.p-badges{display:flex;gap:5px;flex-wrap:wrap;margin-bottom:11px;}
.p-sec{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.7rem;color:var(–muted);
text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;}
.p-detail{font-size:.82rem;color:var(–muted);margin-bottom:12px;line-height:1.5;}
.p-btns{display:flex;gap:8px;}
.btn{flex:1;padding:10px;border-radius:50px;border:none;
font-family:‘Syne’,sans-serif;font-weight:700;font-size:.84rem;
cursor:pointer;transition:all .15s;}
.btn-p{background:var(–green);color:#000;}
.btn-p:hover{background:#2ecc4a;}
.btn-s{background:var(–surface2);color:var(–text);border:1.5px solid var(–border);}
.btn-s:hover{border-color:var(–green);}

/* PERMISSION SCREEN */
.perm-screen{position:fixed;inset:0;background:var(–bg);z-index:9000;
display:flex;flex-direction:column;align-items:center;justify-content:center;
gap:20px;padding:32px;text-align:center;}
.perm-icon{font-size:4rem;margin-bottom:8px;}
.perm-title{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.6rem;}
.perm-title span{color:var(–green);}
.perm-sub{font-size:.92rem;color:var(–muted);max-width:320px;line-height:1.6;}
.perm-btn{padding:14px 36px;background:var(–green);color:#000;border:none;
border-radius:50px;font-family:‘Syne’,sans-serif;font-weight:700;
font-size:1rem;cursor:pointer;transition:all .2s;}
.perm-btn:hover{background:#2ecc4a;transform:translateY(-2px);}
.perm-note{font-size:.75rem;color:var(–muted);max-width:280px;}

/* LOADING */
.loading-screen{position:fixed;inset:0;background:var(–bg);z-index:8000;
display:flex;flex-direction:column;align-items:center;justify-content:center;gap:16px;}
.loading-screen.hidden{display:none;}
.spinner{width:44px;height:44px;border:3px solid var(–border);
border-top-color:var(–green);border-radius:50%;animation:spin .8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg);}}
.loading-txt{font-family:‘Syne’,sans-serif;font-size:.88rem;color:var(–muted);}

/* TOAST */
.toast{position:fixed;top:75px;left:50%;transform:translateX(-50%) translateY(-16px);
background:var(–surface);border:1px solid var(–green);border-radius:50px;
padding:7px 16px;font-size:.8rem;color:var(–green);
opacity:0;pointer-events:none;transition:all .28s;z-index:9999;}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}

/* ERROR */
.error-banner{background:rgba(240,80,80,.15);border:1px solid rgba(240,80,80,.3);
border-radius:10px;padding:10px 14px;font-size:.82rem;color:#f08080;
margin:8px;display:none;}

/* Leaflet overrides */
.leaflet-tile{filter:brightness(.75) saturate(.6);}
.leaflet-control-zoom a{background:var(–surface)!important;color:var(–text)!important;
border-color:var(–border)!important;}
.leaflet-popup-content-wrapper{background:var(–surface2)!important;color:var(–text)!important;
border:1px solid var(–border)!important;border-radius:12px!important;}
.leaflet-popup-tip{background:var(–surface2)!important;}

@media(max-width:680px){
.sidebar{display:none;}
.panel{right:0;}
}
</style>

</head>
<body>

<!-- PERMISSION SCREEN -->

<div class="perm-screen" id="permScreen">
  <div class="perm-icon">📍</div>
  <div class="perm-title">Safe<span>Plate</span></div>
  <div class="perm-sub">Para mostrarte restaurantes y cafeterías veganas y sin gluten cerca tuyo, necesitamos acceder a tu ubicación.</div>
  <button class="perm-btn" onclick="requestLocation()">Usar mi ubicación 🌿</button>
  <div class="perm-note">Solo usamos tu ubicación para buscar lugares cercanos. No la guardamos.</div>
</div>

<!-- LOADING -->

<div class="loading-screen" id="loadingScreen">
  <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:1.5rem;">Safe<span style="color:var(--green)">Plate</span></div>
  <div class="spinner"></div>
  <div class="loading-txt" id="loadingTxt">Obteniendo tu ubicación…</div>
</div>

<!-- APP -->

<div class="topbar">
  <div class="logo">Safe<span>Plate</span></div>
  <div class="search-wrap">
    <span style="color:var(--muted)">🔍</span>
    <input type="text" id="searchInput" placeholder="Buscar restaurante…" oninput="applyFilters()">
  </div>
  <div class="avatar">YO</div>
</div>

<div class="filters">
  <div class="chip on-g" id="chip-todos"  onclick="setFilter('todos')">✦ Todos</div>
  <div class="chip"      id="chip-celiac" onclick="setFilter('celiac')">🌾 Sin Gluten</div>
  <div class="chip"      id="chip-vegan"  onclick="setFilter('vegan')">🌿 Vegano</div>
  <div class="chip"      id="chip-ambos"  onclick="setFilter('ambos')">✨ SG + Vegano</div>
  <div class="chip"      id="chip-cafe"   onclick="setFilter('cafe')">☕ Cafeterías</div>
  <div class="chip"      id="chip-open"   onclick="setFilter('open')">🟢 Abierto</div>
</div>

<div class="main">
  <div id="map"></div>
  <div class="sidebar">
    <div class="sb-head">
      <div class="sb-title">Lugares cercanos</div>
      <div class="sb-count"><b id="cnt">0</b> resultados</div>
    </div>
    <div id="errBanner" class="error-banner"></div>
    <div class="list" id="list"><div style="padding:20px;text-align:center;color:var(--muted);font-size:.85rem;">Buscando lugares…</div></div>
  </div>
</div>

<!-- BOTTOM PANEL -->

<div class="panel" id="panel">
  <div class="handle" onclick="closePanel()"></div>
  <div class="p-close" onclick="closePanel()">✕</div>
  <div class="p-name"   id="pName"></div>
  <div class="p-meta"   id="pMeta"></div>
  <div class="p-badges" id="pBadges"></div>
  <div class="p-sec">Información</div>
  <div class="p-detail" id="pDetail"></div>
  <div class="p-btns">
    <button class="btn btn-p" id="btnNav">🧭 Cómo llegar</button>
    <button class="btn btn-s" id="btnOsm">🔗 Ver en mapa</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
let map, userLat, userLng;
let allPlaces = [];
let visible   = [];
let filter    = 'todos';
let markers   = {};
let selected  = null;

// ── 1. REQUEST LOCATION ───────────────────────────────────────────────────────
function requestLocation() {
  document.getElementById('permScreen').style.display = 'none';
  document.getElementById('loadingScreen').classList.remove('hidden');

  if (!navigator.geolocation) {
    showError('Tu navegador no soporta geolocalización.');
    return;
  }
  navigator.geolocation.getCurrentPosition(
    pos => {
      userLat = pos.coords.latitude;
      userLng = pos.coords.longitude;
      setLoadingTxt('Iniciando mapa…');
      initMap();
      setLoadingTxt('Buscando restaurantes cercanos…');
      fetchPlaces();
    },
    err => {
      let msg = 'No se pudo obtener la ubicación.';
      if (err.code === 1) msg = 'Permiso denegado. Activa la ubicación en tu navegador.';
      if (err.code === 2) msg = 'Ubicación no disponible. Verifica tu GPS.';
      if (err.code === 3) msg = 'Tiempo de espera agotado. Intenta de nuevo.';
      showError(msg);
    },
    { enableHighAccuracy: true, timeout: 12000, maximumAge: 0 }
  );
}

function setLoadingTxt(t) { document.getElementById('loadingTxt').textContent = t; }

// ── 2. INIT MAP ───────────────────────────────────────────────────────────────
function initMap() {
  map = L.map('map', { zoomControl: false }).setView([userLat, userLng], 15);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution:'© OpenStreetMap', maxZoom:19
  }).addTo(map);
  L.control.zoom({ position:'bottomright' }).addTo(map);

  // User location marker
  const userIcon = L.divIcon({
    className:'',
    html:`<div style="width:18px;height:18px;background:#7ab8f5;border:3px solid #fff;
      border-radius:50%;box-shadow:0 0 0 6px rgba(122,184,245,.25)"></div>`,
    iconSize:[18,18], iconAnchor:[9,9]
  });
  L.marker([userLat, userLng], {icon: userIcon})
    .addTo(map)
    .bindPopup('<b style="color:#7ab8f5">📍 Tú estás aquí</b>');

  map.on('click', () => closePanel());
}

// ── 3. FETCH FROM OVERPASS ────────────────────────────────────────────────────
async function fetchPlaces() {
  const radius = 1500; // metros
  // Query: restaurantes, cafeterías, panaderías, tiendas de comida sana
  const query = `
[out:json][timeout:25];
(
  node["amenity"="restaurant"](around:${radius},${userLat},${userLng});
  node["amenity"="cafe"](around:${radius},${userLat},${userLng});
  node["amenity"="fast_food"](around:${radius},${userLat},${userLng});
  node["shop"="bakery"](around:${radius},${userLat},${userLng});
  node["shop"="health_food"](around:${radius},${userLat},${userLng});
  way["amenity"="restaurant"](around:${radius},${userLat},${userLng});
  way["amenity"="cafe"](around:${radius},${userLat},${userLng});
  way["amenity"="fast_food"](around:${radius},${userLat},${userLng});
  way["shop"="bakery"](around:${radius},${userLat},${userLng});
);
out center tags;
`;

  try {
    const res = await fetch('https://overpass-api.de/api/interpreter', {
      method:'POST',
      body: 'data=' + encodeURIComponent(query)
    });
    if (!res.ok) throw new Error('Overpass API error');
    const data = await res.json();
    processPlaces(data.elements);
  } catch(e) {
    showError('No se pudieron cargar lugares. Verifica tu conexión.');
    document.getElementById('loadingScreen').classList.add('hidden');
  }
}

// ── 4. PROCESS OSM DATA ───────────────────────────────────────────────────────
function processPlaces(elements) {
  allPlaces = elements
    .filter(el => el.tags && el.tags.name)
    .map(el => {
      const t = el.tags;
      const lat = el.lat ?? el.center?.lat;
      const lng = el.lon ?? el.center?.lon;
      if (!lat || !lng) return null;

      // Detect dietary info from OSM tags
      const isGlutenFree = t['diet:gluten_free']==='yes'||t['diet:gluten_free']==='only'
        || t['cuisine']?.includes('gluten_free');
      const isVegan = t['diet:vegan']==='yes'||t['diet:vegan']==='only';
      const isVegetarian = t['diet:vegetarian']==='yes'||t['diet:vegetarian']==='only';

      let cat = 'none';
      if (isGlutenFree && isVegan)  cat = 'ambos';
      else if (isGlutenFree)        cat = 'celiac';
      else if (isVegan)             cat = 'vegan';
      else if (isVegetarian)        cat = 'vegetarian';
      // Include all — let filter decide

      const amenity = t.amenity || t.shop || '';
      let type = 'Restaurante';
      if (amenity==='cafe')          type = 'Cafetería';
      else if (amenity==='bakery')   type = 'Panadería';
      else if (amenity==='fast_food')type = 'Fast Food';
      else if (amenity==='health_food') type = 'Tienda Saludable';

      const dist = haversine(userLat, userLng, lat, lng);

      // Build details string
      const details = [];
      if (t.cuisine)     details.push('🍴 ' + t.cuisine.replace(/_/g,' '));
      if (t['addr:street']) details.push('📌 ' + t['addr:street'] + (t['addr:housenumber']?' '+t['addr:housenumber']:''));
      if (t.phone)       details.push('📞 ' + t.phone);
      if (t.website)     details.push('🌐 ' + t.website.replace(/^https?:\/\//,'').slice(0,30));
      if (t.opening_hours) details.push('🕐 ' + t.opening_hours);
      if (t['diet:vegan']==='yes')        details.push('🌿 Opciones veganas disponibles');
      if (t['diet:gluten_free']==='yes')  details.push('🌾 Opciones sin gluten disponibles');
      if (t['diet:vegan']==='only')       details.push('🌿 100% Vegano');
      if (t['diet:gluten_free']==='only') details.push('🌾 100% Sin gluten');
      if (t['diet:vegetarian']==='yes')   details.push('🥗 Opciones vegetarianas');

      return {
        id: el.id, name: t.name, type, cat,
        lat, lng, dist,
        open: t.opening_hours ? null : null, // OSM opening_hours needs a parser
        details: details.length ? details.join(' · ') : 'Sin información adicional en OpenStreetMap.',
        osmId: el.id, osmType: el.type || 'node',
        tags: t
      };
    })
    .filter(Boolean)
    .sort((a,b) => a.dist - b.dist);

  document.getElementById('loadingScreen').classList.add('hidden');
  applyFilters();
  showToast(`✅ ${allPlaces.length} lugares encontrados cerca tuyo`);
}

// ── 5. HAVERSINE DISTANCE ─────────────────────────────────────────────────────
function haversine(lat1,lon1,lat2,lon2){
  const R=6371000, dLat=(lat2-lat1)*Math.PI/180, dLon=(lon2-lon1)*Math.PI/180;
  const a=Math.sin(dLat/2)**2+Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLon/2)**2;
  return Math.round(R*2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a)));
}
function fmtDist(m){ return m<1000 ? m+'m' : (m/1000).toFixed(1)+'km'; }

// ── 6. FILTER ─────────────────────────────────────────────────────────────────
function setFilter(f){
  filter=f;
  document.querySelectorAll('.chip').forEach(c=>c.className='chip');
  const cls=f==='vegan'?'on-t':f==='ambos'?'on-a':'on-g';
  document.getElementById('chip-'+f)?.classList.add(cls);
  applyFilters();
}

function applyFilters(){
  const q=document.getElementById('searchInput').value.toLowerCase();
  visible=allPlaces.filter(p=>{
    const mf=
      filter==='todos'  ? true :
      filter==='celiac' ? (p.cat==='celiac'||p.cat==='ambos') :
      filter==='vegan'  ? (p.cat==='vegan'||p.cat==='ambos') :
      filter==='ambos'  ? p.cat==='ambos' :
      filter==='cafe'   ? p.type==='Cafetería' :
      filter==='open'   ? true : true;
    const ms=q ? p.name.toLowerCase().includes(q)||p.details.toLowerCase().includes(q) : true;
    return mf&&ms;
  });
  document.getElementById('cnt').textContent=visible.length;
  renderList();
  renderMarkers();
}

// ── 7. MARKERS ────────────────────────────────────────────────────────────────
function getColor(cat){
  if(cat==='celiac') return '#39d353';
  if(cat==='vegan')  return '#00c9a7';
  if(cat==='ambos')  return '#f0a500';
  return '#8b949e';
}
function getEmoji(type){
  if(type==='Cafetería') return '☕';
  if(type==='Panadería') return '🥖';
  if(type==='Fast Food') return '🍔';
  if(type==='Tienda Saludable') return '🛒';
  return '🍽';
}

function renderMarkers(){
  // Remove old
  Object.values(markers).forEach(m=>map.removeLayer(m));
  markers={};

  const visIds = new Set(visible.map(p=>p.id));

  allPlaces.forEach(p=>{
    const isVis = visIds.has(p.id);
    const color = getColor(p.cat);
    const opacity = isVis ? 1 : 0.2;
    const size = selected===p.id ? 44 : 36;

    const icon = L.divIcon({
      className:'',
      html:`<div style="
        width:${size}px;height:${size}px;border-radius:50%;
        background:${color}22;border:2.5px solid ${color};
        display:flex;align-items:center;justify-content:center;
        font-size:${selected===p.id?17:13}px;
        box-shadow:0 3px 10px rgba(0,0,0,.5);
        opacity:${opacity};transition:all .2s;cursor:pointer;
      ">${getEmoji(p.type)}</div>`,
      iconSize:[size,size], iconAnchor:[size/2,size/2]
    });

    const marker = L.marker([p.lat,p.lng],{icon}).addTo(map);
    if(isVis) marker.on('click', e=>{e.originalEvent.stopPropagation();openPanel(p);});
    markers[p.id]=marker;
  });
}

// ── 8. SIDEBAR LIST ───────────────────────────────────────────────────────────
function renderList(){
  const el=document.getElementById('list');
  if(!visible.length){
    el.innerHTML=`<div style="padding:24px;text-align:center;color:var(--muted);font-size:.84rem;">
      ${filter!=='todos'?'Sin resultados para este filtro. Prueba con "Todos".':'Sin lugares encontrados cerca tuyo.'}</div>`;
    return;
  }
  el.innerHTML='';
  visible.slice(0,40).forEach(p=>{
    const cls=p.cat==='vegan'?'v':p.cat==='ambos'?'a':'';
    const isSel=selected===p.id?' sel':'';
    el.innerHTML+=`
    <div class="card ${cls}${isSel}" id="card${p.id}" onclick='openPanel(${JSON.stringify(p).replace(/'/g,"&#39;")})'>
      <div class="c-top">
        <div class="c-name">${p.name}</div>
        <div class="c-dist">${fmtDist(p.dist)}</div>
      </div>
      <div class="c-type">${getEmoji(p.type)} ${p.type}</div>
      <div class="c-badges">
        ${p.cat==='celiac'||p.cat==='ambos'?'<span class="badge bg">🌾 Sin Gluten</span>':''}
        ${p.cat==='vegan'||p.cat==='ambos'?'<span class="badge bt">🌿 Vegano</span>':''}
        ${p.cat==='vegetarian'?'<span class="badge bc">🥗 Vegetariano</span>':''}
        ${p.type==='Cafetería'?'<span class="badge bc">☕</span>':''}
      </div>
    </div>`;
  });
}

// ── 9. PANEL ──────────────────────────────────────────────────────────────────
function openPanel(p){
  if(typeof p==='string') p=JSON.parse(p);
  selected=p.id;

  document.querySelectorAll('.card').forEach(c=>c.classList.remove('sel'));
  const card=document.getElementById('card'+p.id);
  if(card){card.classList.add('sel');card.scrollIntoView({behavior:'smooth',block:'nearest'});}

  map.flyTo([p.lat,p.lng],17,{animate:true,duration:.7});

  document.getElementById('pName').textContent=p.name;
  document.getElementById('pMeta').innerHTML=
    `<span>${getEmoji(p.type)} ${p.type}</span><span>📍 ${fmtDist(p.dist)}</span>`;

  const b=document.getElementById('pBadges');
  b.innerHTML='';
  if(p.cat==='celiac'||p.cat==='ambos') b.innerHTML+='<span class="badge bg" style="font-size:.78rem;padding:3px 10px;">🌾 Sin Gluten</span>';
  if(p.cat==='vegan'||p.cat==='ambos')  b.innerHTML+='<span class="badge bt" style="font-size:.78rem;padding:3px 10px;">🌿 Vegano</span>';
  if(p.cat==='vegetarian') b.innerHTML+='<span class="badge bc" style="font-size:.78rem;padding:3px 10px;">🥗 Vegetariano</span>';

  document.getElementById('pDetail').textContent=p.details;

  // Nav button → Google Maps directions
  document.getElementById('btnNav').onclick=()=>{
    window.open(`https://www.google.com/maps/dir/?api=1&destination=${p.lat},${p.lng}`,'_blank');
  };
  // OSM link
  document.getElementById('btnOsm').onclick=()=>{
    window.open(`https://www.openstreetmap.org/${p.osmType}/${p.osmId}`,'_blank');
  };

  document.getElementById('panel').classList.add('open');
  renderMarkers();
}

function closePanel(){
  document.getElementById('panel').classList.remove('open');
  selected=null;
  document.querySelectorAll('.card').forEach(c=>c.classList.remove('sel'));
  renderMarkers();
}

// ── HELPERS ───────────────────────────────────────────────────────────────────
function showError(msg){
  document.getElementById('loadingScreen').classList.add('hidden');
  document.getElementById('permScreen').style.display='flex';
  const btn=document.querySelector('.perm-btn');
  btn.textContent='Reintentar';
  document.querySelector('.perm-sub').textContent=msg;
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),3000);
}
</script>

</body>
</html>
