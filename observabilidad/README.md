# Observabilidad

Observar un agente es distinto de observar software tradicional: en software tradicional, el mismo input produce el mismo output, así que un log de error te dice bastante. Un agente puede responder distinto a la misma pregunta dos veces, puede decidir llamar una tool que no esperabas, o puede fallar de formas que no son un error de código sino una mala decisión del modelo. Por eso la observabilidad de agentes necesita capturar no solo "qué pasó" sino **la cadena completa de razonamiento y llamadas** que llevó a un resultado.

## Qué trackear, concretamente

Cada llamada al modelo, cada tool ejecutada y cada paso de retrieval se registra como un **span** dentro de un **trace** — el equivalente a una pila de llamadas, pero para el razonamiento del agente en vez de para código. Lo mínimo que conviene trackear por trace: costo y latencia por llamada (para detectar cuándo un cambio de prompt o de modelo disparó el gasto), qué tools se llamaron y con qué argumentos (para reconstruir una decisión rara), y alguna forma de evaluar calidad de la respuesta — aunque sea un LLM-as-judge corriendo sobre una muestra (ver `../evals/` para la metodología completa).

Esto ya tiene un estándar: OpenTelemetry (el estándar abierto de observabilidad que ya usa buena parte de la industria para software tradicional) definió convenciones específicas para IA generativa — nombres de spans como `chat`, `invoke_agent`, `execute_tool`, y atributos como tokens de entrada/salida, modelo usado, proveedor — para que una herramienta de observabilidad cualquiera pueda leer el trace sin importar con qué librería se generó.

- **[OpenTelemetry GenAI Semantic Conventions](https://github.com/open-telemetry/semantic-conventions-genai)** (especificación oficial) — los nombres y atributos estándar para spans de modelo, agente, tool y retrieval. Vale la pena mirarlo aunque uses una herramienta propietaria, porque define el vocabulario común del campo.

## Herramientas para no armarlo desde cero

- **[LangSmith](https://docs.langchain.com/langsmith/observability)** — tracing y evals, con integración directa si ya usás LangChain/LangGraph (ver `../orquestadores/`).
- **[Langfuse](https://github.com/langfuse/langfuse)** — alternativa open source, self-hosteable, con tracing, evals y gestión de prompts.

## La conexión con seguridad

Un trace completo no es solo para debuggear — es la evidencia que necesitás cuando un agente con permisos reales (ver `../seguridad-agentes/`) ejecutó una acción y hay que entender por qué. Sin tracing, "¿por qué el agente mandó ese mail?" no tiene respuesta reconstruible.
