<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BDD · Semana 7 — Poblar la base de datos</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@600;700;800&family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #21262d;
    --border: #30363d;
    --text: #e6edf3;
    --muted: #8b949e;
    --accent: #f472b6;
    --accent2: #f9a8d4;
    --accent-soft: rgba(244,114,182,0.12);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; line-height: 1.65; font-size: 15px; }
  header { background: radial-gradient(1200px 400px at 80% -20%, rgba(244,114,182,0.18), transparent 60%), linear-gradient(180deg, #11151c 0%, var(--bg) 100%); border-bottom: 1px solid var(--border); padding: 48px 32px 40px; }
  .badge { display: inline-block; font-family: 'JetBrains Mono', monospace; font-size: 12px; letter-spacing: 0.5px; color: var(--accent2); border: 1px solid var(--border); background: var(--accent-soft); padding: 5px 12px; border-radius: 999px; margin-bottom: 18px; }
  header h1 { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 38px; line-height: 1.1; letter-spacing: -0.5px; max-width: 900px; }
  header h1 span { color: var(--accent); }
  header > p { color: var(--muted); margin-top: 12px; font-size: 16px; max-width: 790px; }
  .pills { margin-top: 22px; display: flex; gap: 10px; flex-wrap: wrap; }
  .pill { font-size: 13px; color: var(--text); background: var(--surface); border: 1px solid var(--border); padding: 7px 13px; border-radius: 8px; }

  .container { max-width: 1180px; margin: 0 auto; padding: 0 32px; }
  .layout { display: grid; grid-template-columns: 240px 1fr; gap: 40px; padding: 40px 0 80px; }
  nav.toc { position: sticky; top: 24px; align-self: start; background: var(--surface); border: 1px solid var(--border); border-radius: 12px; padding: 18px 16px; }
  nav.toc h4 { font-family: 'JetBrains Mono', monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--muted); margin-bottom: 12px; }
  nav.toc a { display: block; color: var(--text); text-decoration: none; font-size: 13.5px; padding: 7px 10px; border-radius: 7px; transition: background .15s, color .15s; }
  nav.toc a:hover { background: var(--accent-soft); color: var(--accent2); }

  main { min-width: 0; }
  section { margin-bottom: 46px; scroll-margin-top: 24px; }
  .section-label { display: flex; align-items: center; gap: 8px; font-family: 'JetBrains Mono', monospace; font-size: 12px; color: var(--accent2); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 10px; }
  .section-label .dot { width: 7px; height: 7px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 10px var(--accent); }
  h2 { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 26px; letter-spacing: -0.3px; margin-bottom: 14px; }
  h3 { font-family: 'Syne', sans-serif; font-weight: 600; font-size: 18px; margin: 26px 0 10px; color: var(--text); }
  p { margin-bottom: 12px; color: #cdd5de; }
  strong { color: var(--text); }
  code.inline { font-family: 'JetBrains Mono', monospace; font-size: 13px; background: var(--surface2); color: var(--accent2); padding: 2px 6px; border-radius: 5px; }

  .definition, .note, .warn { border-radius: 10px; padding: 16px 18px; margin: 18px 0; border: 1px solid var(--border); }
  .definition { background: var(--accent-soft); border-color: rgba(244,114,182,0.35); }
  .definition .label { font-family: 'JetBrains Mono', monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--accent2); margin-bottom: 6px; }
  .note { background: var(--surface); }
  .note .label, .warn .label { font-weight: 700; font-size: 13px; margin-bottom: 4px; display: block; }
  .note .label { color: var(--accent2); }
  .warn { background: rgba(210,153,34,0.10); border-color: rgba(210,153,34,0.40); }
  .warn .label { color: #e3b341; }
  .rule { background: linear-gradient(135deg, rgba(244,114,182,0.16), rgba(244,114,182,0.04)); border: 1px solid rgba(244,114,182,0.45); border-radius: 10px; padding: 16px 18px; margin: 18px 0; }
  .rule .label { font-family: 'JetBrains Mono', monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--accent2); display: block; margin-bottom: 6px; }
  .rule p { margin: 0; color: var(--text); font-size: 15.5px; }

  pre { background: #161b22 !important; color: #e6edf3 !important; border: 1px solid var(--border); border-radius: 10px; padding: 16px 18px; font-family: 'JetBrains Mono', monospace; font-size: 12.5px; overflow-x: auto; margin: 16px 0; white-space: pre; line-height: 1.55; }
  pre .kw { color: #ff7b72; font-weight: 500; }
  pre .ty { color: #79c0ff; }
  pre .st { color: #a5d6ff; }
  pre .cm { color: #8b949e; font-style: italic; }
  pre .er { color: #ffa198; font-weight: 600; }
  pre .ok { color: #56d364; }

  table { width: 100%; border-collapse: collapse; margin: 16px 0; font-size: 13.5px; border: 1px solid var(--border); border-radius: 10px; overflow: hidden; }
  th { background: #21262d !important; color: #e6edf3 !important; text-align: left; padding: 10px 13px; font-weight: 700; border-bottom: 1px solid var(--border); }
  td { background: #0d1117 !important; padding: 9px 13px; border-bottom: 1px solid var(--border); color: #cdd5de; vertical-align: top; }
  tr:nth-child(even) td { background: #161b22 !important; }
  td code, th code { font-family: 'JetBrains Mono', monospace; color: var(--accent2); font-size: 12.5px; }
  .tbl-cap { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: var(--muted); margin: 18px 0 -8px; text-transform: uppercase; letter-spacing: 1px; }

  .practice { background: var(--surface); border: 1px solid var(--border); border-left: 3px solid var(--accent); border-radius: 10px; padding: 18px 20px; margin: 18px 0; }
  .practice .tag { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: var(--accent2); text-transform: uppercase; letter-spacing: 1px; }
  .practice h3 { margin-top: 6px; }
  ul, ol { margin: 8px 0 12px 22px; color: #cdd5de; }
  li { margin-bottom: 5px; }

  .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 14px; margin-top: 18px; }
  .summary-item { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 16px; text-align: center; }
  .summary-item .icon { font-size: 22px; margin-bottom: 6px; }
  .summary-item .label { font-size: 11px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; }
  .summary-item .val { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 600; color: var(--text); margin-top: 4px; }

  footer { border-top: 1px solid var(--border); padding: 24px 32px; text-align: center; color: var(--muted); font-size: 12.5px; }
  @media (max-width: 820px) { .layout { grid-template-columns: 1fr; } nav.toc { position: static; } header h1 { font-size: 30px; } }
</style>
</head>
<body>

<header>
  <div class="badge">ITIID · Bases de Datos · UPTap</div>
  <h1>Semana 7 — <span>Poblar la base de datos</span></h1>
  <p>Unidad III · Construcción — Ya creamos las tablas (Semana 5) y las diseñamos sin redundancia (Semana 6). Ahora les damos vida con datos usando INSERT, UPDATE y DELETE, cuidando que cada operación respete la integridad del diseño.</p>
  <div class="pills">
    <span class="pill">📅 15 – 19 Junio 2026</span>
    <span class="pill">⏱ 5 horas</span>
    <span class="pill">🎯 Cargar y modificar datos respetando restricciones</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#puente">🔗 ¿Dónde estamos?</a>
    <a href="#dml">📚 DML vs DDL</a>
    <a href="#insert">➕ INSERT</a>
    <a href="#orden">🧩 El orden importa</a>
    <a href="#update">✏️ UPDATE</a>
    <a href="#delete">🗑️ DELETE</a>
    <a href="#errores">🔧 Errores de integridad</a>
    <a href="#tx">🔒 Transacciones</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <section id="puente">
      <div class="section-label"><span class="dot"></span>00 · Contexto</div>
      <h2>¿Dónde estamos?</h2>
      <p>Una base de datos vacía no le sirve a nadie. Tras construir el esquema y normalizarlo, el siguiente paso es <strong>poblarlo</strong>: insertar registros, corregirlos cuando cambian y eliminarlos cuando ya no aplican. Para eso usamos tres instrucciones del lenguaje de manipulación de datos (DML): <code class="inline">INSERT</code>, <code class="inline">UPDATE</code> y <code class="inline">DELETE</code>.</p>
      <p>La diferencia con lo anterior es importante: ahora la base ya tiene <strong>llaves foráneas y restricciones</strong> vigilando. Cargar datos en una base normalizada no es escribir filas al azar; hay que respetar el orden y las reglas, o el SGBD rechazará la operación.</p>
      <div class="definition">
        <div class="label">Meta de la semana</div>
        <p style="margin:0;">Cargar y mantener datos en un esquema normalizado <strong>sin violar la integridad</strong>: respetar el orden de inserción, las llaves únicas, los valores obligatorios y las relaciones entre tablas.</p>
      </div>
    </section>

    <section id="dml">
      <div class="section-label"><span class="dot"></span>01 · Recordatorio</div>
      <h2>DML vs DDL</h2>
      <p>En la Semana 5 vimos que SQL tiene sublenguajes. El <strong>DDL</strong> define la estructura; el <strong>DML</strong> trabaja con los datos que viven dentro de esa estructura.</p>
      <table>
        <tr><th>Sublenguaje</th><th>Comandos</th><th>Actúa sobre…</th></tr>
        <tr><td>DDL</td><td><code>CREATE</code>, <code>ALTER</code>, <code>DROP</code></td><td>La estructura (tablas, columnas, restricciones).</td></tr>
        <tr><td><strong>DML</strong></td><td><code>INSERT</code>, <code>UPDATE</code>, <code>DELETE</code>, <code>SELECT</code></td><td>Los datos (las filas).</td></tr>
      </table>
      <div class="note">
        <span class="label">Nota</span>
        <code class="inline">SELECT</code> también es DML, pero como es todo un mundo (consultas, filtros, JOINs) lo dedicaremos a la Unidad IV. Esta semana nos enfocamos en las tres operaciones que <em>modifican</em> los datos.
      </div>
    </section>

    <section id="insert">
      <div class="section-label"><span class="dot"></span>02 · Teoría</div>
      <h2>INSERT — agregar filas</h2>
      <h3>Forma básica</h3>
      <pre><span class="kw">INSERT INTO</span> cliente (id_cliente, nombre_cliente)
<span class="kw">VALUES</span> (<span class="st">'C01'</span>, <span class="st">'Ana López'</span>);</pre>
      <p>Buena práctica: <strong>nombrar siempre las columnas</strong>. Así tu INSERT sigue funcionando aunque después se agregue o reordene una columna.</p>
      <h3>Insertar varias filas a la vez</h3>
      <pre><span class="kw">INSERT INTO</span> producto (id_producto, nombre_producto, precio)
<span class="kw">VALUES</span>
    (<span class="st">'P10'</span>, <span class="st">'Teclado mecánico'</span>, 350.00),
    (<span class="st">'P11'</span>, <span class="st">'Mouse inalámbrico'</span>, 180.00),
    (<span class="st">'P12'</span>, <span class="st">'Monitor 24 pulgadas'</span>, 2500.00);</pre>
      <h3>Columnas con AUTO_INCREMENT o DEFAULT</h3>
      <p>Si una columna es <code class="inline">AUTO_INCREMENT</code> o tiene <code class="inline">DEFAULT</code>, simplemente no la incluyas y el SGBD pondrá el valor por ti:</p>
      <pre><span class="cm">-- id_venta es AUTO_INCREMENT; fecha tiene DEFAULT (CURDATE())</span>
<span class="kw">INSERT INTO</span> venta (id_cliente) <span class="kw">VALUES</span> (<span class="st">'C01'</span>);</pre>
      <div class="note">
        <span class="label">Tip</span>
        Para copiar datos de otra tabla puedes usar <code class="inline">INSERT INTO ... SELECT ...</code>, que inserta el resultado de una consulta. Lo retomaremos cuando veamos SELECT.
      </div>
    </section>

    <section id="orden">
      <div class="section-label"><span class="dot"></span>03 · Integridad</div>
      <h2>El orden importa: primero los padres</h2>
      <p>Cuando una tabla tiene una llave foránea, no puedes insertar un "hijo" si su "padre" todavía no existe. El SGBD lo rechazará para proteger la integridad referencial.</p>
      <pre><span class="cm">-- ERROR: el cliente C09 no existe todavía</span>
<span class="kw">INSERT INTO</span> venta (id_cliente) <span class="kw">VALUES</span> (<span class="st">'C09'</span>);
<span class="er">ERROR 1452: Cannot add or update a child row:
a foreign key constraint fails</span></pre>
      <p>La regla es simple: <strong>inserta primero las tablas referenciadas</strong> (las del lado "uno") y después las que las referencian.</p>
      <div class="rule">
        <span class="label">Orden de carga para el esquema de ventas</span>
        <p><code class="inline">cliente</code> y <code class="inline">producto</code> &nbsp;→&nbsp; <code class="inline">venta</code> &nbsp;→&nbsp; <code class="inline">detalle_venta</code></p>
      </div>
    </section>

    <section id="update">
      <div class="section-label"><span class="dot"></span>04 · Teoría</div>
      <h2>UPDATE — modificar filas</h2>
      <pre><span class="cm">-- Subir 10% el precio de un producto</span>
<span class="kw">UPDATE</span> producto
<span class="kw">SET</span> precio = precio * 1.10
<span class="kw">WHERE</span> id_producto = <span class="st">'P10'</span>;</pre>
      <p>Puedes actualizar varias columnas a la vez separándolas por comas:</p>
      <pre><span class="kw">UPDATE</span> cliente
<span class="kw">SET</span> nombre_cliente = <span class="st">'Ana López Ruiz'</span>,
    correo = <span class="st">'ana.lr@correo.com'</span>
<span class="kw">WHERE</span> id_cliente = <span class="st">'C01'</span>;</pre>
      <div class="warn">
        <span class="label">⚠ Regla de oro</span>
        Un <code class="inline">UPDATE</code> <strong>sin</strong> <code class="inline">WHERE</code> modifica <strong>todas</strong> las filas de la tabla. Antes de ejecutar, pregúntate: "¿qué filas quiero tocar?" y confírmalo en el <code class="inline">WHERE</code>.
      </div>
    </section>

    <section id="delete">
      <div class="section-label"><span class="dot"></span>05 · Teoría</div>
      <h2>DELETE — eliminar filas</h2>
      <pre><span class="cm">-- Borrar una venta específica</span>
<span class="kw">DELETE FROM</span> venta
<span class="kw">WHERE</span> folio = 1003;</pre>
      <p>Igual que con UPDATE, <strong>siempre con <code class="inline">WHERE</code></strong>: un <code class="inline">DELETE FROM tabla;</code> sin condición vacía la tabla entera.</p>
      <h3>¿Y las filas relacionadas?</h3>
      <p>Aquí entran en juego las acciones referenciales que definiste en la llave foránea:</p>
      <table>
        <tr><th>Si la FK tiene…</th><th>Al borrar el padre…</th></tr>
        <tr><td><code>ON DELETE RESTRICT</code></td><td>El SGBD impide el borrado mientras existan hijos. Primero borra los hijos.</td></tr>
        <tr><td><code>ON DELETE CASCADE</code></td><td>Se borran automáticamente también los hijos (p. ej. borrar una venta borra su detalle).</td></tr>
      </table>
      <div class="note">
        <span class="label">DELETE vs TRUNCATE</span>
        <code class="inline">DELETE</code> borra filas según el <code class="inline">WHERE</code> y se puede revertir dentro de una transacción; <code class="inline">TRUNCATE</code> vacía toda la tabla de golpe y reinicia el contador AUTO_INCREMENT.
      </div>
    </section>

    <section id="errores">
      <div class="section-label"><span class="dot"></span>06 · Diagnóstico</div>
      <h2>Errores de integridad al cargar</h2>
      <p>Cuando un INSERT o UPDATE viola una restricción, el SGBD lo rechaza con un mensaje. Aprender a leerlos es media batalla ganada:</p>
      <table>
        <tr><th>Mensaje (resumido)</th><th>Qué significa</th><th>Cómo resolver</th></tr>
        <tr><td><code>a foreign key constraint fails</code></td><td>El valor de la FK no existe en la tabla padre.</td><td>Inserta primero el padre, o corrige el id.</td></tr>
        <tr><td><code>Duplicate entry ... for key</code></td><td>Repetiste un valor de PK o de una columna UNIQUE.</td><td>Usa un valor distinto; revisa si ya existe.</td></tr>
        <tr><td><code>Column ... cannot be null</code></td><td>Dejaste vacía una columna NOT NULL.</td><td>Proporciona un valor para esa columna.</td></tr>
        <tr><td><code>Data too long for column</code></td><td>El texto excede el tamaño del VARCHAR.</td><td>Acorta el dato o amplía la columna.</td></tr>
        <tr><td><code>Incorrect ... value</code></td><td>El dato no corresponde al tipo (p. ej. texto en un INT).</td><td>Corrige el valor al tipo esperado.</td></tr>
      </table>
      <div class="note">
        <span class="label">Mentalidad</span>
        Estos errores no son un fastidio: son tu diseño <em>defendiéndose</em>. Cada rechazo evita que entren datos corruptos a la base.
      </div>
    </section>

    <section id="tx">
      <div class="section-label"><span class="dot"></span>07 · Cargas seguras</div>
      <h2>Transacciones: todo o nada</h2>
      <p>Cuando insertas varios registros relacionados (una venta y su detalle, por ejemplo), quieres que se guarden <strong>todos o ninguno</strong>. Una transacción agrupa varias operaciones en una sola unidad:</p>
      <pre><span class="kw">START TRANSACTION</span>;

<span class="kw">INSERT INTO</span> venta (id_cliente) <span class="kw">VALUES</span> (<span class="st">'C01'</span>);
<span class="kw">INSERT INTO</span> detalle_venta (folio, id_producto, cantidad)
<span class="kw">VALUES</span> (<span class="kw">LAST_INSERT_ID</span>(), <span class="st">'P10'</span>, 2);

<span class="kw">COMMIT</span>;   <span class="cm">-- confirma los cambios</span>
<span class="cm">-- ROLLBACK; deshace todo si algo salió mal</span></pre>
      <p>Si entre los dos INSERT ocurriera un error, un <code class="inline">ROLLBACK</code> deja la base como estaba, sin ventas a medias.</p>
    </section>

    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>08 · Ejemplos demostrativos</div>
      <h2>Ejemplos resueltos</h2>
      <p>Usamos el esquema normalizado de la Semana 6: <code class="inline">cliente</code>, <code class="inline">producto</code>, <code class="inline">venta</code> y <code class="inline">detalle_venta</code>.</p>

      <h3>Ejemplo 1 — Poblar en el orden correcto</h3>
      <pre><span class="cm">-- 1) Primero los padres</span>
<span class="kw">INSERT INTO</span> cliente <span class="kw">VALUES</span> (<span class="st">'C01'</span>, <span class="st">'Ana López'</span>), (<span class="st">'C02'</span>, <span class="st">'Beto Ramírez'</span>);
<span class="kw">INSERT INTO</span> producto <span class="kw">VALUES</span>
    (<span class="st">'P10'</span>, <span class="st">'Teclado mecánico'</span>, 350.00),
    (<span class="st">'P11'</span>, <span class="st">'Mouse inalámbrico'</span>, 180.00);

<span class="cm">-- 2) Luego la venta (referencia a cliente)</span>
<span class="kw">INSERT INTO</span> venta (folio, fecha, id_cliente)
<span class="kw">VALUES</span> (1001, <span class="st">'2026-06-15'</span>, <span class="st">'C01'</span>);

<span class="cm">-- 3) Por último el detalle (referencia a venta y producto)</span>
<span class="kw">INSERT INTO</span> detalle_venta <span class="kw">VALUES</span>
    (1001, <span class="st">'P10'</span>, 2),
    (1001, <span class="st">'P11'</span>, 1);</pre>

      <h3>Ejemplo 2 — Actualizar y borrar</h3>
      <pre><span class="cm">-- El teclado sube de precio</span>
<span class="kw">UPDATE</span> producto <span class="kw">SET</span> precio = 399.00 <span class="kw">WHERE</span> id_producto = <span class="st">'P10'</span>;

<span class="cm">-- Se cancela la venta 1001: con ON DELETE CASCADE,</span>
<span class="cm">-- su detalle se borra automáticamente</span>
<span class="kw">DELETE FROM</span> venta <span class="kw">WHERE</span> folio = 1001;</pre>

      <h3>Ejemplo 3 — Leer un error y corregirlo</h3>
      <pre><span class="cm">-- Intentamos vender un producto que no existe</span>
<span class="kw">INSERT INTO</span> detalle_venta <span class="kw">VALUES</span> (1001, <span class="st">'P99'</span>, 1);
<span class="er">ERROR 1452: a foreign key constraint fails</span>

<span class="cm">-- Solución: primero damos de alta el producto P99</span>
<span class="kw">INSERT INTO</span> producto <span class="kw">VALUES</span> (<span class="st">'P99'</span>, <span class="st">'Audífonos'</span>, 450.00);
<span class="kw">INSERT INTO</span> detalle_venta <span class="kw">VALUES</span> (1001, <span class="st">'P99'</span>, 1);  <span class="ok">-- OK</span></pre>
    </section>

    <section id="practica">
      <div class="section-label"><span class="dot"></span>09 · Ejercicios de práctica</div>
      <h2>Práctica</h2>

      <div class="practice">
        <span class="tag">Ejercicio 1 · Carga ordenada</span>
        <h3>Puebla el esquema de streaming</h3>
        <p>Sobre la base de la Práctica 4 (<code class="inline">artista</code>, <code class="inline">usuario</code>, <code class="inline">cancion</code>, <code class="inline">playlist</code>, <code class="inline">cancion_playlist</code>), escribe los INSERT necesarios para cargar 3 artistas, 4 usuarios, 6 canciones, 3 playlists y al menos 8 filas en la tabla intermedia. Respeta el orden de inserción y entrégalo como un solo script.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 2 · Mantenimiento</span>
        <h3>Modifica y elimina con criterio</h3>
        <p>Escribe sentencias para: (a) subir 5% las reproducciones de todas las canciones de un artista; (b) cambiar el plan de un usuario a "premium"; (c) eliminar una playlist y comprobar qué ocurre con sus filas en la tabla intermedia. Explica el resultado de (c) según la acción referencial.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 3 · Diagnóstico</span>
        <h3>Caza el error de integridad</h3>
        <p>Se te dará un script con cuatro INSERT que fallan. Identifica qué restricción viola cada uno (FK, PK/UNIQUE, NOT NULL o tipo de dato), explica el mensaje de error y corrige la sentencia para que se ejecute.</p>
      </div>
    </section>

    <section id="resumen">
      <div class="section-label"><span class="dot"></span>10 · Cierre</div>
      <h2>Resumen de la semana</h2>
      <div class="summary-grid">
        <div class="summary-item"><div class="icon">➕</div><div class="label">INSERT</div><div class="val">Agregar filas</div></div>
        <div class="summary-item"><div class="icon">✏️</div><div class="label">UPDATE</div><div class="val">Modificar (con WHERE)</div></div>
        <div class="summary-item"><div class="icon">🗑️</div><div class="label">DELETE</div><div class="val">Eliminar (con WHERE)</div></div>
        <div class="summary-item"><div class="icon">🧩</div><div class="label">Orden</div><div class="val">Padres antes que hijos</div></div>
        <div class="summary-item"><div class="icon">🔒</div><div class="label">Seguridad</div><div class="val">Transacciones</div></div>
        <div class="summary-item"><div class="icon">➡️</div><div class="label">Próxima (U4)</div><div class="val">SELECT: consultar datos</div></div>
      </div>
      <div class="note" style="margin-top:22px;">
        <span class="label">Lo esencial</span>
        Poblar una base normalizada es un acto de disciplina: inserta los padres antes que los hijos, pon siempre un <code class="inline">WHERE</code> en tus UPDATE y DELETE, y trata cada error de integridad como una alarma útil. Con la base ya construida, diseñada y poblada, en la Unidad IV aprenderemos a <strong>preguntarle</strong> cosas con SELECT.
      </div>
    </section>

  </main>
</div>
</div>

<footer>
  Bases de Datos · ITIID · Universidad Politécnica de Tapachula · Periodo Mayo–Agosto 2026 · Grupo 3°A<br>
  Elaboró: Christian Jaime García Aquino · Semana 7 (15–19 Junio 2026)
</footer>

</body>
</html>
