<!DOCTYPE html>

<html lang="es" translate="no">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="google" content="notranslate">
<meta name="theme-color" content="#0d1117">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>SafePlate</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@700;800&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
:root{
  --bg:#0d1117;--surf:#161b22;--surf2:#1c2330;
  --bord:#30363d;--green:#39d353;--gs:#1a4a2e;
  --amber:#f0a500;--as:#3d2c00;
  --teal:#00c9a7;--ts:#003d32;
  --text:#e6edf3;--muted:#8b949e;
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{height:100%;overflow:hidden;background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;}
body{display:flex;flex-direction:column;}

.top{display:flex;align-items:center;gap:10px;padding:12px 14px;
padding-top:max(12px,env(safe-area-inset-top));
background:var(–surf);border-bottom:1px solid var(–bord);flex-shrink:0;z-index:1000;}
.logo{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.2rem;white-space:nowrap;}
.logo b{color:var(–green);}
.search{flex:1;display:flex;align-items:center;gap:6px;background:var(–surf2);
border:1px solid var(–bord);border-radius:50px;padding:7px 13px;}
.search input{background:none;border:none;outline:none;color:var(–text);
font-size:.85rem;width:100%;font-family:‘DM Sans’,sans-serif;}
.search input::placeholder{color:var(–muted);}

.chips{display:flex;gap:6px;padding:8px 12px;background:var(–surf);
border-bottom:1px solid var(–bord);overflow-x:auto;flex-shrink:0;scrollbar-width:none;}
.chips::-webkit-scrollbar{display:none;}
.chip{display:flex;align-items:center;gap:4px;padding:5px 12px;border-radius:50px;
border:1.5px solid var(–bord);background:var(–surf2);color:var(–muted);
font-size:.76rem;font-weight:500;cursor:pointer;white-space:nowrap;flex-shrink:0;transition:all .15s;}
.cg{background:var(–gs);border-color:var(–green);color:var(–green);}
.ct{background:var(–ts);border-color:var(–teal);color:var(–teal);}
.ca{background:var(–as);border-color:var(–amber);color:var(–amber);}

#map{flex:1;min-height:0;z-index:1;}

.sheet{flex-shrink:0;background:var(–surf);border-top:1px solid var(–bord);
display:flex;flex-direction:column;height:38vh;max-height:420px;
transition:height .3s ease;z-index:200;}
.sheet.exp{height:70vh;}
.sh-head{display:flex;align-items:center;justify-content:space-between;
padding:10px 14px 8px;cursor:pointer;flex-shrink:0;}
.sh-handle{width:32px;height:4px;background:var(–bord);border-radius:4px;margin:0 auto 7px;}
.sh-title{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.88rem;}
.sh-count{font-size:.72rem;color:var(–muted);}
.sh-count span{color:var(–green);font-weight:600;}
.sh-btn{font-size:.72rem;color:var(–muted);background:var(–surf2);
border:1px solid var(–bord);border-radius:50px;padding:3px 10px;}
.list{flex:1;overflow-y:auto;padding:0 10px 10px;display:flex;flex-direction:column;gap:7px;
-webkit-overflow-scrolling:touch;scrollbar-width:thin;scrollbar-color:var(–bord) transparent;}

.card{background:var(–surf2);border:1px solid var(–bord);border-radius:12px;
padding:11px 13px;cursor:pointer;transition:all .15s;border-left:3px solid var(–green);flex-shrink:0;}
.card.cv{border-left-color:var(–teal);}
.card.ca{border-left-color:var(–amber);}
.card.cn{border-left-color:var(–muted);}
.card.sel{background:rgba(57,211,83,.08);border-color:var(–green);}
.cr{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:3px;}
.cn2{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.86rem;line-height:1.2;flex:1;}
.cd{font-size:.7rem;color:var(–muted);margin-left:8px;white-space:nowrap;}
.cs{font-size:.73rem;color:var(–muted);margin-bottom:5px;}
.bdg{display:flex;flex-wrap:wrap;gap:4px;}
.badge{font-size:.63rem;font-weight:600;padding:2px 7px;border-radius:50px;}
.bg{background:var(–gs);color:var(–green);border:1px solid rgba(57,211,83,.3);}
.bt{background:var(–ts);color:var(–teal);border:1px solid rgba(0,201,167,.3);}
.bv{background:#1e2d3d;color:#7ab8f5;border:1px solid rgba(122,184,245,.3);}
.bm{background:#2a1e1e;color:#f08080;border:1px solid rgba(240,128,128,.3);}

.detail{position:fixed;inset:0;background:var(–bg);z-index:5000;
display:flex;flex-direction:column;
transform:translateY(100%);transition:transform .3s cubic-bezier(.34,1.3,.64,1);
padding-bottom:env(safe-area-inset-bottom);}
.detail.open{transform:translateY(0);}
.dh{display:flex;align-items:center;justify-content:space-between;
padding:14px 16px;border-bottom:1px solid var(–bord);flex-shrink:0;
padding-top:max(14px,env(safe-area-inset-top));}
.dback{display:flex;align-items:center;gap:6px;font-size:.83rem;color:var(–muted);
cursor:pointer;background:var(–surf2);border:1px solid var(–bord);
border-radius:50px;padding:6px 12px;}
.dbody{flex:1;overflow-y:auto;padding:16px;-webkit-overflow-scrolling:touch;}
.dname{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.35rem;margin-bottom:4px;}
.dtype{font-size:.82rem;color:var(–muted);margin-bottom:12px;}
.dbdg{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:14px;}
.dsec{font-family:‘Syne’,sans-serif;font-weight:700;font-size:.7rem;color:var(–muted);
text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;margin-top:12px;}
.dinfo .row{font-size:.84rem;color:var(–muted);line-height:1.7;margin-bottom:4px;}
.dbtns{display:flex;gap:10px;padding:16px;border-top:1px solid var(–bord);flex-shrink:0;}
.btn{flex:1;padding:13px;border-radius:50px;border:none;font-family:‘Syne’,sans-serif;
font-weight:700;font-size:.88rem;cursor:pointer;transition:all .15s;}
.bp{background:var(–green);color:#000;}
.bs2{background:var(–surf2);color:var(–text);border:1.5px solid var(–bord);}

.perm{position:fixed;inset:0;background:var(–bg);z-index:9000;
display:flex;flex-direction:column;align-items:center;justify-content:center;
gap:18px;padding:36px 28px;text-align:center;}
.perm-icon{font-size:3.5rem;}
.perm-title{font-family:‘Syne’,sans-serif;font-weight:800;font-size:1.7rem;}
.perm-title b{color:var(–green);}
.perm-sub{font-size:.9rem;color:var(–muted);max-width:300px;line-height:1.65;}
.perm-btn{width:100%;max-width:300px;padding:15px;background:var(–green);color:#000;
border:none;border-radius:50px;font-family:‘Syne’,sans-serif;font-weight:700;
font-size:1rem;cursor:pointer;}
.perm-note{font-size:.73rem;color:var(–muted);max-width:260px;}
.perm-err{font-size:.8rem;color:#f08080;background:rgba(240,80,80,.1);
border:1px solid rgba(240,80,80,.3);border-radius:10px;
padding:10px 14px;max-width:300px;display:none;}

.loading{position:fixed;inset:0;background:var(–bg);z-index:8000;
display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;}
.loading.off{display:none;}
.spin{width:42px;height:42px;border:3px solid var(–bord);border-top-color:var(–green);
border-radius:50%;animation:sp .8s linear infinite;}
@keyframes sp{to{transform:rotate(360deg);}}
.ltxt{font-size:.85rem;color:var(–muted);}

.toast{position:fixed;bottom:calc(38vh + 16px);left:50%;
transform:translateX(-50%) translateY(10px);
background:var(–surf);border:1px solid var(–green);border-radius:50px;
padding:7px 16px;font-size:.79rem;color:var(–green);
opacity:0;pointer-events:none;transition:all .25s;z-index:4000;white-space:nowrap;}
.toast.on{opacity:1;transform:translateX(-50%) translateY(0);}

.leaflet-tile{filter:brightness(.72) saturate(.55);}
.leaflet-control-zoom a{background:var(–surf)!important;color:var(–text)!important;border-color:var(–bord)!important;}
.leaflet-popup-content-wrapper{background:var(–surf2)!important;color:var(–text)!important;
border:1px solid var(–bord)!important;border-radius:12px!important;font-family:‘DM Sans’,sans-serif!important;}
.leaflet-popup-tip{background:var(–surf2)!important;}
.leaflet-attribution-flag{display:none!important;}
</style>

</head>
<body>

<div class="perm" id="perm">
  <div class="perm-icon">📍</div>
  <div class="perm-title">Safe<b>Plate</b></div>
  <div class="perm-sub">Encontramos restaurantes y cafeterías veganas y sin gluten exactamente donde estás.</div>
  <div class="perm-err" id="permErr"></div>
  <button class="perm-btn" id="permBtn" onclick="startApp()">📍 Usar mi ubicación</button>
  <div class="perm-note">Solo usamos tu GPS para buscar lugares cercanos. No guardamos nada.</div>
</div>

<div class="loading off" id="loading">
  <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:1.4rem;">Safe<span style="color:var(--green)">Plate</span></div>
  <div class="spin"></div>
  <div class="ltxt" id="ltxt">Obteniendo tu ubicación…</div>
</div>

<div class="top">
  <div class="logo">Safe<b>Plate</b></div>
  <div class="search">
    <span style="color:var(--muted)">🔍</span>
    <input type="text" id="q" placeholder="Buscar…" oninput="applyF()">
  </div>
</div>

<div class="chips">
  <div class="chip cg" id="f-todos"  onclick="setF('todos')">✦ Todos</div>
  <div class="chip"    id="f-celiac" onclick="setF('celiac')">🌾 Sin Gluten</div>
  <div class="chip"    id="f-vegan"  onclick="setF('vegan')">🌿 Vegano</div>
  <div class="chip"    id="f-ambos"  onclick="setF('ambos')">✨ SG+Vegano</div>
  <div class="chip"    id="f-cafe"   onclick="setF('cafe')">☕ Cafeterías</div>
</div>

<div id="map"></div>

<div class="sheet" id="sheet">
  <div class="sh-head" onclick="toggleSheet()">
    <div>
      <div class="sh-handle"></div>
      <div class="sh-title">Lugares cercanos</div>
      <div class="sh-count"><span id="cnt">0</span> resultados</div>
    </div>
    <div class="sh-btn" id="shBtn">▲ Ver lista</div>
  </div>
  <div class="list" id="list">
    <div style="text-align:center;color:var(--muted);font-size:.83rem;padding:20px;">
      Acepta el permiso de ubicación para comenzar
    </div>
  </div>
</div>

<div class="detail" id="detail">
  <div class="dh">
    <div class="dback" onclick="closeDetail()">← Volver</div>
    <div style="font-family:'Syne',sans-serif;font-weight:700;font-size:.82rem;color:var(--muted)">Detalle</div>
  </div>
  <div class="dbody">
    <div class="dname"  id="dName"></div>
    <div class="dtype"  id="dType"></div>
    <div class="dbdg"   id="dBdg"></div>
    <div class="dsec">Información del lugar</div>
    <div class="dinfo"  id="dInfo"></div>
  </div>
  <div class="dbtns">
    <button class="btn bp" id="btnNav">🧭 Cómo llegar</button>
    <button class="btn bs2" id="btnOsm">🗺 Ver en mapa</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
let map,uLat,uLng,all=[],vis=[],mkrs={},selId=null,curF='todos',shExp=false;

function startApp(){
  if(!navigator.geolocation){showErr('Tu navegador no soporta GPS.');return;}
  document.getElementById('perm').style.display='none';
  document.getElementById('loading').classList.remove('off');
  setL('Obteniendo tu ubicación…');
  navigator.geolocation.getCurrentPosition(
    pos=>{
      uLat=pos.coords.latitude; uLng=pos.coords.longitude;
      setL('Iniciando mapa…'); initMap();
      setL('Buscando lugares cercanos…'); fetchPlaces();
    },
    err=>{
      let m='No se pudo obtener la ubicación.';
      if(err.code===1) m='Permiso denegado. Ve a Ajustes › Safari/Chrome › Ubicación y actívala, luego recarga.';
      if(err.code===2) m='Ubicación no disponible. Verificá tu GPS o WiFi.';
      if(err.code===3) m='Tiempo agotado. Intentá de nuevo con mejor señal.';
      document.getElementById('loading').classList.add('off');
      showErr(m);
    },
    {enableHighAccuracy:true,timeout:15000,maximumAge:0}
  );
}

function showErr(m){
  const e=document.getElementById('permErr');
  e.textContent=m; e.style.display='block';
  document.getElementById('perm').style.display='flex';
  document.getElementById('permBtn').textContent='🔄 Reintentar';
}
function setL(t){document.getElementById('ltxt').textContent=t;}

function initMap(){
  map=L.map('map',{zoomControl:false,attributionControl:false}).setView([uLat,uLng],15);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{maxZoom:19}).addTo(map);
  L.control.zoom({position:'bottomright'}).addTo(map);
  const yi=L.divIcon({className:'',
    html:`<div style="width:20px;height:20px;background:#7ab8f5;border:3px solid #fff;
      border-radius:50%;box-shadow:0 0 0 8px rgba(122,184,245,.2)"></div>`,
    iconSize:[20,20],iconAnchor:[10,10]});
  L.marker([uLat,uLng],{icon:yi,zIndexOffset:1000}).addTo(map)
    .bindPopup('<b style="color:#7ab8f5">📍 Estás aquí</b>');
  map.on('click',()=>{selId=null;renderMkrs();});
}

async function fetchPlaces(){
  const r=1500;
  const q=`[out:json][timeout:25];(
    node["amenity"="restaurant"](around:${r},${uLat},${uLng});
    node["amenity"="cafe"](around:${r},${uLat},${uLng});
    node["amenity"="fast_food"](around:${r},${uLat},${uLng});
    node["shop"="bakery"](around:${r},${uLat},${uLng});
    node["shop"="health_food"](around:${r},${uLat},${uLng});
    way["amenity"="restaurant"](around:${r},${uLat},${uLng});
    way["amenity"="cafe"](around:${r},${uLat},${uLng});
    way["amenity"="fast_food"](around:${r},${uLat},${uLng});
    way["shop"="bakery"](around:${r},${uLat},${uLng});
  );out center tags;`;
  try{
    const res=await fetch('https://overpass-api.de/api/interpreter',
      {method:'POST',body:'data='+encodeURIComponent(q)});
    if(!res.ok) throw new Error();
    processPlaces((await res.json()).elements);
  }catch{
    document.getElementById('loading').classList.add('off');
    document.getElementById('list').innerHTML=
      `<div style="text-align:center;color:#f08080;font-size:.83rem;padding:20px;">
        ⚠️ Error al cargar lugares. Verificá tu conexión.</div>`;
    toast('⚠️ Sin conexión a OpenStreetMap');
  }
}

function processPlaces(els){
  all=els.filter(e=>e.tags?.name).map(e=>{
    const t=e.tags,lat=e.lat??e.center?.lat,lng=e.lon??e.center?.lon;
    if(!lat||!lng) return null;
    const gf=t['diet:gluten_free']==='yes'||t['diet:gluten_free']==='only';
    const ve=t['diet:vegan']==='yes'||t['diet:vegan']==='only';
    const vg=t['diet:vegetarian']==='yes'||t['diet:vegetarian']==='only';
    let cat='none';
    if(gf&&ve) cat='ambos'; else if(gf) cat='celiac'; else if(ve) cat='vegan'; else if(vg) cat='vegetarian';
    const am=t.amenity||t.shop||'';
    let type='Restaurante';
    if(am==='cafe') type='Cafetería';
    else if(am==='bakery') type='Panadería';
    else if(am==='fast_food') type='Fast Food';
    else if(am==='health_food') type='Tienda Saludable';
    const dist=hav(uLat,uLng,lat,lng);
    const info=[];
    if(t['diet:vegan']==='only')       info.push('🌿 100% Vegano');
    else if(t['diet:vegan']==='yes')   info.push('🌿 Opciones veganas disponibles');
    if(t['diet:gluten_free']==='only') info.push('🌾 100% Sin gluten');
    else if(t['diet:gluten_free']==='yes') info.push('🌾 Opciones sin gluten');
    if(vg) info.push('🥗 Opciones vegetarianas');
    if(t.cuisine)       info.push('🍴 '+t.cuisine.replace(/_/g,' '));
    if(t.opening_hours) info.push('🕐 '+t.opening_hours);
    if(t['addr:street']) info.push('📌 '+t['addr:street']+(t['addr:housenumber']?' '+t['addr:housenumber']:''));
    if(t.phone)    info.push('📞 '+t.phone);
    if(t.website)  info.push('🌐 '+t.website.replace(/^https?:\/\//,'').slice(0,35));
    if(!info.length) info.push('Sin información adicional en OpenStreetMap.');
    return {id:e.id,name:t.name,type,cat,lat,lng,dist,info,osmId:e.id,osmType:e.type||'node'};
  }).filter(Boolean).sort((a,b)=>a.dist-b.dist);
  document.getElementById('loading').classList.add('off');
  applyF();
  toast(`✅ ${all.length} lugares encontrados`);
}

function hav(a,b,c,d){
  const R=6371000,dA=(c-a)*Math.PI/180,dB=(d-b)*Math.PI/180;
  const x=Math.sin(dA/2)**2+Math.cos(a*Math.PI/180)*Math.cos(c*Math.PI/180)*Math.sin(dB/2)**2;
  return Math.round(R*2*Math.atan2(Math.sqrt(x),Math.sqrt(1-x)));
}
function fm(m){return m<1000?m+'m':(m/1000).toFixed(1)+'km';}

function setF(f){
  curF=f;
  document.querySelectorAll('.chip').forEach(c=>{c.className='chip';});
  const cl=f==='vegan'?'ct':f==='ambos'?'ca':'cg';
  document.getElementById('f-'+f)?.classList.add(cl);
  applyF();
}

function applyF(){
  const q=document.getElementById('q').value.toLowerCase();
  vis=all.filter(p=>{
    const mf=curF==='todos'?true:curF==='celiac'?(p.cat==='celiac'||p.cat==='ambos'):
      curF==='vegan'?(p.cat==='vegan'||p.cat==='ambos'):curF==='ambos'?p.cat==='ambos':
      curF==='cafe'?p.type==='Cafetería':true;
    return mf&&(q?p.name.toLowerCase().includes(q):true);
  });
  document.getElementById('cnt').textContent=vis.length;
  renderList(); if(map) renderMkrs();
}

function emo(t){return t==='Cafetería'?'☕':t==='Panadería'?'🥖':t==='Fast Food'?'🍔':'🍽';}
function col(c){return c==='celiac'?'#39d353':c==='vegan'?'#00c9a7':c==='ambos'?'#f0a500':'#8b949e';}
function cc(c){return c==='vegan'?'cv':c==='ambos'?'ca':c==='none'||c==='vegetarian'?'cn':'';}

function renderList(){
  const el=document.getElementById('list');
  if(!vis.length){
    el.innerHTML=`<div style="text-align:center;color:var(--muted);font-size:.83rem;padding:20px;">
      Sin resultados${curF!=='todos'?' para este filtro':''}.</div>`;
    return;
  }
  el.innerHTML='';
  vis.slice(0,60).forEach(p=>{
    const d=document.createElement('div');
    d.className=`card ${cc(p.cat)}${selId===p.id?' sel':''}`;
    d.id='c'+p.id;
    d.innerHTML=`
      <div class="cr"><div class="cn2">${p.name}</div><div class="cd">${fm(p.dist)}</div></div>
      <div class="cs">${emo(p.type)} ${p.type}</div>
      <div class="bdg">
        ${p.cat==='celiac'||p.cat==='ambos'?'<span class="badge bg">🌾 Sin Gluten</span>':''}
        ${p.cat==='vegan'||p.cat==='ambos'?'<span class="badge bt">🌿 Vegano</span>':''}
        ${p.cat==='vegetarian'?'<span class="badge bv">🥗 Vegetariano</span>':''}
        ${p.cat==='none'?'<span class="badge bm">Sin datos dietéticos</span>':''}
      </div>`;
    d.onclick=()=>openDetail(p);
    el.appendChild(d);
  });
}

function renderMkrs(){
  Object.values(mkrs).forEach(m=>map.removeLayer(m));
  mkrs={};
  const vs=new Set(vis.map(p=>p.id));
  all.forEach(p=>{
    const iv=vs.has(p.id),isSel=selId===p.id,c=col(p.cat),sz=isSel?46:34;
    const icon=L.divIcon({className:'',
      html:`<div style="width:${sz}px;height:${sz}px;border-radius:50%;
        background:${c}25;border:2.5px solid ${c};display:flex;align-items:center;
        justify-content:center;font-size:${isSel?18:12}px;opacity:${iv?1:0.15};
        box-shadow:${isSel?`0 0 0 4px ${c}40,`:''}0 3px 10px rgba(0,0,0,.5);
        cursor:pointer;">${emo(p.type)}</div>`,
      iconSize:[sz,sz],iconAnchor:[sz/2,sz/2]});
    const mk=L.marker([p.lat,p.lng],{icon,zIndexOffset:isSel?100:0}).addTo(map);
    if(iv) mk.on('click',e=>{e.originalEvent.stopPropagation();openDetail(p);});
    mkrs[p.id]=mk;
  });
}

function openDetail(p){
  selId=p.id;
  document.querySelectorAll('.card').forEach(c=>c.classList.remove('sel'));
  document.getElementById('c'+p.id)?.classList.add('sel');
  if(map) map.flyTo([p.lat,p.lng],17,{animate:true,duration:.6});
  renderMkrs();
  document.getElementById('dName').textContent=p.name;
  document.getElementById('dType').textContent=`${emo(p.type)} ${p.type} · 📍 ${fm(p.dist)}`;
  const b=document.getElementById('dBdg');
  b.innerHTML='';
  if(p.cat==='celiac'||p.cat==='ambos') b.innerHTML+='<span class="badge bg" style="font-size:.76rem;padding:3px 10px;">🌾 Sin Gluten</span>';
  if(p.cat==='vegan'||p.cat==='ambos')  b.innerHTML+='<span class="badge bt" style="font-size:.76rem;padding:3px 10px;">🌿 Vegano</span>';
  if(p.cat==='vegetarian') b.innerHTML+='<span class="badge bv" style="font-size:.76rem;padding:3px 10px;">🥗 Vegetariano</span>';
  if(p.cat==='none') b.innerHTML+='<span class="badge bm" style="font-size:.76rem;padding:3px 10px;">Sin datos en OSM</span>';
  document.getElementById('dInfo').innerHTML=p.info.map(l=>`<div class="row">${l}</div>`).join('');
  document.getElementById('btnNav').onclick=()=>
    window.open(`https://www.google.com/maps/dir/?api=1&destination=${p.lat},${p.lng}`,'_blank');
  document.getElementById('btnOsm').onclick=()=>
    window.open(`https://www.openstreetmap.org/${p.osmType}/${p.osmId}`,'_blank');
  document.getElementById('detail').classList.add('open');
}

function closeDetail(){
  document.getElementById('detail').classList.remove('open');
  selId=null; renderMkrs();
}

function toggleSheet(){
  shExp=!shExp;
  document.getElementById('sheet').classList.toggle('exp',shExp);
  document.getElementById('shBtn').textContent=shExp?'▼ Minimizar':'▲ Ver lista';
  document.getElementById('toast').style.bottom=shExp?'calc(70vh + 12px)':'calc(38vh + 12px)';
}

function toast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg; t.classList.add('on');
  setTimeout(()=>t.classList.remove('on'),3000);
}
</script>

</body>
</html>
