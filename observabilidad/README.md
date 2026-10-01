# Observabilidad

Cómo saber si tu sistema con IA sigue funcionando bien con el tiempo — no solo el día que lo probaste.

- **[LangSmith](https://docs.langchain.com/langsmith/observability)** — tracing y evals, con integración directa si ya usás LangChain/LangGraph.
- **[Langfuse](https://github.com/langfuse/langfuse)** — alternativa open source, self-hosteable, con tracing, evals y gestión de prompts.
- Qué trackear como mínimo: costo y latencia por llamada, calidad de las respuestas (evals automáticos o LLM-as-judge), y alertas cuando algo degrada (drift).
