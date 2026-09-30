<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Blood Strike — Strategy Board</title>
<style>
:root{
  --bg:#0a0c10;--surface:#111520;--panel:#161b26;--border:#232a3a;
  --accent:#e8333a;--text:#e8eaf0;--muted:#6b7280;
  --green:#22c55e;--blue:#3b82f6;--yellow:#f59e0b;--purple:#a855f7;
}
*{box-sizing:border-box;margin:0;padding:0}
body{background:var(--bg);color:var(--text);font-family:'Segoe UI',system-ui,sans-serif;height:100dvh;display:flex;flex-direction:column;overflow:hidden;user-select:none}

header{background:var(--surface);border-bottom:1px solid var(--border);padding:0 8px;height:46px;display:flex;align-items:center;gap:6px;flex-shrink:0;z-index:20}
.logo{font-size:11px;font-weight:700;letter-spacing:.08em;color:var(--accent);text-transform:uppercase;white-space:nowrap}
.logo span{color:var(--text)}
.map-tabs{display:flex;gap:3px}
.map-tab{padding:4px 10px;border-radius:4px;border:1px solid var(--border);background:transparent;color:var(--muted);font-size:10px;font-weight:600;cursor:pointer;transition:all .15s;white-space:nowrap}
.map-tab:hover{color:var(--text);border-color:var(--accent)}
.map-tab.active{background:var(--accent);border-color:var(--accent);color:#fff}
.map-tab-sub{font-size:7px;font-weight:400;display:block;line-height:1;margin-top:1px;opacity:.7}
.hsep{width:1px;height:24px;background:var(--border);flex-shrink:0}
.hbtn{padding:4px 8px;border-radius:4px;border:1px solid var(--border);background:var(--panel);color:var(--text);font-size:10px;font-weight:600;cursor:pointer;transition:all .15s;white-space:nowrap}
.hbtn:hover{border-color:var(--accent);color:var(--accent)}
.hbtn.act{border-color:var(--accent);color:var(--accent);background:rgba(232,51,58,.12)}
.hbtn.green{border-color:var(--green);color:var(--green)}
.hbtn.green:hover{background:rgba(34,197,94,.15)}
.hsp{flex:1}



.workspace{display:flex;flex:1;overflow:hidden}
.lp{width:196px;background:var(--panel);border-right:1px solid var(--border);display:flex;flex-direction:column;overflow-y:auto;flex-shrink:0}
.rp{width:196px;background:var(--panel);border-left:1px solid var(--border);display:flex;flex-direction:column;overflow-y:auto;flex-shrink:0}
.ps{padding:8px;border-bottom:1px solid var(--border)}
.pl{font-size:9px;font-weight:700;letter-spacing:.1em;color:var(--muted);text-transform:uppercase;margin-bottom:6px;display:flex;align-items:center;justify-content:space-between}
.pl-btn{background:none;border:none;color:var(--accent);font-size:9px;font-weight:700;cursor:pointer;padding:0;letter-spacing:0;text-transform:none}

.tgrid{display:grid;grid-template-columns:1fr 1fr;gap:3px}
.tbtn{padding:6px 3px;border-radius:4px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:9px;font-weight:600;cursor:pointer;transition:all .12s;text-align:center;display:flex;flex-direction:column;align-items:center;gap:2px}
.tbtn .ti{font-size:14px}
.tbtn:hover{color:var(--text);border-color:var(--accent)}
.tbtn.active{background:rgba(232,51,58,.15);border-color:var(--accent);color:var(--accent)}

.cgrid{display:grid;grid-template-columns:repeat(5,1fr);gap:4px}
.cdot{width:100%;aspect-ratio:1;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:all .12s}
.cdot:hover{transform:scale(1.15)}
.cdot.active{border-color:#fff;transform:scale(1.1)}
.cust-row{display:flex;align-items:center;gap:6px;margin-top:5px}
.cust-swatch{width:22px;height:22px;border-radius:50%;border:2px solid var(--border);cursor:pointer;overflow:hidden;position:relative;flex-shrink:0}
.cust-swatch input[type=color]{position:absolute;inset:-4px;opacity:0;cursor:pointer;width:calc(100%+8px);height:calc(100%+8px)}

.trow{display:flex;gap:4px}
.tkbtn{flex:1;background:var(--surface);border:1px solid var(--border);border-radius:4px;cursor:pointer;display:flex;align-items:center;justify-content:center;height:26px;transition:all .12s}
.tkbtn:hover,.tkbtn.active{border-color:var(--accent);background:rgba(232,51,58,.12)}
.tkline{border-radius:99px;background:var(--text);width:80%}
input[type=range]{width:100%;accent-color:var(--accent);cursor:pointer}

.phase-list,.elem-list,.player-list{display:flex;flex-direction:column;gap:3px}
.phase-row,.elem-row,.player-row{display:flex;align-items:center;gap:3px;padding:4px 5px;border-radius:4px;border:1px solid var(--border);background:var(--surface)}
.elem-row,.player-row{cursor:default}
.elem-row.active{border-color:var(--accent);background:rgba(232,51,58,.12)}
.elem-row{cursor:pointer}
.phase-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0;cursor:pointer;border:1px solid rgba(255,255,255,.2)}
.phase-name-inp,.elem-name-inp,.player-name-inp{flex:1;background:transparent;border:none;color:var(--text);font-size:10px;font-weight:600;font-family:inherit;outline:none;min-width:0}
.phase-sel{padding:3px 5px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--muted);font-size:8px;font-weight:700;cursor:pointer;flex-shrink:0}
.phase-sel.active,.phase-sel:hover{border-color:var(--accent);color:var(--accent);background:rgba(232,51,58,.1)}
.xbtn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:11px;padding:0 1px;flex-shrink:0}
.xbtn:hover{color:var(--accent)}
.elem-img{width:22px;height:22px;border-radius:3px;background:rgba(0,0,0,.4);display:flex;align-items:center;justify-content:center;font-size:13px;overflow:hidden;flex-shrink:0;position:relative;border:1px dashed rgba(255,255,255,.15);cursor:pointer}
.elem-img img{width:100%;height:100%;object-fit:cover;border-radius:2px}
.elem-img input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer}
.player-dot{width:11px;height:11px;border-radius:50%;flex-shrink:0;cursor:pointer;border:1px solid rgba(255,255,255,.2)}
.place-btn{padding:2px 5px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--muted);font-size:9px;cursor:pointer;flex-shrink:0}
.place-btn:hover{border-color:var(--accent);color:var(--accent)}
.place-btn.placing{border-color:var(--green);color:var(--green);background:rgba(34,197,94,.12)}
.add-btn{width:100%;padding:4px;border-radius:4px;border:1px dashed var(--border);background:transparent;color:var(--muted);font-size:10px;cursor:pointer;margin-top:3px}
.add-btn:hover{border-color:var(--accent);color:var(--accent)}
.add-btn.green:hover{border-color:var(--green);color:var(--green)}

/* CANVAS AREA */
.cw{flex:1;position:relative;overflow:hidden;background:#0d1117}
#main-canvas{position:absolute;top:0;left:0;display:block;cursor:crosshair}
.map-ph{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:12px;color:var(--muted);cursor:pointer}
.map-ph:hover{color:var(--text)}
.ph-icon{font-size:40px;opacity:.4}
.ph-txt{font-size:13px;font-weight:600}
.ph-sub{font-size:10px;opacity:.6}

.zoom-ctrl{position:absolute;bottom:12px;right:12px;display:flex;flex-direction:column;gap:3px;z-index:10}
.zbtn{width:28px;height:28px;border-radius:5px;border:1px solid var(--border);background:rgba(22,27,38,.92);color:var(--text);font-size:14px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center}
.zbtn:hover{border-color:var(--accent);color:var(--accent)}
.zpct{font-size:9px;text-align:center;color:var(--muted);font-weight:700}

#eraser-ring{position:absolute;border:2px solid rgba(255,255,255,.6);border-radius:50%;pointer-events:none;display:none;transform:translate(-50%,-50%)}
#ruler-tip{position:absolute;background:rgba(0,0,0,.85);border:1px solid var(--yellow);color:var(--yellow);font-size:10px;font-weight:700;padding:3px 7px;border-radius:4px;pointer-events:none;display:none;white-space:nowrap}

/* RIGHT PANEL TABS */
.rtabs{display:flex;border-bottom:1px solid var(--border)}
.rtab{flex:1;padding:6px 4px;font-size:9px;font-weight:700;text-align:center;cursor:pointer;color:var(--muted);border-bottom:2px solid transparent;background:none;border-top:none;border-left:none;border-right:none}
.rtab:hover{color:var(--text)}
.rtab.active{color:var(--accent);border-bottom-color:var(--accent)}
.rpanel{display:none;flex-direction:column;flex:1;overflow-y:auto}
.rpanel.active{display:flex;flex-direction:column}

.notes-ta{width:100%;background:var(--surface);border:1px solid var(--border);border-radius:4px;color:var(--text);font-size:10px;font-family:inherit;padding:5px 7px;resize:none;outline:none;min-height:60px;line-height:1.5}
.notes-ta:focus{border-color:var(--accent)}
.notes-ta::placeholder{color:var(--muted)}
.sinp{width:100%;padding:5px 7px;background:var(--surface);border:1px solid var(--border);border-radius:4px;color:var(--text);font-size:10px;font-family:inherit;outline:none}
.sinp:focus{border-color:var(--accent)}
.sinp::placeholder{color:var(--muted)}
.tag-row{display:flex;flex-wrap:wrap;gap:3px}
.tag{padding:2px 7px;border-radius:99px;border:1px solid var(--border);background:var(--surface);color:var(--muted);font-size:9px;font-weight:600;cursor:pointer}
.tag:hover{border-color:var(--accent);color:var(--accent)}
.tag.active{border-color:var(--accent);background:rgba(232,51,58,.2);color:var(--accent)}
.save-btn{width:100%;padding:6px;border-radius:4px;border:1px solid var(--accent);background:rgba(232,51,58,.15);color:var(--accent);font-size:11px;font-weight:700;cursor:pointer}
.save-btn:hover{background:rgba(232,51,58,.3)}
.scard{padding:7px;border-radius:4px;border:1px solid var(--border);background:var(--surface);margin-bottom:3px}
.scard:hover{border-color:rgba(232,51,58,.4)}
.scard-name{font-size:11px;font-weight:700;color:var(--text);margin-bottom:2px}
.scard-meta{font-size:9px;color:var(--muted);margin-bottom:3px}
.scard-tags{display:flex;flex-wrap:wrap;gap:2px;margin-bottom:4px}
.stag{padding:1px 5px;border-radius:99px;font-size:8px;font-weight:700}
.scard-acts{display:flex;gap:3px}
.scard-btn{flex:1;padding:3px;border-radius:3px;border:1px solid var(--border);background:transparent;color:var(--muted);font-size:9px;font-weight:600;cursor:pointer}
.scard-btn:hover{border-color:var(--accent);color:var(--accent)}
.ilist{display:flex;flex-direction:column;gap:2px}
.ientry{display:flex;align-items:center;gap:4px;font-size:9px;color:var(--muted);padding:3px 4px;border-radius:3px;border:1px solid transparent}
.ientry:hover{border-color:var(--accent);color:var(--accent);background:rgba(232,51,58,.07)}
.idel{margin-left:auto;background:none;border:none;color:var(--muted);cursor:pointer;font-size:11px;padding:0 2px}
.idel:hover{color:var(--accent)}
.upzone{display:flex;flex-direction:column;align-items:center;justify-content:center;gap:4px;padding:10px 6px;border:1.5px dashed var(--border);border-radius:5px;cursor:pointer;background:var(--surface);text-align:center;font-size:10px;color:var(--muted)}
.upzone:hover{border-color:var(--accent);color:var(--accent)}

/* POST MATCH */
.pm-lbl{font-size:9px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:.05em;margin-bottom:2px}
.pm-ta{width:100%;background:var(--surface);border:1px solid var(--border);border-radius:4px;color:var(--text);font-size:10px;font-family:inherit;padding:5px 7px;resize:none;outline:none;line-height:1.4}
.pm-ta:focus{border-color:var(--accent)}
.pm-ta::placeholder{color:var(--muted)}
.pm-stars{display:flex;gap:3px}
.pm-star{font-size:16px;cursor:pointer;opacity:.3}
.pm-star.active{opacity:1}
.pm-save{width:100%;padding:6px;border-radius:4px;border:1px solid var(--blue);background:rgba(59,130,246,.12);color:var(--blue);font-size:11px;font-weight:700;cursor:pointer}
.pm-save:hover{background:rgba(59,130,246,.25)}
.pm-card{padding:7px;border-radius:4px;border:1px solid var(--border);background:var(--surface);margin-bottom:3px}
.pm-card-title{font-size:10px;font-weight:700;color:var(--text);margin-bottom:2px}
.pm-card-meta{font-size:8px;color:var(--muted);margin-bottom:4px}
.pm-card-row{display:flex;gap:6px}
.pm-col{flex:1;min-width:0}
.pm-col-lbl{font-size:8px;color:var(--muted);margin-bottom:1px}
.pm-col-val{font-size:9px;color:var(--text);display:-webkit-box;-webkit-line-clamp:2;-webkit-box-orient:vertical;overflow:hidden}
.pm-del{background:none;border:none;color:var(--muted);cursor:pointer;font-size:11px;float:right}
.pm-del:hover{color:var(--accent)}
.div{height:1px;background:var(--border);margin:4px 0}

/* SHORTCUTS */
.sc-grid{display:grid;grid-template-columns:1fr 1fr;gap:2px}
.sc-row{display:flex;align-items:center;gap:4px;padding:2px 0}
.sc-k{background:var(--surface);border:1px solid var(--border);border-radius:2px;padding:1px 4px;font-size:8px;font-weight:700;color:var(--accent);font-family:monospace}
.sc-d{font-size:8px;color:var(--muted)}

/* MODALS */
.mov{position:fixed;inset:0;background:rgba(0,0,0,.78);display:none;align-items:center;justify-content:center;z-index:100}
.mov.open{display:flex}
.modal{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:18px;width:300px;max-width:92vw}
.modal h3{font-size:13px;font-weight:700;margin-bottom:3px}
.modal p{font-size:10px;color:var(--muted);margin-bottom:10px}
.mi{width:100%;padding:6px 8px;background:var(--surface);border:1px solid var(--border);border-radius:4px;color:var(--text);font-size:11px;font-family:inherit;outline:none;margin-bottom:8px}
.mi:focus{border-color:var(--accent)}
.mrow{display:flex;gap:6px}
.mbtn{flex:1;padding:7px;border-radius:4px;border:1px solid var(--border);font-size:11px;font-weight:700;cursor:pointer}
.mbtn.cancel{background:var(--surface);color:var(--muted)}
.mbtn.confirm{background:var(--accent);border-color:var(--accent);color:#fff}
.mbtn.confirm:hover{background:#c02028}
.cpgrid{display:grid;grid-template-columns:repeat(6,1fr);gap:5px;margin-bottom:8px}
.cpdot{width:100%;aspect-ratio:1;border-radius:50%;cursor:pointer;border:2px solid transparent}
.cpdot:hover,.cpdot.active{border-color:#fff;transform:scale(1.1)}
.cp-cs{width:28px;height:28px;border-radius:50%;border:2px solid var(--border);overflow:hidden;position:relative;flex-shrink:0;cursor:pointer}
.cp-cs input{position:absolute;inset:-4px;opacity:0;cursor:pointer;width:calc(100%+8px);height:calc(100%+8px)}

.toast{position:fixed;bottom:16px;right:16px;background:var(--surface);border:1px solid var(--green);color:var(--green);padding:7px 12px;border-radius:5px;font-size:11px;font-weight:600;z-index:200;opacity:0;transition:opacity .3s;pointer-events:none}
.toast.show{opacity:1}
::-webkit-scrollbar{width:3px}::-webkit-scrollbar-track{background:transparent}::-webkit-scrollbar-thumb{background:var(--border);border-radius:2px}
.no-items{font-size:10px;color:var(--muted);padding:4px 0;text-align:center}
</style>
</head>
<body>

<header>
  <div class="logo">BS <span>// Strategy</span></div>
  <div class="map-tabs">
    <button class="map-tab active" onclick="selMap(0)">MAP 1<span class="map-tab-sub" id="mn0">—</span></button>
    <button class="map-tab" onclick="selMap(1)">MAP 2<span class="map-tab-sub" id="mn1">—</span></button>
    <button class="map-tab" onclick="selMap(2)">MAP 3<span class="map-tab-sub" id="mn2">—</span></button>
  </div>
  <div class="hsp"></div>
  <button class="hbtn" id="gbtn" onclick="togGrid()">⊞ Grid</button>
  <button class="hbtn green" onclick="doExport()">⬇ PNG</button>
  <button class="hbtn" onclick="doUndo()">↩</button>
  <button class="hbtn" onclick="doClear()">🗑</button>
</header>

<div class="workspace">

<!-- LEFT PANEL -->
<div class="lp">
  <div class="ps">
    <div class="pl">Herramienta</div>
    <div class="tgrid">
      <button class="tbtn active" id="tool-pen" onclick="setTool('pen')"><span class="ti">✏️</span>Línea</button>
      <button class="tbtn" id="tool-arrow" onclick="setTool('arrow')"><span class="ti">➡️</span>Flecha</button>
      <button class="tbtn" id="tool-circle" onclick="setTool('circle')"><span class="ti">⭕</span>Zona</button>
      <button class="tbtn" id="tool-rect" onclick="setTool('rect')"><span class="ti">▭</span>Área</button>
      <button class="tbtn" id="tool-ruler" onclick="setTool('ruler')"><span class="ti">📏</span>Distancia</button>
      <button class="tbtn" id="tool-text" onclick="setTool('text')"><span class="ti">🔤</span>Texto</button>
      <button class="tbtn" id="tool-select" onclick="setTool('select')"><span class="ti">🖱️</span>Mover</button>
      <button class="tbtn" id="tool-eraser" onclick="setTool('eraser')"><span class="ti">🧹</span>Borrador</button>
      <button class="tbtn" id="tool-utility" onclick="setTool('utility')"><span class="ti">📍</span>Colocar</button>
    </div>
  </div>

  <div class="ps">
    <div class="pl">Color <span id="cprev" style="display:inline-block;width:12px;height:12px;border-radius:2px;border:1px solid var(--border);background:#e8333a;vertical-align:middle"></span></div>
    <div class="cgrid" id="cgrid"></div>
    <div class="cust-row">
      <div class="cust-swatch" id="cswatch" style="background:#e8333a">
        <input type="color" value="#e8333a" oninput="setColor(this.value)">
      </div>
      <span style="font-size:9px;color:var(--muted)">Color personalizado</span>
    </div>
  </div>

  <div class="ps">
    <div class="pl">Grosor</div>
    <div class="trow">
      <div class="tkbtn active" id="tk1" onclick="setThick(2,1)"><div class="tkline" style="height:1.5px"></div></div>
      <div class="tkbtn" id="tk2" onclick="setThick(4,2)"><div class="tkline" style="height:3px"></div></div>
      <div class="tkbtn" id="tk3" onclick="setThick(7,3)"><div class="tkline" style="height:6px"></div></div>
      <div class="tkbtn" id="tk4" onclick="setThick(12,4)"><div class="tkline" style="height:10px"></div></div>
    </div>
  </div>

  <div class="ps">
    <div class="pl">Opacidad <span id="opval">100%</span></div>
    <input type="range" min="20" max="100" value="100" oninput="setOpacity(this.value)">
    <div class="pl" style="margin-top:6px">Borrador <span id="erval">20px</span></div>
    <input type="range" min="8" max="60" value="20" oninput="setEraserSize(this.value)">
  </div>

  <div class="ps">
    <div class="pl">Fases <button class="pl-btn" onclick="addPhase()">+ Nueva</button></div>
    <div class="phase-list" id="phase-list"></div>
  </div>

  <div class="ps">
    <div class="pl">Elementos <button class="pl-btn" onclick="addElement()">+ Nuevo</button></div>
    <div class="elem-list" id="elem-list"></div>
  </div>

  <div class="ps">
    <div class="pl">Imagen del Mapa</div>
    <div class="upzone" onclick="trigUpload()">
      <span style="font-size:16px">🗺️</span>
      <span>Subir imagen del mapa activo</span>
      <span style="font-size:8px">PNG / JPG / WEBP</span>
    </div>
    <input type="file" id="mapfile" accept="image/*" style="display:none" onchange="loadMap(event)">
  </div>

  <div class="ps">
    <div class="pl">Atajos de teclado</div>
    <div class="sc-grid">
      <div class="sc-row"><span class="sc-k">P</span><span class="sc-d">Línea</span></div>
      <div class="sc-row"><span class="sc-k">A</span><span class="sc-d">Flecha</span></div>
      <div class="sc-row"><span class="sc-k">R</span><span class="sc-d">Distancia</span></div>
      <div class="sc-row"><span class="sc-k">T</span><span class="sc-d">Texto</span></div>
      <div class="sc-row"><span class="sc-k">S</span><span class="sc-d">Mover</span></div>
      <div class="sc-row"><span class="sc-k">E</span><span class="sc-d">Borrador</span></div>
      <div class="sc-row"><span class="sc-k">G</span><span class="sc-d">Grid</span></div>
      <div class="sc-row"><span class="sc-k">Ctrl+Z</span><span class="sc-d">Deshacer</span></div>
      <div class="sc-row"><span class="sc-k">Ctrl+🖱</span><span class="sc-d">Zoom</span></div>
      <div class="sc-row"><span class="sc-k">Rueda</span><span class="sc-d">Pan vertical</span></div>
      <div class="sc-row"><span class="sc-k">Del</span><span class="sc-d">Borrar sel.</span></div>

    </div>
  </div>
</div>

<!-- CANVAS -->
<div class="cw" id="cw">
  <canvas id="main-canvas"></canvas>
  <div class="map-ph" id="map-ph" onclick="trigUpload()">
    <div class="ph-icon">🗺️</div>
    <div class="ph-txt">Click para subir imagen del mapa</div>
    <div class="ph-sub">PNG / JPG / WEBP</div>
  </div>
  <div class="zoom-ctrl">
    <button class="zbtn" onclick="zoomIn()">+</button>
    <div class="zpct" id="zpct">100%</div>
    <button class="zbtn" onclick="zoomOut()">−</button>
    <button class="zbtn" style="font-size:9px" onclick="zoomReset()">⌂</button>
  </div>
  <div id="eraser-ring"></div>
  <div id="ruler-tip"></div>
</div>

<!-- RIGHT PANEL -->
<div class="rp">
  <div class="rtabs">
    <button class="rtab active" onclick="showTab('strats')">Strats</button>
    <button class="rtab" onclick="showTab('players')">Equipo</button>
    <button class="rtab" onclick="showTab('postmatch')">Post-Match</button>
  </div>

  <!-- STRATS TAB -->
  <div class="rpanel active" id="rtab-strats">
    <div class="ps">
      <div class="pl">Notas</div>
      <textarea class="notes-ta" id="strat-notes" placeholder="Callouts, rotaciones, condiciones..."></textarea>
    </div>
    <div class="ps">
      <div class="pl">Guardar Estrategia</div>
      <input class="sinp" type="text" id="sname" placeholder="Nombre..." style="margin-bottom:5px">
      <div class="pl" style="margin-bottom:4px">Tipo</div>
      <div class="tag-row" id="save-tags" style="margin-bottom:6px">
        <div class="tag active" data-tag="Ataque" onclick="togSaveTag(this)">Ataque</div>
        <div class="tag" data-tag="Defensa" onclick="togSaveTag(this)">Defensa</div>
        <div class="tag" data-tag="Rotación" onclick="togSaveTag(this)">Rotación</div>
        <div class="tag" data-tag="Clutch" onclick="togSaveTag(this)">Clutch</div>
        <div class="tag" data-tag="Drop" onclick="togSaveTag(this)">Drop</div>
        <div class="tag" data-tag="Ring" onclick="togSaveTag(this)">Ring</div>
      </div>
      <button class="save-btn" onclick="saveStrat()">💾 Guardar</button>
    </div>
    <div class="ps" style="flex:1">
      <div class="pl">Biblioteca</div>
      <input class="sinp" id="ssearch" placeholder="🔍 Buscar..." style="margin-bottom:4px" oninput="renderStrats()">
      <div class="tag-row" id="ftags" style="margin-bottom:5px">
        <div class="tag active" data-ftag="Todos" onclick="setFTag(this,'Todos')">Todos</div>
        <div class="tag" data-ftag="Ataque" onclick="setFTag(this,'Ataque')">Ataque</div>
        <div class="tag" data-ftag="Defensa" onclick="setFTag(this,'Defensa')">Defensa</div>
        <div class="tag" data-ftag="Rotación" onclick="setFTag(this,'Rotación')">Rotación</div>
        <div class="tag" data-ftag="Clutch" onclick="setFTag(this,'Clutch')">Clutch</div>
        <div class="tag" data-ftag="Drop" onclick="setFTag(this,'Drop')">Drop</div>
      </div>
      <div id="slist"><div class="no-items">Ninguna aún</div></div>
    </div>
    <div class="ps">
      <div class="pl">Elementos en Mapa</div>
      <div class="ilist" id="ilist"><div class="no-items">Ninguno</div></div>
    </div>
  </div>

  <!-- PLAYERS TAB -->
  <div class="rpanel" id="rtab-players">
    <div class="ps" style="flex:1">
      <div class="pl">Jugadores <button class="pl-btn" style="color:var(--green)" onclick="addPlayer()">+ Agregar</button></div>
      <div class="player-list" id="player-list"></div>
    </div>
  </div>

  <!-- POST-MATCH TAB -->
  <div class="rpanel" id="rtab-postmatch">
    <div class="ps">
      <div class="pl">📋 Análisis Post-Partido</div>
      <div class="pm-lbl">Partido / Torneo</div>
      <input class="sinp" id="pm-match" placeholder="Ej: Copa Latina — R3" style="margin-bottom:6px">
      <div class="div"></div>
      <div class="pm-lbl">✅ ¿Qué salió bien?</div>
      <textarea class="pm-ta" id="pm-good" rows="2" placeholder="Rotaciones, comunicación..."></textarea>
      <div class="div"></div>
      <div class="pm-lbl">❌ ¿Qué salió mal?</div>
      <textarea class="pm-ta" id="pm-bad" rows="2" placeholder="Errores, posicionamiento..."></textarea>
      <div class="div"></div>
      <div class="pm-lbl">🔄 ¿Qué cambiar?</div>
      <textarea class="pm-ta" id="pm-improve" rows="2" placeholder="Ajustes para la próxima..."></textarea>
      <div class="div"></div>
      <div class="pm-lbl">Calificación</div>
      <div class="pm-stars" id="pm-stars" style="margin-bottom:8px">
        <span class="pm-star" onclick="setPmR(1)">★</span>
        <span class="pm-star" onclick="setPmR(2)">★</span>
        <span class="pm-star" onclick="setPmR(3)">★</span>
        <span class="pm-star" onclick="setPmR(4)">★</span>
        <span class="pm-star" onclick="setPmR(5)">★</span>
      </div>
      <button class="pm-save" onclick="savePm()">📋 Guardar Análisis</button>
    </div>
    <div class="ps" style="flex:1">
      <div class="pl">Análisis Guardados</div>
      <div id="pmlist"><div class="no-items">Ninguno aún</div></div>
    </div>
  </div>
</div>
</div>

<!-- MODAL LABEL -->
<div class="mov" id="lmod">
  <div class="modal">
    <h3 id="lmod-title">Etiquetar</h3>
    <p id="lmod-sub">Escribe el nombre para este elemento.</p>
    <input class="mi" type="text" id="linput" placeholder="Callout / nombre...">
    <div class="mrow">
      <button class="mbtn cancel" onclick="closeLmod()">Cancelar</button>
      <button class="mbtn confirm" id="lmod-ok">Colocar ✓</button>
    </div>
  </div>
</div>

<!-- MODAL COLOR PICKER -->
<div class="mov" id="cpmod">
  <div class="modal">
    <h3>Color</h3><p>Elige o personaliza.</p>
    <div class="cpgrid" id="cpgrid"></div>
    <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
      <div class="cp-cs" id="cpcs"><input type="color" oninput="applyCP(this.value)"></div>
      <span style="font-size:10px;color:var(--muted)">Color personalizado</span>
    </div>
    <div class="mrow"><button class="mbtn cancel" onclick="closeCPmod()">Cerrar</button></div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ════════════════════════════════
// CONSTANTS
// ════════════════════════════════
const COLORS=['#e8333a','#ff0000','#ff4d6d','#c9184a','#ff758f',
  '#ff6b35','#ff8c00','#ffa500','#ff6000','#e85d04',
  '#f59e0b','#ffd60a','#ffea00','#f4d03f','#d4ac0d',
  '#22c55e','#00ff00','#39d353','#006400','#a8e063',
  '#3b82f6','#0000ff','#1e90ff','#00b4d8','#023e8a',
  '#a855f7','#7c3aed','#06b6d4','#e040fb','#00ffff',
  '#ffffff','#c0c0c0','#888888','#333333','#000000'];
const TAGCOLS={'Ataque':'#e8333a','Defensa':'#3b82f6','Rotación':'#f59e0b','Clutch':'#a855f7','Drop':'#22c55e','Ring':'#06b6d4'};

// ════════════════════════════════
// STATE
// ════════════════════════════════
let mapIdx=0;
let maps=[0,1,2].map(()=>({name:'—',image:null,iw:0,ih:0,drawings:[],items:[],strats:[]}));

let tool='pen', color='#e8333a', thick=2, opacity=1, eraserR=20;
let phaseId=null, elemId=null;
let gridOn=false;

// Zoom/pan state (canvas-based, no CSS transform)
let zoom=1, panX=0, panY=0;
let isPanning=false, panLast={x:0,y:0};

// Drawing state
let drawing=false, sx=0, sy=0, path=[];
// Select/drag state
let selId=null, dragId=null, dragOff={x:0,y:0};
// Ruler state
let rulerP1=null, rulerActive=false;
// Placement state
let pendPos=null, pendType=null;
let placingPl=null;

// Post-match
let pmRating=0, pmReports=[];
// Strat tags
let saveTags=new Set(['Ataque']), filterTag='Todos';
// Color pick target
let cpTarget=null;
// Loaded map image object (for redraw)
let mapImages=[null,null,null];

let phases=[
  {id:'p1',name:'Drop',color:'#e8333a'},
  {id:'p2',name:'Rotación Early',color:'#f59e0b'},
  {id:'p3',name:'Mid Game',color:'#3b82f6'},
  {id:'p4',name:'Late Game',color:'#22c55e'},
];
let elements=[
  {id:'e1',name:'Caja Suministros',icon:'📦',img:null},
  {id:'e2',name:'Enemigo',icon:'🔴',img:null},
  {id:'e3',name:'Punto de Interés',icon:'⭐',img:null},
  {id:'e4',name:'Defensa',icon:'🛡️',img:null},
  {id:'e5',name:'Sniper Spot',icon:'🎯',img:null},
  {id:'e6',name:'Heal',icon:'💊',img:null},
  {id:'e7',name:'Zona Combate',icon:'💥',img:null},
  {id:'e8',name:'Vehículo',icon:'🚗',img:null},
];
let players=[
  {id:'pl1',name:'IGL',color:'#e8333a'},
  {id:'pl2',name:'Soporte',color:'#3b82f6'},
  {id:'pl3',name:'Flanker',color:'#22c55e'},
  {id:'pl4',name:'Slayer',color:'#f59e0b'},
];

// ════════════════════════════════
// CANVAS — single canvas, everything drawn on it
// ════════════════════════════════
const cw=document.getElementById('cw');
const canvas=document.getElementById('main-canvas');
const ctx=canvas.getContext('2d');

function resizeCanvas(){
  canvas.width=cw.clientWidth;
  canvas.height=cw.clientHeight;
  redraw();
}
window.addEventListener('resize',resizeCanvas);
setTimeout(resizeCanvas,50);

// ════════════════════════════════
// COORDINATE HELPERS
// ════════════════════════════════
// Screen → world (canvas drawing) coords
function s2w(sx,sy){
  return {x:(sx-panX)/zoom, y:(sy-panY)/zoom};
}
// World → screen coords
function w2s(wx,wy){
  return {x:wx*zoom+panX, y:wy*zoom+panY};
}
// Get event position relative to canvas element
function evPos(e){
  const r=canvas.getBoundingClientRect();
  const src=e.touches?e.touches[0]:e;
  return {x:src.clientX-r.left, y:src.clientY-r.top};
}

// ════════════════════════════════
// ZOOM & PAN — all canvas-based
// ════════════════════════════════
cw.addEventListener('wheel',e=>{
  e.preventDefault();
  if(e.ctrlKey||e.metaKey){
    // ZOOM with Ctrl+scroll
    const sp=evPos(e);
    const factor=e.deltaY<0?1.12:1/1.12;
    const newZoom=Math.min(4,Math.max(0.2,zoom*factor));
    panX=sp.x-(sp.x-panX)*(newZoom/zoom);
    panY=sp.y-(sp.y-panY)*(newZoom/zoom);
    zoom=newZoom;
  } else {
    // PAN with scroll (no drawing conflict — wheel is never a draw event)
    panX-=e.deltaX*0.8;
    panY-=e.deltaY*0.8;
  }
  document.getElementById('zpct').textContent=Math.round(zoom*100)+'%';
  redraw();
},{passive:false});

// Middle mouse button pan
canvas.addEventListener('mousedown',e=>{
  if(e.button===2){e.preventDefault();return;}
  if(e.button===1){
    isPanning=true; panLast=evPos(e); e.preventDefault(); return;
  }
  onDown(e);
});
window.addEventListener('mousemove',e=>{
  if(isPanning){
    const p=evPos(e);
    panX+=p.x-panLast.x; panY+=p.y-panLast.y;
    panLast=p;
    document.getElementById('zpct').textContent=Math.round(zoom*100)+'%';
    redraw(); return;
  }
  onMove(e);
});
window.addEventListener('mouseup',e=>{
  if(isPanning&&e.button===1){isPanning=false;return;}
  onUp(e);
});
canvas.addEventListener('mouseleave',e=>{
  if(!isPanning) onUp(e);
  hideEraserRing();
  document.getElementById('ruler-tip').style.display='none';
});
canvas.addEventListener('touchstart',e=>{e.preventDefault();onDown(e);},{passive:false});
canvas.addEventListener('touchmove',e=>{e.preventDefault();onMove(e);},{passive:false});
canvas.addEventListener('touchend',onUp);
canvas.addEventListener('contextmenu',e=>e.preventDefault());

function zoomIn(){const c={x:canvas.width/2,y:canvas.height/2};const nz=Math.min(4,zoom*1.2);panX=c.x-(c.x-panX)*(nz/zoom);panY=c.y-(c.y-panY)*(nz/zoom);zoom=nz;document.getElementById('zpct').textContent=Math.round(zoom*100)+'%';redraw();}
function zoomOut(){const c={x:canvas.width/2,y:canvas.height/2};const nz=Math.max(0.2,zoom/1.2);panX=c.x-(c.x-panX)*(nz/zoom);panY=c.y-(c.y-panY)*(nz/zoom);zoom=nz;document.getElementById('zpct').textContent=Math.round(zoom*100)+'%';redraw();}
function zoomReset(){zoom=1;panX=0;panY=0;document.getElementById('zpct').textContent='100%';redraw();}

// ════════════════════════════════
// POINTER EVENTS
// ════════════════════════════════
function onDown(e){
  if(e.button&&e.button!==0) return;
  const sp=evPos(e);
  const wp=s2w(sp.x,sp.y);

  // Place player
  if(placingPl){
    const pl=players.find(p=>p.id===placingPl);
    if(pl){
      maps[mapIdx].items.push({id:Date.now(),type:'player',x:wp.x,y:wp.y,icon:'👤',name:pl.name,label:pl.name,color:pl.color,playerId:pl.id});
      redraw();updItems();
    }
    placingPl=null; renderPlayers();
    showToast('✅ '+pl.name+' colocado');
    return;
  }

  if(tool==='utility'){openLmod(wp,'element');return;}
  if(tool==='text'){openLmod(wp,'text');return;}

  if(tool==='ruler'){
    rulerP1=wp; rulerActive=true; return;
  }

  if(tool==='select'){
    const items=maps[mapIdx].items;
    for(let i=items.length-1;i>=0;i--){
      const it=items[i];
      const ss=w2s(it.x,it.y);
      if(dist(sp.x,sp.y,ss.x,ss.y)<20*zoom){
        dragId=it.id; dragOff={x:wp.x-it.x,y:wp.y-it.y};
        selId=it.id; redraw(); return;
      }
    }
    selId=null; redraw(); return;
  }

  drawing=true; sx=wp.x; sy=wp.y; path=[wp];
  if(tool==='eraser') eraseAt(wp,sp);
}

function onMove(e){
  const sp=evPos(e);
  const wp=s2w(sp.x,sp.y);

  if(tool==='eraser'){
    showEraserRing(sp);
    if(drawing) eraseAt(wp,sp);
    return;
  }
  hideEraserRing();

  if(tool==='ruler'&&rulerActive&&rulerP1){
    redraw();
    // Draw live ruler on top
    ctx.save();
    ctx.strokeStyle='#ffd60a'; ctx.lineWidth=2; ctx.setLineDash([6,4]);
    ctx.beginPath();
    const s1=w2s(rulerP1.x,rulerP1.y);
    ctx.moveTo(s1.x,s1.y); ctx.lineTo(sp.x,sp.y); ctx.stroke();
    [s1,sp].forEach(p=>{ctx.fillStyle='#ffd60a';ctx.beginPath();ctx.arc(p.x,p.y,4,0,Math.PI*2);ctx.fill();});
    ctx.setLineDash([]);
    const d=Math.round(Math.sqrt((wp.x-rulerP1.x)**2+(wp.y-rulerP1.y)**2));
    const mx=(s1.x+sp.x)/2, my=(s1.y+sp.y)/2;
    ctx.font='bold 11px Segoe UI'; ctx.fillStyle='#ffd60a'; ctx.textAlign='center';
    ctx.shadowColor='rgba(0,0,0,.9)'; ctx.shadowBlur=4;
    ctx.fillText(d+' u',mx,my-8); ctx.shadowBlur=0;
    ctx.restore();
    // Tooltip
    const tip=document.getElementById('ruler-tip');
    tip.style.display='block'; tip.style.left=(sp.x+14)+'px'; tip.style.top=(sp.y-28)+'px';
    tip.textContent='📏 '+d+' unidades';
    return;
  }
  document.getElementById('ruler-tip').style.display='none';

  if(tool==='select'&&dragId){
    const it=maps[mapIdx].items.find(i=>i.id===dragId);
    if(it){it.x=wp.x-dragOff.x; it.y=wp.y-dragOff.y; redraw();}
    return;
  }

  if(!drawing) return;

  if(tool==='pen'){
    path.push(wp);
    // Incremental draw
    if(path.length>=2){
      const p=path[path.length-2],c=path[path.length-1];
      const ps=w2s(p.x,p.y), cs=w2s(c.x,c.y);
      ctx.save();ctx.globalAlpha=opacity;ctx.strokeStyle=color;ctx.lineWidth=thick*zoom;ctx.lineCap='round';ctx.lineJoin='round';
      ctx.beginPath();ctx.moveTo(ps.x,ps.y);ctx.lineTo(cs.x,cs.y);ctx.stroke();ctx.restore();
    }
    return;
  }

  // Shape preview
  redraw();
  ctx.save();ctx.globalAlpha=opacity;ctx.strokeStyle=color;ctx.lineWidth=thick*zoom;ctx.lineCap='round';
  const ss=w2s(sx,sy);
  drawShape(ctx,ss.x,ss.y,sp.x,sp.y,tool,color,thick*zoom,opacity);
  ctx.restore();
}

function onUp(e){
  if(tool==='ruler'&&rulerActive){
    rulerActive=false;
    if(rulerP1){
      const sp=evPos(e);
      const wp=s2w(sp.x,sp.y);
      maps[mapIdx].drawings.push({type:'ruler',x1:rulerP1.x,y1:rulerP1.y,x2:wp.x,y2:wp.y,color:color,thick:thick,op:opacity});
      rulerP1=null; redraw();
    }
    document.getElementById('ruler-tip').style.display='none';
    return;
  }
  if(tool==='select'&&dragId){dragId=null;return;}
  if(!drawing){drawing=false;return;}
  drawing=false;

  if(tool==='pen'){
    if(path.length>=2) maps[mapIdx].drawings.push({type:'pen',path:[...path],color,thick,op:opacity,phase:phaseId});
    path=[];
    redraw();
  } else if(['arrow','circle','rect'].includes(tool)){
    const sp=e.touches?evPos({clientX:sx*zoom+panX,clientY:sy*zoom+panY}):evPos(e);
    const wp=s2w(sp.x,sp.y);
    maps[mapIdx].drawings.push({type:tool,x1:sx,y1:sy,x2:wp.x,y2:wp.y,color,thick,op:opacity,phase:phaseId});
    redraw();
  }
}

function dist(x1,y1,x2,y2){return Math.sqrt((x1-x2)**2+(y1-y2)**2);}

// ════════════════════════════════
// ERASER
// ════════════════════════════════
function eraseAt(wp,sp){
  const r=eraserR/zoom; // eraser radius in world coords
  const map=maps[mapIdx];
  map.drawings=map.drawings.filter(d=>{
    if(d.type==='pen') return !d.path.some(p=>dist(p.x,p.y,wp.x,wp.y)<r);
    if(d.type==='ruler') return dist((d.x1+d.x2)/2,(d.y1+d.y2)/2,wp.x,wp.y)>=r*2;
    return dist((d.x1+d.x2)/2,(d.y1+d.y2)/2,wp.x,wp.y)>=r*2;
  });
  map.items=map.items.filter(it=>dist(it.x,it.y,wp.x,wp.y)>=r);
  redraw(); updItems();
}
function showEraserRing(sp){
  const ring=document.getElementById('eraser-ring');
  const sz=eraserR*2+'px';
  ring.style.display='block'; ring.style.width=sz; ring.style.height=sz;
  ring.style.left=sp.x+'px'; ring.style.top=sp.y+'px';
}
function hideEraserRing(){document.getElementById('eraser-ring').style.display='none';}

// ════════════════════════════════
// SHAPES
// ════════════════════════════════
function drawShape(ctx,x1,y1,x2,y2,type,col,lw,op){
  ctx.beginPath();
  if(type==='arrow'){
    ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
    arrowHead(ctx,x1,y1,x2,y2,col,lw,op);
  } else if(type==='circle'){
    const rx=Math.abs(x2-x1)/2,ry=Math.abs(y2-y1)/2;
    ctx.ellipse((x1+x2)/2,(y1+y2)/2,Math.max(1,rx),Math.max(1,ry),0,0,Math.PI*2);ctx.stroke();
  } else if(type==='rect'){
    ctx.strokeRect(x1,y1,x2-x1,y2-y1);
  }
}
function arrowHead(ctx,x1,y1,x2,y2,col,lw,op){
  const ang=Math.atan2(y2-y1,x2-x1), sz=Math.max(10,lw*3);
  ctx.save();ctx.fillStyle=col;ctx.globalAlpha=op;
  ctx.beginPath();ctx.moveTo(x2,y2);
  ctx.lineTo(x2-sz*Math.cos(ang-Math.PI/6),y2-sz*Math.sin(ang-Math.PI/6));
  ctx.lineTo(x2-sz*Math.cos(ang+Math.PI/6),y2-sz*Math.sin(ang+Math.PI/6));
  ctx.closePath();ctx.fill();ctx.restore();
}

// ════════════════════════════════
// MAIN REDRAW
// ════════════════════════════════
function redraw(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
  ctx.save();

  // Background
  ctx.fillStyle='#0d1117';
  ctx.fillRect(0,0,canvas.width,canvas.height);

  // Map image
  const img=mapImages[mapIdx];
  if(img&&img.complete){
    const map=maps[mapIdx];
    ctx.drawImage(img,panX,panY,map.iw*zoom,map.ih*zoom);
  }

  // Grid
  if(gridOn){
    const step=40*zoom;
    const ox=((panX%step)+step)%step;
    const oy=((panY%step)+step)%step;
    ctx.strokeStyle='rgba(255,255,255,0.07)';ctx.lineWidth=0.5;
    for(let x=ox;x<canvas.width;x+=step){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,canvas.height);ctx.stroke();}
    for(let y=oy;y<canvas.height;y+=step){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(canvas.width,y);ctx.stroke();}
  }

  // Drawings
  for(const d of maps[mapIdx].drawings) drawDrawing(d);

  // Items
  for(const it of maps[mapIdx].items) drawItem(it);

  ctx.restore();
}

function drawDrawing(d){
  ctx.save();
  ctx.globalAlpha=d.op||1;ctx.strokeStyle=d.color;ctx.fillStyle=d.color;
  ctx.lineWidth=d.thick*zoom;ctx.lineCap='round';ctx.lineJoin='round';

  if(d.type==='pen'){
    if(d.path.length<2){ctx.restore();return;}
    ctx.beginPath();
    const p0=w2s(d.path[0].x,d.path[0].y);
    ctx.moveTo(p0.x,p0.y);
    for(let i=1;i<d.path.length;i++){const p=w2s(d.path[i].x,d.path[i].y);ctx.lineTo(p.x,p.y);}
    ctx.stroke();
  } else if(d.type==='arrow'){
    const s=w2s(d.x1,d.y1),e=w2s(d.x2,d.y2);
    ctx.beginPath();ctx.moveTo(s.x,s.y);ctx.lineTo(e.x,e.y);ctx.stroke();
    arrowHead(ctx,s.x,s.y,e.x,e.y,d.color,d.thick*zoom,d.op||1);
  } else if(d.type==='circle'){
    const s=w2s(d.x1,d.y1),e=w2s(d.x2,d.y2);
    const rx=Math.abs(e.x-s.x)/2,ry=Math.abs(e.y-s.y)/2;
    ctx.beginPath();ctx.ellipse((s.x+e.x)/2,(s.y+e.y)/2,Math.max(1,rx),Math.max(1,ry),0,0,Math.PI*2);
    const a=ctx.globalAlpha;ctx.globalAlpha=a*.2;ctx.fill();ctx.globalAlpha=a;ctx.stroke();
  } else if(d.type==='rect'){
    const s=w2s(d.x1,d.y1),e=w2s(d.x2,d.y2);
    const a=ctx.globalAlpha;ctx.globalAlpha=a*.15;ctx.fillRect(s.x,s.y,e.x-s.x,e.y-s.y);
    ctx.globalAlpha=a;ctx.strokeRect(s.x,s.y,e.x-s.x,e.y-s.y);
  } else if(d.type==='ruler'){
    const s=w2s(d.x1,d.y1),e=w2s(d.x2,d.y2);
    ctx.save();ctx.strokeStyle='#ffd60a';ctx.lineWidth=1.5;ctx.setLineDash([6,4]);ctx.globalAlpha=.6;
    ctx.beginPath();ctx.moveTo(s.x,s.y);ctx.lineTo(e.x,e.y);ctx.stroke();
    const dx=d.x2-d.x1,dy=d.y2-d.y1,dd=Math.round(Math.sqrt(dx*dx+dy*dy));
    ctx.setLineDash([]);ctx.globalAlpha=1;
    ctx.font=`bold ${Math.round(10*zoom)}px Segoe UI`;ctx.fillStyle='#ffd60a';ctx.textAlign='center';
    ctx.shadowColor='rgba(0,0,0,.9)';ctx.shadowBlur=4;
    ctx.fillText(dd+' u',(s.x+e.x)/2,(s.y+e.y)/2-8*zoom);ctx.shadowBlur=0;
    ctx.restore();
  }
  ctx.restore();
}

function drawItem(it){
  const sp=w2s(it.x,it.y);
  const r=18*zoom;
  ctx.save();
  const sel=it.id===selId;
  ctx.fillStyle=sel?'rgba(232,51,58,0.5)':'rgba(0,0,0,0.7)';
  ctx.beginPath();ctx.arc(sp.x,sp.y,r,0,Math.PI*2);ctx.fill();
  ctx.strokeStyle=it.color||'#fff';ctx.lineWidth=sel?2.5:1.5;ctx.stroke();

  if(it.imgData){
    const img=new Image();img.src=it.imgData;
    ctx.save();ctx.beginPath();ctx.arc(sp.x,sp.y,r*.78,0,Math.PI*2);ctx.clip();
    ctx.drawImage(img,sp.x-r*.78,sp.y-r*.78,r*1.56,r*1.56);ctx.restore();
  } else if(it.type==='player'){
    ctx.font=`bold ${Math.round(10*zoom)}px Segoe UI,system-ui`;
    ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillStyle='#fff';
    ctx.fillText((it.label||'?').substring(0,3).toUpperCase(),sp.x,sp.y);
  } else {
    ctx.font=`${Math.round(13*zoom)}px serif`;
    ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillStyle='#fff';
    ctx.fillText(it.icon||'📍',sp.x,sp.y);
  }
  if(it.label){
    ctx.font=`bold ${Math.round(9*zoom)}px Segoe UI,system-ui,sans-serif`;
    ctx.textAlign='center';ctx.textBaseline='top';
    ctx.shadowColor='rgba(0,0,0,0.95)';ctx.shadowBlur=5;
    ctx.fillStyle=it.color||'#fff';
    ctx.fillText(it.label,sp.x,sp.y+r+2);ctx.shadowBlur=0;
  }
  ctx.restore();
}

// ════════════════════════════════
// TOOL CONTROLS
// ════════════════════════════════
function setTool(t){
  tool=t;
  document.querySelectorAll('.tbtn').forEach(b=>b.classList.remove('active'));
  document.getElementById('tool-'+t)?.classList.add('active');
  const cursors={utility:'copy',eraser:'none',select:'move',ruler:'cell'};
  canvas.style.cursor=cursors[t]||'crosshair';
}
function setColor(c){
  color=c;
  document.querySelectorAll('.cdot').forEach(d=>d.classList.remove('active'));
  document.querySelector(`[data-c="${CSS.escape(c)}"]`)?.classList.add('active');
  document.getElementById('cprev').style.background=c;
  document.getElementById('cswatch').style.background=c;
}
function setThick(t,n){thick=t;document.querySelectorAll('.tkbtn').forEach(b=>b.classList.remove('active'));document.getElementById('tk'+n)?.classList.add('active');}
function setOpacity(v){opacity=v/100;document.getElementById('opval').textContent=v+'%';}
function setEraserSize(v){eraserR=+v;document.getElementById('erval').textContent=v+'px';}

function buildPalette(){
  document.getElementById('cgrid').innerHTML=COLORS.map(c=>`
    <div class="cdot${c===color?' active':''}" style="background:${c}" data-c="${c}" onclick="setColor('${c}')" title="${c}"></div>
  `).join('');
}

// ════════════════════════════════
// GRID
// ════════════════════════════════
function togGrid(){
  gridOn=!gridOn;
  document.getElementById('gbtn').classList.toggle('act',gridOn);
  redraw();
}

// ════════════════════════════════
// PHASES
// ════════════════════════════════
function renderPhases(){
  document.getElementById('phase-list').innerHTML=phases.map(ph=>`
    <div class="phase-row">
      <div class="phase-dot" style="background:${ph.color}" onclick="pickColor('phase','${ph.id}')"></div>
      <input class="phase-name-inp" value="${ph.name}" onchange="renamePhase('${ph.id}',this.value)" placeholder="Nombre">
      <button class="phase-sel${phaseId===ph.id?' active':''}" onclick="selPhase('${ph.id}')">▶</button>
      <button class="xbtn" onclick="delPhase('${ph.id}')">✕</button>
    </div>
  `).join('')+`<button class="add-btn" onclick="addPhase()">+ Nueva Fase</button>`;
}
function selPhase(id){phaseId=id;const ph=phases.find(p=>p.id===id);if(ph)setColor(ph.color);renderPhases();}
function renamePhase(id,v){const p=phases.find(x=>x.id===id);if(p)p.name=v;}
function delPhase(id){phases=phases.filter(p=>p.id!==id);if(phaseId===id)phaseId=null;renderPhases();}
function addPhase(){const cols=['#e8333a','#f59e0b','#22c55e','#3b82f6','#a855f7','#06b6d4'];phases.push({id:'p'+Date.now(),name:'Nueva Fase',color:cols[phases.length%cols.length]});renderPhases();}

// ════════════════════════════════
// ELEMENTS
// ════════════════════════════════
function renderElements(){
  document.getElementById('elem-list').innerHTML=elements.map(el=>`
    <div class="elem-row${elemId===el.id?' active':''}" onclick="selElem('${el.id}')">
      <div class="elem-img" title="Subir imagen">
        ${el.img?`<img src="${el.img}" alt="">`:`<span>${el.icon}</span>`}
        <input type="file" accept="image/*" onchange="setElemImg('${el.id}',event)" onclick="event.stopPropagation()">
      </div>
      <input class="elem-name-inp" value="${el.name}" onchange="renameElem('${el.id}',this.value)" onclick="event.stopPropagation()" placeholder="Nombre">
      <button class="xbtn" onclick="delElem('${el.id}');event.stopPropagation()">✕</button>
    </div>
  `).join('')+`<button class="add-btn" onclick="addElement()">+ Nuevo Elemento</button>`;
}
function selElem(id){elemId=id;setTool('utility');renderElements();}
function renameElem(id,v){const el=elements.find(e=>e.id===id);if(el)el.name=v;}
function delElem(id){elements=elements.filter(e=>e.id!==id);if(elemId===id)elemId=null;renderElements();}
function addElement(){elements.push({id:'e'+Date.now(),name:'Elemento',icon:'📍',img:null});renderElements();}
function setElemImg(id,ev){const f=ev.target.files[0];if(!f)return;const r=new FileReader();r.onload=e=>{const el=elements.find(x=>x.id===id);if(el){el.img=e.target.result;renderElements();}};r.readAsDataURL(f);ev.target.value='';}

// ════════════════════════════════
// PLAYERS
// ════════════════════════════════
function renderPlayers(){
  document.getElementById('player-list').innerHTML=players.map(pl=>`
    <div class="player-row">
      <div class="player-dot" style="background:${pl.color}" onclick="pickColor('player','${pl.id}')"></div>
      <input class="player-name-inp" value="${pl.name}" onchange="renamePlayer('${pl.id}',this.value)" placeholder="Nombre">
      <button class="place-btn${placingPl===pl.id?' placing':''}" onclick="togPlacePl('${pl.id}')">📍</button>
      <button class="xbtn" onclick="delPlayer('${pl.id}')">✕</button>
    </div>
  `).join('')+`<button class="add-btn green" onclick="addPlayer()">+ Agregar Jugador</button>`;
}
function renamePlayer(id,v){const pl=players.find(p=>p.id===id);if(!pl)return;pl.name=v;maps[mapIdx].items.filter(it=>it.playerId===id).forEach(it=>it.label=v);redraw();}
function delPlayer(id){players=players.filter(p=>p.id!==id);if(placingPl===id)placingPl=null;renderPlayers();}
function addPlayer(){const pal=['#e8333a','#3b82f6','#22c55e','#f59e0b','#a855f7','#06b6d4','#ff6b35','#fff'];players.push({id:'pl'+Date.now(),name:'Jugador '+(players.length+1),color:pal[players.length%pal.length]});renderPlayers();}
function togPlacePl(id){placingPl=(placingPl===id)?null:id;renderPlayers();if(placingPl)showToast('📍 Click en el mapa para colocar al jugador');}

// ════════════════════════════════
// COLOR PICKER MODAL
// ════════════════════════════════
function pickColor(type,id){
  cpTarget={type,id};
  const curr=type==='phase'?phases.find(p=>p.id===id)?.color:players.find(p=>p.id===id)?.color;
  document.getElementById('cpgrid').innerHTML=COLORS.map(c=>`<div class="cpdot${c===curr?' active':''}" style="background:${c}" onclick="applyCP('${c}')"></div>`).join('');
  document.getElementById('cpcs').style.background=curr||'#fff';
  document.getElementById('cpmod').classList.add('open');
}
function applyCP(c){
  if(!cpTarget)return;
  if(cpTarget.type==='phase'){const p=phases.find(x=>x.id===cpTarget.id);if(p)p.color=c;renderPhases();}
  else{const pl=players.find(x=>x.id===cpTarget.id);if(pl)pl.color=c;maps[mapIdx].items.filter(it=>it.playerId===cpTarget.id).forEach(it=>it.color=c);redraw();renderPlayers();}
  closeCPmod();
}
function closeCPmod(){document.getElementById('cpmod').classList.remove('open');cpTarget=null;}

// ════════════════════════════════
// PLACEMENT MODAL
// ════════════════════════════════
function openLmod(pos,type){
  pendPos=pos;pendType=type;
  if(type==='element'){
    if(!elemId){showToast('⚠️ Selecciona un elemento');return;}
    const el=elements.find(e=>e.id===elemId);if(!el)return;
    document.getElementById('lmod-title').textContent='Etiquetar: '+el.name;
    document.getElementById('lmod-sub').textContent='Callout o deja el nombre por defecto.';
    document.getElementById('linput').value=el.name;
  } else {
    document.getElementById('lmod-title').textContent='Agregar Texto';
    document.getElementById('lmod-sub').textContent='Escribe el texto que aparecerá en el mapa.';
    document.getElementById('linput').value='';
  }
  document.getElementById('lmod').classList.add('open');
  setTimeout(()=>document.getElementById('linput').select(),80);
}
function closeLmod(){document.getElementById('lmod').classList.remove('open');pendPos=null;pendType=null;}
document.getElementById('lmod-ok').onclick=function(){
  const txt=document.getElementById('linput').value.trim();
  if(!pendPos)return;
  if(pendType==='text'){
    if(!txt){closeLmod();return;}
    maps[mapIdx].items.push({id:Date.now(),type:'text',x:pendPos.x,y:pendPos.y,icon:'📝',imgData:null,name:'Texto',label:txt,color});
  } else {
    const el=elements.find(e=>e.id===elemId);
    maps[mapIdx].items.push({id:Date.now(),type:'element',x:pendPos.x,y:pendPos.y,icon:el?.icon||'📍',imgData:el?.img||null,name:el?.name||'',label:txt||el?.name||'',color,elemId});
  }
  redraw();updItems();closeLmod();
};
document.getElementById('linput').addEventListener('keydown',e=>{
  if(e.key==='Enter')document.getElementById('lmod-ok').click();
  if(e.key==='Escape')closeLmod();
});

// ════════════════════════════════
// MAPS
// ════════════════════════════════
function selMap(idx){mapIdx=idx;document.querySelectorAll('.map-tab').forEach((t,i)=>t.classList.toggle('active',i===idx));loadMap2();}
function loadMap2(){
  const map=maps[mapIdx];
  document.getElementById('map-ph').style.display=map.image?'none':'flex';
  redraw();updItems();renderStrats();
}
function trigUpload(){document.getElementById('mapfile').click();}
function loadMap(ev){
  const f=ev.target.files[0];if(!f)return;
  const reader=new FileReader();
  reader.onload=e=>{
    const img=new Image();
    img.onload=()=>{
      const W=cw.clientWidth,H=cw.clientHeight;
      const sc=Math.min(W/img.naturalWidth,H/img.naturalHeight,1);
      maps[mapIdx].image=e.target.result;
      maps[mapIdx].iw=img.naturalWidth*sc;
      maps[mapIdx].ih=img.naturalHeight*sc;
      maps[mapIdx].name=f.name.replace(/\.[^.]+$/,'');
      mapImages[mapIdx]=img;
      // Center
      panX=(W-maps[mapIdx].iw)/2;panY=(H-maps[mapIdx].ih)/2;zoom=1;
      document.getElementById('zpct').textContent='100%';
      document.getElementById('mn'+mapIdx).textContent=maps[mapIdx].name;
      document.getElementById('map-ph').style.display='none';
      redraw();
      showToast('✅ Mapa cargado: '+maps[mapIdx].name);
    };
    img.src=e.target.result;
  };
  reader.readAsDataURL(f);ev.target.value='';
}

// ════════════════════════════════
// CLEAR / UNDO
// ════════════════════════════════
function doClear(){if(!confirm('¿Limpiar todo?'))return;maps[mapIdx].drawings=[];maps[mapIdx].items=[];selId=null;redraw();updItems();showToast('🗑 Limpiado');}
function doUndo(){const m=maps[mapIdx];if(m.drawings.length>0){m.drawings.pop();redraw();}else if(m.items.length>0){m.items.pop();selId=null;redraw();updItems();}}

// ════════════════════════════════
// ITEMS LIST
// ════════════════════════════════
function updItems(){
  const list=document.getElementById('ilist');
  const items=maps[mapIdx].items;
  if(!items.length){list.innerHTML='<div class="no-items">Ninguno</div>';return;}
  list.innerHTML=items.map(it=>`
    <div class="ientry">
      <span style="color:${it.color||'#fff'}">${it.icon||'📍'}</span>
      <span style="overflow:hidden;text-overflow:ellipsis;white-space:nowrap;max-width:95px">${it.label||it.name}</span>
      <button class="idel" onclick="delItem(${it.id})">✕</button>
    </div>
  `).join('');
}
function delItem(id){maps[mapIdx].items=maps[mapIdx].items.filter(i=>i.id!==id);if(selId===id)selId=null;redraw();updItems();}

// ════════════════════════════════
// STRATEGY TAGS
// ════════════════════════════════
function togSaveTag(el){const t=el.dataset.tag;if(saveTags.has(t))saveTags.delete(t);else saveTags.add(t);el.classList.toggle('active',saveTags.has(t));}
function setFTag(el,t){filterTag=t;document.querySelectorAll('#ftags .tag').forEach(x=>x.classList.remove('active'));el.classList.add('active');renderStrats();}

// ════════════════════════════════
// STRATEGIES
// ════════════════════════════════
function saveStrat(){
  const name=document.getElementById('sname').value.trim();
  if(!name){showToast('⚠️ Escribe un nombre');return;}
  const notes=document.getElementById('strat-notes').value;
  const tags=[...saveTags];
  maps[mapIdx].strats.push({
    id:Date.now(),name,notes,tags,mi:mapIdx,
    drawings:JSON.parse(JSON.stringify(maps[mapIdx].drawings)),
    items:JSON.parse(JSON.stringify(maps[mapIdx].items)),
    ts:new Date().toLocaleString('es-MX',{month:'short',day:'numeric',hour:'2-digit',minute:'2-digit'})
  });
  document.getElementById('sname').value='';
  renderStrats();showToast('💾 Guardada: '+name);
}
function loadStrat(mi,id){
  const s=maps[mi].strats.find(x=>x.id===id);if(!s)return;
  maps[mapIdx].drawings=JSON.parse(JSON.stringify(s.drawings));
  maps[mapIdx].items=JSON.parse(JSON.stringify(s.items));
  if(s.notes)document.getElementById('strat-notes').value=s.notes;
  selId=null;redraw();updItems();showToast('📂 Cargada: '+s.name);
}
function delStrat(mi,id){maps[mi].strats=maps[mi].strats.filter(s=>s.id!==id);renderStrats();showToast('🗑 Eliminada');}
function renderStrats(){
  const list=document.getElementById('slist');
  const q=(document.getElementById('ssearch').value||'').toLowerCase();
  let all=maps.flatMap((m,mi)=>m.strats.map(s=>({...s,_mi:mi})));
  if(filterTag!=='Todos')all=all.filter(s=>(s.tags||[]).includes(filterTag));
  if(q)all=all.filter(s=>s.name.toLowerCase().includes(q)||(s.notes||'').toLowerCase().includes(q));
  if(!all.length){list.innerHTML='<div class="no-items">Ninguna'+(q||filterTag!=='Todos'?' encontrada':' aún')+'</div>';return;}
  list.innerHTML=all.map(s=>`
    <div class="scard">
      <div class="scard-name">${s.name}</div>
      <div class="scard-meta">Mapa ${s._mi+1} · ${s.ts}</div>
      ${(s.tags||[]).length?`<div class="scard-tags">${s.tags.map(t=>`<span class="stag" style="background:${(TAGCOLS[t]||'#444')}22;color:${TAGCOLS[t]||'#aaa'};border:1px solid ${(TAGCOLS[t]||'#444')}44">${t}</span>`).join('')}</div>`:''}
      <div class="scard-acts">
        <button class="scard-btn" onclick="loadStrat(${s._mi},${s.id})">📂 Cargar</button>
        <button class="scard-btn" onclick="delStrat(${s._mi},${s.id})">🗑</button>
      </div>
    </div>
  `).join('');
}

// ════════════════════════════════
// POST-MATCH
// ════════════════════════════════
function setPmR(n){pmRating=n;document.querySelectorAll('.pm-star').forEach((s,i)=>s.classList.toggle('active',i<n));}
function savePm(){
  const match=document.getElementById('pm-match').value.trim();
  if(!match){showToast('⚠️ Escribe el nombre del partido');return;}
  pmReports.push({id:Date.now(),match,good:document.getElementById('pm-good').value,bad:document.getElementById('pm-bad').value,improve:document.getElementById('pm-improve').value,rating:pmRating,ts:new Date().toLocaleString('es-MX',{month:'short',day:'numeric',hour:'2-digit',minute:'2-digit'})});
  ['pm-match','pm-good','pm-bad','pm-improve'].forEach(id=>document.getElementById(id).value='');
  setPmR(0);renderPm();showToast('📋 Análisis guardado');
}
function delPm(id){pmReports=pmReports.filter(r=>r.id!==id);renderPm();}
function renderPm(){
  const list=document.getElementById('pmlist');
  if(!pmReports.length){list.innerHTML='<div class="no-items">Ninguno aún</div>';return;}
  list.innerHTML=[...pmReports].reverse().map(r=>`
    <div class="pm-card">
      <button class="pm-del" onclick="delPm(${r.id})">✕</button>
      <div class="pm-card-title">${r.match} ${'★'.repeat(r.rating)}</div>
      <div class="pm-card-meta">${r.ts}</div>
      <div class="pm-card-row">
        <div class="pm-col"><div class="pm-col-lbl">✅ Bien</div><div class="pm-col-val">${r.good||'—'}</div></div>
        <div class="pm-col"><div class="pm-col-lbl">❌ Mal</div><div class="pm-col-val">${r.bad||'—'}</div></div>
      </div>
      ${r.improve?`<div style="margin-top:3px"><div class="pm-col-lbl">🔄</div><div class="pm-col-val">${r.improve}</div></div>`:''}
    </div>
  `).join('');
}

// ════════════════════════════════
// TABS
// ════════════════════════════════
function showTab(name){
  const names=['strats','players','postmatch'];
  document.querySelectorAll('.rtab').forEach((t,i)=>t.classList.toggle('active',names[i]===name));
  document.querySelectorAll('.rpanel').forEach(p=>p.classList.remove('active'));
  document.getElementById('rtab-'+name).classList.add('active');
}

// ════════════════════════════════
// EXPORT PNG
// ════════════════════════════════
function doExport(){
  const ex=document.createElement('canvas');ex.width=canvas.width;ex.height=canvas.height;
  const ec=ex.getContext('2d');ec.drawImage(canvas,0,0);
  ec.fillStyle='rgba(255,255,255,0.2)';ec.font='bold 10px Segoe UI';ec.textAlign='right';
  ec.fillText('Blood Strike Strategy Board · CGC ESPORT',ex.width-8,ex.height-6);
  const a=document.createElement('a');a.download='bs-strat-'+Date.now()+'.png';a.href=ex.toDataURL('image/png');a.click();
  showToast('📸 PNG exportado');
}

// ════════════════════════════════
// TOAST
// ════════════════════════════════
function showToast(msg){const t=document.getElementById('toast');t.textContent=msg;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2400);}

// ════════════════════════════════
// KEYBOARD
// ════════════════════════════════
document.addEventListener('keydown',e=>{
  if(['INPUT','TEXTAREA'].includes(e.target.tagName))return;
  const m={p:'pen',a:'arrow',c:'circle',r:'ruler',t:'text',s:'select',e:'eraser',u:'utility'};
  if(m[e.key])setTool(m[e.key]);
  if((e.ctrlKey||e.metaKey)&&e.key==='z'){e.preventDefault();doUndo();}
  if(e.key==='Escape'){closeLmod();closeCPmod();placingPl=null;renderPlayers();}
  if(e.key==='Delete'&&selId){maps[mapIdx].items=maps[mapIdx].items.filter(i=>i.id!==selId);selId=null;redraw();updItems();}
  if(e.key==='g')togGrid();

  if(e.key==='='||e.key==='+')zoomIn();
  if(e.key==='-')zoomOut();
  if(e.key==='0')zoomReset();
});

// ════════════════════════════════
// INIT
// ════════════════════════════════
buildPalette();
renderPhases();
renderElements();
renderPlayers();
renderPm();
setTool('pen');
loadMap2();
if(phases.length)selPhase(phases[0].id);
if(elements.length)elemId=elements[0].id;
</script>
</body>
</html>
