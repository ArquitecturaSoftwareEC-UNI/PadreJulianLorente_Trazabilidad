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
1. `AGENTS.md` (contexto del dominio, línea base de 40 RF / 40 HU / 18 RNF, invariantes de negocio y archivos protegidos).
2. `docs/referencias/reglas-vista-logica-y-procesos.md` (especialmente la Sección 1 — Semana 4 Vista Lógica y Sección 3 — DAS v3.0).
3. `docs/especificaciones-casos-de-uso.md` (las 5 especificaciones Cockburn `CU-01`, `CU-02`, `CU-04`, `CU-05`, `CU-06` y las 3 fichas complementarias `CU-03`, `CU-07`, `CU-08`).
4. `docs/uml/casos-de-uso.puml`.
5. Guías de sintaxis PlantUML en `.agents/skills/plantuml/references/class_diagrams.md`, `.agents/skills/plantuml/references/state_diagrams.md` y `.agents/skills/plantuml/references/common_syntax_errors.md`.

## 2. CONTROL ESTRICTO DE ALCANCE Y NO-REGRESIÓN
- **PROHIBIDO** modificar, editar o sobrescribir los entregables cerrados de fases anteriores (`docs/*.xlsx`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png`, ni los diagramas PNG previos en `docs/`).
- **PROHIBIDO** generar todavía la Vista de Procesos (ECB/Secuencia), la Vista de Desarrollo o código fuente.
- **PROHIBIDO** ejecutar `npx`, `npm` o instalar nuevas skills; usa únicamente `.agents/skills/plantuml`.
- **PROHIBIDO CONTAMINAR LA VISTA LÓGICA CON TECNOLOGÍA FÍSICA O UI:**
  - Cero tablas SQL, tipos de motor (`VARCHAR`, `INT8`, `JSONB`) o claves foráneas (`FK`, ej. `idDesarrollador`, `idTicket`). Usa tipos abstractos (`Texto`, `Entero`, `Decimal`, `Booleano`, `FechaHora`, `Duracion`) y relaciones UML con multiplicidades.
  - Cero frameworks (`NestJS`, `React`, `Prisma`), infraestructura (`Docker`, `Redis`) o componentes visuales (`PantallaTablero`, `BotonExportar`, `ModalFiltro`).

## 3. ENTREGABLES EXACTOS A CONSTRUIR

### Entregable A: `docs/vista-logica-clases.md`
Debe incluir obligatoriamente cuatro secciones rigurosas conforme a la Semana 4:
1. **Decodificación Gramatical y Árbol de 4 Filtros Secuenciales:**
   - Sustantivos y frases nominales extraídos de `CU-01` a `CU-08`, verbos candidatos a operaciones/relaciones, adjetivos/estados candidatos a atributos o `«enumeration»`, y cantidades convertidas a multiplicidades.
   - Tabla de **descartes inmediatos** (sinónimos, actores externos sin historia interna gestionada, UI/reportes, operaciones disfrazadas de sustantivos) y evaluación paso a paso contra los **4 filtros secuenciales**: (1) Persistencia conceptual a largo plazo, (2) Complejidad >1 atributo, (3) Identidad propia y ciclo de vida independiente, (4) Participación comprobada en al menos un Caso de Uso.
2. **Validación con Tarjetas CRC y Decisiones de Diseño:**
   - Tarjetas CRC (**Clase – Responsabilidad – Colaboración**) para todas las clases aprobadas, auditando **Alta Cohesión** (sin *Clases Dios*) y **Bajo Acoplamiento**.
   - Justificación explícita de **Roles por Asociación vs. Herencia Rígida** donde aplique (ej. desarrolladores/responsables/líderes o tipos de artefactos) y descomposición de relaciones N:M con atributos propios en clases asociativas.
3. **Diccionario de Clases Completo (5 Campos Obligatorios por Cada Clase):**
   Para **cada clase** del diagrama, documenta exactamente los 5 campos exigidos por la Guía Maestra:
   1. **Nombre y Propósito**
   2. **Atributos y Restricciones** (visibilidad `-`, tipo abstracto, obligatoriedad, valor por defecto/rango; marca con `/` cualquier atributo derivado indispensable como `/indiceCargaPonderada` o `/porcentajeAvanceReal`)
   3. **Operaciones (Entrada / Salida)** (firmas públicas `+` completas con tipos abstractos; **advertencia:** la Vista de Procesos en Semana 5 dependerá 1:1 de estas firmas exactas)
   4. **Invariantes / Reglas de Negocio** (trazando fórmulas y umbrales exactos: timeout `<= 5 s` y preservación de snapshot parcial, ponderación `Alta=3 / Media=2 / Baja=1`, sobrecarga `> 8` ítems o `> 40` horas, desviación `> 20%`, las 3 inconsistencias `RF-012 / RF-013 / RF-016`, estancamiento Certificación `> 5` días hábiles `RF-028`, deduplicación `24 h` `RF-032`, fallback SMTP `RF-030`)
   5. **Casos de Uso Trazados** (`CU-01`..`CU-08` y `RF-001`..`RF-040` asociados)
4. **Matriz de Trazabilidad Escenarios vs. Clases:**
   - Matriz cruzando `CU-01` a `CU-08` (filas) contra todas las clases y enumeraciones del dominio (columnas), verificando cero filas ni columnas vacías.

### Entregable B: `docs/uml/diagrama-clases.puml` y `docs/uml/diagrama-clases.png`
- Diagrama de Clases UML 2.5.1 en **Nivel de Especificación**, aplicando la receta de 5 pasos:
  - Clases núcleo más conectadas al centro y paquetes lógicos claros si mejora la legibilidad.
  - Atributos privados (`-`) con tipos abstractos y derivados marcados con `/`.
  - Operaciones públicas (`+`) con parámetros y tipo de retorno consistentes con el Diccionario de Clases.
  - Uso preciso de las **6 relaciones formales** (Asociación etiquetada con verbo y roles, Agregación `o--`, Composición `*--`, Generalización `--|>` solo si es permanente, Dependencia `..>` ej. `«use»` hacia enumeraciones, y Realización `..|>` si aplica interfaz de dominio).
  - Multiplicidades obligatorias en **ambos extremos** (`1`, `0..1`, `*`, `1..*`, etc.) y bloques `«enumeration»` para estados, prioridades, tipos de plataforma, canales e inconsistencias.

### Entregable C: `docs/uml/diagrama-estados.puml` y `docs/uml/diagrama-estados.png`
- Diagrama de Estados UML 2.5.1 derivado de los flujos básicos y alternos de `docs/especificaciones-casos-de-uso.md` para las entidades con ciclo de vida complejo crítico (como `CicloSincronizacion` — incluyendo estado de sincronización parcial —, `AlertaOperativa` — incluyendo deduplicación, reintento webhook, fallback SMTP y falla crítica — y/o `ItemTrabajo` trazable).

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
- [ ] No existen claves foráneas (`id_...`), tipos SQL (`VARCHAR`, `INT8`) ni nombres de UI/frameworks en `docs/uml/diagrama-clases.puml` ni en `docs/vista-logica-clases.md`.
- [ ] Cada clase de `diagrama-clases.puml` aparece documentada con sus 5 campos obligatorios en `docs/vista-logica-clases.md` y tiene al menos un `CU-XX` en la matriz de trazabilidad.
- [ ] Ambos diagramas `.png` renderizaron sin errores de sintaxis PlantUML y fueron inspeccionados visualmente para confirmar legibilidad limpia.
