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

### 2.1 Backend & Integración ETL (NestJS)
Buenas prácticas de arquitectura modular, inyección de dependencias, controladores, servicios y pruebas en NestJS:
```bash
npx skills add kadajett/agent-nestjs-skills --skill nestjs-best-practices
```

### 2.2 Base de Datos Relacional (Supabase / PostgreSQL)
Diseño de esquema físico (derivado limpiamente desde el Diagrama de Clases cerrado), índices, consultas e integridad transaccional en PostgreSQL:
```bash
npx skills add supabase/agent-skills --skill supabase-postgres-best-practices
```

### 2.3 Frontend del Tablero Ejecutivo (React / Next.js)
Patrones de rendimiento, estado, renderizado y composición de componentes React para implementar el Tablero de Control (`CU-04`, `CU-05`, `CU-06`, `CU-07`, `CU-08`):
```bash
npx skills add vercel-labs/agent-skills --skill vercel-react-best-practices
```

### 2.4 Pruebas Unitarias y Desarrollo Guiado por Pruebas (TDD)
Disciplina Rojo-Verde-Refactor para validar fórmulas de carga (`RF-018`, `RF-021`), avance (`RF-023`, `RF-024`), deduplicación de alertas (`RF-032`) y detección de inconsistencias (`RF-012`, `RF-013`, `RF-016`):
```bash
npx skills add obra/superpowers --skill test-driven-development
```

### 2.5 Seguridad de Aplicaciones (OWASP)
Endurecimiento de credenciales de conectores (`CU-03`), manejo seguro de webhooks, control de acceso y prevención de vulnerabilidades OWASP Top 10:
```bash
npx skills add github/awesome-copilot --skill sec-owasp-security
```

### 2.6 Pruebas End-to-End (Playwright)
Automatización de flujos completos sobre el tablero web, filtros interactivos, exportación y auditoría de causa raíz:
```bash
npx skills add currents-dev/playwright-best-practices-skill --skill playwright-best-practices
```

---

## 3. Referencia Metodológica: Spec-Driven Development (`github/spec-kit`)

Para transformar nuestra arquitectura documentada en código trazable sin improvisación, el equipo puede apoyarse en el flujo de **Spec-Driven Development (SDD)** de GitHub Spec Kit:
- **Repositorio oficial:** https://github.com/github/spec-kit

### Cómo conectar Spec Kit con nuestros artefactos ya cerrados
| Fase de Spec Kit | Insumo oficial en este repositorio | Propósito al entrar a código |
|---|---|---|
| **1. Constitution (`/speckit.constitution`)** | `AGENTS.md` + `docs/Requisitos-padre-julian-lorente.xlsx` (`18 RNF` ISO 25010:2023) | Fijar principios arquitectónicos, restricciones de rendimiento (`<= 5 s`, `<= 2 s`, `<= 500 ms`), resiliencia parcial y estándares de seguridad/calidad |
| **2. Specify (`/speckit.specify`)** | `docs/especificaciones-casos-de-uso.md` + `docs/historias-usuario-padre-julian-lorente.xlsx` (`40 RF / 40 HU`) | Derivar especificaciones funcionales y criterios de aceptación verificables sin reescribir las reglas de negocio ya aprobadas |
| **3. Plan (`/speckit.plan`)** | `docs/vista-logica-clases.md` + `docs/vista-procesos-ecb-secuencia.md` + `docs/uml/*.puml` | Elegir el stack concreto (NestJS, PostgreSQL/Supabase, React) mapeando clases de dominio y flujos ECB hacia módulos de la Vista de Desarrollo |
| **4. Tasks & Implement (`/speckit.tasks` / `/speckit.implement`)** | Plan técnico + `docs/handoff/skills-fase-codigo.md` | Descomponer tareas atómicas aplicables bajo TDD, seguridad OWASP y pruebas Playwright E2E |
