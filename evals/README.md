# Evals de agentes

Observabilidad (ver `../observabilidad/`) te dice qué pasó en una corrida puntual. Evals responde una pregunta distinta y más incómoda: **¿qué tan bien funciona esto en general, de forma medible y repetible?** — no "probé 3 casos a mano y andaba bien", sino un dataset de casos con una métrica que podés volver a correr cada vez que cambiás un prompt, un modelo o una tool.

## El dataset golden: la base de todo

Un **golden dataset** es un conjunto revisado y versionado de casos representativos, cada uno con el resultado esperado (o un criterio para juzgarlo) ya validado por una persona. No hace falta empezar enorme — la práctica común es arrancar con 25-50 casos que cubran los usos más importantes, los modos de falla ya conocidos de incidentes pasados, y los edge cases que fueron apareciendo en producción (ahí es donde la observabilidad alimenta directamente los evals: los traces reales son la mejor fuente de casos nuevos para el dataset). Correr ese dataset en CI antes de cada cambio — no solo una vez — es lo que convierte un eval en una red de seguridad real en vez de un ejercicio que se hizo una vez y se olvidó.

- **[Golden dataset evaluation: build and maintain LLM test sets](https://langfuse.com/resources/engineering/golden-dataset-evaluation)** (Langfuse) — cómo armar uno desde cero, mantenerlo a medida que el producto cambia, y usarlo para comparar versiones de un prompt.
- **[LLM regression testing: fail CI before regressions ship](https://langfuse.com/resources/engineering/llm-regression-testing)** (Langfuse) — cómo engancharlo a CI para que un cambio que empeora la calidad no llegue a producción, el mismo espíritu que un test de regresión en software tradicional.

## LLM-as-a-judge: la técnica para evaluar a escala

Revisar cada respuesta a mano no escala. La alternativa más usada es que **otro LLM actúe de juez**: le das el criterio de evaluación en texto plano (lo que buscás, lo que no es aceptable) y puntúa la respuesta del agente contra ese criterio. Funciona bien, pero tiene un punto ciego real: el juez puede tener los mismos sesgos o puntos ciegos que el modelo que está evaluando, así que conviene calibrarlo contra evaluación humana en una muestra antes de confiar en él para todo.

- **[LLM as a Judge: Scaling AI Evaluation Strategies](https://www.youtube.com/watch?v=trfUBIDeI1Y)** — la técnica explicada, con sus límites.
- **[Observability and Evals for AI Agents: A Simple Breakdown](https://www.youtube.com/watch?v=FDVdLrloFOw)** — por qué la observabilidad de agentes es distinta (y más importante) que la de software tradicional: no sabés qué va a hacer el agente hasta que lo corrés.
- **[You Can Learn AI Agent Harness & Loop Engineering In 19 Min](https://www.youtube.com/watch?v=GrNbuWWJYiI)** (LLM Ops, Eval, Tracing, RAG) — recorre varias piezas juntas; útil para ver cómo encaja el eval dentro del loop completo de un agente (ver `../harness/`), no solo de forma aislada.
