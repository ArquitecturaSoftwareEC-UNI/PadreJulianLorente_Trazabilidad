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
   - `docs/especificaciones-casos-de-uso.md`
   - `.agents/skills/plantuml/references/sequence_diagrams.md` y `.agents/skills/plantuml/references/common_syntax_errors.md`

## 2. CONTROL ESTRICTO DE ALCANCE Y NO-REGRESIÓN
- **PROHIBIDO** modificar los artefactos cerrados previos (`docs/*.xlsx`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png`, PNGs históricos).
- **PROHIBIDO** generar Vista de Desarrollo o código fuente.
- **PROHIBIDO** ejecutar `npx`, `npm` o instalar nuevas skills; usa únicamente `.agents/skills/plantuml`.
- **PROHIBIDO CONTAMINAR CON FRAMEWORKS O INFRAESTRUCTURA FÍSICA:** No nombres librerías concretas (`NestJS`, `React`, `Axios`, `BullMQ`, `Redis`, `Postgres`). Modela procesos/hilos conceptuales y estereotipos UML (`«boundary»`, `«control»`, `«entity»`).
- **REGLA INEXCUSABLE DE SINCRONIZACIÓN 1:1 (SEMANA 4 ↔ SEMANA 5):** Todo participante `«entity»` en los Diagramas de Secuencia debe llevar el **nombre exacto** de una clase existente en `docs/uml/diagrama-clases.puml`, y todo mensaje dirigido a esa entidad debe corresponder **1:1** con una operación pública (`+`) y firma coincidente en el Diccionario de Clases (`docs/vista-logica-clases.md`). Si descubres una operación faltante durante el análisis dinámico, actualiza primero la Vista Lógica con esa operación antes de cerrar la secuencia.

## 3. ENTREGABLES EXACTOS A CONSTRUIR

### Entregable A: `docs/vista-procesos-ecb-secuencia.md`
Debe contener obligatoriamente tres secciones detalladas:
1. **Arquitectura de Procesos y Tareas Concurrentes (Kruchten / Bass et al.):**
   - Identificación explícita de **Procesos independientes** (unidades ejecutables con espacio de memoria propio) y sus **Tareas / hilos concurrentes** (ej. Proceso Scheduler/Worker ETL híbrido de 5 min + Webhooks con extracción concurrente vs. Proceso de Servicio de Consultas del Tablero y Auditoría), aplicando la **Regla de Oro de Distribución** y diferenciando comunicación síncrona (`->`) frente a asíncrona (`->>`).
2. **Tablas de Análisis de Robustez ECB (`«boundary»`, `«control»`, `«entity»`) — Procedimiento de 6 Pasos:**
   Para cada uno de los **5 Casos de Uso arquitectónicamente significativos** (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`), documenta:
   - Actores involucrados (primario y secundarios)
   - Participantes `«boundary»` (uno específico por cada actor humano y cada plataforma/disparador externo)
   - Participante(s) `«control»` (orquestando el flujo sin convertirse en *Clase Dios* ni absorber reglas de negocio propias de las entidades)
   - Participantes `«entity»` (tomados exactamente de `docs/uml/diagrama-clases.puml`)
   - **Auditoría explícita de Reglas de Robustez ECB:** Tabla verificando cero violaciones (`Actor ↔ boundary`, `boundary ↔ control`, `control ↔ control`, `control → entity`, `entity → entity`; prohibido `Actor → control/entity`, `boundary → entity`, `boundary ↔ boundary`, `entity → control/boundary`).
3. **Matriz de Trazabilidad y Sincronización 1:1 (Semana 4 ↔ Semana 5):**
   - Tabla exhaustiva listando cada mensaje enviado a cada `«entity»` en los 5 diagramas de secuencia, verificando coincidencia exacta (nombre + parámetros + retorno) contra `docs/uml/diagrama-clases.puml` y `docs/vista-logica-clases.md`.

### Entregable B: 5 Diagramas de Secuencia UML 2.5.1 (`.puml` y `.png`)
Crea un archivo `.puml` por cada Caso de Uso significativo, respetando estrictamente los participantes ECB auditados y usando **fragmentos combinados UML 2.5.1** (`alt`, `else`, `opt`, `loop`, `break`, `par`, `critical`) además de flechas síncronas (`->`), asíncronas (`->>`), retornos (`-->`) y bloques `activate`/`deactivate`:

1. **`docs/uml/secuencia-cu01-sincronizar-trazabilidad.puml`** (+ `.png`):
   - Disparado por Temporizador ETL (cada 5 min) o Webhook entrante hacia su `«boundary»`.
   - `break` ante error de credenciales/autenticación o payload inválido (flujo alterno).
   - `par` para extracción concurrente desde las **4 fuentes** (Redmine, GLPI, GitLab, SonarQube) respetando timeout `<= 5 s` por fuente (`RNF-001`).
   - `loop` de hasta `3` reintentos con *backoff exponencial* por conector.
   - `alt` manejando éxito total vs. **fallo definitivo de una plataforma tras 3 reintentos**, preservando y reutilizando el **último snapshot válido** de esa fuente con su última marca temporal y registrando **sincronización parcial** (prohibido resetear métricas a `0`).
   - Homologación en memoria, correlación tarea–commit/MR–SonarQube, detección de las 3 inconsistencias (`RF-012`, `RF-013`, `RF-016`) y cálculo de indicadores (`RF-018`, `RF-021`, `RF-023`).
   - `critical` para publicación/intercambio atómico del snapshot consolidado.
   - `opt` para evaluación de umbrales y disparo asíncrono (`->>`) del punto de extensión **`CU-02`** (Emitir alerta operativa).
2. **`docs/uml/secuencia-cu02-emitir-alerta.puml`** (+ `.png`):
   - Activado desde `CU-01` al detectarse sobrecarga (`> 8` ítems o `> 40 h`), desviación `> 20%`, estancamiento en Certificación `> 5` días hábiles o inconsistencia nueva.
   - `critical` + `alt` para verificar **deduplicación en ventana de 24 horas** por `(tipoAlerta + entidadAfectada)`: omitir registro repetido o registrar nueva alerta activa.
   - `loop` de hasta `3` reintentos hacia `«boundary»` de Mensajería (**Discord/Slack**).
   - `alt` ante fallo definitivo del webhook tras 3 reintentos: **fallback automático** hacia `«boundary»` de **Servidor SMTP** (`RF-030`) y registro en bandeja de alertas críticas si ambos canales fallan.
3. **`docs/uml/secuencia-cu04-consultar-tablero.puml`** (+ `.png`):
   - Actor *Jefe de Desarrollo / PMO* interactuando con `«boundary»` del Tablero.
   - `alt` entre lectura rápida desde caché en memoria vigente (`< 5 min`, respuesta `<= 2 s`) vs. reconstrucción desde último snapshot consolidado.
   - `opt` para mostrar advertencia visual no bloqueante cuando el último ciclo registrado corresponda a una **sincronización parcial**, detallando la última actualización por fuente.
4. **`docs/uml/secuencia-cu05-filtrar-tablero.puml`** (+ `.png`):
   - Extensión de `CU-04` activada por *Jefe de Desarrollo / PMO* con criterios (proyecto, período, desarrollador, severidad).
   - `alt` validando rango de fechas y criterios: si el rango es inválido (`fechaInicio > fechaFin`), rechazo controlado con mensaje de validación (`break` o rama de error); si es válido, recálculo/filtrado en `<= 500 ms` y manejo de rama vacía sin coincidencias con opción de restablecer filtros.
5. **`docs/uml/secuencia-cu06-auditar-causa-raiz.puml`** (+ `.png`):
   - Actor *Jefe de Desarrollo / PMO* seleccionando un proyecto, indicador o inconsistencia (`RF-012`, `RF-013`, `RF-016`) para inspeccionar su trazabilidad completa (ticket GLPI / tarea Redmine ➔ rama / commits / MR GitLab ➔ métricas / deuda SonarQube).
   - `alt` manejando registro íntegro vs. **artefacto eliminado o archivado en la plataforma origen**, exhibiendo los metadatos históricos congelados del último snapshot con etiqueta *"No disponible en origen"*.

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
- [ ] Uso explícito de fragmentos combinados (`par`, `loop`, `alt`, `opt`, `critical`, `break`) y semántica correcta síncrona (`->`) vs. asíncrona (`->>`).
- [ ] Todos los archivos `.png` fueron generados con `.agents/skills/plantuml/scripts/convert_puml.py` e inspeccionados visualmente sin errores ni solapamientos.
- [ ] Ningún archivo protegido previo fue alterado.
