# Handoff — Vista Lógica (Semana 4: Diagrama de Clases, Diccionario y Estados)

> **Responsable:** Integrante asignado a la **Vista Lógica (Modelo 4+1 — Semana 4)**.
> **Prerrequisitos del repositorio:** Ya deben estar presentes `AGENTS.md`, `docs/referencias/reglas-vista-logica-y-procesos.md`, `docs/especificaciones-casos-de-uso.md` y `docs/uml/casos-de-uso.puml`.
> **Skill activa:** Únicamente `.agents/skills/plantuml` *(NO ejecutes `npx`, `npm` ni instales nuevas skills)*.

---

## Instrucciones de Uso
1. Clona el repositorio y crea una rama de trabajo (ej. `git checkout -b feat/vista-logica`).
2. Abre tu asistente IA (**Antigravity**, **Claude Code**, **Cursor** o **Copilot**) en la raíz del proyecto.
3. Copia y pega íntegro el siguiente prompt.

---

## Prompt Autocontenido para Copiar y Pegar

~~~markdown
Actúa como Arquitecto de Software experto en Ingeniería del Dominio y UML 2.5.1, siguiendo estrictamente las reglas de la asignatura Arquitectura de Software (DSOF_4069) para el proyecto **"Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)"**.

## 1. LECTURAS OBLIGATORIAS ANTES DE DISEÑAR
Lee en este orden exacto antes de escribir cualquier artefacto:
1. `AGENTS.md` (contexto real del dominio, las 6 épicas oficiales, catálogo real `CU-01`..`CU-08`, reglas de negocio e invariantes verificadas y archivos protegidos).
2. `docs/referencias/reglas-vista-logica-y-procesos.md` (especialmente la Sección 1 — Semana 4 Vista Lógica y Sección 3 — DAS v3.0).
3. `docs/especificaciones-casos-de-uso.md` (las 5 especificaciones Cockburn `CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06` en Sección 3 y las 3 fichas complementarias reales en Sección 4.1: `CU-03: Autenticar usuario`, `CU-07: Gestionar alerta operativa` y `CU-08: Exportar reporte trimestral`).
4. `docs/uml/casos-de-uso.puml`.
5. Guías de sintaxis PlantUML en `.agents/skills/plantuml/references/class_diagrams.md`, `.agents/skills/plantuml/references/state_diagrams.md` y `.agents/skills/plantuml/references/common_syntax_errors.md`.

## 2. CONTROL ESTRICTO DE ALCANCE Y NO-REGRESIÓN
- **PROHIBIDO** modificar, editar o sobrescribir los entregables cerrados de fases anteriores (`docs/*.xlsx`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png`, ni los diagramas PNG previos en `docs/`).
- **PROHIBIDO** generar todavía la Vista de Procesos (ECB/Secuencia), la Vista de Desarrollo o código fuente.
- **PROHIBIDO** ejecutar `npx`, `npm` o instalar nuevas skills; usa únicamente `.agents/skills/plantuml`.
- **PROHIBIDO CONTAMINAR LA VISTA LÓGICA CON TECNOLOGÍA FÍSICA O UI:**
  - Cero tablas SQL, tipos de motor (`VARCHAR`, `INT8`, `JSONB`) o claves foráneas (`FK`, ej. `idDesarrollador`, `idTicket`). Usa tipos abstractos (`Texto`, `Entero`, `Decimal`, `Booleano`, `FechaHora`, `Duracion`) y relaciones UML con multiplicidades.
  - Cero frameworks, marcas comerciales de mensajería ni protocolos físicos (solo actores/conceptos agnósticos del dominio).
  - Cero componentes visuales (`PantallaTablero`, `BotonExportar`, `ModalFiltro`) ni archivos físicos como entidades.

## 3. ENTREGABLES EXACTOS A CONSTRUIR

### Entregable A: `docs/vista-logica-clases.md`
Debe incluir obligatoriamente cuatro secciones rigurosas conforme a la Semana 4:
1. **Decodificación Gramatical y Árbol de 4 Filtros Secuenciales:**
   - Sustantivos y frases nominales extraídos de `CU-01`..`CU-08` (incluyendo los conceptos de `CU-03`, `CU-07` y `CU-08` de la Sección 4.1), verbos candidatos a operaciones/relaciones, adjetivos/estados candidatos a atributos o `«enumeration»`, y cantidades convertidas a multiplicidades.
   - Tabla de **descartes inmediatos** (sinónimos, actores externos `Redmine`, `GLPI`, `GitLab`, `SonarQube`, `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo` y `Tiempo (Temporizador CRON)` en su rol externo sin historia interna propia, UI/reportes, operaciones disfrazadas de sustantivos) y evaluación paso a paso contra los **4 filtros secuenciales**: (1) Persistencia conceptual a largo plazo (considerando retención mínima de 12–24 meses bajo `RNF-001` y `RNF-017`), (2) Complejidad >1 atributo, (3) Identidad propia y ciclo de vida independiente, (4) Participación comprobada en al menos un Caso de Uso.
2. **Validación con Tarjetas CRC y Decisiones de Diseño:**
   - Tarjetas CRC (**Clase – Responsabilidad – Colaboración**) para todas las clases aprobadas, auditando **Alta Cohesión** (sin *Clases Dios*) y **Bajo Acoplamiento**.
   - Justificación explícita de **Roles por Asociación vs. Herencia Rígida** (ej. un mismo `Desarrollador` actuando simultáneamente como responsable asignado en Redmine, autor real de commits o revisor de QA cruzado en GitLab bajo `RF-017`) y descomposición de relaciones N:M con atributos propios en clases asociativas.
   - Modelado conceptual explícito de la **compuerta obligatoria LOPDP 2021** (`RNF-017`, garantizando `0 %` de PII de socios antes de consolidar y retención técnica de 24 meses), del **rastro auditable matemático inmutable** (`RNF-001`, 12 meses de fórmulas/pesos/entradas), de la **jerarquía de 4 niveles** (`Épica POA/PETI` ➔ `Historia/Spec REQ o INI.TI` ➔ `Tarea/Issue #ID` ➔ `Rama/MR/Commits tipados feat/fix/test/refactor` sobre los 6 módulos `FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude`, `Nómina`), de los **registros segregados sin trazabilidad** (`RF-021`, `RF-022`), de los **snapshots parciales por fuente** (`RNF-008`), de las **alertas operativas y su gestión** (`CU-02`, `CU-07`), del **acceso institucional unificado** (`CU-03`) y del **reporte trimestral consolidado** (`CU-08`).
3. **Diccionario de Clases Completo (5 Campos Obligatorios por Cada Clase):**
   Para **cada clase** del diagrama, documenta exactamente los 5 campos exigidos por la Guía Maestra:
   1. **Nombre y Propósito**
   2. **Atributos y Restricciones** (visibilidad `-`, tipo abstracto, obligatoriedad, valor por defecto/rango; marca con `/` cualquier atributo derivado indispensable como `/porcentajeOcupacionSemanal`, `/eficienciaDesarrollador` o `/avanceRealVerificado`)
   3. **Operaciones (Entrada / Salida)** (firmas públicas `+` completas con tipos abstractos; **advertencia:** la Vista de Procesos en Semana 5 dependerá 1:1 de estas firmas exactas)
   4. **Invariantes / Reglas de Negocio** (trazando los contratos reales: solo lectura `RNF-003`, compuerta LOPDP 2021 100 % sin PII y 24 meses `RNF-017`, idempotencia 0 % duplicados con llaves compuestas `RNF-009`, timeout externo `10 s` y 3 reintentos con preservación de último snapshot válido por fuente `RNF-008`, Fibonacci `Nivel 1 Baja = 2`, `Nivel 2 Media = 5`, `Nivel 3 Alta = 13` bajo `RF-001`, carga base `40 h` Verde `< 75 %` / Amarillo `75 %–100 %` / Rojo `> 100 %` bajo `RF-002`, suma `GLPI` `4 h/3 pts` o crítico core `8 h/5 pts` + QA cruzado `2 h/1 pt` bajo `RF-003`, respaldo de avance `> 25 %` y `100 %` bajo `RF-038`, eficiencia `70 %` avance + `30 %` calidad con `-10 pts` por retorno desde pruebas/Certificación y `-5 pts` por bug crítico o vulnerabilidad `SonarQube` excluyendo emergentes `GLPI` bajo `RF-005`, 5 etapas oficiales `Análisis / Desarrollo / Certificación / Despliegue / Estabilización` bajo `RF-019`, supresión anti-spam 24 h y estado «atendida»/«en seguimiento» bajo `RF-023`/`RF-026`)
   5. **Casos de Uso Trazados** (`CU-01`..`CU-08` y `RF-001`..`RF-040` / `RNF-001`..`RNF-018`)
4. **Matriz de Trazabilidad Escenarios vs. Clases:**
   - Matriz cruzando los 8 Casos de Uso reales (`CU-01` Sincronizar y consolidar, `CU-02` Emitir alerta operativa, `CU-03` Autenticar usuario, `CU-04` Consultar tablero, `CU-05` Filtrar indicadores, `CU-06` Auditar causa raíz, `CU-07` Gestionar alerta operativa, `CU-08` Exportar reporte trimestral) contra todas las clases y enumeraciones del dominio, verificando cero filas ni columnas vacías.

### Entregable B: `docs/uml/diagrama-clases.puml` y `docs/uml/diagrama-clases.png`
- Diagrama de Clases UML 2.5.1 en **Nivel de Especificación**, aplicando la receta de 5 pasos:
  - Clases núcleo más conectadas al centro y paquetes lógicos claros si mejora la legibilidad.
  - Atributos privados (`-`) con tipos abstractos y derivados marcados con `/`.
  - Operaciones públicas (`+`) con parámetros y tipo de retorno consistentes con el Diccionario de Clases.
  - Uso preciso de las **6 relaciones formales** (Asociación etiquetada con verbo y roles, Agregación `o--`, Composición `*--`, Generalización `--|>` solo si es permanente, Dependencia `..>` ej. `«use»` hacia enumeraciones, y Realización `..|>` si aplica interfaz de dominio).
  - Multiplicidades obligatorias en **ambos extremos** (`1`, `0..1`, `*`, `1..*`, etc.) y bloques `«enumeration»` para las 5 etapas oficiales, los 6 módulos financieros, niveles de complejidad Fibonacci, semáforo de carga, tipos de commit (`feat`, `fix`, `test`, `refactor`), estados de alerta (`Activa`, `Suprimida24h`, `Atendida`, `EnSeguimiento`, `Fallida`) y tipos de inconsistencia (`RF-021`, `RF-022`, `RF-038`, `RF-017`).

### Entregable C: `docs/uml/diagrama-estados.puml` y `docs/uml/diagrama-estados.png`
- Diagrama de Estados UML 2.5.1 derivado de los flujos básicos y alternos de `docs/especificaciones-casos-de-uso.md` para las entidades con ciclo de vida crítico:
  1. `AlertaOperativa` (evaluada/disparada bajo `RF-031` ➔ filtrada por anti-spam 24 h `RF-023` o estado previo `CU-07` ➔ despachada al `Canal de Mensajería Corporativo` `RF-024` y/o escalada/conmutada al `Servicio de Correo Corporativo` `RF-025` o marcada entrega fallida `RNF-004` ➔ gestionada como «atendida» o «en seguimiento» en `CU-07` / `RF-026`).
  2. `CicloSincronizacion` (iniciado cada 5 min ➔ extrayendo en solo lectura ➔ abortado en compuerta LOPDP `3a` o falla total `2b` ➔ parcial con último snapshot válido por fuente `2a` vs. consolidado idempotente `0 %` duplicados `RNF-009` vs. revertido `8a`).
  3. Transición entre las **5 etapas oficiales** (`Análisis`, `Desarrollo`, `Certificación`, `Despliegue`, `Estabilización`) incluyendo retorno por fallo (`RF-015`: retorno desde `Certificación` o CI/CD hacia «En curso» en `Desarrollo` descontando `-10 pts` bajo `RF-005`).

## 4. VALIDACIÓN OBLIGATORIA PLANTUML ANTES DE FINALIZAR
Ejecuta los siguientes comandos sobre cada `.puml` creado y realiza inspección visual real de los `.png`:
```bash
python .agents/skills/plantuml/scripts/check_setup.py
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/diagrama-clases.puml --format png --output-dir docs/uml/
python .agents/skills/plantuml/scripts/convert_puml.py docs/uml/diagrama-estados.puml --format png --output-dir docs/uml/
```
~~~

---

## Checklist de Cierre (Antes de hacer Commit / Pull Request)
- [ ] Todos los archivos protegidos (`.xlsx`, `casos-de-uso.puml`, `especificaciones-casos-de-uso.md`, `cadena-de-valor-togaf-adm.*`) permanecen sin cambios en `git status`.
- [ ] No existen claves foráneas (`id_...`), tipos SQL (`VARCHAR`, `INT8`) ni nombres de UI/frameworks/productos comerciales en `docs/uml/diagrama-clases.puml` ni en `docs/vista-logica-clases.md`.
- [ ] Cada clase de `diagrama-clases.puml` aparece documentada con sus 5 campos obligatorios en `docs/vista-logica-clases.md`, cubriendo las fórmulas reales (`Fibonacci 2/5/13`, `base 40 h`, `70 %/30 %`, LOPDP 2021) y participando en `CU-01`..`CU-08`.
- [ ] Ambos diagramas `.png` renderizaron sin errores de sintaxis PlantUML y fueron inspeccionados visualmente para confirmar legibilidad limpia.
