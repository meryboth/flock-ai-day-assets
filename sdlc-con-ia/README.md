# SDLC con IA

Cuando un agente escribe código en minutos, el cuello de botella deja de ser escribir y pasa a ser **decidir y revisar**: qué se quiere construir, si lo que salió es eso, y si se puede mergear. Sin un marco, encarar un proyecto con IA termina en "vibe coding" — prompts sueltos, resultados que andan en la demo y nadie sabe bien por qué. Esta carpeta junta los marcos que están apareciendo para darle estructura a ese trabajo: qué artefacto sale de cada etapa, quién lo aprueba y qué lee la etapa siguiente.

## El loop intent → spec → plan

La idea que comparten todos los marcos de acá es la misma: **antes de pedir código, dejar por escrito qué se quiere y cómo se va a hacer**, en archivos versionados junto al repo. Primero el *intent* (qué problema resuelve y para quién), después la *spec* (comportamiento esperado, criterios de aceptación), después el *plan* (arquitectura, tareas chicas y revisables), y recién ahí la implementación. Cada etapa produce un artefacto que la siguiente lee — humano o agente — y entre etapas hay un gate donde una persona aprueba antes de seguir. El agente hace el trabajo de rutina; las decisiones importantes siguen siendo humanas.

- **[The AI-Native SDLC Playbook](https://claude.com/blog/the-ai-native-sdlc-playbook)** (Anthropic) — el marco más nuevo: seis etapas (Plan con `intent.md`, Design con `spec.md`, Build con `plan.md` + código, Test con evals continuos, Deploy con review y gates, Maintain con monitoreo que realimenta el intent). Pone el foco en que la governance se aplique mientras el agente actúa (hooks, políticas), no semanas después en un code review.

## AI-DLC: la versión de AWS

AWS propone el **AI-Driven Development Life Cycle**: el agente arma un plan detallado, pide aclaraciones y difiere las decisiones críticas a personas, en tres fases (Inception, Construction, Operations) y con ciclos cortos ("bolts") en vez de sprints. Lo interesante es que lo publicaron como reglas y steering files listos para usar, no solo como teoría.

- **[AI-Driven Development Life Cycle: Reimagining Software Engineering](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)** (AWS, Raja SP) — el post que presenta la metodología y sus fases.
- **[Open-Sourcing Adaptive Workflows for AI-DLC](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/)** (AWS) — los workflows abiertos ([`awslabs/aidlc-workflows`](https://github.com/awslabs/aidlc-workflows)) para Amazon Q Developer y Kiro, que eligen qué etapas correr según el tamaño del cambio.

## Spec-Driven Development (SDD)

SDD lleva la idea al extremo: **la spec es la fuente de verdad** y el código se genera y se valida contra ella. El argumento es simple — los modelos son muy buenos completando patrones y muy malos leyendo la mente, así que una spec clara elimina la adivinanza. Funciona igual de bien para proyectos nuevos, features sobre sistemas existentes o modernizar legacy.

- **[Spec-driven development with AI: Get started with a new open source toolkit](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)** (GitHub, Den Delimarsky) — presenta el proceso en cuatro fases con checkpoints: Specify → Plan → Tasks → Implement.
- **[Spec Kit](https://github.com/github/spec-kit)** (GitHub) — el toolkit open source: templates, CLI y prompts que funcionan con Claude Code, Copilot y Gemini CLI. El camino más corto para probar SDD en un repo real.

## Cómo encaja con el resto

Las etapas finales del loop son temas que ya tienen carpeta propia: Test es [`../evals/`](../evals), Maintain es [`../observabilidad/`](../observabilidad), y los gates y hooks que hacen cumplir las reglas mientras el agente trabaja viven en [`../harness/`](../harness) y [`../seguridad-agentes/`](../seguridad-agentes).
