# RAG (Retrieval-Augmented Generation)

Cómo hacer que un agente responda con información propia (documentación, código, datos del negocio) en vez de solo lo que el modelo ya sabe de memoria.

- **[Building Effective Agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents)** — tiene una sección específica sobre cuándo RAG es la pieza correcta.
- **[Docs de LangChain](https://docs.langchain.com)** — el camino más directo para armar un primer RAG funcional de punta a punta.
- Vector DBs para arrancar: **pgvector** (si ya tenés Postgres/Supabase, es el camino de menor fricción), **Pinecone** o **Qdrant** (managed, fáciles de levantar).
- Lo que separa un RAG de demo de uno de producción (y que ya está en el rubro de Builder de la plataforma): chunking semántico, reranking de resultados, y trazabilidad de qué fuente respaldó cada respuesta.
