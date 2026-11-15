# AGENTS.md — Guía Maestra de Arquitectura para Asistentes IA

> Este archivo define el contexto oficial, la línea base cerrada, las reglas metodológicas y las restricciones de no-regresión para cualquier asistente de IA (**Antigravity**, **Claude Code**, **Cursor**, **GitHub Copilot**) que trabaje en el repositorio **"Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)"**.

---

## 1. Contexto del Proyecto y Resumen del Dominio

- **Organización:** Cooperativa de Ahorro y Crédito (COAC) **Padre Julián Lorente** (Loja, Ecuador) — Departamento de Desarrollo / TI y PMO.
- **Asignatura y Marco Normativo:** Arquitectura de Software (`DSOF_4069`), bajo los estándares **UML 2.5.1**, **ISO/IEC/IEEE 42010** (Modelo Arquitectónico 4+1 de Kruchten), **ISO/IEC 25010:2023** (Atributos de Calidad y Escenarios de 6 partes) y **TOGAF ADM**.
- **Problema que resuelve el sistema:** La información operativa de proyectos de software se encuentra fragmentada en 4 plataformas heterogéneas sin correlación automática. El sistema implementa un proceso **ETL híbrido** (ejecución programada cada 5 minutos + recepción de Webhooks) que extrae, homologa en memoria y correlaciona artefactos de:
  1. **Redmine** (gestión de proyectos, requerimientos/tareas, horas estimadas, prioridades y estados)
  2. **GLPI** (tickets de incidentes/soporte operativo vinculados a proyectos o módulos)
  3. **GitLab** (ramas, commits, Merge Requests y pipelines)
  4. **SonarQube** (análisis de calidad, cobertura de pruebas, bugs, vulnerabilidades y deuda técnica)
- **Salidas de valor del sistema:**
  - Cálculo consolidado de indicadores de **carga operativa por desarrollador**, **avance real vs. planificado**, **calidad técnica** e **inconsistencias de trazabilidad**.
  - Emisión de **alertas proactivas con deduplicación** hacia **Mensajería (Discord/Slack)** con respaldo automático (**fallback**) por **Servidor SMTP**.
  - Exposición en un **Tablero de Control Ejecutivo** y flujo de **Auditoría de Causa Raíz** para el **Jefe de Desarrollo / PMO**.

---

## 2. Línea Base Oficial Cerrada (Métricas, Actores y Casos de Uso Inmutables)

### 2.1 Cobertura Cuantitativa Verificada
- **Requisitos Funcionales:** `40 RF` (`RF-001` a `RF-040`) estructurados en **6 Épicas**:
  - `EP-01`: Acceso y Extracción ETL
  - `EP-02`: Transformación y Trazabilidad
  - `EP-03`: Carga Operativa y Avance
  - `EP-04`: Alertas Operativas
  - `EP-05`: Carga en Tablero
  - `EP-06`: Filtros y Reportes
- **Historias de Usuario:** `40 HU` (`HU-001` a `HU-040`, **225 puntos** de historia totales) en trazabilidad 1:1 con `RF-001`..`RF-040`.
- **Requisitos No Funcionales:** `18 RNF` (`RNF-001` a `RNF-018`) clasificados bajo **ISO/IEC 25010:2023** y vinculados a Escenarios de Calidad de 6 partes.

### 2.2 Vista de Escenarios (+1) Cerrada — 8 Casos de Uso de Nivel del Mar
Tomados textualmente de `docs/uml/casos-de-uso.puml` y `docs/especificaciones-casos-de-uso.md`:

#### A. Los 5 Casos Arquitectónicamente Significativos (Especificación Alistair Cockburn Completa)
| ID | Nombre Oficial del Caso de Uso | Actor Primario | Actores Secundarios / Relación |
|---|---|---|---|
| `CU-01` | **Sincronizar y consolidar trazabilidad e indicadores** | Temporizador ETL (Cada 5 min / Webhook) | Redmine, GLPI, GitLab, SonarQube |
| `CU-02` | **Emitir alerta operativa** | Disparado por `CU-01` (`«extend»`) | Mensajería (Discord/Slack), Servidor SMTP |
| `CU-04` | **Consultar tablero de control** | Jefe de Desarrollo / PMO | Tablero alimentado por último snapshot de `CU-01` |
| `CU-05` | **Filtrar indicadores del tablero** | Jefe de Desarrollo / PMO | Extiende `CU-04` (`«extend»`) |
| `CU-06` | **Auditar trazabilidad de causa raíz** | Jefe de Desarrollo / PMO | Consulta cadena histórica completa en memoria consolidada |

#### B. Los 3 Casos Complementarios (Ficha Resumida — Sección 9 de `docs/especificaciones-casos-de-uso.md`)
| ID | Nombre Oficial del Caso de Uso | Actor Primario | Propósito |
|---|---|---|---|
| `CU-03` | **Configurar credenciales y umbrales de integración** | Jefe de Desarrollo / PMO | Administrar tokens API, canales de notificación y umbrales de carga/estancamiento (`RF-001`, `RF-002`, `RF-021`, `RF-028`) |
| `CU-07` | **Exportar reporte de trazabilidad** | Jefe de Desarrollo / PMO | Generar exportación CSV o PDF del snapshot consolidado y filtros activos (`RF-039`, `RF-040`) |
| `CU-08` | **Consultar historial de alertas emitidas** | Jefe de Desarrollo / PMO | Revisar bitácora de alertas enviadas, canal efectivo, deduplicación y fallos de entrega (`RF-030`, `RF-032`) |

### 2.3 Invariantes y Reglas de Negocio Consolidadas (Obligatorias para Vista Lógica y Vista de Procesos)
Cualquier modelo de clases (Semana 4) o secuencia (Semana 5) **debe** respetar estas reglas de dominio ya aprobadas:
1. **Resiliencia y Snapshot Parcial en `CU-01`:** Cada conector externo tiene un timeout máximo de `<= 5 s` (`RNF-001`) y hasta `3` reintentos con *backoff exponencial*. Si una plataforma permanece inalcanzable tras los 3 reintentos, el sistema **preserva y reutiliza el último snapshot válido** de esa fuente para los cruces dependientes (`RF-014`, `RF-016`, `RF-038`), registra un estado de **sincronización parcial** con su última marca temporal exitosa y prohíbe degradar a `0` o borrar métricas válidas previas.
2. **Homologación y Correlación en Memoria (`EP-02`):** Homologación normalizada de identidades de desarrolladores (`RF-008`) y estados operativos (`RF-009`); vinculación tarea–commit–Merge Request (`RF-010`, `RF-011`) y deuda técnica/bugs de SonarQube (`RF-014`).
3. **Detección de 3 Inconsistencias de Trazabilidad:**
   - Commits o Merge Requests huérfanos sin identificador de tarea válido (`RF-013`).
   - Tareas cerradas en Redmine con ramas o Merge Requests aún abiertos en GitLab (`RF-012`).
   - Módulos con actividad de código en GitLab sin cobertura de pruebas o análisis vigente en SonarQube (`RF-016`).
4. **Fórmulas y Umbrales de Carga y Avance (`EP-03`):**
   - **Índice de Carga Ponderada por Prioridad:** `Alta = 3`, `Media = 2`, `Baja = 1` (`RF-018`).
   - **Umbral de Sobrecarga Operativa:** Más de `8` ítems activos **o** más de `40` horas estimadas activas (`RF-021`).
   - **Avance Real vs. Planificado:** Consolidación de tareas cerradas, hitos y MR fusionados (`RF-023`), disparando alerta por desviación negativa `> 20%` (`RF-024`).
5. **Reglas de Alertamiento y Deduplicación (`CU-02`):**
   - **Estancamiento en Certificación:** Más de `5 días hábiles` en estado *Certificación* (`RF-028`).
   - **Deduplicación en Ventana de 24 horas:** Se suprimen alertas repetidas para la misma clave `(tipoAlerta + entidadAfectada)` dentro de `24 h` (`RF-032`).
   - **Canal Primario y Fallback:** Envío inicial vía Webhook a **Mensajería (Discord/Slack)** con hasta `3` reintentos; ante fallo definitivo del webhook, **fallback automático por Servidor SMTP** (`RF-030`). Si ambos fallan, se registra en bandeja crítica para reintento diferido.
6. **Rendimiento y Caché del Tablero (`CU-04`, `CU-05`):** Consulta inicial de tablero en `<= 2 s` apoyada en caché en memoria (`< 5 min`) y advertencia visual cuando exista sincronización parcial; recálculo de filtros en `<= 500 ms` con validación de rango temporal.
7. **Integridad Histórica en Auditoría (`CU-06`):** Si un artefacto fue eliminado en la plataforma origen, la vista de auditoría conserva los metadatos del último snapshot y lo etiqueta como *"No disponible en origen"*.

---

## 3. Inventario de Archivos Protegidos (NO MODIFICAR NI SOBRESCRIBIR)

Está **estrictamente prohibido** editar, regenerar o sobrescribir los artefactos cerrados de las fases anteriores sin una instrucción explícita del usuario:

| Archivo Protegido en el Repositorio | Rol / Entregable Cerrado | Nota de Compatibilidad |
|---|---|---|
| `docs/historias-usuario-padre-julian-lorente.xlsx` | Matriz de `40 HU` (225 pts) y alineación con `RF`/`EP` | Antes denominado `historias-usuario-padre-julian-lorente-v2.xlsx` |
| `docs/Requisitos-padre-julian-lorente.xlsx` | Matriz de `40 RF`, `18 RNF` (ISO 25010:2023), Escenarios de Calidad y Trazabilidad | Antes denominado `trazabilidad-proyecto-padre-julian-lorente.xlsx` |
| `docs/uml/casos-de-uso.puml` | Diagrama maestro consolidado de Casos de Uso (`CU-01`..`CU-08`) | Vista de Escenarios (+1) cerrada |
| `docs/especificaciones-casos-de-uso.md` | Especificaciones Cockburn completas (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`) y fichas (`CU-03`, `CU-07`, `CU-08`) | Fuente textual oficial para Semana 4 y Semana 5 |
| `docs/uml/cadena-de-valor-togaf-adm.puml` | Fuente PlantUML ArchiMate de la Cadena de Valor TOGAF ADM | Diseño ejecutivo de dos bloques cerrado |
| `docs/cadena-de-valor-togaf-adm.png` | Imagen renderizada oficial de la Cadena de Valor TOGAF ADM | Movida desde `docs/uml/` hacia `docs/` |
| `docs/Diagrama_Actividades_Reto2.png` | Diagrama de actividades de referencia del Reto 2 | Referencia histórica protegida |
| `docs/Diagrama de Casos de Uso - Padre Julian.drawio.png` | Referencia visual previa de Casos de Uso | Referencia histórica protegida |
| `docs/referencias/Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf` | Guía oficial del curso (`DSOF_4069`) | Referencia normativa inmutable |
| `docs/referencias/reglas-vista-logica-y-procesos.md` | Compendio de reglas verificables (Semanas 4, 5 y DAS v3.0) | Reglas maestras de diseño |
| `skills-lock.json` | Lockfile de skills instaladas (solo `plantuml`) | Prohibido alterarlo durante fases de diseño |

---

## 4. Dependencia Secuencial Obligatoria e Higiene Arquitectónica (Semanas 4 y 5)

### 4.1 Orden Secuencial Inquebrantable
1. **PRIMERO — Vista Lógica (Semana 4):** Debe completarse, auditarse y cerrarse el modelo de dominio (`docs/vista-logica-clases.md`, `docs/uml/diagrama-clases.puml` + `.png` y `docs/uml/diagrama-estados.puml` + `.png`).
2. **DESPUÉS — Vista de Procesos (Semana 5):** Solo puede ejecutarse y cerrarse cuando la Vista Lógica exista y esté aprobada en el repositorio.
3. **Sincronización 1:1 Obligatoria (Semana 4 ↔ Semana 5):**
   - Cada participante `«entity»` en los Diagramas de Secuencia (`docs/uml/secuencia-*.puml`) debe tener el **mismo nombre exacto** de clase definida en `docs/uml/diagrama-clases.puml`.
   - Cada flecha/mensaje enviado a una entidad o controlador en los Diagramas de Secuencia debe existir como **operación pública (`+`) con firma coincidente** (nombre, parámetros y tipo de retorno) en el Diagrama de Clases y en el Diccionario de Clases (`docs/vista-logica-clases.md`).
   - Si al diseñar la Vista de Procesos surge una nueva operación necesaria, es obligatorio **retroalimentar y actualizar primero la Vista Lógica** antes de cerrar la secuencia.

### 4.2 Prohibición de Contaminación Tecnológica, Física y de UI (Agnosticismo)
En la **Vista Lógica (Semana 4)** y la **Vista de Procesos (Semana 5)** está **terminantemente prohibido**:
- Mencionar o modelar **frameworks o librerías** de código (`NestJS`, `Express`, `React`, `Next.js`, `Prisma`, `TypeORM`, `BullMQ`, `Redis`, `Supabase`).
- Modelar **tablas SQL físicas, tipos de motor o claves foráneas (`FK`)** dentro de las clases (ej. prohibido `VARCHAR`, `INT8`, `JSONB`, `id_ticket_fk`, `proyecto_id`); usar exclusivamente tipos abstractos (`Texto`, `Entero`, `Decimal`, `Booleano`, `FechaHora`) y relaciones UML con multiplicidades.
- Introducir **elementos de interfaz gráfica** como clases del dominio o entidades (`TableroComponent`, `BotonFiltrar`, `ModalAlerta`, `TablaHTML`); en Vista de Procesos, la interacción visual externa se abstrae únicamente tras un estereotipo `«boundary»` conceptual.
- Colar **clases de infraestructura o despliegue** (`DockerContainer`, `NginxProxy`, `PostgreSQLPool`).

*(Nota: Las recomendaciones de stack, `spec-kit` y comandos `npx skills add` para la etapa posterior de desarrollo están documentadas en `docs/handoff/skills-fase-codigo.md` y solo deben ejecutarse cuando las Vistas Lógica y de Procesos estén completamente cerradas).*

---

## 5. Flujo Obligatorio de Validación PlantUML

- **Única skill activa permitida durante diseño:** `.agents/skills/plantuml`.
- **PROHIBIDO ejecutar `npx`, `npm` o instalar nuevas skills** mientras se diseñan las vistas arquitectónicas.
- Antes de redactar nuevos `.puml`, consultar las guías de sintaxis en:
  - `.agents/skills/plantuml/references/class_diagrams.md`
  - `.agents/skills/plantuml/references/state_diagrams.md`
  - `.agents/skills/plantuml/references/sequence_diagrams.md`
  - `.agents/skills/plantuml/references/common_syntax_errors.md`
- **Todo archivo `.puml` nuevo debe cumplir obligatoriamente los 3 pasos siguientes antes de dar la tarea por terminada:**
  1. Verificar entorno local (Java, Graphviz y `plantuml.jar`):
     ```bash
     python .agents/skills/plantuml/scripts/check_setup.py
     ```
  2. Validar sintaxis y renderizar la imagen `.png` dentro de `docs/uml/`:
     ```bash
     python .agents/skills/plantuml/scripts/convert_puml.py <ruta/archivo.puml> --format png --output-dir docs/uml/
     ```
  3. **Inspección visual obligatoria del `.png` generado** para comprobar que el diagrama sea legible, no presente textos cortados o flechas encimadas y cumpla al 100% con la notación UML 2.5.1 exigida en `docs/referencias/reglas-vista-logica-y-procesos.md`.
