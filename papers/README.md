# Papers

Los papers y artículos de investigación detrás de los patrones que ya estás usando, para quien quiera ir a la fuente en vez de quedarse con la versión resumida.

- **[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)** — el patrón base de "pensar, actuar, observar" detrás de casi todo agente con tools. Es, literalmente, el loop que implementa cualquier harness (ver `../harness/`).
- **[Toolformer: Language Models Can Teach Themselves to Use Tools](https://arxiv.org/abs/2302.04761)** — cómo un modelo aprende cuándo y cómo llamar una herramienta, en vez de que se lo digan a mano con prompting.
- **[Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366)** — agentes que mejoran iterando sobre sus propios errores (en texto, no con gradientes) — un antecedente directo de por qué el patrón de "debate/crítica entre agentes" en `../coordinacion-multiagente/` funciona.
- **[MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560)** — el framework que popularizó pensar la memoria de un agente como un sistema operativo administrando RAM y disco, en vez de una ventana de contexto cada vez más grande. Ver `../memoria-de-agentes/`.
- **[Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory](https://arxiv.org/abs/2504.19413)** — el diseño de un sistema de memoria de largo plazo real en producción, que extrae y estructura hechos en vez de guardar todo crudo. También en `../memoria-de-agentes/`.
- **[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)** (Anthropic) — no es un paper académico, pero es la referencia más práctica y citada sobre cuándo un workflow simple le gana a un agente autónomo. Ver `../orquestadores/` para cuándo aplica cada uno.
