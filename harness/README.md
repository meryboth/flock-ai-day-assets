# Harness de agentes

Un "harness" es la capa de control alrededor de un LLM que lo convierte en un agente: el loop que decide cuándo llamar una herramienta, cómo le devuelve el resultado al modelo, cómo administra el contexto y la memoria entre pasos, y cómo lo mantiene corriendo de forma sostenida — no solo una respuesta suelta. Claude Code es un harness; también lo es cualquier bot propio que arme el equipo. Entender cómo está hecho uno real ayuda mucho más que leerlo en abstracto.

## Las piezas de un harness

Un harness, en el fondo, resuelve siempre las mismas cuatro cosas:

1. **El loop de tool-use** — el ciclo "el modelo decide qué tool llamar → se ejecuta → el resultado vuelve al modelo → repetir hasta que decide que terminó". Es la implementación práctica del patrón ReAct (ver `../papers/`).
2. **Gestión de contexto** — qué entra y qué sale de la ventana de contexto en cada vuelta del loop. Un harness bien hecho no mete todo el historial crudo en cada llamada: comprime, resume o descarta lo que ya no es relevante (ver `../memoria-de-agentes/` para arquitecturas de memoria de más largo plazo).
3. **Capa de permisos** — qué tools puede ejecutar el agente solo (`always_allow`) y cuáles necesitan confirmación humana antes de correr (`always_ask`). Esto es seguridad aplicada, no un detalle de implementación — ver `../seguridad-agentes/`.
4. **Sesiones como infraestructura** — un harness de verdad no es un script que corre una vez; sostiene sesiones largas, se puede pausar/retomar, y en el caso de agentes más sofisticados, puede disparar sub-agentes aislados para partes de la tarea (ver `../coordinacion-multiagente/`).

## Un caso real para estudiar: Hermes Agent

En vez de quedarse en la teoría, el mejor ejercicio es leer cómo resuelve estas cuatro piezas un harness open source real. **Hermes Agent**, de Nous Research, es un buen caso: trata las sesiones como infraestructura (no solo un loop efímero), separa el *registro* de una tool de su *exposición* al modelo (no todas las tools registradas están siempre visibles, lo que ahorra contexto), implementa compresión de contexto basada en "lineage" (qué parte de la conversación realmente importa para la siguiente decisión), y soporta sub-agentes aislados para tareas que no deberían compartir contexto entre sí.

- **[Hermes Agent Fundamentals In 29 Minutes](https://www.youtube.com/watch?v=5_N84t1rUU0)** (Tina Huang) — intro directa a Hermes Agent para quien nunca armó un harness.
- **[How Hermes implements an open source agent harness architecture](https://arize.com/blog/how-hermes-implements-open-source-agent-harness-architecture/)** (Arize AI) — el artículo técnico con el detalle de las cuatro piezas de arriba aplicadas en código real.
- **[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)** — el código, open source (MIT), para leer la implementación en vez de solo la explicación.

## Si querés armar uno propio en vez de solo entenderlo

Un harness no hace falta escribirlo totalmente desde cero — los SDKs de `../sdks-de-agentes/` (Claude Agent SDK, OpenAI Agents SDK, Pydantic AI, etc.) ya resuelven el loop de tool-use y buena parte de la gestión de contexto; lo que arma el equipo encima es la capa de permisos y las tools específicas del caso de uso.

- **[Harness Engineering Explained in 22 Minutes](https://www.youtube.com/watch?v=UmZytjgs2eo)** (Shaw Talebi) — por qué "harness engineering" se volvió su propia disciplina: qué partes son el modelo y cuáles son la ingeniería alrededor.
- **[AI Engineer Summit 2025 — Agent Engineering (Day 2), playlist completa](https://www.youtube.com/playlist?list=PLcfpQ4tk2k0WzqWDdWkN2DnZOhtYI9jyI)** (canal oficial de AI Engineer) — varias charlas del track dedicado a construir agentes en producción. Arranca con **["How We Build Effective Agents" de Barry Zhang (Anthropic)](https://www.youtube.com/watch?v=D7_ipDqhtwk)**, la charla en vivo detrás del artículo ["Building Effective Agents"](https://www.anthropic.com/engineering/building-effective-agents) que ya está en `../orquestadores/`.
