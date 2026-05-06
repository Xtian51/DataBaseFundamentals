# DataBaseFundamentals

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bases de Datos — Semana 1: Fundamentos</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg:        #0d1117;
    --surface:   #161b22;
    --surface2:  #21262d;
    --border:    #30363d;
    --accent:    #2563eb;
    --accent2:   #0ea5e9;
    --green:     #22c55e;
    --yellow:    #eab308;
    --red:       #ef4444;
    --purple:    #a855f7;
    --text:      #e6edf3;
    --muted:     #8b949e;
    --code-bg:   #161b22;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter', sans-serif;
    font-size: 15px;
    line-height: 1.7;
  }

  /* ── Header ─────────────────────────────── */
  header {
    background: linear-gradient(135deg, #0f172a 0%, #1e3a5f 50%, #0f172a 100%);
    border-bottom: 1px solid var(--border);
    padding: 48px 0 36px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse 60% 60% at 50% 0%, rgba(37,99,235,.18), transparent);
    pointer-events: none;
  }
  .badge {
    display: inline-block;
    background: rgba(37,99,235,.15);
    border: 1px solid rgba(37,99,235,.4);
    color: var(--accent2);
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: .1em;
    padding: 4px 14px;
    border-radius: 20px;
    margin-bottom: 16px;
  }
  header h1 {
    font-family: 'Syne', sans-serif;
    font-size: clamp(26px, 4vw, 42px);
    font-weight: 800;
    letter-spacing: -.02em;
    color: #fff;
  }
  header h1 span { color: var(--accent2); }
  header p {
    color: var(--muted);
    margin-top: 10px;
    font-size: 14px;
  }
  .pills {
    display: flex; gap: 10px; justify-content: center;
    flex-wrap: wrap; margin-top: 20px;
  }
  .pill {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 4px 14px;
    font-size: 12px;
    color: var(--muted);
  }

  /* ── Layout ──────────────────────────────── */
  .container { max-width: 960px; margin: 0 auto; padding: 0 24px; }

  /* ── Nav lateral ─────────────────────────── */
  .toc {
    position: sticky; top: 20px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px;
    font-size: 13px;
  }
  .toc h4 {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    letter-spacing: .12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 12px;
  }
  .toc a {
    display: block;
    color: var(--muted);
    text-decoration: none;
    padding: 4px 0;
    border-left: 2px solid transparent;
    padding-left: 10px;
    transition: all .2s;
  }
  .toc a:hover { color: var(--accent2); border-color: var(--accent2); }

  /* ── Main layout ─────────────────────────── */
  .layout {
    display: grid;
    grid-template-columns: 200px 1fr;
    gap: 32px;
    padding: 40px 0 60px;
    align-items: start;
  }
  @media(max-width: 760px) { .layout { grid-template-columns: 1fr; } .toc { display: none; } }

  /* ── Sections ────────────────────────────── */
  section { margin-bottom: 48px; scroll-margin-top: 24px; }

  .section-label {
    display: inline-flex; align-items: center; gap: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    letter-spacing: .1em;
    text-transform: uppercase;
    color: var(--accent2);
    margin-bottom: 8px;
  }
  .section-label .dot {
    width: 6px; height: 6px;
    background: var(--accent2);
    border-radius: 50%;
  }

  h2 {
    font-family: 'Syne', sans-serif;
    font-size: 22px;
    font-weight: 800;
    color: #fff;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid var(--border);
  }
  h3 {
    font-family: 'Syne', sans-serif;
    font-size: 16px;
    font-weight: 600;
    color: var(--accent2);
    margin: 28px 0 12px;
  }

  p { color: #cdd5e0; margin-bottom: 14px; }

  /* ── Info cards ──────────────────────────── */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px 24px;
    margin-bottom: 20px;
  }
  .card.accent  { border-left: 3px solid var(--accent); }
  .card.green   { border-left: 3px solid var(--green); }
  .card.yellow  { border-left: 3px solid var(--yellow); }
  .card.purple  { border-left: 3px solid var(--purple); }
  .card h4 {
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 600;
    margin-bottom: 8px;
    color: #fff;
  }
  .card p { margin: 0; font-size: 14px; }

  /* ── Definición destacada ────────────────── */
  .definition {
    background: linear-gradient(135deg, rgba(37,99,235,.08), rgba(14,165,233,.06));
    border: 1px solid rgba(37,99,235,.3);
    border-radius: 10px;
    padding: 20px 24px;
    margin: 20px 0;
  }
  .definition .label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 10px;
    letter-spacing: .12em;
    color: var(--accent2);
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .definition p { margin: 0; font-size: 15px; color: var(--text); }

  /* ── Listas ──────────────────────────────── */
  ul, ol { padding-left: 22px; margin-bottom: 14px; }
  li { color: #cdd5e0; margin-bottom: 6px; }
  li strong { color: var(--text); }

  /* ── Tabla ───────────────────────────────── */
  .table-wrap { overflow-x: auto; margin: 20px 0; }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
  }
  th {
    background: var(--surface2);
    border: 1px solid var(--border);
    padding: 10px 14px;
    text-align: left;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--accent2);
    letter-spacing: .06em;
  }
  td {
    border: 1px solid var(--border);
    padding: 10px 14px;
    color: #cdd5e0;
  }
  tr:nth-child(even) td { background: rgba(255,255,255,.02); }

  /* ── Code ────────────────────────────────── */
  .code-block {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 10px;
    margin: 20px 0;
    overflow: hidden;
  }
  .code-header {
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
    padding: 8px 16px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .code-header span {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--muted);
  }
  .code-dots { display: flex; gap: 6px; }
  .code-dots i {
    width: 10px; height: 10px;
    border-radius: 50%;
    display: inline-block;
  }
  .code-dots i:nth-child(1) { background: #ef4444; }
  .code-dots i:nth-child(2) { background: #eab308; }
  .code-dots i:nth-child(3) { background: #22c55e; }
  pre {
    padding: 18px 20px;
    overflow-x: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.6;
    color: #e6edf3;
    background: transparent;
  }
  .kw  { color: #79c0ff; }   /* keywords */
  .str { color: #a5d6ff; }   /* strings  */
  .cm  { color: #8b949e; font-style: italic; } /* comments */
  .fn  { color: #d2a8ff; }   /* functions */
  .num { color: #ffa657; }   /* numbers  */
  .op  { color: #ff7b72; }   /* operators */

  /* ── Ejercicios ──────────────────────────── */
  .ejercicio {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 22px 24px;
    margin-bottom: 20px;
    position: relative;
  }
  .ejercicio::before {
    content: attr(data-num);
    position: absolute; top: -12px; left: 20px;
    background: var(--accent);
    color: #fff;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    font-weight: 600;
    padding: 2px 12px;
    border-radius: 20px;
  }
  .ejercicio.practica::before { background: var(--green); }
  .ejercicio h4 {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 600;
    margin-bottom: 10px;
    color: #fff;
  }

  /* ── Diagrama ASCII ──────────────────────── */
  .diagram {
    background: #0d1117;
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--accent2);
    overflow-x: auto;
    margin: 20px 0;
    white-space: pre;
    line-height: 1.5;
  }

  /* ── Resumen final ───────────────────────── */
  .summary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 14px;
    margin-top: 20px;
  }
  .summary-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px 16px;
    text-align: center;
  }
  .summary-item .icon { font-size: 24px; margin-bottom: 6px; }
  .summary-item .label { font-size: 12px; color: var(--muted); }
  .summary-item .val {
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 600;
    color: var(--text);
  }
</style>
</head>
<body>

<!-- ═══════════════ HEADER ═══════════════ -->
<header>
  <div class="badge">ITIID · Bases de Datos · UPTap</div>
  <h1>Semana 1 — <span>Fundamentos de Bases de Datos</span></h1>
  <p>Unidad I · Conceptos básicos y componentes del sistema de bases de datos</p>
  <div class="pills">
    <span class="pill">📅 06 - 08 MAY</span>
    <span class="pill">⏱ 5 horas teoría</span>
    <span class="pill">🎯 Diagnóstico + conceptos base</span>
  </div>
</header>

<!-- ═══════════════ LAYOUT ═══════════════ -->
<div class="container">
<div class="layout">

  <!-- TOC -->
  <nav class="toc">
    <h4>Contenido</h4>
    <a href="#teoria">📖 Teoría</a>
    <a href="#sgbd">🗄 SGBD</a>
    <a href="#componentes">🔩 Componentes</a>
    <a href="#modelos">📐 Modelos</a>
    <a href="#ejemplos">💡 Ejemplos</a>
    <a href="#practica">🏋 Práctica</a>
    <a href="#resumen">✅ Resumen</a>
  </nav>

  <!-- MAIN -->
  <main>

    <!-- ═══ TEORÍA ═══ -->
    <section id="teoria">
      <div class="section-label"><span class="dot"></span>01 · Fundamentos</div>
      <h2>¿Qué es una Base de Datos?</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>Una <strong>base de datos</strong> es una colección organizada de datos estructurados, almacenados y accedidos electrónicamente desde un sistema informático. Los datos están relacionados entre sí y se gestionan para eliminar redundancia e inconsistencia.</p>
      </div>

      <p>Antes de que existieran las bases de datos, la información se almacenaba en <strong>archivos planos</strong> (archivos de texto o binarios). Este enfoque presentaba serios problemas:</p>

      <div class="card accent">
        <h4>⚠️ Problemas del sistema de archivos tradicional</h4>
        <ul style="margin:8px 0 0 0; padding-left:18px;">
          <li><strong>Redundancia de datos:</strong> el mismo dato se guardaba en múltiples archivos.</li>
          <li><strong>Inconsistencia:</strong> actualizar un archivo no garantizaba actualizar los demás.</li>
          <li><strong>Aislamiento de datos:</strong> cada programa tenía su propio formato, difícil de compartir.</li>
          <li><strong>Falta de seguridad:</strong> cualquier usuario podía acceder a cualquier archivo.</li>
          <li><strong>Sin control de concurrencia:</strong> dos usuarios modificando el mismo archivo causaban corrupción.</li>
        </ul>
      </div>

      <h3>Características de una base de datos</h3>
      <ul>
        <li><strong>Persistencia:</strong> los datos se conservan después de cerrar el programa.</li>
        <li><strong>Compartibilidad:</strong> múltiples usuarios y aplicaciones acceden simultáneamente.</li>
        <li><strong>Integridad:</strong> existen reglas que garantizan la validez de los datos.</li>
        <li><strong>Independencia de datos:</strong> la estructura puede cambiar sin afectar las aplicaciones.</li>
        <li><strong>Seguridad:</strong> control de quién puede leer, insertar, modificar o eliminar datos.</li>
      </ul>

      <h3>Usos en el mundo real</h3>
      <div class="card green">
        <h4>🌐 Ejemplos cotidianos de bases de datos</h4>
        <ul style="margin:8px 0 0 0; padding-left:18px;">
          <li>Sistema de una tienda en línea (productos, clientes, pedidos).</li>
          <li>Expediente médico electrónico de un hospital.</li>
          <li>Sistema de control escolar de una universidad.</li>
          <li>Aplicación bancaria (cuentas, movimientos, clientes).</li>
          <li>Redes sociales (usuarios, publicaciones, relaciones).</li>
        </ul>
      </div>
    </section>

    <!-- ═══ SGBD ═══ -->
    <section id="sgbd">
      <div class="section-label"><span class="dot"></span>02 · Sistema Gestor</div>
      <h2>Sistema Gestor de Base de Datos (SGBD)</h2>

      <div class="definition">
        <div class="label">Definición formal</div>
        <p>Un <strong>SGBD</strong> (o DBMS por sus siglas en inglés) es el software que sirve de interfaz entre la base de datos, el usuario y las aplicaciones. Gestiona cómo se almacenan, recuperan y actualizan los datos.</p>
      </div>

      <h3>Funciones principales del SGBD</h3>
      <div class="table-wrap">
        <table>
          <tr>
            <th>Función</th>
            <th>Descripción</th>
            <th>Ejemplo</th>
          </tr>
          <tr><td>Definición de datos</td><td>Permite crear y modificar la estructura de la BD</td><td>CREATE TABLE, ALTER TABLE</td></tr>
          <tr><td>Manipulación de datos</td><td>Permite consultar, insertar, actualizar y eliminar</td><td>SELECT, INSERT, UPDATE, DELETE</td></tr>
          <tr><td>Control de acceso</td><td>Gestiona usuarios y permisos</td><td>GRANT, REVOKE</td></tr>
          <tr><td>Integridad</td><td>Aplica restricciones y reglas de negocio</td><td>PRIMARY KEY, FOREIGN KEY, CHECK</td></tr>
          <tr><td>Control de concurrencia</td><td>Gestiona accesos simultáneos</td><td>Transacciones (COMMIT, ROLLBACK)</td></tr>
          <tr><td>Recuperación ante fallos</td><td>Restaura la BD tras un error o caída</td><td>Backups, logs de transacciones</td></tr>
        </table>
      </div>

      <h3>SGBD más utilizados en la industria</h3>
      <div class="table-wrap">
        <table>
          <tr>
            <th>SGBD</th>
            <th>Tipo</th>
            <th>Lenguaje</th>
            <th>Uso típico</th>
          </tr>
          <tr><td>MySQL</td><td>Relacional</td><td>SQL</td><td>Aplicaciones web (LAMP stack)</td></tr>
          <tr><td>PostgreSQL</td><td>Relacional</td><td>SQL</td><td>Proyectos complejos, JSON nativo</td></tr>
          <tr><td>SQL Server</td><td>Relacional</td><td>T-SQL</td><td>Entorno corporativo Microsoft</td></tr>
          <tr><td>SQLite</td><td>Relacional</td><td>SQL</td><td>Apps móviles, desarrollo local</td></tr>
          <tr><td>MongoDB</td><td>No relacional (docs)</td><td>MQL</td><td>Datos flexibles, JSON, BigData</td></tr>
          <tr><td>Redis</td><td>No relacional (clave-valor)</td><td>Comandos propios</td><td>Cache, sesiones, tiempo real</td></tr>
        </table>
      </div>
    </section>

    <!-- ═══ COMPONENTES ═══ -->
    <section id="componentes">
      <div class="section-label"><span class="dot"></span>03 · Componentes</div>
      <h2>Componentes de un Sistema de Bases de Datos</h2>

      <p>Un sistema de bases de datos completo está formado por cuatro grandes componentes que trabajan en conjunto:</p>

      <div class="diagram">
┌─────────────────────────────────────────────────────────┐
│               SISTEMA DE BASE DE DATOS                  │
│                                                         │
│   ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│   │ USUARIOS │───▶│  SGBD    │───▶│   BASE DE DATOS  │  │
│   │          │    │          │    │   (Datos + Meta) │  │
│   │ - DBA    │◀───│ - DDL    │◀───│                  │  │
│   │ - Prog.  │    │ - DML    │    │  ┌────────────┐  │  │
│   │ - Naïf   │    │ - DCL    │    │  │  Esquema   │  │  │
│   └──────────┘    └──────────┘    │  │  (estructura)│  │
│                        │          │  ├────────────┤  │  │
│   ┌──────────────────┐ │          │  │  Instancia │  │  │
│   │   APLICACIONES   │◀┘          │  │  (datos)   │  │  │
│   │  (web, móvil…)   │            │  └────────────┘  │  │
│   └──────────────────┘            └──────────────────┘  │
└─────────────────────────────────────────────────────────┘</div>

      <div style="display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-top:6px;">
        <div class="card accent">
          <h4>1️⃣ Hardware</h4>
          <p>Dispositivos físicos: servidores, discos, memoria RAM, red. Donde residen físicamente los datos.</p>
        </div>
        <div class="card purple">
          <h4>2️⃣ Software (SGBD)</h4>
          <p>El motor de base de datos que procesa consultas, controla acceso y garantiza integridad.</p>
        </div>
        <div class="card green">
          <h4>3️⃣ Datos</h4>
          <p>Los datos almacenados (instancia) y la descripción de su estructura (esquema o metadatos).</p>
        </div>
        <div class="card yellow">
          <h4>4️⃣ Usuarios</h4>
          <p>DBA (administrador), programadores de aplicaciones, usuarios finales y usuarios casuales.</p>
        </div>
      </div>

      <h3>Esquema vs. Instancia</h3>
      <div class="table-wrap">
        <table>
          <tr><th>Concepto</th><th>Descripción</th><th>Analogía</th></tr>
          <tr>
            <td><strong>Esquema</strong></td>
            <td>Estructura lógica de la BD — qué tablas existen, qué columnas tienen, qué tipos de datos.</td>
            <td>El plano de una casa (no cambia seguido)</td>
          </tr>
          <tr>
            <td><strong>Instancia</strong></td>
            <td>Los datos reales almacenados en un momento dado. Cambia con cada INSERT/UPDATE/DELETE.</td>
            <td>Los muebles dentro de la casa (cambian constantemente)</td>
          </tr>
        </table>
      </div>
    </section>

    <!-- ═══ MODELOS ═══ -->
    <section id="modelos">
      <div class="section-label"><span class="dot"></span>04 · Modelos de datos</div>
      <h2>Modelos de Datos</h2>

      <p>Un <strong>modelo de datos</strong> es una colección de herramientas conceptuales para describir datos, sus relaciones, su semántica y sus restricciones. Los principales son:</p>

      <div class="card accent">
        <h4>📐 Modelo Relacional (el más utilizado)</h4>
        <p>Organiza los datos en <strong>tablas</strong> (relaciones) formadas por filas (tuplas) y columnas (atributos). Cada tabla representa una entidad del mundo real. Las relaciones entre tablas se establecen mediante claves.</p>
        <br>
        <p><strong>Base matemática:</strong> Álgebra relacional (Codd, 1970) — garantiza operaciones formalmente correctas.</p>
      </div>

      <div class="card purple">
        <h4>📄 Modelo No Relacional (NoSQL)</h4>
        <p>Almacena datos en formatos alternativos: documentos JSON, grafos, clave-valor, o columnas anchas. Útil cuando la estructura es flexible o el volumen de datos es masivo.</p>
      </div>

      <div class="card green">
        <h4>🔷 Modelo Entidad-Relación (conceptual)</h4>
        <p>Herramienta de diseño que usa <strong>entidades</strong>, <strong>atributos</strong> y <strong>relaciones</strong> para modelar el mundo real antes de implementar la BD. Es independiente del SGBD.</p>
      </div>

      <h3>Los tres niveles de abstracción</h3>
      <div class="diagram">
  ┌─────────────────────────────────────────┐
  │         NIVEL EXTERNO (Vistas)          │
  │  Vista Usuario A   │   Vista Usuario B  │
  │  (solo sus datos)  │   (otros datos)    │
  └──────────────────┬──────────────────────┘
                     │   mapeo
  ┌──────────────────▼──────────────────────┐
  │       NIVEL CONCEPTUAL (Esquema)        │
  │  Estructura global de la BD:            │
  │  Tablas, relaciones, restricciones      │
  └──────────────────┬──────────────────────┘
                     │   mapeo
  ┌──────────────────▼──────────────────────┐
  │        NIVEL INTERNO (Físico)           │
  │  Archivos, índices, bloques en disco    │
  └─────────────────────────────────────────┘</div>
    </section>

    <!-- ═══ EJEMPLOS ═══ -->
    <section id="ejemplos">
      <div class="section-label"><span class="dot"></span>05 · Ejemplos</div>
      <h2>Ejemplos Demostrativos</h2>

      <!-- Ejemplo 1 -->
      <div class="ejercicio" data-num="Ejemplo 1">
        <h4>Identificar componentes de un sistema de BD real</h4>
        <p>Analicemos el sistema de control escolar de la UPTap e identifiquemos cada componente:</p>
        <div class="table-wrap">
          <table>
            <tr><th>Componente</th><th>En el sistema escolar</th></tr>
            <tr><td>Hardware</td><td>Servidor del departamento de sistemas, PCs en ventanilla</td></tr>
            <tr><td>SGBD</td><td>MySQL (ejemplo típico en instituciones educativas)</td></tr>
            <tr><td>Esquema</td><td>Tablas: ALUMNO, MATERIA, INSCRIPCION, DOCENTE, GRUPO</td></tr>
            <tr><td>Instancia</td><td>Los 500 alumnos registrados este cuatrimestre</td></tr>
            <tr><td>DBA</td><td>El técnico de sistemas que administra la BD</td></tr>
            <tr><td>Usuarios finales</td><td>Alumnos consultando calificaciones, secretaria capturando</td></tr>
          </table>
        </div>
      </div>

      <!-- Ejemplo 2 -->
      <div class="ejercicio" data-num="Ejemplo 2">
        <h4>Esquema vs. Instancia — Tabla ALUMNO</h4>
        <p>El <strong>esquema</strong> define la estructura; la <strong>instancia</strong> son los datos reales:</p>

        <p><strong>Esquema (definición):</strong></p>
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><i></i><i></i><i></i></div>
            <span>SQL — Definición de esquema (DDL)</span>
          </div>
          <pre><span class="cm">-- Esto es el ESQUEMA: define la estructura</span>
<span class="kw">CREATE TABLE</span> ALUMNO (
    matricula    <span class="kw">VARCHAR</span>(<span class="num">10</span>)   <span class="kw">PRIMARY KEY</span>,
    nombre       <span class="kw">VARCHAR</span>(<span class="num">100</span>)  <span class="kw">NOT NULL</span>,
    apellidos    <span class="kw">VARCHAR</span>(<span class="num">150</span>)  <span class="kw">NOT NULL</span>,
    email        <span class="kw">VARCHAR</span>(<span class="num">200</span>)  <span class="kw">UNIQUE</span>,
    cuatrimestre <span class="kw">INT</span>           <span class="kw">CHECK</span>(cuatrimestre <span class="kw">BETWEEN</span> <span class="num">1</span> <span class="kw">AND</span> <span class="num">12</span>),
    fecha_ingreso <span class="kw">DATE</span>         <span class="kw">NOT NULL</span>
);</pre>
        </div>

        <p><strong>Instancia (datos en un momento dado):</strong></p>
        <div class="table-wrap">
          <table>
            <tr><th>matricula</th><th>nombre</th><th>apellidos</th><th>email</th><th>cuatrimestre</th><th>fecha_ingreso</th></tr>
            <tr><td>UPT-2025-001</td><td>Ana</td><td>García López</td><td>ana@uptap.edu.mx</td><td>3</td><td>2023-09-01</td></tr>
            <tr><td>UPT-2025-002</td><td>Luis</td><td>Hernández Ruiz</td><td>luis@uptap.edu.mx</td><td>3</td><td>2023-09-01</td></tr>
            <tr><td>UPT-2025-003</td><td>María</td><td>Pérez Torres</td><td>maria@uptap.edu.mx</td><td>3</td><td>2023-09-01</td></tr>
          </table>
        </div>
        <p style="margin-top:10px; font-size:13px; color:var(--muted);">💡 El esquema define <em>qué</em> se puede guardar. La instancia es <em>lo que hay guardado</em> ahora.</p>
      </div>

      <!-- Ejemplo 3 -->
      <div class="ejercicio" data-num="Ejemplo 3">
        <h4>Comparación: Archivos planos vs. Base de datos</h4>
        <p>Supongamos que una pequeña empresa maneja clientes y facturas. Veamos ambos enfoques:</p>

        <p><strong>❌ Enfoque con archivos planos (problemático):</strong></p>
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><i></i><i></i><i></i></div>
            <span>clientes.txt y facturas.txt — redundancia e inconsistencia</span>
          </div>
          <pre><span class="cm">-- clientes.txt</span>
C001, Juan Pérez, 9611234567, juan@mail.com, Tapachula
C002, Ana López, 9619876543, ana@mail.com,  Tuxtla

<span class="cm">-- facturas.txt  (el nombre del cliente se repite = redundancia)</span>
F001, C001, Juan Pérez, 15000.00, 2025-01-05
F002, C001, Juan Peres, 8200.00,  2025-01-08  <span class="cm">← error de captura!</span>
F003, C002, Ana López,  22500.00, 2025-01-10</pre>
        </div>

        <p><strong>✅ Enfoque con base de datos relacional:</strong></p>
        <div class="code-block">
          <div class="code-header">
            <div class="code-dots"><i></i><i></i><i></i></div>
            <span>SQL — Sin redundancia, con integridad referencial</span>
          </div>
          <pre><span class="cm">-- Tabla CLIENTE (datos del cliente una sola vez)</span>
<span class="kw">CREATE TABLE</span> CLIENTE (
    id_cliente <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    nombre     <span class="kw">VARCHAR</span>(<span class="num">100</span>) <span class="kw">NOT NULL</span>,
    telefono   <span class="kw">VARCHAR</span>(<span class="num">15</span>),
    email      <span class="kw">VARCHAR</span>(<span class="num">200</span>)
);

<span class="cm">-- Tabla FACTURA (referencia al cliente por su ID)</span>
<span class="kw">CREATE TABLE</span> FACTURA (
    id_factura  <span class="kw">INT PRIMARY KEY AUTO_INCREMENT</span>,
    id_cliente  <span class="kw">INT NOT NULL</span>,
    total       <span class="kw">DECIMAL</span>(<span class="num">10</span>,<span class="num">2</span>) <span class="kw">NOT NULL</span>,
    fecha       <span class="kw">DATE NOT NULL</span>,
    <span class="kw">FOREIGN KEY</span> (id_cliente) <span class="kw">REFERENCES</span> CLIENTE(id_cliente)
);

<span class="cm">-- Ahora no hay redundancia: el nombre está UNA sola vez en CLIENTE</span>
<span class="cm">-- La FOREIGN KEY garantiza que no hay facturas de clientes inexistentes</span></pre>
        </div>
      </div>
    </section>

    <!-- ═══ PRÁCTICA ═══ -->
    <section id="practica">
      <div class="section-label"><span class="dot"></span>06 · Práctica</div>
      <h2>Ejercicios de Práctica</h2>
      <p>Resuelve los siguientes ejercicios de forma individual. Documenta tus respuestas en un reporte.</p>

      <!-- Práctica 1 -->
      <div class="ejercicio practica" data-num="Práctica 1">
        <h4>Análisis de un sistema real — Caso: Tienda de abarrotes</h4>
        <p>Una pequeña tienda de abarrotes lleva sus ventas en cuadernos y hojas de Excel separadas. Identifica y documenta lo siguiente:</p>
        <ol style="margin-top:10px; padding-left:20px;">
          <li>Lista al menos <strong>5 tipos de datos</strong> que manejaría una BD para la tienda (ej: producto, precio, proveedor…).</li>
          <li>Señala <strong>3 problemas concretos</strong> que ocurren al usar solo hojas de Excel sin una BD.</li>
          <li>Propón los <strong>4 componentes del sistema de BD</strong> (hardware, software, datos, usuarios) para esta tienda. Sé específico: ¿qué SGBD usarías? ¿quién sería el DBA?</li>
          <li>Distingue entre el <strong>esquema</strong> y una <strong>instancia</strong> posible para la tabla de Productos. Escribe la definición del esquema (puede ser pseudocódigo SQL si no lo recuerdas) y 3 filas de ejemplo como instancia.</li>
        </ol>
      </div>

      <!-- Práctica 2 -->
      <div class="ejercicio practica" data-num="Práctica 2">
        <h4>Diseño conceptual inicial — Sistema de biblioteca</h4>
        <p>Una biblioteca municipal quiere implementar una base de datos para gestionar libros, socios y préstamos. Con los conocimientos de esta semana:</p>
        <ol style="margin-top:10px; padding-left:20px;">
          <li>Identifica al menos <strong>4 entidades</strong> del mundo real que formarían tablas en la BD (ej: Libro, Socio…).</li>
          <li>Para cada entidad, lista al menos <strong>3 atributos</strong> relevantes.</li>
          <li>Describe con tus palabras <strong>2 relaciones</strong> entre entidades (ej: un socio puede pedir prestados varios libros).</li>
          <li>Indica qué <strong>SGBD</strong> elegirías para este caso y justifica brevemente tu elección.</li>
        </ol>
      </div>

      <!-- Práctica 3 -->
      <div class="ejercicio practica" data-num="Práctica 3">
        <h4>Investigación breve — Comparativa de SGBD</h4>
        <p>Investiga y elabora una tabla comparativa de los siguientes SGBD en los aspectos indicados:</p>
        <div class="table-wrap" style="margin-top:10px;">
          <table>
            <tr>
              <th>Aspecto</th>
              <th>MySQL</th>
              <th>PostgreSQL</th>
              <th>MongoDB</th>
              <th>SQLite</th>
            </tr>
            <tr><td>Tipo (relacional / NoSQL)</td><td>?</td><td>?</td><td>?</td><td>?</td></tr>
            <tr><td>Licencia (libre / comercial)</td><td>?</td><td>?</td><td>?</td><td>?</td></tr>
            <tr><td>Lenguaje de consulta</td><td>?</td><td>?</td><td>?</td><td>?</td></tr>
            <tr><td>Caso de uso principal</td><td>?</td><td>?</td><td>?</td><td>?</td></tr>
            <tr><td>¿Requiere servidor?</td><td>?</td><td>?</td><td>?</td><td>?</td></tr>
          </table>
        </div>
        <p style="margin-top:12px; font-size:13px; color:var(--muted);">Fuentes sugeridas: documentación oficial de cada SGBD, db-engines.com</p>
      </div>
    </section>

    <!-- ═══ RESUMEN ═══ -->
    <section id="resumen">
      <div class="section-label"><span class="dot"></span>07 · Resumen</div>
      <h2>Puntos Clave de la Semana 1</h2>

      <div class="summary-grid">
        <div class="summary-item">
          <div class="icon">🗃️</div>
          <div class="val">Base de Datos</div>
          <div class="label">Colección organizada de datos persistentes</div>
        </div>
        <div class="summary-item">
          <div class="icon">⚙️</div>
          <div class="val">SGBD</div>
          <div class="label">Software que gestiona la BD (MySQL, PostgreSQL…)</div>
        </div>
        <div class="summary-item">
          <div class="icon">📐</div>
          <div class="val">Esquema</div>
          <div class="label">Estructura/definición (no cambia seguido)</div>
        </div>
        <div class="summary-item">
          <div class="icon">📊</div>
          <div class="val">Instancia</div>
          <div class="label">Datos reales en un momento dado</div>
        </div>
        <div class="summary-item">
          <div class="icon">🔩</div>
          <div class="val">4 Componentes</div>
          <div class="label">Hardware, SGBD, Datos, Usuarios</div>
        </div>
        <div class="summary-item">
          <div class="icon">🏗️</div>
          <div class="val">3 Niveles</div>
          <div class="label">Externo · Conceptual · Interno</div>
        </div>
      </div>

      <div class="card green" style="margin-top:24px;">
        <h4>📚 Para la próxima semana</h4>
        <p>Unidad II — Modelado de bases de datos: <strong>Modelo Entidad-Relación</strong>. Traer instalado <strong>draw.io</strong> (app.diagrams.net) o <strong>Lucidchart</strong> para comenzar a diagramar.</p>
      </div>
    </section>

  </main>
</div>
</div>
</body>
</html>
