# Guía de Skills y Herramientas para la Fase de Desarrollo e Implementación (Semana 6+)

> [!CAUTION]
> **NO INSTALAR NI EJECUTAR ESTOS COMANDOS TODAVÍA.**
> Mientras el equipo esté elaborando la **Vista Lógica (Semana 4)** y la **Vista de Procesos (Semana 5)**, está **estrictamente prohibido** ejecutar `npx`, `npm` o instalar estas skills. Hacerlo contaminaría el contexto de los agentes de diseño con patrones de frameworks, SQL físico y detalles de interfaz gráfica que violan las exclusiones de las Semanas 4 y 5.
> Conserva únicamente `.agents/skills/plantuml` durante toda la etapa de arquitectura y diseño.

---

## 1. Cuándo Desbloquear Esta Guía

Habilita estas skills **únicamente cuando se cumplan las tres condiciones siguientes**:
1. La **Vista Lógica (`docs/vista-logica-clases.md`, `docs/uml/diagrama-clases.puml` y `docs/uml/diagrama-estados.puml`)** esté aprobada y cerrada.
2. La **Vista de Procesos (`docs/vista-procesos-ecb-secuencia.md` y los 5 `docs/uml/secuencia-*.puml`)** esté aprobada y verificada 1:1 contra la Vista Lógica.
3. El equipo inicie formalmente la **Vista de Desarrollo (Semana 6)** o la **implementación de código fuente**.

---

## 2. Catálogo de Skills por Capa Tecnológica y Disciplina

Cuando llegue el momento de implementar, ejecuta los comandos correspondientes al rol de cada desarrollador o rama:

### 2.1 Backend e Ingesta ETL (NestJS)
Buenas prácticas de arquitectura modular, inyección de dependencias, controladores, servicios y pruebas en NestJS:
```bash
npx skills add Kadajett/agent-nestjs-skills
```

### 2.2 Base de Datos Relacional (Supabase / PostgreSQL)
Diseño de esquema físico (derivado limpiamente desde el Diagrama de Clases cerrado), índices multidimensionales, retención 12–24 meses e idempotencia con llaves compuestas:
```bash
npx skills add supabase/agent-skills --skill supabase-postgres-best-practices
```

### 2.3 Frontend del Tablero Ejecutivo (React / Next.js)
Patrones de rendimiento, estado, actualización incremental post-sincronización y composición de componentes React para implementar el Tablero Único (`CU-03`, `CU-04`, `CU-05`, `CU-06`, `CU-07`, `CU-08`):
```bash
npx skills add vercel-labs/agent-skills --skill vercel-react-best-practices
```

### 2.4 Pruebas Unitarias y Desarrollo Guiado por Pruebas (TDD)
Disciplina Rojo-Verde-Refactor para validar con 100 % de exactitud matemática las reglas del dominio (`RF-001`..`RF-005` Fibonacci `2/5/13`, base `40 h` con `GLPI` `4h/8h` y QA `2h`, eficiencia `70 %/30 %` con `-10`/`-5` pts; `RF-013`..`RF-022` cruce tolerante `GLPI #ID`, retornos y trabajo sin trazabilidad; `RF-038` respaldo en código `> 25 %` y `100 %`; `RF-023`..`RF-028` supresión anti-spam 24 h y filtros; y compuerta LOPDP 2021 `RNF-017`):
```bash
npx skills add obra/superpowers --skill test-driven-development
```

### 2.5 Seguridad de Aplicaciones (OWASP)
Endurecimiento de credenciales de solo lectura (`RNF-003`), sesiones institucionales (`CU-03`), protección `HTTPS TLS 1.3` (`RNF-013`) y canales salientes `>= TLS 1.2` (`RNF-015`), enmascaramiento LOPDP 2021 (`RNF-017`) y controles OWASP Top 10:
```bash
npx skills add agamm/claude-code-owasp
```

### 2.6 Pruebas End-to-End (Playwright)
Automatización de flujos y presupuestos de desempeño sobre el tablero web: carga inicial `<= 2 s` p95 y refresco `< 1 s` (`RNF-014`), apertura de filtros `< 300 ms` (`RNF-006`), recálculo multidimensional `<= 400 ms` p95 (`RNF-018`), identificación visual en máximo 5 segundos (`RNF-005`), auditoría causal en `< 10 s` (`RNF-007`), liberación al cierre en `< 200 ms` (`RNF-011`) y adaptabilidad en `1920x1080 px`, `768x1024 px` y `360x640 px` (`RNF-012`):
```bash
npx skills add currents-dev/playwright-best-practices-skill
```

---

## 3. Referencia Metodológica: Spec-Driven Development (`github/spec-kit`)

Para transformar nuestra arquitectura documentada en código trazable sin improvisación, el equipo puede apoyarse en el flujo de **Spec-Driven Development (SDD)** de GitHub Spec Kit:
- **Repositorio oficial:** https://github.com/github/spec-kit

### Cómo conectar Spec Kit con nuestros artefactos ya cerrados
| Fase de Spec Kit | Insumo oficial en este repositorio | Propósito al entrar a código |
|---|---|---|
| **1. Constitution (`/speckit.constitution`)** | `AGENTS.md` + `docs/Requisitos-padre-julian-lorente.xlsx` (`18 RNF` ISO 25010:2023) | Fijar principios arquitectónicos, solo lectura `RNF-003`, LOPDP 2021 `RNF-017`, tolerancia a fallos con timeout externo `<= 10 s` `RNF-008`, idempotencia `RNF-009` y presupuestos reales (`< 200 ms`, `< 300 ms`, `<= 400 ms` p95, `< 1 s`, `<= 2 s` p95, `< 10 s`) |
| **2. Specify (`/speckit.specify`)** | `docs/especificaciones-casos-de-uso.md` + `docs/historias-usuario-padre-julian-lorente.xlsx` (`40 RF / 40 HU`) | Derivar especificaciones funcionales (`CU-01`..`CU-08`) y criterios de aceptación Gherkin verificables sin reescribir las reglas de negocio ya aprobadas |
| **3. Plan (`/speckit.plan`)** | `docs/vista-logica-clases.md` + `docs/vista-procesos-ecb-secuencia.md` + `docs/uml/*.puml` | Elegir el stack concreto mapeando entidades del dominio y participantes ECB hacia módulos de la Vista de Desarrollo |
| **4. Tasks & Implement (`/speckit.tasks` / `/speckit.implement`)** | Plan técnico + `docs/handoff/skills-fase-codigo.md` | Descomponer tareas atómicas aplicables bajo TDD, OWASP y pruebas Playwright E2E |
