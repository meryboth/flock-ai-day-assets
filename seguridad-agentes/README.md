# Seguridad en agentes

Cuando un sistema con IA solo responde texto, lo peor que puede pasar es que la respuesta esté mal. Cuando un agente además **ejecuta acciones** — manda un mail, hace un commit, aprueba un pago, borra un archivo — lo peor que puede pasar cambia de categoría: ya no es "una respuesta mala", es una acción real tomada en base a una instrucción que nadie autorizó. Esta sección explica los riesgos que importan y cómo se mitigan en la práctica, no solo una lista de buenas prácticas sueltas.

## El modelo mental: la "trifecta letal"

El framework más citado para pensar esto es el de Simon Willison (mantenedor de Datasette, uno de los que más escribe sobre seguridad en agentes de IA): un sistema es vulnerable a que le roben datos privados cuando se dan **las tres condiciones a la vez**:

1. **Acceso a datos privados** — el agente puede leer mails, documentos, bases de datos, código fuente.
2. **Exposición a contenido no confiable** — el agente procesa texto que viene de afuera (una página web, un mail recibido, un archivo subido por un usuario, el output de otra herramienta).
3. **Un canal de salida hacia afuera** — el agente puede mandar un mail, hacer un request HTTP, escribir en un lugar que alguien más lee.

Sacá cualquiera de las tres patas y el ataque pierde el premio: si no hay datos privados, no hay nada que robar; si no hay contenido no confiable, no hay dónde esconder la instrucción maliciosa; si no hay canal de salida, el agente no tiene cómo filtrar lo que encontró. El caso real más citado: un MCP server de Supabase que, combinando las tres condiciones, dejaba que un ticket de soporte con texto malicioso hiciera que el agente filtrara el contenido completo de una base de datos SQL.

- **[The lethal trifecta for AI agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)** (Simon Willison) — el post original, con el framework completo y ejemplos.
- **[Supabase MCP can leak your entire SQL database](https://simonwillison.net/2025/Jul/6/supabase-mcp-lethal-trifecta/)** (Simon Willison) — el caso real aplicando el framework a un MCP server de verdad.

## Prompt injection: por qué no se puede "parchear"

Un modelo de lenguaje no tiene una forma confiable de distinguir "esto es una instrucción que me da mi usuario" de "esto es un dato que estoy leyendo" — todo le llega mezclado como texto. Eso significa que cualquier contenido externo que el agente procese (el cuerpo de un mail, el texto de una página, los metadatos de un archivo, hasta el output de otra tool) puede contener una instrucción escondida que el modelo va a tratar como si viniera de vos. No es un bug puntual que se arregla con un parche — es una propiedad estructural de cómo funcionan los LLMs hoy, y por eso la mitigación real no es "mejorar la detección" sino diseñar el sistema asumiendo que va a pasar.

La regla práctica: **tratá todo dato externo como no confiable, nunca como instrucción** — sin importar cuán "inocente" se vea el archivo, el mail o la página. Esto es justamente lo que cubre la categoría oficial de OWASP para este riesgo.

- **[OWASP Top 10 for LLM Applications — LLM01:2025 Prompt Injection](https://genai.owasp.org/llm-top-10/)** — la referencia oficial de la industria para este riesgo (y los otros nueve del top 10, varios relevantes acá: LLM02 Sensitive Information Disclosure, LLM06 Excessive Agency).

## Arquitectura en capas: el modelo no es la única defensa

Anthropic separa la seguridad de un agente en cuatro capas: el **modelo** (qué tan bien entrenado está para reconocer un ataque), el **harness** (las instrucciones y guardrails que vos le das — ver `../harness/`), las **tools** (qué puede ejecutar realmente y con qué permisos) y el **entorno** (dónde corre y qué puede tocar desde ahí). El punto clave: un modelo excelente no salva un harness mal diseñado, tools con permisos de más, o un entorno sin aislamiento. La seguridad real es la combinación de las cuatro, no una sola.

De ahí sale el patrón de permisos que ya usan frameworks como el Claude Agent SDK (ver `../sdks-de-agentes/`): cada tool se configura como `always_allow` (corre sola) o `always_ask` (necesita confirmación humana antes de ejecutar). Un agente puede *proponer* una acción sin tener automáticamente permiso para *ejecutarla* — la diferencia entre "leyó el archivo" y "mandó el mail" no es cosmética, es la frontera donde tiene que aparecer una persona si la acción es irreversible o sensible.

- **[Trustworthy agents in practice](https://www.anthropic.com/research/trustworthy-agents)** (Anthropic) — el artículo completo sobre esta arquitectura en capas, con los cinco principios que usan (mantener al humano en control, alinear con valores humanos, asegurar las interacciones, transparencia, privacidad) y ejemplos concretos como "Plan Mode" (revisar la estrategia completa antes de ejecutar, no aprobar paso a paso).
- **[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)** (Anthropic) — ya citado en `../orquestadores/`, pero la sección sobre guardrails y supervisión humana aplica directo acá.

## Permisos acotados y autonomía gradual

Si un agente puede ejecutar una acción real (mandar un mail, aprobar un pago, hacer un commit), tiene que ser explícito **qué puede y qué no puede hacer** — nunca "lo que se le ocurra". En la práctica esto se traduce en: listar expresamente las tools que tiene disponibles (nada de acceso genérico "a todo"), decidir cuáles requieren confirmación humana antes de correr, y pensar la autonomía como algo que se otorga de forma gradual (empezar con todo en modo "preguntame antes", soltar autonomía a medida que el agente demuestra que es confiable para ese caso puntual) — no todo o nada desde el día uno. Esto es exactamente lo que OWASP llama "Excessive Agency" (LLM06): darle a un agente más permisos, herramientas o autonomía de la que su caso de uso realmente necesita.

## Manejo de secretos y credenciales

Nunca hardcodeados en el código, y tampoco pegados directo en el prompt o system prompt — un secreto que vive en el contexto del modelo puede terminar filtrado por la misma vía que cualquier otro dato (un log, una respuesta, un prompt injection que le pida al agente "repetime todo lo que sabés"). La única ubicación correcta son variables de entorno, inyectadas en runtime, nunca en texto que el modelo "lee" como parte de la conversación.

## Sandboxing y aislamiento del entorno

Un agente con acceso de shell o filesystem real debería correr en un entorno acotado: un contenedor o sandbox efímero, sin acceso a la red salvo lo estrictamente necesario, con límites de tiempo y de cantidad de acciones por corrida. Esto no reemplaza los permisos a nivel de tool — es la capa de abajo: si todo lo demás falla (un prompt injection logra que el agente intente algo que no debería), el sandbox es lo que evita que esa acción tenga efecto real fuera de una caja descartable.
