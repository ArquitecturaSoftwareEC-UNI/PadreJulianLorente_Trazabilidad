# Reglas Metodológicas Oficiales: Vista Lógica (Semana 4), Vista de Procesos (Semana 5) y DAS v3.0

> **Fuente Oficial:** Destilado normativo de `docs/referencias/Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf` (Págs. 21–30 y Apéndice A Pág. 36) — Asignatura Arquitectura de Software (`DSOF_4069`), estándares **UML 2.5.1** e **ISO/IEC/IEEE 42010** (Modelo 4+1 de Kruchten).

---

## 1. Semana 4 — Vista Lógica (Págs. 21–25)

### 1.1 Propósito Arquitectónico y Niveles de Abstracción
La Vista Lógica define **CON QUÉ CONCEPTOS** el sistema satisface los requisitos funcionales y soporta los requisitos no funcionales de mantenibilidad, reutilización y extensibilidad. Se construye progresivamente en dos niveles:

1. **Nivel Conceptual (Modelo del Dominio):**
   - Centrado exclusivamente en los conceptos del problema, sus nombres canónicos en español de negocio, atributos esenciales sin tipos técnicos y relaciones estructurales básicas.
2. **Nivel de Especificación (Compromisos de Diseño Lógico):**
   - Evoluciona las clases del dominio incorporando:
     - **Visibilidad UML:** `+` (público), `-` (privado), `#` (protegido), `~` (paquete).
     - **Atributos tipados abstractamente:** `visibilidad nombre : tipo [multiplicidad] = valorPorDefecto` (ej. `- umbralHoras : Decimal = 40.0`).
     - **Operaciones con firma completa:** `visibilidad nombre(parámetro : tipo) : tipoRetorno`.
     - **Multiplicidades exactas** en ambos extremos de cada relación.

### 1.2 Las 5 Exclusiones Estrictas (Qué NO es la Vista Lógica)
Violar cualquiera de estas exclusiones invalida el artefacto arquitectónico:

1. **NO es una base de datos física:** Prohibido modelar tablas SQL, tipos propios de motor (`VARCHAR`, `INT8`, `SERIAL`, `JSONB`, `TIMESTAMPTZ`) o simular claves foráneas (`FK`, ej. `idDesarrollador`, `idProyecto`) como atributos dentro de las clases. Las asociaciones entre objetos se representan **únicamente** mediante líneas de relación UML y multiplicidades.
2. **NO contiene infraestructura de despliegue:** Prohibido incluir servidores, nodos físicos, contenedores Docker, balanceadores o puertos.
3. **NO organiza archivos de código ni concurrencia:** No modela carpetas/capas de frameworks (eso pertenece a la Vista de Desarrollo, Semana 6) ni hilos, colas o procesos paralelos (eso pertenece a la Vista de Procesos, Semana 5).
4. **NO incluye interfaz gráfica (UI):** Prohibido modelar pantallas, formularios, modales, botones, filtros visuales ni reportes CSV/PDF como clases del dominio.
5. **Prohibición de atributos derivables sin marcar:** Evitar almacenar datos calculables como atributos fijos; si un atributo derivado es indispensable a nivel lógico, debe prefijarse obligatoriamente con barra inclinada `/` (ej. `- /porcentajeAvance : Decimal`, `- /indiceSobrecarga : Entero`).

### 1.3 Extracción y Decodificación Gramatical (desde la Vista +1)
El insumo obligatorio son las especificaciones de Casos de Uso de Semana 3 (`docs/especificaciones-casos-de-uso.md`):

| Elemento gramatical en la especificación | Rol candidato en la Vista Lógica |
|---|---|
| **Sustantivos y frases nominales** | Candidatos a **Clases** o **Atributos** |
| **Verbos de acción** | Candidatos a **Operaciones (métodos)** o nombres de **Relaciones** |
| **Adjetivos y valores de estado** | Candidatos a **Atributos**, **Enumeraciones (`«enumeration»`)** o **Estados** |
| **Expresiones de cantidad** ("varios", "un solo", "al menos uno", "cero o muchos") | **Multiplicidades** en los extremos de relación |

### 1.4 Descartes Inmediatos y Árbol de 4 Filtros Secuenciales

#### Descartes inmediatos
Antes de aplicar los filtros, descartar de la lista de candidatos:
- **Sinónimos:** Conservar únicamente el término canónico del glosario del proyecto.
- **Actores externos cuya historia interna no gestiona el sistema:** Ej. *Redmine*, *GitLab*, *GLPI*, *SonarQube*, *Discord/Slack*, *Servidor SMTP* o *Temporizador ETL* no se modelan como entidades de negocio salvo que el sistema almacene configuración/registro lógico propio de una *FuenteExterna* o *CredencialIntegracion*.
- **Elementos de UI y reportes:** Pantallas, vistas, gráficas, botones, archivos CSV/PDF exportados.
- **Operaciones disfrazadas de sustantivos:** Acciones como "búsqueda", "filtrado" o "cálculo" son operaciones de alguna clase, no clases por sí mismas (a diferencia de un evento auditable con identidad y estado propios, como un `CicloSincronizacion` o una `AlertaOperativa`).

#### Árbol de 4 Filtros Secuenciales
Todo candidato sobreviviente debe superar **los 4 filtros** para convertirse en clase del Diagrama de Clases:

1. **Filtro 1 — Memoria / Persistencia Conceptual:** ¿El sistema necesita recordar información de este concepto entre ejecuciones o a lo largo del tiempo? *(Los parámetros transitorios de entrada, como "rango de fechas" o "criterio de búsqueda", se descartan como clases).*
2. **Filtro 2 — Complejidad Estructural:** ¿Posee más de un atributo relevante propio? *(Si es un valor atómico único, como `correo` o `umbralDias`, pasa a ser atributo de una clase existente).*
3. **Filtro 3 — Identidad Propia y Ciclo de Vida Independiente:** ¿Corresponde a Personas/Roles del dominio, Eventos transaccionales/operativos registrables, Elementos tangibles/artefactos trazables o Catálogos del negocio?
4. **Filtro 4 — Participación en Escenarios:** ¿Participa activamente en al menos un Caso de Uso de la Vista +1 (`CU-01` a `CU-08`)? *(Prohibido mantener clases huérfanas o especulativas sin respaldo en requisitos).*

### 1.5 Validación con Tarjetas CRC y Matriz Escenarios vs. Clases
Antes de congelar el Diagrama de Clases definitivo:
- **Tarjetas CRC (Clase – Responsabilidad – Colaboración):** Cada clase se somete a una tarjeta de auditoría para verificar **Alta Cohesión** y **Bajo Acoplamiento**:
  - Demasiadas responsabilidades en una sola clase ➔ señal de **Baja Cohesión** (anti-patrón *Clase Dios / God Class*; debe dividirse).
  - Demasiadas clases colaboradoras ➔ señal de **Alto Acoplamiento** (revisar encapsulamiento o mediación).
- **Matriz de Trazabilidad (Escenarios vs. Clases):** Cruza en filas los Casos de Uso (`CU-01` a `CU-08`) y en columnas todas las clases del dominio:
  - Ninguna fila ni columna puede quedar vacía.
  - Los **Flujos Alternos** de las especificaciones de Semana 3 revelan las transiciones críticas que exigen modelar un **Diagrama de Estados UML** complementario para las clases con ciclo de vida complejo (por ejemplo, `AlertaOperativa`, `CicloSincronizacion` o `ItemTrabajo`).

### 1.6 Las 6 Relaciones Formales UML 2.5.1, Multiplicidades y Roles
Todo enlace en el Diagrama de Clases debe usar la relación semánticamente correcta:

1. **Asociación (`—` / `-->`):** Conexión estructural entre instancias; debe etiquetarse con un verbo activo que exprese sentido de lectura y roles en los extremos.
2. **Agregación (`o--`, rombo blanco en el Todo):** Relación Todo–Parte **débil**; las partes tienen ciclo de vida independiente y sobreviven si el contenedor desaparece.
3. **Composición (`*--`, rombo negro lleno en el Todo):** Relación Todo–Parte **fuerte** con dependencia estricta de ciclo de vida; si el Todo se destruye, las partes mueren con él, y una parte no puede pertenecer a dos Todos a la vez.
4. **Generalización / Herencia (`--|>`, flecha triangular blanca):** Relación taxonómica estricta *"es un tipo de"* (principio de sustitución). Aplica únicamente si la especialización es permanente durante todo el ciclo de vida del objeto.
5. **Dependencia (`..>`, línea punteada con punta abierta):** Uso temporal o paramétrico sin posesión estructural (ej. uso de una `«enumeration»` con estereotipo `«use»`).
6. **Realización (`..|>`, línea punteada con flecha triangular blanca):** Implementación de una interfaz o contrato abstracto (`«interface»`).

**Reglas adicionales de modelado estructural:**
- **Multiplicidades como escudo arquitectónico:** Son obligatorias en **ambos extremos** de toda asociación, agregación y composición (`1`, `0..1`, `*` o `0..*`, `1..*`, `n..m`).
- **Descomposición de N:M con atributos propios:** Toda relación muchos a muchos (`*` a `*`) que tenga atributos, estado o marca temporal propios debe descomponerse en una **Entidad Intermedia / Clase Asociativa**.
- **Patrón de Roles por Asociación vs. Herencia Rígida:** Si un sujeto o entidad del dominio puede cambiar de rol en el tiempo o acumular varios roles simultáneamente, está **prohibido** modelarlo con herencia rígida; debe usarse **Roles por Asociación** y justificarse como decisión arquitectónica en el DAS.

### 1.7 Receta de Construcción en 5 Pasos y Diccionario de Clases Obligatorio

#### Receta de 5 pasos para el Diagrama de Clases
1. Ubicar las clases núcleo más conectadas en el centro del lienzo.
2. Declarar los atributos depurados con visibilidad privada (`-`) y tipo de dato abstracto (`Texto`, `Entero`, `Decimal`, `Booleano`, `FechaHora`, `Duracion`, etc.).
3. Trazar las relaciones formales precisas (Asociación, Agregación, Composición, Generalización, Dependencia) con verbo de lectura y roles.
4. Especificar multiplicidades exactas en ambos extremos y definir todas las enumeraciones (`«enumeration»`) con sus literales en mayúsculas o formato canónico.
5. Incorporar las operaciones públicas (`+`) con firma completa derivadas de los verbos de los Casos de Uso y agrupar en paquetes lógicos si mejora la legibilidad.

#### 5 Campos Obligatorios por Clase en el Diccionario de Clases
1. **Nombre y Propósito:** Definición clara de su única responsabilidad en el dominio.
2. **Atributos y Restricciones:** Visibilidad, tipo abstracto, obligatoriedad, valor por defecto, rangos válidos e indicación `/` si es derivado.
3. **Operaciones (Entrada / Salida):** Firma completa `+ metodo(param : Tipo) : TipoRetorno` y descripción breve de su efecto. *(Estas firmas deben coincidir 1:1 con la Vista de Procesos).*
4. **Invariantes / Reglas de Negocio:** Condiciones que la clase jamás puede violar y códigos `RF-xxx` respaldados (ej. fórmula de índice de carga, umbral `> 5` días hábiles, deduplicación `24 h`).
5. **Casos de Uso Trazados:** Lista exacta de `CU-01` a `CU-08` donde interviene.

---

## 2. Semana 5 — Vista de Procesos (Págs. 26–30)

### 2.1 Propósito y Unidades Ejecutables (Kruchten / Bass et al.)
La Vista de Procesos describe el comportamiento **dinámico en tiempo de ejecución** del sistema y atiende directamente los atributos de calidad de rendimiento, disponibilidad, concurrencia, paralelismo, distribución física, tolerancia a fallos e integridad transaccional.

- **Proceso (Unidad Táctica / Ejecutable):** Espacio de direcciones independiente con memoria, estado y ciclo de vida propios que agrupa una o más tareas y puede replicarse en múltiples instancias.
- **Tarea (Hilo de Control Individual / Thread):** Secuencia de ejecución concurrente dentro de un proceso.
- **Regla de Oro de Distribución:** Las tareas mayores nunca deben asumir que residen en el mismo proceso ni en el mismo nodo físico que sus interlocutoras; toda comunicación inter-proceso debe contemplar latencia, timeouts y fallos parciales.

### 2.2 Semántica de Comunicación en Diagramas de Secuencia UML 2.5.1
- **Mensaje Síncrono (`->` en PlantUML, flecha continua punta rellena):** El emisor se bloquea esperando respuesta del receptor. Genera alto acoplamiento temporal y riesgos de latencia acumulada, agotamiento de hilos y fallos en cascada (exige `timeout` explícito, ej. `<= 5 s` en conectores externos).
- **Mensaje Asíncrono (`->>` en PlantUML, flecha continua punta abierta):** El emisor dispara el evento/mensaje y continúa de inmediato sin bloquearse. Logra bajo acoplamiento temporal y resiliencia ante picos, pero exige controlar consistencia eventual, orden de eventos, deduplicación y colas de reintento.
- **Mensaje de Retorno (`-->`, línea punteada punta abierta):** Devuelve control o resultado de una invocación síncrona.
- **Activación y Destrucción (`activate` / `deactivate` / `destroy`):** Muestran con precisión el foco de control de cada participante y los límites transaccionales.

### 2.3 Patrón de Análisis de Robustez ECB y Procedimiento de 6 Pasos
Todo Diagrama de Secuencia parte obligatoriamente de un **Análisis de Robustez ECB** (*Entity – Control – Boundary*):

- **`«boundary»` (Frontera):** Interfaz con el mundo exterior. Existe un `«boundary»` para cada actor humano (interfaz/portal lógico de consulta o auditoría) y uno por cada sistema externo o disparador (`RedmineAdapter`, `GitLabAdapter`, `GLPIAdapter`, `SonarQubeAdapter`, `MensajeriaWebhookAdapter`, `SMTPAdapter`, `SchedulerETLBoundary`).
- **`«control»` (Controlador de Caso de Uso):** Orquesta la secuencia de aplicación, coordinación temporal, reintentos y manejo de fallos del Caso de Uso.
  - **Diferencia clave vs. MVC y prevención de *Control Clase Dios*:** A diferencia de un *Controller* HTTP delgado en MVC, un `«control»` en ECB sí dirige el flujo completo del caso de uso, pero **no** absorbe las reglas intrínsecas de cálculo ni las invariantes de negocio propias de los datos; esas responsabilidades permanecen encapsuladas en las clases `«entity»`.
- **`«entity»` (Entidad del Dominio):** Objeto conceptual persistente cuyo nombre proviene **exactamente** del Diagrama de Clases de la Semana 4.

#### Procedimiento obligatorio de 6 pasos para construir la Vista de Procesos
1. Tomar la especificación textual Cockburn de un Caso de Uso significativo (Semana 3).
2. Crear un participante `«boundary»` distinto por cada actor primario y cada actor secundario involucrado.
3. Definir el `«control»` (o controladores delegados si hay subflujos autónomos) que orquesta el caso de uso.
4. Mapear los `«entity»` usando **exactamente** los nombres de las clases aprobadas en la Vista Lógica (Semana 4).
5. **Auditar la tabla ECB** contra la matriz de interacciones permitidas/prohibidas para garantizar **cero violaciones de robustez**.
6. Asignar responsabilidades (mensajes/métodos) verificando 1:1 que cada operación invocada sobre una `«entity»` exista en el Diagrama de Clases.

### 2.4 Matriz Estricta de Reglas de Robustez ECB

| Origen ➔ Destino | `Actor` | `«boundary»` | `«control»` | `«entity»` |
|---|:---:|:---:|:---:|:---:|
| **`Actor`** | × | **✓ PERMITIDA** | × PROHIBIDA | × PROHIBIDA |
| **`«boundary»`** | **✓ PERMITIDA** | × PROHIBIDA | **✓ PERMITIDA** | × PROHIBIDA |
| **`«control»`** | × PROHIBIDA | **✓ PERMITIDA** | **✓ PERMITIDA** | **✓ PERMITIDA** |
| **`«entity»`** | × PROHIBIDA | × PROHIBIDA | × PROHIBIDA | **✓ PERMITIDA** |

**Resumen de prohibiciones críticas (Auditoría Obligatoria):**
- × Un **Actor** jamás habla directo con un `«control»` ni con una `«entity»`.
- × Un **`«boundary»`** jamás lee ni modifica directamente una `«entity»`; siempre delega en un `«control»`.
- × Dos **`«boundary»`** no dialogan entre sí sin un `«control»` mediador.
- × Una **`«entity»`** jamás invoca hacia arriba a un `«control»` ni a un `«boundary»` (retorna resultados síncronos, nunca acopla hacia capas externas).

### 2.5 Fragmentos Combinados Obligatorios en Secuencias UML 2.5.1
No utilizar diagramas lineales planos cuando la especificación de Semana 3 describe condiciones, bucles, paralelismo o exclusión mutua. Deben emplearse explícitamente los operadores estándar UML 2.5.1:

- **`alt` / `else` (Alternativas excluyentes):** Ramas condicionales `if / else` (ej. extracción completa vs. preservación de último snapshot válido en sincronización parcial; hit en caché vs. recálculo; webhook Discord/Slack exitoso vs. fallback a Servidor SMTP).
- **`opt` (Opcional):** Ejecución condicionada sin rama alternativa (ej. disparo del punto de extensión `CU-02` únicamente cuando un indicador cruza umbral o existe inconsistencia nueva).
- **`loop` (Iteración):** Repetición con guarda explícita (ej. hasta 3 reintentos con *backoff exponencial*; iteración sobre fuentes externas o ítems/desarrolladores).
- **`break` (Interrupción de excepción crítica):** Aborta inmediatamente el fragmento contenedor ante error terminal (ej. error de autenticación `401/403`, payload corrupto o rango de fechas inválido).
- **`par` (Ejecución paralela/concurrente):** Hilos o tareas simultáneas independientes (ej. consulta concurrente a Redmine, GLPI, GitLab y SonarQube dentro del presupuesto de `<= 5 s` por fuente).
- **`critical` (Región crítica atómica):** Exclusión mutua para prevenir condiciones de carrera (ej. publicación/intercambio atómico del snapshot consolidado o verificación + registro atómico de la ventana de deduplicación de 24 horas en alertas).

### 2.6 Regla Inexcusable de Trazabilidad 1:1 (Semana 4 ↔ Semana 5)
> **REGLA BLOQUEANTE DE CIERRE:** Todo participante `«entity»` en un Diagrama de Secuencia **debe existir** con nombre idéntico en el Diagrama de Clases (`docs/uml/diagrama-clases.puml`), y todo mensaje dirigido a esa `«entity»` **debe coincidir exactamente** (nombre, parámetros y retorno) con una operación pública declarada en la clase correspondiente y documentada en su Diccionario de Clases (`docs/vista-logica-clases.md`). Si al modelar la Vista de Procesos aparece una operación nueva o se ajusta una firma, es obligatorio actualizar primero la Vista Lógica antes de cerrar la entrega.

---

## 3. Apéndice A — Estructura Consolidada del DAS v3.0 (Pág. 36)

El **Documento de Arquitectura de Software (DAS v3.0)** del curso se organiza en 6 secciones integradas:

| Sección DAS v3.0 | Contenido normativo | Estado en el repositorio |
|---|---|---|
| **1. Introducción, TDR y Alcance (Sem. 1–2)** | Propósito del sistema, alcance, glosario, restricciones tecnológicas/organizacionales y Cadena de Valor TOGAF ADM | Cubierto en matrices Excel + `docs/uml/cadena-de-valor-togaf-adm.puml` |
| **2. Interesados y Calidad (Sem. 2)** | Actores/interesados, `40 RF`, `40 HU` (`225 pts`) y `18 RNF` bajo ISO/IEC 25010:2023 + Escenarios de Calidad de 6 partes | **Cerrado** (`docs/Requisitos-padre-julian-lorente.xlsx`, `docs/historias-usuario-padre-julian-lorente.xlsx`) |
| **3. Vista de Escenarios +1 (Sem. 3)** | Modelo consolidado de Casos de Uso (`CU-01`..`CU-08`) y Especificaciones Cockburn de 8 secciones | **Cerrado** (`docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`) |
| **4. Vista Lógica (Sem. 4)** | Extracción gramatical y 4 filtros, Tarjetas CRC, Diagrama de Clases UML 2.5.1 (Nivel Especificación), Diccionario de Clases (5 campos), Matriz Escenarios vs. Clases y Diagrama de Estados | **Pendiente — Siguiente paso obligatorio** (`docs/handoff/prompt-vista-logica.md`) |
| **5. Vista de Procesos (Sem. 5)** | Procesos/tareas concurrentes, Tablas ECB auditadas, Diagramas de Secuencia UML 2.5.1 con fragmentos combinados y Matriz 1:1 con Semana 4 | **Pendiente — Ejecutar tras cerrar Vista Lógica** (`docs/handoff/prompt-vista-procesos.md`) |
| **6. Vista de Desarrollo (Sem. 6)** | Estructura de módulos/componentes implementables, decisiones de stack e infraestructura | **Posterior** (`docs/handoff/skills-fase-codigo.md`) |
