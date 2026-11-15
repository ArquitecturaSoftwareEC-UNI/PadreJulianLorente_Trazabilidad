# Handoff — Vista de Procesos (Semana 5: Análisis ECB y Diagramas de Secuencia UML)

> **Responsable:** Integrante asignado a la **Vista de Procesos (Modelo 4+1 — Semana 5)**.
> **PRECONDICIÓN BLOQUEANTE:** La **Vista Lógica (`docs/vista-logica-clases.md` y `docs/uml/diagrama-clases.puml`)** debe estar completamente terminada y aprobada en el repositorio **ANTES** de ejecutar este handoff.
> **Skill activa:** Únicamente `.agents/skills/plantuml` *(NO ejecutes `npx`, `npm` ni instales nuevas skills)*.

---

## Instrucciones de Uso
1. Sincroniza tu rama con `main` después del merge de la **Vista Lógica**.
2. Confirma que existen `docs/vista-logica-clases.md` y `docs/uml/diagrama-clases.puml`.
3. Abre tu asistente IA (**Antigravity**, **Claude Code**, **Cursor** o **Copilot**) en la raíz del repositorio y pega el siguiente prompt completo.

---

## Prompt Autocontenido para Copiar y Pegar

~~~markdown
Actúa como Arquitecto de Software experto en Comportamiento Dinámico, Concurrencia y UML 2.5.1, siguiendo estrictamente las reglas de la asignatura Arquitectura de Software (DSOF_4069) para el proyecto **"Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)"**.

## 1. VERIFICACIÓN BLOQUEANTE INICIAL Y LECTURAS OBLIGATORIAS
Antes de diseñar nada:
1. **Verifica que existan y lee completos:**
   - `docs/vista-logica-clases.md`
   - `docs/uml/diagrama-clases.puml`
   *(Si alguno de esos dos archivos aún NO existe en el repositorio, DETENTE e informa que primero debe cerrarse la Vista Lógica de Semana 4).*
2. Lee en orden:
   - `AGENTS.md`
   - `docs/referencias/reglas-vista-logica-y-procesos.md` (especialmente la Sección 2 — Semana 5 Vista de Procesos)
   - `docs/especificaciones-casos-de-uso.md` (verificando cada flujo básico, alterno y de excepción de `CU-01`, `CU-02`, `CU-04`, `CU-05` y `CU-06`)
   - `.agents/skills/plantuml/references/sequence_diagrams.md` y `.agents/skills/plantuml/references/common_syntax_errors.md`

## 2. CONTROL ESTRICTO DE ALCANCE Y NO-REGRESIÓN
- **PROHIBIDO** modificar los artefactos cerrados previos (`docs/*.xlsx`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png`, PNGs históricos).
- **PROHIBIDO** generar Vista de Desarrollo o código fuente.
- **PROHIBIDO** ejecutar `npx`, `npm` o instalar nuevas skills; usa únicamente `.agents/skills/plantuml`.
- **PROHIBIDO CONTAMINAR CON FRAMEWORKS, PRODUCTOS COMERCIALES O INFRAESTRUCTURA FÍSICA:** No nombres librerías ni tecnologías concretas (`NestJS`, `React`, `Angular`, `BullMQ`, `Redis`, `Postgres`, `SSE`, marcas comerciales de chat ni protocolos de correo). Modela procesos/hilos conceptuales y estereotipos UML (`«boundary»`, `«control»`, `«entity»`).
- **REGLA INEXCUSABLE DE SINCRONIZACIÓN 1:1 (SEMANA 4 ↔ SEMANA 5):** Todo participante `«entity»` en los Diagramas de Secuencia debe llevar el **nombre exacto** de una clase existente en `docs/uml/diagrama-clases.puml`, y todo mensaje dirigido a esa entidad debe corresponder **1:1** con una operación pública (`+`) y firma coincidente en el Diccionario de Clases (`docs/vista-logica-clases.md`). Si descubres una operación faltante durante el análisis dinámico, actualiza primero la Vista Lógica con esa operación antes de cerrar la secuencia.

## 3. ENTREGABLES EXACTOS A CONSTRUIR

### Entregable A: `docs/vista-procesos-ecb-secuencia.md`
Debe contener obligatoriamente tres secciones detalladas:
1. **Arquitectura de Procesos y Tareas Concurrentes (Kruchten / Bass et al.):**
   - Identificación explícita de **Procesos independientes** (unidades ejecutables con espacio de memoria propio) y sus **Tareas / hilos concurrentes** (por ejemplo: Proceso Orquestador ETL periódico cada 5 minutos disparado por `Tiempo (Temporizador CRON)` con tareas paralelas de extracción en solo lectura hacia `Redmine`, `GLPI`, `GitLab` y `SonarQube`; Proceso Motor de Alertas Multicanal desacoplado de `CU-01` bajo `RNF-016`; y Proceso de Lectura Consultiva para las vistas del `Jefe de Desarrollo / PMO` desacoplado del motor ETL bajo `RNF-008` y `RNF-014`), aplicando la **Regla de Oro de Distribución** y justificando comunicación síncrona (`->`) frente a asíncrona (`->>`).
2. **Tablas de Análisis de Robustez ECB (`«boundary»`, `«control»`, `«entity»`) — Procedimiento de 6 Pasos:**
   Para cada uno de los **5 Casos de Uso arquitectónicamente significativos** (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`), documenta:
   - Actores oficiales involucrados (primario y secundarios exactos según `docs/uml/casos-de-uso.puml`)
   - Participantes `«boundary»` (uno específico por cada vista/disparador del actor primario y uno por cada plataforma externa: `Redmine`, `GLPI`, `GitLab`, `SonarQube`, `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo`)
   - Participante(s) `«control»` (orquestando el flujo del caso de uso sin convertirse en *Clase Dios* ni absorber reglas intrínsecas de cálculo que pertenecen a las entidades)
   - Participantes `«entity»` (tomados **exactamente** de `docs/uml/diagrama-clases.puml`)
   - **Auditoría explícita de Reglas de Robustez ECB:** Tabla verificando cero violaciones (`Actor ↔ boundary`, `boundary ↔ control`, `control ↔ control`, `control → entity`, `entity → entity`; prohibido `Actor → control/entity`, `boundary → entity`, `boundary ↔ boundary`, `entity → control/boundary`).
3. **Matriz de Trazabilidad y Sincronización 1:1 (Semana 4 ↔ Semana 5):**
   - Tabla exhaustiva listando cada mensaje enviado a cada `«entity»` en los 5 diagramas de secuencia, verificando coincidencia exacta (nombre + parámetros + retorno) contra `docs/uml/diagrama-clases.puml` y `docs/vista-logica-clases.md`.

### Entregable B: 5 Diagramas de Secuencia UML 2.5.1 (`.puml` y `.png`)
Crea un archivo `.puml` por cada Caso de Uso significativo alineado paso a paso con su especificación Cockburn de `docs/especificaciones-casos-de-uso.md`, respetando los participantes ECB auditados y usando **fragmentos combinados UML 2.5.1** (`alt`, `else`, `opt`, `loop`, `break`, `par`, `critical`), flechas síncronas (`->`), asíncronas (`->>`), retornos (`-->`) y bloques `activate`/`deactivate`:

1. **`docs/uml/secuencia-cu01-sincronizar-trazabilidad.puml`** (+ `.png`):
   - **Actor primario:** `Tiempo (Temporizador CRON)` disparando el ciclo periódico cada **5 minutos** (`RF-029`).
   - **Actores secundarios:** `Redmine`, `GLPI`, `GitLab`, `SonarQube` mediante adaptadores desacoplados en modo **estrictamente de solo lectura** (`RF-011`, `RNF-003`, `RNF-016`).
   - **`par` + `loop` + `alt` / `break` (Pasos 2, Flujo Alterno `2a` y Excepción `2b`):** Extracción concurrente con timeout externo de **`10 segundos`** (`RNF-008`) y hasta **`3 reintentos automáticos`**. En `alt` (`2a`), si una sola fuente externa permanece inalcanzable tras 3 reintentos, **conserva y reutiliza su último snapshot válido consolidado** registrando **sincronización parcial** con su última marca temporal exitosa (**prohibido** reclasificar falsamente sus actividades como sin trazabilidad ni sin respaldo en código). En `break` (`2b`), si fallan todas las fuentes o credenciales, aborta sin sobreescribir los datos previos.
   - **Compuerta Obligatoria LOPDP 2021 (Paso 3 y Excepción `3a`, `RNF-017`):** Enmascaramiento/eliminación del **100 %** de datos personales sensibles (PII) de socios previo a cualquier análisis o persistencia (retención técnica 24 meses). Si no se verifica el 100 % de depuración (`3a`), `break` inmediato abortando el lote.
   - **Transformación, Correlación y Fórmulas (Pasos 4–7, Alternos `5a`, `5b`, `6a`, `6b`, `7a`, `7b`):**
     - Línea base de **4 niveles** (`Épica POA/PETI` ➔ `Historia/Spec REQ o INI.TI` ➔ `Tarea/Issue #ID` ➔ `Rama/MR/Commits tipados feat/fix/test/refactor`) y los **6 módulos financieros** (`FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude`, `Nómina` bajo `RF-033`, `RF-012`).
     - Cruce tolerante `GLPI #ID` (`RF-013`, `5a`), detección de retorno por fallo hacia «En curso» (`RF-015`, `5b`), segregación de **«Trabajo en código sin trazabilidad planificada»** (`RF-021`, `6a`) e **«Incidente de soporte sin trazabilidad en desarrollo»** (`RF-022`, `6b`), validación de respaldo real de avance (`> 25 %` y `100 %`, `RF-038`, `7a`) y divergencia autor/asignado + QA cruzado (`RF-017`, `7b`).
     - Cálculo de complejidad Fibonacci (`Nivel 1 Baja = 2`, `Nivel 2 Media = 5`, `Nivel 3 Alta = 13`, `RF-001`), ocupación semanal base `40 h` (`Verde < 75 %`, `Amarillo 75 %–100 %`, `Rojo > 100 %`, `RF-002`) sumando `GLPI` (`4 h/3 pts` estándar o `8 h/5 pts` crítico core) y QA cruzado (`2 h/1 pt`, `RF-003`), eficiencia (`70 %` avance + `30 %` calidad con `-10 pts` por retorno y `-5 pts` por bug crítico/vulnerabilidad `SonarQube` excluyendo emergentes `GLPI`, `RF-005`) y normalización en las **5 etapas oficiales** (`Análisis`, `Desarrollo`, `Certificación`, `Despliegue`, `Estabilización`, `RF-019`).
   - **`critical` + `break` (Paso 8 y Excepción `8a`):** Persistencia atómica e idempotente (`0 %` duplicados con llaves naturales compuestas `RNF-009` + registro auditable inmutable `RNF-001` retenido mínimo 12 meses); si hay conflicto (`8a`), reversión completa del lote sin alterar la línea vigente.
   - **`opt` de extensión:** Si se superan umbrales críticos, dispara de manera asíncrona (`->>`) **`<<extend>> CU-02: Emitir alerta operativa`**.

2. **`docs/uml/secuencia-cu02-emitir-alerta.puml`** (+ `.png`):
   - **Actor primario:** `Tiempo (Temporizador CRON)` (como extensión de `CU-01`). **Actores secundarios:** `Canal de Mensajería Corporativo` y `Servicio de Correo Corporativo`.
   - **Evaluación en `< 200 ms` y `break` `1a` (`RNF-010`, `RF-031`, `RF-025`, `RF-006`):** Evalúa sobrecarga semanal `> 40 h`, permanencia `> 5 días hábiles` en Certificación, retorno por fallo (`RF-031`), incidente Prioridad 1 o cambio de prioridad (`RF-025`) y redacta mensaje en lenguaje humano claro (`RF-006`); si falla la evaluación en `< 200 ms`, aborta registrando error técnico (`1a`).
   - **`critical` + `alt` (Paso 3 y Flujo Alterno `3a`):** Consulta la bitácora de las últimas **24 horas** y alertas gestionadas en `CU-07` (`RF-023`, `RF-026`): si es idéntica dentro de 24 h o está marcada como «atendida»/«en seguimiento» sin agravamiento, suprime el envío repetido (`3a`) y audita la supresión.
   - **Despacho cifrado (`>= TLS 1.2`, `RNF-015`), Escalamiento y Conmutación (Pasos 4–5, Alterno `4a`, Excepción `5a`):**
     - Envío al `Canal de Mensajería Corporativo` (`RF-024`).
     - `opt` (Paso 5): envío adicional por `Servicio de Correo Corporativo` cuando supere `5 días hábiles` en Certificación, sea Prioridad 1 o cambie prioridad del proyecto (`RF-025`).
     - `alt` (`4a`): si falla o rechaza el `Canal de Mensajería Corporativo`, conmutación automática obligatoria enviando la alerta íntegra por `Servicio de Correo Corporativo` (`RF-025`).
     - Persistir el **100 %** de eventos aprobados, suprimidos o con fallo simultáneo (`5a`) indicando ID único, fecha/hora, categoría, destinatarios y estado de entrega (`RNF-004`).

3. **`docs/uml/secuencia-cu04-consultar-tablero.puml`** (+ `.png`):
   - **Actor primario:** `Jefe de Desarrollo / PMO` con sesión autenticada vigente (`CU-03`) sobre `HTTPS TLS 1.3` (`RNF-013`).
   - **Lectura consolidada desacoplada (Pasos 1–4, `RF-032`, `RF-007`, `RF-035`, `RF-036`, `RNF-014`):** Recupera del almacenamiento consolidado en **`<= 2 s` (p95, hasta 25 sesiones activas y 5.000 registros)** todos los proyectos POA/PETI y desarrolladores activos, semáforos base 40 h, tarjetas, mapas de calor financieros y embudo de 5 etapas con cuellos de botella en rojo.
   - **`alt` / `break` (Flujo Alterno `2a`, Excepciones `2b` y `2c`):**
     - `2a`: Si `CU-01` reportó fallo externo parcial, entrega el **100 %** de los últimos datos consolidados válidos con la marca temporal de última sincronización exitosa (`RNF-008`).
     - `2b`: Si aún no existe ningún ciclo inicial exitoso, notifica ausencia de consolidación histórica (`break`).
     - `2c`: Si falla el servicio interno de lectura o caduca la sesión, solicita reintento o reingreso sin exponer detalles técnicos internos (`break`).
   - **`loop` / `opt` (Paso 5) y Cierre `5a` (`RF-034`, `RNF-014`, `RNF-011`):** Mientras la sesión permanezca abierta, cada 5 minutos actualiza incrementalmente en **`< 1 s`** solo los indicadores cambiados; incluye habilitación de `<<extend>> CU-05` y flujo de cierre/expiración liberando canales y memoria en **`< 200 ms`** con `0` conexiones huérfanas (`5a`, `RNF-011`).

4. **`docs/uml/secuencia-cu05-filtrar-tablero.puml`** (+ `.png`):
   - **Actor primario:** `Jefe de Desarrollo / PMO` (extensión de `CU-04` bajo `[análisis focalizado]`).
   - **Apertura `< 300 ms` (`RF-008`, `RNF-006`):** Despliega dimensiones de filtrado (desarrollador, módulo financiero, etapa oficial, rango de fechas) conservando contexto visual.
   - **`break` por fecha inválida (`3a`) y Recálculo `<= 400 ms` p95 (`RF-009`, `RNF-018`):** Valida el rango de fechas (si fecha inicial > final o formato inválido, rechaza sin alterar vista actual en `3a`); si es válido, calcula intersección exacta sobre hasta 5.000 registros en `<= 400 ms` (p95).
   - **`alt` (Flujos Alternos `4a`, `4b`, Excepción `4c`, Paso 6 y `6a`):**
     - `4a`: Cero coincidencias mostrando aviso claro y opción de limpieza sin dañar la vista (`RF-039`).
     - `4b`: Sincronización de 5 min mientras hay filtros activos actualizando incrementalmente sin perder criterios (`RF-034`).
     - `4c`: Fallo interno en cálculo preservando los últimos indicadores estables.
     - Exposición y retiro individual de etiquetas activas recalculando sin reiniciar (`RF-028`), y retorno al estado global de `CU-04` al retirar todos (`6a`).

5. **`docs/uml/secuencia-cu06-auditar-causa-raiz.puml`** (+ `.png`):
   - **Actor primario:** `Jefe de Desarrollo / PMO` con sesión autenticada vigente (`CU-03`) sobre `HTTPS TLS 1.3` (`RNF-013`) en modo estrictamente de solo lectura (`RNF-003`).
   - **Consulta causal en `< 10 s` (`RF-010`, `RF-018`, `RF-020`, `RNF-007`, `RNF-001`, `RNF-017`):** Recupera el expediente cronológico del almacenamiento consolidado y expone los 4 niveles (`Épica POA/PETI`, `Historia/Spec REQ/INI.TI`, `Tarea #ID`), `GLPI #ID`, rama/MR/commits tipados (`feat`, `fix`, `test`, `refactor`), QA cruzado, historial por las 5 etapas, retornos por fallo, alertas de `SonarQube`, valores de entrada del cálculo de eficiencia (100 % reproducible) y priorización visual por criticidad en **`< 10 s`**.
   - **`alt` y `break` (Flujos Alternos `2a`, `3a` y Excepciones `2b`, `4a`):**
     - `2a`: Auditoría de «Trabajo en código sin trazabilidad planificada» (`RF-021`) o «Incidente de soporte sin trazabilidad en desarrollo» (`RF-022`), mostrando expediente parcial y eslabón faltante claro (`sin tarea en Redmine` o `sin petición espejo GLPI #ID`).
     - `3a`: Exhibición explícita lado a lado cuando exista brecha de evidencia (`RF-038`) o divergencia entre asignado y autor de commits (`RF-017`).
     - `2b` (`break`): Expediente inexistente o depurado por superar la ventana de retención técnica (`> 24 meses`).
     - `4a` (`break`): Fallo o corrupción en el rastro auditable inmutable que impida 100 % de reproducibilidad (`RNF-001`), interrumpiendo la entrega sin mostrar datos incompletos ni sin enmascarar (`RNF-017`).

## 4. VALIDACIÓN OBLIGATORIA PLANTUML ANTES DE FINALIZAR
Valida sintaxis, exporta los 5 `.png` y revisa visualmente cada imagen resultante:
```bash
python .agents/skills/plantuml/scripts/check_setup.py
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/secuencia-cu01-sincronizar-trazabilidad.puml --format png --output-dir docs/uml/
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/secuencia-cu02-emitir-alerta.puml --format png --output-dir docs/uml/
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/secuencia-cu04-consultar-tablero.puml --format png --output-dir docs/uml/
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/secuencia-cu05-filtrar-tablero.puml --format png --output-dir docs/uml/
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/secuencia-cu06-auditar-causa-raiz.puml --format png --output-dir docs/uml/
```
~~~

---

## Checklist de Cierre (Antes de hacer Commit / Pull Request)
- [ ] Todos los participantes `«entity»` y sus operaciones invocadas coinciden **1:1** con `docs/uml/diagrama-clases.puml` y `docs/vista-logica-clases.md`.
- [ ] Cero violaciones de robustez ECB en los 5 diagramas (`Actor` nunca salta su `«boundary»`, `«boundary»` nunca toca directo una `«entity»`, `«entity»` nunca llama hacia arriba).
- [ ] Cada secuencia modela sus flujos básicos, alternos (`2a`, `3a`, `4a`, etc.) y de excepción (`2b`, `3a`, `8a`, etc.) con fragmentos combinados (`par`, `loop`, `alt`, `opt`, `critical`, `break`) y umbrales reales (`10 s`, `< 200 ms`, `<= 2 s` p95, `< 1 s`, `< 300 ms`, `<= 400 ms` p95, `< 10 s`).
- [ ] Todos los archivos `.png` fueron generados con `.agents/skills/plantuml/scripts/convert_puml.py` e inspeccionados visualmente sin errores ni solapamientos.
- [ ] Ningún archivo protegido previo fue alterado.
