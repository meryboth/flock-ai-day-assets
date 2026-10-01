# Patrones de coordinación multiagente

Más allá de "usá LangGraph/CrewAI": cuándo conviene repartir el trabajo entre varios agentes y cómo organizarlos para que no termine siendo más lento o más caro que uno solo.

Patrones más comunes, de menor a mayor complejidad: **pipeline/secuencial** (cada agente consume la salida del anterior), **supervisor** (un agente reparte subtareas a agentes especializados y junta los resultados — el default en producción hoy), **paralelo/fan-out-fan-in** (varios agentes trabajan a la vez sobre partes independientes), **jerárquico** (un agente dispara sub-agentes propios, como hace Claude Code), y **debate/crítica entre agentes** (se cuestionan entre sí el resultado antes de darlo por bueno).

- **[How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)** (Anthropic, blog de ingeniería) — el recurso ancla de esta carpeta: el sistema real detrás de "Research" de Claude. Patrón orchestrator-worker con un agente líder que arma la estrategia y dispara sub-agentes en paralelo (cada uno con su propio context window), más los principios de prompt engineering para coordinarlos. Con datos duros: 90.2% mejor que un solo agente en su eval interno.
- **[LangGraph — Multi-agent Supervisor (referencia oficial)](https://reference.langchain.com/python/langgraph-supervisor)** — la implementación de referencia del patrón supervisor en código.
- **[Build a Supervisor Multi-agent Architecture with LangGraph](https://www.youtube.com/watch?v=KAwiykWkQ5Q)** — walkthrough en video armando el patrón paso a paso (no pudimos confirmar el canal/creador, pero el contenido es relevante).
