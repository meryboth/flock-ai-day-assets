# Orquestadores

Frameworks para coordinar varios agentes, o varios pasos con estado, en vez de depender de un único prompt aislado — el salto de Practitioner a Builder según el rubro de AI Day.

- **[LangGraph](https://github.com/langchain-ai/langgraph)** ([docs](https://docs.langchain.com)) — modela un flujo como un grafo de nodos con estado y condicionales. Es lo que vimos en la charla de Practitioner.
- **[CrewAI](https://github.com/crewAIInc/crewAI)** — agentes con roles definidos que colaboran entre sí; buena puerta de entrada si tu caso de uso se presta a pensar en "equipo de agentes".
- **[AutoGen](https://github.com/microsoft/autogen)** (Microsoft) — otro framework de multi-agente, con foco en conversaciones entre agentes.
- **[AWS Strands Agents](https://github.com/strands-agents)** ([docs](https://strandsagents.com)) — el SDK open source de AWS que vimos en la charla de Practitioner: enfoque model-driven, agnóstico de modelo y de nube.
- **[Building Effective Agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents)** — antes de elegir un framework, vale la pena leer esto: cuándo conviene un workflow simple y cuándo realmente hace falta un agente autónomo.
