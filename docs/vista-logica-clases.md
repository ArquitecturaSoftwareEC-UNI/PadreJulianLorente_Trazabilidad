# Vista Lógica (Semana 4) — Modelo del Dominio, Tarjetas CRC, Diccionario de Clases y Trazabilidad

**Proyecto:** Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)  
**Asignatura y Marco Normativo:** Arquitectura de Software (`DSOF_4069`) · Modelo Arquitectónico `4+1` de Kruchten (ISO/IEC/IEEE 42010) · UML 2.5.1 · ISO/IEC 25010:2023 · TOGAF ADM  
**Artefactos UML Asociados:**
- Diagrama de Clases (Nivel de Especificación): [diagrama-clases.puml](uml/diagrama-clases.puml) · [diagrama-clases.png](uml/diagrama-clases.png)
- Diagrama de Estados del Ciclo de Vida: [diagrama-estados.puml](uml/diagrama-estados.puml) · [diagrama-estados.png](uml/diagrama-estados.png)
**Insumos Oficiales de la Vista +1 (Cerrada en Semana 3):**
- [especificaciones-casos-de-uso.md](especificaciones-casos-de-uso.md) (`CU-01` a `CU-08`)
- [casos-de-uso.puml](uml/casos-de-uso.puml)
- [Requisitos-padre-julian-lorente.xlsx](Requisitos-padre-julian-lorente.xlsx) (`40 RF` + `18 RNF`) e [historias-usuario-padre-julian-lorente.xlsx](historias-usuario-padre-julian-lorente.xlsx) (`40 HU` / `225 pts Fibonacci`)
- [reglas-vista-logica-y-procesos.md](referencias/reglas-vista-logica-y-procesos.md)

> **Garantía de Agnosticismo y Cumplimiento de las 5 Exclusiones (Semana 4):**  
> Este artefacto modela exclusivamente la estructura estática del dominio en los niveles **Conceptual** y de **Especificación**. Cumple de forma estricta las **5 Exclusiones** de la Guía Maestra: **(1)** Cero tablas SQL físicas, tipos de motor (`VARCHAR`, `INT8`, `JSONB`) o claves foráneas (`FK`) como atributos; **(2)** Cero nodos físicos, servidores o contenedores; **(3)** Cero paquetes de frameworks (`NestJS`, `React`, `Prisma`) o primitivos de hilos/colas; **(4)** Cero componentes visuales de interfaz gráfica (`Pantalla`, `Modal`, `Boton`); y **(5)** Todo atributo calculable indispensable se marca explícitamente como derivado mediante el prefijo `/`.

---

## 1. Decodificación Gramatical y Árbol de 4 Filtros Secuenciales

### 1.1 Extracción Gramatical desde las Especificaciones de Casos de Uso (`CU-01` a `CU-08`)

Siguiendo la técnica de análisis lingüístico de Abbott/Booch establecida en la Sección 1.3 de [reglas-vista-logica-y-procesos.md](referencias/reglas-vista-logica-y-procesos.md), se decodificaron las especificaciones completas de `CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06` y las fichas complementarias de `CU-03`, `CU-07` y `CU-08`:

| Categoría Gramatical en la Vista +1 | Elementos Extraídos de `docs/especificaciones-casos-de-uso.md` | Rol Candidato en la Vista Lógica |
|---|---|---|
| **1. Sustantivos y Frases Nominales** | `Ciclo periódico de sincronización (5 min)`, `Snapshot válido por plataforma externa`, `Compuerta de anonimización LOPDP 2021`, `Expediente cronológico por módulo financiero`, `Épica POA/PETI`, `Historia / Especificación (REQ o INI.TI)`, `Tarea / Issue (#ID)`, `Actividad sin trazabilidad (RF-021 / RF-022)`, `Ticket de incidente GLPI (#ID)`, `Evidencia en código (Rama, Merge Request, Commits tipados)`, `Hallazgo de calidad SonarQube (bug crítico / vulnerabilidad)`, `Desarrollador`, `Asignación / Participación por rol (asignado, autor, revisor QA cruzado, soporte)`, `Registro auditable inmutable de cálculo (12 meses)`, `Alerta operativa`, `Sesión institucional autenticada (Jefe de Desarrollo / PMO)`, `Reporte trimestral ejecutivo`, `Redmine`, `GLPI`, `GitLab`, `SonarQube`, `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo`, `Tiempo (Temporizador CRON)`, `Tablero web`, `Tarjeta visual`, `Mapa de calor`, `Embudo gráfico`, `Etiqueta de filtro`. | Candidatos a **Clases del Dominio**, **Atributos** o **Descartes por Exclusión**. |
| **2. Verbos de Acción y Frases Verbales** | `iniciarCicloPeriodico`, `extraerEnSoloLectura`, `activarContingenciaSnapshotParcial`, `depurarYEnmascararPII`, `verificarCeroExposicionPII`, `unificarExpedienteCronologico`, `consolidarAvanceRealVsPlanificado`, `vincularTareaPlanificada`, `normalizarEtapaOficial`, `verificarRespaldoAvanceEnCodigo`, `registrarRetornoPorFallo`, `clasificarActividadSinTrazabilidad`, `enlazarTicketGLPITolerante`, `contabilizarCommitsTipados`, `calcularCargaLaboralEfectiva`, `determinarSemaforoOcupacion`, `calcularEficienciaPonderada`, `verificarDivergenciaAutoria`, `sellarRegistroInmutable`, `evaluarReglasUmbralCritico`, `verificarSupresionAntiSpam24Horas`, `marcarEstadoGestionJefatura`, `autenticarCredencialesInstitucionales`, `aplicarFiltroMultidimensional`, `retirarCriterioFiltroIndividual`, `consolidarMetricasTrimestrales`. | **Operaciones Públicas (`+`)** de las clases y **Verbos de Lectura** en relaciones UML. |
| **3. Adjetivos, Clasificadores y Estados** | Módulos: `FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude`, `Nómina` (`RF-012`). Etapas: `1. Análisis`, `2. Desarrollo`, `3. Certificación`, `4. Despliegue`, `5. Estabilización` (`RF-019`). Complejidad Fibonacci: `Nivel 1 Baja (2 pts)`, `Nivel 2 Media (5 pts)`, `Nivel 3 Alta (13 pts)` (`RF-001`). Semáforo 40h: `Disponible Verde (< 75 %)`, `Carga Óptima Amarillo (75 %–100 %)`, `Sobreasignado Rojo (> 100 %)` (`RF-002`). Commits: `feat`, `fix`, `test`, `refactor` (`RF-014`). Estado de Alerta: `Evaluada`, `Suprimida24h`, `EnviadaMensajeria`, `EscaladaCorreo`, `ConmutadaCorreoPorFallo`, `EntregaFallida`, `EnSeguimiento`, `Atendida` (`CU-02`, `CU-07`). | **Enumeraciones (`«enumeration»`)**, **Atributos de Estado** y **Máquinas de Estado UML**. |
| **4. Expresiones Cuantitativas y Reglas Numéricas** | «4 plataformas externas» (`1..4`), «6 módulos del core financiero» (`1`), «jerarquía de 4 niveles» (`1` a `1..*`), «una tarea puede tener cero o un ticket GLPI espejo» (`0..1`), «una actividad puede carecer de historia planificada si es trabajo sin trazabilidad» (`0..1`), «un desarrollador participa en múltiples actividades con distintos roles» (`1` a `1..*` vía clase asociativa), «retención mínima de 12 meses para cálculos (`RNF-001`) y 24 meses para expedientes anonimizados (`RNF-017`)». | **Multiplicidades Bilaterales (`1`, `0..1`, `*`, `1..*`, `1..4`)** e **Invariantes Cuantitativas**. |

---

### 1.2 Tabla de Descartes Inmediatos (Previo al Árbol de 4 Filtros)

De acuerdo con la Sección 1.4 de [reglas-vista-logica-y-procesos.md](referencias/reglas-vista-logica-y-procesos.md), se depuran inmediatamente los siguientes candidatos antes de someterlos a los 4 filtros:

| Candidato Extraído | Criterio Normativo de Descarte | Resolución Arquitectónica en la Vista Lógica |
|---|---|---|
| `Colaborador`, `Programador`, `Técnico TI`, `Ingeniero` | **Sinónimo** | Se unifica bajo el término canónico **`Desarrollador`** (`RF-001`..`RF-005`, `RF-030`). |
| `Petición`, `Issue`, ` ítem de trabajo`, `Tarea` | **Sinónimo / Generalización** | Se unifica en **`ActividadTrazable`**, capaz de representar tanto tareas planificadas (`#ID`) como actividades segregadas sin trazabilidad (`RF-021`, `RF-022`). |
| `Aviso`, `Notificación`, `Recordatorio` | **Sinónimo** | Se unifica bajo el término canónico **`AlertaOperativa`** (`CU-02`, `CU-07`). |
| `Proyecto POA`, `Iniciativa Estratégica PETI` | **Sinónimo** | Se unifica en **`EpicaPOA`** (Nivel 1 de la jerarquía oficial `RF-033`). |
| `Requerimiento REQ`, `Iniciativa INI.TI`, `Spec` | **Sinónimo** | Se unifica en **`HistoriaEspecificacion`** (Nivel 2 de la jerarquía oficial `RF-033`). |
| `Redmine`, `GLPI`, `GitLab`, `SonarQube` | **Actor Secundario Externo** | Sistemas externos cuya historia interna no administra nuestra plataforma (`RNF-003`). En la Vista Lógica solo se modela **`SnapshotFuenteExterna`** (el estado consolidado propio que nuestro sistema preserva para tolerancia a fallos bajo `RNF-008`) y la enumeración `FuenteOperativaExterna`. *(Sus adaptadores pertenecen a la Vista de Procesos como `«boundary»`).* |
| `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo` | **Actor Secundario Externo** | Sistemas externos de despacho (`CU-02`). El dominio registra el canal empleado y el resultado de entrega dentro de **`AlertaOperativa`** (`RNF-004`). |
| `Tiempo (Temporizador CRON)` | **Actor Primario Externo** | Disparador temporal externo cada 5 minutos (`RF-029`). El dominio modela la ejecución resultante y su trazabilidad mediante **`CicloSincronizacion`**. |
| `Vista Web Única`, `Pantalla Tablero`, `Tarjeta Visual`, `Mapa de Calor Gráfico`, `Embudo Visual`, `Botón Exportar`, `Etiqueta Visual de Filtro` | **Elemento de Interfaz Gráfica (UI) — Exclusión 4** | Prohibido modelar pantallas o widgets como clases del dominio. Se modelan los **conceptos y proyecciones lógicas subyacentes**: `ExpedienteModuloFinanciero`, `Desarrollador`, `SesionInstitucional` (que conserva el estado lógico de los filtros activos en `CU-05`) y `ReporteTrimestral`. |
| `Filtrado Multidimensional`, `Enmascaramiento`, `Cálculo de Eficiencia`, `Autenticación`, `Correlación Tolerante` | **Operación Disfrazada de Sustantivo** | No son entidades sino **operaciones públicas (`+`)** encapsuladas en `SesionInstitucional`, `AuditorCompuertaLOPDP`, `Desarrollador` y `ActividadTrazable`. *(En cambio, el evento auditable persistido con identidad y retención de 12–24 meses sí es clase: `RegistroAuditoriaCalculo` y `AuditorCompuertaLOPDP`).* |

---

### 1.3 Evaluación en el Árbol de 4 Filtros Secuenciales

Cada candidato sobreviviente fue evaluado secuencialmente contra los **4 filtros obligatorios** (Sección 1.4 de [reglas-vista-logica-y-procesos.md](referencias/reglas-vista-logica-y-procesos.md)):
- **Filtro 1 (Memoria / Persistencia Conceptual):** ¿El sistema debe recordar sus instancias entre ejecuciones o durante la ventana de retención normativa (`12 meses` en `RNF-001` / `24 meses` en `RNF-017`)?
- **Filtro 2 (Complejidad Estructural):** ¿Posee `> 1` atributo relevante propio?
- **Filtro 3 (Identidad Propia y Ciclo de Vida Independiente):** ¿Posee identificador de negocio y transiciones de estado propias?
- **Filtro 4 (Participación en Escenarios):** ¿Interviene activamente en al menos un Caso de Uso (`CU-01`..`CU-08`)?

| # | Candidato Evaluado | Filtro 1: Persistencia | Filtro 2: `> 1` Atributo | Filtro 3: Identidad / Ciclo Propio | Filtro 4: Casos de Uso (`CU-01`..`CU-08`) | Dictamen Final |
|---|---|---|---|---|---|---|
| 1 | **`CicloSincronizacion`** | **SÍ** (auditoría de cada corrida de 5 min) | **SÍ** (8 atributos) | **SÍ** (`codigoCiclo`, máquina de estados `CU-01`) | **SÍ** (`CU-01`, `CU-04`) | ✅ **CLASE APROBADA** |
| 2 | **`SnapshotFuenteExterna`** | **SÍ** (preservado ante caída externa `RNF-008`) | **SÍ** (8 atributos) | **SÍ** (`codigoSnapshot` por plataforma origen) | **SÍ** (`CU-01`, `CU-04`) | ✅ **CLASE APROBADA** |
| 3 | **`AuditorCompuertaLOPDP`** | **SÍ** (certificación legal LOPDP 2021, 24 meses `RNF-017`) | **SÍ** (7 atributos) | **SÍ** (`codigoVerificacion` por lote ETL) | **SÍ** (`CU-01`, `CU-06`) | ✅ **CLASE APROBADA** |
| 4 | **`ExpedienteModuloFinanciero`** | **SÍ** (retención técnica `>= 24 meses`, `RNF-017`) | **SÍ** (6 atributos) | **SÍ** (`codigoExpediente` por módulo core) | **SÍ** (`CU-01`, `CU-04`, `CU-05`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 5 | **`EpicaPOA`** | **SÍ** (Nivel 1 POA/PETI, `RF-033`) | **SÍ** (8 atributos) | **SÍ** (`codigoEpica`) | **SÍ** (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 6 | **`HistoriaEspecificacion`** | **SÍ** (Nivel 2 `REQ` / `INI.TI`, `RF-033`) | **SÍ** (6 atributos) | **SÍ** (`codigoEspecificacion`) | **SÍ** (`CU-01`, `CU-04`, `CU-05`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 7 | **`ActividadTrazable`** | **SÍ** (Nivel 3 `#ID` + segregadas `RF-021`/`022`, `RNF-009`) | **SÍ** (13 atributos) | **SÍ** (`llaveNaturalCompuesta`, máquina 5 etapas) | **SÍ** (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 8 | **`TicketIncidenteGLPI`** | **SÍ** (`GLPI #ID` vinculado o sin traza, `RF-013`, `RF-022`) | **SÍ** (8 atributos) | **SÍ** (`codigoTicketGLPI`) | **SÍ** (`CU-01`, `CU-02`, `CU-04`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 9 | **`EvidenciaCodigo`** | **SÍ** (Nivel 4 rama, MR, commits tipados, CI/CD `RF-014`, `RF-038`) | **SÍ** (9 atributos) | **SÍ** (`nombreRepositorio + nombreRamaActiva`) | **SÍ** (`CU-01`, `CU-04`, `CU-05`, `CU-06`) | ✅ **CLASE APROBADA** |
| 10 | **`HallazgoCalidadSonar`** | **SÍ** (bugs críticos y vulnerabilidades `-5 pts`, `RF-005`) | **SÍ** (6 atributos) | **SÍ** (`codigoHallazgo`) | **SÍ** (`CU-01`, `CU-06`) | ✅ **CLASE APROBADA** |
| 11 | **`Desarrollador`** | **SÍ** (capacidad base 40h, Fibonacci y eficiencia `RF-001`..`005`) | **SÍ** (11 atributos) | **SÍ** (`identificadorInstitucional`) | **SÍ** (`CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06`) | ✅ **CLASE APROBADA** |
| 12 | **`AsignacionActividad`** | **SÍ** (participación por rol y divergencia `RF-003`, `RF-017`) | **SÍ** (6 atributos) | **SÍ** (`codigoAsignacion`, Clase Asociativa N:M) | **SÍ** (`CU-01`, `CU-04`, `CU-05`, `CU-06`) | ✅ **CLASE APROBADA** |
| 13 | **`RegistroAuditoriaCalculo`** | **SÍ** (inmutable mínimo 12 meses, `RNF-001`) | **SÍ** (9 atributos) | **SÍ** (`codigoRegistroAuditoria`) | **SÍ** (`CU-01`, `CU-06`, `CU-08`) | ✅ **CLASE APROBADA** |
| 14 | **`AlertaOperativa`** | **SÍ** (100 % persistida con anti-spam 24h y gestión `RNF-004`) | **SÍ** (12 atributos) | **SÍ** (`identificadorUnicoAlerta`, máquina `CU-02`/`CU-07`) | **SÍ** (`CU-02`, `CU-04`, `CU-06`, `CU-07`) | ✅ **CLASE APROBADA** |
| 15 | **`SesionInstitucional`** | **SÍ** (sesión vigente, filtros activos y cierre `< 200 ms`) | **SÍ** (8 atributos) | **SÍ** (`identificadorSesion`) | **SÍ** (`CU-03`, `CU-04`, `CU-05`, `CU-06`, `CU-07`, `CU-08`) | ✅ **CLASE APROBADA** |
| 16 | **`ReporteTrimestral`** | **SÍ** (consolidado ejecutivo trimestral auditable `RF-040`) | **SÍ** (10 atributos) | **SÍ** (`codigoReporte`) | **SÍ** (`CU-08`) | ✅ **CLASE APROBADA** |
| 17 | `RangoFechasFiltro` | ❌ **NO** (parámetro transitorio de entrada en `CU-05`) | Sí (2 fechas) | No | `CU-05` | ❌ **DESCARTADO EN FILTRO 1** *(Pasa a entradas de `criteriosFiltroActivos` en `SesionInstitucional`)* |
| 18 | `ModuloFinancieroCore` | Sí | ❌ **NO** (valor atómico de dominio cerrado de 6 valores) | No | `CU-01`..`CU-08` | 🔄 **PASA A `«enumeration»` EN FILTRO 2** |
| 19 | `EtapaOficialFlujo` | Sí | ❌ **NO** (dominio cerrado de 5 etapas `RF-019`) | No | `CU-01`..`CU-06` | 🔄 **PASA A `«enumeration»` EN FILTRO 2** |
| 20 | `NivelComplejidadFibonacci` | Sí | ❌ **NO** (escala cerrada `2, 5, 13` `RF-001`) | No | `CU-01`, `CU-04` | 🔄 **PASA A `«enumeration»` EN FILTRO 2** |

---

## 2. Validación con Tarjetas CRC y Decisiones Arquitectónicas de Diseño

### 2.1 Auditoría de Cohesión y Acoplamiento mediante Tarjetas CRC

Cada una de las **16 clases aprobadas** fue sometida a validación mediante **Tarjetas CRC (*Clase – Responsabilidad – Colaboración*)** para garantizar el principio de **Responsabilidad Única (Alta Cohesión)**, evitar **Clases Dios (*God Classes*)** y mantener **Bajo Acoplamiento**:

#### Paquete 1: Gobernanza de Ingesta, Resiliencia y Compuerta LOPDP (`«resilience_gate»`)

| Tarjeta CRC #1: `CicloSincronizacion` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Coordina el estado de cada corrida periódica disparada cada 5 minutos (`RF-029`).<br>• Verifica que toda extracción opere en modo estricto de solo lectura (`RF-011`, `RNF-003`).<br>• Activa el estado de sincronización parcial cuando una fuente cae tras 3 reintentos de `10 s` reutilizando su último snapshot válido (`RNF-008`, Flujo `2a`).<br>• Ejecuta la consolidación atómica e idempotente (`0 %` duplicados, `RNF-009`) o revierte el lote completo ante conflicto (`8a`). | • `SnapshotFuenteExterna`<br>• `AuditorCompuertaLOPDP`<br>• `ExpedienteModuloFinanciero`<br>• `EstadoCicloSincronizacion` |

| Tarjeta CRC #2: `SnapshotFuenteExterna` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Conserva la última marca temporal exitosa, el conteo de registros y la huella de integridad por cada plataforma origen (`Redmine`, `GLPI`, `GitLab`, `SonarQube`).<br>• Registra el tiempo de respuesta externo (`<= 10 s`) y el número de reintentos (`0..3`, `RNF-008`).<br>• Provee los datos del último snapshot válido durante una caída parcial para impedir que las actividades vinculadas se degraden falsamente a «trabajo sin trazabilidad» (Flujo `2a`). | • `CicloSincronizacion`<br>• `FuenteOperativaExterna` |

| Tarjeta CRC #3: `AuditorCompuertaLOPDP` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Actúa como compuerta obligatoria de Privacidad por Diseño (LOPDP Ecuador 2021, `RNF-017`) antes de cualquier análisis o persistencia.<br>• Enmascara o elimina el `100 %` de datos personales sensibles de socios (`cédulas`, `números de cuenta`, `correos particulares`).<br>• Aborta de inmediato el ciclo si la verificación no alcanza el `100 %` de depuración (Excepción `3a`).<br>• Controla la ventana de retención histórica técnica mínima de `24 meses` (`RNF-017`). | • `CicloSincronizacion`<br>• `ExpedienteModuloFinanciero` |

---

#### Paquete 2: Jerarquía de 4 Niveles, Expedientes y Evidencia en Código (`«core_hierarchy»`)

| Tarjeta CRC #4: `ExpedienteModuloFinanciero` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Agrupa y consolida cronológicamente toda la trazabilidad perteneciente a cada uno de los 6 módulos del core financiero (`FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude`, `Nómina` — `RF-012`, `RF-018`).<br>• Calcula la densidad de criticidad para el mapa de calor (`RF-007`) y el conteo del embudo en las 5 etapas oficiales (`RF-019`, `RF-036`).<br>• Reconstruye en `< 10 s` la cadena causal completa o parcial para la auditoría de causa raíz (`CU-06`, `RNF-007`). | • `EpicaPOA`<br>• `ActividadTrazable`<br>• `ModuloFinancieroCore`<br>• `EtapaOficialFlujo` |

| Tarjeta CRC #5: `EpicaPOA` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa el **Nivel 1** de la jerarquía oficial (`POA/PETI`, `RF-033`).<br>• Mantiene las fechas planificadas de línea base (`RF-012`), la prioridad estratégica del proyecto (`RF-025`) y el avance planificado a la fecha (`RF-004`).<br>• Consolida el avance real verificado en código frente al planificado a través de sus historias y tareas (`RF-036`, `RF-037`). | • `ExpedienteModuloFinanciero`<br>• `HistoriaEspecificacion` |

| Tarjeta CRC #6: `HistoriaEspecificacion` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa el **Nivel 2** de la jerarquía oficial (`REQ` o `INI.TI`, `RF-033`).<br>• Custodia las horas estimadas de línea base, fecha de compromiso y el vínculo con sus tareas planificadas en `Redmine` (`RF-012`, `RF-016`). | • `EpicaPOA`<br>• `ActividadTrazable` |

| Tarjeta CRC #7: `ActividadTrazable` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa el **Nivel 3** de la jerarquía (`Tarea/Issue #ID` en `Redmine`) y también las actividades segregadas sin cadena completa (`RF-021`: código sin tarea en Redmine; `RF-022`: incidente GLPI sin espejo en Redmine).<br>• Garantiza unicidad e idempotencia (`0 %` duplicados) mediante su `llaveNaturalCompuesta` (`RNF-009`).<br>• Normaliza su estado en las **5 etapas oficiales** (`RF-019`), contabiliza retornos por fallo desde pruebas/Certificación (`RF-015`, Flujo `5b`) y verifica si el porcentaje reportado en Redmine (`> 25 %` o `100 %`) posee respaldo real en código (`RF-038`, Flujo `7a`).<br>• Identifica si constituye cuello de botella (`> 5 días hábiles` en Certificación o `> 2 días` en Despliegue, `RF-036`). | • `HistoriaEspecificacion`<br>• `TicketIncidenteGLPI`<br>• `EvidenciaCodigo`<br>• `HallazgoCalidadSonar`<br>• `AsignacionActividad`<br>• `RegistroAuditoriaCalculo`<br>• `AlertaOperativa` |

| Tarjeta CRC #8: `TicketIncidenteGLPI` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa los tickets de soporte asignados a tecnología en `GLPI` (`GLPI #ID`).<br>• Normaliza referencias con errores de digitación (mayúsculas, guiones o espacios) para enlazarlos tolerantemente con `Redmine` (`RF-013`, Flujo `5a`).<br>• Determina automáticamente el esfuerzo estándar cuando carece de estimación manual: **`4 h / 3 pts`** para incidente estándar y **`8 h / 5 pts`** para incidente crítico del core financiero (`RF-003`). | • `ActividadTrazable` |

| Tarjeta CRC #9: `EvidenciaCodigo` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa el **Nivel 4** de la jerarquía oficial en `GitLab` (`RF-033`, `RF-014`): repositorio, rama activa, Merge Request, estado de pipeline CI/CD y conteo de commits tipados (`feat(#ID)`, `fix(#ID)`, `test(#ID)`, `refactor(#ID)`).<br>• Certifica objetivamente si se cumplen las condiciones de evidencia para avances `> 25 %` (rama activa + commits tipados) y `100 %` (MR aprobado + CI/CD exitoso) bajo `RF-038`. | • `ActividadTrazable`<br>• `HallazgoCalidadSonar`<br>• `TipoCommitTipado` |

| Tarjeta CRC #10: `HallazgoCalidadSonar` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Representa los bugs críticos y vulnerabilidades reportados por `SonarQube` sobre el código de una actividad (`RF-005`, `RF-014`).<br>• Calcula la penalización técnica exacta de **`-5 puntos`** por cada hallazgo crítico vigente dentro del componente del `30 %` de calidad técnica (`RF-005`). | • `ActividadTrazable`<br>• `EvidenciaCodigo` |

---

#### Paquete 3: Capacidad Base 40h, Roles por Asociación y Rastro Matemático (`«capacity_role»` / `«resilience_gate»`)

| Tarjeta CRC #11: `Desarrollador` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Modela al integrante del equipo de tecnología como sujeto medido por el sistema (sin cuenta de usuario ni acceso a la plataforma según `RF-030`).<br>• Calcula su carga laboral efectiva semanal sobre la base de **`40 horas` (`8 h/día`, lunes a viernes)** sumando tareas Redmine en `Análisis`/`En curso`, incidentes `GLPI` (`4 h` / `8 h`) y revisiones activas de QA cruzado (`2 h / 1 pt`) (`RF-001`..`RF-003`).<br>• Determina su semáforo de capacidad (`Verde < 75 %`, `Amarillo 75 %–100 %`, `Rojo > 100 %`, `RF-002`).<br>• Calcula su eficiencia (`0–100 %`) ponderando **`70 %` avance real verificado vs. planificado + `30 %` calidad técnica** (`-10 pts` por retorno y `-5 pts` por bug/vulnerabilidad `SonarQube`), excluyendo horas de incidentes emergentes `GLPI` (`RF-005`). | • `AsignacionActividad`<br>• `RegistroAuditoriaCalculo`<br>• `AlertaOperativa`<br>• `SemaforoCapacidad` |

| Tarjeta CRC #12: `AsignacionActividad` *(Clase Asociativa N:M)* | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Descompone la relación muchos-a-muchos entre `Desarrollador` y `ActividadTrazable` aplicando el patrón de **Roles por Asociación** (`RF-003`, `RF-017`).<br>• Registra si el desarrollador actúa en esa actividad como `RESPONSABLE_ASIGNADO_REDMINE`, `AUTOR_REAL_COMMIT_GITLAB`, `REVISOR_QA_CRUZADO_GITLAB` (`+2 h / 1 pt`) o `RESPONSABLE_SOPORTE_GLPI`.<br>• Detecta y deja constancia de divergencia cuando el autor de los commits difiere del desarrollador asignado en Redmine (`RF-017`, Flujo `7b`). | • `Desarrollador`<br>• `ActividadTrazable`<br>• `RolParticipacionDesarrollo` |

| Tarjeta CRC #13: `RegistroAuditoriaCalculo` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Conserva de forma **inmutable por al menos 12 meses** la versión de la fórmula, los pesos (`70 % / 30 %`), las entradas numéricas exactas y el resultado de cada cálculo de carga, avance y eficiencia (`RNF-001`).<br>• Permite verificar en cualquier auditoría (`CU-06`) o exportación trimestral (`CU-08`) el **`100 %` de reproducibilidad matemática**, o abortar la entrega si se detecta corrupción del rastro inmutable (Excepción `4a` de `CU-06`). | • `Desarrollador`<br>• `ActividadTrazable` |

---

#### Paquete 4: Gobierno PMO, Alertas Multicanal, Acceso Único y Reportes (`«governance_alert»`)

| Tarjeta CRC #14: `AlertaOperativa` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Evalúa en **`< 200 ms`** (`RNF-010`) los disparadores críticos: sobrecarga `> 40 h`, permanencia `> 5 días hábiles` en Certificación, retorno por fallo (`RF-031`), incidente Prioridad 1 o cambio de prioridad de proyecto (`RF-025`).<br>• Redacta el aviso en lenguaje ejecutivo claro con responsable, módulo financiero y valor exacto sin códigos crudos (`RF-006`).<br>• Aplica la política de **supresión anti-spam de 24 horas** (`RF-023`) y respeta el estado de alertas ya marcadas como `Atendida` o `EnSeguimiento` sin agravamiento (`RF-026`, Flujo `3a`).<br>• Registra el estado de entrega sobre canales cifrados (`>= TLS 1.2`, `RNF-015`), la conmutación automática a correo ante caída de mensajería (Flujo `4a`), y quién/cuándo gestionó la alerta en `CU-07` (`RNF-004`). | • `ActividadTrazable`<br>• `Desarrollador`<br>• `SesionInstitucional`<br>• `EstadoAlertaOperativa`<br>• `CategoriaDisparadorAlerta` |

| Tarjeta CRC #15: `SesionInstitucional` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Gestiona el acceso autenticado sobre `HTTPS TLS 1.3` (`RNF-013`) exclusivamente para `Jefe de Desarrollo` y `PMO / Jefatura de Proyectos`, entregando a ambos exactamente la misma vista única en modo solo lectura (`CU-03`, `RF-030`, `RF-032`).<br>• Mantiene los criterios individuales de filtrado multidimensional activos en `CU-05` (`desarrollador`, `módulo financiero`, `etapa oficial`, `rango de fechas`), permitiendo su retiro selectivo sin reiniciar la consulta (`RF-008`, `RF-009`, `RF-028`, `RF-039`) y preservándolos durante los refrescos incrementales de 5 minutos (Flujo `4b`).<br>• Confirma la consolidación de alertas gestionadas al cierre (`RF-027`) y libera canales/memoria en **`< 200 ms`** con `0` conexiones huérfanas (`RNF-011`). | • `ExpedienteModuloFinanciero`<br>• `AlertaOperativa`<br>• `ReporteTrimestral`<br>• `PerfilAccesoUnico` |

| Tarjeta CRC #16: `ReporteTrimestral` | |
|---|---|
| **Responsabilidades (Qué sabe y qué hace)** | **Colaboradores Directos** |
| • Consolida para el trimestre seleccionado los totales ejecutivos de proyectos `POA/PETI`, requerimientos `REQ`, iniciativas `INI.TI` e incidentes `GLPI` atendidos (`CU-08`, `RF-040`).<br>• Calcula el porcentaje global de cumplimiento y el comparativo de certificaciones cerradas a tiempo (`<= 5 días hábiles`) frente a las atrasadas (`> 5 días hábiles`) con respaldo matemático auditable (`RNF-001`, `RNF-002`). | • `SesionInstitucional`<br>• `ExpedienteModuloFinanciero`<br>• `RegistroAuditoriaCalculo` |

---

### 2.2 Justificación de Decisiones Arquitectónicas Estructurales

1. **Patrón de Roles por Asociación vs. Herencia Rígida (`Desarrollador` + `AsignacionActividad`):**
   - **Problema:** En la operación diaria de la COAC Padre Julián Lorente, un mismo ingeniero puede estar asignado como responsable de una tarea en `Redmine`, ser autor de commits en `GitLab`, actuar simultáneamente como **revisor de QA cruzado** (`+2 h / 1 pt` bajo `RF-003`) en el Merge Request de un compañero y atender un ticket de soporte en `GLPI`. Además, en el Flujo Alterno `7b` (`RF-017`), el autor real de los commits puede diferir del desarrollador asignado en `Redmine`.
   - **Decisión:** Se descarta crear subclases por herencia rígida (`DesarrolladorAsignado`, `RevisorQA`, `AutorCodigo`) porque violaría el principio de sustitución dinámica y obligaría a duplicar instancias de la misma persona. En su lugar, se modela la **Clase Asociativa `AsignacionActividad`** con el atributo `rolDesempenado : RolParticipacionDesarrollo`. Esto permite computar con precisión la carga semanal (`RF-003`) y auditar divergencias de autoría (`RF-017`) manteniendo una única identidad por `Desarrollador`.
2. **Unificación Polimórfica Limpia de la Jerarquía de 4 Niveles y las Actividades Segregadas sin Trazabilidad (`RF-033`, `RF-021`, `RF-022`):**
   - **Problema:** El sistema exige enlazar los 4 niveles (`EpicaPOA` ➔ `HistoriaEspecificacion` ➔ `ActividadTrazable` ➔ `EvidenciaCodigo`), pero los Flujos Alternos `6a` (`RF-021`) y `6b` (`RF-022`) exigen que los commits/ramas en `GitLab` sin tarea en `Redmine` y los incidentes en `GLPI` sin espejo en `Redmine` **no se descarten**, sino que se segreguen explícitamente, sumen horas a la carga del desarrollador y puedan auditarse en `CU-06` (Flujo `2a`).
   - **Decisión:** La relación entre `HistoriaEspecificacion` y `ActividadTrazable` es una **Agregación (`0..1` a `0..*`)**, mientras que `ExpedienteModuloFinanciero` consolida todas las instancias de `ActividadTrazable` (`1` a `0..*`). Cuando una actividad nace huérfana en `GitLab` o `GLPI`, su multiplicidad hacia `HistoriaEspecificacion` es `0`, se clasifica en `tipoInconsistencia` como `TRABAJO_CODIGO_SIN_TRAZABILIDAD_PLANIFICADA` o `INCIDENTE_SOPORTE_SIN_TRAZABILIDAD_DESARROLLO`, y se vincula igualmente al `Desarrollador` vía `AsignacionActividad`.
3. **Blindaje de Snapshot Parcial por Fuente (`SnapshotFuenteExterna`, `RNF-008`):**
   - **Problema:** Si una sola fuente externa (ej. `GitLab`) supera los `10 segundos` y falla tras los `3 reintentos` (Flujo Alterno `2a` de `CU-01`), un diseño ingenuo marcaría todas las tareas de `Redmine` como «sin respaldo en código (`RF-038`)».
   - **Decisión:** `CicloSincronizacion` mantiene hasta 4 instancias de `SnapshotFuenteExterna` (`1..4`). Cuando una fuente cae temporalmente, se reutiliza su último `SnapshotFuenteExterna` válido (`esReutilizadoPorContingencia = verdadero`), preservando intactos los cruces de `EvidenciaCodigo` y registrando el estado del ciclo como `CONSOLIDADO_PARCIAL_CON_SNAPSHOT`.

---

## 3. Diccionario de Clases Completo (5 Campos Obligatorios por Clase)

A continuación se documentan las **16 clases del dominio** con los **5 campos obligatorios** exigidos por la Guía Maestra (`DSOF_4069`). Todas las firmas públicas (`+`) utilizan tipos abstractos (`Texto`, `Entero`, `Decimal`, `Booleano`, `FechaHora`, `Duracion`, `Lista<T>`, `Mapa<K,V>`, `Vacio`) y constituyen el contrato exacto `1:1` para la Vista de Procesos (Semana 5).

---

### 3.1 Clase 01: `CicloSincronizacion` (`«resilience_gate»`)

1. **Nombre y Propósito:**  
   `CicloSincronizacion` — Representa cada ejecución periódica del proceso ETL disparada automáticamente cada 5 minutos por `Tiempo (Temporizador CRON)`, controlando el modo de solo lectura, la contingencia por snapshots parciales y la consolidación atómica e idempotente.
2. **Atributos y Restricciones:**
   - `- codigoCiclo : Texto` — Obligatorio, identificador único del ciclo (ej. `"SYNC-20261115-1500"`).
   - `- fechaHoraInicio : FechaHora` — Obligatorio, instante exacto de disparo CRON.
   - `- fechaHoraFin : FechaHora [0..1]` — Opcional hasta concluir o abortar el ciclo.
   - `- intervaloProgramadoMinutos : Entero = 5` — Obligatorio, constante operativa (`5 min`, `RF-029`).
   - `- modoSoloLecturaVerificado : Booleano = verdadero` — Obligatorio, invariante `verdadero` (`RF-011`, `RNF-003`).
   - `- estadoCiclo : EstadoCicloSincronizacion` — Obligatorio (`INICIADO`, `EXTRAYENDO_SOLO_LECTURA`, `SINCRONIZACION_PARCIAL_SNAPSHOT`, `VALIDANDO_COMPUERTA_LOPDP`, `CONSOLIDADO_EXITOSO`, `ABORTADO_FALLO_TOTAL`, `ABORTADO_RIESGO_PII`, `REVERTIDO_POR_CONFLICTO`).
   - `- /duracionTotalSegundos : Decimal` — **Derivado (`/`)**: diferencia en segundos entre `fechaHoraFin` y `fechaHoraInicio`.
   - `- /porcentajeFuentesDisponibles : Decimal` — **Derivado (`/`)**: porcentaje (`0.0..100.0`) de las 4 fuentes externas extraídas en vivo en el ciclo actual.
3. **Operaciones (Entrada / Salida):**
   - `+ iniciarCicloPeriodico(marcaTemporalDisparo : FechaHora) : CicloSincronizacion` — Inicializa un nuevo ciclo cada 5 minutos verificando credenciales de solo lectura.
   - `+ registrarExtraccionFuente(fuente : FuenteOperativaExterna, snapshot : SnapshotFuenteExterna) : Booleano` — Asocia el snapshot extraído de una plataforma origen dentro del límite de `10 s`.
   - `+ activarContingenciaSnapshotParcial(fuenteCaida : FuenteOperativaExterna, ultimoSnapshotValido : SnapshotFuenteExterna) : EstadoCicloSincronizacion` — Reutiliza el último snapshot válido de una plataforma caída tras 3 reintentos sin degradar actividades (Flujo Alterno `2a`).
   - `+ abortarPorFalloTotalFuentes(motivo : Texto) : Vacio` — Registra el aborto del ciclo cuando las 4 fuentes caen o fallan credenciales sin sobreescribir datos previos (Excepción `2b`).
   - `+ consolidarLoteIdempotente(expedientes : Lista<ExpedienteModuloFinanciero>) : Booleano` — Persiste atómicamente el lote consolidado garantizando `0 %` de duplicados (`RNF-009`).
   - `+ revertirLotePorConflicto(detalleError : Texto) : Vacio` — Ejecuta rollback completo del lote ante error de integridad (Excepción `8a`).
4. **Invariantes / Reglas de Negocio:**
   - **Solo lectura absoluta (`RF-011`, `RNF-003`):** `modoSoloLecturaVerificado` jamás puede ser `falso`; el ciclo tiene prohibido emitir comandos de escritura, actualización o borrado hacia `Redmine`, `GLPI`, `GitLab` o `SonarQube`.
   - **Prohibición de degradación falsa en sincronización parcial (`RNF-008`, Flujo `2a`):** Si `estadoCiclo == SINCRONIZACION_PARCIAL_SNAPSHOT` (al menos 1 fuente activa y hasta 3 caídas con snapshot previo válido), ninguna actividad de la fuente caída puede reclasificarse como «sin trazabilidad» ni «sin respaldo en código».
   - **Atomicidad e idempotencia (`RNF-009`, Excepción `8a`):** O se persiste el lote completo con `0 %` de duplicados, o el estado pasa a `REVERTIDO_POR_CONFLICTO` manteniendo intacta la línea consolidada anterior.
5. **Casos de Uso Trazados:**  
   `CU-01` (Principal), `CU-02` (Habilita punto de extensión al finalizar paso 8), `CU-04` (Provee marca temporal de última sincronización exitosa en Flujo `2a` y detección de ausencia inicial en `2b`).

---

### 3.2 Clase 02: `SnapshotFuenteExterna` (`«resilience_gate»`)

1. **Nombre y Propósito:**  
   `SnapshotFuenteExterna` — Almacena la instantánea verificada de extracción de cada una de las cuatro plataformas externas (`Redmine`, `GLPI`, `GitLab`, `SonarQube`), permitiendo aislar fallos externos y reutilizar el último estado válido cuando una herramienta supera el timeout de `10 segundos` y los `3 reintentos`.
2. **Atributos y Restricciones:**
   - `- codigoSnapshot : Texto` — Obligatorio, único por fuente y ciclo.
   - `- plataformaOrigen : FuenteOperativaExterna` — Obligatorio (`REDMINE`, `GLPI`, `GITLAB`, `SONARQUBE`).
   - `- fechaHoraUltimaExtraccionExitosa : FechaHora` — Obligatorio (`RNF-008`).
   - `- tiempoRespuestaSegundos : Decimal` — Obligatorio, restricción `0.0 < tiempoRespuestaSegundos <= 10.0` en extracciones exitosas (`RNF-008`).
   - `- numeroReintentosEjecutados : Entero = 0` — Obligatorio, rango permitido `0..3` (`RNF-008`).
   - `- totalRegistrosExtraidos : Entero` — Obligatorio, `>= 0`.
   - `- huellaIntegridadLote : Texto` — Obligatorio, resumen criptográfico/lógico de los registros leídos.
   - `- /esReutilizadoPorContingencia : Booleano` — **Derivado (`/`)**: `verdadero` si fue recuperado de un ciclo anterior por caída de la fuente en el ciclo actual (Flujo Alterno `2a`).
3. **Operaciones (Entrada / Salida):**
   - `+ validarTimeoutYReintentos(segundosConsumidos : Decimal, intentos : Entero) : Booleano` — Verifica si la extracción se completó dentro del presupuesto de `10 s` y máximo `3` reintentos.
   - `+ marcarComoSnapshotContingencia(cicloActual : Texto) : SnapshotFuenteExterna` — Marca la instantánea previa como activa por contingencia conservando su `fechaHoraUltimaExtraccionExitosa` original.
   - `+ verificarFrescuraDatos(instanteConsulta : FechaHora) : Duracion` — Calcula la antigüedad del snapshot para exponerla en el tablero durante el Flujo Alterno `2a` de `CU-04`.
4. **Invariantes / Reglas de Negocio:**
   - **Presupuesto estricto de espera externa (`RNF-008`, `RNF-016`):** Ninguna llamada de extracción puede superar `10.0 segundos` por intento ni exceder `3 reintentos automáticos`.
   - **Trazabilidad de marca temporal (`RNF-008`):** Cuando `esReutilizadoPorContingencia == verdadero`, `fechaHoraUltimaExtraccionExitosa` conserva de forma inmutable la fecha/hora real de la última lectura exitosa, nunca la fecha del ciclo fallido.
5. **Casos de Uso Trazados:**  
   `CU-01` (Paso 2, Flujo Alterno `2a`, Excepción `2b`), `CU-04` (Flujo Alterno `2a`).

---

### 3.3 Clase 03: `AuditorCompuertaLOPDP` (`«resilience_gate»`)

1. **Nombre y Propósito:**  
   `AuditorCompuertaLOPDP` — Materializa la compuerta obligatoria de Privacidad por Diseño (Ley Orgánica de Protección de Datos Personales — LOPDP Ecuador 2021) que inspecciona, enmascara y certifica la ausencia del `100 %` de datos personales sensibles de socios antes de permitir cualquier análisis o persistencia.
2. **Atributos y Restricciones:**
   - `- codigoVerificacion : Texto` — Obligatorio, identificador único de la certificación LOPDP del lote.
   - `- fechaHoraAuditoria : FechaHora` — Obligatorio.
   - `- totalTextosInspeccionados : Entero` — Obligatorio, `>= 0`.
   - `- totalPatronesPIIEnmascarados : Entero = 0` — Obligatorio, conteo de cédulas, números de cuenta y correos particulares de socios anonimizados (`RNF-017`).
   - `- mesesRetencionHistorica : Entero = 24` — Obligatorio, restricción `>= 24 meses` (`RNF-017`).
   - `- /porcentajeCumplimientoAnonimizacion : Decimal = 100.0` — **Derivado (`/`)**: proporción de textos libres de PII tras el enmascaramiento (`0.0..100.0 %`).
   - `- compuertaAprobada : Booleano` — Obligatorio, `verdadero` solo si `/porcentajeCumplimientoAnonimizacion == 100.0`.
3. **Operaciones (Entrada / Salida):**
   - `+ depurarYEnmascararPII(loteCrudo : Lista<Texto>) : Lista<Texto>` — Aplica reglas de anonimización sobre descripciones de tickets GLPI, tareas Redmine y mensajes de commits.
   - `+ verificarCeroExposicionPII() : Booleano` — Audita que no sobreviva ningún patrón de cédula, cuenta bancaria o correo personal de socio en el lote depurado.
   - `+ abortarLotePorRiesgoPII(motivoCritico : Texto) : Vacio` — Detiene de inmediato el procesamiento antes de persistir si falla la verificación del 100 % (Excepción `3a` de `CU-01`).
   - `+ evaluarVigenciaRetencion24Meses(fechaRegistro : FechaHora, fechaActual : FechaHora) : Booleano` — Verifica si un expediente se encuentra dentro de la ventana histórica obligatoria de `24 meses` (`RNF-017`, Excepción `2b` de `CU-06`).
4. **Invariantes / Reglas de Negocio:**
   - **Tolerancia cero a exposición de PII (`RNF-017`, Excepción `3a` de `CU-01` y `4a` de `CU-06`):** Ningún expediente, alerta o reporte puede persistirse ni visualizarse si `compuertaAprobada == falso` o `/porcentajeCumplimientoAnonimizacion < 100.0`.
   - **Retención técnica mínima (`RNF-017`):** Los expedientes técnicos anonimizados deben conservarse al menos `24 meses` (`mesesRetencionHistorica >= 24`).
5. **Casos de Uso Trazados:**  
   `CU-01` (Paso 3 y Excepción `3a`), `CU-06` (Excepciones `2b` y `4a`).

---

### 3.4 Clase 04: `ExpedienteModuloFinanciero` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `ExpedienteModuloFinanciero` — Unidad agregadora central del dominio que consolida cronológicamente las épicas, especificaciones, tareas, incidentes, evidencias de código y métricas operativas de cada uno de los **6 módulos del core financiero** de la cooperativa.
2. **Atributos y Restricciones:**
   - `- codigoExpediente : Texto` — Obligatorio, único por módulo financiero.
   - `- moduloCore : ModuloFinancieroCore` — Obligatorio (`FITSWITCH`, `SWI`, `CANALES_ELECTRONICOS`, `CREDITO`, `ANTIFRAUDE`, `NOMINA` — `RF-012`, `RF-018`).
   - `- fechaUltimaConsolidacion : FechaHora` — Obligatorio.
   - `- /totalActividadesActivas : Entero` — **Derivado (`/`)**: conteo de actividades vigentes asociadas al módulo.
   - `- /indiceCalorCritico : Decimal` — **Derivado (`/`)**: indicador compuesto (`0.0..100.0`) de atrasos, sobrecargas y hallazgos críticos para el mapa de calor (`RF-007`).
   - `- /distribucionEtapas : Mapa<EtapaOficialFlujo, Entero>` — **Derivado (`/`)**: total de actividades en cada una de las 5 etapas oficiales para el embudo (`RF-019`, `RF-036`).
3. **Operaciones (Entrada / Salida):**
   - `+ unificarExpedienteCronologico(actividades : Lista<ActividadTrazable>) : ExpedienteModuloFinanciero` — Consolida ordenadamente la cadena de actividades del módulo financiero (`RF-018`, `RF-020`).
   - `+ calcularMapaCalorModulo() : Decimal` — Calcula la densidad de riesgo operativo del módulo (`RF-007`).
   - `+ detectarCuellosDeBotellaEmbudo() : Lista<EtapaOficialFlujo>` — Identifica etapas que concentran `> 35 %` de tareas activas o superan umbrales de permanencia (`RF-036`).
   - `+ reconstruirCadenaCausal(codigoElemento : Texto) : ExpedienteModuloFinanciero` — Devuelve el desglose cronológico de 4 niveles (o el expediente parcial en `RF-021`/`RF-022`) ordenado por criticidad para toma de decisiones en `< 10 s` (`CU-06`, `RF-010`, `RNF-007`).
4. **Invariantes / Reglas de Negocio:**
   - **Clasificación obligatoria en los 6 módulos oficiales (`RF-012`, `RF-018`):** Todo expediente pertenece exactamente a uno de los 6 valores canónicos de `ModuloFinancieroCore`.
   - **Detección de cuellos de botella en embudo (`RF-036`, `RNF-005`):** Una etapa del embudo se marca en estado crítico rojo si concentra `> 35.0 %` de las actividades activas o contiene tareas con `> 5 días hábiles` en `CERTIFICACION` o `> 2 días` en `DESPLIEGUE`.
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 4, 6 y 8), `CU-04` (Pasos 2 y 3), `CU-05` (Filtrado por módulo financiero), `CU-06` (Pasos 2 a 5, Flujo `2a`), `CU-08` (Insumo de agregación trimestral).

---

### 3.5 Clase 05: `EpicaPOA` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `EpicaPOA` — Representa el **Nivel 1** de la línea base jerárquica (`Épica POA/PETI`), agrupando las historias/especificaciones de un proyecto estratégico o anual y permitiendo comparar su avance planificado contra el avance real verificado en código.
2. **Atributos y Restricciones:**
   - `- codigoEpica : Texto` — Obligatorio, identificador oficial `POA/PETI` (`RF-033`).
   - `- nombreProyecto : Texto` — Obligatorio.
   - `- prioridadEstrategica : Entero` — Obligatorio, nivel de prioridad institucional (`RF-025`).
   - `- fechaInicioPlanificada : FechaHora` — Obligatorio (`RF-012`).
   - `- fechaFinPlanificada : FechaHora` — Obligatorio, restricción `fechaFinPlanificada >= fechaInicioPlanificada` (`RF-012`).
   - `- porcentajeAvancePlanificado : Decimal` — Obligatorio, rango `0.0..100.0` (`RF-004`, `RF-036`).
   - `- /porcentajeAvanceRealVerificado : Decimal` — **Derivado (`/`)**: avance real (`0.0..100.0`) calculado exclusivamente a partir del progreso respaldado en código (`GitLab`/`SonarQube`) de sus historias y tareas (`RF-004`, `RF-036`, `RF-037`).
   - `- /desviacionAvancePuntos : Decimal` — **Derivado (`/`)**: diferencia entre `/porcentajeAvanceRealVerificado` y `porcentajeAvancePlanificado`.
3. **Operaciones (Entrada / Salida):**
   - `+ consolidarAvanceRealVsPlanificado() : Decimal` — Recalcula el avance real ponderado por puntos Fibonacci de sus especificaciones y tareas (`RF-004`, `RF-036`).
   - `+ registrarCambioPrioridad(nuevaPrioridad : Entero) : Booleano` — Detecta cambios de prioridad del proyecto que detonan notificación por correo corporativo en `CU-02` (`RF-025`).
   - `+ obtenerDesgloseHistoriasYTareas() : Lista<HistoriaEspecificacion>` — Entrega la estructura descendente de Nivel 2 y Nivel 3 para el tablero (`RF-036`) y la auditoría causal (`RF-010`).
4. **Invariantes / Reglas de Negocio:**
   - **Dependencia de evidencia en código (`RF-004`, `RF-037`, `RF-038`):** `/porcentajeAvanceRealVerificado` nunca puede tomar directamente porcentajes subjetivos digitados en `Redmine` que carezcan de validación en `EvidenciaCodigo`.
   - **Composición jerárquica (`RF-033`):** Toda `EpicaPOA` contiene al menos una `HistoriaEspecificacion` (`1..*`).
5. **Casos de Uso Trazados:**  
   `CU-01`, `CU-02` (Disparador por cambio de prioridad `RF-025`), `CU-04` (`RF-032`, `RF-036`), `CU-05`, `CU-06` (`RF-010`), `CU-08` (`RF-040`).

---

### 3.6 Clase 06: `HistoriaEspecificacion` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `HistoriaEspecificacion` — Representa el **Nivel 2** de la jerarquía oficial (`Historia / Especificación Funcional`), distinguiendo entre requerimientos formales (`REQ`) e iniciativas internas del área de tecnología (`INI.TI`).
2. **Atributos y Restricciones:**
   - `- codigoEspecificacion : Texto` — Obligatorio (`REQ-xxx` o `INI.TI-xxx`, `RF-033`, `RF-040`).
   - `- tipoRequerimiento : TipoEspecificacionOrigen` — Obligatorio (`REQ_REQUERIMIENTO` o `INI_TI_INICIATIVA`, `RF-033`, `RF-040`).
   - `- tituloEspecificacion : Texto` — Obligatorio.
   - `- horasEstimadasLineaBase : Decimal` — Obligatorio, `> 0.0` (`RF-012`).
   - `- fechaCompromisoEntrega : FechaHora` — Obligatorio (`RF-012`).
   - `- /porcentajeCumplimientoHistoria : Decimal` — **Derivado (`/`)**: avance verificado consolidado de sus tareas asociadas (`0.0..100.0`).
3. **Operaciones (Entrada / Salida):**
   - `+ vincularTareaPlanificada(tarea : ActividadTrazable) : Booleano` — Enlaza una tarea `#ID` de `Redmine` dentro de la cadena jerárquica (`RF-016`, `RF-033`).
   - `+ calcularAvancePonderadoTareas() : Decimal` — Calcula el cumplimiento real de la especificación a partir de sus `ActividadTrazable`.
4. **Invariantes / Reglas de Negocio:**
   - **Pertenencia estricta a una Épica (`RF-033`):** Toda `HistoriaEspecificacion` existe en composición (`*--`) bajo exactamente una `EpicaPOA` (`1`).
   - **Conteo diferenciado para reporte ejecutivo (`RF-040`):** Debe clasificar sin ambigüedad si corresponde a `REQ_REQUERIMIENTO` o `INI_TI_INICIATIVA` para el informe trimestral de `CU-08`.
5. **Casos de Uso Trazados:**  
   `CU-01`, `CU-04`, `CU-05`, `CU-06`, `CU-08`.

---

### 3.7 Clase 07: `ActividadTrazable` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `ActividadTrazable` — Entidad operativa nuclear del **Nivel 3** que representa cada tarea planificada (`Tarea/Issue #ID` en `Redmine`) así como las actividades emergentes segregadas sin trazabilidad completa (`RF-021` y `RF-022`), concentrando la etapa oficial vigente, la complejidad Fibonacci, la verificación de respaldo en código y el conteo de retornos por fallo.
2. **Atributos y Restricciones:**
   - `- llaveNaturalCompuesta : Texto` — Obligatorio, único e inmutable (`Épica POA + Historia/Spec + Tarea #ID + Ticket GLPI + Commit`, `RNF-009`).
   - `- codigoTareaRedmine : Texto [0..1]` — Opcional (`#ID` presente en tareas planificadas; ausente cuando `tipoInconsistencia == TRABAJO_CODIGO_SIN_TRAZABILIDAD_PLANIFICADA` o `INCIDENTE_SOPORTE_SIN_TRAZABILIDAD_DESARROLLO`, `RF-021`, `RF-022`).
   - `- tituloActividad : Texto` — Obligatorio (texto depurado por `AuditorCompuertaLOPDP`, `RNF-017`).
   - `- etapaOficialActual : EtapaOficialFlujo` — Obligatorio (`ANALISIS`, `DESARROLLO`, `CERTIFICACION`, `DESPLIEGUE`, `ESTABILIZACION`, `RF-019`).
   - `- complejidadFibonacci : NivelComplejidadFibonacci` — Obligatorio (`NIVEL_1_BAJA_2_PTS`, `NIVEL_2_MEDIA_5_PTS`, `NIVEL_3_ALTA_13_PTS`, `RF-001`).
   - `- horasEstimadas : Decimal` — Obligatorio, `>= 0.0` (`RF-012`, `RF-003`).
   - `- porcentajeReportadoRedmine : Decimal = 0.0` — Obligatorio, rango `0.0..100.0` (`RF-038`).
   - `- /avanceRealVerificado : Decimal` — **Derivado (`/`)**: progreso real (`0.0..100.0`) validado contra `EvidenciaCodigo` (`RF-004`, `RF-037`, `RF-038`).
   - `- diasHabilesEnEtapaActual : Decimal = 0.0` — Obligatorio, `>= 0.0` (`RF-031`, `RF-036`).
   - `- contadorRetornosPorFallo : Entero = 0` — Obligatorio, `>= 0`, se incrementa cada vez que retrocede a «En curso» en `DESARROLLO` desde pruebas CI/CD o `CERTIFICACION` (`RF-015`).
   - `- tipoInconsistencia : TipoInconsistenciaTrazabilidad` — Obligatorio (`NINGUNA_TRAZABILIDAD_COMPLETA`, `TRABAJO_CODIGO_SIN_TRAZABILIDAD_PLANIFICADA`, `INCIDENTE_SOPORTE_SIN_TRAZABILIDAD_DESARROLLO`, `BRECHA_EVIDENCIA_AVANCE`, `DIVERGENCIA_AUTOR_ASIGNADO`).
   - `- /tieneBrechaEvidencia : Booleano` — **Derivado (`/`)**: `verdadero` si `porcentajeReportadoRedmine > 25.0` sin rama activa ni commits tipados, o `porcentajeReportadoRedmine == 100.0` sin Merge Request aprobado ni CI/CD exitoso (`RF-038`, Flujo `7a`).
   - `- /esCuelloDeBotella : Booleano` — **Derivado (`/`)**: `verdadero` si permanece `> 5.0 días hábiles` en `CERTIFICACION` o `> 2.0 días` en `DESPLIEGUE` (`RF-036`).
3. **Operaciones (Entrada / Salida):**
   - `+ normalizarEtapaOficial(estadoOrigen : Texto, tieneMRAprobado : Booleano, estadoCICD : Booleano) : EtapaOficialFlujo` — Traduce los estados heterogéneos de Redmine/GitLab a una de las 5 etapas oficiales (`RF-019`).
   - `+ verificarRespaldoAvanceEnCodigo(evidencia : EvidenciaCodigo) : Decimal` — Contrasta `porcentajeReportadoRedmine` contra `evidencia`; si detecta brecha (`> 25 %` o `100 %` sin respaldo), activa `/tieneBrechaEvidencia` y retorna el avance verificable en código (`RF-038`, Flujo `7a`).
   - `+ registrarRetornoPorFallo(etapaOrigen : EtapaOficialFlujo, motivoRechazo : Texto) : Entero` — Incrementa `contadorRetornosPorFallo` cuando retrocede a `DESARROLLO` y habilita la penalización de `-10 pts` en calidad (`RF-015`, `RF-005`, Flujo `5b`).
   - `+ clasificarActividadSinTrazabilidad(origen : FuenteOperativaExterna) : TipoInconsistenciaTrazabilidad` — Segrega actividades de código sin tarea en Redmine (`RF-021`, Flujo `6a`) o incidentes GLPI sin espejo en Redmine (`RF-022`, Flujo `6b`).
   - `+ enlazarTicketGLPITolerante(referenciaTexto : Texto, ticket : TicketIncidenteGLPI) : Booleano` — Ejecuta el cruce tolerante a errores tipográficos con `TicketIncidenteGLPI` (`RF-013`, Flujo `5a`).
   - `+ evaluarUmbralAtrasoCertificacion(limiteDiasHabiles : Decimal) : Booleano` — Evalúa si `etapaOficialActual == CERTIFICACION` y `diasHabilesEnEtapaActual > 5.0` para disparar alerta en `CU-02` (`RF-031`, `RF-025`) y marcar atraso en `CU-08` (`RF-040`).
4. **Invariantes / Reglas de Negocio:**
   - **Idempotencia estricta (`RNF-009`):** No pueden existir dos instancias de `ActividadTrazable` con la misma `llaveNaturalCompuesta` (`0 %` de duplicados).
   - **Regla de Evidencia de Avance (`RF-038`, Flujo Alterno `7a`):**
     - Si `porcentajeReportadoRedmine > 25.0` y `(evidencia == nulo O evidencia.cumpleEvidenciaMas25Porciento == falso)`, entonces `/tieneBrechaEvidencia = verdadero` y `/avanceRealVerificado <= 25.0`.
     - Si `porcentajeReportadoRedmine == 100.0` y `(evidencia == nulo O evidencia.cumpleEvidencia100Porciento == falso)`, entonces `/tieneBrechaEvidencia = verdadero` y `/avanceRealVerificado < 100.0`.
   - **Escala cerrada Fibonacci (`RF-001`):** `complejidadFibonacci` solo admite los valores equivalentes a `2`, `5` o `13` puntos (o `3` / `5` puntos provenientes de incidentes `GLPI` según `RF-003`).
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 4–8, Flujos `5a`, `5b`, `6a`, `6b`, `7a`, `7b`), `CU-02` (`RF-031`, `RF-025`), `CU-04` (`RF-007`, `RF-035`, `RF-036`), `CU-05` (`RF-009`), `CU-06` (`RF-010`, Flujos `2a`, `3a`), `CU-08` (`RF-040`).

---

### 3.8 Clase 08: `TicketIncidenteGLPI` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `TicketIncidenteGLPI` — Modela cada ticket de incidente operativo o de soporte extraído en modo solo lectura desde `GLPI`, gestionando su correlación tolerante con `Redmine` y el cálculo automático de carga cuando carece de estimación manual.
2. **Atributos y Restricciones:**
   - `- codigoTicketGLPI : Texto` — Obligatorio (`GLPI #ID`).
   - `- referenciaNormalizada : Texto` — Obligatorio, código canónico depurado de espacios, guiones o errores tipográficos (`RF-013`).
   - `- esCriticoCoreFinanciero : Booleano` — Obligatorio (`RF-003`).
   - `- esPrioridadUno : Booleano` — Obligatorio, detona notificación por correo corporativo en `CU-02` (`RF-025`).
   - `- tieneEstimacionManual : Booleano` — Obligatorio (`RF-003`).
   - `- horasEstimadasManuales : Decimal [0..1]` — Opcional, presente solo si `tieneEstimacionManual == verdadero`.
   - `- /horasCargaEstandarAplicadas : Decimal` — **Derivado (`/`)**: si `tieneEstimacionManual == falso`, vale **`8.0 h`** cuando `esCriticoCoreFinanciero == verdadero` y **`4.0 h`** cuando es incidente estándar (`RF-003`).
   - `- /puntosFibonacciEquivalentes : Entero` — **Derivado (`/`)**: **`5 puntos`** para incidente crítico del core financiero y **`3 puntos`** para incidente estándar (`RF-003`).
   - `- /tieneEspejoEnRedmine : Booleano` — **Derivado (`/`)**: `verdadero` si enlazó con una tarea `#ID` en `Redmine`; si es `falso`, detona clasificación `INCIDENTE_SOPORTE_SIN_TRAZABILIDAD_DESARROLLO` (`RF-022`, Flujo `6b`).
3. **Operaciones (Entrada / Salida):**
   - `+ normalizarReferenciaDigitada(textoCrudoRedmine : Texto) : Texto` — Extrae y corrige patrones `GLPI #ID` con errores tipográficos en `Redmine` (`RF-013`, Flujo `5a`).
   - `+ calcularEsfuerzoSoporteAutomatico() : Decimal` — Retorna las horas (`4.0 h` u `8.0 h`) y puntos (`3 pts` o `5 pts`) aplicables a la carga semanal del responsable (`RF-003`).
   - `+ verificarExistenciaEspejoRedmine() : Booleano` — Comprueba si el ticket activo posee petición espejo en `Redmine` (`RF-016`, `RF-022`).
4. **Invariantes / Reglas de Negocio:**
   - **Regla de Esfuerzo Automático GLPI (`RF-003`):** Todo ticket activo sin estimación manual asigna exactamente `4.0 h / 3 pts` (estándar) u `8.0 h / 5 pts` (crítico del core financiero) a la carga semanal de su responsable, incluso si no tiene espejo en `Redmine` (`RF-022`).
   - **Exclusión en fórmula de eficiencia (`RF-005`):** Las horas emergentes de `TicketIncidenteGLPI` sí suman a la ocupación semanal sobre `40 h` (`RF-003`), pero se excluyen del denominador de avance de proyecto en la fórmula de eficiencia (`RF-005`).
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 5–7, Flujos `5a`, `6b`), `CU-02` (`RF-025` Prioridad 1), `CU-04` (`RF-035`), `CU-06` (`RF-010`, Flujo `2a`), `CU-08` (`RF-040`).

---

### 3.9 Clase 09: `EvidenciaCodigo` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `EvidenciaCodigo` — Representa el **Nivel 4** de la jerarquía oficial (`Rama`, `Merge Request`, `Commits tipados` y `Pipeline CI/CD` en `GitLab`), proporcionando la evidencia técnica objetiva que respalda el avance real de cada actividad.
2. **Atributos y Restricciones:**
   - `- nombreRepositorio : Texto` — Obligatorio.
   - `- nombreRamaActiva : Texto [0..1]` — Opcional (`RF-014`, `RF-038`).
   - `- identificadorMergeRequest : Texto [0..1]` — Opcional (`RF-014`, `RF-038`).
   - `- mergeRequestAprobado : Booleano = falso` — Obligatorio (`RF-038`).
   - `- pipelineCICDExitoso : Booleano = falso` — Obligatorio (`RF-014`, `RF-015`, `RF-038`).
   - `- conteoCommitsPorTipo : Mapa<TipoCommitTipado, Entero>` — Obligatorio, desglosa la cantidad de commits `FEAT`, `FIX`, `TEST` y `REFACTOR` que referencian `#ID` (`RF-014`).
   - `- /totalCommitsTipadosValidos : Entero` — **Derivado (`/`)**: suma de los 4 tipos de commits tipados válidos.
   - `- /cumpleEvidenciaMas25Porciento : Booleano` — **Derivado (`/`)**: `verdadero` si existe `nombreRamaActiva` y `/totalCommitsTipadosValidos >= 1` (`RF-038`).
   - `- /cumpleEvidencia100Porciento : Booleano` — **Derivado (`/`)**: `verdadero` si `mergeRequestAprobado == verdadero` y `pipelineCICDExitoso == verdadero` (`RF-038`).
3. **Operaciones (Entrada / Salida):**
   - `+ contabilizarCommitsTipados(mensajesCommit : Lista<Texto>) : Mapa<TipoCommitTipado, Entero>` — Clasifica y cuenta los commits según el estándar `feat(#ID)`, `fix(#ID)`, `test(#ID)` y `refactor(#ID)` (`RF-014`).
   - `+ validarRequisitosAvance25Porciento() : Booleano` — Evalúa si existe rama activa y al menos un commit tipado válido (`RF-038`).
   - `+ validarRequisitosCierre100Porciento() : Booleano` — Evalúa si el Merge Request está aprobado y el pipeline CI/CD aprobó sin errores (`RF-038`).
4. **Invariantes / Reglas de Negocio:**
   - **Taxonomía estricta de commits (`RF-014`, `RF-033`):** Solo los commits que cumplen la gramática `feat(#ID)`, `fix(#ID)`, `test(#ID)` o `refactor(#ID)` acreditan respaldo de avance planificado; los commits sin `#ID` generan una `ActividadTrazable` segregada bajo `RF-021` (Flujo `6a`).
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 4–7, Flujos `6a`, `7a`, `7b`), `CU-04` (`RF-036`), `CU-05` (`RF-009`), `CU-06` (`RF-010`, Flujos `2a`, `3a`).

---

### 3.10 Clase 10: `HallazgoCalidadSonar` (`«core_hierarchy»`)

1. **Nombre y Propósito:**  
   `HallazgoCalidadSonar` — Representa cada defecto técnico severo (`Bug Crítico` o `Vulnerabilidad`) detectado por `SonarQube` sobre el código asociado a una actividad, impactando directamente en el componente del `30 %` de calidad técnica de la eficiencia.
2. **Atributos y Restricciones:**
   - `- codigoHallazgo : Texto` — Obligatorio, identificador único en `SonarQube`.
   - `- categoriaHallazgo : Texto` — Obligatorio (`BUG_CRITICO` o `VULNERABILIDAD`, `RF-005`).
   - `- severidadSonar : Texto` — Obligatorio (`CRITICAL` / `BLOCKER`).
   - `- penalizacionCalidadPuntos : Decimal = 5.0` — Obligatorio, constante normativa de **`5.0 puntos`** de descuento por cada hallazgo (`RF-005`).
   - `- vigenteAbierto : Booleano = verdadero` — Obligatorio.
   - `- fechaDeteccion : FechaHora` — Obligatorio.
3. **Operaciones (Entrada / Salida):**
   - `+ calcularPenalizacionHallazgo() : Decimal` — Devuelve `5.0` si `vigenteAbierto == verdadero`, o `0.0` si fue resuelto (`RF-005`).
   - `+ vincularConEvidenciaCodigo(evidencia : EvidenciaCodigo) : Booleano` — Enlaza el reporte estático de `SonarQube` con la rama/MR correspondiente (`RF-014`).
4. **Invariantes / Reglas de Negocio:**
   - **Descuento exacto en calidad técnica (`RF-005`):** Cada bug crítico o vulnerabilidad abierta descuenta exactamente `5 puntos` sobre la base de `100` del componente de calidad técnica (`30 %` de la eficiencia).
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 5 y 7), `CU-06` (Paso 4, auditoría de calidad y eficiencia).

---

### 3.11 Clase 11: `Desarrollador` (`«capacity_role»`)

1. **Nombre y Propósito:**  
   `Desarrollador` — Representa al profesional de desarrollo/tecnología cuya capacidad semanal (`base 40 h`), puntaje Fibonacci, participación en QA cruzado y eficiencia técnica son consolidados por el sistema, sin que posea cuenta de acceso en la plataforma (`RF-030`).
2. **Atributos y Restricciones:**
   - `- identificadorInstitucional : Texto` — Obligatorio, código único del desarrollador a través de `Redmine`, `GLPI` y `GitLab`.
   - `- nombreCompleto : Texto` — Obligatorio.
   - `- capacidadBaseSemanalHoras : Decimal = 40.0` — Obligatorio, constante de `40.0 horas` semanales (`8 h/día`, lunes a viernes, `RF-002`).
   - `- /horasTareasProyectoActivas : Decimal` — **Derivado (`/`)**: suma de horas en tareas `Redmine` en estado `Análisis` o `En curso` (`RF-003`).
   - `- /horasIncidentesGLPIActivos : Decimal` — **Derivado (`/`)**: suma de horas de tickets `GLPI` activos (`4 h` estándar, `8 h` crítico core o estimación manual, `RF-003`).
   - `- /horasRevisionesQACruzado : Decimal` — **Derivado (`/`)**: `2.0 horas` por cada revisión activa de QA cruzado en `GitLab` (`RF-003`).
   - `- /horasEfectivasSemanales : Decimal` — **Derivado (`/`)**: `/horasTareasProyectoActivas + /horasIncidentesGLPIActivos + /horasRevisionesQACruzado` (`RF-003`).
   - `- /puntosFibonacciSemanales : Entero` — **Derivado (`/`)**: suma de puntos Fibonacci de tareas (`2, 5, 13`), incidentes `GLPI` (`3, 5`) y revisiones QA cruzado (`1 pt` c/u) (`RF-001`, `RF-003`, `RF-035`).
   - `- /porcentajeOcupacionSemanal : Decimal` — **Derivado (`/`)**: `(/horasEfectivasSemanales / 40.0) * 100.0` (`RF-002`).
   - `- /semaforoCapacidad : SemaforoCapacidad` — **Derivado (`/`)**: `DISPONIBLE_VERDE` (`< 75 %` / `< 30 h`), `CARGA_OPTIMA_AMARILLO` (`75 %..100 %` / `30 h..40 h`), `SOBREASIGNADO_ROJO` (`> 100 %` / `> 40 h`) (`RF-002`).
   - `- /eficienciaDesarrollador : Decimal` — **Derivado (`/`)**: indicador `0.0..100.0 %` calculado según la fórmula oficial de `RF-005`.
3. **Operaciones (Entrada / Salida):**
   - `+ calcularCargaLaboralEfectiva() : Decimal` — Suma las horas de tareas activas, incidentes GLPI (`4h/8h`) y revisiones QA cruzado (`2h`) (`RF-003`).
   - `+ determinarSemaforoOcupacion() : SemaforoCapacidad` — Evalúa `/porcentajeOcupacionSemanal` frente a los umbrales `< 75 %`, `75 %–100 %` y `> 100 %` (`RF-002`).
   - `+ calcularEficienciaPonderada(retornos : Entero, hallazgosCriticos : Entero) : Decimal` — Calcula $\text{Eficiencia} = 0.70 \times \text{IndiceAvanceVerificado} + 0.30 \times \max(0, 100 - 10 \times \text{retornos} - 5 \times \text{hallazgosCriticos})$, excluyendo horas de incidentes emergentes `GLPI` (`RF-005`).
   - `+ obtenerResumenTarjetaOperativa() : Mapa<Texto, Texto>` — Devuelve los indicadores listos para las tarjetas por desarrollador del tablero (`RF-035`).
4. **Invariantes / Reglas de Negocio:**
   - **Exclusión de cuentas de desarrollador (`RF-030`):** Un `Desarrollador` jamás se asocia a `SesionInstitucional`; solo opera en las plataformas origen (`Redmine`, `GLPI`, `GitLab`).
   - **Fórmula de Ocupación Semanal (`RF-002`, `RF-003`):**
     $$\text{HorasEfectivas} = \sum H_{\text{Redmine (Análisis/En curso)}} + \sum H_{\text{GLPI (4h o 8h)}} + \sum (2\text{ h} \times N_{\text{QA Cruzado}})$$
   - **Fórmula de Eficiencia (`RF-005`):** Acotada estrictamente al rango `[0.0, 100.0]`, auditada en `RegistroAuditoriaCalculo` (`RNF-001`).
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 5–8), `CU-02` (Sobrecarga `> 40 h` `RF-031` e identificación en alerta `RF-006`), `CU-04` (`RF-032`, `RF-035`), `CU-05` (`RF-009`), `CU-06` (`RF-010`).

---

### 3.12 Clase 12: `AsignacionActividad` (`«capacity_role»` — Clase Asociativa)

1. **Nombre y Propósito:**  
   `AsignacionActividad` — Clase asociativa que materializa el patrón de **Roles por Asociación** entre `Desarrollador` y `ActividadTrazable`, permitiendo que un desarrollador desempeñe múltiples roles sin herencia rígida y detectando divergencias entre el asignado en `Redmine` y el autor real en `GitLab`.
2. **Atributos y Restricciones:**
   - `- codigoAsignacion : Texto` — Obligatorio.
   - `- rolDesempenado : RolParticipacionDesarrollo` — Obligatorio (`RESPONSABLE_ASIGNADO_REDMINE`, `AUTOR_REAL_COMMIT_GITLAB`, `REVISOR_QA_CRUZADO_GITLAB`, `RESPONSABLE_SOPORTE_GLPI`).
   - `- fechaAsignacion : FechaHora` — Obligatorio.
   - `- /horasComputablesCarga : Decimal` — **Derivado (`/`)**: `2.0 h` cuando `rolDesempenado == REVISOR_QA_CRUZADO_GITLAB` (`RF-003`), `4.0 h` u `8.0 h` en soporte `GLPI` sin estimación, o las horas estimadas de la tarea activa.
   - `- /puntosFibonacciComputables : Entero` — **Derivado (`/`)**: `1 pt` en QA cruzado (`RF-003`), `3` o `5 pts` en GLPI, o `2, 5, 13 pts` en tarea de proyecto (`RF-001`).
   - `- /existeDivergenciaAutorAsignado : Booleano` — **Derivado (`/`)**: `verdadero` si el desarrollador con rol `AUTOR_REAL_COMMIT_GITLAB` difiere del desarrollador con rol `RESPONSABLE_ASIGNADO_REDMINE` en la misma actividad (`RF-017`, Flujo `7b`).
3. **Operaciones (Entrada / Salida):**
   - `+ verificarDivergenciaAutoria(asignadoRedmine : Desarrollador, autorGitLab : Desarrollador) : Booleano` — Compara las identidades institucionales y marca la divergencia para auditoría (`RF-017`, Flujo `7b` de `CU-01` y Flujo `3a` de `CU-06`).
   - `+ obtenerEsfuerzoPorRol() : Decimal` — Devuelve las horas efectivas que esta asignación aporta a la semana del `Desarrollador` (`RF-003`).
4. **Invariantes / Reglas de Negocio:**
   - **Cómputo obligatorio de QA cruzado (`RF-003`, `RF-017`):** Toda instancia activa con `rolDesempenado == REVISOR_QA_CRUZADO_GITLAB` suma exactamente `2.0 horas` y `1 punto Fibonacci` al revisor real capturado desde `GitLab`.
5. **Casos de Uso Trazados:**  
   `CU-01` (Pasos 5 y 7, Flujo `7b`), `CU-04` (`RF-035`), `CU-05` (`RF-009`), `CU-06` (Flujo `3a`, `RF-017`).

---

### 3.13 Clase 13: `RegistroAuditoriaCalculo` (`«resilience_gate»`)

1. **Nombre y Propósito:**  
   `RegistroAuditoriaCalculo` — Entidad de solo adición (*append-only*) que conserva de manera inmutable durante al menos **12 meses** los valores de entrada, pesos, penalizaciones y versión de fórmula empleados en cada cálculo de eficiencia, carga y avance, garantizando el **100 % de reproducibilidad matemática** (`RNF-001`).
2. **Atributos y Restricciones:**
   - `- codigoRegistroAuditoria : Texto` — Obligatorio, único e inmutable.
   - `- fechaHoraCalculo : FechaHora` — Obligatorio.
   - `- versionFormulaAplicada : Texto` — Obligatorio (ej. `"RF001-RF003-RF005-v1.0"`).
   - `- pesoAvanceAplicado : Decimal = 0.70` — Obligatorio, constante `0.70` (`70 %`, `RF-005`).
   - `- pesoCalidadAplicado : Decimal = 0.30` — Obligatorio, constante `0.30` (`30 %`, `RF-005`).
   - `- entradasInmutables : Mapa<Texto, Decimal>` — Obligatorio (almacena `avanceRealVerificado`, `avancePlanificado`, `contadorRetornos`, `penalizacionRetornos`, `hallazgosSonar`, `penalizacionSonar`, `horasExcluidasGLPI`).
   - `- resultadoCalculado : Decimal` — Obligatorio.
   - `- huellaInmutabilidad : Texto` — Obligatorio, firma de integridad del registro (`RNF-001`).
   - `- mesesRetencionMinima : Entero = 12` — Obligatorio, restricción `>= 12 meses` (`RNF-001`).
3. **Operaciones (Entrada / Salida):**
   - `+ sellarRegistroInmutable(entradas : Mapa<Texto, Decimal>, resultado : Decimal) : RegistroAuditoriaCalculo` — Crea y sella el rastro matemático durante el paso 8 de `CU-01` (`RNF-001`).
   - `+ verificarReproducibilidad100Porciento() : Booleano` — Recalcula la fórmula a partir de `entradasInmutables` y comprueba coincidencia exacta con `resultadoCalculado` y `huellaInmutabilidad`; si falla, detona la Excepción `4a` en `CU-06` (`RNF-001`).
4. **Invariantes / Reglas de Negocio:**
   - **Inmutabilidad y Retención (`RNF-001`):** Una vez creado, ningún atributo de `RegistroAuditoriaCalculo` puede modificarse ni eliminarse antes de cumplir `12 meses`.
   - **Bloqueo ante corrupción (`CU-06` Excepción `4a`):** Si `verificarReproducibilidad100Porciento() == falso`, el sistema tiene prohibido entregar cifras de auditoría incompletas o no verificadas.
5. **Casos de Uso Trazados:**  
   `CU-01` (Paso 8), `CU-06` (Paso 4 y Excepción `4a`), `CU-08` (`RNF-001`).

---

### 3.14 Clase 14: `AlertaOperativa` (`«governance_alert»`)

1. **Nombre y Propósito:**  
   `AlertaOperativa` — Representa cada evento de desviación crítica detectado tras la consolidación ETL (`CU-02`), gobernando su redacción ejecutiva clara, su filtro de supresión anti-spam de 24 horas, su despacho cifrado multicanal con conmutación por fallo y su ciclo de gestión por parte del Jefe de Desarrollo / PMO (`CU-07`).
2. **Atributos y Restricciones:**
   - `- identificadorUnicoAlerta : Texto` — Obligatorio (`RNF-004`).
   - `- fechaHoraDeteccion : FechaHora` — Obligatorio (`RNF-004`).
   - `- categoriaDesviacion : CategoriaDisparadorAlerta` — Obligatorio (`SOBRECARGA_MAYOR_40H`, `CERTIFICACION_MAYOR_5_DIAS`, `RETORNO_POR_FALLO`, `INCIDENTE_PRIORIDAD_1`, `CAMBIO_PRIORIDAD_PROYECTO` — `RF-031`, `RF-025`).
   - `- nivelCriticidad : Entero` — Obligatorio, permite jerarquizar visualmente en `< 10 s` (`RNF-007`).
   - `- mensajeLenguajeClaro : Texto` — Obligatorio, redactado en español directo con desarrollador responsable, módulo financiero y valor exacto sin códigos crudos (`RF-006`, `RNF-002`).
   - `- destinatariosNotificados : Lista<Texto>` — Obligatorio (`RNF-004`).
   - `- estadoActualAlerta : EstadoAlertaOperativa` — Obligatorio (`EVALUADA`, `SUPRIMIDA_ANTISPAM_24H`, `ENVIADA_MENSAJERIA`, `ESCALADA_CORREO`, `CONMUTADA_CORREO_POR_FALLO`, `ENTREGA_FALLIDA`, `EN_SEGUIMIENTO`, `ATENDIDA`).
   - `- canalEntregaEfectivo : Texto` — Obligatorio (`"Canal de Mensajería Corporativo"`, `"Servicio de Correo Corporativo"` o `"Ambos"`, `RF-024`, `RF-025`, `RNF-004`).
   - `- tiempoEvaluacionMilisegundos : Decimal` — Obligatorio, restricción `< 200.0 ms` (`RNF-010`).
   - `- usuarioGestorJefatura : Texto [0..1]` — Opcional, registrado cuando el Jefe de Desarrollo / PMO gestiona la alerta en `CU-07` (`RF-026`).
   - `- fechaHoraGestion : FechaHora [0..1]` — Opcional (`RF-026`, `RNF-004`).
   - `- /requiereEscalamientoCorreo : Booleano` — **Derivado (`/`)**: `verdadero` si `categoriaDesviacion` es `CERTIFICACION_MAYOR_5_DIAS`, `INCIDENTE_PRIORIDAD_1`, `CAMBIO_PRIORIDAD_PROYECTO` o si falló la entrega en el `Canal de Mensajería Corporativo` (`RF-025`, Flujo `4a`).
3. **Operaciones (Entrada / Salida):**
   - `+ evaluarReglasUmbralCritico(ocupacionHoras : Decimal, diasCertificacion : Decimal, retornos : Entero) : Booleano` — Evalúa en `< 200 ms` si se superó algún umbral crítico (`RF-031`, `RNF-010`).
   - `+ redactarMensajeEjecutivoClaro(responsable : Texto, modulo : ModuloFinancieroCore, valorExacto : Texto) : Texto` — Construye el texto comprensible sin códigos técnicos crudos (`RF-006`).
   - `+ verificarSupresionAntiSpam24Horas(historialReciente : Lista<AlertaOperativa>) : Booleano` — Devuelve `verdadero` (suprimir) si existe una alerta con misma actividad/incidente, causa y severidad emitida en las últimas `24 horas` o marcada como `ATENDIDA`/`EN_SEGUIMIENTO` sin agravamiento (`RF-023`, `RF-026`, Flujo `3a`).
   - `+ registrarResultadoDespacho(canal : Texto, exitoso : Booleano, protocoloTLS : Texto) : EstadoAlertaOperativa` — Actualiza el estado de entrega exigiendo cifrado `>= TLS 1.2` (`RNF-015`) y persiste el evento al `100 %` incluso si ambos canales fallan (`RNF-004`, Excepción `5a`).
   - `+ marcarEstadoGestionJefatura(nuevoEstado : EstadoAlertaOperativa, responsableGestion : Texto, instante : FechaHora) : Booleano` — Cambia el estado a `ATENDIDA` o `EN_SEGUIMIENTO`, deteniendo recordatorios repetitivos y guardando auditoría (`CU-07`, `RF-026`).
4. **Invariantes / Reglas de Negocio:**
   - **Presupuesto de evaluación (`RNF-010`, Excepción `1a`):** `tiempoEvaluacionMilisegundos < 200.0`; si se excede o hay inconsistencia interna, se aborta la emisión sin despachar falsos positivos.
   - **Ventana Anti-Spam de 24 Horas (`RF-023`, `RF-026`):** Ninguna alerta idéntica dentro de `24 h` ni alerta en estado `ATENDIDA` / `EN_SEGUIMIENTO` (sin nuevo agravamiento) puede despacharse hacia canales externos.
   - **Persistencia del 100 % y Cifrado (`RNF-004`, `RNF-015`):** El `100 %` de las alertas (aprobadas, suprimidas o fallidas) queda registrado en bitácora, y todo envío externo exige `TLS 1.2` o superior.
5. **Casos de Uso Trazados:**  
   `CU-02` (Todos los pasos, Flujos `3a`, `4a`, Excepciones `1a`, `5a`), `CU-04` (Visualización de alertas vigentes), `CU-06` (Disparador de auditoría causal), `CU-07` (`RF-026`, `RF-027`).

---

### 3.15 Clase 15: `SesionInstitucional` (`«governance_alert»`)

1. **Nombre y Propósito:**  
   `SesionInstitucional` — Representa la sesión web autenticada en modo estrictamente de solo lectura del `Jefe de Desarrollo / PMO` (`CU-03`), manteniendo el estado lógico de los filtros combinados activos (`CU-05`), el canal de actualización incremental cada 5 minutos (`CU-04`) y la liberación determinista de recursos al cierre (`RNF-011`).
2. **Atributos y Restricciones:**
   - `- identificadorSesion : Texto` — Obligatorio.
   - `- usuarioInstitucional : Texto` — Obligatorio.
   - `- perfilAutorizado : PerfilAccesoUnico` — Obligatorio (`JEFE_DESARROLLO` o `PMO_JEFATURA_PROYECTOS` — ambos acceden a la misma vista única; desarrolladores prohibidos según `RF-030`, `RF-032`).
   - `- protocoloCifrado : Texto = "HTTPS TLS 1.3"` — Obligatorio (`RNF-013`).
   - `- modoSoloLecturaEstricto : Booleano = verdadero` — Obligatorio (`RF-030`, `RF-032`).
   - `- criteriosFiltroActivos : Mapa<Texto, Texto>` — Obligatorio, almacena los criterios individuales vigentes (`desarrollador`, `moduloFinanciero`, `etapaOficial`, `fechaInicio`, `fechaFin` — `RF-009`, `RF-028`).
   - `- canalIncrementalAbierto : Booleano = verdadero` — Obligatorio (`RF-034`, `RNF-011`).
   - `- tiempoLiberacionCierreMs : Decimal` — Obligatorio al cerrar, restricción `< 200.0 ms` (`RNF-011`).
3. **Operaciones (Entrada / Salida):**
   - `+ autenticarCredencialesInstitucionales(usuario : Texto, tokenCifrado : Texto) : Booleano` — Valida el acceso institucional sobre `TLS 1.3` asignando la vista única de solo lectura (`CU-03`, `RF-030`, `RNF-013`).
   - `+ aplicarFiltroMultidimensional(criterios : Mapa<Texto, Texto>) : Lista<ActividadTrazable>` — Valida que el rango de fechas sea coherente (Excepción `3a` de `CU-05`) y calcula en `<= 400 ms` (p95 sobre 5.000 registros) la intersección exacta de criterios (`RF-009`, `RNF-018`).
   - `+ retirarCriterioFiltroIndividual(claveCriterio : Texto) : Mapa<Texto, Texto>` — Elimina selectivamente una etiqueta de filtro y recalcula sobre los criterios restantes sin reiniciar la consulta (`RF-028`).
   - `+ limpiarTodosLosFiltros() : Mapa<Texto, Texto>` — Restablece el estado global del tablero ante cero coincidencias (`RF-039`, Flujo `4a`) o retiro total (`6a`).
   - `+ confirmarBitacoraYLiberarRecursos() : Booleano` — Confirma la persistencia íntegra de alertas gestionadas (`RF-027`) y libera el canal incremental y la memoria en `< 200 ms` dejando `0` conexiones huérfanas (`RNF-011`, Flujo `5a` de `CU-04` y `CU-07`).
4. **Invariantes / Reglas de Negocio:**
   - **Acceso unificado y exclusión de desarrolladores (`RF-030`, `RF-032`):** `perfilAutorizado` solo admite `JEFE_DESARROLLO` o `PMO_JEFATURA_PROYECTOS` con exactamente los mismos permisos de solo lectura.
   - **Presupuestos de respuesta y cierre (`RNF-006`, `RNF-011`, `RNF-014`, `RNF-018`):** Carga inicial `<= 2.0 s` (p95), refresco incremental `< 1.0 s`, apertura de filtros `< 300.0 ms`, recálculo multidimensional `<= 400.0 ms` (p95) y cierre de sesión `< 200.0 ms`.
5. **Casos de Uso Trazados:**  
   `CU-03` (Principal), `CU-04` (Precondición, refresco incremental y cierre `5a`), `CU-05` (Filtrado multidimensional y gestión de etiquetas), `CU-06` (Precondición de sesión), `CU-07` (Confirmación al cierre `RF-027`), `CU-08` (Precondición de exportación).

---

### 3.16 Clase 16: `ReporteTrimestral` (`«governance_alert»`)

1. **Nombre y Propósito:**  
   `ReporteTrimestral` — Representa el informe ejecutivo trimestral consolidado y auditable que sintetiza para la gerencia y PMO el volumen total de proyectos, requerimientos, iniciativas, incidentes atendidos, cumplimiento global y desempeño en certificaciones (`CU-08` / `RF-040`).
2. **Atributos y Restricciones:**
   - `- codigoReporte : Texto` — Obligatorio.
   - `- periodoTrimestre : Texto` — Obligatorio (ej. `"2026-T4"`).
   - `- fechaHoraGeneracion : FechaHora` — Obligatorio.
   - `- totalProyectosPOAPETI : Entero` — Obligatorio, `>= 0` (`RF-040`).
   - `- totalRequerimientosREQ : Entero` — Obligatorio, `>= 0` (`RF-040`).
   - `- totalIniciativasINITI : Entero` — Obligatorio, `>= 0` (`RF-040`).
   - `- totalIncidentesGLPIAtendidos : Entero` — Obligatorio, `>= 0` (`RF-040`).
   - `- /porcentajeCumplimientoGlobal : Decimal` — **Derivado (`/`)**: porcentaje ejecutivo de cumplimiento (`0.0..100.0 %`) respaldado por `RegistroAuditoriaCalculo` (`RF-040`, `RNF-001`).
   - `- totalCertificacionesATiempo : Entero` — Obligatorio, actividades cerradas con permanencia `<= 5 días hábiles` en `CERTIFICACION` (`RF-040`).
   - `- totalCertificacionesAtrasadas : Entero` — Obligatorio, actividades con permanencia `> 5 días hábiles` en `CERTIFICACION` (`RF-040`).
3. **Operaciones (Entrada / Salida):**
   - `+ consolidarMetricasTrimestrales(expedientes : Lista<ExpedienteModuloFinanciero>, trimestre : Texto) : ReporteTrimestral` — Agrega los totales de `EpicaPOA`, `HistoriaEspecificacion` (`REQ` vs. `INI.TI`), `TicketIncidenteGLPI` y porcentaje de cumplimiento del trimestre (`CU-08`, `RF-040`).
   - `+ compararCertificacionesATiempoVsAtrasadas() : Mapa<Texto, Entero>` — Calcula la proporción de certificaciones a tiempo (`<= 5 días hábiles`) frente a las atrasadas (`> 5 días hábiles`) (`RF-040`).
4. **Invariantes / Reglas de Negocio:**
   - **Exactitud ejecutiva y lenguaje claro (`RF-040`, `RNF-001`, `RNF-002`):** Todas las cifras exportadas deben coincidir al `100 %` con los registros auditables inmutables consolidados en `CU-01` y expresarse en español ejecutivo sin códigos crudos.
5. **Casos de Uso Trazados:**  
   `CU-08` (Principal, `RF-040`).

---

### 3.17 Catálogo de Enumeraciones del Dominio (`«enumeration»`)

| Enumeración (`«enumeration»`) | Valores Canónicos Oficiales | Requisitos Respaldados |
|---|---|---|
| **`ModuloFinancieroCore`** | `FITSWITCH`, `SWI`, `CANALES_ELECTRONICOS`, `CREDITO`, `ANTIFRAUDE`, `NOMINA` | `RF-012`, `RF-018`, `RF-007` |
| **`EtapaOficialFlujo`** | `ANALISIS`, `DESARROLLO`, `CERTIFICACION`, `DESPLIEGUE`, `ESTABILIZACION` | `RF-019`, `RF-036` |
| **`NivelComplejidadFibonacci`** | `NIVEL_1_BAJA_2_PTS`, `NIVEL_2_MEDIA_5_PTS`, `NIVEL_3_ALTA_13_PTS` | `RF-001`, `RF-035` |
| **`SemaforoCapacidad`** | `DISPONIBLE_VERDE`, `CARGA_OPTIMA_AMARILLO`, `SOBREASIGNADO_ROJO` | `RF-002`, `RF-007`, `RF-035` |
| **`TipoCommitTipado`** | `FEAT`, `FIX`, `TEST`, `REFACTOR` | `RF-014`, `RF-033`, `RF-038` |
| **`RolParticipacionDesarrollo`** | `RESPONSABLE_ASIGNADO_REDMINE`, `AUTOR_REAL_COMMIT_GITLAB`, `REVISOR_QA_CRUZADO_GITLAB`, `RESPONSABLE_SOPORTE_GLPI` | `RF-003`, `RF-017` |
| **`TipoInconsistenciaTrazabilidad`** | `NINGUNA_TRAZABILIDAD_COMPLETA`, `TRABAJO_CODIGO_SIN_TRAZABILIDAD_PLANIFICADA`, `INCIDENTE_SOPORTE_SIN_TRAZABILIDAD_DESARROLLO`, `BRECHA_EVIDENCIA_AVANCE`, `DIVERGENCIA_AUTOR_ASIGNADO` | `RF-017`, `RF-021`, `RF-022`, `RF-038` |
| **`EstadoAlertaOperativa`** | `EVALUADA`, `SUPRIMIDA_ANTISPAM_24H`, `ENVIADA_MENSAJERIA`, `ESCALADA_CORREO`, `CONMUTADA_CORREO_POR_FALLO`, `ENTREGA_FALLIDA`, `EN_SEGUIMIENTO`, `ATENDIDA` | `RF-023`..`RF-027`, `RNF-004` |
| **`EstadoCicloSincronizacion`** | `INICIADO`, `EXTRAYENDO_SOLO_LECTURA`, `SINCRONIZACION_PARCIAL_SNAPSHOT`, `VALIDANDO_COMPUERTA_LOPDP`, `CONSOLIDADO_EXITOSO`, `ABORTADO_FALLO_TOTAL`, `ABORTADO_RIESGO_PII`, `REVERTIDO_POR_CONFLICTO` | `RF-011`, `RF-029`, `RNF-008`, `RNF-009`, `RNF-017` |
| **`CategoriaDisparadorAlerta`** | `SOBRECARGA_MAYOR_40H`, `CERTIFICACION_MAYOR_5_DIAS`, `RETORNO_POR_FALLO`, `INCIDENTE_PRIORIDAD_1`, `CAMBIO_PRIORIDAD_PROYECTO` | `RF-031`, `RF-025` |
| **`FuenteOperativaExterna`** | `REDMINE`, `GLPI`, `GITLAB`, `SONARQUBE` | `RF-011`, `RNF-003`, `RNF-008` |
| **`TipoEspecificacionOrigen`** | `REQ_REQUERIMIENTO`, `INI_TI_INICIATIVA` | `RF-033`, `RF-040` |
| **`PerfilAccesoUnico`** | `JEFE_DESARROLLO`, `PMO_JEFATURA_PROYECTOS` | `RF-030`, `RF-032` |

---

## 4. Matriz de Trazabilidad: Escenarios (`CU-01`..`CU-08`) vs. Clases del Dominio

La siguiente matriz verifica que **ninguna fila (Caso de Uso) ni columna (Clase del Dominio) quede vacía**, especificando si la clase es **Creada/Actualizada (`C/U`)** o **Consultada/Auditada (`R`)** en cada escenario:

| Clase del Dominio \ Caso de Uso | `CU-01` Sincronizar y consolidar (`21 RF`) | `CU-02` Emitir alerta (`5 RF`) | `CU-03` Autenticar usuario (`RF-030`) | `CU-04` Consultar tablero (`5 RF`) | `CU-05` Filtrar tablero (`4 RF`) | `CU-06` Auditar causa raíz (`RF-010`) | `CU-07` Gestionar alerta (`2 RF`) | `CU-08` Exportar reporte (`RF-040`) | Total CUs por Clase |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **01. `CicloSincronizacion`** | **C/U** | R | — | R *(2a/2b)* | R *(4b)* | — | — | — | **4 CUs** |
| **02. `SnapshotFuenteExterna`** | **C/U** *(2a)* | — | — | R *(2a)* | — | — | — | — | **2 CUs** |
| **03. `AuditorCompuertaLOPDP`** | **C/U** *(3a)* | — | — | — | — | R *(2b/4a)* | — | — | **2 CUs** |
| **04. `ExpedienteModuloFinanciero`** | **C/U** | R | — | **R** | **R** | **R** | — | **R** | **6 CUs** |
| **05. `EpicaPOA`** | **C/U** | R *(`RF-025`)* | — | **R** | **R** | **R** | — | **R** | **6 CUs** |
| **06. `HistoriaEspecificacion`** | **C/U** | — | — | **R** | **R** | **R** | — | **R** | **5 CUs** |
| **07. `ActividadTrazable`** | **C/U** | **R** | — | **R** | **R** | **R** *(2a/3a)* | — | **R** | **6 CUs** |
| **08. `TicketIncidenteGLPI`** | **C/U** *(5a/6b)* | **R** *(P1)* | — | **R** | **R** | **R** *(2a)* | — | **R** | **6 CUs** |
| **09. `EvidenciaCodigo`** | **C/U** *(6a/7a)* | — | — | **R** | **R** | **R** *(2a/3a)* | — | — | **4 CUs** |
| **10. `HallazgoCalidadSonar`** | **C/U** | — | — | R | — | **R** | — | — | **3 CUs** |
| **11. `Desarrollador`** | **C/U** | **R** *(>40h)* | — | **R** | **R** | **R** | — | — | **5 CUs** |
| **12. `AsignacionActividad`** | **C/U** *(7b)* | — | — | **R** | **R** | **R** *(3a)* | — | — | **4 CUs** |
| **13. `RegistroAuditoriaCalculo`** | **C** *(`RNF-001`)* | — | — | — | — | **R** *(4a)* | — | **R** | **3 CUs** |
| **14. `AlertaOperativa`** | — | **C/U** | — | **R** | R | **R** | **U** *(`RF-026`)* | — | **5 CUs** |
| **15. `SesionInstitucional`** | — | — | **C/U** | **R/U** *(5a)* | **U** *(filtros)* | **R** | **U** *(`RF-027`)* | **R** | **6 CUs** |
| **16. `ReporteTrimestral`** | — | — | — | — | — | — | — | **C/R** | **1 CU** |
| **Cobertura por Caso de Uso** | **13 Clases** | **6 Clases** | **1 Clase** | **12 Clases** | **10 Clases** | **12 Clases** | **2 Clases** | **8 Clases** | **100 % Trazado** |

---

## 5. Contrato de Sincronización 1:1 para la Vista de Procesos (Semana 5)

Para garantizar el cumplimiento inmediato de la **Regla Bloqueante de Trazabilidad 1:1 (Semana 4 ↔ Semana 5)** definida en la Sección 2.6 de [reglas-vista-logica-y-procesos.md](referencias/reglas-vista-logica-y-procesos.md), la siguiente tabla mapea exactamente qué participante `«entity»` y qué operación pública (`+`) debe invocar el compañero encargado de los **5 Diagramas de Secuencia** en la Semana 5:

| Caso de Uso Significativo (Semana 5) | Paso / Flujo Alterno o Excepción (Cockburn) | Participante `«entity»` Exacto (Semana 4) | Mensaje / Operación Pública Exacta (`+`) a Invocar en el Diagrama de Secuencia |
|---|---|---|---|
| **`CU-01` Sincronizar y consolidar** | Paso 1: Inicio del ciclo cada 5 min (`RF-029`) | `CicloSincronizacion` | `+ iniciarCicloPeriodico(marcaTemporalDisparo : FechaHora) : CicloSincronizacion` |
| **`CU-01`** | Paso 2: Validación de timeout `<= 10 s` y `0..3` reintentos (`RNF-008`) | `SnapshotFuenteExterna` | `+ validarTimeoutYReintentos(segundosConsumidos : Decimal, intentos : Entero) : Booleano` |
| **`CU-01`** | Flujo Alterno `2a`: Caída parcial tras 3 reintentos — reusar snapshot válido | `CicloSincronizacion`<br>`SnapshotFuenteExterna` | `+ activarContingenciaSnapshotParcial(fuenteCaida : FuenteOperativaExterna, ultimoSnapshotValido : SnapshotFuenteExterna) : EstadoCicloSincronizacion`<br>`+ marcarComoSnapshotContingencia(cicloActual : Texto) : SnapshotFuenteExterna` |
| **`CU-01`** | Excepción `2b`: Fallo total de las 4 fuentes o credenciales (`break`) | `CicloSincronizacion` | `+ abortarPorFalloTotalFuentes(motivo : Texto) : Vacio` |
| **`CU-01`** | Paso 3 y Excepción `3a`: Compuerta obligatoria LOPDP 2021 (`RNF-017`) | `AuditorCompuertaLOPDP` | `+ depurarYEnmascararPII(loteCrudo : Lista<Texto>) : Lista<Texto>`<br>`+ verificarCeroExposicionPII() : Booleano`<br>`+ abortarLotePorRiesgoPII(motivoCritico : Texto) : Vacio` |
| **`CU-01`** | Paso 4: Jerarquía de 4 niveles (`RF-033`, `RF-012`) | `EpicaPOA`<br>`HistoriaEspecificacion` | `+ obtenerDesgloseHistoriasYTareas() : Lista<HistoriaEspecificacion>`<br>`+ vincularTareaPlanificada(tarea : ActividadTrazable) : Booleano` |
| **`CU-01`** | Paso 5 y Flujo `5a`: Cruce tolerante `GLPI #ID` (`RF-013`) | `TicketIncidenteGLPI`<br>`ActividadTrazable` | `+ normalizarReferenciaDigitada(textoCrudoRedmine : Texto) : Texto`<br>`+ enlazarTicketGLPITolerante(referenciaTexto : Texto, ticket : TicketIncidenteGLPI) : Booleano` |
| **`CU-01`** | Paso 5 y Flujo `5b`: Retorno por fallo hacia «En curso» (`RF-015`) | `ActividadTrazable` | `+ registrarRetornoPorFallo(etapaOrigen : EtapaOficialFlujo, motivoRechazo : Texto) : Entero` |
| **`CU-01`** | Paso 6 y Flujos `6a` / `6b`: Segregación sin trazabilidad (`RF-021`, `RF-022`) | `ActividadTrazable`<br>`TicketIncidenteGLPI` | `+ clasificarActividadSinTrazabilidad(origen : FuenteOperativaExterna) : TipoInconsistenciaTrazabilidad`<br>`+ verificarExistenciaEspejoRedmine() : Booleano` |
| **`CU-01`** | Paso 7 y Flujo `7a`: Avance real respaldado en código (`> 25 %` y `100 %`, `RF-038`) | `EvidenciaCodigo`<br>`ActividadTrazable` | `+ contabilizarCommitsTipados(mensajesCommit : Lista<Texto>) : Mapa<TipoCommitTipado, Entero>`<br>`+ validarRequisitosAvance25Porciento() : Booleano`<br>`+ validarRequisitosCierre100Porciento() : Booleano`<br>`+ verificarRespaldoAvanceEnCodigo(evidencia : EvidenciaCodigo) : Decimal` |
| **`CU-01`** | Paso 7 y Flujo `7b`: Divergencia autor vs. asignado y QA cruzado (`RF-017`) | `AsignacionActividad` | `+ verificarDivergenciaAutoria(asignadoRedmine : Desarrollador, autorGitLab : Desarrollador) : Booleano`<br>`+ obtenerEsfuerzoPorRol() : Decimal` |
| **`CU-01`** | Paso 7: Carga base 40h, semáforo, eficiencia 70/30 y 5 etapas (`RF-001`..`005`, `RF-019`) | `Desarrollador`<br>`HallazgoCalidadSonar`<br>`ActividadTrazable`<br>`EpicaPOA` | `+ calcularCargaLaboralEfectiva() : Decimal`<br>`+ determinarSemaforoOcupacion() : SemaforoCapacidad`<br>`+ calcularPenalizacionHallazgo() : Decimal`<br>`+ calcularEficienciaPonderada(retornos : Entero, hallazgosCriticos : Entero) : Decimal`<br>`+ normalizarEtapaOficial(estadoOrigen : Texto, tieneMRAprobado : Booleano, estadoCICD : Booleano) : EtapaOficialFlujo`<br>`+ consolidarAvanceRealVsPlanificado() : Decimal` |
| **`CU-01`** | Paso 8 y Excepción `8a`: Consolidación idempotente (`RNF-009`) y rastro inmutable (`RNF-001`) | `ExpedienteModuloFinanciero`<br>`RegistroAuditoriaCalculo`<br>`CicloSincronizacion` | `+ unificarExpedienteCronologico(actividades : Lista<ActividadTrazable>) : ExpedienteModuloFinanciero`<br>`+ sellarRegistroInmutable(entradas : Mapa<Texto, Decimal>, resultado : Decimal) : RegistroAuditoriaCalculo`<br>`+ consolidarLoteIdempotente(expedientes : Lista<ExpedienteModuloFinanciero>) : Booleano`<br>`+ revertirLotePorConflicto(detalleError : Texto) : Vacio` |
| **`CU-02` Emitir alerta operativa** | Pasos 1–2: Evaluación `< 200 ms` (`RNF-010`) y redacción clara (`RF-006`) | `AlertaOperativa` | `+ evaluarReglasUmbralCritico(ocupacionHoras : Decimal, diasCertificacion : Decimal, retornos : Entero) : Booleano`<br>`+ redactarMensajeEjecutivoClaro(responsable : Texto, modulo : ModuloFinancieroCore, valorExacto : Texto) : Texto` |
| **`CU-02`** | Paso 3 y Flujo `3a`: Supresión anti-spam 24h y estado `CU-07` (`RF-023`, `RF-026`) | `AlertaOperativa` | `+ verificarSupresionAntiSpam24Horas(historialReciente : Lista<AlertaOperativa>) : Booleano` |
| **`CU-02`** | Pasos 4–6, Flujo `4a` y Excepción `5a`: Despacho y bitácora 100 % (`RNF-004`, `RNF-015`) | `AlertaOperativa` | `+ registrarResultadoDespacho(canal : Texto, exitoso : Booleano, protocoloTLS : Texto) : EstadoAlertaOperativa` |
| **`CU-04` Consultar tablero** | Pasos 1–4 y Flujo `2a`: Carga consolidada `<= 2 s`, tarjetas, mapa de calor y embudo | `ExpedienteModuloFinanciero`<br>`Desarrollador`<br>`SnapshotFuenteExterna` | `+ calcularMapaCalorModulo() : Decimal`<br>`+ detectarCuellosDeBotellaEmbudo() : Lista<EtapaOficialFlujo>`<br>`+ obtenerResumenTarjetaOperativa() : Mapa<Texto, Texto>`<br>`+ verificarFrescuraDatos(instanteConsulta : FechaHora) : Duracion` |
| **`CU-04`** | Flujo `5a`: Cierre determinista en `< 200 ms` (`RNF-011`) | `SesionInstitucional` | `+ confirmarBitacoraYLiberarRecursos() : Booleano` |
| **`CU-05` Filtrar tablero** | Pasos 3–6, Flujos `4a`, `6a` y Excepción `3a`: Intersección `<= 400 ms` y etiquetas (`RF-009`, `RF-028`, `RF-039`) | `SesionInstitucional` | `+ aplicarFiltroMultidimensional(criterios : Mapa<Texto, Texto>) : Lista<ActividadTrazable>`<br>`+ retirarCriterioFiltroIndividual(claveCriterio : Texto) : Mapa<Texto, Texto>`<br>`+ limpiarTodosLosFiltros() : Mapa<Texto, Texto>` |
| **`CU-06` Auditar causa raíz** | Pasos 2–5, Flujos `2a`, `3a` y Excepciones `2b`, `4a` (`RF-010`, `RNF-001`, `RNF-007`, `RNF-017`) | `AuditorCompuertaLOPDP`<br>`ExpedienteModuloFinanciero`<br>`RegistroAuditoriaCalculo` | `+ evaluarVigenciaRetencion24Meses(fechaRegistro : FechaHora, fechaActual : FechaHora) : Booleano`<br>`+ reconstruirCadenaCausal(codigoElemento : Texto) : ExpedienteModuloFinanciero`<br>`+ verificarReproducibilidad100Porciento() : Booleano` |
