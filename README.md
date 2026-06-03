<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BDD · Semana 5 — DDL en XAMPP</title>
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
    --accent: #f85149;   /* carmesí */
    --accent2: #ff7b72;  /* coral */
    --accent-soft: rgba(248,81,73,0.12);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    line-height: 1.65;
    font-size: 15px;
  }

  /* ── HEADER ───────────────────────────── */
  header {
    background:
      radial-gradient(1200px 400px at 80% -20%, rgba(248,81,73,0.18), transparent 60%),
      linear-gradient(180deg, #11151c 0%, var(--bg) 100%);
    border-bottom: 1px solid var(--border);
    padding: 48px 32px 40px;
  }
  .badge {
    display: inline-block;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    letter-spacing: 0.5px;
    color: var(--accent2);
    border: 1px solid var(--border);
    background: var(--accent-soft);
    padding: 5px 12px;
    border-radius: 999px;
    margin-bottom: 18px;
  }
  header h1 {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 38px;
    line-height: 1.1;
    letter-spacing: -0.5px;
    max-width: 900px;
  }
  header h1 span { color: var(--accent); }
  header > p {
    color: var(--muted);
    margin-top: 12px;
    font-size: 16px;
    max-width: 760px;
  }
  .pills { margin-top: 22px; display: flex; gap: 10px; flex-wrap: wrap; }
  .pill {
    font-size: 13px;
    color: var(--text);
    background: var(--surface);
    border: 1px solid var(--border);
    padding: 7px 13px;
    border-radius: 8px;
  }

  /* ── LAYOUT ───────────────────────────── */
  .container { max-width: 1180px; margin: 0 auto; padding: 0 32px; }
  .layout {
    display: grid;
    grid-template-columns: 220px 1fr;
    gap: 40px;
    padding: 40px 0 80px;
  }
  nav.toc {
    position: sticky;
    top: 24px;
    align-self: start;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px 16px;
  }
  nav.toc h4 {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: var(--muted);
    margin-bottom: 12px;
  }
  nav.toc a {
    display: block;
    color: var(--text);
    text-decoration: none;
    font-size: 13.5px;
    padding: 7px 10px;
    border-radius: 7px;
    transition: background .15s, color .15s;
  }
  nav.toc a:hover { background: var(--accent-soft); color: var(--accent2); }

  main { min-width: 0; }
  section { margin-bottom: 46px; scroll-margin-top: 24px; }
  .section-label {
    display: flex; align-items: center; gap: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--accent2);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 10px;
  }
  .section-label .dot {
    width: 7px; height: 7px; border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 10px var(--accent);
  }
  h2 {
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 26px;
    letter-spacing: -0.3px;
    margin-bottom: 14px;
  }
  h3 {
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    font-size: 18px;
    margin: 26px 0 10px;
    color: var(--text);
  }
  p { margin-bottom: 12px; color: #cdd5de; }
  strong { color: var(--text); }
  code.inline {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    background: var(--surface2);
    color: var(--accent2);
    padding: 2px 6px;
    border-radius: 5px;
  }

  /* ── DEFINITION / NOTE BOXES ──────────── */
  .definition, .note, .warn {
    border-radius: 10px;
    padding: 16px 18px;
    margin: 18px 0;
    border: 1px solid var(--border);
  }
  .definition { background: var(--accent-soft); border-color: rgba(248,81,73,0.35); }
  .definition .label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; text-transform: uppercase; letter-spacing: 1px;
    color: var(--accent2); margin-bottom: 6px;
  }
  .note { background: var(--surface); }
  .note .label, .warn .label {
    font-weight: 700; font-size: 13px; margin-bottom: 4px; display: block;
  }
  .note .label { color: var(--accent2); }
  .warn { background: rgba(210,153,34,0.10); border-color: rgba(210,153,34,0.40); }
  .warn .label { color: #e3b341; }

  /* ── CODE BLOCKS (fix GitHub Pages) ───── */
  pre {
    background: #161b22 !important;
    color: #e6edf3 !important;
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 18px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12.5px;
    overflow-x: auto;
    margin: 16px 0;
    white-space: pre;
    line-height: 1.55;
  }
  pre .kw { color: #ff7b72; font-weight: 500; }
  pre .ty { color: #79c0ff; }
  pre .st { color: #a5d6ff; }
  pre .cm { color: #8b949e; font-style: italic; }
  .code-cap {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; color: var(--muted);
    margin: 18px 0 -8px; text-transform: uppercase; letter-spacing: 1px;
  }

  /* ── TABLES (fix GitHub Pages) ────────── */
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 16px 0;
    font-size: 13.5px;
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
  }
  th {
    background: #21262d !important;
    color: #e6edf3 !important;
    text-align: left;
    padding: 11px 14px;
    font-weight: 700;
    border-bottom: 1px solid var(--border);
  }
  td {
    background: #0d1117 !important;
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
    color: #cdd5de;
    vertical-align: top;
  }
  tr:nth-child(even) td { background: #161b22 !important; }
  td code, th code {
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent2);
    font-size: 12.5px;
  }

  /* ── PRACTICE CARDS ───────────────────── */
  .practice {
    background: var(--surface);
    border: 1px solid var(--border);
    border-left: 3px solid var(--accent);
    border-radius: 10px;
    padding: 18px 20px;
    margin: 18px 0;
  }
  .practice .tag {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px; color: var(--accent2);
    text-transform: uppercase; letter-spacing: 1px;
  }
  .practice h3 { margin-top: 6px; }
  ul, ol { margin: 8px 0 12px 22px; color: #cdd5de; }
  li { margin-bottom: 5px; }

  /* ── SUMMARY GRID ─────────────────────── */
  .summary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
    gap: 14px;
    margin-top: 18px;
  }
  .summary-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px;
    text-align: center;
  }
  .summary-item .icon { font-size: 22px; margin-bottom: 6px; }
  .summary-item .label { font-size: 11px; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; }
  .summary-item .val {
    font-family: 'Syne', sans-serif;
    font-size: 14px; font-weight: 600; color: var(--text); margin-top: 4px;
  }

  footer {
    border-top: 1px solid var(--border);
    padding: 24px 32px;
    text-align: center;
    color: var(--muted);
    font-size: 12.5px;
  }
  @media (max-width: 820px) {
    .layout { grid-template-columns: 1fr; }
    nav.toc { position: static; }
    header h1 { font-size: 30px; }
  }
</style>
</head>
<body>

<header>
  <div class="badge">ITIID · Bases de Datos · UPTap</div>
  <h1>Semana 5 — <span>DDL: construyendo la base de datos en XAMPP</span></h1>
  <p>Unidad II · Cierre — Del modelo relacional al SGBD físico. Tomamos el esquema y el diccionario de datos que ya diseñaron y lo materializamos como tablas reales en MySQL/MariaDB.</p>
  <div class="pills">
    <span class="pill">📅 01 – 05 Junio 2026</span>
    <span class="pill">⏱ 5 horas · teoría + laboratorio</span>
    <span class="pill">🎯 Crear, alterar y restringir tablas en XAMPP</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#puente">🔗 ¿Dónde estamos?</a>
    <a href="#xampp">🧰 XAMPP</a>
    <a href="#sublenguajes">📚 Sublenguajes SQL</a>
    <a href="#tipos">🔢 Tipos de datos</a>
    <a href="#create">🏗 CREATE</a>
    <a href="#restricciones">🔒 Restricciones</a>
    <a href="#alter">🔧 ALTER / DROP</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <!-- ═══ PUENTE ═══ -->
    <section id="puente">
      <div class="section-label"><span class="dot"></span>00 · Contexto</div>
      <h2>¿Dónde estamos y qué sigue?</h2>
      <p>Durante la Unidad II aprendieron a <strong>modelar</strong>: pasaron del mundo real a un diagrama Entidad-Relación, lo tradujeron al modelo relacional (tablas, llaves primarias y foráneas) y lo documentaron en un diccionario de datos. Ese fue el trabajo de su <strong>Evidencia 2</strong>.</p>
      <p>Pero hasta ahora todo ha vivido en papel y en diagramas. Esta semana damos el salto que hace tangible la materia: vamos a <strong>construir físicamente</strong> ese diseño dentro de un Sistema Gestor de Base de Datos (SGBD) real. Para eso usamos el lenguaje <strong>DDL</strong> (Data Definition Language) y la herramienta <strong>XAMPP</strong>, que trae MySQL/MariaDB listo para usar.</p>
      <div class="definition">
        <div class="label">Idea clave de la semana</div>
        <p style="margin:0;">Cada <strong>entidad</strong> de su diagrama se convierte en una <code class="inline">CREATE TABLE</code>; cada <strong>atributo</strong>, en una columna con su tipo de dato; cada <strong>llave primaria</strong>, en un <code class="inline">PRIMARY KEY</code>; y cada <strong>relación</strong>, en un <code class="inline">FOREIGN KEY</code>. El diccionario de datos es, literalmente, el plano de construcción.</p>
      </div>
    </section>

    <!-- ═══ XAMPP ═══ -->
    <section id="xampp">
      <div class="section-label"><span class="dot"></span>01 · Herramienta</div>
      <h2>XAMPP y phpMyAdmin</h2>
      <p><strong>XAMPP</strong> es un paquete gratuito que instala de un solo golpe un entorno de desarrollo web completo. Su nombre viene de <strong>X</strong> (multiplataforma), <strong>A</strong>pache, <strong>M</strong>ariaDB/MySQL, <strong>P</strong>HP y <strong>P</strong>erl. Para nuestra materia nos interesa principalmente el motor de base de datos.</p>
      <table>
        <tr><th>Componente</th><th>Para qué sirve en esta materia</th></tr>
        <tr><td><strong>MySQL / MariaDB</strong></td><td>El SGBD donde realmente viven las tablas y los datos. Aquí ejecutamos nuestro DDL.</td></tr>
        <tr><td><strong>Apache</strong></td><td>Servidor web. Necesario para que phpMyAdmin funcione en el navegador.</td></tr>
        <tr><td><strong>phpMyAdmin</strong></td><td>Interfaz gráfica para administrar la base de datos desde el navegador, sin escribir todo a mano.</td></tr>
        <tr><td>PHP / Perl</td><td>Lenguajes de servidor (los usaremos más adelante, no esta semana).</td></tr>
      </table>
      <h3>Flujo de trabajo en clase</h3>
      <ol>
        <li>Abrir el <strong>Panel de Control de XAMPP</strong> y presionar <strong>Start</strong> en <code class="inline">Apache</code> y en <code class="inline">MySQL</code> (deben quedar en verde).</li>
        <li>En el navegador ir a <code class="inline">http://localhost/phpmyadmin</code>.</li>
        <li>Usar la pestaña <strong>SQL</strong> para escribir y ejecutar los comandos DDL — así practican la sintaxis real, no solo el modo "clic".</li>
      </ol>
      <div class="note">
        <span class="label">Nota</span>
        Aunque phpMyAdmin permite crear tablas con botones, en esta semana <strong>escribiremos el SQL a mano</strong>. El objetivo es que dominen el lenguaje, no la interfaz: el SQL es portable a cualquier SGBD; los botones no.
      </div>
    </section>

    <!-- ═══ SUBLENGUAJES ═══ -->
    <section id="sublenguajes">
      <div class="section-label"><span class="dot"></span>02 · Teoría</div>
      <h2>Los sublenguajes de SQL</h2>
      <p>SQL no es un solo lenguaje monolítico; se divide en sublenguajes según la tarea. Esta semana nos concentramos en el primero.</p>
      <table>
        <tr><th>Sublenguaje</th><th>Significado</th><th>Comandos principales</th><th>¿Qué hace?</th></tr>
        <tr><td><strong>DDL</strong></td><td>Data Definition Language</td><td><code>CREATE</code>, <code>ALTER</code>, <code>DROP</code>, <code>TRUNCATE</code></td><td>Define y modifica la <em>estructura</em> (bases, tablas, columnas, restricciones).</td></tr>
        <tr><td>DML</td><td>Data Manipulation Language</td><td><code>SELECT</code>, <code>INSERT</code>, <code>UPDATE</code>, <code>DELETE</code></td><td>Trabaja con los <em>datos</em> dentro de las tablas (Unidad IV).</td></tr>
        <tr><td>DCL</td><td>Data Control Language</td><td><code>GRANT</code>, <code>REVOKE</code></td><td>Gestiona permisos y privilegios de usuarios.</td></tr>
        <tr><td>TCL</td><td>Transaction Control Language</td><td><code>COMMIT</code>, <code>ROLLBACK</code></td><td>Controla transacciones (confirmar o deshacer cambios).</td></tr>
      </table>
      <div class="definition">
        <div class="label">DDL en una frase</div>
        <p style="margin:0;">El DDL es el lenguaje con el que le <strong>describimos al SGBD cómo debe ser la base de datos</strong>: qué tablas existen, qué columnas tiene cada una, de qué tipo, y qué reglas deben cumplirse siempre.</p>
      </div>
    </section>

    <!-- ═══ TIPOS DE DATOS ═══ -->
    <section id="tipos">
      <div class="section-label"><span class="dot"></span>03 · Teoría</div>
      <h2>Tipos de datos en MySQL</h2>
      <p>Cada columna debe declarar un <strong>tipo de dato</strong>, que determina qué valores puede almacenar y cuánto espacio ocupa. Elegir bien el tipo es la primera decisión de calidad de un diseño: aquí es donde el <strong>dominio</strong> que escribieron en su diccionario de datos se vuelve un tipo concreto.</p>
      <table>
        <tr><th>Categoría</th><th>Tipo MySQL</th><th>Uso típico</th></tr>
        <tr><td rowspan="3">Numéricos</td><td><code>INT</code> / <code>BIGINT</code></td><td>Identificadores, cantidades enteras (matrícula, stock).</td></tr>
        <tr><td><code>DECIMAL(p,s)</code></td><td>Dinero y valores exactos. Ej. <code>DECIMAL(8,2)</code> → 999999.99</td></tr>
        <tr><td><code>FLOAT</code> / <code>DOUBLE</code></td><td>Valores con decimales aproximados (mediciones científicas).</td></tr>
        <tr><td rowspan="3">Cadenas</td><td><code>VARCHAR(n)</code></td><td>Texto de longitud variable hasta <code>n</code> (nombres, correos).</td></tr>
        <tr><td><code>CHAR(n)</code></td><td>Texto de longitud fija (RFC, códigos de tamaño constante).</td></tr>
        <tr><td><code>TEXT</code></td><td>Texto largo sin límite práctico (descripciones, comentarios).</td></tr>
        <tr><td rowspan="3">Fecha / hora</td><td><code>DATE</code></td><td>Solo fecha: <code>2026-06-01</code>.</td></tr>
        <tr><td><code>DATETIME</code></td><td>Fecha y hora: <code>2026-06-01 14:30:00</code>.</td></tr>
        <tr><td><code>TIMESTAMP</code></td><td>Marca temporal automática (fecha de creación/registro).</td></tr>
        <tr><td>Lógicos / otros</td><td><code>BOOLEAN</code>, <code>ENUM(...)</code></td><td>Verdadero/falso, o lista cerrada de valores (ej. estado).</td></tr>
      </table>
      <div class="warn">
        <span class="label">⚠ Error común</span>
        Usar <code class="inline">FLOAT</code> o <code class="inline">DOUBLE</code> para guardar dinero. Por su representación binaria pueden introducir errores de redondeo. Para precios y montos siempre usen <code class="inline">DECIMAL(p,s)</code>.
      </div>
    </section>

    <!-- ═══ CREATE ═══ -->
    <section id="create">
      <div class="section-label"><span class="dot"></span>04 · Teoría</div>
      <h2>CREATE DATABASE y CREATE TABLE</h2>
      <h3>Crear y seleccionar la base de datos</h3>
      <pre><span class="kw">CREATE DATABASE</span> escuela
    <span class="kw">CHARACTER SET</span> utf8mb4
    <span class="kw">COLLATE</span> utf8mb4_spanish_ci;

<span class="kw">USE</span> escuela;</pre>
      <p>El <code class="inline">CHARACTER SET utf8mb4</code> garantiza que se guarden correctamente acentos, ñ y emojis; el <code class="inline">COLLATE</code> define cómo se ordenan y comparan los textos (ej. que la "ñ" se ordene después de la "n").</p>

      <h3>Anatomía de un CREATE TABLE</h3>
      <pre><span class="kw">CREATE TABLE</span> nombre_tabla (
    columna1   <span class="ty">TIPO</span>   [restricciones de columna],
    columna2   <span class="ty">TIPO</span>   [restricciones de columna],
    ...
    [restricciones de tabla: PRIMARY KEY, FOREIGN KEY, UNIQUE]
);</pre>
      <p>Cada columna se define con un <strong>nombre</strong>, un <strong>tipo de dato</strong> y, opcionalmente, una o más <strong>restricciones</strong>. Las restricciones que involucran varias columnas o que son llaves se suelen declarar al final, como restricciones de tabla.</p>
    </section>

    <!-- ═══ RESTRICCIONES ═══ -->
    <section id="restricciones">
      <div class="section-label"><span class="dot"></span>05 · Teoría</div>
      <h2>Restricciones (constraints)</h2>
      <p>Las restricciones son las <strong>reglas de integridad</strong> que el SGBD vigilará por nosotros. Son el corazón de esta semana: aquí es donde el diseño deja de ser "una tabla con columnas" y se convierte en una base de datos confiable.</p>
      <table>
        <tr><th>Restricción</th><th>Garantiza que…</th></tr>
        <tr><td><code>PRIMARY KEY</code></td><td>La columna identifica de forma única cada fila y no puede repetirse ni ser nula.</td></tr>
        <tr><td><code>AUTO_INCREMENT</code></td><td>El SGBD genera automáticamente el siguiente número (ideal para IDs).</td></tr>
        <tr><td><code>NOT NULL</code></td><td>La columna siempre debe tener un valor (no se admite vacío).</td></tr>
        <tr><td><code>UNIQUE</code></td><td>No se repiten valores en esa columna (ej. correo, RFC).</td></tr>
        <tr><td><code>DEFAULT</code></td><td>Si no se indica valor al insertar, se usa uno por defecto.</td></tr>
        <tr><td><code>CHECK</code></td><td>El valor cumple una condición (ej. <code>edad >= 0</code>).</td></tr>
        <tr><td><code>FOREIGN KEY</code></td><td>El valor existe en la tabla referenciada — implementa las <strong>relaciones</strong> del modelo ER.</td></tr>
      </table>
      <h3>La FOREIGN KEY: las relaciones del ER hechas SQL</h3>
      <p>Cuando en su diagrama una entidad se relacionaba con otra (ej. un <em>alumno pertenece a una carrera</em>), esa relación se implementa con una <strong>llave foránea</strong>. Además podemos indicar qué pasa al borrar o actualizar el registro padre:</p>
      <table>
        <tr><th>Acción referencial</th><th>Significado</th></tr>
        <tr><td><code>ON DELETE RESTRICT</code></td><td>Impide borrar el padre si tiene hijos (es lo más seguro por defecto).</td></tr>
        <tr><td><code>ON DELETE CASCADE</code></td><td>Al borrar el padre, borra también automáticamente sus hijos.</td></tr>
        <tr><td><code>ON DELETE SET NULL</code></td><td>Al borrar el padre, deja la FK del hijo en <code>NULL</code>.</td></tr>
        <tr><td><code>ON UPDATE CASCADE</code></td><td>Si cambia la llave del padre, propaga el cambio a los hijos.</td></tr>
      </table>
    </section>

    <!-- ═══ ALTER / DROP ═══ -->
    <section id="alter">
      <div class="section-label"><span class="dot"></span>06 · Teoría</div>
      <h2>ALTER TABLE, DROP y TRUNCATE</h2>
      <p>El diseño casi nunca queda perfecto al primer intento. <code class="inline">ALTER TABLE</code> nos permite <strong>modificar la estructura</strong> de una tabla ya creada sin tener que borrarla.</p>
      <pre><span class="cm">-- Agregar una columna</span>
<span class="kw">ALTER TABLE</span> alumno <span class="kw">ADD COLUMN</span> telefono <span class="ty">VARCHAR</span>(15);

<span class="cm">-- Cambiar el tipo o las restricciones de una columna</span>
<span class="kw">ALTER TABLE</span> alumno <span class="kw">MODIFY COLUMN</span> nombre <span class="ty">VARCHAR</span>(120) <span class="kw">NOT NULL</span>;

<span class="cm">-- Eliminar una columna</span>
<span class="kw">ALTER TABLE</span> alumno <span class="kw">DROP COLUMN</span> telefono;

<span class="cm">-- Agregar una llave foránea despues de crear la tabla</span>
<span class="kw">ALTER TABLE</span> alumno
    <span class="kw">ADD CONSTRAINT</span> fk_alumno_carrera
    <span class="kw">FOREIGN KEY</span> (id_carrera) <span class="kw">REFERENCES</span> carrera(id_carrera);</pre>
      <h3>Eliminar estructuras</h3>
      <table>
        <tr><th>Comando</th><th>Qué elimina</th><th>¿Recuperable?</th></tr>
        <tr><td><code>DROP TABLE alumno;</code></td><td>La tabla completa: estructura + datos.</td><td>No</td></tr>
        <tr><td><code>DROP DATABASE escuela;</code></td><td>Toda la base de datos.</td><td>No</td></tr>
        <tr><td><code>TRUNCATE TABLE alumno;</code></td><td>Todos los datos, pero conserva la estructura.</td><td>No</td></tr>
        <tr><td><code>DELETE FROM alumno;</code></td><td>Filas (DML, no DDL); puede filtrarse con WHERE.</td><td>Sí (con transacción)</td></tr>
      </table>
      <div class="warn">
        <span class="label">⚠ Cuidado</span>
        <code class="inline">DROP</code> y <code class="inline">TRUNCATE</code> no piden confirmación y no se pueden deshacer fácilmente. En clase trabajamos sobre bases de prueba, pero en producción un <code class="inline">DROP</code> mal escrito es un desastre.
      </div>
    </section>

    <!-- ═══ EJEMPLOS ═══ -->
    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>07 · Ejemplos demostrativos</div>
      <h2>Ejemplos resueltos</h2>

      <h3>Ejemplo 1 — Tabla ALUMNO completa</h3>
      <p>Retomamos la entidad <strong>ALUMNO</strong> que vimos en la Semana 1, ahora con todas sus restricciones. Observen cómo cada decisión refleja una regla del negocio.</p>
      <pre><span class="kw">CREATE TABLE</span> alumno (
    id_alumno     <span class="ty">INT</span>           <span class="kw">AUTO_INCREMENT</span>,
    matricula     <span class="ty">CHAR</span>(10)      <span class="kw">NOT NULL</span> <span class="kw">UNIQUE</span>,
    nombre        <span class="ty">VARCHAR</span>(100)  <span class="kw">NOT NULL</span>,
    correo        <span class="ty">VARCHAR</span>(120)  <span class="kw">NOT NULL</span> <span class="kw">UNIQUE</span>,
    fecha_nac     <span class="ty">DATE</span>,
    promedio      <span class="ty">DECIMAL</span>(4,2)  <span class="kw">DEFAULT</span> 0.00,
    activo        <span class="ty">BOOLEAN</span>       <span class="kw">DEFAULT</span> <span class="kw">TRUE</span>,
    <span class="kw">PRIMARY KEY</span> (id_alumno)
);</pre>
      <ul>
        <li><code class="inline">id_alumno</code> es la PK artificial con <code class="inline">AUTO_INCREMENT</code>: el sistema asigna el número.</li>
        <li><code class="inline">matricula</code> y <code class="inline">correo</code> son <code class="inline">UNIQUE</code>: no pueden repetirse entre alumnos.</li>
        <li><code class="inline">promedio</code> usa <code class="inline">DECIMAL(4,2)</code> (rango 0.00–99.99) con valor por defecto 0.</li>
      </ul>

      <h3>Ejemplo 2 — Relación 1:N (CARRERA → ALUMNO)</h3>
      <p>Una carrera tiene muchos alumnos; cada alumno pertenece a una carrera. Esa relación del ER se vuelve una FK.</p>
      <pre><span class="kw">CREATE TABLE</span> carrera (
    id_carrera    <span class="ty">INT</span>           <span class="kw">AUTO_INCREMENT</span>,
    nombre        <span class="ty">VARCHAR</span>(80)   <span class="kw">NOT NULL</span> <span class="kw">UNIQUE</span>,
    <span class="kw">PRIMARY KEY</span> (id_carrera)
);

<span class="kw">CREATE TABLE</span> alumno (
    id_alumno     <span class="ty">INT</span>           <span class="kw">AUTO_INCREMENT</span>,
    matricula     <span class="ty">CHAR</span>(10)      <span class="kw">NOT NULL</span> <span class="kw">UNIQUE</span>,
    nombre        <span class="ty">VARCHAR</span>(100)  <span class="kw">NOT NULL</span>,
    id_carrera    <span class="ty">INT</span>           <span class="kw">NOT NULL</span>,
    <span class="kw">PRIMARY KEY</span> (id_alumno),
    <span class="kw">FOREIGN KEY</span> (id_carrera)
        <span class="kw">REFERENCES</span> carrera(id_carrera)
        <span class="kw">ON DELETE</span> <span class="kw">RESTRICT</span>
        <span class="kw">ON UPDATE</span> <span class="kw">CASCADE</span>
);</pre>
      <div class="note">
        <span class="label">Orden importa</span>
        La tabla referenciada (<code class="inline">carrera</code>) debe crearse <strong>antes</strong> que la que la referencia (<code class="inline">alumno</code>), o MySQL no encontrará la tabla destino de la FK.
      </div>

      <h3>Ejemplo 3 — Corregir el diseño con ALTER</h3>
      <p>Supongamos que ya creamos <code class="inline">alumno</code> pero olvidamos el correo y la FK. Lo arreglamos sin borrar la tabla:</p>
      <pre><span class="kw">ALTER TABLE</span> alumno <span class="kw">ADD COLUMN</span> correo <span class="ty">VARCHAR</span>(120) <span class="kw">NOT NULL</span> <span class="kw">UNIQUE</span>;

<span class="kw">ALTER TABLE</span> alumno
    <span class="kw">ADD CONSTRAINT</span> fk_alumno_carrera
    <span class="kw">FOREIGN KEY</span> (id_carrera) <span class="kw">REFERENCES</span> carrera(id_carrera);</pre>
    </section>

    <!-- ═══ PRACTICA ═══ -->
    <section id="practica">
      <div class="section-label"><span class="dot"></span>08 · Ejercicios de práctica</div>
      <h2>Práctica en XAMPP</h2>

      <div class="practice">
        <span class="tag">Práctica 1 · Individual</span>
        <h3>Materializa tu Evidencia 2</h3>
        <p>Toma el esquema relacional y el diccionario de datos de <strong>tu propio proyecto de la Evidencia 2</strong> y constrúyelo en XAMPP.</p>
        <ul>
          <li>Crea la base de datos con <code class="inline">utf8mb4</code>.</li>
          <li>Crea todas las tablas con sus tipos de dato correctos (justificados por el dominio del diccionario).</li>
          <li>Define todas las <code class="inline">PRIMARY KEY</code> y las <code class="inline">FOREIGN KEY</code> que correspondan a las relaciones de tu diagrama.</li>
          <li><strong>Entregable:</strong> archivo <code class="inline">.sql</code> con todo el DDL + captura de phpMyAdmin mostrando las tablas creadas.</li>
        </ul>
      </div>

      <div class="practice">
        <span class="tag">Práctica 2 · Equipos de 2</span>
        <h3>Sistema de biblioteca (incluye relación N:M)</h3>
        <p>Diseña y crea en XAMPP la base <code class="inline">biblioteca</code> con estas tablas:</p>
        <ul>
          <li><code class="inline">autor</code>, <code class="inline">libro</code>, <code class="inline">socio</code> y <code class="inline">prestamo</code>.</li>
          <li>Un libro puede tener varios autores y un autor varios libros (<strong>N:M</strong>): resuélvelo con la tabla intermedia <code class="inline">libro_autor</code> con FK a ambas.</li>
          <li><code class="inline">prestamo</code> debe registrar qué socio se llevó qué libro, con <code class="inline">fecha_prestamo</code> y <code class="inline">fecha_devolucion</code>.</li>
          <li>Aplica <code class="inline">NOT NULL</code>, <code class="inline">UNIQUE</code> (ej. ISBN) y al menos un <code class="inline">DEFAULT</code> donde tenga sentido.</li>
        </ul>
      </div>

      <div class="practice">
        <span class="tag">Práctica 3 · Reto</span>
        <h3>Cirugía con ALTER TABLE</h3>
        <p>Se te entregará un script con un diseño defectuoso ya creado. Sin borrar las tablas, corrígelo usando solo <code class="inline">ALTER TABLE</code>:</p>
        <ul>
          <li>Cambiar una columna de <code class="inline">FLOAT</code> a <code class="inline">DECIMAL(8,2)</code> para un precio.</li>
          <li>Agregar una <code class="inline">FOREIGN KEY</code> que faltaba.</li>
          <li>Agregar la restricción <code class="inline">UNIQUE</code> a una columna de correo.</li>
          <li>Documentar en comentarios qué problema resolvía cada cambio.</li>
        </ul>
      </div>
    </section>

    <!-- ═══ RESUMEN ═══ -->
    <section id="resumen">
      <div class="section-label"><span class="dot"></span>09 · Cierre</div>
      <h2>Resumen de la semana</h2>
      <div class="summary-grid">
        <div class="summary-item"><div class="icon">🧰</div><div class="label">Herramienta</div><div class="val">XAMPP · phpMyAdmin</div></div>
        <div class="summary-item"><div class="icon">📚</div><div class="label">Sublenguaje</div><div class="val">DDL</div></div>
        <div class="summary-item"><div class="icon">🏗</div><div class="label">Comandos</div><div class="val">CREATE · ALTER · DROP</div></div>
        <div class="summary-item"><div class="icon">🔒</div><div class="label">Restricciones</div><div class="val">PK · FK · UNIQUE · NOT NULL</div></div>
        <div class="summary-item"><div class="icon">📦</div><div class="label">Entregable</div><div class="val">Script .sql + capturas</div></div>
        <div class="summary-item"><div class="icon">➡️</div><div class="label">Próxima (U3)</div><div class="val">Normalización 1FN–3FN</div></div>
      </div>
      <div class="note" style="margin-top:22px;">
        <span class="label">Conexión con la Unidad III</span>
        Ya tienen tablas reales. La siguiente pregunta natural es: ¿está bien diseñada esa estructura o tiene datos redundantes que generarán problemas al insertar, actualizar y borrar? Eso lo responde la <strong>normalización</strong>, con la que abrimos la Unidad III.
      </div>
    </section>

  </main>
</div>
</div>

<footer>
  Bases de Datos · ITIID · Universidad Politécnica de Tapachula · Periodo Mayo–Agosto 2026 · Grupo 3°A<br>
  Elaboró: Christian Jaime García Aquino · Semana 5 (01–05 Junio 2026)
</footer>

</body>
</html>
