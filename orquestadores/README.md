# Orquestadores

Un SDK de base (ver `../sdks-de-agentes/`) te da el loop de un agente. Un **orquestador** resuelve un problema distinto: cómo coordinar *varios* agentes, o varios pasos con estado propio, cuando una sola conversación lineal con un modelo ya no alcanza. El salto de Practitioner a Builder según el rubro de AI Day pasa justamente por acá — y por elegir *qué patrón* de coordinación usar (ver `../coordinacion-multiagente/`), no solo qué framework.

Cada framework modela la coordinación de forma distinta, y eso importa a la hora de elegir:

- **[LangGraph](https://github.com/langchain-ai/langgraph)** ([docs](https://docs.langchain.com)) — modela todo como un **grafo de nodos con estado**: cada nodo es un paso (puede ser un agente, una tool, una condición), y las aristas definen a qué nodo seguir según el resultado. Es el modelo más explícito y más fácil de debuggear paso a paso, a costa de más código de setup. Es lo que vimos en la charla de Practitioner.
- **[CrewAI](https://github.com/crewAIInc/crewAI)** — modela la coordinación como **roles que colaboran**: armás una "crew" de agentes, cada uno con un rol y un objetivo, y el framework resuelve cómo se reparten el trabajo. Más rápido para arrancar si tu problema ya se piensa naturalmente como "un equipo", más difícil de controlar con precisión que un grafo explícito.
- **[AutoGen](https://github.com/microsoft/autogen)** (Microsoft) — modela la coordinación como una **conversación entre agentes**: los agentes se mandan mensajes entre sí (incluso un agente puede representar a un humano en el loop) hasta llegar a una resolución. Útil cuando el patrón real del problema es más parecido a una discusión que a un pipeline fijo.
- **[AWS Strands Agents](https://github.com/strands-agents)** ([docs](https://strandsagents.com)) — el SDK open source de AWS que vimos en la charla de Practitioner: enfoque **model-driven** (el propio modelo decide buena parte de la lógica de coordinación en vez de que esté codificada a mano), agnóstico de modelo y de nube — también sirve como SDK de base, está listado en `../sdks-de-agentes/` por eso.

## Cuándo (no) conviene un orquestador

El error más común es reachar a un orquestador para un problema que un solo agente con un loop de tool-use resuelve mejor y más barato. Antes de elegir framework, vale la pena entender el criterio de cuándo de verdad hace falta — y ahí la referencia obligada es la de Anthropic, escrita por el mismo equipo que después dio la charla que inspiró parte de esta sección de Practitioner.

- **[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)** (Anthropic) — el criterio de "no construyas un agente para todo, empezá simple": cuándo un workflow fijo (secuencial, paralelo) le gana a un agente autónomo, y cuándo realmente conviene la autonomía completa de un orquestador.
- **["How We Build Effective Agents" — charla de Barry Zhang (Anthropic)](https://www.youtube.com/watch?v=D7_ipDqhtwk)**, dentro de la **[playlist completa del track de Agent Engineering del AI Engineer Summit 2025](https://www.youtube.com/playlist?list=PLcfpQ4tk2k0WzqWDdWkN2DnZOhtYI9jyI)** — la versión en vivo del artículo de arriba, con preguntas del público.

Una vez que decidiste que sí hace falta coordinar varios agentes, el siguiente paso es elegir el patrón (supervisor, pipeline, paralelo, jerárquico, debate) antes que el framework — eso está en `../coordinacion-multiagente/`.
