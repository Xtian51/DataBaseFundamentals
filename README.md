<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BDD · Semana 8 — Índices y Vistas</title>
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
    --accent: #fbbf24;
    --accent2: #fcd34d;
    --accent-soft: rgba(251,191,36,0.12);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg); color: var(--text); font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; line-height: 1.65; font-size: 15px; }
  header { background: radial-gradient(1200px 400px at 80% -20%, rgba(251,191,36,0.18), transparent 60%), linear-gradient(180deg, #11151c 0%, var(--bg) 100%); border-bottom: 1px solid var(--border); padding: 48px 32px 40px; }
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
  .definition { background: var(--accent-soft); border-color: rgba(251,191,36,0.35); }
  .definition .label { font-family: 'JetBrains Mono', monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--accent2); margin-bottom: 6px; }
  .note { background: var(--surface); }
  .note .label, .warn .label { font-weight: 700; font-size: 13px; margin-bottom: 4px; display: block; }
  .note .label { color: var(--accent2); }
  .warn { background: rgba(210,153,34,0.10); border-color: rgba(210,153,34,0.40); }
  .warn .label { color: #e3b341; }

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
  <h1>Semana 8 — <span>Índices y Vistas</span></h1>
  <p>Unidad III · Construcción — La base ya está construida, normalizada y poblada. Esta semana la hacemos rápida con índices, fácil de consultar y más segura con vistas, y más robusta con restricciones CHECK. Son objetos del esquema que todo desarrollador usa en el día a día.</p>
  <div class="pills">
    <span class="pill">📅 22 – 26 Junio 2026</span>
    <span class="pill">⏱ 5 horas</span>
    <span class="pill">🎯 Optimizar, abstraer y validar el esquema</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#puente">🔗 ¿Dónde estamos?</a>
    <a href="#problema">🐌 El problema</a>
    <a href="#indices">⚡ Índices</a>
    <a href="#costo">⚖️ El costo de indexar</a>
    <a href="#vistas">🪟 Vistas</a>
    <a href="#check">🛡️ Restricción CHECK</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <section id="puente">
      <div class="section-label"><span class="dot"></span>00 · Contexto</div>
      <h2>¿Dónde estamos?</h2>
      <p>Ya recorrimos el ciclo completo: construimos el esquema (Semana 5), lo diseñamos sin redundancia (Semana 6) y lo llenamos de datos (Semana 7). La base ya funciona. Pero "funcionar" no basta en un sistema real: también debe ser <strong>rápida</strong>, <strong>fácil de consultar con seguridad</strong> y <strong>difícil de corromper</strong>.</p>
      <p>Para eso existen tres herramientas que viven junto a las tablas, como objetos del esquema: los <strong>índices</strong> (velocidad), las <strong>vistas</strong> (abstracción y seguridad) y las restricciones <strong>CHECK</strong> (validación). Son, además, conceptos que un desarrollador toca prácticamente a diario.</p>
      <div class="definition">
        <div class="label">Meta de la semana</div>
        <p style="margin:0;">Acelerar las búsquedas con índices bien elegidos, simplificar y proteger el acceso con vistas, y reforzar la integridad del dominio con restricciones CHECK.</p>
      </div>
    </section>

    <section id="problema">
      <div class="section-label"><span class="dot"></span>01 · Motivación</div>
      <h2>El problema: búsquedas lentas</h2>
      <p>Imagina una tabla <code class="inline">socio</code> con 500 000 filas y esta consulta:</p>
      <pre><span class="kw">SELECT</span> * <span class="kw">FROM</span> socio <span class="kw">WHERE</span> nombre = <span class="st">'Ana López'</span>;</pre>
      <p>Sin ayuda, el SGBD revisa <strong>fila por fila</strong> hasta encontrarla: medio millón de comparaciones. A esto se le llama <em>recorrido completo de tabla</em> (full table scan) y es lento.</p>
      <div class="definition">
        <div class="label">La analogía del libro</div>
        <p style="margin:0;">Buscar sin índice es como buscar una palabra leyendo un libro de 800 páginas desde el inicio. Un <strong>índice</strong> es como el índice alfabético al final del libro: vas directo a la página correcta. La base de datos hace lo mismo.</p>
      </div>
    </section>

    <section id="indices">
      <div class="section-label"><span class="dot"></span>02 · Índices</div>
      <h2>Índices — velocidad de búsqueda</h2>
      <p>Un <strong>índice</strong> es una estructura auxiliar que el SGBD mantiene para localizar filas rápidamente por el valor de una o más columnas, sin recorrer toda la tabla.</p>
      <div class="note">
        <span class="label">Ya tienes índices sin saberlo</span>
        Cada <code class="inline">PRIMARY KEY</code> y cada columna <code class="inline">UNIQUE</code> crean un índice automáticamente. Por eso buscar por la llave primaria siempre es rápido.
      </div>
      <h3>Crear un índice</h3>
      <pre><span class="cm">-- Acelera las búsquedas y los JOIN por id_instructor</span>
<span class="kw">CREATE INDEX</span> idx_clase_instructor
<span class="kw">ON</span> clase (id_instructor);</pre>
      <h3>Índice compuesto (varias columnas)</h3>
      <pre><span class="cm">-- Útil para consultas que filtran por las dos columnas juntas</span>
<span class="kw">CREATE INDEX</span> idx_socio_nombre
<span class="kw">ON</span> socio (apellido, nombre);</pre>
      <p>En un índice compuesto <strong>el orden importa</strong>: <code class="inline">(apellido, nombre)</code> sirve para buscar por apellido, o por apellido + nombre, pero no para buscar solo por nombre.</p>
      <h3>¿Qué columnas conviene indexar?</h3>
      <ul>
        <li>Las que aparecen seguido en <code class="inline">WHERE</code>.</li>
        <li>Las usadas en <code class="inline">JOIN</code> (típicamente las llaves foráneas).</li>
        <li>Las usadas en <code class="inline">ORDER BY</code> sobre tablas grandes.</li>
      </ul>
      <div class="note">
        <span class="label">Ver si una consulta usa el índice</span>
        Antepón <code class="inline">EXPLAIN</code> a tu consulta (<code class="inline">EXPLAIN SELECT ...</code>). Si en la columna <code class="inline">key</code> aparece tu índice, lo está usando; si dice <code class="inline">NULL</code>, está haciendo recorrido completo.
      </div>
    </section>

    <section id="costo">
      <div class="section-label"><span class="dot"></span>03 · Equilibrio</div>
      <h2>El costo de indexar</h2>
      <p>Los índices no son gratis. Aceleran las <strong>lecturas</strong>, pero tienen un precio:</p>
      <table>
        <tr><th>Beneficio</th><th>Costo</th></tr>
        <tr><td>Las consultas <code>SELECT</code> con filtro son mucho más rápidas.</td><td>Ocupan espacio en disco adicional.</td></tr>
        <tr><td>Aceleran <code>JOIN</code> y <code>ORDER BY</code>.</td><td>Cada <code>INSERT</code>, <code>UPDATE</code> y <code>DELETE</code> es un poco más lento, porque hay que actualizar también el índice.</td></tr>
      </table>
      <div class="warn">
        <span class="label">⚠ Regla práctica</span>
        No indexes "por si acaso" todas las columnas. Indexa las que realmente se usan para buscar. En una tabla con muchas escrituras, demasiados índices la vuelven lenta para insertar.
      </div>
      <pre><span class="cm">-- Eliminar un índice que ya no aporta</span>
<span class="kw">DROP INDEX</span> idx_socio_nombre <span class="kw">ON</span> socio;</pre>
    </section>

    <section id="vistas">
      <div class="section-label"><span class="dot"></span>04 · Vistas</div>
      <h2>Vistas — consultas guardadas</h2>
      <div class="definition">
        <div class="label">Vista (VIEW)</div>
        <p style="margin:0;">Una <strong>vista</strong> es una consulta guardada que se comporta como una <strong>tabla virtual</strong>. No almacena datos: cada vez que la consultas, ejecuta por debajo su <code class="inline">SELECT</code> sobre las tablas reales.</p>
      </div>
      <h3>Crear y usar una vista</h3>
      <pre><span class="kw">CREATE VIEW</span> vista_inscripciones <span class="kw">AS</span>
<span class="kw">SELECT</span> s.nombre <span class="kw">AS</span> socio,
       c.nombre <span class="kw">AS</span> clase,
       i.fecha_inscripcion
<span class="kw">FROM</span> inscripcion i
<span class="kw">JOIN</span> socio s <span class="kw">ON</span> s.id_socio = i.id_socio
<span class="kw">JOIN</span> clase c <span class="kw">ON</span> c.id_clase = i.id_clase;

<span class="cm">-- Ahora se consulta como si fuera una tabla</span>
<span class="kw">SELECT</span> * <span class="kw">FROM</span> vista_inscripciones <span class="kw">WHERE</span> clase = <span class="st">'Yoga matutino'</span>;</pre>
      <h3>¿Para qué sirven?</h3>
      <table>
        <tr><th>Uso</th><th>Beneficio</th></tr>
        <tr><td><strong>Simplificar</strong></td><td>Encapsulan un JOIN complejo: lo escribes una vez y luego consultas la vista con un SELECT simple.</td></tr>
        <tr><td><strong>Seguridad</strong></td><td>Puedes dar acceso a una vista con solo ciertas columnas/filas, sin exponer la tabla completa (p. ej. ocultar el correo).</td></tr>
        <tr><td><strong>Abstracción</strong></td><td>Si la estructura de las tablas cambia, ajustas la vista y las aplicaciones que la usan no se enteran.</td></tr>
      </table>
      <div class="note">
        <span class="label">Nota</span>
        Algunas vistas permiten <code class="inline">INSERT</code>/<code class="inline">UPDATE</code> (si son simples), pero muchas (las que tienen JOIN o funciones) son de <strong>solo lectura</strong>. Para eliminar una vista: <code class="inline">DROP VIEW vista_inscripciones;</code>
      </div>
    </section>

    <section id="check">
      <div class="section-label"><span class="dot"></span>05 · Integridad</div>
      <h2>Restricción CHECK — validar el dominio</h2>
      <p>Ya conoces <code class="inline">NOT NULL</code>, <code class="inline">UNIQUE</code> y <code class="inline">FOREIGN KEY</code>. La restricción <strong>CHECK</strong> agrega una más: obliga a que un valor cumpla una <strong>condición lógica</strong> antes de aceptarlo.</p>
      <pre><span class="cm">-- Al crear la tabla</span>
cupo_max <span class="ty">INT</span> <span class="kw">NOT NULL</span> <span class="kw">CHECK</span> (cupo_max &gt; 0),

<span class="cm">-- O agregarla después con ALTER TABLE</span>
<span class="kw">ALTER TABLE</span> clase
<span class="kw">ADD CONSTRAINT</span> chk_cupo <span class="kw">CHECK</span> (cupo_max &gt; 0);</pre>
      <p>A partir de ahí, un intento de guardar un cupo de cero o negativo será rechazado:</p>
      <pre><span class="kw">INSERT INTO</span> clase (nombre, cupo_max, id_instructor)
<span class="kw">VALUES</span> (<span class="st">'Clase fantasma'</span>, 0, 1);
<span class="er">ERROR 4025: CONSTRAINT `chk_cupo` failed</span></pre>
      <div class="warn">
        <span class="label">⚠ Compatibilidad</span>
        <code class="inline">CHECK</code> se aplica de verdad en MariaDB 10.2+ y MySQL 8.0.16+. En versiones muy antiguas se aceptaba la sintaxis pero se ignoraba. Verifica la versión de tu XAMPP.
      </div>
    </section>

    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>06 · Ejemplos demostrativos</div>
      <h2>Ejemplos resueltos</h2>
      <p>Usamos la base del gimnasio (<code class="inline">instructor</code>, <code class="inline">socio</code>, <code class="inline">clase</code>, <code class="inline">inscripcion</code>) de la práctica guiada.</p>

      <h3>Ejemplo 1 — Un índice que sí hace falta</h3>
      <p>La llave primaria de <code class="inline">inscripcion</code> es <code class="inline">(id_socio, id_clase)</code>, así que buscar por socio ya es rápido. Pero buscar "quién está en la clase 2" filtra por <code class="inline">id_clase</code>, que va en segundo lugar de la PK y no se aprovecha. Agregamos un índice:</p>
      <pre><span class="kw">CREATE INDEX</span> idx_inscripcion_clase
<span class="kw">ON</span> inscripcion (id_clase);</pre>

      <h3>Ejemplo 2 — Una vista para reportes</h3>
      <pre><span class="kw">CREATE VIEW</span> vista_clases_llenas <span class="kw">AS</span>
<span class="kw">SELECT</span> c.nombre <span class="kw">AS</span> clase,
       i.nombre <span class="kw">AS</span> instructor,
       <span class="kw">COUNT</span>(ins.id_socio) <span class="kw">AS</span> inscritos,
       c.cupo_max
<span class="kw">FROM</span> clase c
<span class="kw">JOIN</span> instructor i <span class="kw">ON</span> i.id_instructor = c.id_instructor
<span class="kw">LEFT JOIN</span> inscripcion ins <span class="kw">ON</span> ins.id_clase = c.id_clase
<span class="kw">GROUP BY</span> c.id_clase;

<span class="kw">SELECT</span> * <span class="kw">FROM</span> vista_clases_llenas;</pre>
      <p>Ahora cualquiera puede ver el reporte con un simple <code class="inline">SELECT * FROM vista_clases_llenas;</code> sin escribir el JOIN cada vez.</p>

      <h3>Ejemplo 3 — Un CHECK que protege los datos</h3>
      <pre><span class="kw">ALTER TABLE</span> clase
<span class="kw">ADD CONSTRAINT</span> chk_cupo <span class="kw">CHECK</span> (cupo_max <span class="kw">BETWEEN</span> 1 <span class="kw">AND</span> 100);

<span class="cm">-- Esto ahora es imposible: el cupo fuera de rango se rechaza</span>
<span class="kw">UPDATE</span> clase <span class="kw">SET</span> cupo_max = 500 <span class="kw">WHERE</span> id_clase = 1;  <span class="er">-- ERROR</span></pre>
    </section>

    <section id="practica">
      <div class="section-label"><span class="dot"></span>07 · Ejercicios de práctica</div>
      <h2>Práctica</h2>

      <div class="practice">
        <span class="tag">Ejercicio 1 · Índices</span>
        <h3>Elige qué indexar</h3>
        <p>Dadas tres consultas frecuentes sobre la base del gimnasio (buscar socios por correo, listar clases de un instructor, y ordenar socios por fecha de alta), decide qué índices crearías y cuáles <strong>no</strong> hacen falta porque ya existen. Justifica cada decisión y escribe los <code class="inline">CREATE INDEX</code> necesarios.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 2 · Vistas</span>
        <h3>Crea una vista segura</h3>
        <p>Crea una vista <code class="inline">vista_directorio</code> que muestre solo el <code class="inline">nombre</code> y la <code class="inline">especialidad</code> de los instructores (sin exponer otros datos), y otra vista que liste cada socio con el número de clases en las que está inscrito.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 3 · CHECK</span>
        <h3>Blinda el dominio</h3>
        <p>Agrega restricciones <code class="inline">CHECK</code> que garanticen: (a) que <code class="inline">cupo_max</code> sea mayor que 0; (b) que el <code class="inline">correo</code> contenga el carácter <code class="inline">@</code>. Después intenta insertar datos que las violen y documenta el mensaje de error.</p>
      </div>
    </section>

    <section id="resumen">
      <div class="section-label"><span class="dot"></span>08 · Cierre</div>
      <h2>Resumen de la semana</h2>
      <div class="summary-grid">
        <div class="summary-item"><div class="icon">⚡</div><div class="label">Índice</div><div class="val">Lecturas rápidas</div></div>
        <div class="summary-item"><div class="icon">⚖️</div><div class="label">Costo</div><div class="val">Escrituras + lentas</div></div>
        <div class="summary-item"><div class="icon">🪟</div><div class="label">Vista</div><div class="val">Consulta guardada</div></div>
        <div class="summary-item"><div class="icon">🛡️</div><div class="label">CHECK</div><div class="val">Valida el dominio</div></div>
        <div class="summary-item"><div class="icon">🔍</div><div class="label">Diagnóstico</div><div class="val">EXPLAIN</div></div>
        <div class="summary-item"><div class="icon">➡️</div><div class="label">Próxima (S9)</div><div class="val">Administración del SGBD</div></div>
      </div>
      <div class="note" style="margin-top:22px;">
        <span class="label">Lo esencial</span>
        Un índice cambia lecturas lentas por lecturas rápidas, a cambio de escrituras un poco más caras: indexa con criterio. Una vista guarda una consulta y la presenta como tabla, ideal para simplificar y dar seguridad. Y un <code class="inline">CHECK</code> pone reglas de sentido común sobre los valores. Con esto, la Unidad III deja una base no solo correcta, sino también rápida y robusta.
      </div>
    </section>

  </main>
</div>
</div>

<footer>
  Bases de Datos · ITIID · Universidad Politécnica de Tapachula · Periodo Mayo–Agosto 2026 · Grupo 3°A<br>
  Elaboró: Christian Jaime García Aquino · Semana 8 (22–26 Junio 2026)
</footer>

</body>
</html>
