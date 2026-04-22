
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dogfy — Gestão de Sprint</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root {
  --bg:#F7F6F2; --surface:#FFFFFF; --surface2:#F0EFE9;
  --border:rgba(0,0,0,0.08); --border-md:rgba(0,0,0,0.14);
  --text:#1A1917; --text-2:#6B6A66; --text-3:#A09E99;
  --green:#1D9E75; --green-bg:#E1F5EE; --green-text:#085041;
  --amber:#EF9F27; --amber-bg:#FEF3DC; --amber-text:#7A4F0A;
  --red:#E24B4A; --red-bg:#FDEAEA; --red-text:#8B1F1F;
  --blue:#378ADD; --blue-bg:#EBF3FC; --blue-text:#0F4A82;
  --pink-bg:#FBEAF0; --pink-text:#7A2248;
  --gray-bg:#EEEDE8; --gray-text:#4A4946;
  --r:10px; --rl:14px;
  --sh:0 1px 3px rgba(0,0,0,0.06),0 1px 2px rgba(0,0,0,0.04);
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);font-size:14px;line-height:1.5;min-height:100vh;}
.shell{display:flex;height:100vh;}
.sidebar{width:220px;flex-shrink:0;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;padding:24px 0;}
.brand{padding:0 20px 20px;border-bottom:1px solid var(--border);margin-bottom:16px;display:flex;align-items:center;gap:10px;}
.brand-icon{width:32px;height:32px;background:var(--text);border-radius:8px;display:flex;align-items:center;justify-content:center;}
.brand-icon svg{width:18px;height:18px;}
.brand-name{font-size:15px;font-weight:600;letter-spacing:-0.02em;}
.brand-badge{font-size:10px;font-family:'DM Mono',monospace;background:var(--green-bg);color:var(--green-text);padding:2px 6px;border-radius:4px;margin-left:auto;}
.nav-label{font-size:10px;font-weight:600;letter-spacing:0.08em;text-transform:uppercase;color:var(--text-3);padding:0 20px 8px;}
.nav-item{display:flex;align-items:center;gap:10px;padding:9px 20px;cursor:pointer;color:var(--text-2);font-size:13.5px;border-left:2px solid transparent;transition:all 0.12s;user-select:none;}
.nav-item:hover{color:var(--text);background:var(--surface2);}
.nav-item.active{color:var(--text);font-weight:500;background:var(--surface2);border-left:2px solid var(--text);}
.nav-item svg{width:15px;height:15px;opacity:0.6;}
.nav-item.active svg{opacity:1;}
.nav-count{margin-left:auto;font-size:11px;font-family:'DM Mono',monospace;background:var(--gray-bg);color:var(--gray-text);padding:1px 6px;border-radius:4px;}
.sidebar-footer{margin-top:auto;padding:16px 20px 0;border-top:1px solid var(--border);}
.sprint-pill{background:var(--green-bg);color:var(--green-text);border-radius:8px;padding:10px 12px;font-size:12px;}
.sprint-pill strong{display:block;font-weight:600;margin-bottom:2px;}
.sprint-dates{color:var(--green);font-family:'DM Mono',monospace;font-size:11px;}
.main{flex:1;display:flex;flex-direction:column;overflow:hidden;}
.topbar{background:var(--surface);border-bottom:1px solid var(--border);padding:14px 28px;display:flex;align-items:center;gap:12px;}
.topbar-title{font-size:15px;font-weight:600;}
.topbar-sub{font-size:12px;color:var(--text-3);margin-left:4px;}
.topbar-right{margin-left:auto;display:flex;align-items:center;gap:8px;}
.btn{display:inline-flex;align-items:center;gap:6px;padding:7px 14px;border-radius:var(--r);font-size:13px;font-weight:500;cursor:pointer;border:1px solid var(--border-md);background:var(--surface);color:var(--text);transition:all 0.12s;font-family:'DM Sans',sans-serif;}
.btn:hover{background:var(--surface2);}
.btn-primary{background:var(--text);color:#fff;border-color:var(--text);}
.btn-primary:hover{opacity:0.85;}
.btn-sm{padding:5px 10px;font-size:12px;}
.btn-danger{background:var(--red-bg);color:var(--red-text);border-color:rgba(226,75,74,0.2);}
.btn-danger:hover{background:#f7d0d0;}
.content{flex:1;overflow-y:auto;padding:28px;}
.section{display:none;}
.section.active{display:block;}
.kanban-board{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:16px;align-items:start;}
.col-head{display:flex;align-items:center;gap:8px;margin-bottom:12px;padding:0 4px;}
.col-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
.col-name{font-size:12px;font-weight:600;letter-spacing:0.03em;color:var(--text-2);text-transform:uppercase;}
.col-count{margin-left:auto;font-size:11px;font-family:'DM Mono',monospace;background:var(--gray-bg);color:var(--gray-text);padding:1px 6px;border-radius:4px;}
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:12px 14px;margin-bottom:8px;box-shadow:var(--sh);transition:box-shadow 0.12s,transform 0.12s;cursor:pointer;}
.card:hover{box-shadow:0 4px 12px rgba(0,0,0,0.09);transform:translateY(-1px);}
.card-task{font-size:13px;color:var(--text);line-height:1.4;margin-bottom:8px;font-weight:500;}
.card-foot{display:flex;align-items:center;justify-content:space-between;}
.avatar{width:24px;height:24px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:600;flex-shrink:0;}
.av-g{background:var(--blue-bg);color:var(--blue-text);}
.av-a{background:var(--pink-bg);color:var(--pink-text);}
.av-l{background:var(--green-bg);color:var(--green-text);}
.av-d{background:var(--amber-bg);color:var(--amber-text);}
.av-all{background:var(--gray-bg);color:var(--gray-text);}
.badge{font-size:10px;font-weight:600;padding:3px 8px;border-radius:6px;letter-spacing:0.02em;white-space:nowrap;}
.b-todo{background:var(--blue-bg);color:var(--blue-text);}
.b-doing{background:var(--amber-bg);color:var(--amber-text);}
.b-blocked{background:var(--red-bg);color:var(--red-text);}
.b-waiting{background:var(--pink-bg);color:var(--pink-text);}
.b-done{background:var(--green-bg);color:var(--green-text);}
.pbar{height:3px;background:var(--surface2);border-radius:2px;margin:8px 0 4px;}
.pfill{height:3px;border-radius:2px;background:var(--green);}
.card-kpi{font-size:10px;color:var(--text-3);font-family:'DM Mono',monospace;}
.card-deadline{font-size:11px;color:var(--text-3);}
.empty-col{border:1.5px dashed var(--border);border-radius:var(--r);padding:20px 14px;text-align:center;font-size:12px;color:var(--text-3);}
.add-card-btn{display:flex;align-items:center;justify-content:center;gap:6px;width:100%;padding:9px;border:1.5px dashed var(--border-md);border-radius:var(--r);background:none;cursor:pointer;font-size:12px;color:var(--text-3);font-family:'DM Sans',sans-serif;transition:all 0.12s;margin-top:4px;}
.add-card-btn:hover{border-color:var(--green);color:var(--green);background:var(--green-bg);}
.kpi-alert{border-radius:var(--r);padding:14px 16px;margin-bottom:20px;font-size:13px;border:1px solid;display:flex;align-items:flex-start;gap:10px;}
.kpi-alert.warn{background:var(--amber-bg);color:var(--amber-text);border-color:rgba(239,159,39,0.3);}
.kpi-alert.ok{background:var(--green-bg);color:var(--green-text);border-color:rgba(29,158,117,0.3);}
.metrics-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:24px;}
.metric-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:16px;box-shadow:var(--sh);}
.metric-label{font-size:11px;color:var(--text-3);text-transform:uppercase;letter-spacing:0.06em;font-weight:500;margin-bottom:6px;}
.metric-value{font-size:28px;font-weight:600;letter-spacing:-0.02em;}
.metric-sub{font-size:11px;color:var(--text-3);margin-top:4px;font-family:'DM Mono',monospace;}
.kpi-sections{display:grid;grid-template-columns:repeat(2,1fr);gap:16px;}
.kpi-section{background:var(--surface);border:1px solid var(--border);border-radius:var(--rl);padding:18px;box-shadow:var(--sh);}
.kpi-section-title{font-size:13px;font-weight:600;margin-bottom:14px;}
.kpi-row{display:flex;align-items:center;justify-content:space-between;padding:9px 0;border-bottom:1px solid var(--border);gap:10px;}
.kpi-row:last-child{border-bottom:none;}
.kpi-row-name{font-size:12.5px;color:var(--text-2);}
.kpi-row-right{display:flex;align-items:center;gap:6px;flex-shrink:0;}
.status-dot{width:7px;height:7px;border-radius:50%;}
.sd-ok{background:var(--green);}
.sd-warn{background:var(--amber);}
.sd-risk{background:var(--red);}
.sd-pend{background:var(--text-3);}
.kpi-val{font-size:11px;color:var(--text-3);font-family:'DM Mono',monospace;}
.sprint-overview{background:var(--surface);border:1px solid var(--border);border-radius:var(--rl);padding:20px 24px;margin-bottom:20px;box-shadow:var(--sh);}
.sprint-overview-title{font-size:16px;font-weight:600;letter-spacing:-0.01em;margin-bottom:4px;}
.sprint-meta-row{display:flex;gap:20px;font-size:12px;color:var(--text-3);font-family:'DM Mono',monospace;margin-bottom:16px;}
.sprint-pbar-row{display:flex;align-items:center;gap:14px;}
.sprint-pbar{flex:1;height:6px;background:var(--surface2);border-radius:3px;}
.sprint-pfill{height:6px;border-radius:3px;background:var(--green);}
.sprint-ppct{font-size:13px;font-weight:600;font-family:'DM Mono',monospace;min-width:36px;}
.sprint-goal{margin-top:10px;font-size:12px;color:var(--text-2);background:var(--surface2);padding:8px 12px;border-radius:8px;}
.members-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:16px;}
.member-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--rl);overflow:hidden;box-shadow:var(--sh);}
.member-head{padding:14px 18px;background:var(--surface2);display:flex;align-items:center;gap:12px;border-bottom:1px solid var(--border);}
.member-av{width:34px;height:34px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:600;}
.member-name{font-size:14px;font-weight:600;}
.member-role{font-size:11px;color:var(--text-3);margin-top:1px;}
.member-tasks{padding:6px 18px;}
.sprint-task-row{display:flex;align-items:center;gap:8px;padding:9px 0;border-bottom:1px solid var(--border);}
.sprint-task-row:last-child{border-bottom:none;}
.sprint-task-name{font-size:12.5px;flex:1;}
.sprint-task-date{font-size:10px;color:var(--text-3);font-family:'DM Mono',monospace;flex-shrink:0;}
/* MODAL */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.38);z-index:9999;align-items:center;justify-content:center;}
.overlay.open{display:flex;}
.modal{background:var(--surface);border-radius:var(--rl);padding:28px;width:490px;max-width:95vw;box-shadow:0 24px 64px rgba(0,0,0,0.18);position:relative;}
.modal-hd{font-size:16px;font-weight:600;margin-bottom:20px;}
.modal-x{position:absolute;top:16px;right:16px;background:none;border:none;cursor:pointer;color:var(--text-3);font-size:20px;line-height:1;padding:4px 6px;}
.modal-x:hover{color:var(--text);}
.field{margin-bottom:15px;}
.field label{display:block;font-size:11px;font-weight:600;color:var(--text-2);margin-bottom:5px;text-transform:uppercase;letter-spacing:0.05em;}
.field input,.field select,.field textarea{width:100%;padding:9px 12px;border:1px solid var(--border-md);border-radius:var(--r);font-size:13px;font-family:'DM Sans',sans-serif;color:var(--text);background:var(--surface);outline:none;transition:border-color 0.12s;}
.field input:focus,.field select:focus,.field textarea:focus{border-color:var(--green);}
.field-row{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
.range-wrap{display:flex;align-items:center;gap:12px;}
.range-wrap input[type=range]{flex:1;accent-color:var(--green);}
.range-lbl{font-size:13px;font-weight:600;font-family:'DM Mono',monospace;min-width:38px;}
.modal-foot{display:flex;justify-content:flex-end;gap:8px;margin-top:24px;padding-top:20px;border-top:1px solid var(--border);}
::-webkit-scrollbar{width:5px;}
::-webkit-scrollbar-track{background:transparent;}
::-webkit-scrollbar-thumb{background:var(--border-md);border-radius:3px;}
@media(max-width:900px){
  .kanban-board{grid-template-columns:repeat(2,1fr);}
  .metrics-grid{grid-template-columns:repeat(2,1fr);}
  .kpi-sections,.members-grid{grid-template-columns:1fr;}
  .sidebar{width:64px;}
  .brand-name,.nav-label,.nav-item span:not(.nav-count),.sidebar-footer{display:none;}
  .nav-item{justify-content:center;padding:10px;}
  .nav-item svg{width:18px;height:18px;}
}
</style>
</head>
<body>
<div class="shell">
  <aside class="sidebar">
    <div class="brand">
      <div class="brand-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2L2 7l10 5 10-5-10-5z"/><path d="M2 17l10 5 10-5"/><path d="M2 12l10 5 10-5"/></svg>
      </div>
      <span class="brand-name">Dogfy</span>
      <span class="brand-badge">S1</span>
    </div>
    <div class="nav-label">Workspace</div>
    <div class="nav-item active" onclick="switchTab('kanban',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="18" rx="1"/><rect x="14" y="3" width="7" height="10" rx="1"/><rect x="14" y="17" width="7" height="4" rx="1"/></svg>
      Tarefas <span class="nav-count" id="nav-count">—</span>
    </div>
    <div class="nav-item" onclick="switchTab('kpis',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/></svg>
      KPIs
    </div>
    <div class="nav-item" onclick="switchTab('sprints',this)">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>
      Sprints
    </div>
    <div class="sidebar-footer">
      <div class="sprint-pill">
        <strong>Sprint 1 — Ativa</strong>
        <span class="sprint-dates">27/04 → 11/05</span>
      </div>
    </div>
  </aside>

  <div class="main">
    <div class="topbar">
      <div>
        <span class="topbar-title" id="tb-title">Tarefas</span>
        <span class="topbar-sub" id="tb-sub">Sprint 1</span>
      </div>
      <div class="topbar-right">
        <button class="btn" onclick="resetData()" title="Resetar para dados iniciais" style="font-size:12px;padding:6px 10px;color:var(--text-3);">↺ Resetar</button>
        <button class="btn btn-primary" onclick="openNew()">
          <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
          Nova tarefa
        </button>
      </div>
    </div>
    <div class="content">
      <div id="kanban" class="section active"><div class="kanban-board" id="kb"></div></div>
      <div id="kpis" class="section"><div id="kpi-alert"></div><div class="metrics-grid" id="metrics"></div><div class="kpi-sections" id="kpi-sec"></div></div>
      <div id="sprints" class="section">
        <div class="sprint-overview">
          <div class="sprint-overview-title">Sprint 1 — Validação e lançamento</div>
          <div class="sprint-meta-row"><span>Início: 27/04</span><span>Término: 11/05</span><span>Duração: 15 dias</span></div>
          <div class="sprint-pbar-row">
            <div class="sprint-pbar"><div class="sprint-pfill" id="sp-fill" style="width:0%"></div></div>
            <div class="sprint-ppct" id="sp-pct">0%</div>
          </div>
          <div class="sprint-goal">Meta principal: App funcionando para divulgação até 11/05</div>
        </div>
        <div class="members-grid" id="members"></div>
      </div>
    </div>
  </div>
</div>

<!-- MODAL -->
<div class="overlay" id="overlay" onclick="overlayClick(event)">
  <div class="modal">
    <button class="modal-x" onclick="closeModal()">✕</button>
    <div class="modal-hd" id="modal-hd">Tarefa</div>
    <div class="field">
      <label>Nome da tarefa</label>
      <input id="f-task" type="text" placeholder="Descreva a tarefa...">
    </div>
    <div class="field-row">
      <div class="field">
        <label>Responsável</label>
        <select id="f-owner">
          <option value="todos">Todos</option>
          <option value="gustavo">Gustavo</option>
          <option value="ana">Ana</option>
          <option value="luana">Luana</option>
          <option value="dayane">Dayane</option>
        </select>
      </div>
      <div class="field">
        <label>Status</label>
        <select id="f-status">
          <option value="todo">To Do</option>
          <option value="doing">Doing</option>
          <option value="blocked">Blocked</option>
          <option value="waiting">Aguardando</option>
          <option value="done">Concluído ✓</option>
        </select>
      </div>
    </div>
    <div class="field-row">
      <div class="field">
        <label>Início</label>
        <input id="f-start" type="text" placeholder="DD/MM">
      </div>
      <div class="field">
        <label>Prazo</label>
        <input id="f-deadline" type="text" placeholder="DD/MM ou —">
      </div>
    </div>
    <div class="field">
      <label>Progresso — <span id="plbl">0%</span></label>
      <div class="range-wrap">
        <input type="range" id="f-prog" min="0" max="100" step="5" value="0" oninput="document.getElementById('plbl').textContent=this.value+'%'">
        <span class="range-lbl" id="plbl2">0%</span>
      </div>
    </div>
    <div class="field">
      <label>KPI / Meta</label>
      <input id="f-kpi" type="text" placeholder="Ex: 5 roteiros prontos">
    </div>
    <div class="modal-foot">
      <button class="btn btn-danger btn-sm" id="btn-del" onclick="deleteTask()" style="margin-right:auto;display:none;">Excluir tarefa</button>
      <button class="btn btn-sm" onclick="closeModal()">Cancelar</button>
      <button class="btn btn-primary btn-sm" onclick="saveTask()">Salvar</button>
    </div>
  </div>
</div>

<script>
const OWNERS = {
  todos:{label:'Todos',av:'av-all',init:'T'},
  gustavo:{label:'Gustavo',av:'av-g',init:'G'},
  ana:{label:'Ana',av:'av-a',init:'A'},
  luana:{label:'Luana',av:'av-l',init:'L'},
  dayane:{label:'Dayane',av:'av-d',init:'D'}
};
const ROLES = {todos:'Squad completo',gustavo:'Desenvolvedor',ana:'UX · Marketing',luana:'Influenciadora',dayane:'Comercial · Negociações'};
const SL = {todo:'To Do',doing:'Doing',blocked:'Blocked',waiting:'Aguardando',done:'Concluído'};
const SB = {todo:'b-todo',doing:'b-doing',blocked:'b-blocked',waiting:'b-waiting',done:'b-done'};
const COLS = [
  {id:'todo',label:'To Do',dot:'#A09E99',statuses:['todo']},
  {id:'doing',label:'Doing',dot:'#EF9F27',statuses:['doing']},
  {id:'blocked',label:'Bloqueado / Aguardando',dot:'#E24B4A',statuses:['blocked','waiting']},
  {id:'done',label:'Concluído',dot:'#1D9E75',statuses:['done']}
];
const DEFAULTS = [
  {id:1,task:'Validar fluxo completo do app',owner:'todos',status:'doing',start:'27/04',deadline:'11/05',progress:20,kpi:'Fluxo 100% validado + bugs listados'},
  {id:2,task:'Analisar frontend',owner:'gustavo',status:'doing',start:'27/04',deadline:'30/04',progress:40,kpi:'Diagnóstico técnico completo'},
  {id:3,task:'Definir data de lançamento',owner:'gustavo',status:'waiting',start:'27/04',deadline:'—',progress:0,kpi:'Data definida'},
  {id:4,task:'Ajustes de UX',owner:'ana',status:'todo',start:'30/04',deadline:'05/05',progress:10,kpi:'3 melhorias aplicadas'},
  {id:5,task:'Definir canais de aquisição',owner:'ana',status:'doing',start:'27/04',deadline:'30/04',progress:30,kpi:'2 canais definidos'},
  {id:6,task:'Criar roteiros para Luana',owner:'ana',status:'doing',start:'27/04',deadline:'02/05',progress:40,kpi:'5 roteiros prontos'},
  {id:7,task:'Validar roteiros',owner:'luana',status:'blocked',start:'02/05',deadline:'03/05',progress:0,kpi:'Feedback entregue'},
  {id:8,task:'Produzir vídeos',owner:'luana',status:'todo',start:'03/05',deadline:'09/05',progress:0,kpi:'5 vídeos gravados'},
  {id:9,task:'Contatar passadores',owner:'dayane',status:'doing',start:'27/04',deadline:'02/05',progress:50,kpi:'4 contatos feitos'},
  {id:10,task:'Converter passadores',owner:'dayane',status:'todo',start:'02/05',deadline:'11/05',progress:0,kpi:'2 passadores ativos'}
];

let tasks = [];
let editId = null;

function persist(){ localStorage.setItem('dogfy_v2', JSON.stringify(tasks)); }
function loadData(){
  const s = localStorage.getItem('dogfy_v2');
  tasks = s ? JSON.parse(s) : JSON.parse(JSON.stringify(DEFAULTS));
}
function resetData(){
  if(!confirm('Resetar todas as tarefas para os dados iniciais?')) return;
  tasks = JSON.parse(JSON.stringify(DEFAULTS));
  persist(); renderAll();
}
function nextId(){ return tasks.length ? Math.max(...tasks.map(t=>t.id))+1 : 1; }

function badgeLabel(t){
  if(t.status==='done') return 'Concluído';
  if(t.progress>0) return t.progress+'%';
  return SL[t.status];
}

/* KANBAN */
function renderKanban(){
  const kb = document.getElementById('kb');
  kb.innerHTML = COLS.map(col=>{
    const ct = tasks.filter(t=>col.statuses.includes(t.status));
    const cards = ct.length
      ? ct.map(t=>{
          const o=OWNERS[t.owner];
          return `<div class="card" onclick="openEdit(${t.id})">
            <div class="card-task">${t.task}</div>
            <div class="pbar"><div class="pfill" style="width:${t.progress}%"></div></div>
            <div class="card-kpi">${t.kpi||''}</div>
            <div class="card-foot" style="margin-top:8px">
              <div class="avatar ${o.av}">${o.init}</div>
              <div style="display:flex;align-items:center;gap:6px">
                <span class="card-deadline">${t.deadline}</span>
                <span class="badge ${SB[t.status]}">${badgeLabel(t)}</span>
              </div>
            </div>
          </div>`;
        }).join('')
      : `<div class="empty-col">Nenhuma tarefa aqui</div>`;
    return `<div>
      <div class="col-head">
        <div class="col-dot" style="background:${col.dot}"></div>
        <span class="col-name">${col.label}</span>
        <span class="col-count">${ct.length}</span>
      </div>
      ${cards}
      <button class="add-card-btn" onclick="openNew('${col.statuses[0]}')">+ Adicionar</button>
    </div>`;
  }).join('');
  document.getElementById('nav-count').textContent = tasks.length;
}

/* KPIs */
function renderKPIs(){
  const total=tasks.length, done=tasks.filter(t=>t.status==='done').length,
        doing=tasks.filter(t=>t.status==='doing').length,
        blocked=tasks.filter(t=>['blocked','waiting'].includes(t.status)).length,
        avg=total?Math.round(tasks.reduce((s,t)=>s+t.progress,0)/total):0;

  const end=new Date('2025-05-11'), today=new Date();
  const diff=Math.max(0,Math.ceil((end-today)/86400000));

  const alertEl=document.getElementById('kpi-alert');
  if(done===total&&total>0){
    alertEl.innerHTML=`<div class="kpi-alert ok">✅ Sprint concluída! Todas as ${total} entregas foram finalizadas.</div>`;
  } else if(blocked>0){
    alertEl.innerHTML=`<div class="kpi-alert warn"><span style="font-size:16px;flex-shrink:0">⚠️</span><span><strong>Atenção da gestão:</strong> ${blocked} tarefa(s) bloqueada(s). ${done} de ${total} entregas concluídas. Verifique dependências críticas.</span></div>`;
  } else {
    alertEl.innerHTML=`<div class="kpi-alert ok">🟢 Sprint em andamento. ${doing} tarefa(s) em execução, ${done} concluída(s).</div>`;
  }

  document.getElementById('metrics').innerHTML=`
    <div class="metric-card"><div class="metric-label">Progresso geral</div><div class="metric-value">${avg}%</div><div class="metric-sub">Sprint 1 · ${total} tarefas</div></div>
    <div class="metric-card"><div class="metric-label">Em execução</div><div class="metric-value">${doing}</div><div class="metric-sub">tarefas doing</div></div>
    <div class="metric-card"><div class="metric-label">Bloqueadas</div><div class="metric-value" style="color:${blocked>0?'var(--red)':'var(--green)'}">${blocked}</div><div class="metric-sub">risco ativo</div></div>
    <div class="metric-card"><div class="metric-label">Concluídas</div><div class="metric-value" style="color:var(--green)">${done}</div><div class="metric-sub">de ${total} tarefas</div></div>`;

  function dotClass(t){
    if(!t) return 'sd-pend';
    if(t.status==='done') return 'sd-ok';
    if(['blocked','waiting'].includes(t.status)) return 'sd-risk';
    return t.progress>=50?'sd-warn':'sd-pend';
  }
  function valStr(t){
    if(!t) return 'Pendente';
    if(t.status==='done') return 'Concluído ✓';
    if(t.status==='blocked') return 'Blocked';
    if(t.status==='waiting') return 'Aguardando';
    return t.progress+'%';
  }

  const areas=[
    {title:'🚀 Produto',rows:[{name:'Fluxo 100% validado',id:1},{name:'Lista de bugs priorizada',id:null}]},
    {title:'💻 Tecnologia',rows:[{name:'Data de lançamento definida',id:3},{name:'App funcional (MVP)',id:2}]},
    {title:'📢 Marketing',rows:[{name:'5 vídeos prontos',id:8},{name:'2 canais ativos',id:5},{name:'5 roteiros prontos',id:6}]},
    {title:'🤝 Comercial',rows:[{name:'4 passadores contatados',id:9},{name:'2 passadores ativos',id:10}]}
  ];

  document.getElementById('kpi-sec').innerHTML=areas.map(a=>{
    const rows=a.rows.map(r=>{
      const t=r.id?tasks.find(x=>x.id===r.id):null;
      return `<div class="kpi-row"><span class="kpi-row-name">${r.name}</span><div class="kpi-row-right"><div class="status-dot ${dotClass(t)}"></div><span class="kpi-val">${valStr(t)}</span></div></div>`;
    }).join('');
    return `<div class="kpi-section"><div class="kpi-section-title">${a.title}</div>${rows}</div>`;
  }).join('');
}

/* SPRINTS */
function renderSprints(){
  const avg=tasks.length?Math.round(tasks.reduce((s,t)=>s+t.progress,0)/tasks.length):0;
  document.getElementById('sp-fill').style.width=avg+'%';
  document.getElementById('sp-pct').textContent=avg+'%';

  const memberOrder=['todos','gustavo','ana','luana','dayane'];
  document.getElementById('members').innerHTML=memberOrder.map(m=>{
    const o=OWNERS[m], mt=tasks.filter(t=>t.owner===m);
    if(!mt.length) return '';
    return `<div class="member-card">
      <div class="member-head">
        <div class="member-av ${o.av}">${o.init}</div>
        <div><div class="member-name">${o.label}</div><div class="member-role">${ROLES[m]}</div></div>
      </div>
      <div class="member-tasks">${mt.map(t=>`
        <div class="sprint-task-row">
          <span class="sprint-task-name">${t.task}</span>
          <span class="badge ${SB[t.status]}">${badgeLabel(t)}</span>
          <span class="sprint-task-date">→ ${t.deadline}</span>
        </div>`).join('')}
      </div>
    </div>`;
  }).join('');
}

function renderAll(){ renderKanban(); renderKPIs(); renderSprints(); }

/* MODAL */
function openEdit(id){
  editId=id;
  const t=tasks.find(x=>x.id===id);
  document.getElementById('modal-hd').textContent='Editar tarefa';
  document.getElementById('f-task').value=t.task;
  document.getElementById('f-owner').value=t.owner;
  document.getElementById('f-status').value=t.status;
  document.getElementById('f-start').value=t.start;
  document.getElementById('f-deadline').value=t.deadline;
  document.getElementById('f-prog').value=t.progress;
  document.getElementById('plbl').textContent=t.progress+'%';
  document.getElementById('plbl2').textContent=t.progress+'%';
  document.getElementById('f-kpi').value=t.kpi||'';
  document.getElementById('btn-del').style.display='inline-flex';
  document.getElementById('overlay').classList.add('open');
}
function openNew(defaultStatus){
  editId=null;
  document.getElementById('modal-hd').textContent='Nova tarefa';
  document.getElementById('f-task').value='';
  document.getElementById('f-owner').value='todos';
  document.getElementById('f-status').value=defaultStatus||'todo';
  document.getElementById('f-start').value='';
  document.getElementById('f-deadline').value='';
  document.getElementById('f-prog').value=0;
  document.getElementById('plbl').textContent='0%';
  document.getElementById('plbl2').textContent='0%';
  document.getElementById('f-kpi').value='';
  document.getElementById('btn-del').style.display='none';
  document.getElementById('overlay').classList.add('open');
}
function closeModal(){ document.getElementById('overlay').classList.remove('open'); }
function overlayClick(e){ if(e.target===document.getElementById('overlay')) closeModal(); }

document.getElementById('f-prog').addEventListener('input', function(){
  document.getElementById('plbl').textContent=this.value+'%';
  document.getElementById('plbl2').textContent=this.value+'%';
});

function saveTask(){
  const task=document.getElementById('f-task').value.trim();
  if(!task){ alert('Informe o nome da tarefa.'); return; }
  const obj={task,owner:document.getElementById('f-owner').value,status:document.getElementById('f-status').value,
    start:document.getElementById('f-start').value||'—',deadline:document.getElementById('f-deadline').value||'—',
    progress:parseInt(document.getElementById('f-prog').value),kpi:document.getElementById('f-kpi').value.trim()};
  if(editId){ const i=tasks.findIndex(t=>t.id===editId); tasks[i]={...tasks[i],...obj}; }
  else tasks.push({id:nextId(),...obj});
  persist(); renderAll(); closeModal();
}
function deleteTask(){
  if(!editId||!confirm('Excluir esta tarefa permanentemente?')) return;
  tasks=tasks.filter(t=>t.id!==editId);
  persist(); renderAll(); closeModal();
}

/* TABS */
const TTITLES={kanban:['Tarefas','Sprint 1'],kpis:['KPIs','Sprint 1 · Visão geral'],sprints:['Sprints','1 sprint ativa']};
function switchTab(tab,el){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById(tab).classList.add('active');
  el.classList.add('active');
  document.getElementById('tb-title').textContent=TTITLES[tab][0];
  document.getElementById('tb-sub').textContent=TTITLES[tab][1];
}

loadData(); renderAll();
</script>
</body>
</html>
