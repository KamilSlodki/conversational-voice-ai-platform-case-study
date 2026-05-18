# Protocolo de documentación

Este proyecto se desarrolló bajo el protocolo de documentación propio del autor, replicado cross-proyecto en 13+ proyectos del ecosistema. **La documentación es activo de primera clase, no afterthought.**

> Este case-study es **anonimizado**. Los nombres del cliente BPO, de los sub-clientes B2C verticales, del stack comercial específico de telefonía heredada, las IPs de infraestructura y los volúmenes absolutos del negocio del cliente se omiten. El protocolo descrito es el mismo aplicado al proyecto real entregado bajo acuerdo de confidencialidad.

---

## 5 capas documentales

### 1. arc42 — Marco de arquitectura del software

Marco estandarizado de 12 secciones (introducción + objetivos · restricciones · contexto · estrategia de solución · vista de bloques · vista de runtime · vista de despliegue · conceptos cross-cutting · decisiones arquitectónicas · requisitos de calidad · riesgos · glosario).

**En este repo (versión pública anonimizada):**
- Resumen sistémico del producto, contexto comercial y restricciones en el README (secciones *Sobre este case-study* + *El problema*).
- Estrategia de solución y vista de bloques en *La solución arquitectónica*.
- Vista de runtime detallada en *Workflows principales* (configuración <2 min + llamada real del lead).
- Vista de despliegue resumida en *Escala y operación*.
- Resto vive en el entregable original bajo NDA.

### 2. C4 — Modelo visual de arquitectura

Notación Simon Brown: Context · Container · Component · Code. Diagramas embebidos como **Mermaid** para render nativo en GitHub.

**En este repo:**
- [`assets/diagrams/c4-context.mmd`](../assets/diagrams/c4-context.mmd) — diagrama Context (3 actores + plataforma + 7 sistemas externos).
- [`assets/diagrams/c4-container.mmd`](../assets/diagrams/c4-container.mmd) — diagrama Container (6 tiers internos + sistemas externos · 24 containers).
- Component + Code permanecen en repo privado por superficie técnica sensible (lógica de templates, integraciones específicas con la PBX heredada).

### 3. ADR — Architecture Decision Records

Cada decisión estructural del proyecto real se documentó como ADR versionada con plantilla fija:

```
Contexto      → qué problema / qué tensión
Decisión      → qué se eligió
Alternativas  → qué más se evaluó y por qué se descartó
Consecuencias → trade-offs aceptados (positivos y negativos)
Plan rollout  → cómo se materializa
Plan rollback → cómo se revierte si rompe algo
```

**32 ADRs versionadas** en el entregable original (numeración `ADR-0001..0014` Fase 1 + serie `ADR-S2-XX` Fase 1 final + serie `ADR-F2-XX` Fase 2). Permanecen bajo NDA. Este case-study cita en el README **5 ADRs representativas** anonimizadas que demuestran criterio técnico (concurrencia transaccional · hardening de secrets · quota management de terceros · prompt rendering policy · SIP BYOC architecture).

### 4. Runbooks operativos

Procedimientos paso a paso para incidentes recurrentes y mantenimiento programado del entregable original, entregados al cliente bajo NDA:

- Cleanup idempotente de demos expiradas + sentinel `demos.activa = 0`.
- Alerta proactiva cuando trunks disponibles ≤ 4 por tipo (mail + dashboard).
- Verificación de cuota mensual ElevenLabs (alerta al 70 % del plan).
- Procedimiento de retry en `external_resource_retries` con backoff.
- Rotación de credenciales (API keys · webhook Basic Auth · Service Accounts).
- Procedimiento de coexistencia Fase 1 / Fase 2 (no tocar trunks de F1 durante deploys de F2).

Cada uno con cabecera fija: disparadores · precondiciones · severidad · owner · pasos numerados · verificación final.

### 5. Postmortems formales

Cada incidente productivo cierra con postmortem versionado: cronología · causa raíz (5 whys) · acción correctiva (que escala a ADR si aplica) · métrica observable para prevenir recurrencia. Los aprendizajes transferibles (no específicos del cliente) alimentan el `engineering-playbook` cross-proyecto del autor.

**Patrones de retos técnicos resueltos** documentados como case-study generalizable en la sección "Retos técnicos resueltos" del README (6 retos con estructura Síntoma / Causa raíz / Fix / Lección).

---

## Disciplina cross-proyecto

- **Engineering-playbook propio** con 60+ archivos doctrinales cross-proyecto, evolucionados iterativamente desde incidentes reales (no copy-paste de buenas prácticas genéricas).
- **`verify-findings` adversarial**: cualquier auditoría / hallazgo pasa por un segundo agente independiente antes de cerrar. Ratio de ruido objetivo < 15 %.
- **Convenciones**: español, sin emojis en archivos de trabajo, tono operativo y directo, "lo que no aporta evidencia, fuera".
- **Contratos formales prompt-tool-payload** (C1/C2/C3): completitud + coherencia + sin huérfanos. Bloqueante en merge.
- **Sistema de branching para coexistencia de generaciones**: `kamil_v2` + sub-ramas `f2/*` que protegen producción Fase 1 durante desarrollo paralelo Fase 2.

---

*Detalles operativos completos del playbook y del entregable original bajo acuerdo de confidencialidad.*
