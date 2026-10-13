# SOURCES.md

Provenance of every slide of the two lectures: either __new__ (written for this talk), or copied (then possibly trimmed or adapted)
from the lecture of [slides-module2](https://github.com/unibo-fc-isi-iag/slides-module2) named in the _Origin_ column,
at the given line of its `content/<lecture>/_index.md`, as of commit
[`67093ed`](https://github.com/unibo-fc-isi-iag/slides-module2/commit/67093ed) (October 2026).

Section title slides (in bold) are new, unless stated otherwise. Copied slides keep their `{{< slide id >}}` anchor, when they had one;
examples taken from other lectures are renumbered (Examples 4–7 of `llmaas`), but keep their original `lab-snippets` coordinates in commands.
To port a later fix of slides-module2 here, find the slide below, and diff it against the same heading upstream (line numbers drift).

## `content/llmaas/_index.md`

| Slide | Origin |
|---|---|
| **LLM-as-a-Service: APIs, Tools, and Agents** | __new__ |
| Outline | __new__ |
| **LLM-as-a-Service** | __new__ |
| The five dimensions of the trade-off | `governance` (line 45) |
| Where should the model run? The general concept | `governance` (line 90) |
| Where should the model run? An intuitive example | `governance` (line 105) |
| Deployment options: comparison | `governance` (line 111) |
| Hybrid architectures | __new__ |
| Providers, catalogues, and selection criteria | __new__ |
| **The Ecosystem of APIs for Generative AI** | __new__ |
| The general concept: _Chat Completion_ as a request–response API | `llmaas` (line 573) |
| Chat Completion: an intuitive example | `llmaas` (line 589) |
| What to expect in general from Web APIs of LLM-as-a-Service providers? | `llmaas` (line 618) |
| About LLMs' Web API Compatibility (pt. 1) – OpenAI family | `llmaas` (line 640) |
| About LLMs' Web API Compatibility (pt. 2) – OpenAI-compatible APIs | `llmaas` (line 653) |
| About LLMs' Web API Compatibility (pt. 3) – Anthropic Claude API | `llmaas` (line 671) |
| About LLMs' Web API Compatibility (pt. 4) – Google API | `llmaas` (line 683) |
| About LLMs' Web API Compatibility (pt. 5) – Cloud-provider APIs | `llmaas` (line 696) |
| About LLMs' Web API Compatibility (pt. 6) – Gateways and routers | `llmaas` (line 708) |
| About LLMs' Web API Compatibility (pt. 7) – Wrap Up | `llmaas` (line 718) |
| Analogies and differences among providers' APIs | `llmaas` (line 1273) |
| **Integrating an LLM into an Application** | __new__ |
| About client libraries | `llmaas` (line 734) |
| About LangChain | `prompting` (line 363) |
| Which interface for which use case? | __new__ |
| **The OpenAI APIs in Practice** | __new__ |
| OpenAI's Chat Completion API with Python | `llmaas` (line 749) |
| Example 1: Sync CLI Chat (pt. 1) | `llmaas` (line 779) |
| Example 1: Sync CLI Chat (pt. 2) | `llmaas` (line 802) |
| Example 1: Sync CLI Chat (pt. 3) | `llmaas` (line 820) |
| Guided exercise 1: same program, different providers | __new__ |
| Example 2: Async CLI Chat with Streaming | `llmaas` (line 1025) |
| Example 3: CLI Chat with Attachments, via the Responses API (pt. 1) | `llmaas` (line 1111) |
| Example 3: CLI Chat with Attachments, via the Responses API (pt. 2) | `llmaas` (line 1131) |
| Example 3: CLI Chat with Attachments, via the Responses API (pt. 3) | `llmaas` (line 1146) |
| Example 3: CLI Chat with Attachments, via the Responses API (pt. 4) | `llmaas` (line 1161) |
| Which endpoint for which scenario? | __new__ |
| When things go wrong: errors, rate limits, and costs | `llmaas` (line 1420) |
| **From Text Generation to Processes** | __new__ |
| About Structured Output | `prompting` (line 109) |
| How to represent target structured format to the model? | `prompting` (line 117) |
| How is structured output _enforced_? (pt. 1) | `prompting` (line 189) |
| Example 4: Structured Output with Pydantic Classes (pt. 1) | `prompting` (line 235) |
| Example 4: Structured Output with Pydantic Classes (pt. 2) | `prompting` (line 257) |
| Example 4: Structured Output with Pydantic Classes (pt. 3) | `prompting` (line 281) |
| Example 4: Structured Output with Pydantic Classes (pt. 4, cont.) | `prompting` (line 315) |
| Guided exercise 2: changing the output schema | __new__ |
| Tools: the general concept | `agents` (line 105) |
| Tools: an intuitive example | `agents` (line 130) |
| Tools as the bridge to enterprise systems | __new__ |
| The ReAct loop: the general concept | `agents` (line 250) |
| The ReAct loop: an intuitive example | `agents` (line 268) |
| The spectrum of autonomy | `agents` (line 83) |
| Workflows: the general concept | `workflows` (line 42) |
| Kinds of steps | `workflows` (line 56) |
| Workflows: an intuitive example | `workflows` (line 72) |
| Workflow patterns: a catalogue | `workflows` (line 376) |
| Tools change the risk profile | `agents` (line 699) |
| Prompt injection | `agents` (line 717) |
| Mitigations | `agents` (line 742) |
| **Fundamentals of Agentic AI** | __new__ |
| Agents built around LLMs: the agentic metaphor | `agents` (line 56) |
| LLMs are not agents by themselves | `agents` (line 66) |
| Skills: the general concept | `skills` (line 42) |
| Skills: an intuitive example | `skills` (line 63) |
| Skills vs. prompts, tools, MCP servers, and workflows | `skills` (line 76) |
| Anatomy of a skill | `skills` (line 152) |
| **Reviewing a PhD application** | __new__ |
| Skills as a context-loading mechanism: progressive disclosure | `skills` (line 209) |
| Skills and tools: scripts vs. instructions | `skills` (line 223) |
| Where do skills run? | __new__ |
| Skills: the technological landscape | `skills` (line 242) |
| Example 5: Using Skills from OpenClaw | `skills` (line 346) |
| Skills in your own agents | `skills` (line 399) |
| Security of skills: threats | `skills` (line 896) |
| Security of skills: mitigations | `skills` (line 912) |
| Architectural and decision implications | __new__ |
| Which technique? Trade-offs | `governance` (line 639) |
| **Building an Agent, Incrementally** | __new__ |
| Example 6: an Agent from Scratch, with OpenAI's Client (pt. 1) | `agents` (line 278) |
| Example 6: an Agent from Scratch, with OpenAI's Client (pt. 2) | `agents` (line 295) |
| Example 6: an Agent from Scratch, with OpenAI's Client (pt. 3) | `agents` (line 314) |
| The Model Context Protocol (MCP): the general concept | `agents` (line 437) |
| MCP: an intuitive example | `agents` (line 459) |
| Example 7: Simple Tools as an MCP Server (pt. 1) | `agents` (line 485) |
| Example 7: Simple Tools as an MCP Server (pt. 2) | `agents` (line 505) |
| Local tools vs. MCP servers | `agents` (line 546) |
| Wrap-up | __new__ |

## `content/governance/_index.md`

| Slide | Origin |
|---|---|
| **Sovereign and Open Alternatives to Large LLMs** | __new__ |
| Outline | __new__ |
| Recall: where should the model run? | __new__ |
| **The Landscape of Open and Open-Weight Models** | __new__ |
| Model zoos (pt. 1): the general concept | `llmaas` (line 55) |
| Model zoos (pt. 2): the technological landscape | `llmaas` (line 77) |
| Model zoos (pt. 3): what next? | `llmaas` (line 98) |
| Key parameters, at a glance | __new__ |
| Guided tour: the same model, in three zoos | __new__ |
| What makes a model _"open"_? The general concept | `governance` (line 328) |
| Openness is _graded_, not binary | `governance` (line 344) |
| The OSI Open Source AI Definition (OSAID 1.0, Oct. 2024) | `governance` (line 361) |
| The licensing landscape (as of September 2026) | `governance` (line 380) |
| The licensing landscape (as of September 2026) | `governance` (line 391) |
| What do restrictive licenses mean in practice? | `governance` (line 405) |
| Derivatives: fine-tunes, distillation, merges, data, outputs | `governance` (line 420) |
| Open models under the EU AI Act | `governance` (line 438) |
| **Models, Providers, and Platforms** | __new__ |
| Primary providers, aggregators, and marketplaces | __new__ |
| About Open Router (cf. <https://openrouter.ai/>), pt. 1 | `llmaas` (line 414) |
| About Open Router (cf. <https://openrouter.ai/>), pt. 2 | `llmaas` (line 432) |
| On-cloud Example with Open Router (pt. 1) | `llmaas` (line 454) |
| On-cloud Example with Open Router (pt. 2) | `llmaas` (line 467) |
| On-cloud Example with Open Router (pt. 3) | `llmaas` (line 475) |
| On-cloud Example with Open Router (pt. 4) | `llmaas` (line 488) |
| On-cloud Example with Open Router (pt. 4, cont.) | `llmaas` (line 501) |
| One client, _many_ free providers | `free-access` (line 71) |
| Portability of applications, and dependency on the provider | __new__ |
| **Local Execution and Infrastructural Sovereignty** | __new__ |
| About Ollama (cf. <https://ollama.com/>) | `llmaas` (line 121) |
| On-premise Example with Ollama pt. 1 | `llmaas` (line 137) |
| On-premise Example with Ollama pt. 2 | `llmaas` (line 159) |
| On-premise Example with Ollama pt. 3 | `llmaas` (line 169) |
| On-premise Example with Ollama pt. 4 | `llmaas` (line 209) |
| Running models locally: what does it take? (pt. 1) | `llmaas` (line 264) |
| Running models locally: what does it take? (pt. 2) | `llmaas` (line 286) |
| Quantization (pt. 1): the concept | `llmaas` (line 308) |
| Decoding GGUF quantization labels (as seen in Ollama's tags) | `llmaas` (line 337) |
| Running models locally: hardware & software (pt. 1) | `llmaas` (line 351) |
| Running models locally: hardware & software (pt. 2) | `llmaas` (line 371) |
| Deployment options: a worked cost example | `governance` (line 131) |
| When is local execution _really_ sustainable? | __new__ |
| **European Digital Sovereignty** | __new__ |
| "Sovereign AI": a framing, and its limits | `governance` (line 459) |
| The gap, in numbers | `governance` (line 477) |
| European initiatives | `governance` (line 508) |
| Infrastructure and policy | `governance` (line 521) |
| Recurring issues of European open models | `governance` (line 547) |
| **The Italian Landscape** | __new__ |
| Italian initiatives | `governance` (line 494) |
| How good are Italian models? | `governance` (line 533) |
| **How to Evaluate an LLM** | __new__ |
| Which model? The general concept | `governance` (line 671) |
| Model documentation: model cards, system cards, datasheets | `governance` (line 685) |
| How to evaluate models? | `governance` (line 817) |
| What to evaluate: quality, efficiency, robustness, specialisation | __new__ |
| Measuring _general_ capabilities: frontier benchmarks | `governance` (line 698) |
| Frontier benchmarks: METR's time horizons | `governance` (line 726) |
| Frontier benchmarks: the ARC-AGI-3 leaderboard | `governance` (line 758) |
| Agentic benchmarks: how good are agents, in general? | `agents` (line 621) |
| A realistic evaluation, for an organisation | __new__ |
| Validating generative software: the general concept | `validating` (line 30) |
| Validating generative software: an intuitive example | `validating` (line 54) |
| LLM-as-a-Judge | `validating` (line 62) |
| How much evidence? | `validating` (line 84) |
| **Decision Criteria for Adoption** | __new__ |
| Commercial cloud, European provider, or on-premise? | __new__ |
| EU AI Act: a risk-based approach | `governance` (line 219) |
| How to choose a model for a given task? | `governance` (line 769) |
| Engineering take-aways: pre-adoption checklist | `governance` (line 563) |
| Engineering take-aways: three scenarios | `governance` (line 583) |
| Guided exercise 1: comparing models on the running example | __new__ |
| Guided exercise 2: a decision record, in groups | __new__ |
| Wrap-up | __new__ |
