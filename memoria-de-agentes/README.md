# Memoria de agentes

"Mi agente se olvida de lo que hizo hace diez tool calls" no se arregla con una ventana de contexto más grande — el problema no es cuánto entra, es **qué vale la pena conservar**. Memoria, como disciplina, es decidir qué información persiste, en qué forma, y cómo se recupera cuando hace falta.

## Dos tipos de memoria, dos problemas distintos

**Memoria de corto plazo** es lo que vive dentro de una sesión: el historial de la conversación actual, los resultados de tools ya llamadas. El harness (ver `../harness/`) la administra comprimiendo o descartando lo que ya no es relevante para la próxima decisión — eso es, literalmente, lo que hace Hermes Agent con su compresión basada en "lineage".

**Memoria de largo plazo** es lo que persiste *entre* sesiones: preferencias del usuario, decisiones pasadas, hechos aprendidos. Acá el framework más citado es **MemGPT**, que plantea pensar la memoria de un agente como un sistema operativo administra RAM y disco: el modelo "pagina" información adentro y afuera de su contexto activo según la necesita, en vez de tratar todo como una sola ventana plana.

- **[MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560)** (paper) — el framework que popularizó pensar la memoria de un agente como jerarquía de almacenamiento, no como una ventana de contexto cada vez más grande.
- **[Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory](https://arxiv.org/abs/2504.19413)** (paper) — el enfoque de un sistema real de memoria en producción: extrae y consolida hechos estructurados en el momento de escribir (no solo guarda todo crudo), lo que hace la recuperación posterior mucho más precisa.
- **[Agent Memory EXPLAINED - Complete Architecture](https://www.youtube.com/watch?v=aYfZN8t6AQs)** — recorre la arquitectura completa de memoria de largo plazo (mucho más que un vector search), con un caso real usando Mem0.

## Cómo se implementa en la práctica

La forma más común de memoria de largo plazo hoy es, de hecho, RAG (ver `../rag/`): guardar hechos o conversaciones pasadas como vectores y recuperar los relevantes para la consulta actual. Pero no es la única: sistemas como Mem0 extraen y estructuran hechos en vez de guardar texto crudo, y otros enfoques (como Zep) mantienen un grafo de conocimiento temporal — útil cuando lo que importa no es solo "qué se dijo" sino "cuándo cambió" (por ejemplo, la preferencia de un usuario que cambió con el tiempo).

Antes de elegir una arquitectura, vale la pena medir si de verdad hace falta: agregar memoria tiene un costo real (más tokens, más latencia, más superficie para que algo se filtre mal — ver `../seguridad-agentes/` si la memoria incluye datos sensibles), y no todos los casos de uso lo justifican.
