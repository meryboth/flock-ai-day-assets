# Patrones de coordinación multiagente

Más allá de "usá LangGraph/CrewAI" (ver `../orquestadores/` para los frameworks): esto es sobre cuándo conviene repartir el trabajo entre varios agentes, y cómo organizarlos para que no termine siendo más lento, más caro o menos confiable que uno solo. Elegir mal el patrón es la causa más común de que un sistema multiagente falle en producción — no el framework.

## Los patrones, de menor a mayor complejidad

- **Pipeline / secuencial** — cada agente consume la salida del anterior, uno atrás del otro. El más simple y el más fácil de debuggear (el error siempre está en un paso identificable), pero no hay paralelismo: todo el pipeline es tan lento como la suma de sus partes.
- **Supervisor** — un agente "jefe" recibe la tarea completa, la parte en subtareas, se las reparte a agentes especializados, y junta los resultados en una respuesta final. Es el patrón default en producción hoy: los sub-agentes no se ven entre sí ni comparten contexto durante la ejecución, lo que lo hace más predecible que un patrón donde todos hablan con todos.
- **Paralelo (fan-out / fan-in)** — variante del supervisor donde las subtareas son independientes entre sí y corren al mismo tiempo, no una tras otra. Reduce la latencia total, pero multiplica el costo (tantas llamadas simultáneas como agentes) y complica el manejo de errores (¿qué hacés si 2 de 5 fallan?).
- **Jerárquico (sub-agentes)** — un agente dispara sub-agentes propios para partes de la tarea, cada uno con su propio contexto aislado — es literalmente cómo trabaja Claude Code con sus subagentes, y lo que usa Hermes Agent (ver `../harness/`) para aislar tareas que no deberían compartir memoria entre sí.
- **Handoff** — un agente decide, en medio de la ejecución, pasarle el control completo a otro más especializado (por ejemplo: un agente de soporte general que detecta que la consulta es de facturación y se la pasa a un agente de facturación). Es un patrón de primera clase en el OpenAI Agents SDK (ver `../sdks-de-agentes/`), no algo que tenés que armar vos arriba de un supervisor genérico.
- **Debate / crítica entre agentes** — a diferencia del supervisor (donde los sub-agentes no se ven entre sí), acá varios agentes revisan y cuestionan la misma respuesta antes de darla por buena. Mejora la calidad a costa de mucho más tiempo y tokens — tiene sentido solo cuando el costo de un error es alto.
- **Blackboard** — en vez de que los agentes se hablen directo entre sí, todos leen y escriben a un estado compartido (un "pizarrón"), y cada uno actúa cuando ve algo relevante ahí. Útil cuando no sabés de antemano en qué orden van a hacer falta los agentes.

## El caso real que mejor ilustra esto

- **[How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)** (Anthropic, blog de ingeniería) — el recurso ancla de esta carpeta: el sistema real detrás de "Research" de Claude. Patrón **supervisor + paralelo combinados**: un agente líder arma la estrategia y dispara sub-agentes en paralelo (cada uno con su propio context window, sin verse entre sí), después compila los hallazgos. Con datos duros: 90.2% mejor que un solo agente en su eval interno, y con los principios de prompt engineering concretos que usaron para que la coordinación no se rompiera (cómo definir el rol de cada sub-agente, cómo evitar que se pisen entre sí).
- **[LangGraph — Multi-agent Supervisor (referencia oficial)](https://reference.langchain.com/python/langgraph-supervisor)** — la implementación de referencia del patrón supervisor en código, si querés ver exactamente cómo se estructura.
- **[Build a Supervisor Multi-agent Architecture with LangGraph](https://www.youtube.com/watch?v=KAwiykWkQ5Q)** — walkthrough en video armando el patrón paso a paso (no pudimos confirmar el canal/creador, pero el contenido es relevante).

## El error más común

Splitear un problema en agentes separados cuando en realidad era un solo agente con varias tools — eso no es coordinación multiagente, es overhead sin beneficio (cada "salto" entre agentes cuesta tokens y latencia, y suma una superficie más grande para que algo salga mal). La pregunta que conviene hacerse antes de elegir un patrón: ¿el problema realmente tiene partes que se benefician de contextos separados o de ejecución en paralelo? Si la respuesta es no, un solo agente con un buen harness (ver `../harness/`) resuelve mejor.
