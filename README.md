[final dashboard github testeoo.html](https://github.com/user-attachments/files/30858625/final.dashboard.github.testeoo.html) 
# Panel de Cobranza — Cuentas por Cobrar Multi-País

Dashboard interactivo construido en HTML, CSS y JavaScript puro que centraliza los indicadores clave de cobranza de una operación multi-país, con filtros dinámicos y seguimiento de facturas caso por caso.

[Uploading final<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Panel de cobranza — Cuentas por cobrar multi-país</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600&family=IBM+Plex+Sans:wght@400;500&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  :root {
    --bg: #EEF1EF;
    --card: #FFFFFF;
    --ink: #1B2A38;
    --ink-soft: #5B6B72;
    --ink-faint: #8B979B;
    --border: #DCE2DE;
    --teal: #2F6F62;
    --teal-bg: #E3EFEA;
    --amber: #B8791E;
    --amber-bg: #FBF0DC;
    --coral: #B94A3C;
    --coral-bg: #F8E6E2;
  }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: var(--bg);
    font-family: 'IBM Plex Sans', sans-serif;
    color: var(--ink);
  }
  .mono { font-family: 'IBM Plex Mono', monospace; }
  .wrap { max-width: 1100px; margin: 0 auto; padding: 28px 20px 60px; }
  header { display: flex; justify-content: space-between; align-items: flex-end; flex-wrap: wrap; gap: 8px; margin-bottom: 20px; }
  h1 { font-family: 'Space Grotesk', sans-serif; font-weight: 600; font-size: 24px; margin: 0; }
  header p { color: var(--ink-soft); font-size: 13px; margin: 4px 0 0; }
  .tag { font-size: 11px; color: var(--ink-faint); }
  .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 12px; margin-bottom: 20px; }
  .kpi { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 14px 16px; position: relative; }
  .kpi .bar { position: absolute; top: 0; left: 14px; width: 28px; height: 4px; border-radius: 0 0 3px 3px; }
  .kpi .label { font-size: 12px; color: var(--ink-soft); margin: 4px 0 6px; }
  .kpi .value { font-size: 22px; font-weight: 500; margin: 0; }
  .filters { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; margin-bottom: 16px; }
  select, input[type="text"], button {
    font-family: 'IBM Plex Sans', sans-serif;
    font-size: 13px;
    color: var(--ink);
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 8px 10px;
    outline: none;
  }
  select:focus, input[type="text"]:focus { border-color: var(--teal); }
  button { cursor: pointer; display: flex; align-items: center; gap: 6px; }
  .search-wrap { position: relative; }
  .search-wrap input { padding-left: 28px; width: 190px; }
  .search-wrap span { position: absolute; left: 9px; top: 9px; color: var(--ink-faint); font-size: 12px; }
  .count { font-size: 12px; color: var(--ink-faint); margin-left: auto; }
  .chart-card { background: var(--card); border: 1px solid var(--border); border-radius: 10px; padding: 16px; margin-bottom: 20px; }
  .chart-card p { font-size: 12px; color: var(--ink-soft); margin: 0 0 8px; }
  .chart-holder { position: relative; height: 240px; }
  .table-card { background: var(--card); border: 1px solid var(--border); border-radius: 10px; overflow: hidden; }
  .table-scroll { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  thead th {
    background: #F5F6F4; text-align: left; padding: 10px 12px; font-weight: 500;
    color: var(--ink-soft); font-size: 11px; text-transform: uppercase; letter-spacing: .03em;
    border-bottom: 1px solid var(--border); white-space: nowrap;
  }
  tbody td { padding: 9px 12px; border-bottom: 1px solid var(--border); white-space: nowrap; }
  tbody tr:hover { background: #F7F8F7; }
  .badge { font-size: 11px; font-weight: 500; padding: 3px 8px; border-radius: 6px; }
  .comment-cell { white-space: normal; min-width: 220px; }
  .comment-cell input {
    border: none; background: transparent; font-size: 12px; width: 100%; padding: 4px 0;
  }
  .pager { display: flex; justify-content: space-between; align-items: center; padding: 10px 14px; border-top: 1px solid var(--border); font-size: 12px; color: var(--ink-faint); }
  .pager .btns { display: flex; gap: 6px; }
  .pager button { padding: 5px 10px; }
  .pager button:disabled { opacity: .4; cursor: default; }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div>
      <h1>Panel de cobranza</h1>
      <p>Cuentas por cobrar multi-país · agencias de viaje</p>
    </div>
    <span class="tag">Datos ficticios · proyecto de portfolio</span>
  </header>

  <div class="kpi-grid" id="kpiGrid"></div>

  <div class="filters">
    <select id="fCountry"></select>
    <select id="fAgency"></select>
    <select id="fStatus">
      <option>Todos</option>
      <option>Pagada</option>
      <option>Pendiente</option>
      <option>En disputa</option>
    </select>
    <div class="search-wrap">
      <span>&#128269;</span>
      <input type="text" id="fSearch" placeholder="Buscar factura o agencia" />
    </div>
    <button id="btnReset">&#8635; Limpiar</button>
    <span class="count" id="countLabel"></span>
  </div>

  <div class="chart-card">
    <p>Facturas por país y estado</p>
    <div class="chart-holder"><canvas id="chart" role="img" aria-label="Facturas por país, apiladas por estado"></canvas></div>
  </div>

  <div class="table-card">
    <div class="table-scroll">
      <table>
        <thead>
          <tr>
            <th>Factura</th><th>País</th><th>Agencia</th><th>Monto</th><th>Emisión</th>
            <th>Vencimiento</th><th>Pago</th><th>Estado</th><th>Días</th><th>Comentario</th>
          </tr>
        </thead>
        <tbody id="tbody"></tbody>
      </table>
    </div>
    <div class="pager">
      <span id="pageLabel"></span>
      <div class="btns">
        <button id="btnPrev">Anterior</button>
        <button id="btnNext">Siguiente</button>
      </div>
    </div>
  </div>
</div>

<script>
// ---------- Datos ----------
const COUNTRIES = ["Argentina","México","Chile","Perú","Colombia","Brasil","Ecuador"];
const AGENCIES = [
  { name: "Andes Travel", country: "Argentina" },
  { name: "Pampa Turismo", country: "Argentina" },
  { name: "Porteña Viajes", country: "Argentina" },
  { name: "Azteca Tours", country: "México" },
  { name: "Riviera Maya DMC", country: "México" },
  { name: "Cactus Travel", country: "México" },
  { name: "Atacama Adventures", country: "Chile" },
  { name: "Patagonia Sur", country: "Chile" },
  { name: "Inca Trail Co", country: "Perú" },
  { name: "Machu Travel", country: "Perú" },
  { name: "Cafetal Tours", country: "Colombia" },
  { name: "Caribe Bogotá", country: "Colombia" },
  { name: "Ipanema Viagens", country: "Brasil" },
  { name: "Amazonia Turismo", country: "Brasil" },
  { name: "Galápagos Experience", country: "Ecuador" },
  { name: "Quito Andes Tours", country: "Ecuador" },
];
const TODAY = new Date();
const DAY = 86400000;
const addDays = (d, n) => new Date(d.getTime() + n * DAY);
const diffDays = (a, b) => Math.round((a.getTime() - b.getTime()) / DAY);
const fmtDate = (d) => d.toLocaleDateString("es-UY", { day: "2-digit", month: "2-digit", year: "numeric" });
const fmtUSD = (n) => new Intl.NumberFormat("en-US", { style: "currency", currency: "USD", maximumFractionDigits: 0 }).format(n);

function mulberry32(seed) {
  return function () {
    seed |= 0; seed = (seed + 0x6d2b79f5) | 0;
    let t = Math.imul(seed ^ (seed >>> 15), 1 | seed);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}

function generateInvoices() {
  const rnd = mulberry32(20260808);
  const invoices = [];
  for (let i = 0; i < 190; i++) {
    const agency = AGENCIES[Math.floor(rnd() * AGENCIES.length)];
    const issue = addDays(TODAY, -Math.floor(rnd() * 200) - 3);
    const due = addDays(issue, 30);
    const amount = Math.round(150 + rnd() * 3100);
    const roll = rnd();
    let status, paymentDate, daysToCollect;
    if (roll < 0.6) {
      status = "Pagada";
      const lag = Math.floor(5 + rnd() * 50);
      paymentDate = addDays(issue, lag);
      if (paymentDate > TODAY) paymentDate = TODAY;
      daysToCollect = diffDays(paymentDate, issue);
    } else if (roll < 0.86) {
      status = "Pendiente"; paymentDate = null; daysToCollect = null;
    } else {
      status = "En disputa"; paymentDate = null; daysToCollect = null;
    }
    invoices.push({
      id: `F${10000 + i}`, country: agency.country, agency: agency.name, amount,
      issue, due, paymentDate, status, daysToCollect,
      daysLate: status !== "Pagada" && TODAY > due ? diffDays(TODAY, due) : 0,
    });
  }
  return invoices;
}

const SEED_COMMENTS = {
  F10032: "En disputa por cargo duplicado, escalado a soporte.",
  F10101: "Agencia pidió extensión de plazo, nuevo compromiso 15/09.",
};

const STATUS_STYLE = {
  "Pagada": { bg: "var(--teal-bg)", fg: "var(--teal)" },
  "Pendiente": { bg: "var(--amber-bg)", fg: "var(--amber)" },
  "En disputa": { bg: "var(--coral-bg)", fg: "var(--coral)" },
};

const PAGE_SIZE = 12;
const invoices = generateInvoices();
let comments = JSON.parse(localStorage.getItem("invoice-comments") || "null") || SEED_COMMENTS;

let state = { country: "Todos", agency: "Todas", status: "Todos", search: "", page: 1 };
let chartInstance = null;

// ---------- Render helpers ----------
function agencyOptionsFor(country) {
  return country === "Todos" ? AGENCIES : AGENCIES.filter(a => a.country === country);
}

function getFiltered() {
  return invoices.filter(inv => {
    if (state.country !== "Todos" && inv.country !== state.country) return false;
    if (state.agency !== "Todas" && inv.agency !== state.agency) return false;
    if (state.status !== "Todos" && inv.status !== state.status) return false;
    if (state.search && !`${inv.id} ${inv.agency}`.toLowerCase().includes(state.search.toLowerCase())) return false;
    return true;
  });
}

function renderFilters() {
  const cSel = document.getElementById("fCountry");
  cSel.innerHTML = `<option>Todos</option>` + COUNTRIES.map(c => `<option${c===state.country?" selected":""}>${c}</option>`).join("");
  const aSel = document.getElementById("fAgency");
  aSel.innerHTML = `<option>Todas</option>` + agencyOptionsFor(state.country).map(a => `<option${a.name===state.agency?" selected":""}>${a.name}</option>`).join("");
  document.getElementById("fStatus").value = state.status;
  document.getElementById("fSearch").value = state.search;
}

function renderKPIs(filtered) {
  const pend = filtered.filter(i => i.status === "Pendiente");
  const pagadas = filtered.filter(i => i.status === "Pagada");
  const disputa = filtered.filter(i => i.status === "En disputa");
  const montoTotal = filtered.reduce((s, i) => s + i.amount, 0);
  const montoPagado = pagadas.reduce((s, i) => s + i.amount, 0);
  const pct = montoTotal ? (montoPagado / montoTotal) * 100 : 0;
  const avgDias = pagadas.length ? pagadas.reduce((s, i) => s + i.daysToCollect, 0) / pagadas.length : 0;

  const cards = [
    { label: "Facturas pendientes", value: pend.length, color: "var(--amber)" },
    { label: "Facturas pagadas", value: pagadas.length, color: "var(--teal)" },
    { label: "En disputa", value: disputa.length, color: "var(--coral)" },
    { label: "% cobranza (monto)", value: pct.toFixed(1) + "%", color: "var(--ink)" },
    { label: "Prom. días para cobrar", value: avgDias ? avgDias.toFixed(1) : "—", color: "var(--ink)" },
  ];
  document.getElementById("kpiGrid").innerHTML = cards.map(k => `
    <div class="kpi">
      <div class="bar" style="background:${k.color}"></div>
      <p class="label">${k.label}</p>
      <p class="value mono">${k.value}</p>
    </div>
  `).join("");
}

function renderChart(filtered) {
  const data = COUNTRIES.map(c => {
    const rows = filtered.filter(i => i.country === c);
    return {
      country: c,
      Pagada: rows.filter(i => i.status === "Pagada").length,
      Pendiente: rows.filter(i => i.status === "Pendiente").length,
      Disputa: rows.filter(i => i.status === "En disputa").length,
    };
  }).filter(r => r.Pagada + r.Pendiente + r.Disputa > 0);

  const ctx = document.getElementById("chart");
  const cfg = {
    type: "bar",
    data: {
      labels: data.map(d => d.country),
      datasets: [
        { label: "Pagada", data: data.map(d => d.Pagada), backgroundColor: "#2F6F62", stack: "s" },
        { label: "Pendiente", data: data.map(d => d.Pendiente), backgroundColor: "#C98A2E", stack: "s" },
        { label: "En disputa", data: data.map(d => d.Disputa), backgroundColor: "#B94A3C", stack: "s" },
      ],
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      scales: {
        x: { stacked: true, grid: { display: false } },
        y: { stacked: true, beginAtZero: true, ticks: { precision: 0 } },
      },
      plugins: { legend: { position: "top", labels: { boxWidth: 10, font: { size: 11 } } } },
    },
  };
  if (chartInstance) { chartInstance.data = cfg.data; chartInstance.update(); }
  else { chartInstance = new Chart(ctx, cfg); }
}

function renderTable(filtered) {
  const order = { "En disputa": 0, "Pendiente": 1, "Pagada": 2 };
  const sorted = [...filtered].sort((a, b) => {
    if (order[a.status] !== order[b.status]) return order[a.status] - order[b.status];
    if (a.status === "Pendiente") return b.daysLate - a.daysLate;
    return b.issue - a.issue;
  });
  const totalPages = Math.max(1, Math.ceil(sorted.length / PAGE_SIZE));
  if (state.page > totalPages) state.page = totalPages;
  const pageRows = sorted.slice((state.page - 1) * PAGE_SIZE, state.page * PAGE_SIZE);

  const tbody = document.getElementById("tbody");
  if (pageRows.length === 0) {
    tbody.innerHTML = `<tr><td colspan="10" style="text-align:center;padding:24px;color:var(--ink-faint)">No hay facturas para estos filtros.</td></tr>`;
  } else {
    tbody.innerHTML = pageRows.map(inv => {
      const st = STATUS_STYLE[inv.status];
      const daysLabel = inv.status === "Pagada" ? `${inv.daysToCollect}d`
        : inv.status === "Pendiente" && inv.daysLate > 0 ? `${inv.daysLate}d atraso`
        : inv.status === "Pendiente" ? "al día" : "—";
      const comment = (comments[inv.id] || "").replace(/"/g, "&quot;");
      return `
        <tr>
          <td class="mono">${inv.id}</td>
          <td style="color:var(--ink-soft)">${inv.country}</td>
          <td style="color:var(--ink-soft)">${inv.agency}</td>
          <td class="mono">${fmtUSD(inv.amount)}</td>
          <td class="mono" style="color:var(--ink-faint)">${fmtDate(inv.issue)}</td>
          <td class="mono" style="color:var(--ink-faint)">${fmtDate(inv.due)}</td>
          <td class="mono" style="color:var(--ink-faint)">${inv.paymentDate ? fmtDate(inv.paymentDate) : "—"}</td>
          <td><span class="badge" style="background:${st.bg};color:${st.fg}">${inv.status}</span></td>
          <td class="mono" style="color:var(--ink-soft)">${daysLabel}</td>
          <td class="comment-cell">
            <input type="text" data-id="${inv.id}" placeholder="Agregar comentario…" value="${comment}" />
          </td>
        </tr>`;
    }).join("");
  }

  document.getElementById("pageLabel").textContent = `Página ${state.page} de ${totalPages}`;
  document.getElementById("btnPrev").disabled = state.page === 1;
  document.getElementById("btnNext").disabled = state.page === totalPages;
  document.getElementById("btnPrev").onclick = () => { state.page = Math.max(1, state.page - 1); renderAll(); };
  document.getElementById("btnNext").onclick = () => { state.page = Math.min(totalPages, state.page + 1); renderAll(); };

  tbody.querySelectorAll("input[data-id]").forEach(input => {
    input.addEventListener("blur", (e) => {
      const id = e.target.getAttribute("data-id");
      comments[id] = e.target.value;
      localStorage.setItem("invoice-comments", JSON.stringify(comments));
    });
  });
}

function renderAll() {
  const filtered = getFiltered();
  renderFilters();
  renderKPIs(filtered);
  renderChart(filtered);
  renderTable(filtered);
  document.getElementById("countLabel").textContent = `Mostrando ${filtered.length} de ${invoices.length} facturas`;
}

// ---------- Eventos ----------
document.getElementById("fCountry").addEventListener("change", (e) => {
  state.country = e.target.value; state.agency = "Todas"; state.page = 1; renderAll();
});
document.getElementById("fAgency").addEventListener("change", (e) => { state.agency = e.target.value; state.page = 1; renderAll(); });
document.getElementById("fStatus").addEventListener("change", (e) => { state.status = e.target.value; state.page = 1; renderAll(); });
document.getElementById("fSearch").addEventListener("input", (e) => { state.search = e.target.value; state.page = 1; renderAll(); });
document.getElementById("btnReset").addEventListener("click", () => {
  state = { country: "Todos", agency: "Todas", status: "Todos", search: "", page: 1 }; renderAll();
});

renderAll();
</script>
</body>
</html>
 dashboard github testeoo.html…]()


## El problema

En cobranza multi-país, la información suele vivir repartida: un reporte de facturas pendientes acá, un cálculo de días de atraso hecho a mano en Excel allá, y el contexto de por qué una factura puntual no se cobra (una disputa, una promesa de pago) que solo alguien recuerda de memoria o queda perdido en un hilo de mail. Responder "¿cómo estamos con la cobranza de Brasil este mes?" implica cruzar varias fuentes a mano.

## La solución

Un panel único donde se puede:

- Ver de un vistazo cuántas facturas están pendientes, pagadas o en disputa
- Filtrar por país, agencia o estado para aislar un segmento puntual
- Ver el % de cobranza y el promedio de días que tarda en pagarse una factura
- Dejar un comentario por factura (por ejemplo, por qué una está en disputa) para que ese contexto no se pierda

## Cómo funciona

```
Se generan facturas (dataset de ejemplo)
        ↓
Se aplican los filtros activos (país, agencia, estado, búsqueda)
        ↓
Se recalculan los KPIs y el gráfico sobre el subconjunto filtrado
        ↓
La tabla se ordena por urgencia (disputas primero, luego atrasos)
        ↓
Los comentarios se guardan en el navegador al editarlos
```

## Características técnicas

- **KPIs dinámicos**: facturas pendientes, pagadas, en disputa, % de cobranza (por monto) y promedio de días para cobrar — todos recalculados en tiempo real según los filtros activos.
- **Filtros encadenados**: el selector de agencia se ajusta automáticamente según el país elegido.
- **Comentarios editables por factura**: persisten en `localStorage`, así el contexto de una factura puntual (ej. "en disputa por cargo duplicado") queda documentado.
- **Ordenamiento por urgencia**: la tabla prioriza automáticamente facturas en disputa y pendientes más atrasadas.
- **Gráfico de estado por país**: barras apiladas (pagada / pendiente / en disputa) usando Chart.js.
- **Sin dependencias de build**: es un solo archivo HTML, no requiere `npm install` ni proceso de compilación.

## Interfaz

Diseño responsive, con la misma identidad visual del resto del portfolio.

## Stack

| Capa | Tecnología |
|---|---|
| Lógica / estado | JavaScript vanilla |
| Gráficos | Chart.js (vía CDN) |
| Estilos | CSS puro |
| Persistencia de comentarios | localStorage del navegador |
 
└── index.html   # Dashboard completo: datos, lógica, estilos e interfaz
```
 
