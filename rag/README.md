# RAG (Retrieval-Augmented Generation)

RAG es la técnica para que un agente responda con información propia (documentación, código, datos del negocio) en vez de solo lo que el modelo aprendió durante su entrenamiento. En el fondo son tres pasos: **partir** los documentos en pedazos manejables (chunking), **buscar** los pedazos más relevantes para la pregunta (retrieval, normalmente con embeddings + similitud vectorial), y **generar** la respuesta dándole esos pedazos como contexto al modelo. El problema real no es ese pipeline básico — es que cada uno de esos tres pasos tiene formas de fallar que no se notan hasta que el sistema está con datos reales.

## Por qué un RAG "de demo" falla con datos reales

El punto más débil típico es el chunking: si partís un documento en pedazos demasiado chicos, cada pedazo pierde el contexto que lo rodeaba (una tabla sin su título, una cláusula sin saber a qué contrato pertenece), y la búsqueda por similitud encuentra el pedazo correcto pero sin la información para entenderlo. Partir en pedazos más grandes preserva contexto pero empeora la precisión de la búsqueda. Anthropic publicó una solución concreta a esto — **Contextual Retrieval** — que antepone a cada chunk una explicación de dónde viene antes de indexarlo, con resultados medidos: 35% menos fallos de retrieval solo con embeddings contextuales, 49% combinando eso con BM25 (búsqueda por palabras clave) además de la vectorial, y hasta 67% sumando un paso de reranking.

- **[Introducing Contextual Retrieval](https://www.anthropic.com/news/contextual-retrieval)** (Anthropic, blog de ingeniería) — la técnica completa, con los números de arriba y por qué la búsqueda híbrida (vectorial + BM25) le gana a cualquiera de las dos solas.
- **[Retrieval-Augmented Generation (RAG) Best Practices for Enterprise AI](https://www.stackai.com/insights/retrieval-augmented-generation-(rag)-best-practices-for-enterprise-ai-chunking-embeddings-reranking-and-hybrid-search-optimization)** (StackAI) — guía práctica con defaults razonables para arrancar: chunks de 256-1024 tokens con 10-20% de superposición, cuándo conviene hybrid search.

## Qué separa un RAG de demo de uno de producción

No es un modelo más grande — es: **chunking semántico** (respetar la estructura real del documento, no cortar cada N caracteres), **reranking** (un segundo paso que reordena los resultados de la búsqueda inicial por relevancia real, no solo similitud vectorial) y **trazabilidad de fuentes** (poder mostrar de qué documento salió cada afirmación de la respuesta — necesario tanto para confiar en el sistema como para debuggear cuándo se equivoca). Medir si estas tres cosas funcionan bien no es lo mismo que evaluar la respuesta final — ver `../evals/` para evals específicos de retrieval (recall@k, no solo "la respuesta estuvo bien").

## Dónde se guarda lo que se indexa

Vector DBs para arrancar: **pgvector** (si ya tenés Postgres/Supabase, es el camino de menor fricción), **Pinecone** o **Qdrant** (managed, fáciles de levantar). RAG también es, en la práctica, una de las formas más comunes de implementar memoria de largo plazo en un agente — ver `../memoria-de-agentes/` para cuándo conviene eso en vez de (o además de) una base de datos estructurada.
