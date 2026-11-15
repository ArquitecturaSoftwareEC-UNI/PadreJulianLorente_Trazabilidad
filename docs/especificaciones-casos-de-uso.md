# Especificaciones de Casos de Uso — Vista de Escenarios (+1)

**Proyecto:** Sistema de Trazabilidad TI — Reto 2 (COAC Padre Julián Lorente)  
**Marco Normativo:** Modelo 4+1 de Kruchten (Vista +1), UML 2.5.1, Plantilla de Alistair Cockburn, ISO/IEC/IEEE 42010 e ISO/IEC 25010:2023  
**Diagrama Maestro Asociado:** [`docs/uml/casos-de-uso.puml`](uml/casos-de-uso.puml)  
**Fuente de Requisitos:** [`historias-usuario-padre-julian-lorente-v2.xlsx`](../historias-usuario-padre-julian-lorente-v2.xlsx) y [`trazabilidad-proyecto-padre-julian-lorente.xlsx`](../trazabilidad-proyecto-padre-julian-lorente.xlsx)

> **Alcance Estricto del Artefacto:** Este documento consolida exclusivamente la **Vista de Escenarios (+1)** (catálogo general, selección arquitectónica, especificaciones Cockburn y trazabilidad de requisitos). El diseño estructural, dinámico y de organización de código queda fuera del alcance de esta fase y será derivado por el equipo a partir de estos contratos funcionales y no funcionales.

---

## 1. Catálogo Consolidado de Casos de Uso (CU-01 a CU-08)

De acuerdo con el diagrama maestro [`docs/uml/casos-de-uso.puml`](uml/casos-de-uso.puml), la frontera del sistema comprende **ocho casos de uso a nivel del mar**, sin fragmentación ni descomposición funcional, con **dos actores primarios** a la izquierda (`Tiempo (Temporizador CRON)` y `Jefe de Desarrollo / PMO`) y **seis actores secundarios** a la derecha (`Redmine`, `GLPI`, `GitLab`, `SonarQube`, `Canal de Mensajería Corporativo` y `Servicio de Correo Corporativo`). Los desarrolladores operan únicamente sobre sus herramientas de origen y no ingresan ni poseen cuentas en la plataforma (`RF-030`).

| Código    | Caso de Uso (Verbo + Objeto)                        | Actor Primario                               | Actores Secundarios / Relación UML                                                                                 | Requisitos Funcionales (1:1 con HU)                                                              | RNFs Asociados (ISO/IEC 25010:2023)                                         | Prioridad / Tipo                  |
|:--------- |:--------------------------------------------------- |:-------------------------------------------- |:------------------------------------------------------------------------------------------------------------------ |:------------------------------------------------------------------------------------------------ |:--------------------------------------------------------------------------- |:--------------------------------- |
| **CU-01** | Sincronizar y consolidar trazabilidad e indicadores | `Tiempo (Temporizador CRON)`                 | `Redmine`, `GLPI`, `GitLab`, `SonarQube`                                                                           | `RF-001`..`RF-005`, `RF-011`..`RF-022`, `RF-029`, `RF-033`, `RF-037`, `RF-038` *(21 RF / 21 HU)* | `RNF-001`, `RNF-003`, `RNF-008`, `RNF-009`, `RNF-016`, `RNF-017`            | Significativo (Cockburn completo) |
| **CU-02** | Emitir alerta operativa                             | `Tiempo (Temporizador CRON)` *(vía `CU-01`)* | `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo` · `<<extend>> CU-01 [umbral crítico superado]` | `RF-006`, `RF-023`, `RF-024`, `RF-025`, `RF-031` *(5 RF / 5 HU)*                                 | `RNF-004`, `RNF-010`, `RNF-015`, `RNF-016`                                  | Significativo (Cockburn completo) |
| **CU-03** | Autenticar usuario                                  | `Jefe de Desarrollo / PMO`                   | Ninguno                                                                                                            | `RF-030` *(1 RF / `HU-001`)*                                                                     | `RNF-011`, `RNF-012`, `RNF-013`                                             | Complementario (Ficha resumida)   |
| **CU-04** | Consultar tablero de control                        | `Jefe de Desarrollo / PMO`                   | Ninguno *(lee datos consolidados de `CU-01`)*                                                                      | `RF-007`, `RF-032`, `RF-034`, `RF-035`, `RF-036` *(5 RF / 5 HU)*                                 | `RNF-002`, `RNF-005`, `RNF-008`, `RNF-011`, `RNF-012`, `RNF-013`, `RNF-014` | Significativo (Cockburn completo) |
| **CU-05** | Filtrar indicadores del tablero                     | `Jefe de Desarrollo / PMO`                   | `<<extend>> CU-04 [análisis focalizado]`                                                                           | `RF-008`, `RF-009`, `RF-028`, `RF-039` *(4 RF / 4 HU)*                                           | `RNF-006`, `RNF-012`, `RNF-013`, `RNF-018`                                  | Significativo (Cockburn completo) |
| **CU-06** | Auditar trazabilidad de causa raíz                  | `Jefe de Desarrollo / PMO`                   | Ninguno *(lee expedientes de `CU-01`)*                                                                             | `RF-010` *(1 RF / `HU-037`)*                                                                     | `RNF-001`, `RNF-007`, `RNF-012`, `RNF-013`                                  | Significativo (Cockburn completo) |
| **CU-07** | Gestionar alerta operativa                          | `Jefe de Desarrollo / PMO`                   | Ninguno                                                                                                            | `RF-026`, `RF-027` *(2 RF / `HU-038`, `HU-040`)*                                                 | `RNF-004`, `RNF-007`, `RNF-011`, `RNF-012`, `RNF-013`                       | Complementario (Ficha resumida)   |
| **CU-08** | Exportar reporte trimestral                         | `Jefe de Desarrollo / PMO`                   | Ninguno                                                                                                            | `RF-040` *(1 RF / `HU-039`)*                                                                     | `RNF-001`, `RNF-002`, `RNF-012`, `RNF-013`                                  | Complementario (Ficha resumida)   |

---

## 2. Filtro de Relevancia Arquitectónica

Para concentrar la especificación detallada donde residen las decisiones de diseño críticas, el catálogo completo fue evaluado mediante los cinco criterios oficiales de relevancia arquitectónica:

| Caso de Uso                                                    | 1. Núcleo del negocio | 2. Cobertura extremo a extremo | 3. Prueba atributos de calidad                          | 4. Fronteras externas       | 5. Alto riesgo técnico                                             | Resultado de Selección |
|:-------------------------------------------------------------- |:---------------------:|:------------------------------:|:-------------------------------------------------------:|:---------------------------:|:------------------------------------------------------------------:|:---------------------- |
| **CU-01: Sincronizar y consolidar trazabilidad e indicadores** | **Sí**                | **Sí**                         | **Sí** (`RNF-001`, `003`, `008`, `009`, `016`, `017`)   | **Sí** (4 plataformas)      | **Sí** (idempotencia, reintentos, LOPDP 2021)                      | **Seleccionado (5/5)** |
| **CU-02: Emitir alerta operativa**                             | **Sí**                | **Sí**                         | **Sí** (`RNF-004`, `010 < 200 ms`, `015`, `016`)        | **Sí** (2 canales externos) | **Sí** (anti-spam 24h, conmutación ante caída)                     | **Seleccionado (5/5)** |
| **CU-03: Autenticar usuario**                                  | No (soporte)          | Parcial                        | Medio (`RNF-013`)                                       | No                          | Bajo (acceso institucional único solo lectura)                     | Complementario (Ficha) |
| **CU-04: Consultar tablero de control**                        | **Sí**                | **Sí**                         | **Sí** (`RNF-002`, `005`, `008`, `011`, `014 <= 2 s`)   | No directa                  | **Sí** (25 sesiones concurrentes, refresco incremental)            | **Seleccionado (4/5)** |
| **CU-05: Filtrar indicadores del tablero**                     | **Sí**                | **Sí**                         | **Sí** (`RNF-006 < 300 ms`, `RNF-018 <= 400 ms`)        | No                          | **Sí** (intersección multidimensional sobre 5.000 registros)       | **Seleccionado (4/5)** |
| **CU-06: Auditar trazabilidad de causa raíz**                  | **Sí**                | **Sí**                         | **Sí** (`RNF-001` 100 % reproducible, `RNF-007 < 10 s`) | No directa                  | **Sí** (correlación de 4 niveles + código + excepciones sin traza) | **Seleccionado (4/5)** |
| **CU-07: Gestionar alerta operativa**                          | Medio                 | Parcial                        | Medio (`RNF-004`, `011`)                                | No                          | Bajo (persistencia de estado y cierre)                             | Complementario (Ficha) |
| **CU-08: Exportar reporte trimestral**                         | Medio                 | Parcial                        | Medio (`RNF-001`, `002`)                                | No                          | Bajo (agregación trimestral sobre consolidado)                     | Complementario (Ficha) |

---

## 3. Especificaciones Detalladas (Plantilla de Alistair Cockburn)

---

### CU-01: Sincronizar y consolidar trazabilidad e indicadores

#### 1. Identificador, Nombre y Descripción

- **Identificador y Nombre:** `CU-01` — Sincronizar y consolidar trazabilidad e indicadores.
- **Descripción:** Ejecuta automáticamente cada cinco minutos la extracción en solo lectura desde las herramientas operativas externas, depura datos personales sensibles, enlaza la cadena de cuatro niveles hasta el código, calcula capacidad, avance y eficiencia en las cinco etapas y consolida los resultados sin duplicados.

#### 2. Actores

- **Actor Primario:** `Tiempo (Temporizador CRON)` *(consumidor posterior de los resultados consolidados: `Jefe de Desarrollo / PMO`)*.
- **Actores Secundarios:** `Redmine`, `GLPI`, `GitLab`, `SonarQube`.

#### 3. Disparador

Vencimiento automático del intervalo periódico programado cada **5 minutos** (`RF-029` / `HU-003`).

#### 4. Precondiciones

- El temporizador periódico de ciclos cada 5 minutos se encuentra activo.
- Existen credenciales institucionales vigentes con permisos estrictos de **solo lectura** para `Redmine`, `GLPI`, `GitLab` y `SonarQube` (`RNF-003`).
- El almacenamiento consolidado histórico y la tabla de auditoría matemática se encuentran operativos.

#### 5. Postcondiciones Verificables

- El almacenamiento consolidado queda actualizado de manera idempotente (**0 %** de registros duplicados, `RNF-009`) con la jerarquía de cuatro niveles (`Épica POA/PETI`, `Historia/Spec REQ o INI.TI`, `Tarea/Issue #ID`, `Rama/Merge Request/Commits tipados`), tickets `GLPI #ID` y hallazgos de `SonarQube`, agrupados en expedientes por módulo del core financiero.
- Toda actividad sin cadena completa queda segregada y visible como «Trabajo en código sin trazabilidad planificada» (`RF-021`) o «Incidente de soporte sin trazabilidad en desarrollo» (`RF-022`).
- Los indicadores vigentes de ocupación semanal (`base 40 h`), puntos Fibonacci (`2, 5, 13`), avance real verificado frente al planificado, eficiencia (`70 %` avance + `30 %` calidad), retornos por fallo y distribución en las **5 etapas oficiales** quedan persistidos junto con el rastro inmutable de cálculo (`RNF-001`) y sin datos personales sensibles de socios (`RNF-017`).
- De superarse algún umbral crítico operativo, queda habilitada la evaluación del punto de extensión `<<extend>> CU-02: Emitir alerta operativa`.

#### 6. Flujo Básico ("Camino Feliz")

1. El actor `Tiempo (Temporizador CRON)` inicia el ciclo periódico de sincronización cada 5 minutos (`RF-029`).
2. El sistema consulta en modo exclusivamente de solo lectura los registros vigentes en `Redmine`, `GLPI`, `GitLab` y `SonarQube` (`RF-011`, `RNF-003`).
3. El sistema depura los textos extraídos enmascarando o eliminando todo dato personal sensible de socios antes de su análisis y persistencia (`RNF-017`).
4. El sistema construye la línea base de fechas, tiempos, complejidad y dependencias, clasifica cada petición en los cuatro niveles oficiales e identifica el módulo del core financiero (`RF-012`, `RF-033`).
5. El sistema cruza referencias entre incidentes `GLPI #ID`, tareas de `Redmine`, ramas, Merge Requests, commits tipados, revisiones de QA cruzado, estado de validaciones automáticas y reportes de `SonarQube`, detectando retornos por fallo (`RF-013`, `RF-014`, `RF-015`, `RF-017`).
6. El sistema verifica si cada actividad posee vínculo completo para unificar el expediente cronológico por módulo financiero o clasificar la actividad como trabajo sin trazabilidad (`RF-016`, `RF-018`, `RF-020`, `RF-021`, `RF-022`).
7. El sistema calcula la carga laboral efectiva (`base 40 h` y puntos Fibonacci), el avance real respaldado por evidencia en código frente al planificado, la eficiencia (`70 %` avance + `30 %` calidad) y normaliza la etapa oficial vigente (`RF-001`..`RF-005`, `RF-019`, `RF-037`, `RF-038`).
8. El sistema persiste de forma idempotente los expedientes, indicadores y registros auditables de cálculo sin duplicados y, ante superación de umbrales críticos, habilita la extensión condicional hacia `CU-02` (`RNF-001`, `RNF-009`).

#### 7. Flujos Alternos vs. Flujos de Excepción

- **Flujos Alternos (logran el objetivo o resuelven variación válida):**
  
  - **2a (Falla transitoria o espera `> 10 segundos` en plataforma externa — Blindaje con snapshot parcial):** Si `Redmine`, `GLPI`, `GitLab` o `SonarQube` no responde en 10 segundos o devuelve indisponibilidad temporal, el sistema conserva intacto el **100 %** de los últimos datos consolidados visibles, registra el fallo técnico y ejecuta hasta **3 reintentos automáticos** en segundo plano. Si tras los 3 reintentos esa única fuente externa permanece inalcanzable en el ciclo, el sistema conserva y reutiliza el último snapshot válido consolidado de dicha plataforma para los cruces dependientes (`RF-014`, `RF-016` y `RF-038`), registra un estado de sincronización parcial con la última marca temporal exitosa de la fuente caída y procesa las demás plataformas disponibles antes de reincorporarse al paso 3; queda estrictamente prohibido degradar o reclasificar falsamente como «trabajo sin trazabilidad» o «sin respaldo en código» las actividades cuyo origen haya caído temporalmente en ese ciclo.
    - **Insight Arquitectónico → TOLERANCIA A FALLOS, SNAPSHOT PARCIAL Y MODULARIDAD (`RNF-008`, `RNF-016`):** Impone adaptadores desacoplados por plataforma con temporizador límite de 10 segundos, política acotada de 3 reintentos y preservación granular del último snapshot válido por fuente origen junto con su marca temporal verificada, evitando que la indisponibilidad temporal de una herramienta externa contamine los indicadores de trazabilidad o bloquee el tablero.
  - **5a (Errores de digitación en código `GLPI #ID` dentro de Redmine):** Si la referencia escrita en `Redmine` presenta mayúsculas, espacios o guiones irregulares, el sistema aplica coincidencia tolerante de patrones para enlazar correctamente el ticket `GLPI #ID` con la tarea y el desarrollador asignado, reincorporándose al paso 6 (`RF-013`).
  - **5b (Detección de retorno por fallo hacia desarrollo):** Si una actividad retrocede en `Redmine` a estado «En curso» por rechazo en Certificación o fallo en validaciones automatizadas de `GitLab`, el sistema registra el evento de retorno para descontar 10 puntos en el componente de calidad de eficiencia y se reincorpora al paso 6 (`RF-015`, `RF-005`).
  - **6a (Actividad en GitLab sin tarea planificada en Redmine):** Toda rama, commit o Merge Request sin petición vinculada en `Redmine` se clasifica bajo «Trabajo en código sin trazabilidad planificada», registrando autor, repositorio y esfuerzo en su carga operativa, y continúa al paso 7 (`RF-021`).
  - **6b (Incidente activo en GLPI sin petición espejo en Redmine):** Todo ticket asignado a tecnología en `GLPI` sin espejo `GLPI #ID` en `Redmine` se registra como «Incidente de soporte sin trazabilidad en desarrollo» e igualmente suma su esfuerzo estimado o estándar en la carga del responsable, continuando al paso 7 (`RF-022`, `RF-003`).
  - **7a (Avance reportado en Redmine sin respaldo real en código):** Si una tarea reporta `> 25 %` sin rama activa ni commits tipados (`feat(#ID)`, `fix(#ID)`, `test(#ID)`, `refactor(#ID)`), o `100 %` sin Merge Request aprobado ni validación automatizada exitosa, el sistema marca brecha de evidencia y utiliza el progreso verificable en código para calcular el avance real, pasando al paso 8 (`RF-038`).
  - **7b (Divergencia entre asignado en Redmine y autor de commits):** Si el autor del código difiere del asignado en `Redmine`, el sistema deja constancia de la divergencia y registra igualmente a los revisores reales de QA cruzado antes de continuar al paso 8 (`RF-017`).

- **Flujos de Excepción (terminan en fracaso `(X)` sin cumplir la postcondición del ciclo):**
  
  - **2b (Indisponibilidad total de fuentes externas o credenciales inválidas tras 3 reintentos) `(X)`:** Si todas las plataformas origen fallan tras agotarse los 3 reintentos o se rechazan las credenciales de solo lectura, el sistema registra el evento crítico sin sobreescribir los datos previos y **termina sin cumplir la postcondición de sincronización (`X`)** (`RNF-003`, `RNF-008`).
  - **3a (Fallo en el componente de enmascaramiento de datos personales sensibles) `(X)`:** Si no es posible verificar la eliminación o enmascaramiento del **100 %** de datos sensibles de socios (`cédulas`, `números de cuenta`, `correos particulares`), el sistema aborta de inmediato el lote antes de transformar o persistir cualquier dato y **termina en fracaso sin cumplir la postcondición (`X`)**.
    - **Insight Arquitectónico → PRIVACIDAD POR DISEÑO LOPDP 2021 (`RNF-017`):** La depuración actúa como compuerta obligatoria previa a toda persistencia o analítica, garantizando que ningún dato personal sensible de socios cruce hacia los expedientes retenidos por 24 meses.
  - **8a (Error de integridad o consistencia durante la consolidación final) `(X)`:** Si falla el almacenamiento consolidado o se produce conflicto irrecuperable al actualizar el lote, el sistema revierte completamente las escrituras parciales del ciclo y **termina sin actualizar la línea vigente (`X`)**.
    - **Insight Arquitectónico → IDEMPOTENCIA Y ATOMICIDAD (`RNF-009`):** Exige llaves naturales compuestas (`Épica POA + Historia/Spec + Tarea #ID + Ticket GLPI + Commit`) y consolidación atómica para garantizar **0 %** de duplicados ante ejecuciones repetidas o reintentos.

#### 8. Reglas de Negocio y Requisitos Especiales

- **Trazabilidad Funcional Completa (`21 RF` / `21 HU`):**
  1. **Disparador y extracción no intrusiva (`RF-029`/`HU-003`, `RF-011`/`HU-004`):** Ciclo automático cada 5 minutos mediante credenciales exclusivas de solo lectura; cero operaciones de escritura o borrado en herramientas origen (`RNF-003`).
  2. **Jerarquía de 4 niveles y módulos del core financiero (`RF-033`/`HU-005`, `RF-012`/`HU-006`, `RF-018`/`HU-012`):** Estructura `Épica (POA/PETI)` ➔ `Historia/Spec (REQ / INI.TI)` ➔ `Tarea/Issue (#ID)` ➔ `Rama, Merge Request y Commits tipados`; clasificación obligatoria en `FitSwitch`, `SWI`, `Canales Electrónicos`, `Crédito`, `Antifraude` o `Nómina`.
  3. **Correlación, código, retornos y verificación (`RF-013`/`HU-007`, `RF-014`/`HU-008`, `RF-015`/`HU-009`, `RF-016`/`HU-010`, `RF-017`/`HU-011`, `RF-020`/`HU-013`, `RF-021`/`HU-014`, `RF-022`/`HU-015`):** Cruce tolerante `GLPI #ID`, clasificación de commits (`feat(#ID)`, `fix(#ID)`, `test(#ID)`, `refactor(#ID)`), QA cruzado, estado de validaciones automáticas, hallazgos de `SonarQube`, historial en 5 etapas y segregación explícita de actividades sin trazabilidad.
  4. **Esfuerzo Fibonacci y carga semanal base 40h (`RF-001`/`HU-016`, `RF-002`/`HU-017`, `RF-003`/`HU-018`):**
     - Conversión de complejidad: `Nivel 1 Baja = 2 puntos`, `Nivel 2 Media = 5 puntos`, `Nivel 3 Alta = 13 puntos`.
     - Base semanal lunes a viernes (`40 horas = 8 h/día`): **Disponible (Verde)** `< 75 %` (`< 30 h`), **Carga Óptima (Amarillo)** `75 %–100 %` (`30 h–40 h`), **Sobreasignado (Rojo)** `> 100 %` (`> 40 h`).
     - Carga efectiva = Redmine (`Análisis` / `En curso`) + incidentes `GLPI` (`4 h / 3 pts` estándar; `8 h / 5 pts` crítico core sin estimación manual) + QA cruzado `GitLab` (`2 h / 1 pt` por revisión activa).
  5. **Avance real, evidencia en código, eficiencia y 5 etapas (`RF-004`/`HU-019`, `RF-037`/`HU-020`, `RF-038`/`HU-021`, `RF-005`/`HU-022`, `RF-019`/`HU-028`):**
     - Avance `> 25 %` exige rama activa y commits tipados; `100 %` exige Merge Request aprobado y validaciones automatizadas exitosas.
     - Eficiencia (`0–100 %`) = `70 %` avance real verificado frente al planificado a la fecha + `30 %` calidad técnica (`-10 pts` por cada retorno desde pruebas/Certificación y `-5 pts` por bug crítico o vulnerabilidad en `SonarQube`), excluyendo horas de incidentes emergentes `GLPI`.
     - Normalización estricta a las **5 etapas oficiales**: `1. Análisis`, `2. Desarrollo`, `3. Certificación`, `4. Despliegue`, `5. Estabilización`.
- **Atributos de Calidad Asociados (`6 RNF`):**
  - `RNF-001` (Responsabilidad): Retención inmutable mínimo **12 meses** de fórmulas, pesos y entradas con **100 %** de reproducibilidad matemática.
  - `RNF-003` (Seguridad): **100 %** extracciones de solo lectura y **0** modificaciones en sistemas externos.
  - `RNF-008` (Fiabilidad): Timeout externo `10 s`, hasta **3 reintentos automáticos** y **100 %** de continuidad sobre los últimos datos consolidados.
  - `RNF-009` (Integridad): **0 %** de duplicados tras ciclos sucesivos mediante llaves naturales compuestas.
  - `RNF-016` (Mantenibilidad): Desacoplamiento por adaptadores con **0** cambios al núcleo de dominio al incorporar fuentes o notificaciones.
  - `RNF-017` (Privacidad LOPDP 2021): Enmascaramiento/eliminación del **100 %** de datos sensibles de socios previo a la persistencia y retención histórica técnica de al menos **24 meses**.

---

### CU-02: Emitir alerta operativa

#### 1. Identificador, Nombre y Descripción

- **Identificador y Nombre:** `CU-02` — Emitir alerta operativa *(extiende a `CU-01` bajo la condición `[umbral crítico superado]`)*.
- **Descripción:** Evalúa desviaciones operativas tras cada ciclo de consolidación, redacta la alerta en lenguaje directo, suprime duplicados dentro de las últimas 24 horas y despacha el aviso por los canales corporativos correspondientes dejando registro persistente.

#### 2. Actores

- **Actor Primario:** `Tiempo (Temporizador CRON)` — actor primario real que ejecuta `CU-01` y detona la extensión `CU-02` al superarse un umbral crítico *(destinatario pasivo de los avisos emitidos: `Jefe de Desarrollo / PMO`)*.
- **Actores Secundarios:** `Canal de Mensajería Corporativo`, `Servicio de Correo Corporativo`.

#### 3. Disparador

Cumplimiento de la condición de extensión `[umbral crítico superado]` al término del paso 8 de `CU-01` por sobrecarga semanal (`> 40 h`), permanencia `> 5 días hábiles` en Certificación, retorno por fallo (`RF-031`), incidente Prioridad 1 o cambio de prioridad de proyecto (`RF-025`).

#### 4. Precondiciones

- `CU-01` ha finalizado la consolidación actualizada de indicadores para el ciclo de 5 minutos vigente.
- Están configurados los canales cifrados autorizados hacia `Canal de Mensajería Corporativo` y `Servicio de Correo Corporativo` (`RNF-015`).
- Se encuentra accesible el historial de alertas enviadas en las últimas 24 horas y de alertas atendidas en `CU-07`.

#### 5. Postcondiciones Verificables

- Todas las reglas de umbral quedan evaluadas dentro de `< 200 ms` (`RNF-010`).
- Cada alerta nueva o con cambio de estado se transmite al `Canal de Mensajería Corporativo` (`RF-024`) y, cuando corresponda a criterios de escalamiento o fallo de entrega en mensajería, mediante el `Servicio de Correo Corporativo` (`RF-025`).
- El **100 %** de las alertas aprobadas y despachadas (así como los intentos fallidos o suprimidos) queda persistido con identificador único del evento, fecha/hora, categoría de desviación, destinatarios y estado real de entrega (`RNF-004`).

#### 6. Flujo Básico ("Camino Feliz")

1. Invocado como extensión de `CU-01` bajo el impulso de `Tiempo (Temporizador CRON)`, el sistema evalúa el conjunto completo de umbrales críticos sobre los indicadores consolidados (`RF-031`, `RNF-010`).
2. El sistema redacta el contenido de la alerta en español claro indicando el desarrollador responsable, el módulo financiero afectado y el valor exacto de la desviación (`RF-006`).
3. El sistema verifica en la bitácora de las últimas 24 horas que no exista una alerta idéntica vigente sin cambios ni marcada como atendida para el mismo incidente o tarea (`RF-023`, `RF-026`).
4. El sistema aprueba el envío y remite la notificación por canal cifrado al `Canal de Mensajería Corporativo` del equipo de gestión (`RF-024`, `RNF-015`).
5. Cuando el evento supera 5 días hábiles en Certificación, corresponde a Prioridad 1 o implica cambio de prioridad de proyecto, el sistema despacha además la notificación por canal cifrado a través del `Servicio de Correo Corporativo` (`RF-025`, `RNF-015`).
6. El sistema registra de forma persistente el evento emitido con su identificador único, fecha/hora, categoría, destinatarios y resultado de entrega (`RNF-004`).

#### 7. Flujos Alternos vs. Flujos de Excepción

- **Flujos Alternos (logran el objetivo o aplican mitigación válida):**
  
  - **3a (Supresión anti-spam dentro de 24 horas o alerta atendida en `CU-07`):** Si la alerta coincide en incidente/tarea, causa y severidad con un envío realizado dentro de las últimas 24 horas, o la alerta figura como «atendida»/«en seguimiento» sin agravamiento nuevo, el sistema bloquea la emisión repetida para evitar saturar a la jefatura, deja constancia en auditoría y termina cumpliendo la política anti-saturación (`RF-023`, `RF-026`).
  - **4a (Fallo o indisponibilidad del `Canal de Mensajería Corporativo`):** Si el `Canal de Mensajería Corporativo` rechaza o no confirma la recepción de la notificación, el sistema activa de forma automática el canal alterno obligatorio remitiendo la alerta íntegra mediante el `Servicio de Correo Corporativo` (`RF-025`) y continúa en el paso 6 registrando el fallo en mensajería y la entrega efectiva por correo.
    - **Insight Arquitectónico → DESACOPLAMIENTO ASÍNCRONO Y CONMUTACIÓN POR ADAPTADORES (`RF-025`, `RNF-016`):** El motor de alertas debe operar desacoplado en segundo plano mediante contratos intercambiables de notificación, garantizando que la caída o lentitud de un proveedor externo jamás retrase el ciclo principal ETL de `CU-01` ni exija cambios en las reglas de negocio.

- **Flujos de Excepción (terminan en fracaso `(X)` sin cumplir la postcondición de envío):**
  
  - **1a (Fallo interno en motor de evaluación de reglas) `(X)`:** Si una inconsistencia imprevista en las métricas impide completar la evaluación dentro de `< 200 ms`, el sistema registra el error técnico en bitácora sin emitir alertas erróneas y **termina sin cumplir la postcondición de notificación (`X`)**.
  - **5a (Caída simultánea de `Canal de Mensajería Corporativo` y `Servicio de Correo Corporativo`) `(X)`:** Si ambos canales externos fallan o rechazan la conexión cifrada, el sistema persiste obligatoriamente la alerta en bitácora con estado de entrega fallida para visibilidad en la vista única y reintento posterior (`RNF-004`), pero **termina sin satisfacer la postcondición de distribución externa (`X`)**.
    - **Insight Arquitectónico → AUDITORÍA PERSISTENTE ANTE FALLO DE ENTREGA (`RNF-004`, `RNF-015`):** Persistir la intención y resultado del evento en almacenamiento durable antes y después del envío garantiza cero pérdida de alertas críticas aun ante cortes simultáneos de infraestructura externa, exigiendo en todo momento cifrado de transporte `>= TLS 1.2`.

#### 8. Reglas de Negocio y Requisitos Especiales

- **Trazabilidad Funcional Completa (`5 RF` / `5 HU`):**
  - `RF-031` / `HU-023`: Disparadores inmediatos: carga semanal `> 40 horas`, permanencia `> 5 días hábiles` en Certificación, o retorno por fallo desde pruebas/Certificación hacia desarrollo.
  - `RF-006` / `HU-024`: Lenguaje humano claro con desarrollador responsable, módulo financiero afectado y dato exacto de la desviación, sin códigos técnicos crudos.
  - `RF-023` / `HU-025`: Comparación contra alertas enviadas en las últimas **24 horas** para bloquear duplicados idénticos y aprobar únicamente novedades o cambios.
  - `RF-024` / `HU-026`: Envío de toda alerta aprobada al `Canal de Mensajería Corporativo`.
  - `RF-025` / `HU-027`: Envío mediante `Servicio de Correo Corporativo` ante `> 5 días hábiles` en Certificación, incidente Prioridad 1, cambio de prioridad de proyecto o fallo de entrega en el canal de mensajería.
- **Atributos de Calidad Asociados (`4 RNF`):**
  - `RNF-004` (Responsabilidad): **100 %** de alertas persistidas con ID único, fecha/hora, categoría, destinatarios y estado de entrega.
  - `RNF-010` (Eficiencia Temporal): Evaluación completa de umbrales en `< 200 ms`.
  - `RNF-015` (Seguridad en Comunicaciones): **100 %** del despacho externo cifrado sobre `TLS 1.2` o superior.
  - `RNF-016` (Mantenibilidad): Cambio o adición de proveedores de alerta con **0** archivos modificados en el núcleo de dominio.

---

### CU-04: Consultar tablero de control

#### 1. Identificador, Nombre y Descripción

- **Identificador y Nombre:** `CU-04` — Consultar tablero de control *(caso base extendido por `CU-05: Filtrar indicadores del tablero`)*.
- **Descripción:** Presenta al Jefe de Desarrollo y al PMO en una vista web única de solo lectura el estado consolidado de capacidad, avance real de proyectos POA/PETI, embudo de cinco etapas, cuellos de botella y alertas vigentes, con refresco incremental automático cada cinco minutos.

#### 2. Actores

- **Actor Primario:** `Jefe de Desarrollo / PMO` *(ambos roles comparten exactamente la misma vista y permisos en solo lectura)*.
- **Actores Secundarios:** Ninguno en tiempo real *(opera íntegramente sobre las proyecciones consolidadas por `CU-01`)*.

#### 3. Disparador

El `Jefe de Desarrollo / PMO` ingresa a la vista principal consolidada del sistema tras autenticarse o solicita visualizar el estado actualizado del área (`RF-032`).

#### 4. Precondiciones

- El usuario posee una **sesión institucional autenticada y vigente (`CU-03: Autenticar usuario`)** bajo perfil `Jefe de Desarrollo` o `PMO / Jefatura de Proyectos` (`RF-030` — la validación de credenciales no forma parte de este flujo).
- La comunicación del navegador hacia el sistema opera sobre sesión cifrada (`RNF-013`).
- Existe al menos una consolidación válida almacenada por `CU-01` (o estado inicial disponible para consulta).

#### 5. Postcondiciones Verificables

- El usuario accede en solo lectura a la vista única compartida con **todos** los proyectos POA/PETI vigentes y **todos** los desarrolladores activos sin tope fijo (`RF-032`).
- Se visualizan en español directo los semáforos de capacidad sobre 40 horas, tarjetas individuales (tareas de proyecto, incidentes GLPI, puntos Fibonacci y QA cruzado), comparativa de avance real vs. planificado por Épica POA/PETI, mapas de calor por módulo financiero y embudo de 5 etapas con cuellos de botella en rojo (`RF-007`, `RF-035`, `RF-036`, `RNF-002`).
- La carga inicial cumple `<= 2 s` (p95, hasta 25 sesiones / 5.000 registros) y los cambios derivados de cada sincronización cada 5 minutos se reflejan incrementalmente en `< 1 s` sin reiniciar ni recargar la sesión visual completa (`RF-034`, `RNF-014`).

#### 6. Flujo Básico ("Camino Feliz")

1. El `Jefe de Desarrollo / PMO` solicita consultar el estado consolidado de los proyectos y la carga operativa del equipo desde la vista única (`RF-032`).
2. El sistema recupera desde el almacenamiento consolidado las métricas vigentes de todos los desarrolladores activos, proyectos POA/PETI, etapas oficiales, módulos financieros y alertas (`RF-007`, `RF-032`).
3. El sistema organiza y presenta en lenguaje claro las tarjetas por desarrollador, la comparativa de avance real frente al planificado, el mapa de calor financiero y el embudo de cinco etapas resaltando en rojo los cuellos de botella (`RF-035`, `RF-036`, `RNF-002`, `RNF-005`).
4. El `Jefe de Desarrollo / PMO` revisa los indicadores críticos de sobrecarga, atraso y calidad sin necesidad de asistencia técnica (`RNF-002`, `RNF-005`).
5. Mientras la sesión permanece abierta, cada vez que concluye un ciclo programado de sincronización de `CU-01`, el sistema actualiza automáticamente en la vista únicamente los indicadores y registros que presentaron variaciones (`RF-034`, `RNF-014`).
6. Cuando el usuario requiera focalizar el análisis sobre un subconjunto de datos, el sistema habilita el punto de extensión `<<extend>> CU-05: Filtrar indicadores del tablero [análisis focalizado]`.

#### 7. Flujos Alternos vs. Flujos de Excepción

- **Flujos Alternos (cumplen la postcondición manteniendo operabilidad):**
  
  - **2a (Consulta mientras un sistema externo falla en el ciclo ETL):** Si `CU-01` reportó caída parcial en una plataforma origen, el sistema entrega de inmediato el **100 %** de los últimos datos consolidados válidos acompañados de la marca temporal de última sincronización exitosa, cumpliendo la postcondición de disponibilidad sin bloqueo (`RNF-008`).
    - **Insight Arquitectónico → LECTURA DESACOPLADA SOBRE PROYECCIONES CONSOLIDADAS (`RNF-008`, `RNF-014`):** Separar estrictamente el camino de lectura del tablero respecto al motor ETL impide que caídas o latencias en Redmine, GLPI, GitLab o SonarQube afecten la experiencia consultiva, garantizando `<= 2 s` (p95) bajo 25 sesiones activas simultáneas.
  - **3a (Acceso desde tableta o dispositivo móvil):** Si el usuario consulta desde resoluciones `768x1024 px` o `360x640 px`, el sistema reorganiza automáticamente los bloques de indicadores sin perder datos, controles ni visibilidad inmediata en `<= 5 s` de desviaciones críticas en rojo (`RNF-005`, `RNF-012`).
  - **5a (Cierre o expiración normal de sesión):** Al finalizar voluntariamente la sesión o expirar el tiempo de inactividad, el sistema cierra el canal de refresco incremental y libera la memoria asignada en `< 200 ms`, dejando **0** canales abiertos ni conexiones huérfanas (`RNF-011`).
    - **Insight Arquitectónico → LIBERACIÓN DETERMINISTA DE CANALES INCREMENTALES (`RNF-011`):** El cierre explícito y acotado a `< 200 ms` previene fugas de descriptores o suscripciones fantasma cuando múltiples directivos dejan pestañas abiertas o cierran su navegador abruptamente.

- **Flujos de Excepción (terminan en fracaso `(X)` sin cumplir la postcondición):**
  
  - **2b (Ausencia total de sincronización inicial en el sistema) `(X)`:** Si aún no existe ningún ciclo exitoso de `CU-01` en el almacenamiento consolidado, el sistema informa claramente que no hay consolidación histórica inicial disponible en lugar de mostrar métricas vacías engañosas y **termina sin presentar indicadores operativos (`X`)**.
  - **2c (Indisponibilidad del almacenamiento consolidado o pérdida de validez de sesión) `(X)`:** Si falla el servicio interno de lectura o la sesión autenticada caduca antes de completar la carga, el sistema muestra un mensaje comprensible de reintento o reingreso sin exponer detalles técnicos internos y **termina en fracaso sin cumplir la postcondición (`X`)**.

#### 8. Reglas de Negocio y Requisitos Especiales

- **Trazabilidad Funcional Completa (`5 RF` / `5 HU`):**
  - `RF-032` / `HU-002`: Consulta panorámica dinámica de todos los proyectos POA/PETI vigentes y todos los desarrolladores activos sin límite fijo de equipo, idéntica para Jefe de Desarrollo y PMO.
  - `RF-007` / `HU-029`: Tablero consolidado tras fase Load con semáforos de capacidad, embudo de 5 etapas y mapas de calor por módulo del core financiero actualizados cada 5 minutos.
  - `RF-034` / `HU-030`: Actualización incremental automática tras cada ciclo de 5 minutos únicamente de elementos modificados, sin recarga completa de la vista.
  - `RF-035` / `HU-031`: Tarjeta por desarrollador con semáforo semanal sobre 40h, división proyectos vs. incidentes GLPI, puntos Fibonacci (`2, 5, 13`) y revisiones QA cruzado.
  - `RF-036` / `HU-032`: Avance real vs. planificado por Épica POA/PETI (desglose a Historias y Tareas) y embudo de 5 etapas resaltando en rojo cuellos de botella (`> 35 %` tareas activas, `> 5 días` en Certificación o `> 2 días` en Despliegue).
- **Atributos de Calidad Asociados (`7 RNF`):**
  - `RNF-002`: Español directo sin códigos crudos (`>= 90 %` comprensión autónoma).
  - `RNF-005`: Identificación visual de desviación crítica roja en `<= 5 segundos`.
  - `RNF-008`: Disponibilidad del `100 %` del último estado consolidado ante fallos externos.
  - `RNF-011`: Cierre de actualización incremental y liberación de recursos en `< 200 ms` (`0` conexiones huérfanas).
  - `RNF-012`: Adaptabilidad completa en `1920x1080 px`, `768x1024 px` y `360x640 px`.
  - `RNF-013`: **100 %** del tráfico cifrado mediante `HTTPS` con `TLS 1.3`.
  - `RNF-014`: Respuesta inicial `<= 2 segundos` (p95, `25 sesiones`, `5.000 registros`) y actualización incremental post-sincronización `< 1 segundo`.

---

### CU-05: Filtrar indicadores del tablero

#### 1. Identificador, Nombre y Descripción

- **Identificador y Nombre:** `CU-05` — Filtrar indicadores del tablero *(extiende a `CU-04` bajo la condición `[análisis focalizado]`)*.
- **Descripción:** Permite al Jefe de Desarrollo y al PMO combinar criterios por desarrollador, módulo financiero, etapa oficial y rango de fechas sobre la vista única, recalculando de manera instantánea todos los indicadores e identificando cada criterio activo para su ajuste selectivo.

#### 2. Actores

- **Actor Primario:** `Jefe de Desarrollo / PMO` — actor primario real que activa esta extensión sobre `CU-04` al requerir `[análisis focalizado]`.
- **Actores Secundarios:** Ninguno.

#### 3. Disparador

El `Jefe de Desarrollo / PMO` activa el análisis detallado a partir de una alerta, cuello de botella o anomalía observada en el tablero (`CU-04` paso 6, `RF-008`).

#### 4. Precondiciones

- El usuario posee **sesión institucional autenticada vigente (`CU-03: Autenticar usuario`)** con perfil `Jefe de Desarrollo` o `PMO / Jefatura de Proyectos` (`RF-030` — no se autentica en este flujo).
- La vista del tablero de control (`CU-04`) se encuentra activa y con indicadores consolidados cargados.
- La conexión opera bajo cifrado de sesión vigente (`RNF-013`).

#### 5. Postcondiciones Verificables

- La vista única despliega exclusivamente las métricas, gráficos, tareas y evidencias de código que cumplen la intersección exacta de los filtros aplicados, recalculados en `<= 400 ms` (p95 sobre 5.000 registros) y sin parpadeos (`RF-009`, `RNF-018`).
- Se conserva visibilidad contextual de los indicadores iniciales para comparación (`RF-008`) y de cada criterio de filtrado activo de forma independiente para permitir su retiro selectivo (`RF-028`, `RF-039`).

#### 6. Flujo Básico ("Camino Feliz")

1. Ante un requerimiento de `[análisis focalizado]` sobre `CU-04`, el `Jefe de Desarrollo / PMO` solicita activar las opciones de segmentación del tablero conservando visibles las métricas generales (`RF-008`).
2. El sistema habilita en menos de 300 milisegundos las dimensiones de filtrado: desarrollador responsable, módulo del core financiero, etapa oficial y rango de fechas (`RF-009`, `RNF-006`).
3. El `Jefe de Desarrollo / PMO` selecciona uno o varios criterios simultáneos conforme al foco de su análisis (`RF-009`).
4. El sistema calcula en `<= 400 ms` la intersección exacta sobre los registros consolidados y recalcula de manera coherente todas las métricas, gráficos, tareas y evidencias visibles (`RF-009`, `RNF-018`).
5. El sistema muestra la vista focalizada manteniendo identificados de manera individual en la cabecera de consulta todos los criterios activos aplicados (`RF-028`).
6. Cuando el `Jefe de Desarrollo / PMO` decide retirar alguno de los criterios individuales, el sistema recalcula de inmediato la vista sobre los criterios restantes sin reiniciar la consulta completa (`RF-028`).

#### 7. Flujos Alternos vs. Flujos de Excepción

- **Flujos Alternos (resuelven variaciones garantizando coherencia de la vista):**
  
  - **4a (Combinación sin resultados — Cero coincidencias):** Si ningún registro satisface simultáneamente todos los criterios elegidos, el sistema conserva íntegro el diseño de la vista, presenta un aviso claro indicando cero resultados y ofrece una acción directa para limpiar todos los filtros y restablecer los indicadores globales coherentes (`RF-039`).
  - **4b (Actualización periódica cada 5 minutos mientras existen filtros activos):** Si concluye una sincronización de `CU-01` durante el análisis focalizado, el sistema aplica automáticamente la actualización incremental respetando exactamente la combinación de criterios activa sin reiniciar ni perder los filtros seleccionados (`RF-009`, `RF-034`).
  - **6a (Retiro completo de criterios activos):** Si el usuario remueve uno a uno los criterios aplicados (o ejecuta limpieza total), el sistema retorna limpiamente al estado global vigente de `CU-04`.

- **Flujos de Excepción (terminan en fracaso `(X)` preservando la vista estable previa):**
  
  - **3a (Criterio de fechas inválido o inconsistente) `(X)`:** Si el rango temporal indicado presenta fecha inicial posterior a la final o formato no admitido, el sistema rechaza ejecutar el recálculo, advierte el error en lenguaje claro sin alterar los indicadores actuales y **termina sin cumplir la postcondición de vista filtrada (`X`)**.
  - **4c (Fallo interno en el cálculo de intersección multidimensional) `(X)`:** Si ocurre un error en la consulta sobre el almacenamiento consolidado, el sistema registra el evento técnico, mantiene intactos los últimos indicadores estables desplegados y **termina en fracaso sin aplicar la consulta (`X`)**.
    - **Insight Arquitectónico → CONSULTA INDEXADA MULTIDIMENSIONAL Y DESACOPLO DE PRESENTACIÓN (`RNF-006 < 300 ms`, `RNF-018 <= 400 ms` p95):** Alcanzar `< 300 ms` en apertura contextual y `<= 400 ms` (percentil 95) en recálculo combinado hasta 5.000 registros sin parpadeos exige mantener la lógica de segmentación desacoplada sobre índices multidimensionales precalculados en la capa consolidada, evitando consultas en vivo hacia las plataformas externas o recargas completas de vista.

#### 8. Reglas de Negocio y Requisitos Especiales

- **Trazabilidad Funcional Completa (`4 RF` / `4 HU`):**
  - `RF-008` / `HU-033`: Transición contextual directa desde una alerta o anomalía hacia el filtrado detallado conservando los indicadores iniciales para comparación.
  - `RF-009` / `HU-034`: Combinación simultánea multidimensional (`desarrollador`, `módulo financiero`, `etapa oficial`, `rango de fechas`) aplicando intersección exacta sobre todos los indicadores y evidencias.
  - `RF-028` / `HU-036`: Exposición individualizada de cada criterio activo permitiendo su eliminación selectiva para recalcular sin reiniciar la consulta.
  - `RF-039` / `HU-035`: Manejo explícito de cero coincidencias con aviso claro, integridad visual y acción directa para restablecer el estado global.
- **Atributos de Calidad Asociados (`4 RNF`):**
  - `RNF-006`: Apertura de opciones de filtrado en `< 300 ms` preservando contexto visual.
  - `RNF-012`: Adaptabilidad funcional en `1920x1080 px`, `768x1024 px` y `360x640 px`.
  - `RNF-013`: Tráfico protegido en **100 %** con `HTTPS` y `TLS 1.3`.
  - `RNF-018`: Recálculo completo sin parpadeos en `<= 400 ms` (p95) sobre hasta **5.000 registros consolidados**.

---

### CU-06: Auditar trazabilidad de causa raíz

#### 1. Identificador, Nombre y Descripción

- **Identificador y Nombre:** `CU-06` — Auditar trazabilidad de causa raíz.
- **Descripción:** Permite al Jefe de Desarrollo y al PMO consultar el desglose completo de causa raíz de desviaciones, alertas, incidencias críticas o trabajo sin trazabilidad recorriendo los cuatro niveles jerárquicos hasta los commits tipados y las evidencias de calidad sin modificar datos de origen.

#### 2. Actores

- **Actor Primario:** `Jefe de Desarrollo / PMO`.
- **Actores Secundarios:** Ninguno en tiempo de consulta *(consume expedientes consolidados históricos y enlaces referenciales generados por `CU-01` desde `Redmine`, `GLPI`, `GitLab` y `SonarQube`)*.

#### 3. Disparador

El `Jefe de Desarrollo / PMO` selecciona una tarea con más de 5 días hábiles en Certificación, un desarrollador sobre 40 horas, un incidente crítico, una alerta o un registro sin trazabilidad para investigar su causa raíz (`RF-010`).

#### 4. Precondiciones

- El usuario posee **sesión institucional autenticada vigente (`CU-03: Autenticar usuario`)** bajo perfil `Jefe de Desarrollo` o `PMO / Jefatura de Proyectos` (`RF-030` — sin autenticación dentro de este flujo).
- La sesión opera sobre transporte web cifrado (`RNF-013`).
- Existen expedientes cronológicos consolidados por módulo financiero o registros sin trazabilidad almacenados por `CU-01`.

#### 5. Postcondiciones Verificables

- El usuario visualiza en modo exclusivamente de solo lectura la cadena causal íntegra desde la `Épica POA/PETI` y la `Historia/Spec (REQ/INI.TI)` hasta la `Tarea #ID` de Redmine, el ticket `GLPI #ID`, la rama, Merge Request, conteo de commits tipados (`feat`, `fix`, `test`, `refactor`), revisiones de QA cruzado, historial por las 5 etapas, retornos por fallo y alertas de `SonarQube` (`RF-010`, `RF-018`, `RF-020`).
- La jerarquización visual de criticidad permite priorizar el análisis causal y definir acciones correctivas en `< 10 segundos` (`RNF-007`), con respaldo matemático auditable (`RNF-001`) y heredando la anonimización de datos sensibles aplicada en `CU-01` (`RNF-017`).
- No se ejecuta ninguna modificación sobre el estado consolidado ni sobre las plataformas de origen (en coherencia con la política de solo lectura de `CU-01` / `RNF-003`).

#### 6. Flujo Básico ("Camino Feliz")

1. El `Jefe de Desarrollo / PMO` solicita auditar la trazabilidad de causa raíz sobre una alerta, incidente crítico, desarrollador sobrecargado, tarea atrasada o registro sin vínculo (`RF-010`).
2. El sistema recupera del almacenamiento consolidado el expediente cronológico completo correspondiente al elemento y módulo del core financiero (`RF-018`, `RF-020`).
3. El sistema reconstruye y presenta la cadena completa de cuatro niveles desde la Épica POA/PETI e Historia/Spec hasta la `Tarea #ID` en Redmine, el ticket `GLPI #ID` vinculado y las evidencias de `GitLab` (`rama`, `Merge Request`, commits `feat`, `fix`, `test`, `refactor`) (`RF-010`, `RF-014`).
4. El sistema expone la evolución a través de las cinco etapas oficiales, el historial de retornos por fallo, el QA cruzado, los hallazgos de `SonarQube` y los valores de entrada que reproducen al 100 % el indicador de eficiencia (`RF-015`, `RF-017`, `RF-020`, `RNF-001`).
5. El sistema resalta ordenadamente por nivel de criticidad los puntos de bloqueo y facilita los enlaces referenciales directos hacia la plataforma de origen para que la jefatura decida medidas en menos de 10 segundos (`RF-010`, `RNF-007`).
6. El `Jefe de Desarrollo / PMO` concluye la auditoría causal con evidencia verificable para tomar decisiones o proceder a gestionar la alerta relacionada (`CU-07`).

#### 7. Flujos Alternos vs. Flujos de Excepción

- **Flujos Alternos (logran el objetivo de diagnóstico causal aun ante brechas de origen):**
  
  - **2a (Auditoría sobre trabajo en código o incidente GLPI sin trazabilidad completa):** Cuando el elemento seleccionado corresponde a «Trabajo en código sin trazabilidad planificada» (`RF-021`) o a «Incidente de soporte sin trazabilidad en desarrollo» (`RF-022`), el sistema despliega el expediente parcial recuperado indicando con claridad el eslabón faltante (`sin tarea en Redmine` o `sin petición espejo GLPI #ID`), mostrando igualmente el autor o responsable, repositorio y esfuerzo consumido, y se reincorpora al paso 5 para priorizar acción correctiva.
  - **3a (Auditoría de tarea con brecha de evidencia o divergencia de autor):** Si existe diferencia entre el porcentaje reportado en Redmine y el respaldo real en código (`RF-038`), o si el autor de los commits no coincide con el responsable asignado (`RF-017`), el sistema exhibe explícitamente ambas fuentes lado a lado señalando la inconsistencia y continúa en el paso 4.

- **Flujos de Excepción (terminan en fracaso `(X)` sin reconstruir expediente confiable):**
  
  - **2b (Expediente inexistente o fuera del período histórico retenido) `(X)`:** Si el identificador consultado fue depurado por superar la ventana de retención técnica (`> 24 meses`) o no corresponde a ningún registro consolidado válido, el sistema notifica claramente la ausencia del expediente y **termina sin cumplir la postcondición de auditoría (`X`)**.
  - **4a (Fallo o corrupción en el rastro auditable inmutable) `(X)`:** Si durante la lectura ocurre un error interno o falta el respaldo histórico de fórmula/entradas que impide reproducir con **100 %** de exactitud la trazabilidad o el cálculo (`RNF-001`), el sistema interrumpe la entrega sin mostrar datos incompletos ni exponer información sin enmascarar (`RNF-017`) y **termina en fracaso (`X`)**.
    - **Insight Arquitectónico → TRAZABILIDAD INMUTABLE Y SEPARACIÓN ANALÍTICA (`RNF-001`, `RNF-007`, `RNF-017`):** Conservar registros auditables inmutables durante al menos 12–24 meses (desprovistos al 100 % de PII de socios desde `CU-01`) desacoplados del tablero sintético permite explicar con exactitud cualquier cifra directivamente cuestionada y ordenar visualmente su gravedad en `< 10 s` sin consultar ni afectar las herramientas de los desarrolladores.

#### 8. Reglas de Negocio y Requisitos Especiales

- **Trazabilidad Funcional Directa (`1 RF` / `1 HU`):**
  - `RF-010` / `HU-037`: Presentación obligatoria de causa raíz en lenguaje claro y directo para el Jefe de Desarrollo y PMO con enlace hacia `Épica POA/PETI`, `Historia/Spec`, `Tarea #ID` en Redmine, ticket `GLPI` y `rama/commits` en GitLab sobre todo incidente crítico, atraso `> 5 días` en Certificación, sobrecarga `> 40 h` o actividad sin trazabilidad. *(Consume la integridad relacional consolidada en `CU-01` bajo `RF-014`, `RF-015`, `RF-017`, `RF-018`, `RF-020`, `RF-021`, `RF-022` y `RF-038`.)*
- **Atributos de Calidad Asociados (`4 RNF`):**
  - `RNF-001` (Responsabilidad): Registro auditable inmutable mínimo **12 meses** que garantice **100 %** de reproducibilidad matemática de eficiencia y avance.
  - `RNF-007` (Operabilidad): Ordenamiento y diferenciación visual de alertas por criticidad para priorizar y decidir en `< 10 segundos`.
  - `RNF-012` (Adaptabilidad): Visualización completa adaptada a `1920x1080 px`, `768x1024 px` y `360x640 px`.
  - `RNF-013` (Seguridad): Transmisión cifrada al **100 %** mediante `HTTPS` con `TLS 1.3`.

---

## 4. Casos de Uso Complementarios y Matriz Global de Cobertura (100 %)

### 4.1 Fichas Resumidas de Casos Complementarios (`CU-03`, `CU-07`, `CU-08`)

Para preservar la trazabilidad completa de todos los requisitos depurados en la Fase 1 y Fase 2 sin introducir burocracia de especificación en casos no seleccionados por el Filtro Arquitectónico, se resumen sus contratos verificables:

| Caso de Uso                            | Actor Primario             | Requisitos Trazados (`RF` / `HU` / `RNF`)                                                        | Precondición, Disparador y Comportamiento Central Verificable                                                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------------------------------- |:-------------------------- |:------------------------------------------------------------------------------------------------ |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CU-03: Autenticar usuario**          | `Jefe de Desarrollo / PMO` | `RF-030` (`HU-001`) · `RNF-011`, `RNF-012`, `RNF-013`                                            | Valida credenciales institucionales sobre canal cifrado (`TLS 1.3`, `RNF-013`) entregando a `Jefe de Desarrollo` y `PMO / Jefatura de Proyectos` **exactamente la misma vista web unificada en modo solo lectura**. Los desarrolladores no tienen cuentas ni ingreso a la plataforma. Su resultado autenticado constituye precondición de `CU-04`..`CU-08`. Al cierre libera recursos en `< 200 ms` (`RNF-011`).                                                     |
| **CU-07: Gestionar alerta operativa**  | `Jefe de Desarrollo / PMO` | `RF-026` (`HU-038`), `RF-027` (`HU-040`) · `RNF-004`, `RNF-007`, `RNF-011`, `RNF-012`, `RNF-013` | *Precondición:* sesión autenticada (`CU-03`). Permite marcar cualquier alerta operativa priorizada (`RNF-007`) como «atendida» o «en seguimiento», deteniendo nuevos recordatorios externos y persistiendo de inmediato quién y cuándo la gestionó (`RF-026`, `RNF-004`). Al cierre o expiración de sesión confirma la consolidación íntegra de toda la bitácora gestionada sin dejar operaciones pendientes (`RF-027`) y libera el canal en `< 200 ms` (`RNF-011`). |
| **CU-08: Exportar reporte trimestral** | `Jefe de Desarrollo / PMO` | `RF-040` (`HU-039`) · `RNF-001`, `RNF-002`, `RNF-012`, `RNF-013`                                 | *Precondición:* sesión autenticada (`CU-03`). Genera y exporta en lenguaje ejecutivo (`RNF-002`) y con exactitud auditable (`RNF-001`) el informe del trimestre seleccionado con el total de proyectos `POA/PETI`, requerimientos `REQ`, iniciativas `INI.TI` e incidentes `GLPI` atendidos, el porcentaje global de cumplimiento y el comparativo de certificaciones cerradas a tiempo frente a las atrasadas (`> 5 días hábiles`) (`RF-040`).                      |

---

### 4.2 Verificación de Cobertura Completa (`40 RF` + `40 HU` + `18 RNF` = `100 %`)

- **Requisitos Funcionales cubiertos (`40/40`):**
  - `CU-01`: `RF-001`, `RF-002`, `RF-003`, `RF-004`, `RF-005`, `RF-011`, `RF-012`, `RF-013`, `RF-014`, `RF-015`, `RF-016`, `RF-017`, `RF-018`, `RF-019`, `RF-020`, `RF-021`, `RF-022`, `RF-029`, `RF-033`, `RF-037`, `RF-038` *(21)*.
  - `CU-02`: `RF-006`, `RF-023`, `RF-024`, `RF-025`, `RF-031` *(5)*.
  - `CU-03`: `RF-030` *(1)*.
  - `CU-04`: `RF-007`, `RF-032`, `RF-034`, `RF-035`, `RF-036` *(5)*.
  - `CU-05`: `RF-008`, `RF-009`, `RF-028`, `RF-039` *(4)*.
  - `CU-06`: `RF-010` *(1)*.
  - `CU-07`: `RF-026`, `RF-027` *(2)*.
  - `CU-08`: `RF-040` *(1)*.
- **Historias de Usuario cubiertas (`40/40`, relación `1:1` con cada RF):** `HU-001` a `HU-040`.
- **Requisitos No Funcionales cubiertos (`18/18` bajo ISO/IEC 25010:2023):**
  - `RNF-001` (`CU-01`, `CU-06`, `CU-08`) · `RNF-002` (`CU-04`, `CU-08`) · `RNF-003` (`CU-01`) · `RNF-004` (`CU-02`, `CU-07`) · `RNF-005` (`CU-04`) · `RNF-006` (`CU-05`) · `RNF-007` (`CU-06`, `CU-07`) · `RNF-008` (`CU-01`, `CU-04`) · `RNF-009` (`CU-01`) · `RNF-010` (`CU-02`) · `RNF-011` (`CU-03`, `CU-04`, `CU-07`) · `RNF-012` (`CU-03`, `CU-04`, `CU-05`, `CU-06`, `CU-07`, `CU-08`) · `RNF-013` (`CU-03`, `CU-04`, `CU-05`, `CU-06`, `CU-07`, `CU-08`) · `RNF-014` (`CU-04`) · `RNF-015` (`CU-02`) · `RNF-016` (`CU-01`, `CU-02`) · `RNF-017` (`CU-01`) · `RNF-018` (`CU-05`).
