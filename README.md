<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bases de Datos — Semana 3: Modelo Relacional, NoSQL y Diccionario de Datos</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:      #0d1117;
    --surface: #161b22;
    --border:  #30363d;
    --accent:  #f97316;
    --accent2: #fb923c;
    --green:   #22c55e;
    --blue:    #3b82f6;
    --purple:  #a855f7;
    --yellow:  #eab308;
    --red:     #ef4444;
    --text:    #e6edf3;
    --muted:   #8b949e;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: #0d1117; color: #e6edf3; font-family: 'Inter', sans-serif; font-size: 15px; line-height: 1.7; }

  /* ── Header ── */
  header {
    background: linear-gradient(135deg, #0f172a 0%, #2d1a0e 50%, #0f172a 100%);
    border-bottom: 1px solid #30363d;
    padding: 48px 0 36px; text-align: center; position: relative; overflow: hidden;
  }
  header::before {
    content: ''; position: absolute; inset: 0;
    background: radial-gradient(ellipse 60% 60% at 50% 0%, rgba(249,115,22,.13), transparent);
    pointer-events: none;
  }
  .badge { display: inline-block; background: rgba(249,115,22,.12); border: 1px solid rgba(249,115,22,.35); color: #fb923c; font-family: 'JetBrains Mono', monospace; font-size: 11px; letter-spacing: .1em; padding: 4px 14px; border-radius: 20px; margin-bottom: 16px; }
  header h1 { font-family: 'Syne', sans-serif; font-size: clamp(22px,4vw,38px); font-weight: 800; color: #fff; }
  header h1 span { color: #fb923c; }
  header p { color: #8b949e; margin-top: 10px; font-size: 14px; }
  .pills { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; margin-top: 20px; }
  .pill { background: #21262d; border: 1px solid #30363d; border-radius: 20px; padding: 4px 14px; font-size: 12px; color: #8b949e; }

  /* ── Layout ── */
  .container { max-width: 960px; margin: 0 auto; padding: 0 24px; }
  .layout { display: grid; grid-template-columns: 200px 1fr; gap: 32px; padding: 40px 0 60px; align-items: start; }
  @media(max-width:760px){ .layout{ grid-template-columns:1fr; } .toc{ display:none; } }

  /* ── TOC ── */
  .toc { position: sticky; top: 20px; background: #161b22; border: 1px solid #30363d; border-radius: 10px; padding: 16px; font-size: 13px; }
  .toc h4 { font-family: 'Syne', sans-serif; font-size: 11px; letter-spacing: .12em; text-transform: uppercase; color: #8b949e; margin-bottom: 12px; }
  .toc a { display: block; color: #8b949e; text-decoration: none; padding: 4px 0 4px 10px; border-left: 2px solid transparent; transition: all .2s; }
  .toc a:hover { color: #fb923c; border-color: #fb923c; }

  /* ── Sections ── */
  section { margin-bottom: 48px; scroll-margin-top: 24px; }
  .section-label { display: inline-flex; align-items: center; gap: 8px; font-family: 'JetBrains Mono', monospace; font-size: 11px; letter-spacing: .1em; text-transform: uppercase; color: #fb923c; margin-bottom: 8px; }
  .section-label .dot { width: 6px; height: 6px; background: #fb923c; border-radius: 50%; }
  h2 { font-family: 'Syne', sans-serif; font-size: 22px; font-weight: 800; color: #fff; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 1px solid #30363d; }
  h3 { font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 600; color: #fb923c; margin: 28px 0 12px; }
  p { color: #cdd5e0; margin-bottom: 14px; }
  ul, ol { padding-left: 22px; margin-bottom: 14px; }
  li { color: #cdd5e0; margin-bottom: 6px; }
  li strong { color: #e6edf3; }

  /* ── Cards ── */
  .card { background: #161b22; border: 1px solid #30363d; border-radius: 10px; padding: 20px 24px; margin-bottom: 20px; }
  .card.orange { border-left: 3px solid #f97316; }
  .card.green  { border-left: 3px solid #22c55e; }
  .card.blue   { border-left: 3px solid #3b82f6; }
  .card.yellow { border-left: 3px solid #eab308; }
  .card.purple { border-left: 3px solid #a855f7; }
  .card.red    { border-left: 3px solid #ef4444; }
  .card h4 { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 600; margin-bottom: 8px; color: #fff; }
  .card p { margin: 0; font-size: 14px; }

  /* ── Definition ── */
  .definition { background: linear-gradient(135deg,rgba(249,115,22,.07),rgba(251,146,60,.04)); border: 1px solid rgba(249,115,22,.28); border-radius: 10px; padding: 20px 24px; margin: 20px 0; }
  .definition .label { font-family: 'JetBrains Mono', monospace; font-size: 10px; letter-spacing: .12em; color: #fb923c; text-transform: uppercase; margin-bottom: 8px; }
  .definition p { margin: 0; font-size: 15px; }

  /* ── Tables — todos los valores sólidos, sin transparent, con !important ── */
  .table-wrap { overflow-x: auto; margin: 20px 0; }
  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  th { background: #21262d !important; border: 1px solid #30363d !important; padding: 10px 14px; text-align: left; font-family: 'JetBrains Mono', monospace; font-size: 12px; color: #fb923c !important; letter-spacing: .06em; }
  td { background: #0d1117 !important; border: 1px solid #30363d !important; padding: 10px 14px; color: #cdd5e0 !important; }
  tr:nth-child(even) td { background: #161b22 !important; }

  /* ── Code — background sólido con !important ── */
  .code-block { background: #161b22; border: 1px solid #30363d; border-radius: 10px; margin: 20px 0; overflow: hidden; }
  .code-header { background: #21262d; border-bottom: 1px solid #30363d; padding: 8px 16px; display: flex; align-items: center; justify-content: space-between; }
  .code-header span { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: #8b949e; }
  .code-dots { display: flex; gap: 6px; }
  .code-dots i { width: 10px; height: 10px; border-radius: 50%; display: inline-block; }
  .code-dots i:nth-child(1){ background:#ef4444; } .code-dots i:nth-child(2){ background:#eab308; } .code-dots i:nth-child(3){ background:#22c55e; }
  pre { padding: 18px 20px !important; overflow-x: auto; font-family: 'JetBrains Mono', monospace !important; font-size: 13px; line-height: 1.6; color: #e6edf3 !important; background: #161b22 !important; }
  .kw  { color: #79c0ff; } .str { color: #a5d6ff; } .cm { color: #8b949e; font-style: italic; }
  .fn  { color: #d2a8ff; } .num { color: #ffa657; } .op  { color: #ff7b72; }

  /* ── Diagram ── */
  .diagram { background: #0d1117 !important; border: 1px solid #30363d; border-radius: 10px; padding: 20px; font-family: 'JetBrains Mono', monospace; font-size: 12px; color: #fb923c !important; overflow-x: auto; margin: 20px 0; white-space: pre; line-height: 1.5; }

  /* ── Ejercicios ── */
  .ejercicio { background: #161b22; border: 1px solid #30363d; border-radius: 10px; padding: 22px 24px; margin-bottom: 20px; position: relative; }
  .ejercicio::before { content: attr(data-num); position: absolute; top: -12px; left: 20px; background: #f97316; color: #fff; font-family: 'JetBrains Mono', monospace; font-size: 11px; font-weight: 600; padding: 2px 12px; border-radius: 20px; }
  .ejercicio.practica::before { background: #22c55e; }
  .ejercicio h4 { font-family: 'Syne', sans-serif; font-size: 15px; font-weight: 600; margin-bottom: 10px; color: #fff; }

  /* ── Summary ── */
  .summary-grid { display: grid; grid-template-columns: repeat(auto-fit,minmax(180px,1fr)); gap: 14px; margin-top: 20px; }
  .summary-item { background: #161b22; border: 1px solid #30363d; border-radius: 8px; padding: 14px 16px; text-align: center; }
  .summary-item .icon { font-size: 24px; margin-bottom: 6px; }
  .summary-item .label { font-size: 12px; color: #8b949e; }
  .summary-item .val { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 600; color: #e6edf3; }

  /* ── DD table especial ── */
  .dd-table td:first-child { color: #fb923c !important; font-family: 'JetBrains Mono', monospace; font-size: 13px; }
</style>
</head>
<body>

<header>
  <div class="badge">ITIID · Bases de Datos · UPTap · U2</div>
  <h1>Semana 3 — <span>Modelo Relacional, NoSQL y Diccionario de Datos</span></h1>
  <p>Unidad II · Temas 2.3, 2.4 y 2.5</p>
  <div class="pills">
    <span class="pill">📅 18 – 22 Mayo 2026</span>
    <span class="pill">⏱ Temas 2.3 · 2.4 · 2.5</span>
    <span class="pill">🎯 Transformación ER → Relacional</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#relacional">📐 Modelo Relacional</a>
    <a href="#transformacion">🔄 Transformación ER</a>
    <a href="#normalizacion">📏 Normalización intro</a>
    <a href="#nosql">🍃 NoSQL</a>
    <a href="#diccionario">📖 Diccionario</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <!-- ═══ MODELO RELACIONAL ═══ -->
    <section id="relacional">
      <div class="section-label"><span class="dot"></span>01 · Modelo Relacional</div>
      <h2>Modelo Relacional</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>El <strong>modelo relacional</strong> es el modelo lógico que representa los datos y sus relaciones usando exclusivamente <strong>tablas</strong>. A diferencia del modelo ER (conceptual), el modelo relacional es directamente implementable en un SGBD. Se expresa mediante un <strong>esquema relacional</strong>: el conjunto de tablas con sus atributos, claves y restricciones.</p>
      </div>

      <h3>Notación del esquema relacional</h3>
      <p>Un esquema relacional se escribe así:</p>
      <div class="diagram">
  NOMBRE_TABLA(<u>pk_atributo</u>, atributo2, atributo3, <u style="color:#3b82f6">fk_atributo</u>)

  Donde:
    ─ Nombre en MAYÚSCULAS
    ─ Atributo subrayado = Llave Primaria (PK)
    ─ Atributo en cursiva subrayado = Llave Foránea (FK)

  Ejemplos:
  CLIENTE(<u>id_cliente</u>, nombre, email, telefono)
  PEDIDO(<u>id_pedido</u>, fecha, total, id_cliente*)
                                        └── * indica FK → CLIENTE(id_cliente)</div>

      <h3>Restricciones del modelo relacional</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Restricción</th><th>Descripción</th><th>En SQL</th></tr>
          <tr><td><strong>Dominio</strong></td><td>Cada atributo solo acepta valores de su dominio definido</td><td>tipos de datos, CHECK</td></tr>
          <tr><td><strong>Clave</strong></td><td>La PK debe ser única y no nula en cada tupla</td><td>PRIMARY KEY</td></tr>
          <tr><td><strong>Integridad de entidad</strong></td><td>Ningún atributo de la PK puede ser NULL</td><td>NOT NULL en PK</td></tr>
          <tr><td><strong>Integridad referencial</strong></td><td>Toda FK debe existir como PK en la tabla referenciada, o ser NULL</td><td>FOREIGN KEY</td></tr>
          <tr><td><strong>Semántica de usuario</strong></td><td>Reglas de negocio específicas</td><td>CHECK, TRIGGER</td></tr>
        </table>
      </div>
    </section>

    <!-- ═══ TRANSFORMACIÓN ER → RELACIONAL ═══ -->
    <section id="transformacion">
      <div class="section-label"><span class="dot"></span>02 · Transformación ER → Relacional</div>
      <h2>Transformación del Modelo ER al Modelo Relacional</h2>

      <p>Esta transformación convierte el modelo <strong>conceptual</strong> (ER) al modelo <strong>lógico</strong> (relacional). Existen reglas formales para cada caso:</p>

      <h3>Regla 1 — Entidad fuerte</h3>
      <div class="card orange">
        <h4>Cada entidad fuerte → una tabla</h4>
        <p>Los atributos simples se convierten en columnas. El atributo clave se convierte en PK. Los atributos compuestos se aplanan (se guardan sus componentes simples).</p>
      </div>
      <div class="diagram">
  ER:  PRODUCTO(id_prod, nombre, precio, descripcion)
       ──────────────────────────────────────────────
  SQL: PRODUCTO(<u>id_prod</u>, nombre, precio, descripcion)

  CREATE TABLE PRODUCTO (
      id_prod     INT          PRIMARY KEY AUTO_INCREMENT,
      nombre      VARCHAR(100) NOT NULL,
      precio      DECIMAL(10,2) NOT NULL,
      descripcion TEXT
  );</div>

      <h3>Regla 2 — Relación 1:N</h3>
      <div class="card blue">
        <h4>La FK va en la tabla del lado "N"</h4>
        <p>No se crea una tabla nueva. La PK de la entidad del lado "1" se agrega como FK en la tabla del lado "N".</p>
      </div>
      <div class="diagram">
  ER:  CLIENTE (1) ──── realiza ──── (N) PEDIDO
       ─────────────────────────────────────────
  SQL: CLIENTE(<u>id_cliente</u>, nombre, email)
       PEDIDO(<u>id_pedido</u>, fecha, total, id_cliente*)
                                            └── FK → CLIENTE

  La FK id_cliente se agrega en PEDIDO (lado N), NO en CLIENTE.</div>

      <h3>Regla 3 — Relación N:M</h3>
      <div class="card purple">
        <h4>Se crea una tabla intermedia</h4>
        <p>La tabla intermedia contiene las PK de ambas entidades como FK, y juntas forman la PK compuesta. Puede incluir atributos propios de la relación.</p>
      </div>
      <div class="diagram">
  ER:  ESTUDIANTE (N) ──── cursa ──── (M) MATERIA
       ─────────────────────────────────────────────
  SQL: ESTUDIANTE(<u>id_est</u>, nombre)
       MATERIA(<u>id_mat</u>, nombre, creditos)
       INSCRIPCION(<u>id_est*</u>, <u>id_mat*</u>, calificacion, fecha)
                    └─FK─┘  └─FK─┘
                    PK compuesta = (id_est, id_mat)</div>

      <h3>Regla 4 — Relación 1:1</h3>
      <div class="card green">
        <h4>La FK va en la tabla de participación total (o en cualquiera)</h4>
        <p>Si una entidad tiene participación total en la relación, la FK va en esa tabla. Si ambas son parciales, se elige la más conveniente, o se fusionan en una sola tabla.</p>
      </div>
      <div class="diagram">
  ER:  EMPLEADO (1) ──── tiene ──── (1) ESTACIONAMIENTO
       Participación total en ESTACIONAMIENTO
       ──────────────────────────────────────────────────
  SQL: EMPLEADO(<u>id_emp</u>, nombre, puesto)
       ESTACIONAMIENTO(<u>id_est</u>, numero, zona, id_emp* UNIQUE)
                                              └── FK UNIQUE garantiza 1:1</div>

      <h3>Regla 5 — Atributo multivaluado</h3>
      <div class="card yellow">
        <h4>Se crea una tabla nueva con la PK de la entidad padre</h4>
        <p>La nueva tabla tiene: la PK de la entidad padre (FK) + el atributo multivaluado. Juntos forman la PK compuesta.</p>
      </div>
      <div class="diagram">
  ER:  EMPLEADO tiene atributo multivaluado: telefonos
       ─────────────────────────────────────────────────
  SQL: EMPLEADO(<u>id_emp</u>, nombre, puesto)
       TELEFONO_EMP(<u>id_emp*</u>, <u>numero</u>)
                     └─FK─┘
       PK compuesta = (id_emp, numero)</div>
    </section>

    <!-- ═══ NORMALIZACIÓN (INTRO) ═══ -->
    <section id="normalizacion">
      <div class="section-label"><span class="dot"></span>03 · Normalización (introducción)</div>
      <h2>Introducción a la Normalización</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>La <strong>normalización</strong> es el proceso de organizar los atributos y tablas de una base de datos relacional para minimizar la redundancia y las anomalías de actualización. Se basa en el concepto de <strong>dependencia funcional</strong>.</p>
      </div>

      <h3>Dependencia funcional</h3>
      <div class="card orange">
        <h4>Definición: X → Y</h4>
        <p>Se dice que Y depende funcionalmente de X si, para cada valor de X, existe exactamente un valor de Y. Se escribe X → Y ("X determina Y").<br><br>
        <strong>Ejemplo:</strong> id_cliente → nombre, email (el id del cliente determina su nombre y email).</p>
      </div>

      <h3>Formas Normales (resumen introductorio)</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Forma Normal</th><th>Requisito</th><th>Problema que elimina</th></tr>
          <tr><td><strong>1FN</strong></td><td>Todos los atributos son atómicos (sin listas ni grupos repetidos)</td><td>Grupos repetidos y atributos multivaluados</td></tr>
          <tr><td><strong>2FN</strong></td><td>Está en 1FN + todo atributo no clave depende de TODA la PK (no de parte de ella)</td><td>Dependencias parciales (solo aplica con PK compuesta)</td></tr>
          <tr><td><strong>3FN</strong></td><td>Está en 2FN + no hay dependencias transitivas (A → B → C, siendo B no clave)</td><td>Dependencias transitivas entre atributos no clave</td></tr>
        </table>
      </div>
      <p style="font-size:13px; color:#8b949e;">💡 La normalización completa (1FN, 2FN, 3FN, BCNF) se estudia a fondo en la Unidad III. Aquí se presenta como contexto para entender el diseño relacional correcto.</p>
    </section>

    <!-- ═══ NoSQL ═══ -->
    <section id="nosql">
      <div class="section-label"><span class="dot"></span>04 · Bases de datos No Relacionales</div>
      <h2>Bases de Datos No Relacionales (NoSQL)</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>Las bases de datos <strong>NoSQL</strong> (<em>Not Only SQL</em>) son sistemas de gestión de datos que no usan el modelo relacional como estructura principal. Priorizan <strong>escalabilidad horizontal</strong>, <strong>flexibilidad de esquema</strong> y <strong>alto rendimiento</strong> en escenarios de grandes volúmenes de datos o estructuras cambiantes.</p>
      </div>

      <h3>Tipos de bases de datos NoSQL</h3>

      <div class="card orange">
        <h4>1️⃣ Documentos (Document Store)</h4>
        <p>Almacena datos como documentos JSON/BSON. Cada documento puede tener estructura diferente. Ideal para datos semi-estructurados.<br>
        <strong>Ejemplos:</strong> MongoDB, CouchDB, Firebase Firestore.</p>
      </div>

      <div class="card blue">
        <h4>2️⃣ Clave-Valor (Key-Value Store)</h4>
        <p>Almacena pares clave → valor. Extremadamente rápido para lecturas/escrituras simples. Ideal para caché y sesiones.<br>
        <strong>Ejemplos:</strong> Redis, DynamoDB, Memcached.</p>
      </div>

      <div class="card purple">
        <h4>3️⃣ Columnar (Wide-Column Store)</h4>
        <p>Organiza datos por columnas en lugar de filas. Óptimo para analítica y consultas sobre grandes conjuntos de datos.<br>
        <strong>Ejemplos:</strong> Apache Cassandra, HBase, Google Bigtable.</p>
      </div>

      <div class="card green">
        <h4>4️⃣ Grafos (Graph Database)</h4>
        <p>Almacena datos como nodos y aristas (relaciones). Ideal para redes sociales, recomendaciones, rutas.<br>
        <strong>Ejemplos:</strong> Neo4j, Amazon Neptune, ArangoDB.</p>
      </div>

      <h3>Relacional vs. NoSQL — Comparativa</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Aspecto</th><th>Relacional (SQL)</th><th>No Relacional (NoSQL)</th></tr>
          <tr><td><strong>Esquema</strong></td><td>Fijo (rígido) — definido antes de insertar datos</td><td>Flexible (dinámico) — cada registro puede variar</td></tr>
          <tr><td><strong>Escalabilidad</strong></td><td>Vertical (más RAM/CPU al servidor)</td><td>Horizontal (más servidores)</td></tr>
          <tr><td><strong>Transacciones</strong></td><td>ACID completo (Atomicidad, Consistencia, Aislamiento, Durabilidad)</td><td>BASE (Basically Available, Eventually Consistent)</td></tr>
          <tr><td><strong>Consultas</strong></td><td>SQL estándar, JOINs potentes</td><td>API propia, limitado para JOINs complejos</td></tr>
          <tr><td><strong>Relaciones</strong></td><td>Mediante FK e integridad referencial</td><td>Embebidas en el documento o por referencia manual</td></tr>
          <tr><td><strong>Casos de uso</strong></td><td>ERP, bancos, sistemas transaccionales</td><td>BigData, redes sociales, IoT, catálogos</td></tr>
          <tr><td><strong>Ejemplos</strong></td><td>MySQL, PostgreSQL, SQL Server</td><td>MongoDB, Redis, Cassandra, Neo4j</td></tr>
        </table>
      </div>

      <h3>Ejemplo de documento MongoDB vs. tabla SQL</h3>
      <div class="code-block">
        <div class="code-header">
          <div class="code-dots"><i></i><i></i><i></i></div>
          <span>MongoDB — Documento JSON (NoSQL)</span>
        </div>
        <pre><span class="cm">// Un solo documento almacena cliente + pedidos embebidos</span>
{
  <span class="str">"_id"</span>: <span class="str">"C001"</span>,
  <span class="str">"nombre"</span>: <span class="str">"Ana García"</span>,
  <span class="str">"email"</span>: <span class="str">"ana@mail.com"</span>,
  <span class="str">"telefonos"</span>: [<span class="str">"961-123-456"</span>, <span class="str">"961-789-012"</span>],
  <span class="str">"pedidos"</span>: [
    { <span class="str">"id_pedido"</span>: <span class="str">"P001"</span>, <span class="str">"fecha"</span>: <span class="str">"2026-05-01"</span>, <span class="str">"total"</span>: <span class="num">1500.00</span> },
    { <span class="str">"id_pedido"</span>: <span class="str">"P002"</span>, <span class="str">"fecha"</span>: <span class="str">"2026-05-10"</span>, <span class="str">"total"</span>: <span class="num">820.50</span>  }
  ]
}
<span class="cm">// Ventaja: una sola lectura obtiene cliente + todos sus pedidos
// Desventaja: si Ana cambia su email, hay que actualizar TODOS los documentos que la referencien</span></pre>
      </div>

      <div class="code-block">
        <div class="code-header">
          <div class="code-dots"><i></i><i></i><i></i></div>
          <span>MySQL — Modelo relacional equivalente (SQL)</span>
        </div>
        <pre><span class="cm">-- Mismo dato distribuido en tablas normalizadas</span>
<span class="cm">-- CLIENTE</span>
<span class="kw">SELECT</span> * <span class="kw">FROM</span> CLIENTE <span class="kw">WHERE</span> id_cliente = <span class="str">'C001'</span>;
<span class="cm">-- id_cliente | nombre      | email</span>
<span class="cm">-- C001       | Ana García  | ana@mail.com</span>

<span class="cm">-- TELEFONO_CLIENTE (tabla separada por multivaluado)</span>
<span class="kw">SELECT</span> * <span class="kw">FROM</span> TELEFONO <span class="kw">WHERE</span> id_cliente = <span class="str">'C001'</span>;
<span class="cm">-- C001 | 961-123-456</span>
<span class="cm">-- C001 | 961-789-012</span>

<span class="cm">-- PEDIDO</span>
<span class="kw">SELECT</span> * <span class="kw">FROM</span> PEDIDO <span class="kw">WHERE</span> id_cliente = <span class="str">'C001'</span>;
<span class="cm">-- P001 | 2026-05-01 | 1500.00 | C001</span>
<span class="cm">-- P002 | 2026-05-10 |  820.50 | C001</span>

<span class="cm">-- Ventaja: actualizar el email de Ana = 1 sola fila en CLIENTE
-- Desventaja: recuperar cliente + pedidos requiere JOIN</span></pre>
      </div>
    </section>

    <!-- ═══ DICCIONARIO DE DATOS ═══ -->
    <section id="diccionario">
      <div class="section-label"><span class="dot"></span>05 · Diccionario de Datos</div>
      <h2>Diccionario de Datos</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>El <strong>diccionario de datos</strong> es un repositorio centralizado que describe la estructura, significado y restricciones de todos los datos que conforman una base de datos. Es la <strong>documentación técnica del esquema</strong>: define qué es cada campo, qué valores acepta, quién lo usa y cómo se relaciona con otros datos.</p>
      </div>

      <h3>¿Para qué sirve?</h3>
      <div class="card orange">
        <h4>Utilidad del diccionario de datos</h4>
        <ul style="margin:8px 0 0; padding-left:18px;">
          <li>Facilita el <strong>mantenimiento</strong>: cualquier desarrollador entiende la BD sin conocerla de antemano.</li>
          <li>Evita <strong>inconsistencias</strong>: todos usan el mismo nombre y tipo para cada campo.</li>
          <li>Sirve como <strong>contrato</strong> entre el equipo de BD y los desarrolladores de aplicaciones.</li>
          <li>Apoya las <strong>auditorías</strong> y el cumplimiento de normativas.</li>
          <li>Es la base para generar <strong>scripts DDL</strong> automatizados.</li>
        </ul>
      </div>

      <h3>Estructura típica de un diccionario de datos</h3>
      <p>Por cada tabla se documenta una ficha con los siguientes campos:</p>

      <div class="table-wrap">
        <table>
          <tr><th>Campo del diccionario</th><th>Descripción</th><th>Ejemplo</th></tr>
          <tr><td><strong>Nombre de la tabla</strong></td><td>Nombre exacto en la BD</td><td>CLIENTE</td></tr>
          <tr><td><strong>Descripción</strong></td><td>Qué representa esta tabla</td><td>Almacena los datos de los clientes registrados</td></tr>
          <tr><td><strong>Nombre del atributo</strong></td><td>Nombre exacto de la columna</td><td>id_cliente</td></tr>
          <tr><td><strong>Tipo de dato</strong></td><td>Tipo y longitud del campo</td><td>INT, VARCHAR(100), DATE…</td></tr>
          <tr><td><strong>Restricciones</strong></td><td>PK, FK, NOT NULL, UNIQUE, CHECK</td><td>PRIMARY KEY, NOT NULL</td></tr>
          <tr><td><strong>Valor por defecto</strong></td><td>Valor si no se especifica al insertar</td><td>NOW(), 0, NULL</td></tr>
          <tr><td><strong>Descripción del campo</strong></td><td>Qué representa este campo en el negocio</td><td>Identificador único autoincremental del cliente</td></tr>
          <tr><td><strong>Referencia (FK)</strong></td><td>A qué tabla/columna apunta si es FK</td><td>→ CIUDAD(id_ciudad)</td></tr>
        </table>
      </div>

      <h3>Ejemplo de ficha de diccionario — Tabla CLIENTE</h3>
      <p><strong>Tabla:</strong> CLIENTE &nbsp;|&nbsp; <strong>Descripción:</strong> Almacena los datos de los clientes registrados en el sistema.</p>
      <div class="table-wrap">
        <table class="dd-table">
          <tr>
            <th>Atributo</th><th>Tipo</th><th>Restricciones</th><th>Default</th><th>Descripción</th>
          </tr>
          <tr>
            <td>id_cliente</td>
            <td>INT</td>
            <td>PK, NOT NULL, AUTO_INCREMENT</td>
            <td>AUTO</td>
            <td>Identificador único autoincremental del cliente</td>
          </tr>
          <tr>
            <td>nombre</td>
            <td>VARCHAR(100)</td>
            <td>NOT NULL</td>
            <td>—</td>
            <td>Nombre(s) del cliente</td>
          </tr>
          <tr>
            <td>apellidos</td>
            <td>VARCHAR(150)</td>
            <td>NOT NULL</td>
            <td>—</td>
            <td>Apellido paterno y materno del cliente</td>
          </tr>
          <tr>
            <td>email</td>
            <td>VARCHAR(200)</td>
            <td>UNIQUE, NOT NULL</td>
            <td>—</td>
            <td>Correo electrónico. Usado para login. Debe ser único.</td>
          </tr>
          <tr>
            <td>telefono</td>
            <td>VARCHAR(15)</td>
            <td>NULL permitido</td>
            <td>NULL</td>
            <td>Número telefónico de contacto (opcional)</td>
          </tr>
          <tr>
            <td>fecha_registro</td>
            <td>DATETIME</td>
            <td>NOT NULL</td>
            <td>NOW()</td>
            <td>Fecha y hora en que el cliente se registró en el sistema</td>
          </tr>
          <tr>
            <td>activo</td>
            <td>TINYINT(1)</td>
            <td>NOT NULL, CHECK (0 o 1)</td>
            <td>1</td>
            <td>Estado del cliente: 1=activo, 0=dado de baja</td>
          </tr>
        </table>
      </div>
    </section>

    <!-- ═══ EJEMPLOS ═══ -->
    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>06 · Ejemplos</div>
      <h2>Ejemplos Demostrativos</h2>

      <!-- Ejemplo 1 -->
      <div class="ejercicio" data-num="Ejemplo 1">
        <h4>Transformación completa: ER → Esquema relacional → DDL</h4>
        <p>Partimos del diagrama ER de una tienda y aplicamos las 5 reglas de transformación:</p>
        <div class="diagram">
  DIAGRAMA ER:
  ─────────────────────────────────────────────────────
  CATEGORIA(id_cat, nombre)
      │ 1
      │
  PRODUCTO(id_prod, nombre, precio, stock)  ← multivaluado: imagenes
      │ N
      │
  DETALLE_VENTA ─────── N:M entre PRODUCTO y VENTA
      │ N
      │
  VENTA(id_venta, fecha, total)
      │ N
      │
  CLIENTE(id_cliente, nombre, email)

  ESQUEMA RELACIONAL RESULTANTE:
  ─────────────────────────────────────────────────────
  CATEGORIA(id_cat, nombre)
  PRODUCTO(id_prod, nombre, precio, stock, id_cat*)
  IMAGEN_PRODUCTO(id_prod*, url_imagen)   ← atrib. multivaluado
  CLIENTE(id_cliente, nombre, email)
  VENTA(id_venta, fecha, total, id_cliente*)
  DETALLE_VENTA(id_prod*, id_venta*, cantidad, precio_unitario)</div>

        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><i></i><i></i><i></i></div>
            <span>SQL — DDL completo de la tienda</span>
          </div>
          <pre><span class="kw">CREATE TABLE</span> CATEGORIA (
    id_cat  <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre  <span class="kw">VARCHAR</span>(<span class="num">80</span>) <span class="kw">NOT NULL UNIQUE</span>
);

<span class="kw">CREATE TABLE</span> PRODUCTO (
    id_prod  <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre   <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    precio   <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL CHECK</span>(precio > <span class="num">0</span>),
    stock    <span class="kw">INT NOT NULL DEFAULT</span> <span class="num">0</span>,
    id_cat   <span class="kw">INT NOT NULL</span>,
    <span class="kw">FOREIGN KEY</span> (id_cat) <span class="kw">REFERENCES</span> CATEGORIA(id_cat)
);

<span class="cm">-- Atributo multivaluado: una tabla por imagen</span>
<span class="kw">CREATE TABLE</span> IMAGEN_PRODUCTO (
    id_prod    <span class="kw">INT NOT NULL</span>,
    url_imagen <span class="kw">VARCHAR</span>(<span class="num">300</span>) <span class="kw">NOT NULL</span>,
    <span class="kw">PRIMARY KEY</span> (id_prod, url_imagen),
    <span class="kw">FOREIGN KEY</span> (id_prod) <span class="kw">REFERENCES</span> PRODUCTO(id_prod)
);

<span class="kw">CREATE TABLE</span> CLIENTE (
    id_cliente <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre     <span class="kw">VARCHAR</span>(<span class="num">150</span>) <span class="kw">NOT NULL</span>,
    email      <span class="kw">VARCHAR</span>(<span class="num">200</span>) <span class="kw">NOT NULL UNIQUE</span>
);

<span class="kw">CREATE TABLE</span> VENTA (
    id_venta   <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    fecha      <span class="kw">DATETIME NOT NULL DEFAULT</span> <span class="fn">NOW</span>(),
    total      <span class="kw">DECIMAL</span>(<span class="num">12</span>,<span class="num">2</span>) <span class="kw">NOT NULL</span>,
    id_cliente <span class="kw">INT NOT NULL</span>,
    <span class="kw">FOREIGN KEY</span> (id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente)
);

<span class="cm">-- Tabla intermedia N:M con atributos propios</span>
<span class="kw">CREATE TABLE</span> DETALLE_VENTA (
    id_prod        <span class="kw">INT NOT NULL</span>,
    id_venta       <span class="kw">INT NOT NULL</span>,
    cantidad       <span class="kw">INT NOT NULL CHECK</span>(cantidad > <span class="num">0</span>),
    precio_unitario <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL</span>,
    <span class="kw">PRIMARY KEY</span> (id_prod, id_venta),
    <span class="kw">FOREIGN KEY</span> (id_prod)  <span class="kw">REFERENCES</span> PRODUCTO(id_prod),
    <span class="kw">FOREIGN KEY</span> (id_venta) <span class="kw">REFERENCES</span> VENTA(id_venta)
);</pre>
        </div>
      </div>

      <!-- Ejemplo 2 -->
      <div class="ejercicio" data-num="Ejemplo 2">
        <h4>Diccionario de datos — Tabla VENTA</h4>
        <p><strong>Tabla:</strong> VENTA &nbsp;|&nbsp; <strong>Descripción:</strong> Registra cada transacción de venta realizada en el sistema.</p>
        <div class="table-wrap">
          <table class="dd-table">
            <tr><th>Atributo</th><th>Tipo</th><th>Restricciones</th><th>Default</th><th>Descripción</th></tr>
            <tr><td>id_venta</td><td>INT</td><td>PK, NOT NULL, AUTO_INCREMENT</td><td>AUTO</td><td>Identificador único de la venta</td></tr>
            <tr><td>fecha</td><td>DATETIME</td><td>NOT NULL</td><td>NOW()</td><td>Fecha y hora exacta en que se realizó la venta</td></tr>
            <tr><td>total</td><td>DECIMAL(12,2)</td><td>NOT NULL, CHECK > 0</td><td>—</td><td>Monto total de la venta en pesos MXN</td></tr>
            <tr><td>id_cliente</td><td>INT</td><td>FK → CLIENTE(id_cliente), NOT NULL</td><td>—</td><td>Cliente que realizó la compra</td></tr>
          </table>
        </div>
      </div>
    </section>

    <!-- ═══ PRÁCTICA ═══ -->
    <section id="practica">
      <div class="section-label"><span class="dot"></span>07 · Práctica</div>
      <h2>Ejercicios de Práctica</h2>

      <div class="ejercicio practica" data-num="Práctica 1">
        <h4>Transformación ER → Modelo Relacional</h4>
        <p>Toma el diagrama ER de la Farmacia San Agustín que diseñaste en la Semana 2 (Práctica 2) y aplica las reglas de transformación:</p>
        <ol style="margin-top:10px; padding-left:20px;">
          <li>Escribe el <strong>esquema relacional</strong> completo en notación textual (TABLA(pk, atrib, fk*)).</li>
          <li>Identifica qué regla de transformación aplicaste en cada tabla (R1–R5).</li>
          <li>Escribe el <strong>DDL completo</strong> en MySQL con tipos de datos apropiados, PK, FK y al menos un CHECK por tabla.</li>
          <li>Verifica que el orden de creación de tablas respete las dependencias de FK (primero las tablas referenciadas).</li>
        </ol>
      </div>

      <div class="ejercicio practica" data-num="Práctica 2">
        <h4>SQL vs. MongoDB — Comparativa práctica</h4>
        <div class="card yellow" style="margin:12px 0;">
          <h4>📋 Caso: Sistema de blog</h4>
          <p>Un blog tiene autores que publican artículos. Cada artículo puede tener múltiples etiquetas y múltiples comentarios. Los comentarios tienen autor (puede ser anónimo) y fecha.</p>
        </div>
        <ol style="margin-top:10px; padding-left:20px;">
          <li>Diseña el <strong>modelo relacional</strong> (esquema + DDL) para el sistema de blog.</li>
          <li>Diseña el <strong>documento MongoDB</strong> equivalente que embeba artículos con sus comentarios y etiquetas.</li>
          <li>Analiza: ¿en qué escenario usarías SQL y en cuál MongoDB para este blog? Justifica con al menos 3 argumentos.</li>
        </ol>
      </div>

      <div class="ejercicio practica" data-num="Práctica 3">
        <h4>Elaborar un diccionario de datos completo</h4>
        <p>Para el siguiente esquema relacional de un sistema hospitalario, elabora el diccionario de datos completo (una ficha por tabla):</p>
        <div class="diagram">
  MEDICO(id_medico, nombre, especialidad, cedula_prof)
  PACIENTE(id_paciente, nombre, fecha_nac, nss, id_medico*)
  CONSULTA(id_consulta, fecha, diagnostico, costo, id_paciente*, id_medico*)
  MEDICAMENTO(id_med, nombre, laboratorio, presentacion)
  RECETA(id_consulta*, id_med*, dosis, duracion_dias)</div>
        <p style="margin-top:10px;">Para cada tabla documenta: nombre de tabla, descripción, y para cada atributo: nombre, tipo de dato, restricciones, valor por defecto y descripción del campo en el contexto hospitalario.</p>
      </div>
    </section>

    <!-- ═══ RESUMEN ═══ -->
    <section id="resumen">
      <div class="section-label"><span class="dot"></span>08 · Resumen</div>
      <h2>Puntos Clave de la Semana 3</h2>

      <div class="summary-grid">
        <div class="summary-item">
          <div class="icon">📐</div>
          <div class="val">Modelo Relacional</div>
          <div class="label">Esquema lógico implementable. 5 restricciones clave.</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔄</div>
          <div class="val">5 Reglas ER→SQL</div>
          <div class="label">Entidad fuerte · 1:N · N:M · 1:1 · Multivaluado</div>
        </div>
        <div class="summary-item">
          <div class="icon">🍃</div>
          <div class="val">NoSQL</div>
          <div class="label">Docs · Clave-valor · Columnar · Grafos</div>
        </div>
        <div class="summary-item">
          <div class="icon">⚖️</div>
          <div class="val">SQL vs NoSQL</div>
          <div class="label">ACID vs BASE · Esquema fijo vs flexible</div>
        </div>
        <div class="summary-item">
          <div class="icon">📖</div>
          <div class="val">Diccionario</div>
          <div class="label">Documentación del esquema: nombre, tipo, restricciones, descripción</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔗</div>
          <div class="val">Dep. funcional</div>
          <div class="label">X → Y. Base de la normalización (U3)</div>
        </div>
      </div>

      <div class="card green" style="margin-top:24px;">
        <h4>📚 Para la próxima semana (Semana 4 — 25–29 Mayo)</h4>
        <p>Continuamos con la Unidad II: <strong>Semana 4 cierra la Unidad II</strong> — repaso y ejercicios integradores de modelado completo (ER → Relacional → Diccionario). Trae las prácticas de esta semana resueltas para revisión en clase.</p>
      </div>
    </section>

  </main>
</div>
</div>
</body>
</html>
