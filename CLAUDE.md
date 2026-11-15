@AGENTS.md

# CLAUDE.md — Guía Operativa para Claude Code

Este repositorio sigue todas las instrucciones normativas, línea base, invariantes de dominio y restricciones de no-regresión definidos en `@AGENTS.md`.

## Resumen Operativo Rápido

1. **Fuente de verdad cerrada de Vista +1:** Consulta [especificaciones-casos-de-uso.md](docs/especificaciones-casos-de-uso.md), [casos-de-uso.puml](docs/uml/casos-de-uso.puml) y los libros Excel en `docs/` (`CU-01`..`CU-08`, `40 RF`, `40 HU / 225 pts`, `18 RNF` ISO/IEC 25010:2023).
2. **Reglas metodológicas (`DSOF_4069`):** Lee primero [reglas-vista-logica-y-procesos.md](docs/referencias/reglas-vista-logica-y-procesos.md) (destilado de [Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf](docs/referencias/Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf)).
3. **Handoff por vista:**
   - Para construir la **Vista Lógica (Semana 4)**: ejecuta la guía y prompt en [prompt-vista-logica.md](docs/handoff/prompt-vista-logica.md).
   - Para construir la **Vista de Procesos (Semana 5)** *(solo tras cerrar Vista Lógica)*: ejecuta la guía y prompt en [prompt-vista-procesos.md](docs/handoff/prompt-vista-procesos.md).
   - Para la futura **Fase de Implementación / Código**: consulta [skills-fase-codigo.md](docs/handoff/skills-fase-codigo.md) *(PROHIBIDO instalar esas skills durante las Semanas 4 y 5)*.
4. **Protección estricta de entregables cerrados:** Nunca edites ni sobrescribas los archivos Excel en `docs/`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png` ni los PNG históricos de referencia.
5. **Validación PlantUML obligatoria:**
   - Verificar entorno: `python .agents/skills/plantuml/scripts/check_setup.py`
   - Validar sintaxis y exportar PNG: `python .agents/skills/plantuml/scripts/convert_puml.py <ruta.puml> --format png --output-dir docs/uml/`
   - Inspeccionar visualmente cada `.png` generado antes de reportar éxito.
6. **No instalar paquetes Node durante diseño:** No ejecutes `npx` ni `npm` mientras trabajes en Vista Lógica o Vista de Procesos; conserva intacto `skills-lock.json` y `.agents/skills/plantuml/`.
