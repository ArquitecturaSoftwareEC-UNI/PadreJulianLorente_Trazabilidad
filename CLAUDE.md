@AGENTS.md

# CLAUDE.md — Guía Operativa para Claude Code

Este repositorio sigue todas las instrucciones normativas, restricciones de alcance e inventarios definidos en `@AGENTS.md`.

## Resumen Operativo Rápido

1. **Reglas obligatorias del curso (`DSOF_4069`):** Lee primero [reglas-vista-logica-y-procesos.md](docs/referencias/reglas-vista-logica-y-procesos.md) (destilado de [Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf](docs/referencias/Guia_Maestra_Arquitectura_de_Software_Semanas_1_a_6.pdf)).
2. **Handoff por vista:**
   - Para construir la **Vista Lógica (Semana 4)**: ejecuta la guía y prompt en [prompt-vista-logica.md](docs/handoff/prompt-vista-logica.md).
   - Para construir la **Vista de Procesos (Semana 5)** *(solo tras cerrar Vista Lógica)*: ejecuta la guía y prompt en [prompt-vista-procesos.md](docs/handoff/prompt-vista-procesos.md).
   - Para la futura **Fase de Implementación / Código**: consulta [skills-fase-codigo.md](docs/handoff/skills-fase-codigo.md) *(PROHIBIDO instalar esas skills durante las Semanas 4 y 5)*.
3. **Protección estricta de entregables cerrados:** Nunca edites ni sobrescribas los archivos Excel en `docs/`, `docs/uml/casos-de-uso.puml`, `docs/especificaciones-casos-de-uso.md`, `docs/uml/cadena-de-valor-togaf-adm.puml`, `docs/cadena-de-valor-togaf-adm.png` ni los PNG históricos de referencia.
4. **Validación PlantUML obligatoria:**
   - Verificar entorno: `python .agents/skills/plantuml/scripts/check_setup.py`
   - Validar sintaxis y exportar PNG: `python .agents/skills/plantuml/scripts/convert_puml.py <ruta.puml> --format png --output-dir docs/uml/`
   - Inspeccionar visualmente cada `.png` generado antes de reportar éxito.
5. **No instalar paquetes Node durante diseño:** No ejecutes `npx` ni `npm` mientras trabajes en Vista Lógica o Vista de Procesos; conserva intacto `skills-lock.json` y `.agents/skills/plantuml/`.
