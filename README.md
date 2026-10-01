# Flock AI Day — Recursos

Recursos curados para el día del evento, pensados para el perfil **Builder**: gente que ya construyó su primera solución agéntica o automatización, y que este día dedica la jornada a analizar y mejorar su propio repositorio en vez de ir a una charla.

No es una colección de todo lo que existe sobre IA — es una selección enfocada en lo que más sirve para subir de nivel según el mismo rubro que ya usa la plataforma de AI Day (orquestación, MCP, RAG, observabilidad, seguridad e impacto de negocio).

## Cómo está organizado

| Carpeta | De qué se trata |
|---|---|
| [`orquestadores/`](./orquestadores) | Frameworks para coordinar varios agentes o pasos con estado: LangGraph, CrewAI, AutoGen, AWS Strands. |
| [`coordinacion-multiagente/`](./coordinacion-multiagente) | Patrones (supervisor, pipeline, paralelo, jerárquico, debate) para repartir trabajo entre varios agentes sin que termine siendo más lento o caro que uno solo. |
| [`sdks-de-agentes/`](./sdks-de-agentes) | SDKs de base para programar un agente propio: Claude Agent SDK, OpenAI Agents SDK, Google ADK, Vercel AI SDK, Pydantic AI. |
| [`harness/`](./harness) | Qué es un "harness" (la capa de control de un agente) y un ejemplo real open source para estudiarlo. |
| [`mcp/`](./mcp) | Model Context Protocol: el estándar para conectar agentes a herramientas y fuentes de datos. |
| [`rag/`](./rag) | Retrieval-Augmented Generation: chunking, reranking, vector DBs, RAG en producción. |
| [`observabilidad/`](./observabilidad) | Tracing y métricas de costo/latencia/calidad para sistemas con IA en producción. |
| [`evals/`](./evals) | Cómo medir la calidad de un agente de forma sistemática, con un dataset de casos, antes de shippear. |
| [`memoria-de-agentes/`](./memoria-de-agentes) | Arquitecturas de memoria de corto y largo plazo para que un agente no "olvide" entre sesiones. |
| [`seguridad-agentes/`](./seguridad-agentes) | Prompt injection, sandboxing, permisos y manejo de secretos al darle autonomía a un agente. |
| [`papers/`](./papers) | Los papers detrás de los patrones que ya estás usando (ReAct, Toolformer, Reflexion, etc). |
| [`repos-de-referencia/`](./repos-de-referencia) | Proyectos open source reales para ver estos patrones aplicados, no solo en teoría. |
| [`cursos/`](./cursos) | Videos y charlas más largas para profundizar antes o después del evento. |

Esto conecta directo con las charlas del día: la de Practitioner (Francisco Sempé) introduce LangGraph, MCP, RAG y AWS Strands — este repo es donde profundizás cualquiera de esos temas si ya los conocés y querés ir más allá.

## Cómo sumar algo

Esto es una lista viva. Si encontrás un recurso que te sirvió, mandá un PR agregándolo al README de la carpeta que corresponda, con una línea explicando por qué vale la pena — no hace falta nada más elaborado que eso.
