# Guión Maestro Extenso de Presentación, Explicación Paso a Paso de Diagramas y Diccionario de Terminología — DAS

- **Proyecto:** Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)
- **Asignatura y Docente:** Arquitectura de Software (`DSOF_4069` — UTPL) · Mgtr. Wayner Xavier Bustamante Granda
- **Fecha de Defensa:** Lunes 23/12/2025
- **Presentación en Google Slides (16 Diapositivas):** https://docs.google.com/presentation/d/1XhC_nUP-Cw_9Y_JT10V-KC8nJarB7jXlBdomDe8beUM/edit
- **Respaldo Local PowerPoint (`.pptx`):** [`docs/entregables/Presentacion_Defensa_DAS_COAC_Padre_Julian_Lorente.pptx`](Presentacion_Defensa_DAS_COAC_Padre_Julian_Lorente.pptx)
- **Informe Consolidado Word (51 págs.):** [`docs/entregables/Informe_Final_DAS_COAC_Padre_Julian_Lorente.docx`](Informe_Final_DAS_COAC_Padre_Julian_Lorente.docx)
- **Equipo Expositor (3 Integrantes):**
  1. **Carlos Valladares** *(Líder Técnico — Contexto, TOGAF: Cadena/Flujo/Mapa de Capacidades, Vista de Escenarios `+1`, Vista de Desarrollo y Vista Física)*
  2. **Alex Daniel Gutiérrez Córdova** *(Arquitecto de Dominio — Vista Lógica: Diagrama de Clases en 4 Paquetes, Diagrama de Estados y Diseño Detallado)*
  3. **Luis Carrión** *(Arquitecto de Procesos — Vista de Procesos: Concurrencia de 3 Procesos, Robustez ECB y los 6 Diagramas de Secuencia)*

---

## PARTE 1 — Diccionario Completo de Terminología (Explicado en Español Sencillo)

> **Propósito de esta sección:** Antes de exponer, lee este glosario. Aquí está explicado **qué significa cada palabra técnica o financiera** que aparece en los diagramas para que ninguno de los tres memorice a ciegas y puedan responder cualquier pregunta del ingeniero con total seguridad.

### 1.1 Términos del Negocio Financiero (COAC Padre Julián Lorente)

| Término / Sigla | ¿Qué significa en palabras sencillas? | ¿Cómo se usa en nuestro proyecto? |
|---|---|---|
| **COAC** | **Cooperativa de Ahorro y Crédito** (*Padre Julián Lorente*, con sede en Loja, Ecuador). | Es la institución financiera dueña del caso de estudio. Sus clientes se llaman **socios**. |
| **Core / Core Financiero** | *«Core»* en inglés significa **«Núcleo» o «Corazón»**. El **Core Financiero** es el sistema informático central de un banco o cooperativa donde se procesan las cuentas de ahorro, préstamos, transferencias, cajeros y pagos. Si el *Core* falla, la cooperativa se detiene. | La cooperativa divide su *Core Financiero* en **6 módulos críticos obligatorios (`RF-018`)**: **`FitSwitch`**, **`SWI`**, **`Canales Electrónicos`**, **`Crédito`**, **`Antifraude`** y **`Nómina`**. Todo proyecto, tarea o incidente debe clasificarse obligatoriamente en uno de estos 6 módulos. |
| **Los 6 Módulos del Core (`FitSwitch`, `SWI`, etc.)** | 1. **`FitSwitch`:** El interruptor (*switch*) transaccional que conecta cajeros automáticos (ATM), tarjetas y redes interbancarias.<br>2. **`SWI`:** *Servicios Web de Integración* (bus que comunica sistemas internos y corresponsales).<br>3. **`Canales Electrónicos`:** Banca web y aplicación móvil que usan los socios.<br>4. **`Crédito`:** Módulo de evaluación, otorgamiento y cobro de préstamos.<br>5. **`Antifraude`:** Motor que vigila transacciones inusuales o robos de identidad.<br>6. **`Nómina`:** Procesamiento de pagos de sueldos y roles institucionales. | Son los 6 valores cerrados de la enumeración `ModuloCoreFinanciero`. En el tablero (`CU-04`) se dibuja un **mapa de calor** sobre estos 6 módulos para ver cuál tiene más atrasos o sobrecarga. |
| **POA / PETI (Nivel 1 — Épica)** | - **`POA`:** *Plan Operativo Anual* (proyectos aprobados con presupuesto para el año).<br>- **`PETI`:** *Plan Estratégico de Tecnologías de la Información* (proyectos estratégicos de TI a mediano/largo plazo). | Representan el **Nivel 1 (más alto)** de nuestra jerarquía de 4 niveles (clase `ProyectoPOA`). Ejemplo: *"Modernización de Canales Electrónicos 2026"*. |
| **REQ e INI.TI (Nivel 2 — Historia / Spec)** | - **`REQ`:** *Requerimiento funcional* solicitado por un área de negocio de la cooperativa (ej. Crédito o Captaciones).<br>- **`INI.TI`:** *Iniciativa de TI* propuesta por el propio departamento de tecnología (ej. actualizar seguridad o refactorizar base de datos). | Representan el **Nivel 2** de la jerarquía (clase `HistoriaSpec`). Un proyecto `POA/PETI` se divide en varios `REQ` o `INI.TI`, y en el reporte trimestral (`CU-08`) hay que contar cuántos fueron de negocio (`REQ`) y cuántos de tecnología (`INI.TI`). |
| **Tarea / Issue `#ID` (Nivel 3)** | Es la unidad concreta de trabajo asignada a un programador dentro de **Redmine**, identificada por un número `#ID` (ej. `Tarea #1042`). | Representa el **Nivel 3** de la jerarquía (clase `TareaIssue`). Sobre ella se mide la complejidad Fibonacci (`2, 5, 13`), la etapa del embudo y el `%` de avance. |
| **PMO** | *Project Management Office* (**Oficina de Gestión de Proyectos**). Es el rol directivo que supervisa plazos, presupuestos, cumplimiento del POA/PETI y cuellos de botella. | Junto con el **Jefe de Desarrollo**, conforman el actor primario **`Jefe de Desarrollo / PMO`**. Ambos ven exactamente la misma pantalla web en modo solo lectura (`RF-032`). |
| **LOPDP 2021 y PII** | - **`LOPDP`:** *Ley Orgánica de Protección de Datos Personales* vigente en Ecuador desde 2021.<br>- **`PII`:** *Personally Identifiable Information* (**Datos Personales Sensibles** que identifican a una persona: número de cédula, número de cuenta de ahorros, correo personal o teléfono de un socio). | En los tickets de soporte (`GLPI`), los empleados a veces pegan por error la cédula o cuenta de un socio. Por ley (`RNF-017`), nuestro sistema tiene una **compuerta bloqueante LOPDP** que borra o enmascara (`***`) el **100 %** de esos datos **antes** de guardarlos en la base de datos. |
| **Privacy by Design (Privacidad por Diseño)** | Principio de ingeniería donde la privacidad no es un "parche" visual al final, sino que la base de datos y las clases se diseñan desde cero para que sea **imposible** guardar datos prohibidos. | En nuestro Diagrama de Clases ninguna entidad tiene atributos como `cedulaSocio` o `cuentaSocio`, y `ExpedienteTrazabilidad` exige `anonimizadoLOPDP = verdadero`. |

---

### 1.2 Herramientas Técnicas y Métricas del Equipo de Desarrollo

| Término / Sigla | ¿Qué significa en palabras sencillas? | ¿Cómo se usa en nuestro proyecto? |
|---|---|---|
| **Redmine** | Sistema web open-source de gestión de proyectos y tareas. | Fuente externa #1 (leída cada 5 min en solo lectura): de ahí sacamos las Épicas `POA/PETI`, Historias `REQ/INI.TI`, Tareas `#ID`, quién está asignado, fechas y qué porcentaje de avance reportó manualmente el programador. |
| **GLPI** | Sistema open-source de Mesa de Ayuda (*Help Desk*) y gestión de incidentes de soporte técnico. | Fuente externa #2: de ahí extraemos los tickets de soporte (`GLPI #ID`) asignados a los programadores. Si un ticket no tiene estimación manual, la regla `RF-003` le asigna automáticamente **`4 horas / 3 puntos`** si es normal, u **`8 horas / 5 puntos`** si es un **incidente crítico del core financiero**. |
| **GitLab** | Plataforma donde se guarda el código fuente (`Git`), se revisa el código entre compañeros y se ejecutan pruebas automáticas. | Fuente externa #3 (Nivel 4 de la jerarquía, clase `ActividadCodigo`): de ahí verificamos si realmente existe código para cada tarea `#ID`. |
| **Rama (*Branch*), Commit Tipado y Merge Request (*MR*)** | - **Rama:** Copia de trabajo del código para desarrollar una tarea sin dañar producción.<br>- **Commit tipado:** Guardado de cambios en el código cuyo mensaje empieza obligatoriamente con una categoría y el `#ID` de Redmine: `feat(#ID)` (nueva funcionalidad), `fix(#ID)` (corrección de fallo), `test(#ID)` (prueba unitaria) o `refactor(#ID)` (mejora interna del código).<br>- **Merge Request (`!MR`):** Solicitud formal para fusionar una rama terminada hacia la rama principal tras superar revisión y pruebas. | Sirven para **validar si el avance reportado en Redmine es real (`RF-038`)**:<br>• Si un programador pone en Redmine **`> 25 %`** de avance, el sistema exige que exista **rama activa y commits tipados** en GitLab.<br>• Si pone **`100 %`**, exige que el **Merge Request esté aprobado y el pipeline CI/CD sea exitoso**. Si no, marca **brecha de evidencia**. |
| **QA Cruzado** | *Quality Assurance Cruzado* (Revisión de código entre pares): antes de aprobar un *Merge Request* en GitLab, otro desarrollador del equipo revisa el código de su compañero. | Cada revisión activa de QA cruzado en GitLab le suma **`2 horas y 1 punto Fibonacci`** a la carga laboral semanal del desarrollador revisor (`RF-003`), y nos permite auditar quién revisó el código (`RF-017`). |
| **Pipeline CI/CD** | *Continuous Integration / Continuous Delivery* (Tubería automatizada en GitLab que compila el código y corre pruebas unitarias y de seguridad cada vez que alguien sube un commit). | Para aceptar que una tarea está al `100 %`, su `pipelineCICDExitoso` debe ser `verdadero`. Si falla o la tarea es rechazada en Certificación, se registra un **Retorno por Fallo (`-10 puntos`)**. |
| **SonarQube** | Herramienta de análisis estático de código que escanea los repositorios buscando errores graves (*bugs críticos*) y huecos de seguridad (*vulnerabilidades*). | Fuente externa #4 (clase `ReporteCalidad`): cada *bug* crítico o vulnerabilidad abierta descuenta **`-5 puntos`** sobre el `30 %` de calidad técnica en la fórmula de eficiencia (`RF-005`). |
| **Puntos Fibonacci (`2, 5, 13`)** | En estimación ágil no se usan números lineales (1, 2, 3), sino la sucesión de Fibonacci porque la incertidumbre crece exponencialmente en tareas grandes. | En `RF-001` convertimos automáticamente la complejidad de cada tarea de Redmine a Fibonacci: **Nivel 1 Baja = `2 puntos`**, **Nivel 2 Media = `5 puntos`**, **Nivel 3 Alta = `13 puntos`**. |
| **Capacidad Base 40 h y Semáforo** | Una semana laboral estándar (lunes a viernes, 8 horas diarias) equivale a **`40.0 horas` (`100 %` de capacidad)**. | Sumamos `Tareas Redmine activas + Incidentes GLPI (4h/8h) + QA cruzado (2h)` y dividimos para `40 h` (`RF-002`, `RF-003`):<br>• **Verde (Disponible):** `< 75 %` (`< 30 h`)<br>• **Amarillo (Carga Óptima):** `75 % a 100 %` (`30 h a 40 h`)<br>• **Rojo (Sobreasignado):** `> 100 %` (`> 40 h`, dispara alerta en `CU-02`). |
| **Fórmula de Eficiencia `70 / 30`** | Indicador de `0 a 100 %` que mide el desempeño real combinando velocidad de entrega con calidad técnica (`RF-005`). | $$\text{Eficiencia} = 70\% \times \left(\frac{\text{Avance Real en Código}}{\text{Avance Planificado}}\right) + 30\% \times \max(0,\; 100 - 10\times\text{Retornos} - 5\times\text{Hallazgos Sonar})$$<br>*(Nota clave: las horas de incidentes emergentes de `GLPI` sí suman a la carga de 40 h, pero **se excluyen** de esta fórmula para no castigar injustamente la eficiencia de proyectos planificados).* |
| **Trabajo Huérfano / Sin Trazabilidad (`RF-021` y `RF-022`)** | Es trabajo real que hizo un programador pero al que le falta su vínculo en Redmine. | Hay dos tipos:<br>1. **`RF-021` (Código sin tarea):** subieron ramas/commits a GitLab sin `#ID` válido en Redmine.<br>2. **`RF-022` (Soporte sin tarea espejo):** atendieron un ticket en GLPI sin crear su tarea espejo en Redmine.<br>**Nunca se borran:** se muestran en una bandeja de auditoría y sus horas sí cuentan en la carga de 40 h. |

---

### 1.3 Términos de Arquitectura de Software, UML 2.5.1 e Infraestructura

| Término / Sigla | ¿Qué significa en palabras sencillas? | ¿Dónde aparece y por qué importa? |
|---|---|---|
| **ETL y Temporizador CRON** | - **`ETL`:** *Extract, Transform, Load* (Extraer de las 4 fuentes, Transformar/Cruzar/Limpiar PII y Cargar en la base consolidada).<br>- **`CRON`:** Planificador de tiempo del sistema operativo que dispara una tarea automáticamente cada cierto intervalo. | Nuestro actor primario **`Tiempo (Temporizador CRON)`** dispara el proceso ETL exactamente **cada 5 minutos** (`RF-029`). |
| **Modo Estricto de Solo Lectura (`RNF-003`)** | Conectarse a un sistema externo únicamente con permiso de consulta (`SELECT` / `GET`), teniendo bloqueado por completo crear, editar o borrar datos allí. | Nuestro sistema hace **`0` escrituras** sobre Redmine, GLPI, GitLab y SonarQube. Así jamás puede corromper ni alterar las herramientas diarias de la cooperativa. |
| **Snapshot Parcial (`Flujo Alterno 2a`)** | *Snapshot* significa **«captura o fotografía del último estado válido»**. | Si en un ciclo de 5 minutos **una** plataforma (ej. GitLab) tarda más de `10 segundos` y falla tras `3 reintentos`, el sistema **no** marca las tareas como "sin código": reutiliza la última foto válida (`Snapshot`) de GitLab con su hora exacta y marca el ciclo como `SincronizacionParcialSnapshot` (`RNF-008`). |
| **Idempotencia y Llave Natural Compuesta (`RNF-009`)** | En matemáticas y software, una operación es **idempotente** si ejecutarla 1 vez o 1.000 veces seguidas produce exactamente el mismo resultado, **con `0 %` de registros duplicados**. | Como el ETL corre cada 5 minutos (288 veces al día), usamos la llave compuesta `POA + Spec + Tarea #ID + GLPI #ID + Commit`. Si el registro ya existe, lo actualiza atómicamente en lugar de duplicarlo. |
| **TOGAF ADM** | *The Open Group Architecture Framework — Architecture Development Method*. Es el estándar mundial de **Arquitectura Empresarial** para conectar la estrategia del negocio con los sistemas de software. | Lo usamos en los 3 primeros diagramas: **Cadena de Valor**, **Flujo de Valor** (*Value Stream*) y **Mapa de Capacidades** (*Capability Map*), cubriendo desde la Fase Preliminar y Fase A (Visión), Fase B (Negocio), Fase C (Datos y Aplicaciones), Fase D (Tecnología) hasta Fases E–H (Gobierno y Evolución). |
| **Modelo `4+1` de Philippe Kruchten (ISO/IEC/IEEE 42010)** | Metodología que explica que un sistema complejo no cabe en un solo dibujo, por lo que debe documentarse desde **4 vistas técnicas + 1 vista integradora de escenarios**:<br>1. **Vista Lógica:** Clases y estados del dominio.<br>2. **Vista de Procesos:** Concurrencia, hilos y diagramas de secuencia.<br>3. **Vista de Desarrollo:** Módulos de código, paquetes y componentes.<br>4. **Vista Física:** Servidores, redes y despliegue.<br>**+1. Vista de Escenarios:** Casos de uso que atraviesan y validan las 4 vistas. | Es la columna vertebral de todo nuestro informe DAS y de la presentación. |
| **Patrón ECB (`«boundary»`, `«control»`, `«entity»`)** | Patrón de análisis de robustez usado en la Vista de Procesos:<br>• **`«boundary»` (Frontera):** Interfaces con el exterior (pantallas web, temporizador CRON o adaptadores hacia APIs externas).<br>• **`«control»` (Controlador):** Orquestador que dirige la lógica de un caso de uso.<br>• **`«entity»` (Entidad):** Las 16 clases de dominio que guardan estado y reglas de negocio. | **Reglas que nunca deben violarse (`0` violaciones en nuestras 6 secuencias):**<br>1. Un Actor externo solo puede hablar con un `«boundary»`.<br>2. Un `«boundary»` solo habla con un `«control»` (jamás toca directamente una `«entity»`).<br>3. Solo el `«control»` invoca métodos de las `«entity»`. |
| **Asociación, Agregación (`◇`), Composición (`◆`) y Atributo Derivado (`/`)** | En el Diagrama de Clases UML 2.5.1:<br>• **Asociación (`───`):** Vínculo entre dos clases (ej. `Desarrollador` asignado a `TareaIssue`).<br>• **Agregación (`◇──`, rombo blanco):** Relación Todo-Parte débil (la parte puede existir sin el todo, ej. `ModuloFinanciero ◇── ProyectoPOA`).<br>• **Composición (`◆──`, rombo negro):** Relación Todo-Parte fuerte (si muere el todo, mueren sus partes, ej. `ExpedienteTrazabilidad ◆── HitoEtapa`).<br>• **Atributo Derivado (`/nombre`):** Atributo que lleva una barra `/` adelante porque **se calcula con una fórmula** en lugar de digitarse. | El ingeniero siempre revisa en la defensa que sepan distinguir rombo blanco (`◇`), rombo negro (`◆`), barra de atributo derivado (`/`) y visibilidad (`-` privado, `+` público). |
| **Fragmentos de Secuencia (`par`, `opt`, `alt`, `loop`, `break`, `critical`)** | Cajas con etiquetas en la esquina superior izquierda dentro de los Diagramas de Secuencia UML 2.5.1:<br>• **`par` (Paralelo):** Ejecuta varios bloques al mismo tiempo (las 4 extracciones de APIs).<br>• **`opt` (Opcional):** Un `if` sin `else` (solo entra si se cumple la guarda `[...]`).<br>• **`alt` (Alternativa):** Un `if / else` con caminos separados por una línea punteada.<br>• **`loop` (Bucle):** Repite los mensajes mientras se cumpla la condición.<br>• **`break` (Ruptura / Excepción):** Aborta inmediatamente el escenario cuando ocurre una excepción fatal (ej. `[3a: fallo LOPDP]`).<br>• **`critical` (Región Crítica):** Operación atómica e indivisible (transacción en base de datos). | Permiten demostrar que los diagramas de secuencia no solo cubren el "camino feliz", sino todos los flujos alternos (`2a`, `3a`, `4a`, `6a`, `7a`) y excepciones (`2b`, `3a`, `4a`, `8a`) de los Casos de Uso. |
| **Lollipop (`──()`) y Socket (`──(`) en Componentes** | En el Diagrama de Componentes UML 2.5.1:<br>• **Lollipop / Chupete (`──()`):** **Interfaz Provista** (contrato o servicio que un componente ofrece a los demás).<br>• **Socket / Enchufe (`──(`):** **Interfaz Requerida** (contrato que un componente necesita consumir de otro). | Cuando el enchufe de un componente de Dominio se conecta al chupete de un Utilitario (ej. `IExtractorSoloLectura`), ambos quedan **desacoplados (`RNF-016`)**: puedes cambiar la pieza interna sin romper al consumidor. |
| **DAG (Grafo Acíclico Dirigido), Principio Gravitacional y de Aislamiento** | En el Diagrama de Paquetes:<br>• **`DAG`:** Red de dependencias donde las flechas van en una sola dirección y **jamás forman un ciclo cerrado** (`0` ciclos).<br>• **Principio Gravitacional:** Las dependencias siempre caen hacia abajo (`«Domain» ➔ «Utility» ➔ «Platform»`). Ninguna capa inferior puede importar a una superior.<br>• **Principio de Aislamiento:** Un paquete `«Domain»` tiene prohibido saltar directamente a `«Platform»` (siempre debe pasar por un `«Utility»`). | Garantiza un código limpio, modular y mantenible. En nuestro proyecto se audita automáticamente en cada commit con la herramienta **`dependency-cruiser`**. |
| **ADR (*Architecture Decision Record*)** | **Registro de Decisión Arquitectónica**: ficha formal donde el arquitecto documenta **por qué** eligió una solución (ej. Monolito Modular) frente a otras alternativas descartadas (ej. Microservicios), indicando contexto, decisión y consecuencias. | Incluimos **4 ADRs oficiales** (`ADR-001` Monolito Modular, `ADR-002` Adaptadores y Snapshot Parcial, `ADR-003` Bus de Eventos Asíncronos para romper ciclos, `ADR-004` Compuerta LOPDP y Auditoría SHA-256). |
| **Nodos `«device»`, `«executionEnvironment»`, `«artifact»`, `«deployment spec»` y `«manifest»`** | En la Vista Física (Despliegue UML 2.5.1):<br>• **`«device»`:** El servidor o equipo físico/virtual (ej. `Servidor de Aplicaciones Web`).<br>• **`«executionEnvironment»`:** El software base o motor donde corre el código dentro del servidor (ej. `Docker Engine 27 / Node.js 22 LTS` o `RDBMS PostgreSQL 16`).<br>• **`«artifact»`:** El archivo físico real desplegado (ej. la imagen contenedor `trazabilidad-api`).<br>• **`«deployment spec»`:** El archivo de configuración y parámetros (`config-produccion` o `config-dev`).<br>• **`«manifest»`:** Relación que indica qué componentes lógicos de la Vista de Desarrollo van empaquetados dentro de qué archivo físico `«artifact»`. | En UML 2.5.1 es un error grave meter un componente lógico directamente en un servidor: primero el componente se empaqueta (`«manifest»`) en un artefacto (`«artifact»`), y el artefacto se despliega (`«deploy»`) dentro del `«executionEnvironment»` del nodo `«device»`. |
| **DMZ, WAF, TLS 1.3 y Subred Privada (Acceso sin VPN)** | - **`DMZ` (*Demilitarized Zone* / Zona Desmilitarizada):** Perímetro de borde donde se ubica el único servidor que da la cara a Internet.<br>- **`WAF` (*Web Application Firewall*):** Cortafuegos inteligente que bloquea ataques web.<br>- **`TLS 1.3`:** Protocolo criptográfico moderno que cifra el `100 %` del tráfico HTTPS.<br>- **Subred Interna Privada:** Red aislada detrás de la DMZ a la que nadie desde Internet puede conectarse directamente. | Gracias a esta arquitectura (igual a la de la banca en línea), **la PMO entra desde su casa o celular por Internet público sin instalar VPN**, llegando solo al Proxy `Nginx + WAF` en la DMZ (`:443`), mientras `PostgreSQL 16` y los servidores internos quedan blindados en la subred privada. |
| **Stateless, SPOF, Hot Standby, Streaming Replication y PITR** | - **`Stateless` (*Sin estado en memoria local*):** Las 2 instancias del servidor web no guardan sesiones adentro; si una se apaga, la otra sigue atendiendo al usuario sin que lo note.<br>- **`SPOF` (*Single Point of Failure* / Punto Único de Falla):** Cualquier servidor único cuya caída tumbaría todo el sistema.<br>- **`Hot Standby` y `Streaming Replication`:** Un segundo servidor de base de datos encendido que recibe copia en vivo de cada dato; si el primario falla, asume el control en segundos (*Failover*).<br>- **`PITR` (*Point-in-Time Recovery*):** Respaldos continuos para restaurar la base de datos a cualquier minuto exacto de los últimos 24 meses. | Así demostramos en la Vista Física cómo eliminamos los **SPOF** (tanto en el Proxy de entrada con un Par HA Activo-Pasivo como en la Base de Datos con la Réplica *Hot Standby*). |

---

## PARTE 2 — Guión Extenso Paso a Paso por Diapositiva (Cómo Explicar Cada Diagrama al Detalle)

---

### LÁMINA 1 — Portada e Introducción del Equipo
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 40 segundos)*
- **Qué se ve en pantalla:** Título del proyecto, marco normativo (`Modelo 4+1 de Kruchten`, `TOGAF ADM`, `UML 2.5.1`) y las 3 tarjetas de los integrantes con los módulos que lideró cada uno.

#### Guión Hablado Completo
> «Buenos días/tardes, ingeniero Wayner Bustamante y compañeros. Nosotros somos el grupo encargado del caso de estudio de la **Cooperativa de Ahorro y Crédito (COAC) Padre Julián Lorente**, conformado por **Alex Daniel Gutiérrez Córdova** (Arquitecto de Dominio y Vista Lógica), **Luis Carrión** (Arquitecto de Procesos y Comportamiento Dinámico) y mi persona, **Carlos Valladares** (Líder Técnico, Arquitectura Empresarial TOGAF, Vista de Escenarios, Vista de Desarrollo y Vista Física).
>
> En esta defensa vamos a recorrer de principio a fin nuestro **Documento de Arquitectura de Software (DAS)** siguiendo el **Modelo 4+1 de Philippe Kruchten** bajo el estándar **ISO/IEC/IEEE 42010**, articulando una cadena de trazabilidad ininterrumpida desde la estrategia del negocio en **TOGAF ADM** hasta los contenedores y nodos físicos de despliegue.»

---

### LÁMINA 2 — Contexto COAC: Fragmentación Operativa y Línea Base Arquitectónica
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto)*
- **Qué se ve en pantalla:** Arriba, 3 tarjetas (`1. Problema: 4 Islas de Datos`, `2. Solución No Intrusiva`, `3. Reglas Críticas COAC`). Abajo, 3 indicadores gigantes (`58 Requisitos`, `6 Módulos Core`, `<= 2 s / <= 400 ms`).

#### Guión Hablado Completo
> «Para entender por qué diseñamos esta arquitectura, primero debemos entender el dolor operativo de la cooperativa. Actualmente, el departamento de desarrollo y soporte de la COAC trabaja sobre **cuatro plataformas separadas que no conversan entre sí**:
> 1. En **Redmine** registran el Plan Operativo Anual (`POA/PETI`), los requerimientos funcionales (`REQ`), las iniciativas tecnológicas (`INI.TI`) y las tareas (`#ID`).
> 2. En **GLPI** reciben los tickets de incidentes de soporte y emergencias del sistema financiero.
> 3. En **GitLab** los programadores crean ramas, suben *commits*, abren *Merge Requests*, hacen revisiones de *QA cruzado* y ejecutan pruebas automáticas *CI/CD*.
> 4. Y en **SonarQube** se auditan los *bugs* críticos y las vulnerabilidades de seguridad del código.
>
> **¿Qué problema provocaba esto?** Cuando la Jefatura de Desarrollo o la PMO querían saber si un proyecto de alguno de los **6 módulos del core financiero** (*FitSwitch, SWI, Canales Electrónicos, Crédito, Antifraude o Nómina*) iba atrasado, tenían que cruzar hojas de cálculo a mano. Peor aún: un desarrollador podía digitar en Redmine que su tarea estaba al `80 %` sin haber subido una sola línea de código en GitLab, o podía estar sobrecargado trabajando más de `40 horas` semanales porque nadie sumaba sus horas de soporte en GLPI ni sus revisiones de QA cruzado.
>
> **Nuestra solución arquitectónica se apoya en tres pilares innegociables:**
> - **Primero (Modo Estricto de Solo Lectura — `RNF-003`):** Ejecutamos un motor **ETL automático cada 5 minutos** que solo lee de las 4 herramientas con `0` escrituras en origen, y donde **los desarrolladores no tienen cuentas ni deben llenar nada extra en nuestra plataforma (`RF-030`)**.
> - **Segundo (Cumplimiento Legal LOPDP Ecuador 2021 — `RNF-017`):** Antes de analizar o guardar cualquier dato, una compuerta bloqueante enmascara al `100 %` cualquier cédula, número de cuenta o correo personal de socios que venga en los tickets.
> - **Tercero (Línea Base Cuantitativa Cerrada):** Trazamos **58 requisitos oficiales** (`40 Requerimientos Funcionales` en relación `1:1` con `40 Historias de Usuario` que suman **225 puntos Fibonacci** en `6 Épicas`, más `18 Requerimientos No Funcionales` medibles bajo **ISO/IEC 25010:2023**), garantizando tiempos de respuesta de **`<= 2 segundos`** en la carga del tablero y **`<= 400 milisegundos`** en el filtrado multidimensional.»

---

### LÁMINA 3 — Diagrama 1: Cadena de Valor y Arquitectura Empresarial (TOGAF ADM)
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto y 15 segundos)*
- **Imagen proyectada:** [`docs/cadena-de-valor-togaf-adm.png`](../cadena-de-valor-togaf-adm.png)

#### Cómo leer y señalar el diagrama con el mouse
1. **Señala el gran bloque superior (`ACTIVIDADES PRIMARIAS — FLUJO OPERATIVO DE VALOR · TOGAF Fase B`):** Tiene **6 tarjetas conectadas de izquierda a derecha** por flechas horizontales:
   - Las tarjetas **1, 2 y 3 (en azul claro)** son lo que ya ocurre en las herramientas de origen (`1. Demanda y Origen`, `2. Planificación en Redmine`, `3. Construcción y Calidad en GitLab/SonarQube`).
   - Las tarjetas **4 y 5 (resaltadas en amarillo/ámbar)** son el **motor interno de nuestro sistema** (`4. Correlación Core CU-01` cada 5 min y `5. Métricas y Alertas CU-02`).
   - La tarjeta **6 (azul a la derecha)** es la entrega de valor a la directiva (`6. Gobierno y Decisión PMO: CU-03 a CU-08`).
2. **Señala el bloque inferior (`ACTIVIDADES DE SOPORTE Y ARQUITECTURA TOGAF ADM`):** Contiene las **4 tarjetas transversales** que sostienen a las actividades primarias a través de las fases de TOGAF (`Visión y Gobierno Fases Preliminar, A y G`, `Datos, Aplicaciones y LOPDP Fases B–C`, `Tecnología y Solo Lectura Fase D` y `Resiliencia y Evolución Fases E–H`).

#### Guión Hablado Completo
> «Iniciamos la secuencia oficial de diagramas con la **Cadena de Valor bajo TOGAF ADM**.
>
> En el contenedor superior modelamos las **6 actividades primarias de la Fase B (Arquitectura de Negocio)** siguiendo el flujo horizontal de creación de valor:
> - En los **eslabones 1, 2 y 3** (tarjetas azules de la izquierda) nace la demanda en las Épicas `POA/PETI`, especificaciones `REQ/INI.TI` e incidentes `GLPI` clasificados en los 6 módulos financieros; luego se planifican en **Redmine** convirtiendo su dificultad a escala Fibonacci (`2, 5, 13`) sobre la capacidad base semanal de `40 horas`; y pasan a construcción técnica en **GitLab** y **SonarQube** mediante ramas, commits tipados y QA cruzado.
> - En los **eslabones 4 y 5** (resaltados en color ámbar porque constituyen el corazón diferencial de nuestro sistema) actúa el motor de **Correlación Core (`CU-01`)** cada 5 minutos: extrae en solo lectura, pasa el filtro obligatorio **LOPDP**, enlaza los 4 niveles jerárquicos y alimenta el cálculo de carga real, avance respaldado en código, eficiencia ponderada `70/30`, embudo de 5 etapas y emisión proactiva de alertas anti-spam (`CU-02`).
> - Finalmente, el **eslabón 6** materializa el valor ejecutivo en el **Gobierno y Decisión PMO (`CU-03` a `CU-08`)**, entregando el tablero único, filtros combinados, auditoría de causa raíz y reportes trimestrales.
>
> En el contenedor inferior articulamos las **actividades de soporte a lo largo del ciclo ADM de TOGAF**:
> 1. **Fases Preliminar, A y G (Visión y Gobierno):** definen la transición del estado *As-Is* fragmentado al estado *To-Be* correlacionado, estandarizando los códigos `#ID`, los commits tipados y la reproducibilidad matemática al `100 %`.
> 2. **Fases B y C (Datos, Aplicaciones y LOPDP):** gobiernan la integración desde las 4 fuentes hacia nuestro sistema y el enmascaramiento del `100 %` de datos personales de socios.
> 3. **Fase D (Tecnología y Solo Lectura):** asegura credenciales estrictas de solo lectura, cifrado `TLS 1.3` y disparo periódico por temporizador `CRON`.
> 4. **Fases E a H (Resiliencia y Evolución):** garantizan adaptadores intercambiables, preservación del último *snapshot* válido ante caídas externas y retención histórica de 24 meses.»

---

### LÁMINA 4 — Diagrama 2: Flujo de Valor End-to-End (*TOGAF Value Stream*)
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto)*
- **Imagen proyectada:** [`docs/uml/flujo-de-valor.png`](../uml/flujo-de-valor.png)

#### Cómo leer y señalar el diagrama con el mouse
1. **Arriba (`DETONANTE DE VALOR / TRIGGER`):** Qué dispara la necesidad (solicitud POA/PETI, REQ o incidente GLPI).
2. **Al centro (`ETAPAS SECUENCIALES DEL FLUJO DE VALOR`):** Las 6 etapas indicando explícitamente **`Entrada:`** y **`Salida:`** en cada tarjeta (`1. Capturar Demanda TI` ➔ `2. Ingestar y Proteger CU-01` ➔ `3. Correlacionar 4 Niveles` ➔ `4. Cuantificar Carga y Eficiencia` ➔ `5. Detectar y Alertar CU-02` ➔ `6. Gobernar y Decidir CU-03..08`).
3. **Debajo (`CAPACIDADES HABILITADORAS POR ETAPA`):** Qué capacidad técnica hace posible cada etapa.
4. **Al pie (`PROPUESTA DE VALOR FINAL / VALUE OUTCOME`):** El resultado logrado para la cooperativa.

#### Guión Hablado Completo
> «Mientras la Cadena de Valor nos muestra la visión macro de la organización, el segundo diagrama exigido es el **Flujo de Valor (*Value Stream*) de la Fase B de TOGAF**, donde modelamos extremo a extremo cómo una petición se transforma en decisiones directivas verificables.
>
> El flujo arranca arriba en el **Detonante de Valor (*Trigger*)**: cualquier iniciativa `POA/PETI`, requerimiento `REQ/INI.TI` o incidente crítico `GLPI` cuyo avance y esfuerzo necesitan ser monitoreados en tiempo real. A partir de ahí recorre **6 etapas de agregación de valor**, donde cada etapa define su contrato de **Entrada** y **Salida**:
> 1. **Capturar Demanda TI:** toma las peticiones de origen y entrega como salida una línea base clasificada sin excepción en uno de los 6 módulos del core financiero (`RF-012`, `RF-018`).
> 2. **Ingestar y Proteger (`CU-01`):** extrae desde Redmine, GLPI, GitLab y SonarQube con *timeout* de `10 segundos` y `3 reintentos`, y entrega un lote `100 %` libre de PII de socios gracias a la compuerta LOPDP (`RNF-008`, `RNF-017`).
> 3. **Correlacionar 4 Niveles:** cruza tareas `#ID`, ramas, *Merge Requests*, commits tipados e incidentes `GLPI`, produciendo expedientes idempotentes con `0 %` de duplicados y segregando el trabajo huérfano sin trazabilidad (`RF-021`, `RF-022`).
> 4. **Cuantificar Carga y Eficiencia:** aplica la conversión Fibonacci (`2, 5, 13`), suma la carga semanal sobre `40 horas` (incluyendo `4 h / 8 h` de GLPI y `2 h` de QA cruzado), contrasta el avance reportado frente al código real y calcula la eficiencia `70 % avance + 30 % calidad`.
> 5. **Detectar y Alertar (`CU-02`):** evalúa en menos de `200 milisegundos` si algún desarrollador superó las `40 horas`, si una tarea lleva más de `5 días hábiles` estancada en Certificación o si hubo retornos por fallo, emitiendo alertas claras con supresión anti-spam de `24 horas` hacia Mensajería y Correo Corporativo sobre `TLS >= 1.2`.
> 6. **Gobernar y Decidir (`CU-03` a `CU-08`):** habilita a la PMO a consultar el tablero en `<= 2 s`, filtrar en `<= 400 ms`, auditar causa raíz en `< 10 s` y exportar el reporte trimestral, alcanzando la **Propuesta de Valor Final** (caja verde inferior): control real del portafolio respaldado en código, sostenibilidad del equipo técnico y cumplimiento auditable LOPDP.»

---

### LÁMINA 5 — Diagrama 3: Mapa de Capacidades Institucionales (*Business Capability Map*)
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto)*
- **Imagen proyectada:** [`docs/uml/mapa-de-capacidades.png`](../uml/mapa-de-capacidades.png)

#### Cómo leer y señalar el diagrama con el mouse
- Es un mapa estratificado en **3 niveles horizontales**:
  - **Nivel 1 (Azul arriba — 4 tarjetas):** Capacidades Estratégicas y de Gobierno TI / PMO (lo que usa la jefatura para dirigir).
  - **Nivel 2 (Amarillo al centro — 6 tarjetas en cuadrícula 3×2):** Capacidades Core de Trazabilidad TI (el corazón algorítmico del sistema).
  - **Nivel 3 (Verde abajo — 5 tarjetas):** Capacidades de Soporte, Seguridad y Plataforma (la base legal, criptográfica y de infraestructura).

#### Guión Hablado Completo
> «El tercer artefacto de Arquitectura Empresarial es el **Mapa de Capacidades Institucionales (*Business Capability Map*)**, que responde a la pregunta: *¿Qué capacidades concretas debe poseer la cooperativa para sostener el flujo de valor anterior?* Lo estructuramos en tres estratos:
>
> - **En el Nivel 1 (Estratégico y de Gobierno TI / PMO)** ubicamos las 4 capacidades directivas:
>   1. *Gobierno de Portafolio POA/PETI* por cada uno de los 6 módulos financieros.
>   2. *Gestión de Capacidad y Carga Laboral sobre 40 horas* con semáforos Verde (`< 75 %`), Amarillo (`75 %–100 %`) y Rojo (`> 100 %`).
>   3. *Control de Desempeño y Reporte Trimestral (`CU-08`)* desglosando `POA`, `REQ`, `INI.TI`, `GLPI` y certificaciones a tiempo vs. atrasadas (`> 5 días hábiles`).
>   4. *Auditoría Causal y Transparencia (`CU-06`)* para investigar cualquier desviación en menos de `10 segundos`.
>
> - **En el Nivel 2 (Capacidades Core de Trazabilidad TI)** concentramos las 6 capacidades diferenciadoras que cubren nuestras épicas de transformación (`EP-02`, `EP-03` y `EP-04`):
>   1. *Ingesta Multiplataforma en Solo Lectura (`CU-01`)*.
>   2. *Correlación Jerárquica de 4 Niveles* con cruce tolerante a errores de digitación en códigos `GLPI #ID` (`RF-013`) y captura de trabajo huérfano (`RF-021`, `RF-022`).
>   3. *Validación de Avance Real Respaldado en Código (`RF-038`)*.
>   4. *Cálculo Fibonacci (`2/5/13`) y Eficiencia `70/30` (`-10 pts` por retorno, `-5 pts` por hallazgo en SonarQube)*.
>   5. *Embudo de 5 Etapas Oficiales* (*Análisis, Desarrollo, Certificación, Despliegue y Estabilización*, resaltando cuellos de botella cuando una etapa concentra `> 35 %` de tareas o supera `> 5 días` en Certificación).
>   6. *Detección de Desviaciones y Alertas Anti-Spam de 24 horas (`CU-02`)*.
>
> - **Y en el Nivel 3 (Capacidades de Soporte, Seguridad y Plataforma)** tenemos los 5 cimientos técnicos y normativos: *Protección de Datos LOPDP 2021*, *Identidad y Acceso Web Único sin VPN sobre TLS 1.3 (`CU-03`)*, *Resiliencia Operativa con Snapshot Parcial y Mitigación SPOF*, *Auditoría Matemática Inmutable por 12 meses* y *Consultas de Alto Desempeño sobre Contenedores OCI*.»

---

### LÁMINA 6 — Diagrama 4: Vista de Escenarios (`+1`) — Diagrama de Casos de Uso UML 2.5.1
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto y 30 segundos)*
- **Imagen proyectada:** [`docs/Diagrama de Casos de Uso - Padre Julian.drawio.png`](../Diagrama%20de%20Casos%20de%20Uso%20-%20Padre%20Julian.drawio.png)

#### Cómo leer y señalar el diagrama con el mouse
- **A la izquierda (Actores Primarios — los que inician acciones):**
  - Arriba a la izquierda: `Tiempo (Temporizador CRON)` conectado a `CU-01`.
  - Abajo a la izquierda: `Jefe de Desarrollo / PMO` conectado a `CU-03`, `CU-04`, `CU-05`, `CU-06`, `CU-07` y `CU-08`.
- **Al centro (Rectángulo de frontera del sistema):** Los **8 óvalos de Casos de Uso (`CU-01` a `CU-08`)**. Señala las dos flechas punteadas de extensión **`«extend»`**:
  - `CU-02` extiende a `CU-01` bajo la condición `[umbral crítico superado]`.
  - `CU-05` extiende a `CU-04` bajo la condición `[análisis focalizado]`.
- **A la derecha (6 Actores Secundarios / `«External System»`):** `Servicio de Correo Corporativo` y `Canal de Mensajería Corporativo` (conectados a `CU-02`), y `Redmine`, `GLPI`, `GitLab` y `SonarQube` (conectados a `CU-01`).

#### Guión Hablado Completo
> «Entramos ahora al corazón del **Modelo 4+1 de Kruchten** con la **Vista de Escenarios (`+1`)** y su **Diagrama de Casos de Uso UML 2.5.1**, el cual sintetiza el `100 %` de nuestros `40 RF` y `18 RNF` en **8 Casos de Uso a nivel del mar**.
>
> Primero, observemos la frontera del sistema y sus actores:
> - A la izquierda tenemos únicamente **2 Actores Primarios**:
>   1. **`Tiempo (Temporizador CRON)`:** dispara automáticamente cada 5 minutos el **`CU-01: Sincronizar y consolidar trazabilidad e indicadores`** (que agrupa `21 RF`). En este caso de uso el sistema consulta en paralelo y en modo solo lectura a los 4 actores secundarios inferiores de la derecha: **`Redmine`, `GLPI`, `GitLab` y `SonarQube`**.
>   2. Observemos la relación **`«extend»`** entre `CU-01` y **`CU-02: Emitir alerta operativa`**: ¿por qué es un `«extend»` y no un `«include»`? Porque no en todos los ciclos de 5 minutos se emiten alertas; **solo cuando se cumple la condición de extensión `[umbral crítico superado]`** (`> 40 horas` de carga, `> 5 días hábiles` en Certificación, retorno por fallo, Prioridad 1 o cambio de prioridad de proyecto) se extiende el comportamiento hacia `CU-02`, despachando el aviso a los 2 actores secundarios superiores de la derecha: el **`Canal de Mensajería Corporativo`** y el **`Servicio de Correo Corporativo`**.
> - El segundo actor primario es el **`Jefe de Desarrollo / PMO`**, quien interactúa con los casos de uso directivos:
>   - **`CU-03: Autenticar usuario`:** valida sus credenciales institucionales sobre `HTTPS TLS 1.3` entregando a ambos roles exactamente la misma vista única en solo lectura.
>   - **`CU-04: Consultar tablero de control`:** presenta en `<= 2 segundos` los semáforos de 40 h, tarjetas por desarrollador, mapas de calor por módulo financiero y el embudo de 5 etapas con actualización incremental cada 5 minutos en `< 1 segundo`.
>   - **`CU-05: Filtrar indicadores del tablero`:** extiende mediante **`«extend»`** a `CU-04` bajo la condición `[análisis focalizado]`, recalculando la intersección simultánea por desarrollador, módulo, etapa y rango de fechas en `<= 400 milisegundos` sin parpadeos y permitiendo quitar etiquetas individuales (`RF-028`).
>   - **`CU-06: Auditar trazabilidad de causa raíz`:** despliega en `< 10 segundos` el expediente completo de 4 niveles, las brechas de evidencia o el trabajo huérfano junto a la huella matemática inmutable.
>   - **`CU-07: Gestionar alerta operativa`:** permite marcar alertas como *«En seguimiento»* o *«Atendida»* para detener recordatorios repetidos y confirmar la bitácora al cerrar sesión en `< 200 ms`.
>   - **`CU-08: Exportar reporte trimestral`:** genera en lenguaje ejecutivo el consolidado trimestral de proyectos `POA/PETI`, `REQ`, `INI.TI`, incidentes `GLPI` y certificaciones a tiempo vs. atrasadas.
>
> Aplicando el **Filtro de Relevancia Arquitectónica** (evaluando riesgo técnico, fronteras externas y atributos de calidad), seleccionamos los **5 casos arquitectónicamente significativos** (**`CU-01`, `CU-02`, `CU-04`, `CU-05` y `CU-06`**) para especificarlos a profundidad con la plantilla de Alistair Cockburn y modelar sus diagramas de secuencia.
>
> A continuación, cedo la palabra a mi compañero **Alex Gutiérrez**, quien defenderá la **Vista Lógica**.»

---

### LÁMINA 7 — Diagrama 5: Vista Lógica — Diagrama de Clases del Dominio en 4 Paquetes
- **Expositor:** **Alex Daniel Gutiérrez Córdova** *(Tiempo aprox.: 2 minutos y 30 segundos)*
- **Imagen proyectada:** [`docs/uml/vista-logica-clases.drawio.png`](../uml/vista-logica-clases.drawio.png)

#### Cómo leer y señalar el diagrama con el mouse
- El diagrama está organizado limpiamente en **4 grandes cuadrantes de colores (los 4 Paquetes Lógicos)** más una barra inferior con el **Catálogo de las 10 Enumeraciones**:
  1. **Cuadrante Superior Izquierdo (Azul — `PAQUETE 1: Planificación y Jerarquía POA/PETI`):** `ModuloFinanciero` ➔ `ProyectoPOA` ➔ `HistoriaSpec` ➔ `TareaIssue`.
  2. **Cuadrante Superior Derecho (Verde — `PAQUETE 2: Ejecución Técnica y Calidad`):** `IncidenteSoporte`, `Desarrollador`, `ActividadCodigo`, `ReporteCalidad`.
  3. **Cuadrante Inferior Izquierdo (Amarillo/Crema — `PAQUETE 3: Trazabilidad, 5 Etapas y Métricas`):** `ExpedienteTrazabilidad`, `HitoEtapa`, `IndicadorDesempeno`, `RetornoFallo`.
  4. **Cuadrante Inferior Derecho (Morado/Lila — `PAQUETE 4: Alertas, Gestión y Auditoría PMO`):** `AlertaOperativa`, `DespachoNotificacion`, `UsuarioGestion`, `ReporteTrimestral`.

#### Guión Hablado Completo
> «Muchas gracias, Carlos. En la **Vista Lógica** definimos con qué conceptos y estructura de dominio el sistema soporta los 8 Casos de Uso.
>
> Antes de dibujar, aplicamos una **decomposición gramatical** sobre los escenarios y pasamos cada sustantivo candidato por los **4 criterios de filtrado de entidad** (¿el sistema debe recordarlo a largo plazo?, ¿tiene más de un atributo?, ¿posee identidad y ciclo de vida propio?, ¿participa en al menos un caso de uso?). Gracias a este filtro **descartamos como clases de dominio a los elementos visuales de interfaz gráfica** (como *Tablero*, *Tarjeta*, *Mapa de calor* o *Botón de filtro*), **a los parámetros volátiles** (como *Rango de fechas*) y **a los actores externos**.
>
> El resultado es un modelo altamente cohesivo y sin "clases dios", compuesto por **16 clases de entidad (`«entity»`)**, **40 operaciones públicas (`+`)**, **78 atributos** (distinguiendo con barra inclinada `/` los atributos derivados por fórmula) y **10 enumeraciones cerradas**, distribuidos en **4 paquetes lógicos**:
>
> 1. **En el Paquete 1 — Planificación y Jerarquía (`POA/PETI`, cuadrante azul superior izquierdo):**
>    Modelamos los tres primeros niveles de la jerarquía oficial (`RF-033`):
>    - `ModuloFinanciero` representa los 6 sistemas core (`[1..6]`) y agrupa mediante **agregación (`◇`, rombo blanco)** de `0..*` instancias de `ProyectoPOA` (Nivel 1).
>    - `ProyectoPOA` posee **composición fuerte (`◆`, rombo negro)** hacia `1..*` `HistoriaSpec` (Nivel 2, que distingue entre requerimientos `REQ` e iniciativas `INI.TI`), porque una especificación no existe fuera de su proyecto POA.
>    - `HistoriaSpec` desglosa mediante agregación `0..*` `TareaIssue` (Nivel 3, `#ID` en Redmine), cuyas operaciones públicas `+ validarRespaldoEnCodigo()`, `+ cambiarEtapaOficial()` y `+ enlazarIncidenteGLPI()` convierten la complejidad a Fibonacci (`2, 5 o 13 pts`) y verifican el avance real.
>
> 2. **En el Paquete 2 — Ejecución Técnica y Calidad (cuadrante verde superior derecho):**
>    Modelamos `Desarrollador`, `ActividadCodigo` (Nivel 4 en GitLab), `IncidenteSoporte` (tickets GLPI) y `ReporteCalidad` (análisis de SonarQube que penaliza `-5 puntos` por cada bug crítico o vulnerabilidad).
>    **Aquí defendemos las dos decisiones estructurales más importantes de nuestro diagrama de clases:**
>    - **Primera decisión (Asociación por Roles en lugar de Herencia sobre `Desarrollador`):** En una semana laboral, un mismo programador puede actuar como *responsable asignado* de una tarea en Redmine, *autor real de commits* en GitLab, *revisor de QA cruzado* (`+2 horas / 1 punto`) en el Merge Request de un compañero, y *técnico de soporte* en un incidente GLPI (`+4 horas / 3 puntos` estándar u `+8 horas / 5 puntos` si es crítico del core financiero). Si hubiéramos usado herencia (`DesarrolladorRedmine`, `DesarrolladorQA`, etc.), habríamos creado acoplamiento rígido e imposibilidad de que una misma instancia cumpla varios roles simultáneos. Al modelarlo con **asociaciones etiquetadas por rol**, calculamos limpiamente su carga semanal sobre la base fija `capacidadBaseSemanal = 40.0` (`+ calcularCargaLaboral40h()` y `+ determinarSemaforoCapacidad()`), y además auditamos con `+ auditarDivergenciaAutoria()` cuándo quien subió el código en GitLab no es quien figuraba asignado en Redmine (`RF-017`).
>    - **Segunda decisión (Multiplicidad Opcional `0..1` entre `TareaIssue` y `ActividadCodigo` / `IncidenteSoporte`):** Observemos las dos líneas que cruzan del Paquete 1 al Paquete 2 con cardinalidad **`0..1`** del lado de `TareaIssue`. ¿Por qué `0..1` y no `1` obligatorio? Porque en la vida real los programadores a veces suben commits urgentes a GitLab sin haber creado la tarea en Redmine, o atienden un ticket en GLPI sin tarea espejo. Cuando esa multiplicidad vale `0`, los métodos `+ clasificarCodigoSinTrazabilidad()` (`RF-021`) y `+ clasificarSoporteSinTrazabilidad()` (`RF-022`) capturan ese trabajo huérfano y **suman sus horas a las 40 h semanales** en lugar de rechazar el registro.
>
> 3. **En el Paquete 3 — Trazabilidad, 5 Etapas y Métricas (cuadrante amarillo inferior izquierdo):**
>    - El núcleo es `ExpedienteTrazabilidad`, que consolida cada 5 minutos los 4 niveles sin duplicados mediante `llaveNaturalCompuesta` (`RNF-009`) y aplica **Privacy by Design (`LOPDP 2021`)**: ninguna de las 16 clases tiene campos para datos personales de socios, y la operación `+ verificarCompuertaLOPDP()` garantiza `anonimizadoLOPDP = verdadero` antes de persistir.
>    - `ExpedienteTrazabilidad` **compone fuertemente (`◆`)** de `1..5` `HitoEtapa` (el recorrido por las 5 etapas oficiales donde `+ detectarAtrasoCertificacion()` marca cuello de botella si supera `> 5 días hábiles` en Certificación o `> 2 días` en Despliegue).
>    - De `HitoEtapa` nace `RetornoFallo` (`-10.0 puntos` por cada retroceso desde Certificación/CI-CD hacia Desarrollo), el cual junto con `ReporteCalidad` alimenta a `IndicadorDesempeno`, donde `+ calcularEficiencia7030()` aplica el `70 %` al avance real y el `30 %` a la calidad técnica, y `+ sellarRastroInmutable()` guarda el hash criptográfico de entradas y pesos por al menos **12 meses** (`RNF-001`).
>
> 4. **Finalmente, en el Paquete 4 — Alertas, Gestión y Auditoría PMO (cuadrante morado inferior derecho):**
>    - `AlertaOperativa` evalúa los umbrales en `< 200 ms` (`+ evaluarUmbralEn200ms()`), verifica la ventana anti-spam de 24 horas (`+ verificarSupresion24Horas()`) y gestiona el cambio de estado en `CU-07` (`+ marcarEstadoGestion()`).
>    - Compone fuertemente (`◆`) a `1..*` `DespachoNotificacion` (que envía al Canal de Mensajería sobre `TLS >= 1.2`, conmuta a Correo con `+ escalarACorreoPorFallo()` y persiste el `100 %` en bitácora).
>    - Y cerramos con `UsuarioGestion` (sesión autenticada en solo lectura del Jefe de Desarrollo / PMO con cierre en `< 200 ms`) y `ReporteTrimestral` (`CU-08`).»

---

### LÁMINA 8 — Diagrama 6: Vista Lógica — Diagrama de Máquinas de Estado UML 2.5.1
- **Expositor:** **Alex Daniel Gutiérrez Córdova** *(Tiempo aprox.: 1 minuto)*
- **Imagen proyectada:** [`docs/uml/diagrama-estados.png`](../uml/diagrama-estados.png)

#### Cómo leer y señalar el diagrama con el mouse
- El diagrama contiene **3 máquinas de estado apiladas verticalmente**:
  1. **Arriba (Azul):** Ciclo de vida de `ExpedienteTrazabilidad` durante `CU-01`.
  2. **Al centro (Verde):** Ciclo de vida de `TareaIssue / HitoEtapa` a través del embudo de 5 etapas.
  3. **Abajo (Morado):** Ciclo de vida de `AlertaOperativa` entre `CU-02` y `CU-07`.

#### Guión Hablado Completo
> «Para que ningún flujo alterno o excepción de los Casos de Uso quede en el aire, formalizamos en este **Diagrama de Estados UML 2.5.1** los ciclos de vida de las tres entidades con mayor riqueza conductual:
>
> 1. **En la Máquina de Estados 1 (`ExpedienteTrazabilidad`, bloque superior):**
>    Cada 5 minutos nace en `ExtrayendoSoloLectura`.
>    - Si fallan las 4 fuentes tras 3 reintentos (`[2b]`), pasa a `AbortadoFalloTotal` conservando intacta la línea base previa.
>    - Si cae **solo 1 fuente** tras `10 segundos` y `3 reintentos` (`[2a]`), entra al estado **`SincronizacionParcialSnapshot`**, donde la acción `entry` preserva el último *snapshot* válido de esa herramienta y prohíbe degradar falsamente sus tareas como trabajo sin trazabilidad.
>    - Luego entra a `EvaluandoCompuertaLOPDP`: si el filtro detecta cualquier riesgo de PII sin enmascarar (`[3a]`), salta inmediatamente a **`AbortadoPorRiesgoLOPDP`** y destruye el lote antes de persistir. Si el lote está `100 %` limpio, segrega el `TrabajoSinTrazabilidad` (`[6a/6b]`) y culmina en **`ConsolidadoCompleto`** sellando la huella inmutable de 12 meses.
>
> 2. **En la Máquina de Estados 2 (`TareaIssue / HitoEtapa`, bloque central):**
>    Modela el **Embudo de 5 Etapas Oficiales (`RF-019`)**: `1_Analisis` ➔ `2_Desarrollo` ➔ `3_Certificacion` ➔ `4_Despliegue` ➔ `5_Estabilizacion`. Observemos dos guardas cruciales: para pasar de `Desarrollo` a `Certificación` se exige `[100 % desarrollo: MR aprobado + CI/CD exitoso (RF-038)]`; y si en `Certificación` las pruebas fallan (`[5b]`), la flecha de retorno regresa la tarea a `2_Desarrollo` instanciando un `RetornoFallo` que descuenta `-10 puntos`.
>
> 3. **En la Máquina de Estados 3 (`AlertaOperativa`, bloque inferior):**
>    Inicia en `EvaluandoUmbral` (`< 200 ms`): si ya existe una alerta igual en las últimas `24 horas` o está gestionada (`[3a]`), termina en **`Suprimida24h`** sin molestar a la jefatura. Si ambos canales externos caen (`[5a]`), queda persistida en **`EntregaFallida`** visible en el tablero. Si se despacha con éxito, pasa a **`Emitida`**, desde donde el Jefe de Desarrollo o PMO en `CU-07` la mueve a **`EnSeguimiento`** y finalmente a **`Atendida`**, confirmando la bitácora al cierre de sesión en `< 200 ms`.
>
> Doy paso a mi compañero **Luis Carrión**, quien explicará la **Vista de Procesos y los Diagramas de Secuencia**.»

---

### LÁMINA 9 — Diagramas 7 y 8: Vista de Procesos — Secuencia `CU-01` (Partes 1/2 y 2/2)
- **Expositor:** **Luis Carrión** *(Tiempo aprox.: 1 minuto y 30 segundos)*
- **Imágenes proyectadas:** [`docs/uml/secuencia-cu01a-extraccion-privacidad.png`](../uml/secuencia-cu01a-extraccion-privacidad.png) (izquierda) y [`docs/uml/secuencia-cu01b-calculo-persistencia.png`](../uml/secuencia-cu01b-calculo-persistencia.png) (derecha).

#### Cómo leer y señalar los diagramas con el mouse
- **En el diagrama izquierdo (`CU-01 Parte 1/2 — Extracción y Privacidad`):**
  - Arriba están las líneas de vida (*lifelines*): el actor `Tiempo (Temporizador CRON)`, la frontera `TemporizadorCron` (`«boundary»`), el orquestador `ControladorSincronizacion` (`«control»`), los 4 adaptadores (`AdaptadorRedmine`, `AdaptadorGLPI`, `AdaptadorGitLab`, `AdaptadorSonarQube` como `«boundary»`), los 4 sistemas externos y la entidad `ExpedienteTrazabilidad` (`«entity»`).
  - Señala de arriba hacia abajo los fragmentos: caja **`par`** (con 4 `loop` internos de hasta 3 reintentos y `timeout <= 10 s`), caja **`opt [2a]`** (snapshot parcial), caja **`break [2b]`** (aborto si caen todas las fuentes) y caja **`break [3a]`** (aborto si `verificarCompuertaLOPDP()` devuelve falso).
- **En el diagrama derecho (`CU-01 Parte 2/2 — Correlación, Fórmulas y Persistencia`):**
  - El mismo `ControladorSincronizacion` (`«control»`) invoca en orden a las **9 entidades del dominio** dentro de un **`loop`** por los 6 módulos financieros, entra al bloque **`critical`** para persistir sin duplicados (con **`break [8a]`** si hay error de integridad -> *rollback*), y al final lanza dos flechas **asíncronas (`->>`)** hacia `ControladorEmisionAlertas (CU-02)` y el refresco incremental de `CU-04`.

#### Guión Hablado Completo
> «Muchas gracias, Alex. En la **Vista de Procesos** modelamos cómo se comporta el sistema en tiempo de ejecución, cómo manejamos la concurrencia y cómo se comunican los objetos para cumplir los presupuestos de tiempo (`RNF`).
>
> **Primero, nuestra decisión de concurrencia:** dividimos la ejecución en **3 procesos independientes con memoria propia**:
> - **Proceso 1:** el Orquestador ETL que corre cada 5 minutos (`CU-01`).
> - **Proceso 2:** el Motor de Alertas Multicanal (`CU-02`), invocado de forma **asíncrona (`->>`)** para que si el servidor de correo tarda en responder, jamás frene al ETL.
> - **Proceso 3:** el Servicio Consultivo Web (`CU-04` a `CU-06`), que lee únicamente datos ya consolidados e indexados, logrando que las consultas de la PMO tarden menos de `2 segundos` aunque Redmine o GitLab estén lentos o caídos.
>
> Además, aplicamos estrictamente las reglas de robustez del patrón **ECB (`«boundary»`, `«control»`, `«entity»`) con `0` violaciones** y logramos **sincronización `1:1` en las 51 llamadas a métodos**: cada mensaje enviado a una entidad en nuestras 6 secuencias existe con el mismo nombre exacto y tipo de retorno en el Diagrama de Clases de Alex.
>
> Para que la secuencia de **`CU-01: Sincronizar y consolidar trazabilidad e indicadores`** sea completamente legible, la estructuramos en dos partes:
> - **A la izquierda (`CU-01 Parte 1/2 — Extracción y Privacidad`):**
>   1. El actor `Tiempo (Temporizador CRON)` envía el pulso cada 5 minutos a la frontera `TemporizadorCron`, que delega en `ControladorSincronizacion`.
>   2. Entramos a un fragmento combinado **`par` (Paralelo)**: los cuatro adaptadores (`AdaptadorRedmine`, `AdaptadorGLPI`, `AdaptadorGitLab` y `AdaptadorSonarQube`) consultan simultáneamente en modo solo lectura (`RNF-003`) a sus plataformas. Cada adaptador tiene un **`loop`** de hasta `3 reintentos` con `timeout <= 10 segundos` (`RNF-008`).
>   3. Debajo, el fragmento **`opt [2a]`** implementa nuestra resiliencia: si una sola plataforma falló tras los 3 reintentos, el controlador recupera su último *snapshot* válido sin degradar las tareas. En cambio, si fallaron todas las plataformas, el fragmento **`break [2b]`** aborta el ciclo conservando intactos los datos anteriores.
>   4. Antes de hacer cualquier cálculo, el controlador invoca sobre `ExpedienteTrazabilidad` el método `+ verificarCompuertaLOPDP()`. Si el enmascaramiento del `100 %` de PII de socios falla, el fragmento **`break [3a]`** corta inmediatamente la ejecución en memoria antes de tocar la base de datos (`RNF-017`).
>
> - **A la derecha (`CU-01 Parte 2/2 — Correlación, Fórmulas y Persistencia`):**
>   1. Una vez superada la compuerta LOPDP, entramos a un **`loop`** que recorre las actividades de los 6 módulos financieros:
>      - Invoca `IncidenteSoporte.normalizarReferenciaGLPI()` y `TareaIssue.enlazarIncidenteGLPI()` para el cruce tolerante a errores tipográficos (`Flujo 5a`).
>      - En el fragmento **`opt [5b]`**, si detecta retroceso desde Certificación o CI/CD, invoca `RetornoFallo.registrarRetornoADesarrollo()` y `aplicarDescuentoDiezPuntos()`.
>      - En el fragmento **`alt` de trazabilidad**, si la actividad no tiene tarea en Redmine (`[6a]`) o el ticket GLPI no tiene tarea espejo (`[6b]`), invoca `ActividadCodigo.clasificarCodigoSinTrazabilidad()` o `IncidenteSoporte.clasificarSoporteSinTrazabilidad()` y `asignarEsfuerzoAutomatico()` (`4 h / 8 h`). Si sí tiene vínculo completo, verifica commits `> 25 %` (`verificarCommitsTipados25PorCiento()`), cierre `100 %` (`validarCierreCienPorCiento()`), brecha de evidencia (`validarRespaldoEnCodigo()`, `Flujo 7a`), divergencia de autoría (`Desarrollador.auditarDivergenciaAutoria()`, `Flujo 7b`), normaliza las 5 etapas (`cambiarEtapaOficial()`) y calcula la penalización de SonarQube (`ReporteCalidad.calcularPenalizacionCalidad()`, `-5 pts`).
>      - Luego calcula la carga semanal sobre 40 h (`Desarrollador.calcularCargaLaboral40h()` y `determinarSemaforoCapacidad()`), la desviación del proyecto (`ProyectoPOA.calcularDesviacionAvance()`) y la eficiencia ponderada (`IndicadorDesempeno.calcularEficiencia7030()`).
>   2. Para guardar en base de datos, entramos a una región **`critical` (Transacción Atómica)**: ejecuta `ExpedienteTrazabilidad.unificarHistorial4Niveles()` (idempotente con `0 %` duplicados, `RNF-009`) e `IndicadorDesempeno.sellarRastroInmutable()` (`RNF-001`). Si ocurre un conflicto de escritura, el **`break [8a]`** hace *rollback* total del lote.
>   3. Finalmente, al salir de la región crítica, lanza con flecha **asíncrona (`->>`)** el punto de extensión hacia `ControladorEmisionAlertas (CU-02)` si se superó algún umbral crítico.»

---

### LÁMINA 10 — Diagrama 9: Vista de Procesos — Secuencia `CU-02` (Emitir Alerta Operativa)
- **Expositor:** **Luis Carrión** *(Tiempo aprox.: 45 segundos)*
- **Imagen proyectada:** [`docs/uml/secuencia-cu02-emitir-alerta.png`](../uml/secuencia-cu02-emitir-alerta.png)

#### Cómo leer y señalar el diagrama con el mouse
- Participantes de izquierda a derecha: `ControladorSincronizacion (CU-01)` ➔ `ControladorEmisionAlertas` (`«control»`) ➔ `AlertaOperativa` (`«entity»`) ➔ `DespachoNotificacion` (`«entity»`) ➔ `AdaptadorCanalMensajeria` (`«boundary»`) ➔ `Canal de Mensajería Corporativo` ➔ `AdaptadorServicioCorreo` (`«boundary»`) ➔ `Servicio de Correo Corporativo`.
- Señala el gran bloque **`alt`** principal (Anti-spam `24 h` vs. Alerta nueva) y el sub-bloque **`alt`** de envío (entrega exitosa en mensajería vs. `[4a]` fallo de mensajería con conmutación automática a Correo vs. `[5a]` caída de ambos canales).

#### Guión Hablado Completo
> «En la secuencia de **`CU-02: Emitir alerta operativa`** vemos en acción al **Proceso 2**, activado por el mensaje asíncrono `->>` desde `CU-01`:
> 1. `ControladorEmisionAlertas` invoca `AlertaOperativa.evaluarUmbralEn200ms()` (`RNF-010`). Si la evaluación falla internamente, el **`break [1a]`** registra el error técnico sin emitir falsas alarmas.
> 2. Enseguida invoca `AlertaOperativa.verificarSupresion24Horas()`, entrando al fragmento **`alt`**:
>    - **Primer camino (`[3a: Alerta repetida en < 24 h o marcada EnSeguimiento/Atendida en CU-07]`):** el sistema **suprime el envío externo** para no saturar de spam a la jefatura (`RF-023`, `RF-026`) e invoca `DespachoNotificacion.registrarBitacora100PorCiento()` con estado `Suprimida24h`.
>    - **Segundo camino (`[Alerta nueva o agravamiento crítico]`):** prepara el envío en `DespachoNotificacion.despacharCanalMensajeria()` y lo transmite por `AdaptadorCanalMensajeria` sobre cifrado `>= TLS 1.2` (`RNF-015`).
> 3. Dentro de este envío tenemos dos mecanismos de resiliencia:
>    - Si el `Canal de Mensajería Corporativo` responde OK y además la alerta es de alta severidad (`> 5 días` en Certificación, `Prioridad 1` o `cambio de prioridad`), el fragmento **`opt`** envía copia en paralelo por `AdaptadorServicioCorreo` (`RF-025`).
>    - Pero si el `Canal de Mensajería Corporativo` **falla o da timeout (`[Flujo Alterno 4a]`)**, el controlador invoca automáticamente `DespachoNotificacion.escalarACorreoPorFallo()` y redirige la alerta a través de `AdaptadorServicioCorreo`. E incluso si ambos canales externos estuvieran caídos al mismo tiempo (`[Excepción 5a]`), siempre se ejecuta al final `registrarBitacora100PorCiento()` (`RNF-004`), asegurando `0 %` de pérdida de alertas.»

---

### LÁMINA 11 — Diagramas 10 y 11: Vista de Procesos — Secuencias `CU-04` (Tablero) y `CU-05` (Filtros)
- **Expositor:** **Luis Carrión** *(Tiempo aprox.: 1 minuto)*
- **Imágenes proyectadas:** [`docs/uml/secuencia-cu04-consultar-tablero.png`](../uml/secuencia-cu04-consultar-tablero.png) (izquierda) y [`docs/uml/secuencia-cu05-filtrar-tablero.png`](../uml/secuencia-cu05-filtrar-tablero.png) (derecha).

#### Cómo leer y señalar los diagramas con el mouse
- **Izquierda (`CU-04 Consultar tablero`):** `Jefe de Desarrollo / PMO` ➔ `PanelTableroControl` (`«boundary»`) ➔ `ControladorConsultaTablero` (`«control»`) ➔ `UsuarioGestion`, `ModuloFinanciero`, `HitoEtapa`, `Desarrollador`, `ProyectoPOA` (`«entity»`). Abajo se ven los bloques `opt` para refresco incremental asíncrono (`->>`) cada 5 min en `< 1 s`, extensión a `CU-05` y cierre de sesión en `< 200 ms`.
- **Derecha (`CU-05 Filtrar indicadores`):** `Jefe de Desarrollo / PMO` ➔ `PanelFiltroTablero` (`«boundary»`) ➔ `ControladorFiltradoTablero` (`«control»`) ➔ `UsuarioGestion`, `ModuloFinanciero`, `HitoEtapa`, `Desarrollador`. Destaca el `break [3a]` (fechas inválidas) y el `alt` (con resultados vs. `[4a]` cero coincidencias).

#### Guión Hablado Completo
> «Pasamos al **Proceso 3 (Servicio Consultivo Web)** con los diagramas de secuencia de **`CU-04`** y **`CU-05`**:
> - **A la izquierda (`CU-04: Consultar tablero de control`):**
>   1. El `Jefe de Desarrollo / PMO` solicita el tablero a través de `PanelTableroControl` (`«boundary»`). `ControladorConsultaTablero` verifica primero la sesión con `UsuarioGestion.autenticarSesionTLS13()` (`CU-03`). Si la sesión caducó o no existe aún ningún ciclo inicial consolidado, los fragmentos **`break [2c]`** y **`break [2b]`** informan limpiamente al usuario sin mostrar métricas vacías engañosas.
>   2. Luego invoca `UsuarioGestion.consultarYFiltrarTablero()`, `ModuloFinanciero.evaluarCuelloBotellaEmbudo()`, `HitoEtapa.detectarAtrasoCertificacion()`, `Desarrollador.calcularCargaLaboral40h()`, `determinarSemaforoCapacidad()` y `ProyectoPOA.calcularDesviacionAvance()`. Como todo se lee sobre proyecciones ya consolidadas por `CU-01`, la carga inicial responde en **`<= 2 segundos (p95)`** hasta con `25 sesiones concurrentes` y `5.000 registros` (`RNF-014`), permitiendo identificar en rojo cualquier cuello de botella en **`<= 5 segundos`** (`RNF-005`).
>   3. En la parte inferior modelamos el refresco incremental asíncrono (`->>`) cada 5 minutos en **`< 1 segundo`** (`RF-034`) y el flujo alterno **`[5a]` de cierre de sesión**, donde `UsuarioGestion.gestionarAlertaOperativa()` confirma la bitácora y libera todos los descriptores en **`< 200 milisegundos`** con `0` conexiones huérfanas (`RNF-011`).
>
> - **A la derecha (`CU-05: Filtrar indicadores del tablero`):**
>   1. Cuando el directivo requiere análisis focalizado, `PanelFiltroTablero` despliega los controles en **`< 300 milisegundos`** (`RNF-006`) conservando visibles los indicadores generales de contexto (`RF-008`).
>   2. Al seleccionar criterios combinados (desarrollador, módulo financiero, etapa oficial y rango de fechas), el controlador valida primero las fechas: si la fecha inicial es mayor a la final, el **`break [3a]`** rechaza el filtro sin alterar la vista previa.
>   3. Si el rango es válido, ejecuta `UsuarioGestion.consultarYFiltrarTablero()` recalculando la intersección exacta en **`<= 400 milisegundos (p95)`** (`RNF-018`) junto con los métodos de `ModuloFinanciero`, `HitoEtapa` y `Desarrollador`.
>   4. El fragmento **`alt`** cubre tanto el despliegue de resultados con etiquetas individuales removibles (`RF-028`) como el flujo **`[4a]` de cero coincidencias** (`RF-039`, ofreciendo un botón directo para restablecer la vista global), además de preservar los filtros activos cuando llega una actualización incremental de 5 minutos (`[4b]`).»

---

### LÁMINA 12 — Diagrama 12: Vista de Procesos — Secuencia `CU-06` (Auditar Causa Raíz)
- **Expositor:** **Luis Carrión** *(Tiempo aprox.: 45 segundos)*
- **Imagen proyectada:** [`docs/uml/secuencia-cu06-auditar-causa-raiz.png`](../uml/secuencia-cu06-auditar-causa-raiz.png)

#### Cómo leer y señalar el diagrama con el mouse
- Participantes: `Jefe de Desarrollo / PMO` ➔ `PanelAuditoriaCausaRaiz` (`«boundary»`) ➔ `ControladorAuditoriaCausaRaiz` (`«control»`) ➔ `ExpedienteTrazabilidad`, `TareaIssue`, `Desarrollador`, `IndicadorDesempeno` (`«entity»`).
- Señala en orden descendente: `verificarCompuertaLOPDP()` con **`break [2b]`** (si el expediente tiene `> 24 meses` o no existe), `reconstruirCausaRaiz()` con **`alt`** (expediente de 4 niveles vs. `[2a]` trabajo sin trazabilidad), **`opt [3a]`** (brecha de evidencia y divergencia de autoría lado a lado), y `calcularEficiencia7030()` + `sellarRastroInmutable()` con **`break [4a]`** (corrupción de huella matemática).

#### Guión Hablado Completo
> «Cerramos la Vista de Procesos con la secuencia del quinto caso arquitectónicamente significativo, **`CU-06: Auditar trazabilidad de causa raíz`**:
> 1. Cuando el `Jefe de Desarrollo / PMO` selecciona una tarea atrasada `> 5 días` en Certificación, un desarrollador en semáforo rojo `> 40 h` o un ítem sin trazabilidad, `PanelAuditoriaCausaRaiz` invoca a `ControladorAuditoriaCausaRaiz`.
> 2. Primero se ejecuta `ExpedienteTrazabilidad.verificarCompuertaLOPDP()`: si el expediente superó la ventana legal de retención de `24 meses` o no existe, el **`break [2b]`** detiene la consulta limpiamente.
> 3. Luego invoca `ExpedienteTrazabilidad.reconstruirCausaRaiz()`:
>    - En el fragmento **`alt`**, si tiene cadena completa recupera los 4 niveles (`POA ➔ Spec ➔ Tarea #ID ➔ Rama/MR/Commits + GLPI + SonarQube + 5 etapas + retornos`); y si es un registro huérfano (`[Flujo 2a]`), despliega el expediente parcial señalando exactamente qué eslabón falta (`sin tarea en Redmine` o `sin espejo GLPI`).
> 4. En el fragmento **`opt [3a]`**, invoca `TareaIssue.validarRespaldoEnCodigo()` y `Desarrollador.auditarDivergenciaAutoria()` para exhibir **lado a lado** la diferencia entre lo digitado en Redmine y la evidencia real en GitLab.
> 5. Por último, verifica la reproducibilidad matemática al `100 %` con `IndicadorDesempeno.calcularEficiencia7030()` y `sellarRastroInmutable()` (`RNF-001`). Si la huella criptográfica estuviera corrupta, el **`break [4a]`** aborta la entrega para jamás mostrar datos adulterados; si está íntegra, jerarquiza los bloqueos por criticidad para que la jefatura tome decisiones en **`< 10 segundos`** (`RNF-007`).
>
> De esta forma comprobamos `0` violaciones ECB y `51 de 51` invocaciones sincronizadas 1:1 con la Vista Lógica. Cedo nuevamente la palabra a **Carlos Valladares** para las Vistas de Desarrollo y Despliegue.»

---

### LÁMINA 13 — Diagrama 13: Vista de Desarrollo — Diagrama de Componentes e Interfaces UML 2.5.1
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto y 15 segundos)*
- **Imagen proyectada:** [`docs/uml/diagrama-componentes-desarrollo.png`](../uml/diagrama-componentes-desarrollo.png)

#### Cómo leer y señalar el diagrama con el mouse
- El diagrama tiene **3 grandes bloques de izquierda a derecha / arriba a abajo**:
  1. **Izquierda (`Frontera Web`):** `WebAppTableroUnico` (`React 19 / Next.js 15`).
  2. **Centro (`Capa 1 — Componentes de Dominio`):** Los 4 componentes del negocio (`ModuloPlanificacionPOA`, `ModuloEjecucionTecnica`, `ModuloTrazabilidadYMetricas`, `ModuloAlertasYGobiernoPMO`).
  3. **Abajo y Derecha (`Capa 2 — Componentes Utilitarios y Modelo de Datos`):** `AdaptadoresIngestaETL`, `ComponenteSeguridadYLOPDP`, `BusEventosYPushIncremental`, `AdaptadoresNotificacionTLS`, `MotorExportacionEjecutiva`, `CapaPersistenciaYAuditoria` y `ModeloDatosConsolidado`.
- **Señala los símbolos de interfaz en medio (`──( )──`):** El círculo (`lollipop`) sale del Utilitario que **provee** el contrato, y el semicírculo (`socket`) sale del Dominio que **consume** el contrato (`IExtractorSoloLectura`, `IServicioPrivacidad`, `IBusEventosDominio`, `IPasarelaNotificaciones`, `IRepositorioConsolidado`, `IRegistroAuditoria`, `IGeneradorReportes`, `IServicioAutenticacion`).

#### Guión Hablado Completo
> «Gracias, Luis. Pasamos a la **Vista de Desarrollo**, pensada para los programadores y la gestión de configuración del código fuente. El séptimo diagrama de la ruta oficial es nuestro **Diagrama de Componentes e Interfaces UML 2.5.1**, compuesto por **12 componentes lógicos**.
>
> **¿Qué problema arquitectónico resolvemos aquí?** El requisito no funcional **`RNF-016` (Mantenibilidad y Modularidad)** exige que si mañana la cooperativa actualiza la versión de Redmine o GitLab, agrega una quinta herramienta o cambia su proveedor de mensajería o correo corporativo, se deban modificar **`0` líneas de código en el núcleo del negocio**.
>
> Para lograrlo aplicamos **Inversión de Dependencias mediante Puertos e Interfaces UML (`lollipop` provista y `socket` requerida)**:
> - Observemos en el centro a los **4 Componentes de Dominio** (`ModuloPlanificacionPOA`, `ModuloEjecucionTecnica`, `ModuloTrazabilidadYMetricas` y `ModuloAlertasYGobiernoPMO`). Ninguno de ellos importa librerías de base de datos ni clientes HTTP.
> - Cuando `ModuloTrazabilidadYMetricas` necesita extraer datos en `CU-01`, se enchufa mediante un *socket* a la interfaz **`IExtractorSoloLectura`**, la cual es provista (*lollipop*) por el componente utilitario **`AdaptadoresIngestaETL`** (que encapsula los 4 adaptadores de solo lectura, el timeout de `10 s`, los `3 reintentos` y el almacén de *snapshot* parcial, documentado en nuestro **ADR-002**).
> - Cuando necesita pasar la compuerta LOPDP 2021, consume la interfaz **`IServicioPrivacidad`** provista por **`ComponenteSeguridadYLOPDP`** (**ADR-004**).
> - Cuando necesita guardar o auditar sin duplicados, consume **`IRepositorioConsolidado`** e **`IRegistroAuditoria`** provistos por **`CapaPersistenciaYAuditoria`**, que es el único componente que toca al **`ModeloDatosConsolidado`** (`PostgreSQL 16` / `Prisma 6`).
> - Y cuando `ModuloAlertasYGobiernoPMO` despacha alertas en `CU-02` o exporta reportes en `CU-08`, consume **`IPasarelaNotificaciones`** (provista por **`AdaptadoresNotificacionTLS`** con conmutación automática a correo) e **`IGeneradorReportes`** (provista por **`MotorExportacionEjecutiva`**).»

---

### LÁMINA 14 — Diagrama 14: Vista de Desarrollo — Diagrama de Paquetes en 3 Capas Acíclicas (DAG)
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto y 15 segundos)*
- **Imagen proyectada:** [`docs/uml/diagrama-paquetes-desarrollo.png`](../uml/diagrama-paquetes-desarrollo.png)

#### Cómo leer y señalar el diagrama con el mouse
- Muestra **3 estratos horizontales apilados de arriba hacia abajo**:
  1. **Arriba (`CAPA 1 — DOMINIOS «Domain»`):** `AlertasYGobiernoPMO` (Nivel 4) ➔ `TrazabilidadYMetricas` (Nivel 3) ➔ `EjecucionTecnica` (Nivel 2) ➔ `PlanificacionPOA` (Nivel 1, hoja base).
  2. **Al centro (`CAPA 2 — UTILITARIOS «Utility»`):** Los 7 paquetes transversales (`Seguridad`, `Persistencia`, `Auditoria`, `IntegracionExterna`, `Notificaciones`, `Validaciones`, `Reportes`).
  3. **Abajo (`CAPA 3 — PLATAFORMAS «Platform»`):** `ServiciosExternosTI`, `CanalesEIdentidadCOAC`, `InfraestructuraDatos` (`PostgreSQL 16`, `Valkey 8.0`, `Nginx 1.26`) y `FrameworksYRuntime` (`NestJS 11`, `Prisma 6`, `BullMQ 5`, `React 19 / Next.js 15`, `Node.js 22 LTS`, `Ubuntu 24.04 / Docker 27`).
- **Todas las flechas punteadas van hacia abajo o de izquierda a derecha en la Capa 1 (jamás hacia arriba).**

#### Guión Hablado Completo
> «El octavo diagrama es nuestro **Diagrama de Paquetes UML 2.5.1**, donde organizamos la estructura de carpetas del repositorio Git en correspondencia `1:1` bajo un **Monolito Modular en 3 Capas** (**ADR-001**), gobernado por tres leyes arquitectónicas:
>
> 1. **Primera ley (Partición sin "cajones de sastre"):** Prohibimos crear carpetas ambiguas como `Util`, `Common` o `General`. En la **Capa 1 (`«Domain»`)** viven los 4 paquetes del negocio que agrupan las 16 entidades y los 5 controladores ECB. En la **Capa 2 (`«Utility»`)** viven únicamente **7 utilitarios transversales** que aprobaron la **Prueba de las 3 Preguntas** (*¿lo usan 2 o más dominios?, ¿es copiable a otra empresa como una clínica sin tocar su código?, ¿tiene 0 reglas de negocio de la COAC?*). Candidatos como el calculador `70/30` o el conversor `Fibonacci` reprobaron esa prueba y fueron ubicados correctamente dentro de sus paquetes de Dominio.
> 2. **Segunda ley (Principio Gravitacional y Principio de Aislamiento):** Por gravedad, todas las dependencias caen estrictamente hacia abajo: **`«Domain»` ➔ `«Utility»` ➔ `«Platform»`**. Ningún `«Utility»` importa jamás a un `«Domain»`, y ningún `«Domain»` salta directamente a `«Platform»`.
> 3. **Tercera ley (Grafo Acíclico Dirigido — `0` Ciclos y ADR-003):**
>    Dentro de la Capa 1 definimos un orden topológico estricto: `AlertasYGobiernoPMO` (Nivel 4) depende de `TrazabilidadYMetricas` (Nivel 3), este de `EjecucionTecnica` (Nivel 2), y este de `PlanificacionPOA` (Nivel 1, que es la hoja base con `0` dependencias laterales).
>    **Aquí resolvimos un reto clásico de arquitectura:** cuando termina `CU-01` en `TrazabilidadYMetricas` (Nivel 3), debe disparar las alertas de `CU-02` y el refresco del tablero `CU-04`, que viven en `AlertasYGobiernoPMO` (Nivel 4). Si `TrazabilidadYMetricas` importara a `AlertasYGobiernoPMO`, habríamos creado un **ciclo prohibido `Nivel 3 ↔ Nivel 4`**. ¿Cómo lo rompimos (**ADR-003**)? `TrazabilidadYMetricas` publica el evento `CicloConsolidadoEvento` hacia abajo, al paquete utilitario **`Notificaciones`**, y `AlertasYGobiernoPMO` se suscribe a dicho evento.
>
> De esta manera auditamos **36 aristas con `0` ciclos**, asignamos carpetas disjuntas en Git para que **Alex**, **Luis** y **yo** programemos en paralelo sin conflictos de *merge*, y dejamos reglas automáticas en `.dependency-cruiser.cjs` que hacen fallar el pipeline CI/CD si alguien intenta introducir un ciclo o violar una capa.»

---

### LÁMINA 15 — Diagrama 15: Vista Física — Diagrama de Despliegue en Producción UML 2.5.1
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 1 minuto y 30 segundos)*
- **Imagen proyectada:** [`docs/uml/diagrama-despliegue-produccion.png`](../uml/diagrama-despliegue-produccion.png)

#### Cómo leer y señalar el diagrama con el mouse
- Muestra **3 Zonas de Red claramente delimitadas** y cada nodo respeta el anidamiento triple de UML 2.5.1 (`Caja 3D «device»` ➔ adentro `Caja «executionEnvironment»` ➔ adentro `Icono de archivo «artifact»` y `«deployment spec»`), con **protocolo y puerto etiquetado en todas las líneas**:
  1. **Arriba a la izquierda (Azul — `ZONA 1: INTERNET PÚBLICO / ACCESO DIRECTIVO SIN VPN`):** `Estación / Dispositivo Directivo (1..25 sesiones)` ➔ `Navegador Web Estándar` ➔ `«artifact» trazabilidad-web`. Sale por `HTTPS TLS 1.3 (:443) [Acceso Público sin VPN · RNF-013]`.
  2. **Centro/Izquierda (Verde — `ZONA 2: INFRAESTRUCTURA COAC` dividida en dos sub-bloques):**
     - **Sub-bloque superior (`Borde DMZ Público — Único Punto Expuesto a Internet · WAF + HTTPS :443`):** `Servidor Web / Proxy Inverso (Par HA Activo-Pasivo · VIP Pública)` con `Nginx 1.26 LTS + WAF` y `«deployment spec» config-proxy-tls13`.
     - **Sub-bloque inferior (`Subred Interna Privada de Servidores y BD — 0 Exposición Directa a Internet · LOPDP 2021`):**
       - `Servidor de Aplicaciones Web (2 instancias activas · Stateless)` con `Docker Engine 27 / Node.js 22 LTS` alojando `«artifact» trazabilidad-api` + `config-produccion`.
       - `Servidor Worker ETL y Alertas (Instancia Activa + Standby)` alojando `«artifact» trazabilidad-etl-worker` + `config-produccion`.
       - `Servidor de Colas y Caché` con `Valkey 8.0` alojando `«artifact» colas-bullmq-snapshot`.
       - `Servidor de Base de Datos Primario` con `RDBMS PostgreSQL 16 (Cifrado en Reposo LOPDP)` alojando `«artifact» esquema-trazabilidad`, conectado por `Streaming Replication TCP/TLS (:5432)` hacia el **`Servidor de Base de Datos Réplica (Hot Standby · Failover + PITR)`**.
  3. **Abajo a la derecha (Naranja suave — `ZONA 3: SERVICIOS INSTITUCIONALES Y PLATAFORMAS EXTERNAS`):** `Directorio de Identidad COAC (LDAP/OIDC :636/:443)`, `Plataformas Origen TI (Redmine, GLPI, GitLab, SonarQube — HTTPS Solo Lectura :443)`, `Canal de Mensajería Corporativo (HTTPS TLS >= 1.2 :443)` y `Servicio de Correo Corporativo (SMTP/TLS >= 1.2 :587)`.

#### Guión Hablado Completo
> «Cerramos el recorrido de los 9 diagramas con la **Vista Física (Diagrama de Despliegue en Producción bajo UML 2.5.1)**, donde mapeamos los artefactos de software hacia los servidores, entornos de ejecución, zonas de seguridad y protocolos de red.
>
> Primero, destacamos el cumplimiento estricto de la notación **UML 2.5.1**: ningún componente lógico se dibuja suelto en un servidor; cada nodo físico o virtual está estereotipado como **`«device»`**, contiene anidado su motor de ejecución **`«executionEnvironment»`** (`Docker Engine 27 / Node.js 22 LTS`, `Nginx 1.26 LTS`, `Valkey 8.0` o `RDBMS PostgreSQL 16`) y dentro de él aloja mediante despliegue a los artefactos físicos **`«artifact»`** junto con su especificación **`«deployment spec»`** (`config-produccion`), etiquetando protocolo y puerto exacto en cada enlace de comunicación.
>
> **Segundo, ¿cómo resolvimos la exigencia operativa de la PMO de revisar el tablero desde su casa o teléfono móvil sin usar VPN, pero manteniendo al mismo tiempo la base de datos blindada bajo la ley LOPDP 2021?**
> Diseñamos una topología segmentada en **3 Zonas de Seguridad** idéntica al estándar de banca electrónica:
> - **En la Zona 1 (Internet Público / Acceso Directivo sin VPN):** el `Jefe de Desarrollo / PMO` (`1..25 sesiones simultáneas`) abre su navegador desde casa, oficina o celular (`1920x1080`, `768x1024` o `360x640 px`, `RNF-012`), descarga el bundle `trazabilidad-web` y se conecta directamente por **`HTTPS TLS 1.3` en el puerto `:443`** (`RNF-013`) sin requerir cliente VPN.
> - **En la Zona 2 — Borde DMZ Público:** el **único** nodo que tiene IP pública y escucha a Internet es el **`Servidor Web / Proxy Inverso`**, configurado en **Par de Alta Disponibilidad Activo-Pasivo (VIP Pública)** con `Nginx 1.26 LTS + WAF` (cortafuegos de aplicaciones web y *rate limiting*). Este nodo termina el cifrado `TLS 1.3` y reenvía únicamente peticiones válidas por `HTTP/2 interno (:3000)` hacia adentro.
> - **En la Zona 2 — Subred Interna Privada de Servidores y Base de Datos (`0` exposición directa a Internet):**
>   1. **Separación física entre Consulta y ETL:** En total coherencia con la Vista de Procesos, separamos el **`Servidor de Aplicaciones Web`** (que corre **2 instancias activas *stateless*** de la imagen OCI `trazabilidad-api` para garantizar carga `<= 2 s` y filtros `<= 400 ms`) respecto del **`Servidor Worker ETL y Alertas`** (Activo + Standby, que corre la imagen `trazabilidad-etl-worker`). Así, cuando cada 5 minutos el Worker consume CPU extrayendo y limpiando PII de las 4 fuentes, el tablero web de la PMO no sufre ni un milisegundo de degradación.
>   2. **Colas y Caché en Memoria:** Ambos servidores se apoyan en `Valkey 8.0` (`RESP/TLS :6379`) con persistencia AOF, donde viven las colas `BullMQ` de `CU-02`, el caché de lectura, el canal de *push* incremental `< 1 s` y el almacén de *snapshots* parciales (`RNF-008`).
>   3. **Eliminación de Puntos Únicos de Falla (SPOF) en Base de Datos:** Si tuviéramos un solo servidor de base de datos, su caída apagaría todo el sistema. Lo mitigamos desplegando un **`Servidor de Base de Datos Primario` (`PostgreSQL 16` con cifrado en reposo `AES-256` LOPDP)** conectado mediante **`Streaming Replication TCP/TLS (:5432)`** en tiempo real hacia un **`Servidor de Base de Datos Réplica (`Hot Standby`)`** con *failover* automático y respaldos diarios *Point-in-Time Recovery (`PITR`)* para cubrir la retención legal de `12 a 24 meses`.
> - **Finalmente, en la Zona 3 (Servicios Institucionales y Plataformas Externas):** el Servidor App Web valida credenciales contra el **`Directorio de Identidad COAC` (`LDAPS / OIDC :636/:443`)** en `CU-03`, mientras que únicamente el Worker ETL sale por **`HTTPS Solo Lectura (:443)`** hacia `Redmine`, `GLPI`, `GitLab` y `SonarQube`, y despacha alertas cifradas hacia el **`Canal de Mensajería Corporativo` (`HTTPS TLS >= 1.2 :443`)** con conmutación automática hacia el **`Servicio de Correo Corporativo` (`SMTP/TLS >= 1.2 :587`)**.»

---

### LÁMINA 16 — Manifestación de Artefactos (`«manifest»`), Desarrollo Local y Cierre de Defensa
- **Expositor:** **Carlos Valladares** *(Tiempo aprox.: 45 segundos)*
- **Imágenes proyectadas:** [`docs/uml/diagrama-artefactos-manifest.png`](../uml/diagrama-artefactos-manifest.png) y [`docs/uml/diagrama-despliegue-desarrollo.png`](../uml/diagrama-despliegue-desarrollo.png)

#### Cómo leer y señalar los diagramas con el mouse
- **Imagen central (`diagrama-artefactos-manifest.png`):** Muestra a la izquierda los **5 artefactos físicos (`«artifact»`) + `config-produccion` (`«deployment spec»`)** y a la derecha los **12 componentes lógicos (`«component»`)** de la Vista de Desarrollo, unidos por flechas punteadas **`«manifest»`**.
- **Imagen derecha (`diagrama-despliegue-desarrollo.png`):** Muestra cómo en la laptop de cualquiera de los 3 desarrolladores (**Carlos, Alex o Luis**) corren exactamente los mismos 5 artefactos dentro de contenedores locales de **Docker Compose**, cambiando únicamente `config-produccion` por **`config-dev`** y apuntando a un Sandbox/Mocks de pruebas.

#### Guión Hablado Completo
> «Para cerrar nuestra defensa en la Lámina 16, mostramos los dos diagramas complementarios que cierran la trazabilidad de la Vista Física:
> 1. **En el diagrama central (`Manifestación de Artefactos — «manifest»`):** demostramos el puente formal de UML 2.5.1 entre la Vista de Desarrollo y la Vista Física: cómo nuestros **12 componentes lógicos** se empaquetan mediante relaciones **`«manifest»`** dentro de **5 artefactos físicos concretos** (`trazabilidad-web`, `trazabilidad-api`, `trazabilidad-etl-worker`, `colas-bullmq-snapshot` y `esquema-trazabilidad`).
> 2. **En el diagrama de la derecha (`Despliegue Simplificado en Desarrollo Local`):** demostramos la **portabilidad total de la arquitectura**: cada uno de nosotros tres en su estación de trabajo levanta con `Docker Compose` exactamente las mismas imágenes de contenedor que van a producción, sustituyendo únicamente la especificación **`config-produccion` por `config-dev`** sin tocar ni recompilar una sola línea de código fuente.
> 3. **Recomendación de Infraestructura (On-Premise DMZ vs. Nube):** Tras comparar el despliegue en nube pública (`Kubernetes + OpEx mensual + túneles VPN Site-to-Site`) frente al despliegue **On-Premise en contenedores Docker + OpenTofu (IaC) dentro del Data Center de la COAC con salida pública por la DMZ sin VPN**, recomendamos el modelo **On-Premise Contenerizado**, porque aprovecha la infraestructura virtualizada ya amortizada por la cooperativa (`CapEx`), mantiene latencia local `< 1 ms` hacia Redmine y GitLab, garantiza soberanía física absoluta bajo la **LOPDP Ecuador 2021**, y gracias a los artefactos OCI e Infraestructura como Código con **OpenTofu**, queda `100 %` preparado para migrar a Kubernetes en el futuro si la cooperativa lo decidiera.
>
> Con esto hemos demostrado una arquitectura completa, coherente y verificada de extremo a extremo. Muchas gracias, ingeniero; quedamos a su disposición para la ronda de preguntas.»

---

## PARTE 3 — Banco Ampliado de 12 Preguntas de Defensa del Docente (Q&A)

| # | Pregunta Probable del Ingeniero | Respuesta Exacta para Defender el Proyecto | Expositor Recomendado |
|:---:|---|---|---|
| **1** | *«¿Cómo conectan la Cadena de Valor, el Flujo de Valor y el Mapa de Capacidades con los Casos de Uso?»* | «Se conectan de forma directa: en la **Cadena de Valor** identificamos los 6 eslabones primarios del negocio; en el **Flujo de Valor** detallamos las entradas y salidas de esos 6 eslabones; en el **Mapa de Capacidades** definimos las capacidades de Nivel 1 (Gobierno PMO), Nivel 2 (Core de Trazabilidad) y Nivel 3 (Soporte LOPDP/Plataforma) que habilitan cada etapa; y en la **Vista de Escenarios (`+1`)** cada capacidad operativa se materializa en los **8 Casos de Uso (`CU-01` a `CU-08`)** y los **58 requisitos (`40 RF` y `18 RNF`)**.» | **Carlos Valladares** |
| **2** | *«¿Por qué pusieron a `Tiempo (Temporizador CRON)` como actor primario de `CU-01` y `CU-02` y no al administrador o al desarrollador?»* | «Porque bajo `RF-029` la sincronización cada 5 minutos y la detección de alertas son **100 % autónomas y desatendidas**: ningún humano presiona un botón cada 5 minutos para extraer datos. En UML y la metodología de Alistair Cockburn, cuando un caso de uso se dispara por un evento temporal periódico, el actor primario es el **Tiempo (`Temporizador CRON`)**, mientras que el `Jefe de Desarrollo / PMO` es el receptor de valor.» | **Carlos Valladares** |
| **3** | *«¿Por qué la relación de `CU-02` hacia `CU-01` y la de `CU-05` hacia `CU-04` son `«extend»` y no `«include»`?»* | «En UML 2.5.1, `«include»` es obligatorio (se ejecuta el 100 % de las veces), mientras que **`«extend»` es condicional**. En `CU-01`, si en un ciclo de 5 minutos todos los desarrolladores están bajo 40 h y no hay atrasos, **no se emite ninguna alerta**; `CU-02` solo extiende a `CU-01` cuando se cumple la condición `[umbral crítico superado]`. Igualmente, en `CU-04` el directivo puede solo mirar el tablero general; `CU-05` solo lo extiende cuando el usuario decide activar `[análisis focalizado]`.» | **Carlos / Luis** |
| **4** | *«En el Diagrama de Clases, ¿por qué no existe una clase llamada `TableroControl`, `MapaDeCalor` o `Redmine`?»* | «Porque en la **Vista Lógica del Modelo 4+1** solo se modelan **conceptos persistentes del dominio del negocio (`«entity»`)**. Al aplicar la Tarjeta de Filtro y los 4 rombos de entidad, *Tablero* y *Mapa de calor* se descartan por ser **vistas de interfaz de usuario (UI)** (en la Vista de Procesos pasan a ser `«boundary»` como `PanelTableroControl`), y *Redmine* o *GitLab* se descartan como entidades porque son **actores/sistemas externos** operados en solo lectura.» | **Alex Gutiérrez** |
| **5** | *«Explíquenme la diferencia entre rombo blanco (`◇`) y rombo negro (`◆`) en su Diagrama de Clases con un ejemplo de su proyecto.»* | «La diferencia es el **ciclo de vida**:<br>• Usamos **Agregación (`◇`, rombo blanco)** en `ModuloFinanciero ◇── ProyectoPOA`: si se reorganiza un catálogo de módulo, el proyecto POA tiene identidad propia en Redmine.<br>• Usamos **Composición Fuerte (`◆`, rombo negro)** en `ExpedienteTrazabilidad ◆── HitoEtapa` y `AlertaOperativa ◆── DespachoNotificacion`: un hito de etapa o una bitácora de despacho **nacen y mueren** exclusivamente dentro de su expediente o alerta dueña; jamás pueden existir sueltos.» | **Alex Gutiérrez** |
| **6** | *«¿Por qué pusieron multiplicidad `0..1` entre `TareaIssue` y `ActividadCodigo` / `IncidenteSoporte`?»* | «Esa multiplicidad `0..1` implementa una regla crítica del negocio (`RF-021` y `RF-022`): si hubiéramos puesto `1` obligatorio, cualquier commit subido a GitLab sin `#ID` de Redmine o cualquier ticket GLPI sin tarea espejo habría sido rechazado por la base de datos y **se habrían perdido esas horas de trabajo**. Al poner `0..1`, cuando no existe `TareaIssue` (`0`), el sistema lo acepta, lo etiqueta como *Trabajo sin trazabilidad* y suma su esfuerzo a la carga de 40 h.» | **Alex Gutiérrez** |
| **7** | *«¿Cómo se calcula exactamente la carga de 40 horas y la eficiencia 70/30? ¿Los incidentes de GLPI entran en ambas fórmulas?»* | «Ahí hay un invariante muy importante (**`RN-07`**):<br>• **En la Carga Semanal (`40 h`, `RF-002`/`RF-003`):** **SÍ entran** los incidentes activos de GLPI (`+4 h / 3 pts` si es estándar, `+8 h / 5 pts` si es crítico del core sin estimación manual), sumados a las tareas activas de Redmine y a las revisiones de QA cruzado (`+2 h / 1 pt`).<br>• **En la Eficiencia de Proyectos (`70 % avance + 30 % calidad`, `RF-005`):** las horas de incidentes emergentes de GLPI **SE EXCLUYEN**, para no castigar la eficiencia de los proyectos planificados cuando un programador tuvo que apagar un incendio de soporte. El `30 %` de calidad arranca en 100 y descuenta `-10 pts` por cada retorno desde Certificación y `-5 pts` por cada bug crítico o vulnerabilidad en SonarQube.» | **Alex / Carlos** |
| **8** | *«En los Diagramas de Secuencia, ¿qué significa que haya `0` violaciones ECB y sincronización `1:1`?»* | «Significa dos cosas verificadas línea por línea:<br>1. **Cero violaciones ECB:** Ningún actor habla directo con un controlador o entidad (siempre pasa por un `«boundary»`), y ningún `«boundary»` invoca métodos de una `«entity»` saltándose al `«control»`.<br>2. **Sincronización 1:1 (`51/51` llamadas):** Cada flecha que un `«control»` envía a una `«entity»` en los 6 diagramas de secuencia (por ejemplo, `+ verificarCompuertaLOPDP() : Booleano` o `+ calcularEficiencia7030() : Decimal`) está declarada exactamente con el mismo nombre y firma en la clase correspondiente del Diagrama de Clases y en el Diseño Detallado.» | **Luis Carrión** |
| **9** | *«¿Qué pasa si durante `CU-01` se cae GitLab, o qué pasa si durante `CU-02` se cae el Canal de Mensajería Corporativo?»* | «Ambos casos están cubiertos con resiliencia:<br>• **En `CU-01` (Flujo Alterno `2a`, `opt` en secuencia):** `AdaptadorGitLab` espera hasta `10 s` y hace `3 reintentos`. Si sigue caído, `ControladorSincronizacion` reutiliza el **último snapshot válido** de GitLab y marca `SincronizacionParcialSnapshot`, prohibiendo reclasificar las tareas como si no tuvieran código.<br>• **En `CU-02` (Flujo Alterno `4a`, `alt` en secuencia):** Si falla `AdaptadorCanalMensajeria`, el controlador invoca `DespachoNotificacion.escalarACorreoPorFallo()` y **conmuta automáticamente al `Servicio de Correo Corporativo`**, guardando siempre el `100 %` en bitácora (`RNF-004`).» | **Luis Carrión** |
| **10** | *«¿Cómo evitaron que el Diagrama de Paquetes tuviera dependencias circulares entre `TrazabilidadYMetricas` y `AlertasYGobiernoPMO`?»* | «Aplicando el **ADR-003 (Desacoplamiento por Eventos)**: en el orden DAG, `AlertasYGobiernoPMO` (Nivel 4) depende de `TrazabilidadYMetricas` (Nivel 3) para leer los expedientes en el tablero. Por tanto, `TrazabilidadYMetricas` tenía prohibido importar a `AlertasYGobiernoPMO` al terminar `CU-01`. En vez de llamarlo directo, `TrazabilidadYMetricas` publica un evento asíncrono (`CicloConsolidadoEvento`) hacia el paquete utilitario inferior **`Notificaciones`** (`BullMQ / Valkey`), y `AlertasYGobiernoPMO` escucha ese evento. Así todas las flechas apuntan hacia abajo y existen `0` ciclos.» | **Carlos Valladares** |
| **11** | *«¿Por qué en el Diagrama de Despliegue hay dos contenedores separados (`trazabilidad-api` y `trazabilidad-etl-worker`) si dijeron que la arquitectura de código es un Monolito Modular?»* | «Porque **Monolito Modular** describe cómo se organiza y compila el código en un solo repositorio coherente (**Vista de Desarrollo**), pero en la **Vista Física (Despliegue)** separamos su ejecución en **dos procesos contenedores OCI independientes**: `trazabilidad-api` (2 instancias *stateless* dedicadas exclusivamente a responder consultas web de la PMO en `<= 2 s`) y `trazabilidad-etl-worker` (dedicado al trabajo pesado del CRON cada 5 minutos y envío de alertas). Así obtenemos lo mejor de ambos mundos: simplicidad de código para un equipo de 3 personas y aislamiento físico de CPU y memoria en producción.» | **Carlos Valladares** |
| **12** | *«¿Cómo garantizan que la PMO entre desde su casa sin VPN sin poner en peligro la base de datos de la cooperativa?»* | «Mediante la separación en **Borde DMZ Público + Subred Interna Privada** en la Zona 2 del Diagrama de Despliegue: a Internet solo se publica el puerto `:443` del **Proxy Inverso `Nginx 1.26 LTS + WAF`** con `HTTPS TLS 1.3` obligatorio y autenticación institucional `CU-03` (`LDAP/OIDC`). El servidor `PostgreSQL 16` (`:5432`), `Valkey 8.0` (`:6379`) y el `Worker ETL` viven en la subred interna privada sin IP pública ni ruta desde Internet, con cifrado en reposo `AES-256` y datos `100 %` libres de PII de socios.» | **Carlos Valladares** |
