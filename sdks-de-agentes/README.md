# SDKs para construir agentes

A diferencia de `orquestadores/` (frameworks pensados para coordinar *varios* agentes o pasos con estado), acá van los SDKs de base para programar un agente propio — el loop de tool-use, el manejo de contexto y la integración con un modelo, sin tener que escribirlo desde cero.

- **[Claude Agent SDK](https://github.com/anthropics/claude-agent-sdk-typescript)** ([Python](https://github.com/anthropics/claude-agent-sdk-python), [docs](https://docs.claude.com/en/api/agent-sdk/overview)) — el mismo motor detrás de Claude Code, con tools de archivo/bash/búsqueda ya resueltas (Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch). El camino más directo si ya usás Claude Code y querés programar algo parecido.
- **[OpenAI Agents SDK](https://github.com/openai/openai-agents-python)** ([JS/TS](https://github.com/openai/openai-agents-js), [docs](https://openai.github.io/openai-agents-python/)) — liviano y agnóstico de proveedor (soporta 100+ modelos, no solo OpenAI). Conceptos clave: agentes como tools, handoffs entre agentes, y guardrails para validar inputs/outputs.
- **[Google ADK (Agent Development Kit)](https://github.com/google/adk-python)** ([docs](https://github.com/google/adk-docs), también en Java/Go) — toolkit code-first de Google, optimizado para Gemini pero model-agnostic, con una UI web propia (`adk-web`) para debuggear el agente mientras lo armás.
- **[Vercel AI SDK](https://github.com/vercel/ai)** — el toolkit de TypeScript de los creadores de Next.js (la misma stack de esta plataforma de AI Day) para armar agentes y apps con IA con cualquier modelo. Buena opción si tu proyecto ya es un repo Next.js/React.
- **[Pydantic AI](https://github.com/pydantic/pydantic-ai)** ([docs](https://ai.pydantic.dev)) — el equivalente en Python, tipado de punta a punta (menos errores que aparecen recién en runtime), soporta prácticamente cualquier proveedor incluyendo OpenRouter.
- **[AWS Strands Agents](https://github.com/strands-agents)** — ya está en `orquestadores/`, pero también cuenta acá: es un SDK para construir un agente propio, no solo para coordinar varios.

¿Cuál elegir? Si ya programás en TypeScript con Next.js, arrancá por Vercel AI SDK o Claude Agent SDK. Si tu stack es Python, Pydantic AI u OpenAI Agents SDK. Si ya estás en el ecosistema de AWS o Google Cloud, Strands o ADK respectivamente.
