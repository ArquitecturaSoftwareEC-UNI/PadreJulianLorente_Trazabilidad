# AGENTS.md — Guía Maestra de Arquitectura para Asistentes IA

> Este archivo define el contexto oficial, la línea base cerrada, las reglas metodológicas y las restricciones de no-regresión para cualquier asistente de IA (**Antigravity**, **Claude Code**, **Cursor**, **GitHub Copilot**) que trabaje en el repositorio **"Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)"**.

---

## 1. Contexto del Proyecto y Resumen del Dominio

- **Organización:** Cooperativa de Ahorro y Crédito (COAC) **Padre Julián Lorente** (Loja, Ecuador) — Departamento de Desarrollo / TI y PMO.
- **Asignatura y Marco Normativo:** Arquitectura de Software (`DSOF_4069`), bajo los estándares **UML 2.5.1**, **ISO/IEC/IEEE 42010** (Modelo Arquitectónico 4+1 de Kruchten), **ISO/IEC 25010:2023** (Atributos de Calidad y Escenarios de 6 partes) y **TOGAF ADM**.
- **Problema y Operación del Sistema:** La información operativa de proyectos de software se encuentra fragmentada en cuatro plataformas externas sin correlación automatizada. El sistema ejecuta un proceso **ETL periódico cada 5 minutos**, disparado por `Tiempo (Temporizador CRON)` en modo **estrictamente de solo lectura** (`RF-029`, `RF-011`, `RNF-003`), que extrae información de:
  1. **Redmine** (épicas POA/PETI, historias/specs REQ/INI.TI, tareas/issues `#ID`, estimaciones, porcentajes reportados, fechas y dependencias)
  2. **GLPI** (tickets de incidentes de soporte asignados a tecnología e incidentes críticos del core financiero)
  3. **GitLab** (ramas, Merge Requests, commits tipados `feat(#ID)`, `fix(#ID)`, `test(#ID)`, `refactor(#ID)`, revisores de QA cruzado y estado de validaciones CI/CD)
  4. **SonarQube** (reportes de calidad técnica, bugs críticos y vulnerabilidades)
- **Modelo de Acceso y Salidas de Valor:**
  - Los desarrolladores operan únicamente en sus herramientas de origen (`Redmine`, `GLPI`, `GitLab`) y **no ingresan ni poseen cuentas en esta plataforma** (`RF-030`).
  - El **Jefe de Desarrollo / PMO** ingresa mediante autenticación institucional (`CU-03`) a una **única vista web compartida en modo solo lectura** (`RF-030`, `RF-032`).
  - El sistema consolida expedientes cronológicos por módulo financiero, calcula carga laboral, avance real y eficiencia, emite alertas proactivas con supresión anti-spam de 24 horas hacia el **Canal de Mensajería Corporativo** (`RF-024`) y el **Servicio de Correo Corporativo** (`RF-025`), permite auditoría causal (`CU-06`), gestión de alertas (`CU-07`) y exportación de reportes trimestrales (`CU-08`).

---

## 2. Línea Base Oficial Cerrada (Métricas, Épicas, Actores y Casos de Uso)

### 2.1 Cobertura Cuantitativa y las 6 Épicas Oficiales
Tomadas textualmente de las hojas `Épicas y Desglose` y `Resumen y Reglas Reto 2` de `docs/historias-usuario-padre-julian-lorente.xlsx`:

- **Total de Requisitos Oficiales:** `58` requisitos (`40 RF` continuos `RF-001`..`RF-040` + `18 RNF` continuos `RNF-001`..`RNF-018` bajo ISO/IEC 25010:2023).
- **Total de Historias de Usuario:** `40 HU` (`HU-001`..`HU-040`, relación `1:1` con `RF-001`..`RF-040`), sumando **225 puntos Fibonacci**.

| ID | Nombre Oficial de la Épica | HU / Puntos | Requisitos Funcionales (1:1) | Casos de Uso |
|---|---|---|---|---|
| `EP-01` | **Acceso Unificado (Jefe de Desarrollo / PMO) e Ingesta Periódica (Extract)** | `4 HU` (`28 pts`) | `RF-011`, `RF-029`, `RF-030`, `RF-032` | `CU-01`, `CU-03`, `CU-04` |
| `EP-02` | **Transformación ETL: Línea Base de 4 Niveles, Commits Tipados y Trazabilidad** | `11 HU` (`68 pts`) | `RF-012`..`RF-018`, `RF-020`..`RF-022`, `RF-033` | `CU-01` |
| `EP-03` | **Transformación ETL: Carga Laboral (40h / Fibonacci) y Avance Real en Código** | `7 HU` (`43 pts`) | `RF-001`..`RF-005`, `RF-037`, `RF-038` | `CU-01` |
| `EP-04` | **Detección de Desviaciones y Alertas Multicanal Corporativas** | `5 HU` (`22 pts`) | `RF-006`, `RF-023`, `RF-024`, `RF-025`, `RF-031` | `CU-02` |
| `EP-05` | **Consolidación de Carga (Load), Embudo de 5 Etapas y Tablero en Tiempo Real** | `3 HU` (`23 pts`) | `RF-007`, `RF-019`, `RF-034` | `CU-01`, `CU-04` |
| `EP-06` | **Vista Única de Carga/Avance, Filtros Combinados, Decisiones y Reportes** | `10 HU` (`41 pts`) | `RF-008`..`RF-010`, `RF-026`..`RF-028`, `RF-035`, `RF-036`, `RF-039`, `RF-040` | `CU-04`..`CU-08` |

### 2.2 Vista de Escenarios (+1) Cerrada — Actores y 8 Casos de Uso de Nivel del Mar
Tomados textualmente de `docs/uml/casos-de-uso.puml` y `docs/especificaciones-casos-de-uso.md`:

- **Actores Primarios (2):** `Tiempo (Temporizador CRON)` y `Jefe de Desarrollo / PMO`.
- **Actores Secundarios / Sistemas Externos (6):** `Redmine`, `GLPI`, `GitLab`, `SonarQube`, `Canal de Mensajería Corporativo` y `Servicio de Correo Corporativo`.

#### A. Los 5 Casos Arquitectónicamente Significativos (Sección 3 — Especificación Alistair Cockburn Completa)
| ID | Nombre Oficial del Caso de Uso | Actor Primario | Actores Secundarios / Relación UML | Cobertura (`RF` / `RNF`) |
|---|---|---|---|---|
| `CU-01` | **Sincronizar y consolidar trazabilidad e indicadores** | `Tiempo (Temporizador CRON)` | `Redmine`, `GLPI`, `GitLab`, `SonarQube` | `21 RF` (`RF-001`..`005`, `011`..`022`, `029`, `033`, `037`, `038`) · `6 RNF` (`001`, `003`, `008`, `009`, `016`, `017`) |
| `CU-02` | **Emitir alerta operativa** | `Tiempo (Temporizador CRON)` *(vía `CU-01`)* | `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo` · `«extend» CU-01 [umbral crítico superado]` | `5 RF` (`RF-006`, `023`, `024`, `025`, `031`) · `4 RNF` (`004`, `010`, `015`, `016`) |
| `CU-04` | **Consultar tablero de control** | `Jefe de Desarrollo / PMO` | Ninguno *(lee almacenamiento consolidado de `CU-01`)* | `5 RF` (`RF-007`, `032`, `034`, `035`, `036`) · `7 RNF` (`002`, `005`, `008`, `011`, `012`, `013`, `014`) |
| `CU-05` | **Filtrar indicadores del tablero** | `Jefe de Desarrollo / PMO` | `«extend» CU-04 [análisis focalizado]` | `4 RF` (`RF-008`, `009`, `028`, `039`) · `4 RNF` (`006`, `012`, `013`, `018`) |
| `CU-06` | **Auditar trazabilidad de causa raíz** | `Jefe de Desarrollo / PMO` | Ninguno *(lee expedientes consolidados históricos de `CU-01`)* | `1 RF` (`RF-010` / `HU-037` sobre integridad de `CU-01`) · `4 RNF` (`001`, `007`, `012`, `013`) |

#### B. Los 3 Casos Complementarios (Sección 4.1 de `docs/especificaciones-casos-de-uso.md` — Fichas Resumidas)
| ID | Nombre Oficial del Caso de Uso | Actor Primario | Requisitos Trazados (`RF` / `HU` / `RNF`) | Propósito Verificable |
|---|---|---|---|---|
| `CU-03` | **Autenticar usuario** | `Jefe de Desarrollo / PMO` | `RF-030` (`HU-001`) · `RNF-011`, `RNF-012`, `RNF-013` | Autentica con credenciales institucionales sobre `TLS 1.3` entregando a Jefatura y PMO la misma vista web única en solo lectura (desarrolladores sin cuentas). Precondición de `CU-04`..`CU-08`. Libera recursos al cierre en `< 200 ms`. |
| `CU-07` | **Gestionar alerta operativa** | `Jefe de Desarrollo / PMO` | `RF-026` (`HU-038`), `RF-027` (`HU-040`) · `RNF-004`, `RNF-007`, `RNF-011`, `RNF-012`, `RNF-013` | Permite marcar alertas priorizadas como «atendida» o «en seguimiento» deteniendo recordatorios y persistiendo quién y cuándo la gestionó; confirma consolidación íntegra al cierre de sesión (`< 200 ms`). |
| `CU-08` | **Exportar reporte trimestral** | `Jefe de Desarrollo / PMO` | `RF-040` (`HU-039`) · `RNF-001`, `RNF-002`, `RNF-012`, `RNF-013` | Genera y exporta en lenguaje ejecutivo el informe trimestral con totales de proyectos `POA/PETI`, requerimientos `REQ`, iniciativas `INI.TI`, incidentes `GLPI`, `%` cumplimiento y certificaciones a tiempo vs. atrasadas (`> 5 días hábiles`). |

---

## 3. Reglas de Negocio, Fórmulas e Invariantes Oficiales (Obligatorias en Semanas 4 y 5)

Cualquier modelo de clases (Vista Lógica — Semana 4) o secuencia (Vista de Procesos — Semana 5) **debe** respetar exactamente estos contratos verificados en `docs/especificaciones-casos-de-uso.md`:

### 3.1 `CU-01` — Sincronizar y consolidar trazabilidad e indicadores
1. **Disparo y Modo Estricto de Solo Lectura (`RF-029`, `RF-011`, `RNF-003`):** Ejecución automática cada **5 minutos** por `Tiempo (Temporizador CRON)` mediante credenciales institucionales de solo lectura (`0` operaciones de escritura, alteración o borrado en `Redmine`, `GLPI`, `GitLab` o `SonarQube`).
2. **Tolerancia a Fallos, Timeout 10 s y Snapshot Parcial (`RNF-008`, `RNF-016`):** Adaptadores desacoplados con límite de espera externo de **10 segundos** y hasta **3 reintentos automáticos** en segundo plano. Si una única plataforma permanece inalcanzable tras los 3 reintentos (Flujo Alterno `2a`), el sistema **conserva y reutiliza el último snapshot válido consolidado** de esa fuente para los cruces dependientes (`RF-014`, `RF-016`, `RF-038`), registra un estado de **sincronización parcial** con su última marca temporal exitosa y **prohíbe degradar o reclasificar falsamente** sus actividades como «sin trazabilidad» o «sin respaldo en código». Si fallan todas las fuentes o se rechazan credenciales (Excepción `2b`), aborta sin sobreescribir datos previos.
3. **Compuerta Obligatoria LOPDP 2021 (`RNF-017`, Excepción `3a`):** Antes de analizar o persistir cualquier dato, el sistema enmascara o elimina el **100 %** de datos personales sensibles (PII) de socios (`cédulas`, `números de cuenta`, `correos particulares`), con retención histórica técnica mínima de **24 meses**. Si la verificación al 100 % falla, se aborta de inmediato todo el lote antes de persistir.
4. **Jerarquía de 4 Niveles y 6 Módulos del Core Financiero (`RF-033`, `RF-012`, `RF-018`):** Clasifica cada petición en la jerarquía `Épica (POA/PETI)` ➔ `Historia/Spec (REQ o INI.TI)` ➔ `Tarea/Issue (#ID)` ➔ `Rama / Merge Request / Commits tipados (feat(#ID), fix(#ID), test(#ID), refactor(#ID))` e identifica obligatoriamente el módulo financiero: **`FitSwitch`**, **`SWI`**, **`Canales Electrónicos`**, **`Crédito`**, **`Antifraude`** o **`Nómina`**.
5. **Correlación Tolerante, Retornos, Divergencia y Actividades sin Trazabilidad (`RF-013`..`RF-017`, `RF-021`, `RF-022`):**
   - Cruce tolerante a errores tipográficos de `GLPI #ID` dentro de `Redmine` (`RF-013`, Flujo Alterno `5a`).
   - Detección de retorno por fallo cuando una tarea retrocede en `Redmine` a «En curso» desde pruebas CI/CD o Certificación (`RF-015`, Flujo Alterno `5b`).
   - Segregación explícita de **«Trabajo en código sin trazabilidad planificada»** para ramas/commits/MR en `GitLab` sin tarea en `Redmine` (`RF-021`, Flujo Alterno `6a`) y de **«Incidente de soporte sin trazabilidad en desarrollo»** para tickets activos en `GLPI` sin espejo `GLPI #ID` en `Redmine` (`RF-022`, Flujo Alterno `6b`), sumando igualmente su esfuerzo en carga.
   - Registro de divergencia cuando el autor de commits difiere del desarrollador asignado en `Redmine` y captura de revisores reales de **QA cruzado** (`RF-017`, Flujo Alterno `7b`).
6. **Complejidad Fibonacci, Carga Semanal Base 40 h y Semáforo (`RF-001`, `RF-002`, `RF-003`):**
   - Conversión Fibonacci (`RF-001`): **`Nivel 1 Baja = 2 puntos`**, **`Nivel 2 Media = 5 puntos`**, **`Nivel 3 Alta = 13 puntos`**.
   - Base semanal lunes a viernes (`40 horas = 8 h/día`, `RF-002`): **Disponible (Verde)** `< 75 %` (`< 30 h`), **Carga Óptima (Amarillo)** `75 %–100 %` (`30 h–40 h`), **Sobreasignado (Rojo)** `> 100 %` (`> 40 h`).
   - Carga laboral efectiva (`RF-003`): tareas `Redmine` en «Análisis» o «En curso» + incidentes activos `GLPI` (**`4 h / 3 pts`** por incidente estándar y **`8 h / 5 pts`** por incidente crítico del core financiero cuando carecen de estimación manual) + revisiones activas de QA cruzado en `GitLab` (**`2 h / 1 pt`** por revisión).
7. **Avance Real Respaldado en Código, Eficiencia y 5 Etapas (`RF-004`, `RF-005`, `RF-019`, `RF-037`, `RF-038`):**
   - Validación de evidencia (`RF-038`, Flujo Alterno `7a`): reportar **`> 25 %`** exige rama activa y commits tipados (`feat(#ID)`, `fix(#ID)`, `test(#ID)`, `refactor(#ID)`); reportar **`100 %`** exige Merge Request aprobado y pipeline CI/CD exitoso. Sin respaldo, se marca brecha de evidencia y se usa el progreso verificable en código.
   - Fórmula de eficiencia (`0–100 %`, `RF-005`): **`70 %`** avance real verificado frente al planificado a la fecha + **`30 %`** calidad técnica (**`-10 pts`** por cada retorno desde pruebas/Certificación y **`-5 pts`** por cada bug crítico o vulnerabilidad en `SonarQube`), excluyendo horas de incidentes emergentes de `GLPI`.
   - Normalización estricta en las **5 etapas oficiales** (`RF-019`): `1. Análisis`, `2. Desarrollo`, `3. Certificación`, `4. Despliegue`, `5. Estabilización`.
8. **Idempotencia, Atomicidad y Rastro Matemático Inmutable (`RNF-009`, `RNF-001`, Excepción `8a`):** Persistencia atómica e idempotente (**`0 %` de duplicados**) mediante llaves naturales compuestas (`Épica POA + Historia/Spec + Tarea #ID + Ticket GLPI + Commit`), conservando al menos **12 meses** de registro inmutable de fórmulas, pesos y entradas para **100 % de reproducibilidad matemática** (`RNF-001`). Ante conflicto al persistir (Excepción `8a`), se revierte completamente el lote sin actualizar la línea vigente.

### 3.2 `CU-02` — Emitir alerta operativa (`«extend» CU-01`)
1. **Eficiencia de Evaluación y Disparadores (`RNF-010`, `RF-031`, `RF-025`, Excepción `1a`):** Evalúa el conjunto de reglas en **`< 200 ms`** (`RNF-010`). Disparadores: carga semanal **`> 40 horas`**, permanencia **`> 5 días hábiles` en Certificación**, retorno por fallo (`RF-031`), incidente de **Prioridad 1** o **cambio de prioridad de proyecto** (`RF-025`). Redacción directa con desarrollador responsable, módulo financiero y dato exacto sin códigos técnicos crudos (`RF-006`).
2. **Supresión Anti-Spam 24 h y Estado de `CU-07` (`RF-023`, `RF-026`, Flujo Alterno `3a`):** Bloquea alertas idénticas emitidas en las últimas **24 horas** (mismo incidente/tarea, causa y severidad) o marcadas como «atendida» / «en seguimiento» en `CU-07` sin agravamiento nuevo.
3. **Canales Corporativos, Escalamiento, Cifrado y Bitácora (`RF-024`, `RF-025`, `RNF-015`, `RNF-004`):** Despacho al **`Canal de Mensajería Corporativo`** (`RF-024`) y mediante el **`Servicio de Correo Corporativo`** cuando exista `> 5 días hábiles` en Certificación, Prioridad 1, cambio de prioridad o **fallo de entrega en el canal de mensajería** (`RF-025`, Flujo Alterno `4a`). Todo despacho externo exige cifrado **`>= TLS 1.2`** (`RNF-015`) y persistencia del **100 %** de eventos aprobados, suprimidos o fallidos con ID único, fecha/hora, categoría, destinatarios y resultado real (`RNF-004`, Excepción `5a`).

### 3.3 `CU-04` — Consultar tablero de control (`«base»` de `CU-05`)
1. **Vista Única Consolidada y Rendimiento (`RF-032`, `RF-007`, `RF-035`, `RF-036`, `RNF-014`):** Vista compartida de solo lectura para `Jefe de Desarrollo / PMO` leyendo el almacenamiento consolidado de `CU-01` (desacoplado de las herramientas origen, Flujo Alterno `2a`). Carga inicial en **`<= 2 s` (p95, hasta 25 sesiones activas y 5.000 registros)** y actualización incremental automática cada 5 minutos en **`< 1 s`** únicamente de elementos cambiados (`RF-034`, `RNF-014`).
2. **Visualización, Cuellos de Botella, Seguridad y Cierre (`RNF-002`, `RNF-005`, `RNF-011`, `RNF-012`, `RNF-013`):** Semáforos sobre 40 h, tarjetas por desarrollador, mapas de calor por módulo financiero, embudo de 5 etapas resaltando cuellos de botella en rojo (`> 35 %` tareas activas, `> 5 días` en Certificación o `> 2 días` en Despliegue — `RF-036`, identificación en máximo 5 segundos según `RNF-005`), adaptabilidad (`1920x1080 px`, `768x1024 px`, `360x640 px` — `RNF-012`), tráfico `HTTPS TLS 1.3` (`RNF-013`) y liberación determinista de recursos al cerrar sesión en **`< 200 ms`** con `0` conexiones huérfanas (`RNF-011`, Flujo Alterno `5a`). Contempla ausencia de sincronización inicial (`2b`) e indisponibilidad de lectura o sesión caducada (`2c`).

### 3.4 `CU-05` — Filtrar indicadores del tablero (`«extend» CU-04`)
1. **Apertura y Recálculo Multidimensional (`RF-008`, `RF-009`, `RNF-006`, `RNF-018`):** Despliegue de controles en **`< 300 ms`** conservando métricas generales de contexto (`RNF-006`). Combinación simultánea por desarrollador, módulo financiero, etapa oficial y rango de fechas recalculando la intersección exacta en **`<= 400 ms` (p95 sobre 5.000 registros)** sin parpadeos (`RNF-018`).
2. **Etiquetas Individuales, Cero Coincidencias y Validaciones (`RF-028`, `RF-039`, Flujos `3a`, `4a`, `4b`, `4c`, `6a`):** Validación de rango temporal (Excepción `3a`: fecha inicial posterior a final o formato inválido rechazado sin alterar la vista), aviso claro ante cero coincidencias con limpieza directa (`4a`, `RF-039`), preservación de filtros activos ante actualización de 5 min (`4b`), etiquetas individuales con retiro selectivo sin reiniciar (`RF-028`) y retorno al estado global (`6a`).

### 3.5 `CU-06` — Auditar trazabilidad de causa raíz
1. **Desglose Causal Completo y Operabilidad (`RF-010`, `RNF-007`, `RNF-001`, `RNF-017`):** Consulta cronológica en solo lectura de los 4 niveles + `GLPI #ID` + conteo de commits tipados + QA cruzado + `SonarQube` + historial en 5 etapas + retornos y entradas exactas del cálculo de eficiencia, jerarquizada por criticidad para decidir acciones correctivas en **`< 10 s`** (`RNF-007`).
2. **Brechas, Expedientes sin Trazabilidad y Excepciones Reales (`RF-021`, `RF-022`, `RF-038`, `RF-017`, Flujos `2a`, `3a`, `2b`, `4a`):** Despliega expedientes parciales de trabajo sin trazabilidad (`RF-021`/`RF-022`, Flujo Alterno `2a`) y exhibe lado a lado brechas de evidencia (`RF-038`) o divergencia autor/asignado (`RF-017`, Flujo Alterno `3a`). Si un expediente fue depurado (`> 24 meses`) o no existe (`2b`), o si falla/se corrompe el rastro auditable inmutable (`4a`), aborta sin mostrar datos incompletos ni exponer información sin enmascarar (`RNF-001`, `RNF-017`).

---

## 4. Inventario de Archivos Protegidos (NO MODIFICAR NI SOBRESCRIBIR)

Está **estrictamente prohibido** editar, regenerar o sobrescribir los artefactos cerrados de las fases anteriores sin una instrucción explícita del usuario:

| Archivo Protegido en el Repositorio | Rol / Entregable Cerrado | Nota |
|---|---|---|
| `docs/historias-usuario-padre-julian-lorente.xlsx` | Matriz oficial de `40 HU` (225 pts Fibonacci), `6 Épicas` y equivalencia histórica | Antes `historias-usuario-padre-julian-lorente-v2.xlsx` |
| `docs/Requisitos-padre-julian-lorente.xlsx` | Matriz oficial de `40 RF` + `18 RNF` (ISO 25010:2023) y trazabilidad | Antes `trazabilidad-proyecto-padre-julian-lorente.xlsx` |
| `docs/uml/casos-de-uso.puml` | Diagrama maestro consolidado de Casos de Uso (`CU-01`..`CU-08`) | Vista de Escenarios (+1) cerrada |
| `docs/especificaciones-casos-de-uso.md` | Especificaciones Cockburn completas (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`) y Sección 4.1 (`CU-03`, `CU-07`, `CU-08`) | Fuente textual oficial para Semana 4 y Semana 5 |
| `docs/uml/cadena-de-valor-togaf-adm.puml` | Fuente PlantUML de la Cadena de Valor TOGAF ADM construida con bloques `rectangle` y `card` estándar | Diseño ejecutivo de dos bloques cerrado |
| `docs/cadena-de-valor-togaf-adm.png` | Imagen renderizada oficial de la Cadena de Valor TOGAF ADM | Ubicada en `docs/` |
| `docs/Diagrama_Actividades_Reto2.png` | Diagrama de actividades de referencia del Reto 2 | Referencia histórica protegida |
| `docs/Diagrama de Casos de Uso - Padre Julian.drawio.png` | Referencia visual previa de Casos de Uso | Referencia histórica protegida |
| `docs/referencias/Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf` | Guía oficial del curso (`DSOF_4069`) | Referencia normativa inmutable |
| `docs/referencias/reglas-vista-logica-y-procesos.md` | Compendio de reglas verificables (Semanas 4, 5 y DAS v3.0) | Reglas maestras de diseño |
| `skills-lock.json` | Lockfile de skills instaladas (solo `plantuml`) | Prohibido alterarlo durante fases de diseño |

---

## 5. Dependencia Secuencial Obligatoria y Agnosticismo (Semanas 4 y 5)

1. **PRIMERO — Vista Lógica (Semana 4):** Debe completarse, auditarse y cerrarse el modelo de dominio (`docs/vista-logica-clases.md`, `docs/uml/diagrama-clases.puml` + `.png` y `docs/uml/diagrama-estados.puml` + `.png`).
2. **DESPUÉS — Vista de Procesos (Semana 5):** Solo puede ejecutarse y cerrarse cuando la Vista Lógica exista y esté aprobada en el repositorio.
3. **Sincronización 1:1 Obligatoria (Semana 4 ↔ Semana 5):**
   - Cada participante `«entity»` en los Diagramas de Secuencia (`docs/uml/secuencia-*.puml`) debe tener el **mismo nombre exacto** de clase definida en `docs/uml/diagrama-clases.puml`.
   - Cada flecha/mensaje enviado a una entidad o controlador en los Diagramas de Secuencia debe existir como **operación pública (`+`) con firma coincidente** (nombre, parámetros y tipo de retorno) en `docs/uml/diagrama-clases.puml` y `docs/vista-logica-clases.md`.
   - Si al diseñar la Vista de Procesos surge una nueva operación necesaria, es obligatorio **actualizar primero la Vista Lógica** antes de cerrar la secuencia.
4. **Prohibición de Contaminación Tecnológica, Física y de UI:** En las Vistas Lógica y de Procesos está **terminantemente prohibido** mencionar frameworks o productos concretos (`NestJS`, `Express`, `React`, `Angular`, `Prisma`, `TypeORM`, `BullMQ`, `Redis`, `Supabase`, `PostgreSQL`, marcas comerciales de chat o protocolos físicos de correo), modelar tablas SQL físicas o claves foráneas (`VARCHAR`, `INT8`, `JSONB`, `id_tarea_fk`), o introducir componentes visuales UI como entidades del dominio; en Vista de Procesos, toda interacción externa se encapsula tras participantes `«boundary»` agnósticos.

*(Las recomendaciones de stack, `spec-kit` y comandos `npx skills add` para la etapa posterior de desarrollo están documentadas en `docs/handoff/skills-fase-codigo.md` y solo deben ejecutarse cuando las Vistas Lógica y de Procesos estén cerradas).*

---

## 6. Flujo Obligatorio de Validación PlantUML

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
  3. **Inspección visual obligatoria del `.png` generado** para comprobar legibilidad, ausencia de superposiciones y cumplimiento estricto de la notación UML 2.5.1 exigida en `docs/referencias/reglas-vista-logica-y-procesos.md`.
