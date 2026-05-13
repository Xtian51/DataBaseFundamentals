<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bases de Datos — Semana 2: Modelo Entidad-Relación</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:       #0d1117;
    --surface:  #161b22;
    --surface2: #21262d;
    --border:   #30363d;
    --accent:   #2563eb;
    --accent2:  #0ea5e9;
    --green:    #22c55e;
    --yellow:   #eab308;
    --purple:   #a855f7;
    --red:      #ef4444;
    --text:     #e6edf3;
    --muted:    #8b949e;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: 'Inter', sans-serif; font-size: 15px; line-height: 1.7; }

  /* Header */
  header {
    background: linear-gradient(135deg, #0f172a 0%, #1a3a2a 50%, #0f172a 100%);
    border-bottom: 1px solid var(--border);
    padding: 48px 0 36px; text-align: center; position: relative; overflow: hidden;
  }
  header::before {
    content: ''; position: absolute; inset: 0;
    background: radial-gradient(ellipse 60% 60% at 50% 0%, rgba(34,197,94,.12), transparent);
    pointer-events: none;
  }
  .badge {
    display: inline-block;
    background: rgba(34,197,94,.12); border: 1px solid rgba(34,197,94,.35);
    color: #4ade80; font-family: 'JetBrains Mono', monospace;
    font-size: 11px; letter-spacing: .1em; padding: 4px 14px; border-radius: 20px; margin-bottom: 16px;
  }
  header h1 { font-family: 'Syne', sans-serif; font-size: clamp(24px,4vw,40px); font-weight: 800; color: #fff; }
  header h1 span { color: #4ade80; }
  header p { color: var(--muted); margin-top: 10px; font-size: 14px; }
  .pills { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; margin-top: 20px; }
  .pill { background: #21262d; border: 1px solid var(--border); border-radius: 20px; padding: 4px 14px; font-size: 12px; color: var(--muted); }

  /* Layout */
  .container { max-width: 960px; margin: 0 auto; padding: 0 24px; }
  .layout { display: grid; grid-template-columns: 200px 1fr; gap: 32px; padding: 40px 0 60px; align-items: start; }
  @media(max-width:760px){ .layout{ grid-template-columns:1fr; } .toc{ display:none; } }

  /* TOC */
  .toc { position: sticky; top: 20px; background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 16px; font-size: 13px; }
  .toc h4 { font-family: 'Syne', sans-serif; font-size: 11px; letter-spacing: .12em; text-transform: uppercase; color: var(--muted); margin-bottom: 12px; }
  .toc a { display: block; color: var(--muted); text-decoration: none; padding: 4px 0 4px 10px; border-left: 2px solid transparent; transition: all .2s; }
  .toc a:hover { color: #4ade80; border-color: #4ade80; }

  /* Sections */
  section { margin-bottom: 48px; scroll-margin-top: 24px; }
  .section-label { display: inline-flex; align-items: center; gap: 8px; font-family: 'JetBrains Mono', monospace; font-size: 11px; letter-spacing: .1em; text-transform: uppercase; color: #4ade80; margin-bottom: 8px; }
  .section-label .dot { width: 6px; height: 6px; background: #4ade80; border-radius: 50%; }
  h2 { font-family: 'Syne', sans-serif; font-size: 22px; font-weight: 800; color: #fff; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 1px solid var(--border); }
  h3 { font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 600; color: #4ade80; margin: 28px 0 12px; }
  p { color: #cdd5e0; margin-bottom: 14px; }
  ul, ol { padding-left: 22px; margin-bottom: 14px; }
  li { color: #cdd5e0; margin-bottom: 6px; }
  li strong { color: var(--text); }

  /* Cards */
  .card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 20px 24px; margin-bottom: 20px; }
  .card.accent  { border-left: 3px solid var(--accent); }
  .card.green   { border-left: 3px solid var(--green); }
  .card.yellow  { border-left: 3px solid var(--yellow); }
  .card.purple  { border-left: 3px solid var(--purple); }
  .card.red     { border-left: 3px solid var(--red); }
  .card h4 { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 600; margin-bottom: 8px; color: #fff; }
  .card p { margin: 0; font-size: 14px; }

  /* Definition */
  .definition { background: linear-gradient(135deg,rgba(34,197,94,.07),rgba(14,165,233,.05)); border: 1px solid rgba(34,197,94,.25); border-radius: 10px; padding: 20px 24px; margin: 20px 0; }
  .definition .label { font-family: 'JetBrains Mono', monospace; font-size: 10px; letter-spacing: .12em; color: #4ade80; text-transform: uppercase; margin-bottom: 8px; }
  .definition p { margin: 0; font-size: 15px; }

  /* Tables */
  .table-wrap { overflow-x: auto; margin: 20px 0; }
  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  th { background: #21262d; border: 1px solid #30363d; padding: 10px 14px; text-align: left; font-family: 'JetBrains Mono', monospace; font-size: 12px; color: #4ade80; letter-spacing: .06em; }
  td { background: #0d1117; border: 1px solid #30363d; padding: 10px 14px; color: #cdd5e0; }
  tr:nth-child(even) td { background: #161b22; }

  /* Code */
  .code-block { background: #161b22; border: 1px solid var(--border); border-radius: 10px; margin: 20px 0; overflow: hidden; }
  .code-header { background: #21262d; border-bottom: 1px solid var(--border); padding: 8px 16px; display: flex; align-items: center; justify-content: space-between; }
  .code-header span { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: var(--muted); }
  .code-dots { display: flex; gap: 6px; }
  .code-dots i { width: 10px; height: 10px; border-radius: 50%; display: inline-block; }
  .code-dots i:nth-child(1){ background:#ef4444; } .code-dots i:nth-child(2){ background:#eab308; } .code-dots i:nth-child(3){ background:#22c55e; }
  pre { padding: 18px 20px; overflow-x: auto; font-family: 'JetBrains Mono', monospace; font-size: 13px; line-height: 1.6; color: #e6edf3; background: #161b22; }
  .kw  { color: #79c0ff; } .str { color: #a5d6ff; } .cm { color: #8b949e; font-style: italic; }
  .fn  { color: #d2a8ff; } .num { color: #ffa657; } .op  { color: #ff7b72; }

  /* Diagram */
  .diagram { background: #0d1117; border: 1px solid var(--border); border-radius: 10px; padding: 20px; font-family: 'JetBrains Mono', monospace; font-size: 12px; color: #4ade80; overflow-x: auto; margin: 20px 0; white-space: pre; line-height: 1.5; }

  /* Ejercicios */
  .ejercicio { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 22px 24px; margin-bottom: 20px; position: relative; }
  .ejercicio::before { content: attr(data-num); position: absolute; top: -12px; left: 20px; background: var(--accent); color: #fff; font-family: 'JetBrains Mono', monospace; font-size: 11px; font-weight: 600; padding: 2px 12px; border-radius: 20px; }
  .ejercicio.practica::before { background: var(--green); }
  .ejercicio h4 { font-family: 'Syne', sans-serif; font-size: 15px; font-weight: 600; margin-bottom: 10px; color: #fff; }

  /* ER shapes */
  .er-legend { display: flex; gap: 16px; flex-wrap: wrap; margin: 16px 0; }
  .er-shape { display: flex; align-items: center; gap: 8px; font-size: 13px; color: var(--muted); }
  .er-box    { width: 48px; height: 28px; border: 2px solid #4ade80; border-radius: 4px; background: rgba(34,197,94,.08); }
  .er-diamond{ width: 36px; height: 36px; border: 2px solid #0ea5e9; background: rgba(14,165,233,.08); transform: rotate(45deg); }
  .er-ellipse{ width: 52px; height: 28px; border: 2px solid #a855f7; border-radius: 50%; background: rgba(168,85,247,.08); }
  .er-dellipse{ width: 52px; height: 28px; border: 2px solid #eab308; border-radius: 50%; box-shadow: 0 0 0 3px rgba(234,179,8,.25); background: rgba(234,179,8,.05); }

  /* Summary */
  .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 14px; margin-top: 20px; }
  .summary-item { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 14px 16px; text-align: center; }
  .summary-item .icon { font-size: 24px; margin-bottom: 6px; }
  .summary-item .label { font-size: 12px; color: var(--muted); }
  .summary-item .val { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 600; color: var(--text); }
</style>
</head>
<body>

<header>
  <div class="badge">ITIID · Bases de Datos · UPTap · U2</div>
  <h1>Semana 2 — <span>Modelo Entidad-Relación</span></h1>
  <p>Unidad II · Bases de datos relacionales y Modelo ER</p>
  <div class="pills">
    <span class="pill">📅 11 – 15 Mayo 2026</span>
    <span class="pill">⏱ Temas 2.1 y 2.2</span>
    <span class="pill">🎯 Diagramas ER con draw.io</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#relacional">🗃 BD Relacional</a>
    <a href="#er">🔷 Modelo ER</a>
    <a href="#entidades">📦 Entidades</a>
    <a href="#relaciones">🔗 Relaciones</a>
    <a href="#cardinalidad">📐 Cardinalidad</a>
    <a href="#llaves">🔑 Llaves</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <!-- ═══ BD RELACIONAL ═══ -->
    <section id="relacional">
      <div class="section-label"><span class="dot"></span>01 · Base de datos relacional</div>
      <h2>¿Qué es una Base de Datos Relacional?</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>Una <strong>base de datos relacional</strong> organiza los datos en <strong>tablas</strong> (denominadas formalmente <em>relaciones</em>), donde cada tabla está compuesta por filas (<em>tuplas</em>) y columnas (<em>atributos</em>). Las tablas se relacionan entre sí mediante valores compartidos llamados <strong>claves</strong>.</p>
      </div>

      <p>El modelo relacional fue propuesto por <strong>Edgar F. Codd</strong> en 1970 en su artículo <em>"A Relational Model of Data for Large Shared Data Banks"</em>, publicado en Communications of the ACM. Su base matemática es el <strong>álgebra relacional</strong>.</p>

      <h3>Terminología: formal vs. informal</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Término formal</th><th>Término informal</th><th>Descripción</th></tr>
          <tr><td>Relación</td><td>Tabla</td><td>Conjunto de tuplas con los mismos atributos</td></tr>
          <tr><td>Tupla</td><td>Fila / Registro</td><td>Un elemento individual de la relación</td></tr>
          <tr><td>Atributo</td><td>Columna / Campo</td><td>Característica o propiedad de la entidad</td></tr>
          <tr><td>Dominio</td><td>Tipo de dato</td><td>Conjunto de valores válidos para un atributo</td></tr>
          <tr><td>Grado</td><td>Número de columnas</td><td>Cantidad de atributos de la relación</td></tr>
          <tr><td>Cardinalidad</td><td>Número de filas</td><td>Cantidad de tuplas de la relación</td></tr>
        </table>
      </div>

      <h3>Propiedades de una relación (tabla)</h3>
      <div class="card green">
        <h4>✅ Reglas que toda tabla relacional debe cumplir</h4>
        <ul style="margin:8px 0 0; padding-left:18px;">
          <li>Cada celda contiene exactamente <strong>un valor atómico</strong> (no listas ni grupos).</li>
          <li>Todos los valores de una columna pertenecen al <strong>mismo dominio</strong>.</li>
          <li><strong>No hay filas duplicadas</strong>: cada tupla es única.</li>
          <li>El <strong>orden de las filas es irrelevante</strong> (no existe primera ni última fila).</li>
          <li>El <strong>orden de las columnas es irrelevante</strong> (se accede por nombre, no por posición).</li>
          <li>Cada columna tiene un <strong>nombre único</strong> dentro de la tabla.</li>
        </ul>
      </div>
    </section>

    <!-- ═══ MODELO ER ═══ -->
    <section id="er">
      <div class="section-label"><span class="dot"></span>02 · Modelo ER</div>
      <h2>Modelo Entidad-Relación (ER)</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>El <strong>Modelo Entidad-Relación</strong> es una herramienta de modelado conceptual propuesta por <strong>Peter Chen (1976)</strong> que describe la estructura de una base de datos a través de <em>entidades</em>, sus <em>atributos</em> y las <em>relaciones</em> entre ellas. Es independiente del SGBD y sirve como puente entre los requisitos del negocio y el diseño lógico.</p>
      </div>

      <h3>Simbología estándar del diagrama ER</h3>
      <div class="er-legend">
        <div class="er-shape"><div class="er-box"></div><span>Entidad (rectángulo)</span></div>
        <div class="er-shape"><div class="er-diamond"></div><span style="margin-left:8px">Relación (rombo)</span></div>
        <div class="er-shape"><div class="er-ellipse"></div><span>Atributo (elipse)</span></div>
        <div class="er-shape"><div class="er-dellipse"></div><span>Atrib. clave (elipse subrayada)</span></div>
      </div>

      <div class="diagram">
        ┌─────────────┐          ┌────────────────┐          ┌─────────────┐
        │   CLIENTE   │──────────◇   REALIZA   ◇──────────│    PEDIDO   │
        │  (entidad)  │  1     (relación)    N  │  (entidad)  │
        └──────┬──────┘          └────────────────┘          └──────┬──────┘
               │                                                     │
        ○ id_cliente (PK)                                   ○ id_pedido (PK)
        ○ nombre                                            ○ fecha
        ○ email                                             ○ total
        ○ telefono                                          ○ estado</div>

      <p style="font-size:13px; color:var(--muted);">💡 El diagrama ER es el <em>modelo conceptual</em>. Después se transforma al <em>modelo lógico</em> (tablas con claves foráneas) y finalmente al <em>modelo físico</em> (scripts SQL).</p>
    </section>

    <!-- ═══ ENTIDADES ═══ -->
    <section id="entidades">
      <div class="section-label"><span class="dot"></span>03 · Entidades y atributos</div>
      <h2>Entidades y Atributos</h2>

      <div class="definition">
        <div class="label">Entidad</div>
        <p>Una <strong>entidad</strong> es cualquier objeto del mundo real, distinguible de otros objetos, sobre el que queremos almacenar información. Se representa como un <strong>rectángulo</strong> en el diagrama ER.</p>
      </div>

      <h3>Tipos de entidades</h3>
      <div style="display:grid; grid-template-columns:1fr 1fr; gap:14px; margin:16px 0;">
        <div class="card accent">
          <h4>Entidad fuerte</h4>
          <p>Existe por sí misma, independiente de otras entidades. Tiene su propia clave primaria.<br><br>
          <strong>Ejemplo:</strong> CLIENTE, PRODUCTO, EMPLEADO</p>
        </div>
        <div class="card yellow">
          <h4>Entidad débil</h4>
          <p>Depende de otra entidad para existir. Su clave primaria incluye la clave de la entidad fuerte.<br><br>
          <strong>Ejemplo:</strong> DEPENDIENTE (depende de EMPLEADO)</p>
        </div>
      </div>

      <div class="definition">
        <div class="label">Atributo</div>
        <p>Un <strong>atributo</strong> es una propiedad o característica de una entidad o relación. Se representa como una <strong>elipse</strong> conectada a su entidad.</p>
      </div>

      <h3>Tipos de atributos</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Tipo</th><th>Descripción</th><th>Ejemplo</th><th>Símbolo ER</th></tr>
          <tr><td><strong>Simple</strong></td><td>No se puede subdividir</td><td>edad, precio</td><td>Elipse simple</td></tr>
          <tr><td><strong>Compuesto</strong></td><td>Se puede dividir en partes con significado propio</td><td>nombre → (nombre, apellido_p, apellido_m)</td><td>Elipse con sub-elipses</td></tr>
          <tr><td><strong>Monovaluado</strong></td><td>Un solo valor por entidad</td><td>fecha_nacimiento</td><td>Elipse simple</td></tr>
          <tr><td><strong>Multivaluado</strong></td><td>Puede tener varios valores</td><td>telefonos (puede tener 2 o 3)</td><td>Elipse doble</td></tr>
          <tr><td><strong>Derivado</strong></td><td>Su valor se calcula a partir de otro atributo</td><td>edad (calculada de fecha_nacimiento)</td><td>Elipse punteada</td></tr>
          <tr><td><strong>Clave (PK)</strong></td><td>Identifica unívocamente a cada entidad</td><td>id_cliente, matricula</td><td>Elipse con texto subrayado</td></tr>
        </table>
      </div>

      <div class="card red">
        <h4>⚠️ Atributo multivaluado en la práctica</h4>
        <p>Los atributos multivaluados <strong>no se implementan directamente</strong> en una tabla relacional — violarían la 1FN. Se convierten en una tabla separada. Ej: TELEFONO(id_cliente, numero) en lugar de una columna "telefonos" con múltiples valores.</p>
      </div>
    </section>

    <!-- ═══ RELACIONES ═══ -->
    <section id="relaciones">
      <div class="section-label"><span class="dot"></span>04 · Relaciones</div>
      <h2>Relaciones entre Entidades</h2>

      <div class="definition">
        <div class="label">Relación</div>
        <p>Una <strong>relación</strong> (en el modelo ER) es una asociación entre dos o más entidades. Se representa como un <strong>rombo</strong> conectado a las entidades participantes. No confundir con "tabla" (que en el modelo relacional también se llama relación).</p>
      </div>

      <h3>Grado de una relación</h3>
      <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:12px; margin:16px 0;">
        <div class="card accent">
          <h4>Unaria (grado 1)</h4>
          <p>Una entidad se relaciona consigo misma.<br><br><strong>Ej:</strong> EMPLEADO <em>supervisa</em> EMPLEADO</p>
        </div>
        <div class="card green">
          <h4>Binaria (grado 2)</h4>
          <p>Dos entidades distintas participan. Es el tipo más común.<br><br><strong>Ej:</strong> CLIENTE <em>realiza</em> PEDIDO</p>
        </div>
        <div class="card purple">
          <h4>Ternaria (grado 3)</h4>
          <p>Tres entidades participan simultáneamente.<br><br><strong>Ej:</strong> MÉDICO <em>prescribe</em> MEDICAMENTO a PACIENTE</p>
        </div>
      </div>

      <h3>Participación total vs. parcial</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Tipo de participación</th><th>Significado</th><th>Símbolo</th><th>Ejemplo</th></tr>
          <tr><td><strong>Total</strong></td><td>Toda instancia de la entidad DEBE participar en la relación</td><td>Línea doble ══</td><td>Todo PEDIDO debe pertenecer a un CLIENTE</td></tr>
          <tr><td><strong>Parcial</strong></td><td>Una instancia PUEDE o no participar en la relación</td><td>Línea simple ─</td><td>Un CLIENTE puede o no tener PEDIDOS</td></tr>
        </table>
      </div>
    </section>

    <!-- ═══ CARDINALIDAD ═══ -->
    <section id="cardinalidad">
      <div class="section-label"><span class="dot"></span>05 · Cardinalidad</div>
      <h2>Cardinalidad de las Relaciones</h2>

      <p>La <strong>cardinalidad</strong> indica cuántas instancias de una entidad pueden asociarse con cuántas instancias de otra entidad a través de una relación.</p>

      <h3>Tipos de cardinalidad</h3>

      <div class="card accent">
        <h4>1 : 1 — Uno a Uno</h4>
        <p>Una instancia de A se asocia con <strong>una sola</strong> instancia de B, y viceversa.</p>
      </div>
      <div class="diagram">
  PERSONA ──────────────── PASAPORTE
    1                           1
  (una persona tiene un solo pasaporte; un pasaporte pertenece a una sola persona)</div>

      <div class="card green" style="margin-top:16px;">
        <h4>1 : N — Uno a Muchos</h4>
        <p>Una instancia de A se asocia con <strong>muchas</strong> instancias de B, pero cada instancia de B se asocia con <strong>una sola</strong> de A. Es el más común.</p>
      </div>
      <div class="diagram">
  CLIENTE ──────────────── PEDIDO
    1                          N
  (un cliente puede tener muchos pedidos; cada pedido pertenece a un solo cliente)</div>

      <div class="card purple" style="margin-top:16px;">
        <h4>N : M — Muchos a Muchos</h4>
        <p>Una instancia de A se asocia con <strong>muchas</strong> de B, y viceversa. Requiere una <strong>tabla intermedia</strong> en la implementación relacional.</p>
      </div>
      <div class="diagram">
  ESTUDIANTE ─────────────── MATERIA
      N                          M
  (un estudiante cursa varias materias; una materia es cursada por varios estudiantes)
  → Tabla intermedia: INSCRIPCION(id_estudiante, id_materia, calificacion)</div>

      <h3>Notaciones de cardinalidad</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Notación</th><th>Representación 1:N</th><th>Dónde se usa</th></tr>
          <tr><td>Chen (original)</td><td>1 — rombo — N</td><td>Diagramas académicos clásicos</td></tr>
          <tr><td>Crow's Foot (pata de cuervo)</td><td>línea recta — línea bifurcada</td><td>draw.io, Lucidchart, herramientas CASE</td></tr>
          <tr><td>Notación UML</td><td>1..1 — 0..*</td><td>Diagramas de clases UML</td></tr>
          <tr><td>Min-Max</td><td>(1,1) — (0,N)</td><td>Especificación formal</td></tr>
        </table>
      </div>
      <p style="font-size:13px; color:var(--muted);">💡 En este curso usaremos principalmente la notación <strong>Crow's Foot</strong> en draw.io y la notación <strong>Chen</strong> para los diagramas conceptuales.</p>
    </section>

    <!-- ═══ LLAVES ═══ -->
    <section id="llaves">
      <div class="section-label"><span class="dot"></span>06 · Llaves</div>
      <h2>Llaves (Claves)</h2>

      <div class="table-wrap">
        <table>
          <tr><th>Tipo de llave</th><th>Descripción</th><th>Ejemplo</th></tr>
          <tr>
            <td><strong>Superllave</strong></td>
            <td>Cualquier conjunto de atributos que identifica únicamente una tupla</td>
            <td>{id_cliente}, {id_cliente, nombre}, {email}</td>
          </tr>
          <tr>
            <td><strong>Llave candidata</strong></td>
            <td>Superllave mínima (sin atributos redundantes)</td>
            <td>{id_cliente}, {email}</td>
          </tr>
          <tr>
            <td><strong>Llave primaria (PK)</strong></td>
            <td>La llave candidata elegida como identificador principal. No puede ser NULL ni repetirse.</td>
            <td>id_cliente</td>
          </tr>
          <tr>
            <td><strong>Llave foránea (FK)</strong></td>
            <td>Atributo que referencia la PK de otra tabla. Implementa la relación entre tablas.</td>
            <td>id_cliente en tabla PEDIDO</td>
          </tr>
          <tr>
            <td><strong>Llave compuesta</strong></td>
            <td>PK formada por dos o más atributos. Común en tablas intermedias N:M.</td>
            <td>(id_estudiante, id_materia) en INSCRIPCION</td>
          </tr>
          <tr>
            <td><strong>Llave natural</strong></td>
            <td>Atributo del dominio del problema que puede usarse como PK</td>
            <td>RFC, CURP, matrícula</td>
          </tr>
          <tr>
            <td><strong>Llave sustituta (surrogate)</strong></td>
            <td>ID artificial generado por el sistema, sin significado de negocio</td>
            <td>AUTO_INCREMENT: 1, 2, 3…</td>
          </tr>
        </table>
      </div>

      <div class="card yellow">
        <h4>💡 Llave natural vs. sustituta — ¿cuándo usar cada una?</h4>
        <p>Usa <strong>llave natural</strong> cuando el identificador del dominio es estable y único (ej: CURP para personas físicas en México). Usa <strong>llave sustituta</strong> (AUTO_INCREMENT) cuando no hay un identificador natural confiable, o cuando la llave natural puede cambiar (los emails cambian; el AUTO_INCREMENT no).</p>
      </div>

      <h3>Integridad referencial</h3>
      <div class="card accent">
        <h4>Regla de integridad referencial</h4>
        <p>Si una tabla B tiene una FK que referencia la PK de la tabla A, entonces <strong>todo valor de esa FK debe existir como PK en A</strong>, o ser NULL. El SGBD impide insertar en B un valor que no exista en A, y no permite eliminar de A un registro que tenga dependientes en B (a menos que se use CASCADE).</p>
      </div>
    </section>

    <!-- ═══ EJEMPLOS ═══ -->
    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>07 · Ejemplos</div>
      <h2>Ejemplos Demostrativos</h2>

      <!-- Ejemplo 1 -->
      <div class="ejercicio" data-num="Ejemplo 1">
        <h4>Diagrama ER — Sistema de veterinaria</h4>
        <p>Modelamos una veterinaria que atiende mascotas y lleva historial de consultas:</p>
        <div class="diagram">
         ┌──────────────┐        ┌─────────────────┐        ┌──────────────┐
         │   PROPIETARIO│        │     MASCOTA      │        │  VETERINARIO │
         │──────────────│        │─────────────────-│        │──────────────│
         │ id_prop (PK) │1──────N│ id_mascota (PK)  │        │ id_vet (PK)  │
         │ nombre       │        │ nombre           │        │ nombre       │
         │ telefono     │        │ especie          │        │ especialidad │
         │ email        │        │ raza             │        └──────┬───────┘
         └──────────────┘        │ fecha_nacimiento │               │
                                 │ id_prop (FK)     │               │ N
                                 └────────┬─────────┘               │
                                          │ N           ┌───────────◇ ATIENDE ◇──┘
                                          └─────────────┤
                                                   ┌────▼────────────┐
                                                   │    CONSULTA     │
                                                   │─────────────────│
                                                   │ id_consulta(PK) │
                                                   │ fecha           │
                                                   │ diagnostico     │
                                                   │ costo           │
                                                   │ id_mascota (FK) │
                                                   │ id_vet (FK)     │
                                                   └─────────────────┘</div>
        <p><strong>Cardinalidades:</strong> Un propietario puede tener N mascotas (1:N). Una mascota puede tener N consultas con N veterinarios (N:M → tabla CONSULTA resuelve esto con las dos FK).</p>
      </div>

      <!-- Ejemplo 2 -->
      <div class="ejercicio" data-num="Ejemplo 2">
        <h4>Identificar tipos de atributos — Entidad EMPLEADO</h4>
        <p>Clasifica cada atributo de la entidad EMPLEADO:</p>
        <div class="table-wrap">
          <table>
            <tr><th>Atributo</th><th>Tipo</th><th>Justificación</th></tr>
            <tr><td>id_empleado</td><td>Simple, Clave (PK)</td><td>Identifica unívocamente; no se puede subdividir</td></tr>
            <tr><td>nombre_completo</td><td>Compuesto</td><td>Se divide en: nombre, apellido_p, apellido_m</td></tr>
            <tr><td>fecha_nacimiento</td><td>Simple, Monovaluado</td><td>Un solo valor, no se subdivide para este contexto</td></tr>
            <tr><td>edad</td><td>Derivado</td><td>Se calcula: YEAR(NOW()) - YEAR(fecha_nacimiento)</td></tr>
            <tr><td>telefonos</td><td>Multivaluado</td><td>Un empleado puede tener teléfono casa, móvil, trabajo</td></tr>
            <tr><td>salario</td><td>Simple, Monovaluado</td><td>Un valor numérico único</td></tr>
          </table>
        </div>
        <p style="margin-top:10px; font-size:13px; color:var(--muted);">⚠️ <code>edad</code> no se almacena en la BD — se calcula en cada consulta. <code>telefonos</code> se convierte en tabla separada: <code>TELEFONO_EMPLEADO(id_empleado FK, numero, tipo)</code>.</p>
      </div>

      <!-- Ejemplo 3 -->
      <div class="ejercicio" data-num="Ejemplo 3">
        <h4>De diagrama ER a tablas SQL — Relación N:M</h4>
        <p>La relación <strong>ESTUDIANTE cursa MATERIA</strong> (N:M) se transforma así:</p>

        <div class="diagram">
  ER conceptual:
  ESTUDIANTE (N) ──── cursa ──── (M) MATERIA

  ↓ Transformación al modelo relacional ↓

  ESTUDIANTE(id_est PK, nombre, carrera)
  MATERIA(id_mat PK, nombre, creditos, semestre)
  INSCRIPCION(id_est FK, id_mat FK, calificacion, fecha_inscripcion)
               └── PK compuesta: (id_est, id_mat) ──┘</div>

        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><i></i><i></i><i></i></div>
            <span>SQL — Implementación de la relación N:M</span>
          </div>
          <pre><span class="kw">CREATE TABLE</span> ESTUDIANTE (
    id_est   <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre   <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    carrera  <span class="kw">VARCHAR</span>(<span class="num">100</span>)
);

<span class="kw">CREATE TABLE</span> MATERIA (
    id_mat   <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre   <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>,
    creditos <span class="kw">INT</span>,
    semestre <span class="kw">INT</span>
);

<span class="cm">-- Tabla intermedia que resuelve la relación N:M</span>
<span class="kw">CREATE TABLE</span> INSCRIPCION (
    id_est          <span class="kw">INT NOT NULL</span>,
    id_mat          <span class="kw">INT NOT NULL</span>,
    calificacion    <span class="kw">DECIMAL</span>(<span class="num">4</span>,<span class="num">2</span>),
    fecha_inscripcion <span class="kw">DATE NOT NULL</span>,
    <span class="cm">-- Llave primaria compuesta</span>
    <span class="kw">PRIMARY KEY</span> (id_est, id_mat),
    <span class="cm">-- Llaves foráneas que garantizan integridad referencial</span>
    <span class="kw">FOREIGN KEY</span> (id_est) <span class="kw">REFERENCES</span> ESTUDIANTE(id_est),
    <span class="kw">FOREIGN KEY</span> (id_mat) <span class="kw">REFERENCES</span> MATERIA(id_mat)
);</pre>
        </div>
      </div>
    </section>

    <!-- ═══ PRÁCTICA ═══ -->
    <section id="practica">
      <div class="section-label"><span class="dot"></span>08 · Práctica</div>
      <h2>Ejercicios de Práctica</h2>
      <p>Usa <strong>draw.io</strong> (app.diagrams.net) o lápiz y papel para los diagramas. Entrega imagen + descripción escrita.</p>

      <!-- P1 -->
      <div class="ejercicio practica" data-num="Práctica 1">
        <h4>Identificación de entidades, atributos y relaciones</h4>
        <p>Lee el siguiente enunciado y responde las preguntas:</p>
        <div class="card yellow" style="margin: 12px 0;">
          <h4>📋 Caso: Agencia de viajes "Tapachula Tours"</h4>
          <p>La agencia gestiona clientes que reservan paquetes turísticos. Cada paquete incluye varios servicios (vuelo, hotel, tours). Los clientes pueden hacer múltiples reservas a lo largo del tiempo. Cada reserva tiene un guía turístico asignado. Los guías hablan varios idiomas.</p>
        </div>
        <ol style="padding-left:20px; margin-top:12px;">
          <li>Identifica al menos <strong>5 entidades</strong> del caso.</li>
          <li>Para cada entidad, lista <strong>4 atributos</strong> e indica el tipo de cada uno (simple, compuesto, derivado, multivaluado, clave).</li>
          <li>Identifica al menos <strong>4 relaciones</strong> entre entidades y determina su cardinalidad (1:1, 1:N, N:M).</li>
          <li>Señala cuáles relaciones serán N:M y qué tabla intermedia necesitarían.</li>
        </ol>
      </div>

      <!-- P2 -->
      <div class="ejercicio practica" data-num="Práctica 2">
        <h4>Diseño del diagrama ER — Sistema de farmacia</h4>
        <p>Diseña el diagrama Entidad-Relación completo para el siguiente caso:</p>
        <div class="card accent" style="margin: 12px 0;">
          <h4>📋 Caso: Farmacia "San Agustín"</h4>
          <p>Una farmacia necesita gestionar su inventario de medicamentos y ventas. Maneja proveedores que surten medicamentos. Los clientes pueden comprar varios medicamentos en una venta. Cada medicamento pertenece a una categoría y tiene un laboratorio fabricante. Los empleados registran las ventas.</p>
        </div>
        <ol style="padding-left:20px; margin-top:12px;">
          <li>Dibuja el <strong>diagrama ER completo</strong> con todas las entidades, atributos y relaciones. Usa notación Chen o Crow's Foot en draw.io.</li>
          <li>Indica la <strong>cardinalidad</strong> de cada relación.</li>
          <li>Señala la <strong>PK</strong> de cada entidad (subrayada en el diagrama).</li>
          <li>Identifica las relaciones que generarán tablas intermedias.</li>
        </ol>
      </div>

      <!-- P3 -->
      <div class="ejercicio practica" data-num="Práctica 3">
        <h4>Transformación ER → Tablas relacionales</h4>
        <p>Dado el siguiente fragmento de diagrama ER de un sistema de biblioteca:</p>
        <div class="diagram">
  AUTOR (id_autor, nombre, nacionalidad)
     N
     │
  ESCRIBE
     │
     M
  LIBRO (id_libro, titulo, isbn, año, id_editorial FK)
     │ N
     │
  PRESTAMO (id_prestamo, fecha_prestamo, fecha_devolucion)
     │ M
     │
  SOCIO (id_socio, nombre, email, telefono)</div>
        <ol style="padding-left:20px; margin-top:12px;">
          <li>Escribe el <strong>DDL completo</strong> (CREATE TABLE) para todas las tablas, incluyendo PK, FK y constraints básicos.</li>
          <li>Identifica qué relaciones son N:M y crea las <strong>tablas intermedias</strong> correspondientes con su PK compuesta.</li>
          <li>Inserta <strong>3 registros de ejemplo</strong> en cada tabla usando INSERT INTO.</li>
        </ol>
      </div>
    </section>

    <!-- ═══ RESUMEN ═══ -->
    <section id="resumen">
      <div class="section-label"><span class="dot"></span>09 · Resumen</div>
      <h2>Puntos Clave de la Semana 2</h2>

      <div class="summary-grid">
        <div class="summary-item">
          <div class="icon">🗃️</div>
          <div class="val">BD Relacional</div>
          <div class="label">Tablas, tuplas, atributos. Base: álgebra relacional (Codd 1970)</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔷</div>
          <div class="val">Modelo ER</div>
          <div class="label">Diseño conceptual. Entidades + Atributos + Relaciones (Chen 1976)</div>
        </div>
        <div class="summary-item">
          <div class="icon">📦</div>
          <div class="val">Entidades</div>
          <div class="label">Fuerte vs. débil. Atributos: simple, compuesto, derivado, multivaluado</div>
        </div>
        <div class="summary-item">
          <div class="icon">📐</div>
          <div class="val">Cardinalidad</div>
          <div class="label">1:1 · 1:N · N:M. Las N:M requieren tabla intermedia</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔑</div>
          <div class="val">Llaves</div>
          <div class="label">PK identifica. FK relaciona. Compuesta en tablas intermedias</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔗</div>
          <div class="val">Integridad ref.</div>
          <div class="label">FK debe existir en la tabla referenciada o ser NULL</div>
        </div>
      </div>

      <div class="card green" style="margin-top:24px;">
        <h4>📚 Para la próxima semana (Semana 3 — 18–22 Mayo)</h4>
        <p>Continuamos con la Unidad II: <strong>Modelo Relacional, Bases de Datos No Relacionales y Diccionario de Datos</strong>. Trae tu diagrama ER de la Práctica 2 terminado — lo usaremos para transformarlo al modelo relacional.</p>
      </div>
    </section>

  </main>
</div>
</div>
</body>
</html>
