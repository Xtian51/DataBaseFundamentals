<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BDD · Semana 6 — Normalización</title>
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
    --accent: #2dd4bf;   /* teal */
    --accent2: #5eead4;  /* teal claro */
    --accent-soft: rgba(45,212,191,0.12);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    line-height: 1.65;
    font-size: 15px;
  }
  header {
    background:
      radial-gradient(1200px 400px at 80% -20%, rgba(45,212,191,0.18), transparent 60%),
      linear-gradient(180deg, #11151c 0%, var(--bg) 100%);
    border-bottom: 1px solid var(--border);
    padding: 48px 32px 40px;
  }
  .badge {
    display: inline-block;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px; letter-spacing: 0.5px;
    color: var(--accent2);
    border: 1px solid var(--border);
    background: var(--accent-soft);
    padding: 5px 12px; border-radius: 999px; margin-bottom: 18px;
  }
  header h1 {
    font-family: 'Syne', sans-serif; font-weight: 800;
    font-size: 38px; line-height: 1.1; letter-spacing: -0.5px; max-width: 900px;
  }
  header h1 span { color: var(--accent); }
  header > p { color: var(--muted); margin-top: 12px; font-size: 16px; max-width: 780px; }
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

  .definition, .note, .warn {
    border-radius: 10px; padding: 16px 18px; margin: 18px 0; border: 1px solid var(--border);
  }
  .definition { background: var(--accent-soft); border-color: rgba(45,212,191,0.35); }
  .definition .label { font-family: 'JetBrains Mono', monospace; font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--accent2); margin-bottom: 6px; }
  .note { background: var(--surface); }
  .note .label, .warn .label { font-weight: 700; font-size: 13px; margin-bottom: 4px; display: block; }
  .note .label { color: var(--accent2); }
  .warn { background: rgba(210,153,34,0.10); border-color: rgba(210,153,34,0.40); }
  .warn .label { color: #e3b341; }

  pre { background: #161b22 !important; color: #e6edf3 !important; border: 1px solid var(--border); border-radius: 10px; padding: 16px 18px; font-family: 'JetBrains Mono', monospace; font-size: 12.5px; overflow-x: auto; margin: 16px 0; white-space: pre; line-height: 1.55; }
  pre .kw { color: #ff7b72; font-weight: 500; }
  pre .ty { color: #79c0ff; }
  pre .cm { color: #8b949e; font-style: italic; }

  table { width: 100%; border-collapse: collapse; margin: 16px 0; font-size: 13.5px; border: 1px solid var(--border); border-radius: 10px; overflow: hidden; }
  th { background: #21262d !important; color: #e6edf3 !important; text-align: left; padding: 10px 13px; font-weight: 700; border-bottom: 1px solid var(--border); }
  td { background: #0d1117 !important; padding: 9px 13px; border-bottom: 1px solid var(--border); color: #cdd5de; vertical-align: top; }
  tr:nth-child(even) td { background: #161b22 !important; }
  td code, th code { font-family: 'JetBrains Mono', monospace; color: var(--accent2); font-size: 12.5px; }
  td.bad { color: #ff7b72 !important; }
  td.key { color: #5eead4 !important; font-weight: 700; }
  .tbl-cap { font-family: 'JetBrains Mono', monospace; font-size: 11px; color: var(--muted); margin: 18px 0 -8px; text-transform: uppercase; letter-spacing: 1px; }

  .fnbadge { display: inline-flex; align-items: center; justify-content: center; min-width: 56px; height: 28px; padding: 0 10px; border-radius: 7px; background: var(--accent-soft); border: 1px solid rgba(45,212,191,0.4); color: var(--accent2); font-family: 'JetBrains Mono', monospace; font-weight: 700; font-size: 14px; margin-right: 10px; }

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

  .mnemo { background: linear-gradient(135deg, rgba(45,212,191,0.14), rgba(45,212,191,0.04)); border: 1px solid rgba(45,212,191,0.4); border-radius: 12px; padding: 22px 24px; margin: 22px 0; text-align: center; }
  .mnemo p { font-family: 'Syne', sans-serif; font-size: 19px; color: var(--text); margin: 0; line-height: 1.5; }
  .mnemo .src { font-size: 12px; color: var(--muted); margin-top: 8px; font-family: 'JetBrains Mono', monospace; }

  footer { border-top: 1px solid var(--border); padding: 24px 32px; text-align: center; color: var(--muted); font-size: 12.5px; }
  @media (max-width: 820px) { .layout { grid-template-columns: 1fr; } nav.toc { position: static; } header h1 { font-size: 30px; } }
</style>
</head>
<body>

<header>
  <div class="badge">ITIID · Bases de Datos · UPTap</div>
  <h1>Semana 6 — <span>Normalización: diseñar sin redundancia</span></h1>
  <p>Unidad III · Construcción — Ya tienen tablas reales construidas con DDL. Ahora aprenderán a evaluar y rediseñar esas estructuras para eliminar la redundancia y las anomalías, aplicando las formas normales 1FN, 2FN y 3FN.</p>
  <div class="pills">
    <span class="pill">📅 08 – 12 Junio 2026</span>
    <span class="pill">⏱ 5 horas</span>
    <span class="pill">🎯 Aplicar 1FN, 2FN y 3FN a un diseño</span>
  </div>
</header>

<div class="container">
<div class="layout">

  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#porque">⚠️ ¿Por qué normalizar?</a>
    <a href="#df">🔗 Dependencias funcionales</a>
    <a href="#fn1">1️⃣ Primera Forma Normal</a>
    <a href="#fn2">2️⃣ Segunda Forma Normal</a>
    <a href="#fn3">3️⃣ Tercera Forma Normal</a>
    <a href="#integrador">🧪 Ejemplo integrador</a>
    <a href="#bcnf">➕ Más allá: BCNF</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <main>

    <!-- ¿POR QUÉ NORMALIZAR? -->
    <section id="porque">
      <div class="section-label"><span class="dot"></span>00 · Motivación</div>
      <h2>¿Por qué normalizar?</h2>
      <p>Imaginen que registramos las inscripciones de los alumnos en una sola tabla "gigante" que lo guarda todo junto:</p>
      <div class="tbl-cap">Tabla INSCRIPCION (sin normalizar)</div>
      <table>
        <tr><th>matricula</th><th>nombre_alumno</th><th>id_curso</th><th>nombre_curso</th><th>profesor</th><th>calif</th></tr>
        <tr><td class="key">A01</td><td>Ana López</td><td class="key">C10</td><td>Bases de Datos</td><td>García</td><td>9.0</td></tr>
        <tr><td class="key">A01</td><td>Ana López</td><td class="key">C20</td><td>Redes</td><td>Pérez</td><td>8.5</td></tr>
        <tr><td class="key">A02</td><td>Beto Ramírez</td><td class="key">C10</td><td>Bases de Datos</td><td>García</td><td>7.8</td></tr>
      </table>
      <p>Esta tabla <strong>funciona</strong>, pero la información se repite: "Ana López" y "Bases de Datos" aparecen varias veces. Esa redundancia genera tres problemas clásicos, las <strong>anomalías</strong>:</p>
      <table>
        <tr><th>Anomalía</th><th>Problema</th></tr>
        <tr><td><strong>de inserción</strong></td><td>No puedo registrar un curso nuevo hasta que algún alumno se inscriba en él (necesitaría una matrícula).</td></tr>
        <tr><td><strong>de actualización</strong></td><td>Si "Bases de Datos" cambia de nombre, debo editar muchas filas; si olvido una, los datos quedan inconsistentes.</td></tr>
        <tr><td><strong>de eliminación</strong></td><td>Si Beto cancela su única inscripción y borro la fila, pierdo también el dato de que el curso C10 existe.</td></tr>
      </table>
      <div class="definition">
        <div class="label">Normalización</div>
        <p style="margin:0;">Proceso de organizar las columnas y tablas de una base de datos para <strong>minimizar la redundancia</strong> y evitar las anomalías, descomponiendo una tabla en varias relacionadas mediante una serie de reglas llamadas <strong>formas normales</strong>.</p>
      </div>
    </section>

    <!-- DEPENDENCIAS FUNCIONALES -->
    <section id="df">
      <div class="section-label"><span class="dot"></span>01 · Fundamento</div>
      <h2>Dependencias funcionales</h2>
      <p>Antes de las formas normales necesitamos una herramienta: la <strong>dependencia funcional</strong>. Es la base teórica de toda la normalización.</p>
      <div class="definition">
        <div class="label">Dependencia funcional  ·  A → B</div>
        <p style="margin:0;">Decimos que <strong>B depende funcionalmente de A</strong> (se escribe <code class="inline">A → B</code>) si cada valor de A determina un único valor de B. Es decir: si conozco A, conozco B sin ambigüedad.</p>
      </div>
      <p>Ejemplos en la tabla anterior:</p>
      <ul>
        <li><code class="inline">matricula → nombre_alumno</code> &nbsp;(cada matrícula identifica a un solo alumno)</li>
        <li><code class="inline">id_curso → nombre_curso, profesor</code> &nbsp;(cada curso tiene un nombre y un profesor)</li>
        <li><code class="inline">(matricula, id_curso) → calif</code> &nbsp;(la calificación depende de la combinación alumno + curso)</li>
      </ul>
      <p>Dos tipos de dependencia serán claves para las siguientes reglas:</p>
      <table>
        <tr><th>Tipo</th><th>Qué es</th><th>La ataca…</th></tr>
        <tr><td><strong>Parcial</strong></td><td>Un atributo depende solo de <em>parte</em> de una clave compuesta.</td><td>2FN</td></tr>
        <tr><td><strong>Transitiva</strong></td><td>Un atributo no clave depende de <em>otro atributo no clave</em> (A → B → C).</td><td>3FN</td></tr>
      </table>
    </section>

    <!-- 1FN -->
    <section id="fn1">
      <div class="section-label"><span class="dot"></span>02 · Regla</div>
      <h2><span class="fnbadge">1FN</span>Primera Forma Normal</h2>
      <div class="definition">
        <div class="label">Una tabla está en 1FN si…</div>
        <p style="margin:0;">Cada celda contiene un <strong>valor atómico</strong> (indivisible), no hay <strong>grupos repetitivos</strong> ni listas dentro de una columna, y existe una <strong>llave primaria</strong> que identifica cada fila.</p>
      </div>
      <p>Esta tabla <strong>viola</strong> la 1FN: la columna <code class="inline">telefonos</code> guarda varios valores en una celda.</p>
      <div class="tbl-cap">ALUMNO — viola 1FN</div>
      <table>
        <tr><th>matricula</th><th>nombre</th><th>telefonos</th></tr>
        <tr><td class="key">A01</td><td>Ana López</td><td class="bad">961-111, 962-222</td></tr>
        <tr><td class="key">A02</td><td>Beto Ramírez</td><td class="bad">963-333</td></tr>
      </table>
      <p>Para llevarla a 1FN, sacamos los teléfonos a su propia tabla, con un valor por fila:</p>
      <div class="tbl-cap">ALUMNO (1FN)  +  TELEFONO (1FN)</div>
      <table>
        <tr><th>matricula</th><th>nombre</th></tr>
        <tr><td class="key">A01</td><td>Ana López</td></tr>
        <tr><td class="key">A02</td><td>Beto Ramírez</td></tr>
      </table>
      <table>
        <tr><th>id_tel</th><th>matricula</th><th>numero</th></tr>
        <tr><td class="key">1</td><td>A01</td><td>961-111</td></tr>
        <tr><td class="key">2</td><td>A01</td><td>962-222</td></tr>
        <tr><td class="key">3</td><td>A02</td><td>963-333</td></tr>
      </table>
    </section>

    <!-- 2FN -->
    <section id="fn2">
      <div class="section-label"><span class="dot"></span>03 · Regla</div>
      <h2><span class="fnbadge">2FN</span>Segunda Forma Normal</h2>
      <div class="definition">
        <div class="label">Una tabla está en 2FN si…</div>
        <p style="margin:0;">Está en <strong>1FN</strong> y, además, todo atributo que no es clave depende de la <strong>clave completa</strong>, no solo de una parte de ella. La 2FN solo es un riesgo cuando la llave primaria es <strong>compuesta</strong>.</p>
      </div>
      <p>Volvamos a la inscripción. Su clave es compuesta: <code class="inline">(matricula, id_curso)</code>. Observen las dependencias:</p>
      <div class="tbl-cap">INSCRIPCION (1FN, pero viola 2FN)</div>
      <table>
        <tr><th>matricula</th><th>id_curso</th><th>nombre_alumno</th><th>nombre_curso</th><th>calif</th></tr>
        <tr><td class="key">A01</td><td class="key">C10</td><td class="bad">Ana López</td><td class="bad">Bases de Datos</td><td>9.0</td></tr>
        <tr><td class="key">A01</td><td class="key">C20</td><td class="bad">Ana López</td><td class="bad">Redes</td><td>8.5</td></tr>
      </table>
      <ul>
        <li><code class="inline">nombre_alumno</code> depende solo de <code class="inline">matricula</code> → <strong>dependencia parcial</strong>.</li>
        <li><code class="inline">nombre_curso</code> depende solo de <code class="inline">id_curso</code> → <strong>dependencia parcial</strong>.</li>
        <li><code class="inline">calif</code> depende de la clave completa → correcto.</li>
      </ul>
      <p>Para llegar a 2FN, separamos cada dependencia parcial a su propia tabla:</p>
      <div class="tbl-cap">ALUMNO  ·  CURSO  ·  INSCRIPCION (2FN)</div>
      <table>
        <tr><th>matricula (PK)</th><th>nombre_alumno</th></tr>
        <tr><td class="key">A01</td><td>Ana López</td></tr>
      </table>
      <table>
        <tr><th>id_curso (PK)</th><th>nombre_curso</th></tr>
        <tr><td class="key">C10</td><td>Bases de Datos</td></tr>
      </table>
      <table>
        <tr><th>matricula (PK,FK)</th><th>id_curso (PK,FK)</th><th>calif</th></tr>
        <tr><td class="key">A01</td><td class="key">C10</td><td>9.0</td></tr>
      </table>
    </section>

    <!-- 3FN -->
    <section id="fn3">
      <div class="section-label"><span class="dot"></span>04 · Regla</div>
      <h2><span class="fnbadge">3FN</span>Tercera Forma Normal</h2>
      <div class="definition">
        <div class="label">Una tabla está en 3FN si…</div>
        <p style="margin:0;">Está en <strong>2FN</strong> y ningún atributo no clave depende de <strong>otro atributo no clave</strong> (no hay <strong>dependencias transitivas</strong>). Todo atributo debe depender directamente de la llave.</p>
      </div>
      <p>Supongamos que la tabla CURSO también guarda el nombre del profesor:</p>
      <div class="tbl-cap">CURSO (2FN, pero viola 3FN)</div>
      <table>
        <tr><th>id_curso (PK)</th><th>nombre_curso</th><th>id_profesor</th><th>nombre_profesor</th></tr>
        <tr><td class="key">C10</td><td>Bases de Datos</td><td>P1</td><td class="bad">García</td></tr>
        <tr><td class="key">C20</td><td>Redes</td><td>P2</td><td class="bad">Pérez</td></tr>
      </table>
      <p>Aquí <code class="inline">nombre_profesor</code> no depende de <code class="inline">id_curso</code>, sino de <code class="inline">id_profesor</code>, que tampoco es clave: <code class="inline">id_curso → id_profesor → nombre_profesor</code>. Eso es una <strong>dependencia transitiva</strong>. La rompemos sacando al profesor a su tabla:</p>
      <div class="tbl-cap">CURSO  ·  PROFESOR (3FN)</div>
      <table>
        <tr><th>id_curso (PK)</th><th>nombre_curso</th><th>id_profesor (FK)</th></tr>
        <tr><td class="key">C10</td><td>Bases de Datos</td><td>P1</td></tr>
      </table>
      <table>
        <tr><th>id_profesor (PK)</th><th>nombre_profesor</th></tr>
        <tr><td class="key">P1</td><td>García</td></tr>
      </table>
      <div class="mnemo">
        <p>“Cada atributo depende de <strong>la clave</strong>, de <strong>toda la clave</strong><br>y de <strong>nada más que la clave</strong>.”</p>
        <div class="src">// la clave → 1FN · toda la clave → 2FN · nada más que la clave → 3FN</div>
      </div>
    </section>

    <!-- EJEMPLO INTEGRADOR -->
    <section id="integrador">
      <div class="section-label"><span class="dot"></span>05 · Ejemplo integrador</div>
      <h2>De 0 a 3FN, paso a paso</h2>
      <p>Reunimos todo. Partimos de una sola tabla desnormalizada y la llevamos hasta 3FN aplicando las reglas en orden.</p>
      <h3>Punto de partida (sin normalizar)</h3>
      <div class="tbl-cap">INSCRIPCION_FULL</div>
      <table>
        <tr><th>matricula</th><th>nombre_alumno</th><th>cursos_y_calif</th><th>id_profesor</th><th>nombre_profesor</th></tr>
        <tr><td class="key">A01</td><td>Ana López</td><td class="bad">C10:9.0, C20:8.5</td><td>P1, P2</td><td>García, Pérez</td></tr>
      </table>
      <ol>
        <li><strong>1FN</strong> — separamos el grupo repetitivo <code class="inline">cursos_y_calif</code>: una fila por (alumno, curso).</li>
        <li><strong>2FN</strong> — sacamos <code class="inline">nombre_alumno</code> a ALUMNO y <code class="inline">nombre_curso</code> a CURSO (dependencias parciales de la clave compuesta).</li>
        <li><strong>3FN</strong> — sacamos al profesor a su tabla (dependencia transitiva vía <code class="inline">id_profesor</code>).</li>
      </ol>
      <h3>Esquema final en 3FN</h3>
      <pre><span class="cm">-- Cinco relaciones limpias, sin redundancia</span>
ALUMNO     (<span class="ty">matricula</span> PK, nombre_alumno)
PROFESOR   (<span class="ty">id_profesor</span> PK, nombre_profesor)
CURSO      (<span class="ty">id_curso</span> PK, nombre_curso, id_profesor FK)
INSCRIPCION(<span class="ty">matricula</span> PK/FK, <span class="ty">id_curso</span> PK/FK, calif)</pre>
      <div class="note">
        <span class="label">Conexión con la Semana 5</span>
        Una vez normalizado el diseño en papel, cada una de estas relaciones se construye con un <code class="inline">CREATE TABLE</code> y sus <code class="inline">FOREIGN KEY</code>, tal como practicamos en XAMPP.
      </div>
    </section>

    <!-- BCNF -->
    <section id="bcnf">
      <div class="section-label"><span class="dot"></span>06 · Para ir más allá</div>
      <h2>Un paso más: BCNF</h2>
      <p>Existen formas normales más estrictas. La más conocida es la <strong>Forma Normal de Boyce-Codd (BCNF)</strong>, un refuerzo de la 3FN que exige que <em>toda</em> dependencia funcional tenga como determinante una superclave. En la práctica, llevar un diseño a <strong>3FN suele ser suficiente</strong> para la mayoría de los sistemas; mencionamos BCNF para que sepan que el camino continúa.</p>
      <div class="warn">
        <span class="label">⚠ Equilibrio</span>
        Normalizar demasiado puede fragmentar la base en muchas tablas y obligar a consultas con muchos <code class="inline">JOIN</code>. El objetivo no es "máxima normalización", sino un diseño <strong>correcto y mantenible</strong>: en general, 3FN.
      </div>
    </section>

    <!-- PRÁCTICA -->
    <section id="practica">
      <div class="section-label"><span class="dot"></span>07 · Ejercicios de práctica</div>
      <h2>Práctica</h2>

      <div class="practice">
        <span class="tag">Ejercicio 1 · Diagnóstico</span>
        <h3>Detecta las anomalías</h3>
        <p>Se te dará una tabla desnormalizada de una clínica (paciente, médico, cita, especialidad). Identifica por escrito una anomalía de inserción, una de actualización y una de eliminación, y señala qué redundancia las causa.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 2 · Normalización guiada</span>
        <h3>Lleva la tabla a 3FN</h3>
        <p>Parte de una tabla <code class="inline">VENTA(folio, fecha, id_cliente, nombre_cliente, id_producto, nombre_producto, precio, cantidad)</code>. Lista las dependencias funcionales y descompón la tabla hasta 3FN, indicando en cada paso qué regla aplicaste y por qué.</p>
      </div>

      <div class="practice">
        <span class="tag">Ejercicio 3 · Del papel a XAMPP</span>
        <h3>Construye tu diseño normalizado</h3>
        <p>Toma tu esquema en 3FN del ejercicio 2 y materialízalo en XAMPP con <code class="inline">CREATE TABLE</code> y sus <code class="inline">FOREIGN KEY</code>. Inserta datos de ejemplo que demuestren que ya no hay redundancia.</p>
      </div>
    </section>

    <!-- RESUMEN -->
    <section id="resumen">
      <div class="section-label"><span class="dot"></span>08 · Cierre</div>
      <h2>Resumen de la semana</h2>
      <div class="summary-grid">
        <div class="summary-item"><div class="icon">1️⃣</div><div class="label">1FN</div><div class="val">Valores atómicos</div></div>
        <div class="summary-item"><div class="icon">2️⃣</div><div class="label">2FN</div><div class="val">Sin dep. parciales</div></div>
        <div class="summary-item"><div class="icon">3️⃣</div><div class="label">3FN</div><div class="val">Sin dep. transitivas</div></div>
        <div class="summary-item"><div class="icon">🎯</div><div class="label">Objetivo</div><div class="val">Eliminar redundancia</div></div>
        <div class="summary-item"><div class="icon">📦</div><div class="label">Entregable</div><div class="val">Diseño 3FN + .sql</div></div>
        <div class="summary-item"><div class="icon">➡️</div><div class="label">Próxima</div><div class="val">DML: poblar datos</div></div>
      </div>
      <div class="note" style="margin-top:22px;">
        <span class="label">Lo esencial</span>
        Normalizar no es un trámite: es lo que hace que una base de datos sea confiable. 1FN, 2FN y 3FN son tres preguntas sucesivas sobre <em>de qué depende cada atributo</em>. Si cada atributo depende de la clave, de toda la clave y de nada más que la clave, su diseño está sano.
      </div>
    </section>

  </main>
</div>
</div>

<footer>
  Bases de Datos · ITIID · Universidad Politécnica de Tapachula · Periodo Mayo–Agosto 2026 · Grupo 3°A<br>
  Elaboró: Christian Jaime García Aquino · Semana 6 (08–12 Junio 2026)
</footer>

</body>
</html>
