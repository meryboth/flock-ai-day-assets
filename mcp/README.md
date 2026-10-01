# MCP (Model Context Protocol)

Antes de MCP, conectar un agente a una herramienta externa (Slack, GitHub, una base de datos) significaba escribir una integración ad-hoc por cada combinación de agente y herramienta — el clásico problema de **N agentes × M herramientas = N×M integraciones**. MCP es el estándar abierto que lo resuelve: cualquier servidor MCP habla el mismo protocolo, así que cualquier cliente (agente) compatible lo puede usar sin código a medida. Lo anunció Anthropic en noviembre de 2024 y hoy lo adoptaron también OpenAI y Google DeepMind, entre otros.

## Cómo está armado

Un servidor MCP expone tres tipos de cosas a quien se conecte: **tools** (funciones que el agente puede ejecutar), **resources** (datos que puede leer, como archivos o filas de una base) y **prompts** (plantillas reutilizables). El cliente (el agente, a través de su harness — ver `../harness/`) se conecta, descubre qué expone ese servidor, y lo usa como si fuera una tool más. La mayoría de los SDKs de `../sdks-de-agentes/` ya traen cliente MCP incorporado, así que conectar un servidor existente no requiere escribir el protocolo a mano.

- **[modelcontextprotocol.io](https://modelcontextprotocol.io/)** — el sitio oficial: qué es MCP y por qué existe.
- **[Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol)** (Anthropic, anuncio original) — la motivación completa y los primeros adoptantes (Block, Apollo, Zed, Replit).
- **[Especificación](https://github.com/modelcontextprotocol/modelcontextprotocol)** — el protocolo en detalle, para cuando quieras construir tu propio server.
- **[Servers de referencia](https://github.com/modelcontextprotocol/servers)** — implementaciones reales de MCP servers (filesystem, GitHub, Slack, etc.) para ver el patrón aplicado antes de escribir uno propio.
- **[SDKs oficiales](https://modelcontextprotocol.io/docs/sdk)** — Python, TypeScript y otros, para construir tanto un server como un cliente MCP.

## Lo que MCP no resuelve: quién confía en quién

Un MCP server es, ni más ni menos, una fuente de contenido que tu agente va a procesar — y si ese contenido no es confiable (un ticket de soporte, un archivo subido por un usuario), se cumple una de las tres condiciones de la "trifecta letal" que armar un MCP server mal pensado puede terminar filtrando datos privados. El caso real más citado conecta directo con esto: un MCP server de Supabase que, sin las validaciones correctas, dejaba que texto malicioso en un ticket hiciera que el agente filtrara una base SQL completa.

- **[Supabase MCP can leak your entire SQL database](https://simonwillison.net/2025/Jul/6/supabase-mcp-lethal-trifecta/)** (Simon Willison) — el caso aplicado, y el framework completo de la "trifecta letal" está en `../seguridad-agentes/`.
