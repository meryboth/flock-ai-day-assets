# Seguridad en agentes

Lo que cambia cuando un agente no solo responde, sino que ejecuta acciones por su cuenta.

- **[Building Effective Agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents)** — ya citado en `orquestadores/`, pero también toca guardrails y supervisión humana.
- **Prompt injection**: tratá cualquier dato externo (un email, una página, un archivo) como no confiable — nunca como una instrucción.
- **Permisos acotados**: si un agente puede ejecutar una acción real (mandar un mail, aprobar un pago, hacer un commit), que sea explícito qué puede y qué no puede hacer, no "lo que se le ocurra".
- **Secretos y API keys**: nunca hardcodeados ni pegados en el prompt — siempre variables de entorno.
