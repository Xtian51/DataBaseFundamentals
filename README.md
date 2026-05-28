<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bases de Datos — Semana 4: Cierre Unidad II</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root { --bg:#0d1117; --surface:#161b22; --border:#30363d; --text:#e6edf3; --muted:#8b949e; }
  * { box-sizing:border-box; margin:0; padding:0; }
  body { background:#0d1117; color:#e6edf3; font-family:'Inter',sans-serif; font-size:15px; line-height:1.7; }
  header { background:linear-gradient(135deg,#0f172a 0%,#1a1a3e 50%,#0f172a 100%); border-bottom:1px solid #30363d; padding:48px 0 36px; text-align:center; position:relative; overflow:hidden; }
  header::before { content:''; position:absolute; inset:0; background:radial-gradient(ellipse 60% 60% at 50% 0%,rgba(168,85,247,.13),transparent); pointer-events:none; }
  .badge { display:inline-block; background:rgba(168,85,247,.12); border:1px solid rgba(168,85,247,.35); color:#c084fc; font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.1em; padding:4px 14px; border-radius:20px; margin-bottom:16px; }
  header h1 { font-family:'Syne',sans-serif; font-size:clamp(22px,4vw,38px); font-weight:800; color:#fff; }
  header h1 span { color:#c084fc; }
  header p { color:#8b949e; margin-top:10px; font-size:14px; }
  .pills { display:flex; gap:10px; justify-content:center; flex-wrap:wrap; margin-top:20px; }
  .pill { background:#21262d; border:1px solid #30363d; border-radius:20px; padding:4px 14px; font-size:12px; color:#8b949e; }
  .container { max-width:960px; margin:0 auto; padding:0 24px; }
  .layout { display:grid; grid-template-columns:200px 1fr; gap:32px; padding:40px 0 60px; align-items:start; }
  @media(max-width:760px){ .layout{ grid-template-columns:1fr; } .toc{ display:none; } }
  .toc { position:sticky; top:20px; background:#161b22; border:1px solid #30363d; border-radius:10px; padding:16px; font-size:13px; }
  .toc h4 { font-family:'Syne',sans-serif; font-size:11px; letter-spacing:.12em; text-transform:uppercase; color:#8b949e; margin-bottom:12px; }
  .toc a { display:block; color:#8b949e; text-decoration:none; padding:4px 0 4px 10px; border-left:2px solid transparent; transition:all .2s; }
  .toc a:hover { color:#c084fc; border-color:#c084fc; }
  section { margin-bottom:48px; scroll-margin-top:24px; }
  .section-label { display:inline-flex; align-items:center; gap:8px; font-family:'JetBrains Mono',monospace; font-size:11px; letter-spacing:.1em; text-transform:uppercase; color:#c084fc; margin-bottom:8px; }
  .section-label .dot { width:6px; height:6px; background:#c084fc; border-radius:50%; }
  h2 { font-family:'Syne',sans-serif; font-size:22px; font-weight:800; color:#fff; margin-bottom:20px; padding-bottom:10px; border-bottom:1px solid #30363d; }
  h3 { font-family:'Syne',sans-serif; font-size:16px; font-weight:600; color:#c084fc; margin:28px 0 12px; }
  p { color:#cdd5e0; margin-bottom:14px; }
  ul,ol { padding-left:22px; margin-bottom:14px; }
  li { color:#cdd5e0; margin-bottom:6px; }
  li strong { color:#e6edf3; }
  .card { background:#161b22; border:1px solid #30363d; border-radius:10px; padding:20px 24px; margin-bottom:20px; }
  .card.purple { border-left:3px solid #a855f7; }
  .card.green  { border-left:3px solid #22c55e; }
  .card.blue   { border-left:3px solid #3b82f6; }
  .card.yellow { border-left:3px solid #eab308; }
  .card.orange { border-left:3px solid #f97316; }
  .card h4 { font-family:'Syne',sans-serif; font-size:14px; font-weight:600; margin-bottom:8px; color:#fff; }
  .card p  { margin:0; font-size:14px; }
  .definition { background:linear-gradient(135deg,rgba(168,85,247,.07),rgba(192,132,252,.04)); border:1px solid rgba(168,85,247,.28); border-radius:10px; padding:20px 24px; margin:20px 0; }
  .definition .label { font-family:'JetBrains Mono',monospace; font-size:10px; letter-spacing:.12em; color:#c084fc; text-transform:uppercase; margin-bottom:8px; }
  .definition p { margin:0; }
  .table-wrap { overflow-x:auto; margin:20px 0; }
  table { width:100%; border-collapse:collapse; font-size:14px; }
  th { background:#21262d !important; border:1px solid #30363d !important; padding:10px 14px; text-align:left; font-family:'JetBrains Mono',monospace; font-size:12px; color:#c084fc !important; letter-spacing:.06em; }
  td { background:#0d1117 !important; border:1px solid #30363d !important; padding:10px 14px; color:#cdd5e0 !important; }
  tr:nth-child(even) td { background:#161b22 !important; }
  .code-block { background:#161b22; border:1px solid #30363d; border-radius:10px; margin:20px 0; overflow:hidden; }
  .code-header { background:#21262d; border-bottom:1px solid #30363d; padding:8px 16px; display:flex; align-items:center; justify-content:space-between; }
  .code-header span { font-family:'JetBrains Mono',monospace; font-size:11px; color:#8b949e; }
  .code-dots { display:flex; gap:6px; }
  .code-dots i { width:10px; height:10px; border-radius:50%; display:inline-block; }
  .code-dots i:nth-child(1){ background:#ef4444; } .code-dots i:nth-child(2){ background:#eab308; } .code-dots i:nth-child(3){ background:#22c55e; }
  pre { padding:18px 20px !important; overflow-x:auto; font-family:'JetBrains Mono',monospace !important; font-size:13px; line-height:1.6; color:#e6edf3 !important; background:#161b22 !important; }
  .kw{color:#79c0ff} .str{color:#a5d6ff} .cm{color:#8b949e;font-style:italic} .fn{color:#d2a8ff} .num{color:#ffa657} .op{color:#ff7b72}
  .diagram { background:#0d1117 !important; border:1px solid #30363d; border-radius:10px; padding:20px; font-family:'JetBrains Mono',monospace; font-size:12px; color:#c084fc !important; overflow-x:auto; margin:20px 0; white-space:pre; line-height:1.5; }
  .ejercicio { background:#161b22; border:1px solid #30363d; border-radius:10px; padding:22px 24px; margin-bottom:24px; position:relative; }
  .ejercicio::before { content:attr(data-num); position:absolute; top:-12px; left:20px; background:#a855f7; color:#fff; font-family:'JetBrains Mono',monospace; font-size:11px; font-weight:600; padding:2px 12px; border-radius:20px; }
  .ejercicio.practica::before  { background:#22c55e; }
  .ejercicio.integrador::before{ background:#f97316; }
  .ejercicio h4 { font-family:'Syne',sans-serif; font-size:15px; font-weight:600; margin-bottom:10px; color:#fff; }
  .pipeline { display:flex; gap:0; align-items:center; flex-wrap:wrap; margin:20px 0; }
  .pipe-step { background:#161b22; border:1px solid #30363d; border-radius:8px; padding:12px 16px; text-align:center; min-width:110px; }
  .pipe-step .icon { font-size:22px; margin-bottom:4px; }
  .pipe-step .name { font-family:'Syne',sans-serif; font-size:12px; font-weight:700; color:#fff; }
  .pipe-step .sub  { font-size:11px; color:#8b949e; }
  .pipe-arrow { color:#a855f7; font-size:22px; padding:0 8px; }
  .summary-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(160px,1fr)); gap:14px; margin-top:20px; }
  .summary-item { background:#161b22; border:1px solid #30363d; border-radius:8px; padding:14px 16px; text-align:center; }
  .summary-item .icon { font-size:24px; margin-bottom:6px; }
  .summary-item .label { font-size:11px; color:#8b949e; margin-top:4px; }
  .summary-item .val { font-family:'Syne',sans-serif; font-size:13px; font-weight:600; color:#e6edf3; }
  .checklist { list-style:none; padding:0; }
  .checklist li { display:flex; align-items:flex-start; gap:10px; padding:8px 0; border-bottom:1px solid #30363d; color:#cdd5e0; font-size:14px; }
  .checklist li:last-child { border-bottom:none; }
  .checklist li::before { content:"☐"; color:#c084fc; font-size:16px; flex-shrink:0; margin-top:1px; }
</style>
</head>
<body>
<header>
  <div class="badge">ITIID · Bases de Datos · UPTap · U2 — Cierre</div>
  <h1>Semana 4 — <span>Cierre Unidad II: Integración del Modelado</span></h1>
  <p>Unidad II · Ejercicios integradores · ER → Relacional → Diccionario → DDL</p>
  <div class="pills">
    <span class="pill">📅 25 – 29 Mayo 2026</span>
    <span class="pill">⏱ Temas 2.1 – 2.5 (repaso integral)</span>
    <span class="pill">📋 Presentación Evidencia 2</span>
  </div>
</header>
<div class="container">
<div class="layout">
  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#pipeline">🔄 Pipeline completo</a>
    <a href="#repaso">📐 Repaso rápido</a>
    <a href="#errores">⚠️ Errores comunes</a>
    <a href="#caso1">🏥 Caso: Clínica</a>
    <a href="#caso2">🛒 Caso: E-commerce</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#evidencia">📋 Evidencia 2</a>
    <a href="#resumen">✅ Resumen U2</a>
  </nav>
  <main>

    <section id="pipeline">
      <div class="section-label"><span class="dot"></span>01 · El proceso completo</div>
      <h2>Pipeline: Del problema al DDL</h2>
      <p>Esta semana cerramos la Unidad II integrando todos los pasos del modelado de bases de datos. El flujo completo es:</p>
      <div class="pipeline">
        <div class="pipe-step"><div class="icon">📋</div><div class="name">Análisis</div><div class="sub">Requisitos</div></div>
        <div class="pipe-arrow">→</div>
        <div class="pipe-step"><div class="icon">🔷</div><div class="name">Modelo ER</div><div class="sub">Conceptual</div></div>
        <div class="pipe-arrow">→</div>
        <div class="pipe-step"><div class="icon">📐</div><div class="name">Esquema Relacional</div><div class="sub">Lógico</div></div>
        <div class="pipe-arrow">→</div>
        <div class="pipe-step"><div class="icon">📖</div><div class="name">Diccionario</div><div class="sub">Documentación</div></div>
        <div class="pipe-arrow">→</div>
        <div class="pipe-step"><div class="icon">🗄</div><div class="name">DDL</div><div class="sub">Físico</div></div>
      </div>
      <div class="card purple">
        <h4>🎯 Objetivo de la semana</h4>
        <p>Ejecutar el pipeline completo sobre dos casos reales de principio a fin. Al terminar, cada equipo aplica el mismo proceso sobre su proyecto final para la Evidencia 2.</p>
      </div>
    </section>

    <section id="repaso">
      <div class="section-label"><span class="dot"></span>02 · Repaso rápido</div>
      <h2>Repaso Rápido — Unidad II en una tabla</h2>
      <div class="table-wrap">
        <table>
          <tr><th>Concepto</th><th>Definición clave</th><th>Regla práctica</th></tr>
          <tr><td><strong>Entidad</strong></td><td>Objeto del mundo real del que guardamos datos</td><td>Si necesitas varios datos de algo → es entidad</td></tr>
          <tr><td><strong>Atrib. multivaluado</strong></td><td>Campo con más de un valor por instancia</td><td>Siempre genera tabla separada (R5)</td></tr>
          <tr><td><strong>Relación 1:N</strong></td><td>Un registro de A ↔ muchos de B</td><td>FK va en la tabla del lado N (R2)</td></tr>
          <tr><td><strong>Relación N:M</strong></td><td>Muchos de A ↔ muchos de B</td><td>Siempre genera tabla intermedia con PK compuesta (R3)</td></tr>
          <tr><td><strong>Relación 1:1</strong></td><td>Un registro de A ↔ exactamente uno de B</td><td>FK + UNIQUE en la tabla de participación total (R4)</td></tr>
          <tr><td><strong>Diccionario de datos</strong></td><td>Documentación técnica de cada campo</td><td>Nombre · tipo · restricciones · default · descripción</td></tr>
        </table>
      </div>
      <h3>Las 5 reglas de transformación</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Regla</th><th>Situación ER</th><th>Resultado en el modelo relacional</th><th>¿Tabla nueva?</th></tr>
          <tr><td><strong>R1</strong></td><td>Entidad fuerte</td><td>Una tabla con sus atributos simples</td><td>✅ Sí</td></tr>
          <tr><td><strong>R2</strong></td><td>Relación 1:N</td><td>PK del lado "1" como FK en la tabla del lado "N"</td><td>❌ No</td></tr>
          <tr><td><strong>R3</strong></td><td>Relación N:M</td><td>Tabla intermedia con PK compuesta (FK1 + FK2)</td><td>✅ Sí</td></tr>
          <tr><td><strong>R4</strong></td><td>Relación 1:1</td><td>FK + UNIQUE en la tabla de participación total</td><td>❌ No</td></tr>
          <tr><td><strong>R5</strong></td><td>Atributo multivaluado</td><td>Tabla nueva: PK_padre (FK) + atributo. PK compuesta.</td><td>✅ Sí</td></tr>
        </table>
      </div>
    </section>

    <section id="errores">
      <div class="section-label"><span class="dot"></span>03 · Errores frecuentes</div>
      <h2>Errores Comunes al Modelar</h2>

      <div class="ejercicio" data-num="Error 1 — FK en el lado equivocado">
        <h4>Poner la FK en el lado "1" en lugar del lado "N"</h4>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>❌ Incorrecto</span></div>
<pre><span class="cm">-- CLIENTE (1) ──── realiza ──── (N) PEDIDO</span>
<span class="kw">CREATE TABLE</span> CLIENTE (
    id_cliente <span class="kw">INT PRIMARY KEY</span>,
    nombre     <span class="kw">VARCHAR</span>(<span class="num">100</span>),
    id_pedido  <span class="kw">INT</span>  <span class="cm">-- ❌ Limita el cliente a UN solo pedido</span>
);</pre></div>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>✅ Correcto — FK en el lado N</span></div>
<pre><span class="kw">CREATE TABLE</span> PEDIDO (
    id_pedido  <span class="kw">INT PRIMARY KEY</span>,
    fecha      <span class="kw">DATE</span>,
    id_cliente <span class="kw">INT NOT NULL</span>,  <span class="cm">-- ✅ FK en PEDIDO (lado N)</span>
    <span class="kw">FOREIGN KEY</span>(id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente)
);</pre></div>
      </div>

      <div class="ejercicio" data-num="Error 2 — N:M sin tabla intermedia">
        <h4>Tratar una relación N:M como si fuera 1:N</h4>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>❌ Incorrecto — un estudiante solo cursaría 1 materia</span></div>
<pre><span class="kw">CREATE TABLE</span> ESTUDIANTE (
    id_est <span class="kw">INT PRIMARY KEY</span>,
    nombre <span class="kw">VARCHAR</span>(<span class="num">100</span>),
    id_mat <span class="kw">INT</span>  <span class="cm">-- ❌ Solo puede cursar UNA materia</span>
);</pre></div>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>✅ Correcto — tabla intermedia INSCRIPCION</span></div>
<pre><span class="kw">CREATE TABLE</span> INSCRIPCION (
    id_est  <span class="kw">INT NOT NULL</span>,
    id_mat  <span class="kw">INT NOT NULL</span>,
    periodo <span class="kw">VARCHAR</span>(<span class="num">20</span>),
    <span class="kw">PRIMARY KEY</span>(id_est, id_mat),
    <span class="kw">FOREIGN KEY</span>(id_est) <span class="kw">REFERENCES</span> ESTUDIANTE(id_est),
    <span class="kw">FOREIGN KEY</span>(id_mat) <span class="kw">REFERENCES</span> MATERIA(id_mat)
);</pre></div>
      </div>

      <div class="ejercicio" data-num="Error 3 — Atributo multivaluado en columna">
        <h4>Guardar múltiples valores en una sola columna (viola 1FN)</h4>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>❌ Incorrecto</span></div>
<pre><span class="kw">CREATE TABLE</span> EMPLEADO (
    id_emp    <span class="kw">INT PRIMARY KEY</span>,
    telefonos <span class="kw">VARCHAR</span>(<span class="num">200</span>)  <span class="cm">-- ❌ '961-111,961-222' en una celda</span>
);</pre></div>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>✅ Correcto — tabla separada (R5)</span></div>
<pre><span class="kw">CREATE TABLE</span> TELEFONO_EMP (
    id_emp <span class="kw">INT NOT NULL</span>,
    numero <span class="kw">VARCHAR</span>(<span class="num">20</span>) <span class="kw">NOT NULL</span>,
    tipo   <span class="kw">ENUM</span>(<span class="str">'casa'</span>,<span class="str">'movil'</span>,<span class="str">'trabajo'</span>),
    <span class="kw">PRIMARY KEY</span>(id_emp, numero),
    <span class="kw">FOREIGN KEY</span>(id_emp) <span class="kw">REFERENCES</span> EMPLEADO(id_emp)
);</pre></div>
      </div>

      <div class="ejercicio" data-num="Error 4 — Orden de creación de tablas">
        <h4>Crear una tabla con FK antes que la tabla referenciada</h4>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>❌ Incorrecto — error en tiempo de ejecución</span></div>
<pre><span class="kw">CREATE TABLE</span> PEDIDO (
    id_pedido  <span class="kw">INT PRIMARY KEY</span>,
    id_cliente <span class="kw">INT</span>,
    <span class="kw">FOREIGN KEY</span>(id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente) <span class="cm">-- ❌ CLIENTE no existe aún</span>
);
<span class="kw">CREATE TABLE</span> CLIENTE ( id_cliente <span class="kw">INT PRIMARY KEY</span>, ... );</pre></div>
        <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>✅ Correcto — primero las tablas referenciadas</span></div>
<pre><span class="cm">-- Regla: primero tablas SIN FK, luego las que tienen FK</span>
<span class="kw">CREATE TABLE</span> CLIENTE ( id_cliente <span class="kw">INT PRIMARY KEY</span>, ... );   <span class="cm">-- 1ro</span>
<span class="kw">CREATE TABLE</span> PEDIDO  ( ..., <span class="kw">FOREIGN KEY</span>(id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente) ); <span class="cm">-- 2do</span></pre></div>
      </div>
    </section>

    <section id="caso1">
      <div class="section-label"><span class="dot"></span>04 · Caso integrador 1</div>
      <h2>Caso Integrador 1 — Clínica Médica</h2>
      <div class="card blue">
        <h4>📋 Enunciado</h4>
        <p>Una clínica gestiona médicos de distintas especialidades. Los pacientes se registran con múltiples teléfonos y tienen consultas. En cada consulta el médico emite recetas con medicamentos. Los médicos trabajan en varias sucursales.</p>
      </div>
      <h3>Relaciones identificadas</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Relación</th><th>Cardinalidad</th><th>Regla</th><th>Resultado</th></tr>
          <tr><td>PACIENTE — CONSULTA</td><td>1:N</td><td>R2</td><td>FK id_paciente en CONSULTA</td></tr>
          <tr><td>MEDICO — CONSULTA</td><td>1:N</td><td>R2</td><td>FK id_medico en CONSULTA</td></tr>
          <tr><td>CONSULTA — MEDICAMENTO</td><td>N:M</td><td>R3</td><td>Tabla RECETA</td></tr>
          <tr><td>MEDICO — SUCURSAL</td><td>N:M</td><td>R3</td><td>Tabla MEDICO_SUCURSAL</td></tr>
          <tr><td>PACIENTE — teléfono</td><td>multivaluado</td><td>R5</td><td>Tabla TELEFONO_PACIENTE</td></tr>
        </table>
      </div>
      <h3>Esquema relacional</h3>
      <div class="diagram">MEDICO(<u>id_medico</u>, nombre, especialidad, cedula, email)
SUCURSAL(<u>id_suc</u>, nombre, direccion, telefono)
MEDICO_SUCURSAL(<u>id_medico*</u>, <u>id_suc*</u>, horario)
PACIENTE(<u>id_paciente</u>, nombre, fecha_nac, nss)
TELEFONO_PACIENTE(<u>id_paciente*</u>, <u>numero</u>, tipo)
MEDICAMENTO(<u>id_med</u>, nombre, laboratorio, presentacion)
CONSULTA(<u>id_consulta</u>, fecha, diagnostico, costo, id_paciente*, id_medico*)
RECETA(<u>id_consulta*</u>, <u>id_med*</u>, dosis, duracion_dias)</div>
      <h3>DDL completo</h3>
      <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>MySQL — Clínica médica</span></div>
<pre><span class="cm">-- 1. Tablas sin FK</span>
<span class="kw">CREATE TABLE</span> MEDICO (
    id_medico    <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre       <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    especialidad <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>,
    cedula       <span class="kw">VARCHAR</span>(<span class="num">20</span>)  <span class="kw">NOT NULL UNIQUE</span>,
    email        <span class="kw">VARCHAR</span>(<span class="num">200</span>) <span class="kw">UNIQUE</span>
);
<span class="kw">CREATE TABLE</span> SUCURSAL (
    id_suc    <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre    <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>,
    direccion <span class="kw">VARCHAR</span>(<span class="num">300</span>),
    telefono  <span class="kw">VARCHAR</span>(<span class="num">15</span>)
);
<span class="kw">CREATE TABLE</span> PACIENTE (
    id_paciente <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre      <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    fecha_nac   <span class="kw">DATE NOT NULL</span>,
    nss         <span class="kw">VARCHAR</span>(<span class="num">20</span>) <span class="kw">UNIQUE</span>
);
<span class="kw">CREATE TABLE</span> MEDICAMENTO (
    id_med       <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre       <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    laboratorio  <span class="kw">VARCHAR</span>(<span class="num">100</span>),
    presentacion <span class="kw">VARCHAR</span>(<span class="num">100</span>)
);
<span class="cm">-- 2. Tablas con FK</span>
<span class="kw">CREATE TABLE</span> MEDICO_SUCURSAL (
    id_medico <span class="kw">INT NOT NULL</span>, id_suc <span class="kw">INT NOT NULL</span>,
    horario   <span class="kw">VARCHAR</span>(<span class="num">100</span>),
    <span class="kw">PRIMARY KEY</span>(id_medico, id_suc),
    <span class="kw">FOREIGN KEY</span>(id_medico) <span class="kw">REFERENCES</span> MEDICO(id_medico),
    <span class="kw">FOREIGN KEY</span>(id_suc)    <span class="kw">REFERENCES</span> SUCURSAL(id_suc)
);
<span class="kw">CREATE TABLE</span> TELEFONO_PACIENTE (
    id_paciente <span class="kw">INT NOT NULL</span>,
    numero      <span class="kw">VARCHAR</span>(<span class="num">20</span>) <span class="kw">NOT NULL</span>,
    tipo        <span class="kw">ENUM</span>(<span class="str">'casa'</span>,<span class="str">'movil'</span>,<span class="str">'trabajo'</span>) <span class="kw">DEFAULT</span> <span class="str">'movil'</span>,
    <span class="kw">PRIMARY KEY</span>(id_paciente, numero),
    <span class="kw">FOREIGN KEY</span>(id_paciente) <span class="kw">REFERENCES</span> PACIENTE(id_paciente)
);
<span class="kw">CREATE TABLE</span> CONSULTA (
    id_consulta <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    fecha       <span class="kw">DATETIME NOT NULL DEFAULT</span> <span class="fn">NOW</span>(),
    diagnostico <span class="kw">TEXT</span>,
    costo       <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL CHECK</span>(costo >= <span class="num">0</span>),
    id_paciente <span class="kw">INT NOT NULL</span>, id_medico <span class="kw">INT NOT NULL</span>,
    <span class="kw">FOREIGN KEY</span>(id_paciente) <span class="kw">REFERENCES</span> PACIENTE(id_paciente),
    <span class="kw">FOREIGN KEY</span>(id_medico)   <span class="kw">REFERENCES</span> MEDICO(id_medico)
);
<span class="kw">CREATE TABLE</span> RECETA (
    id_consulta   <span class="kw">INT NOT NULL</span>, id_med <span class="kw">INT NOT NULL</span>,
    dosis         <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>,
    duracion_dias <span class="kw">INT NOT NULL CHECK</span>(duracion_dias > <span class="num">0</span>),
    <span class="kw">PRIMARY KEY</span>(id_consulta, id_med),
    <span class="kw">FOREIGN KEY</span>(id_consulta) <span class="kw">REFERENCES</span> CONSULTA(id_consulta),
    <span class="kw">FOREIGN KEY</span>(id_med)      <span class="kw">REFERENCES</span> MEDICAMENTO(id_med)
);</pre></div>
      <h3>Fragmento del diccionario — Tabla CONSULTA</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Atributo</th><th>Tipo</th><th>Restricciones</th><th>Default</th><th>Descripción</th></tr>
          <tr><td><strong>id_consulta</strong></td><td>INT</td><td>PK, NOT NULL, AUTO_INC</td><td>AUTO</td><td>Identificador único de la consulta</td></tr>
          <tr><td><strong>fecha</strong></td><td>DATETIME</td><td>NOT NULL</td><td>NOW()</td><td>Fecha y hora en que se realizó la consulta</td></tr>
          <tr><td><strong>diagnostico</strong></td><td>TEXT</td><td>NULL permitido</td><td>NULL</td><td>Diagnóstico emitido por el médico</td></tr>
          <tr><td><strong>costo</strong></td><td>DECIMAL(10,2)</td><td>NOT NULL, CHECK ≥ 0</td><td>—</td><td>Costo de la consulta en pesos MXN</td></tr>
          <tr><td><strong>id_paciente</strong></td><td>INT</td><td>FK → PACIENTE, NOT NULL</td><td>—</td><td>Paciente atendido</td></tr>
          <tr><td><strong>id_medico</strong></td><td>INT</td><td>FK → MEDICO, NOT NULL</td><td>—</td><td>Médico que realizó la consulta</td></tr>
        </table>
      </div>
    </section>

    <section id="caso2">
      <div class="section-label"><span class="dot"></span>05 · Caso integrador 2</div>
      <h2>Caso Integrador 2 — Tienda en Línea</h2>
      <div class="card orange">
        <h4>📋 Enunciado</h4>
        <p>Una tienda en línea vende productos por categorías. Los clientes tienen múltiples direcciones. Las órdenes contienen varios productos. Los productos tienen imágenes múltiples y son surtidos por proveedores (N:M).</p>
      </div>
      <h3>Esquema relacional</h3>
      <div class="diagram">CATEGORIA(<u>id_cat</u>, nombre)
PROVEEDOR(<u>id_prov</u>, nombre, contacto, telefono)
CLIENTE(<u>id_cliente</u>, nombre, email, fecha_registro)
PRODUCTO(<u>id_prod</u>, nombre, precio, stock, id_cat*)
IMAGEN_PRODUCTO(<u>id_prod*</u>, <u>url</u>, es_portada)            -- R5
SUMINISTRO(<u>id_prov*</u>, <u>id_prod*</u>, precio_costo)           -- R3 N:M
DIRECCION(<u>id_dir</u>, calle, ciudad, cp, id_cliente*)
ORDEN(<u>id_orden</u>, fecha, total, estatus, id_cliente*, id_dir*)
DETALLE_ORDEN(<u>id_orden*</u>, <u>id_prod*</u>, cantidad, precio_unit) -- R3 N:M</div>
      <div class="code-block"><div class="code-header"><div class="code-dots"><i></i><i></i><i></i></div><span>MySQL — E-commerce completo</span></div>
<pre><span class="cm">-- Nivel 0: sin FK</span>
<span class="kw">CREATE TABLE</span> CATEGORIA  (<span class="kw">id_cat</span>  <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, nombre <span class="kw">VARCHAR</span>(<span class="num">80</span>) <span class="kw">NOT NULL UNIQUE</span>);
<span class="kw">CREATE TABLE</span> PROVEEDOR  (<span class="kw">id_prov</span> <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, nombre <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>, contacto <span class="kw">VARCHAR</span>(<span class="num">200</span>), telefono <span class="kw">VARCHAR</span>(<span class="num">20</span>));
<span class="kw">CREATE TABLE</span> CLIENTE    (<span class="kw">id_cliente</span> <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, nombre <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>, email <span class="kw">VARCHAR</span>(<span class="num">200</span>) <span class="kw">NOT NULL UNIQUE</span>, fecha_registro <span class="kw">DATETIME DEFAULT</span> <span class="fn">NOW</span>());

<span class="cm">-- Nivel 1: FK a nivel 0</span>
<span class="kw">CREATE TABLE</span> PRODUCTO (
    id_prod <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, nombre <span class="kw">VARCHAR</span>(<span class="num">200</span>) <span class="kw">NOT NULL</span>,
    precio  <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL CHECK</span>(precio > <span class="num">0</span>),
    stock   <span class="kw">INT NOT NULL DEFAULT</span> <span class="num">0</span> <span class="kw">CHECK</span>(stock >= <span class="num">0</span>),
    id_cat  <span class="kw">INT NOT NULL</span>, <span class="kw">FOREIGN KEY</span>(id_cat) <span class="kw">REFERENCES</span> CATEGORIA(id_cat)
);
<span class="kw">CREATE TABLE</span> DIRECCION (
    id_dir <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, calle <span class="kw">VARCHAR</span>(<span class="num">200</span>) <span class="kw">NOT NULL</span>,
    ciudad <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>, cp <span class="kw">VARCHAR</span>(<span class="num">10</span>),
    id_cliente <span class="kw">INT NOT NULL</span>, <span class="kw">FOREIGN KEY</span>(id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente)
);

<span class="cm">-- Nivel 2: FK a nivel 1</span>
<span class="kw">CREATE TABLE</span> IMAGEN_PRODUCTO (
    id_prod <span class="kw">INT NOT NULL</span>, url <span class="kw">VARCHAR</span>(<span class="num">500</span>) <span class="kw">NOT NULL</span>, es_portada <span class="kw">TINYINT</span>(<span class="num">1</span>) <span class="kw">DEFAULT</span> <span class="num">0</span>,
    <span class="kw">PRIMARY KEY</span>(id_prod, url), <span class="kw">FOREIGN KEY</span>(id_prod) <span class="kw">REFERENCES</span> PRODUCTO(id_prod)
);
<span class="kw">CREATE TABLE</span> SUMINISTRO (
    id_prov <span class="kw">INT NOT NULL</span>, id_prod <span class="kw">INT NOT NULL</span>, precio_costo <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>),
    <span class="kw">PRIMARY KEY</span>(id_prov, id_prod),
    <span class="kw">FOREIGN KEY</span>(id_prov) <span class="kw">REFERENCES</span> PROVEEDOR(id_prov),
    <span class="kw">FOREIGN KEY</span>(id_prod) <span class="kw">REFERENCES</span> PRODUCTO(id_prod)
);
<span class="kw">CREATE TABLE</span> ORDEN (
    id_orden   <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>, fecha <span class="kw">DATETIME NOT NULL DEFAULT</span> <span class="fn">NOW</span>(),
    total      <span class="kw">DECIMAL</span>(<span class="num">12</span>,<span class="num">2</span>) <span class="kw">NOT NULL</span>,
    estatus    <span class="kw">ENUM</span>(<span class="str">'pendiente'</span>,<span class="str">'enviado'</span>,<span class="str">'entregado'</span>,<span class="str">'cancelado'</span>) <span class="kw">DEFAULT</span> <span class="str">'pendiente'</span>,
    id_cliente <span class="kw">INT NOT NULL</span>, id_dir <span class="kw">INT NOT NULL</span>,
    <span class="kw">FOREIGN KEY</span>(id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente),
    <span class="kw">FOREIGN KEY</span>(id_dir)     <span class="kw">REFERENCES</span> DIRECCION(id_dir)
);
<span class="kw">CREATE TABLE</span> DETALLE_ORDEN (
    id_orden <span class="kw">INT NOT NULL</span>, id_prod <span class="kw">INT NOT NULL</span>,
    cantidad        <span class="kw">INT NOT NULL CHECK</span>(cantidad > <span class="num">0</span>),
    precio_unitario <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL</span>,
    <span class="kw">PRIMARY KEY</span>(id_orden, id_prod),
    <span class="kw">FOREIGN KEY</span>(id_orden) <span class="kw">REFERENCES</span> ORDEN(id_orden),
    <span class="kw">FOREIGN KEY</span>(id_prod)  <span class="kw">REFERENCES</span> PRODUCTO(id_prod)
);</pre></div>
    </section>

    <section id="practica">
      <div class="section-label"><span class="dot"></span>06 · Práctica</div>
      <h2>Ejercicio Integrador — Tu Proyecto Final</h2>
      <p>El ejercicio de esta semana <strong>es</strong> el trabajo de la Evidencia 2. Aplica el pipeline completo sobre el sistema de tu equipo:</p>
      <div class="ejercicio integrador" data-num="Ejercicio integrador">
        <h4>Pipeline completo para el proyecto del equipo</h4>
        <ol style="margin-top:12px; padding-left:20px;">
          <li style="margin-bottom:10px;"><strong>Análisis:</strong> 3–5 líneas: qué hace el sistema, quiénes son los usuarios, qué datos gestiona.</li>
          <li style="margin-bottom:10px;"><strong>Entidades y atributos:</strong> lista todas (mín. 5) con atributos clasificados por tipo.</li>
          <li style="margin-bottom:10px;"><strong>Relaciones:</strong> cardinalidad y regla de transformación (R1–R5) para cada relación.</li>
          <li style="margin-bottom:10px;"><strong>Diagrama ER:</strong> draw.io completo (Crow's Foot o Chen).</li>
          <li style="margin-bottom:10px;"><strong>Esquema relacional:</strong> notación textual TABLA(<u>pk</u>, atribs, fk*).</li>
          <li><strong>Diccionario:</strong> ficha completa de al menos 3 tablas del sistema.</li>
        </ol>
      </div>
      <div class="card yellow">
        <h4>⏰ Recordatorio — Evidencia 2</h4>
        <p>Presentación oral y entrega del documento escrito este <strong>viernes 29 de Mayo</strong>. Revisa el checklist antes de presentar.</p>
      </div>
    </section>

    <section id="evidencia">
      <div class="section-label"><span class="dot"></span>07 · Checklist Evidencia 2</div>
      <h2>Lista de Verificación — Evidencia 2</h2>
      <h3>Documento escrito</h3>
      <ul class="checklist">
        <li>Portada con nombre del sistema, integrantes, matrícula y fecha.</li>
        <li>Descripción del sistema (mín. media cuartilla).</li>
        <li>Justificación: por qué necesita BD (mín. 3 argumentos).</li>
        <li>Al menos 5 entidades con sus atributos principales.</li>
        <li>Relaciones entre entidades con cardinalidad indicada.</li>
        <li>Tecnología elegida (Web o Python) con justificación.</li>
        <li>Lista de módulos del sistema (mín. 4 módulos).</li>
        <li>Hoja de registro firmada por todos los integrantes.</li>
      </ul>
      <h3>Presentación oral (10 minutos)</h3>
      <ul class="checklist">
        <li>Nombre del sistema y descripción breve.</li>
        <li>Justificación oral: por qué necesita base de datos.</li>
        <li>Entidades principales mostradas (diagrama o pizarrón).</li>
        <li>Tecnología elegida y razón de la elección.</li>
        <li>Módulos del sistema presentados.</li>
        <li>División de trabajo entre integrantes.</li>
        <li>Todos los integrantes participan en la exposición.</li>
      </ul>
    </section>

    <section id="resumen">
      <div class="section-label"><span class="dot"></span>08 · Cierre Unidad II</div>
      <h2>Cierre de la Unidad II</h2>
      <div class="summary-grid">
        <div class="summary-item"><div class="icon">🔷</div><div class="val">Modelo ER</div><div class="label">Entidades · 6 tipos de atributos · Relaciones · Cardinalidad</div></div>
        <div class="summary-item"><div class="icon">📐</div><div class="val">5 Reglas</div><div class="label">Transformación completa ER → Relacional</div></div>
        <div class="summary-item"><div class="icon">🍃</div><div class="val">NoSQL</div><div class="label">4 tipos · ACID vs BASE · cuándo usar cada uno</div></div>
        <div class="summary-item"><div class="icon">📖</div><div class="val">Diccionario</div><div class="label">Ficha por tabla: nombre · tipo · restricciones · default · descripción</div></div>
        <div class="summary-item"><div class="icon">⚠️</div><div class="val">4 Errores</div><div class="label">FK lado equivocado · N:M sin intermedia · multivaluado en col. · orden DDL</div></div>
        <div class="summary-item"><div class="icon">🔄</div><div class="val">Pipeline</div><div class="label">Análisis → ER → Relacional → Diccionario → DDL</div></div>
      </div>
      <div class="card purple" style="margin-top:24px;">
        <h4>📚 Siguiente: Unidad III (Semana 6 — 08–12 Junio)</h4>
        <p>Construcción de bases de datos: comparativa de SGBD, <strong>normalización completa (1FN, 2FN, 3FN)</strong> e implementación física con DDL avanzado. Asegúrate de tener instalado <strong>MySQL Workbench</strong>.</p>
      </div>
    </section>

  </main>
</div>
</div>
</body>
</html>
