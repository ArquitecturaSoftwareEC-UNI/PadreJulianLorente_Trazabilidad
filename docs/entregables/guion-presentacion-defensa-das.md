# Guión Maestro de Presentación y Defensa — Documento de Arquitectura de Software (DAS)

- **Proyecto:** Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)
- **Asignatura y Docente:** Arquitectura de Software (`DSOF_4069` — UTPL) · Mgtr. Wayner Xavier Bustamante Granda
- **Fecha de Defensa:** Lunes 23/12/2025
- **Equipo Expositor (3 Integrantes):**
  1. **Carlos Valladares** *(Líder Técnico — Arquitectura Empresarial TOGAF, Vista de Escenarios `+1`, Vista de Desarrollo y Vista Física)*
  2. **Alex Daniel Gutiérrez Córdova** *(Arquitecto de Dominio — Vista Lógica: Diagrama de Clases, Estados y Diseño Detallado)*
  3. **Luis Carrión** *(Arquitecto de Procesos — Vista de Procesos: Concurrencia, Análisis ECB y Diagramas de Secuencia)*

---

## Distribución Recomendada del Tiempo (12 a 15 minutos + Ronda de Preguntas)

| Orden | Bloque y Diagramas a Explicar | Expositor | Tiempo Est. |
|:---:|---|---|---:|
| **1** | Apertura, Problema COAC, **Cadena de Valor** ➔ **Flujo de Valor** ➔ **Mapa de Capacidades** ➔ **Diagrama de Casos de Uso (`+1`)** | **Carlos Valladares** | `4.5 min` |
| **2** | **Vista Lógica:** **Diagrama de Clases** (4 paquetes, 16 entidades, 40 métodos) + **Diagrama de Estados** | **Alex Daniel Gutiérrez Córdova** | `3.5 min` |
| **3** | **Vista de Procesos:** 3 Procesos Concurrentes, Robustez ECB (`0` violaciones) y **Diagramas de Secuencia** (`CU-01`..`CU-06`) | **Luis Carrión** | `3.5 min` |
| **4** | **Vista de Desarrollo** (**Diagrama de Componentes** + **Diagrama de Paquetes**) y **Vista Física** (**Diagrama de Despliegue** + Conclusión) | **Carlos Valladares** | `3.5 min` |

---

## BLOQUE 1 — Contexto Empresarial (TOGAF) y Vista de Escenarios (`+1`)
**Expositor:** **Carlos Valladares**

### 1.1 Apertura y Problema de Arquitectura (`30 s`)
> **Guión hablado:**
> «Buenos días/tardes, ingeniero y compañeros. Somos el equipo conformado por Alex Gutiérrez, Luis Carrión y mi persona, Carlos Valladares. Presentamos la arquitectura de software bajo el **Modelo 4+1 de Philippe Kruchten** y **TOGAF ADM** para el **Sistema de Trazabilidad TI de la Cooperativa de Ahorro y Crédito Padre Julián Lorente**.
>
> El problema central de la cooperativa es que la trazabilidad de sus proyectos tecnológicos está fragmentada en cuatro herramientas sin correlación automática: **Redmine** (planificación POA/PETI y tareas), **GLPI** (incidentes de soporte), **GitLab** (ramas, Merge Requests, commits y QA cruzado) y **SonarQube** (calidad y vulnerabilidades). Esto impedía a la Jefatura de TI y a la PMO saber la carga laboral real sobre 40 horas semanales, verificar si el porcentaje de avance reportado tenía respaldo real en código y auditar los cuellos de botella en los 6 módulos críticos del core financiero (`FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude` y `Nómina`).»

---

### 1.2 Diagrama 1: Cadena de Valor TOGAF ADM (`1 min`)
- **Imagen a mostrar:** [`docs/cadena-de-valor-togaf-adm.png`](../cadena-de-valor-togaf-adm.png)

> **Guión hablado:**
> «Partimos de la **Cadena de Valor bajo TOGAF ADM**. En el bloque superior modelamos las **6 actividades primarias del flujo operativo**:
> 1. La demanda nace en **Épicas POA/PETI**, requerimientos `REQ`/`INI.TI` o incidentes `GLPI`.
> 2. Se planifica en **Redmine** con puntos Fibonacci (`2, 5, 13`) sobre una capacidad base de `40 horas` semanales.
> 3. Se construye en **GitLab** con commits tipados (`feat`, `fix`, `test`, `refactor`) e inspección en **SonarQube**.
> 4. Nuestro sistema interviene en los eslabones 4, 5 y 6: ejecuta la **correlación core cada 5 minutos en modo estrictamente de solo lectura**, calcula métricas reales y emite alertas anti-spam para el gobierno y toma de decisiones de la PMO.
>
> En el bloque inferior están las **actividades de soporte TOGAF (Fases Preliminar y A–H)**: gobierno de identificadores, protección de datos personales bajo la **LOPDP Ecuador 2021**, aislamiento en solo lectura y resiliencia ante caídas externas.»

- **Decisión que defiendes:** El sistema **no reemplaza** a Redmine, GLPI ni GitLab; actúa como una **capa de observabilidad y correlación no intrusiva en solo lectura (`RNF-003`)**.

---

### 1.3 Diagrama 2: Flujo de Valor (*Value Stream*) (`45 s`)
- **Imagen a mostrar:** [`docs/uml/flujo-de-valor.png`](../uml/flujo-de-valor.png)

> **Guión hablado:**
> «De la Cadena de Valor derivamos el **Flujo de Valor End-to-End** (*Asegurar Trazabilidad, Capacidad y Control de Entregas TI*). Aquí detallamos qué entra y qué valor sale en cada una de las 6 etapas: desde el detonante estratégico hasta la propuesta de valor final.
>
> Destacamos dos transformaciones críticas: en la **Etapa 2**, antes de procesar cualquier texto extraído de tickets GLPI o Redmine, pasa por una **compuerta bloqueante LOPDP** que enmascara al `100 %` cédulas, cuentas y correos de socios; y en las **Etapas 3 y 4**, el avance ya no depende de lo que alguien digite manualmente en Redmine, sino de la evidencia verificada en GitLab y SonarQube.»

---

### 1.4 Diagrama 3: Mapa de Capacidades Institucionales (`45 s`)
- **Imagen a mostrar:** [`docs/uml/mapa-de-capacidades.png`](../uml/mapa-de-capacidades.png)

> **Guión hablado:**
> «Este flujo se sustenta en nuestro **Mapa de Capacidades en 3 niveles**:
> - **Nivel 1 (Estratégico y Gobierno PMO):** gobierno de portafolio POA/PETI, balanceo de carga sobre 40 horas, reporte ejecutivo trimestral y auditoría de causa raíz en menos de 10 segundos.
> - **Nivel 2 (Capacidades Core Diferenciadoras):** ingesta cada 5 minutos en solo lectura, correlación de 4 niveles, validación de avance real (`> 25 %` exige rama y commits; `100 %` exige Merge Request aprobado y CI/CD exitoso), fórmula de eficiencia `70 % avance + 30 % calidad`, embudo de 5 etapas y alertas con supresión anti-spam de 24 horas.
> - **Nivel 3 (Soporte, Seguridad y Plataforma):** compuerta LOPDP 2021, acceso web único sobre `HTTPS TLS 1.3` sin requerir VPN, tolerancia a fallos con snapshot parcial y rastro matemático inmutable por 12 a 24 meses.»

---

### 1.5 Diagrama 4: Diagrama de Casos de Uso — Vista de Escenarios (`+1`) (`1.5 min`)
- **Imagen a mostrar:** [`docs/Diagrama de Casos de Uso - Padre Julian.drawio.png`](../Diagrama%20de%20Casos%20de%20Uso%20-%20Padre%20Julian.drawio.png)

> **Guión hablado:**
> «Aterrizamos las capacidades en la **Vista de Escenarios (+1)**, que consolida nuestros **40 Requerimientos Funcionales (40 Historias de Usuario = 225 puntos Fibonacci)** y **18 Requerimientos No Funcionales (ISO/IEC 25010:2023)** en **8 Casos de Uso a nivel del mar**:
> - A la izquierda tenemos únicamente **2 Actores Primarios**:
>   1. **`Tiempo (Temporizador CRON)`**, que dispara cada 5 minutos el **`CU-01: Sincronizar y consolidar trazabilidad e indicadores`** consultando a los 4 sistemas externos secundarios (`Redmine`, `GLPI`, `GitLab` y `SonarQube`). Si al cerrar el ciclo detecta sobrecarga `> 40 h`, atraso `> 5 días hábiles` en Certificación, retorno por fallo o incidente Prioridad 1, activa por relación **`«extend»`** al **`CU-02: Emitir alerta operativa`** hacia el `Canal de Mensajería Corporativo` y el `Servicio de Correo Corporativo`.
>   2. **`Jefe de Desarrollo / PMO`**, quien se autentica en **`CU-03`** y accede a una **única vista web compartida en solo lectura** para: **`CU-04 Consultar tablero de control`** (extendido por **`CU-05 Filtrar indicadores`** en `<= 400 ms`), **`CU-06 Auditar trazabilidad de causa raíz`**, **`CU-07 Gestionar alerta operativa`** y **`CU-08 Exportar reporte trimestral`**.
>
> **Decisión arquitectónica clave que defendemos aquí:** Los desarrolladores **no son actores ni tienen cuentas en este sistema (`RF-030`)**. Ellos siguen trabajando sin fricción en Redmine, GitLab y GLPI.
>
> Doy paso a mi compañero **Alex Gutiérrez**, quien explicará cómo estructuramos el dominio en la **Vista Lógica**.»

---

## BLOQUE 2 — Vista Lógica: Diagrama de Clases y Diagrama de Estados
**Expositor:** **Alex Daniel Gutiérrez Córdova**

### 2.1 Diagrama 5: Diagrama de Clases del Dominio UML 2.5.1 (`2.5 min`)
- **Imagen a mostrar:** [`docs/uml/vista-logica-clases.drawio.png`](../uml/vista-logica-clases.drawio.png)

> **Guión hablado:**
> «Gracias, Carlos. En la **Vista Lógica** diseñamos la estructura estática del dominio aplicando análisis gramatical y la tarjeta de filtro de entidades: descartamos elementos visuales de interfaz (como *tablero* o *mapa de calor*) y sistemas externos, quedándonos con **16 clases de entidad (`«entity»`)**, **40 operaciones públicas** y **10 enumeraciones**, organizadas en **4 paquetes cohesivos**:
>
> 1. **Paquete 1 — Planificación y Jerarquía (`POA/PETI`):** Modela los primeros 3 niveles jerárquicos: `ModuloFinanciero` (restringido a los 6 sistemas core) agrupa por agregación (`◇`) a `ProyectoPOA` (Nivel 1), que compone fuertemente (`◆`) a `HistoriaSpec` (`REQ` o `INI.TI`, Nivel 2), la cual desglosa `TareaIssue` (`#ID`, Nivel 3), donde se convierte la complejidad a Fibonacci (`2, 5 o 13 pts`) y se valida el respaldo en código.
> 2. **Paquete 2 — Ejecución Técnica y Calidad:** Contiene a `Desarrollador`, `ActividadCodigo` (Nivel 4 en GitLab), `IncidenteSoporte` (GLPI) y `ReporteCalidad` (SonarQube).
>    - **Aquí defendemos dos decisiones críticas de diseño:**
>      - **Primera (Asociación por Roles en lugar de Herencia):** Un mismo `Desarrollador` puede ser *asignado* en Redmine, *autor de commit* en GitLab, *revisor de QA cruzado* (`+2 h / 1 pt`) y *responsable de soporte GLPI* (`+4 h` estándar u `+8 h` crítico core). En vez de crear subclases rígidas por herencia, usamos **asociaciones etiquetadas por rol**, lo que además permite auditar cuándo el autor del código difiere del asignado en Redmine (`RF-017`).
>      - **Segunda (Multiplicidad `0..1` hacia `TareaIssue` para Trabajo Huérfano):** Tanto `ActividadCodigo` como `IncidenteSoporte` tienen multiplicidad `0..1` hacia `TareaIssue`. Cuando es `0`, el sistema **no descarta el trabajo**: lo clasifica como *«Trabajo en código sin trazabilidad planificada»* (`RF-021`) o *«Incidente de soporte sin trazabilidad en desarrollo»* (`RF-022`) y suma igualmente sus horas en la carga semanal de 40 h del desarrollador.
> 3. **Paquete 3 — Trazabilidad, 5 Etapas y Métricas:** Su núcleo es `ExpedienteTrazabilidad`, que exige `anonimizadoLOPDP = verdadero` (*Privacy by Design*) y consolida con `0 %` de duplicados mediante llave natural compuesta (`RNF-009`). Compone fuertemente (`◆`) de `1..5` `HitoEtapa` (las 5 etapas oficiales), registra `RetornoFallo` (`-10 pts` por retroceso desde Certificación) y sella `IndicadorDesempeno` con la fórmula `70 % avance + 30 % calidad` y huella inmutable de 12 meses (`RNF-001`).
> 4. **Paquete 4 — Alertas, Gestión y Auditoría PMO:** Modela `AlertaOperativa` (evaluación `< 200 ms` y supresión anti-spam de 24 horas), que compone a `DespachoNotificacion` (mensajería y conmutación a correo sobre `TLS >= 1.2`), junto con `UsuarioGestion` y `ReporteTrimestral`.»

---

### 2.2 Diagrama 6: Diagrama de Estados UML 2.5.1 (`1 min`)
- **Imagen a mostrar:** [`docs/uml/diagrama-estados.png`](../uml/diagrama-estados.png)

> **Guión hablado:**
> «Complementamos el modelo de clases con el **Diagrama de Estados UML 2.5.1** para las tres entidades con ciclo de vida crítico:
> 1. En **`ExpedienteTrazabilidad`** vemos cómo ante la caída de una sola fuente externa tras 3 reintentos (`[2a]`), pasa al estado `SincronizacionParcialSnapshot` reutilizando el último snapshot válido sin degradar falsamente las tareas a trabajo sin trazabilidad; mientras que si falla la compuerta LOPDP (`[3a]`), transiciona a `AbortadoPorRiesgoLOPDP` descartando el lote antes de tocar la base de datos.
> 2. En **`TareaIssue / HitoEtapa`** modelamos el embudo de 5 etapas (`Análisis` ➔ `Desarrollo` ➔ `Certificación` ➔ `Despliegue` ➔ `Estabilización`) y el bucle de retroceso `[5b]` desde Certificación a Desarrollo que descuenta `-10 puntos`.
> 3. En **`AlertaOperativa`** modelamos la transición hacia `Suprimida24h`, `Emitida`, `EntregaFallida` (`5a`) y el paso a `EnSeguimiento` o `Atendida` (`CU-07`), que detiene recordatorios repetidos.
>
> A continuación, mi compañero **Luis Carrión** explicará la **Vista de Procesos y los Diagramas de Secuencia**.»

---

## BLOQUE 3 — Vista de Procesos: Concurrencia, Análisis ECB y Diagramas de Secuencia
**Expositor:** **Luis Carrión**

### 3.1 Arquitectura de 3 Procesos y Reglas de Robustez ECB (`45 s`)
> **Guión hablado:**
> «Gracias, Alex. En la **Vista de Procesos** resolvemos la concurrencia y el comportamiento dinámico en tiempo de ejecución. Dividimos el sistema en **3 procesos independientes**:
> 1. **Proceso 1 (Orquestador ETL):** corre cada 5 minutos con 4 hilos paralelos de extracción en solo lectura (`timeout <= 10 s` y 3 reintentos).
> 2. **Proceso 2 (Motor de Alertas):** se dispara de manera **asíncrona (`->>`)** al terminar `CU-01`, para que una lentitud o caída del correo o mensajería jamás bloquee el ciclo ETL.
> 3. **Proceso 3 (Servicio Consultivo Web):** atiende a la PMO leyendo únicamente el almacenamiento ya consolidado (`<= 2 s` en carga y `<= 400 ms` en filtros), totalmente aislado de Redmine o GitLab.
>
> Todos los diagramas de secuencia aplican el patrón **Entity–Control–Boundary (`«boundary»`, `«control»`, `«entity»`) con `0` violaciones de robustez** y están **sincronizados 1:1 en las 51 invocaciones** con los nombres exactos de clases y métodos de la Vista Lógica.»

---

### 3.2 Diagramas 7 y 8: Secuencias `CU-01` (Parte 1/2 y Parte 2/2) (`1 min`)
- **Imágenes a mostrar:** [`docs/uml/secuencia-cu01a-extraccion-privacidad.png`](../uml/secuencia-cu01a-extraccion-privacidad.png) y [`docs/uml/secuencia-cu01b-calculo-persistencia.png`](../uml/secuencia-cu01b-calculo-persistencia.png)

> **Guión hablado:**
> «Por claridad visual dividimos `CU-01` en dos partes conectadas:
> - En **`CU-01 (Parte 1/2 — Extracción y Privacidad)`**, el actor `Tiempo` activa `TemporizadorCron` (`«boundary»`) y `ControladorSincronizacion` (`«control»`). Dentro del fragmento **`par`**, los cuatro adaptadores consultan en paralelo a `Redmine`, `GLPI`, `GitLab` y `SonarQube`. Con **`opt [2a]`** manejamos la contingencia de snapshot parcial si cae una fuente; con **`break [2b]`** abortamos si caen todas; y luego ejecutamos `ExpedienteTrazabilidad.verificarCompuertaLOPDP()`, con un **`break [3a]`** que aborta todo el lote si no se garantiza el `100 %` libre de PII.
> - En **`CU-01 (Parte 2/2 — Correlación, Fórmulas y Persistencia)`**, dentro del bucle por módulo financiero invocamos los métodos exactos de las entidades: `normalizarReferenciaGLPI()`, `enlazarIncidenteGLPI()`, `registrarRetornoADesarrollo()`, `clasificarCodigoSinTrazabilidad()`, `validarRespaldoEnCodigo()`, `calcularCargaLaboral40h()`, `determinarSemaforoCapacidad()` y `calcularEficiencia7030()`. Finalmente, en un bloque **`critical`** ejecutamos la consolidación atómica idempotente (`unificarHistorial4Niveles()` y `sellarRastroInmutable()`), y disparamos con flecha **asíncrona (`->>`)** a `ControladorEmisionAlertas (CU-02)` y el push incremental del tablero.»

---

### 3.3 Diagrama 9: Secuencia `CU-02` — Emitir alerta operativa (`45 s`)
- **Imagen a mostrar:** [`docs/uml/secuencia-cu02-emitir-alerta.png`](../uml/secuencia-cu02-emitir-alerta.png)

> **Guión hablado:**
> «En la secuencia de **`CU-02`**, `ControladorEmisionAlertas` invoca `AlertaOperativa.evaluarUmbralEn200ms()` y luego `verificarSupresion24Horas()`.
> - En el fragmento **`alt`**, si la alerta ya fue enviada en las últimas 24 horas o fue marcada como atendida en `CU-07` (`[3a]`), se suprime y solo se registra en bitácora.
> - Si es nueva, se despacha a través de `AdaptadorCanalMensajeria` sobre `TLS >= 1.2`. Si el canal de mensajería falla (`[4a]`), el controlador ejecuta `DespachoNotificacion.escalarACorreoPorFallo()` conmutando automáticamente hacia `AdaptadorServicioCorreo`, y cierra invocando `registrarBitacora100PorCiento()` (`RNF-004`).»

---

### 3.4 Diagramas 10, 11 y 12: Secuencias `CU-04`, `CU-05` y `CU-06` (`1 min`)
- **Imágenes a mostrar:**
  - [`docs/uml/secuencia-cu04-consultar-tablero.png`](../uml/secuencia-cu04-consultar-tablero.png)
  - [`docs/uml/secuencia-cu05-filtrar-tablero.png`](../uml/secuencia-cu05-filtrar-tablero.png)
  - [`docs/uml/secuencia-cu06-auditar-causa-raiz.png`](../uml/secuencia-cu06-auditar-causa-raiz.png)

> **Guión hablado:**
> «Las tres secuencias consultivas del `Jefe de Desarrollo / PMO` muestran cómo garantizamos los tiempos de respuesta:
> - En **`CU-04 (Consultar tablero)`**, tras validar `autenticarSesionTLS13()`, `ControladorConsultaTablero` recupera las proyecciones consolidadas en `<= 2 segundos (p95)` resaltando cuellos de botella en rojo (`evaluarCuelloBotellaEmbudo()` y `detectarAtrasoCertificacion()`). Además incluye el fragmento asíncrono de refresco incremental en `< 1 s` cada 5 minutos y el cierre determinista en `< 200 ms` (`RNF-011`).
> - En **`CU-05 (Filtrar indicadores)`**, `PanelFiltroTablero` abre controles en `< 300 ms`, rechaza rangos de fecha inválidos con **`break [3a]`** sin alterar la vista, y recalcula la intersección multidimensional en `<= 400 ms (p95)` manejando cero coincidencias (`[4a]`) y retiro selectivo de etiquetas individuales (`RF-028`).
> - En **`CU-06 (Auditar causa raíz)`**, `ControladorAuditoriaCausaRaiz` verifica la ventana de retención LOPDP de 24 meses (`break [2b]`), reconstruye los 4 niveles o el trabajo huérfano en `< 10 segundos`, exhibe brechas de evidencia y autoría lado a lado (`opt [3a]`) y comprueba la huella matemática inmutable (`break [4a]`).
>
> Doy paso nuevamente a **Carlos Valladares** para cerrar con la **Vista de Desarrollo y la Vista Física de Despliegue**.»

---

## BLOQUE 4 — Vista de Desarrollo (Componentes y Paquetes) y Vista Física (Despliegue)
**Expositor:** **Carlos Valladares**

### 4.1 Diagrama 13: Diagrama de Componentes e Interfaces (`1 min`)
- **Imagen a mostrar:** [`docs/uml/diagrama-componentes-desarrollo.png`](../uml/diagrama-componentes-desarrollo.png)

> **Guión hablado:**
> «Gracias, Luis. En la **Vista de Desarrollo** definimos primero la **Arquitectura de Componentes UML 2.5.1** con **12 componentes lógicos** conectados exclusivamente mediante **puertos e interfaces provistas (`lollipop`) y requeridas (`socket`)** para cumplir `RNF-016` (Mantenibilidad):
> - El frontend `WebAppTableroUnico` consume por HTTPS TLS 1.3 a los módulos de dominio.
> - Los 4 componentes de dominio (`ModuloPlanificacionPOA`, `ModuloEjecucionTecnica`, `ModuloTrazabilidadYMetricas` y `ModuloAlertasYGobiernoPMO`) **no conocen directamente a PostgreSQL, ni a Redmine, ni al servidor de correo**. Se comunican únicamente a través de 6 contratos: `IExtractorSoloLectura`, `IServicioPrivacidad`, `IRepositorioConsolidado`, `IRegistroAuditoria`, `IBusEventosDominio` e `IPasarelaNotificaciones`.
> - Gracias a esto, si mañana la cooperativa cambia de proveedor de mensajería o actualiza la API de GitLab, se modifica únicamente el adaptador en la capa utilitaria y se tocan **`0` líneas de código en el dominio**.»

---

### 4.2 Diagrama 14: Diagrama de Paquetes en 3 Capas Acíclicas (`1 min`)
- **Imagen a mostrar:** [`docs/uml/diagrama-paquetes-desarrollo.png`](../uml/diagrama-paquetes-desarrollo.png)

> **Guión hablado:**
> «En el **Diagrama de Paquetes** organizamos el repositorio Git 1:1 en un **Monolito Modular de 3 Capas** (`«Domain»` ➔ `«Utility»` ➔ `«Platform»`, justificado en nuestro **ADR-001** frente a microservicios para un equipo de 3 integrantes y atomicidad `RNF-009`):
> - **Capa 1 (`«Domain»`):** los 4 paquetes de negocio en orden topológico estricto (`AlertasYGobiernoPMO` ➔ `TrazabilidadYMetricas` ➔ `EjecucionTecnica` ➔ `PlanificacionPOA`).
>   - **¿Cómo rompimos el ciclo entre `CU-01` y `CU-02` (`ADR-003`)?** Al terminar `CU-01`, `TrazabilidadYMetricas` debe disparar a `AlertasYGobiernoPMO`. Si lo importara directamente, crearía una dependencia circular prohibida. Lo resolvimos con inversión de dependencias: `TrazabilidadYMetricas` publica un evento asíncrono hacia el utilitario inferior `Notificaciones`, y `AlertasYGobiernoPMO` se suscribe a él. Resultado: **36 relaciones auditadas y `0` ciclos (DAG estricto)**.
> - **Capa 2 (`«Utility»`):** 7 utilitarios transversales (`Seguridad`, `Persistencia`, `Auditoria`, `IntegracionExterna`, `Notificaciones`, `Validaciones` y `Reportes`) que superaron la prueba de neutralidad (cero reglas de negocio de la cooperativa).
> - **Capa 3 (`«Platform»`):** `PostgreSQL 16`, `Valkey 8.0`, `Nginx 1.26`, `NestJS 11`, `React 19 / Next.js 15` y las APIs externas. Además, dejamos configuradas reglas ejecutables con `dependency-cruiser` en CI/CD que bloquean cualquier commit que viole el Principio Gravitacional o de Aislamiento.»

---

### 4.3 Diagramas 15, 16 y 17: Diagrama de Despliegue (Vista Física) (`1.5 min`)
- **Imagen principal a mostrar:** [`docs/uml/diagrama-despliegue-produccion.png`](../uml/diagrama-despliegue-produccion.png)
- **Imágenes de apoyo:** [`docs/uml/diagrama-artefactos-manifest.png`](../uml/diagrama-artefactos-manifest.png) y [`docs/uml/diagrama-despliegue-desarrollo.png`](../uml/diagrama-despliegue-desarrollo.png)

> **Guión hablado:**
> «Finalmente, cerramos el Modelo 4+1 con la **Vista Física de Despliegue UML 2.5.1**:
> 1. Primero, mediante el diagrama de **`«manifest»`**, empaquetamos los 12 componentes en **5 artefactos físicos instalables (`«artifact»`)**: el bundle `trazabilidad-web`, dos imágenes OCI de contenedores Docker (`trazabilidad-api` consultiva y `trazabilidad-etl-worker`), `colas-bullmq-snapshot` y `esquema-trazabilidad`, parametrizados por `config-produccion` o `config-dev` sin recompilar código.
> 2. En el **Diagrama de Despliegue en Producción**, cumplimos dos requisitos que parecían opuestos: **la PMO necesitaba consultar el tablero desde su casa o celular sin depender de VPN**, pero **la base de datos y las herramientas TI debían permanecer en una red privada bajo soberanía LOPDP 2021**:
>    - **Zona 1 (Internet Público sin VPN):** el navegador directivo (`1..25 sesiones`) se conecta directamente por **`HTTPS TLS 1.3 (:443)`**.
>    - **Zona 2 — Borde DMZ Público:** únicamente el `Servidor Web / Proxy Inverso` (**Par HA Activo-Pasivo con `Nginx 1.26 LTS + WAF`**) está expuesto a Internet en el puerto `:443`.
>    - **Zona 2 — Subred Interna Privada de Servidores y Datos (0 exposición directa a Internet):** detrás del proxy corren **2 instancias activas *stateless*** del `Servidor de Aplicaciones Web` (`trazabilidad-api`), separadas físicamente del `Servidor Worker ETL y Alertas` (`trazabilidad-etl-worker`, Activo-Standby), `Valkey 8.0` y el clúster **`PostgreSQL 16` con cifrado en reposo**.
>    - **Mitigación de Puntos Únicos de Falla (SPOF):** eliminamos el SPOF de base de datos incorporando un **`Servidor de Base de Datos Réplica (Hot Standby)`** con *Streaming Replication* en tiempo real y respaldos diarios `PITR` (retención de 24 meses).
>    - **Zona 3:** contiene el Directorio de Identidad `LDAP/OIDC` (`CU-03`), las 4 plataformas origen accedidas exclusivamente desde el Worker en solo lectura y las pasarelas de Mensajería y Correo (`TLS >= 1.2`).
>
> Con esto demostramos una trazabilidad completa de extremo a extremo: desde la Cadena de Valor y los 58 requisitos, pasando por los 8 Casos de Uso, las 16 Clases del Dominio, los 6 Diagramas de Secuencia ECB, los 12 Componentes y Paquetes acíclicos, hasta su despliegue físico seguro y sin SPOF. Muchas gracias, quedamos atentos a sus preguntas.»

---

## Banco de Preguntas de Defensa (Q&A Rápido para el Equipo)

| # | Posible Pregunta del Docente | Respuesta Corta y Contundente (Con Evidencia del DAS) | Quién Responde |
|:---:|---|---|---|
| **1** | *«¿Por qué la PMO entra sin VPN si ustedes dicen que es un despliegue On-Premise con red privada?»* | «Igual que la banca web de la cooperativa: solo el **Proxy Inverso (`Nginx + WAF`) en la DMZ** tiene IP pública en el puerto `HTTPS TLS 1.3 (:443)` exigiendo autenticación institucional (`CU-03`). La base de datos `PostgreSQL`, `Valkey`, el `Worker ETL` y `Redmine/GitLab` viven en la **Subred Interna Privada** sin ruta directa desde Internet.» | **Carlos Valladares** |
| **2** | *«¿Qué pasa si GitLab o Redmine se caen a mitad de un ciclo de 5 minutos? ¿Las tareas aparecen como "sin respaldo en código"?»* | «No. Según el **Flujo Alterno `2a` (`RNF-008`)** y el estado `SincronizacionParcialSnapshot`, el adaptador espera máximo `10 segundos`, reintenta `3 veces` y, si esa fuente sigue caída, **reutiliza su último snapshot válido consolidado** con su marca temporal. Está prohibido degradar falsamente tareas de una fuente caída; el tablero sigue operando al `100 %` en `<= 2 s`.» | **Luis Carrión / Carlos** |
| **3** | *«¿Por qué un Desarrollador no hereda de Usuario o por qué no hay herencia `DesarrolladorGitLab`, `DesarrolladorRedmine`?»* | «Por dos razones de dominio: (1) los desarrolladores **no tienen cuenta ni inician sesión en este sistema (`RF-030`)**, a diferencia de `UsuarioGestion` (Jefe/PMO); y (2) un mismo desarrollador cumple varios roles simultáneos en la misma semana (asignado en Redmine, autor en GitLab, revisor QA `+2 h` y soporte GLPI `+4 h/8 h`). Modelarlo con **asociaciones etiquetadas por rol** evita explosión de subclases y permite auditar divergencias autor vs. asignado (`RF-017`).» | **Alex Gutiérrez** |
| **4** | *«¿Cómo evita su arquitectura que un desarrollador digite "90 % de avance" en Redmine sin haber subido código?»* | «Mediante la operación `TareaIssue.validarRespaldoEnCodigo()` (`RF-038`, Flujo `7a`): reportar `> 25 %` exige rama activa y commits tipados (`feat/fix/test/refactor(#ID)`); reportar `100 %` exige Merge Request aprobado por QA cruzado y pipeline CI/CD exitoso. Si no existe esa evidencia en `ActividadCodigo`, se marca `tieneBrechaEvidencia = verdadero` y se calcula el avance real solo con el progreso verificado en código.» | **Alex Gutiérrez** |
| **5** | *«¿Qué ocurre si un programador sube commits a GitLab o atiende un ticket GLPI pero olvidó crear la tarea en Redmine?»* | «Gracias a la multiplicidad opcional **`0..1`** entre `TareaIssue` y `ActividadCodigo` / `IncidenteSoporte`, ese esfuerzo **nunca se pierde**: se segrega automáticamente como *«Trabajo en código sin trazabilidad planificada»* (`RF-021`) o *«Incidente de soporte sin trazabilidad en desarrollo»* (`RF-022`), suma sus horas a la carga semanal de 40 h y queda visible para auditoría en `CU-06`.» | **Alex Gutiérrez / Luis** |
| **6** | *«¿Cómo garantizan que `CU-01` (en `TrazabilidadYMetricas`) dispare a `CU-02` (en `AlertasYGobiernoPMO`) sin crear un ciclo entre paquetes?»* | «Aplicando el **ADR-003 (Inversión de Dependencias por Eventos)**: `TrazabilidadYMetricas` no importa a `AlertasYGobiernoPMO`. Publica asíncronamente `CicloConsolidadoEvento` hacia la interfaz `IBusEventosDominio` en el utilitario inferior `Notificaciones` (`BullMQ/Valkey`), y `AlertasYGobiernoPMO` se suscribe. Así mantenemos un **Grafo Acíclico Dirigido (DAG) con `0` ciclos**, verificado automáticamente por `dependency-cruiser`.» | **Carlos Valladares / Luis** |
| **7** | *«¿Cómo cumplen la LOPDP Ecuador 2021 si en los tickets de soporte GLPI los usuarios suelen escribir cédulas o números de cuenta de socios?»* | «Con **Privacy by Design (`RNF-017`, `ADR-004`)**: antes de correlacionar o guardar en disco, `ExpedienteTrazabilidad.verificarCompuertaLOPDP()` invoca síncronamente a `IServicioPrivacidad`. Si por cualquier fallo no se puede garantizar el `100 %` de enmascaramiento de PII, se activa la **Excepción `3a` (`break`)** que aborta todo el lote en memoria antes de persistir.» | **Carlos Valladares** |
| **8** | *«¿Por qué eligieron Monolito Modular con contenedores Docker y no Microservicios en Kubernetes?»* | «Por adecuación arquitectónica (**ADR-001** y comparativa de Vista Física): tenemos un equipo de **3 desarrolladores**, una carga acotada (`<= 25` sesiones concurrentes y `<= 5.000` registros) y una exigencia dura de **transacciones atómicas idempotentes (`0 %` duplicados, `RNF-009`)** y baja latencia (`<= 400 ms` en filtros). El Monolito Modular desplegado en dos contenedores (`trazabilidad-api` y `trazabilidad-etl-worker`) da aislamiento físico de procesos sin pagar la complejidad de red ni el costo `OpEx` de Kubernetes, quedando 100 % listo para migrar en el futuro con `OpenTofu`.» | **Carlos Valladares** |
