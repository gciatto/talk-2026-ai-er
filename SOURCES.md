# SOURCES.md

Provenance of every slide of the two lectures: either __new__ (written for this talk), or copied (then possibly trimmed or adapted)
from the lecture of [slides-module2](https://github.com/unibo-fc-isi-iag/slides-module2) named in the _Origin_ column,
at the given line of its `content/<lecture>/_index.md`, as of commit
[`67093ed`](https://github.com/unibo-fc-isi-iag/slides-module2/commit/67093ed) (October 2026).

Section title slides (in bold) are new, unless stated otherwise. Copied slides keep their `{{< slide id >}}` anchor, when they had one;
examples taken from other lectures are renumbered (Examples 4–7 of `llmaas`), but keep their original `lab-snippets` coordinates in commands.
The slides and editable diagrams are translated into Italian; code and actual example outputs remain in English.
To port a later fix of slides-module2 here, locate the slide by its Italian title below, then use the original English heading to find it upstream (line numbers drift).

## `content/llmaas/_index.md`

| Slide (Italian) | Original heading (English) | Origin |
|---|---|---|
| **LLM-as-a-Service: API, tool e agenti** | **LLM-as-a-Service: APIs, Tools, and Agents** | __new__ |
| Programma | Outline | __new__ |
| **LLM-as-a-Service** | **LLM-as-a-Service** | __new__ |
| Le cinque dimensioni del compromesso | The five dimensions of the trade-off | `governance` (line 45) |
| Dove eseguire il modello? Il concetto generale | Where should the model run? The general concept | `governance` (line 90) |
| Dove eseguire il modello? Un esempio intuitivo | Where should the model run? An intuitive example | `governance` (line 105) |
| Opzioni di deployment a confronto | Deployment options: comparison | `governance` (line 111) |
| Architetture ibride | Hybrid architectures | __new__ |
| Fornitori, cataloghi e criteri di scelta | Providers, catalogues, and selection criteria | __new__ |
| **L'ecosistema delle API per l'IA generativa** | **The Ecosystem of APIs for Generative AI** | __new__ |
| Il concetto generale: _Chat Completion_ come API richiesta–risposta | The general concept: _Chat Completion_ as a request–response API | `llmaas` (line 573) |
| Chat Completion: un esempio intuitivo | Chat Completion: an intuitive example | `llmaas` (line 589) |
| Cosa aspettarsi dalle API Web dei fornitori di LLM-as-a-Service? | What to expect in general from Web APIs of LLM-as-a-Service providers? | `llmaas` (line 618) |
| Compatibilità delle API Web degli LLM (pt. 1) – Famiglia OpenAI | About LLMs' Web API Compatibility (pt. 1) – OpenAI family | `llmaas` (line 640) |
| Compatibilità delle API Web degli LLM (pt. 2) – API compatibili con OpenAI | About LLMs' Web API Compatibility (pt. 2) – OpenAI-compatible APIs | `llmaas` (line 653) |
| Compatibilità delle API Web degli LLM (pt. 3) – API di Anthropic Claude | About LLMs' Web API Compatibility (pt. 3) – Anthropic Claude API | `llmaas` (line 671) |
| Compatibilità delle API Web degli LLM (pt. 4) – API di Google | About LLMs' Web API Compatibility (pt. 4) – Google API | `llmaas` (line 683) |
| Compatibilità delle API Web degli LLM (pt. 5) – API dei fornitori cloud | About LLMs' Web API Compatibility (pt. 5) – Cloud-provider APIs | `llmaas` (line 696) |
| Compatibilità delle API Web degli LLM (pt. 6) – Gateway e router | About LLMs' Web API Compatibility (pt. 6) – Gateways and routers | `llmaas` (line 708) |
| Compatibilità delle API Web degli LLM (pt. 7) – Riepilogo | About LLMs' Web API Compatibility (pt. 7) – Wrap Up | `llmaas` (line 718) |
| Analogie e differenze tra le API dei fornitori | Analogies and differences among providers' APIs | `llmaas` (line 1273) |
| **Integrare un LLM in un'applicazione** | **Integrating an LLM into an Application** | __new__ |
| Le librerie client | About client libraries | `llmaas` (line 734) |
| LangChain | About LangChain | `prompting` (line 363) |
| Quale interfaccia per quale caso d'uso? | Which interface for which use case? | __new__ |
| **Le API di OpenAI in pratica** | **The OpenAI APIs in Practice** | __new__ |
| API Chat Completion di OpenAI con Python | OpenAI's Chat Completion API with Python | `llmaas` (line 749) |
| Esempio 1: chat CLI sincrona (pt. 1) | Example 1: Sync CLI Chat (pt. 1) | `llmaas` (line 779) |
| Esempio 1: chat CLI sincrona (pt. 2) | Example 1: Sync CLI Chat (pt. 2) | `llmaas` (line 802) |
| Esempio 1: chat CLI sincrona (pt. 3) | Example 1: Sync CLI Chat (pt. 3) | `llmaas` (line 820) |
| La cronologia cresce a ogni turno (e con lei i costi) | Context management: the general concept + an intuitive example | `prompting` (lines 853, 876), condensed |
| Esercizio guidato 1: stesso programma, diversi fornitori | Guided exercise 1: same program, different providers | __new__ |
| Esempio 2: chat CLI asincrona con streaming | Example 2: Async CLI Chat with Streaming | `llmaas` (line 1025) |
| Esempio 3: chat CLI con allegati tramite API Responses (pt. 1) | Example 3: CLI Chat with Attachments, via the Responses API (pt. 1) | `llmaas` (line 1111) |
| Esempio 3: chat CLI con allegati tramite API Responses (pt. 2) | Example 3: CLI Chat with Attachments, via the Responses API (pt. 2) | `llmaas` (line 1131) |
| Esempio 3: chat CLI con allegati tramite API Responses (pt. 3) | Example 3: CLI Chat with Attachments, via the Responses API (pt. 3) | `llmaas` (line 1146) |
| Esempio 3: chat CLI con allegati tramite API Responses (pt. 4) | Example 3: CLI Chat with Attachments, via the Responses API (pt. 4) | `llmaas` (line 1161) |
| Quale endpoint per quale scenario? | Which endpoint for which scenario? | __new__ |
| Modelli di ragionamento: qualità in cambio di latenza e costi | Reasoning models: the general concept + an intuitive example | `prompting` (lines 737, 752), condensed |
| Quando qualcosa va storto: errori, limiti di utilizzo e costi | When things go wrong: errors, rate limits, and costs | `llmaas` (line 1420) |
| **Dalla generazione di testo ai processi** | **From Text Generation to Processes** | __new__ |
| Output strutturato | About Structured Output | `prompting` (line 109) |
| Come descrivere al modello il formato strutturato desiderato? | How to represent target structured format to the model? | `prompting` (line 117) |
| Come si _impone_ l'output strutturato? (pt. 1) | How is structured output _enforced_? (pt. 1) | `prompting` (line 189) |
| Esempio 4: output strutturato con classi Pydantic (pt. 1) | Example 4: Structured Output with Pydantic Classes (pt. 1) | `prompting` (line 235) |
| Esempio 4: output strutturato con classi Pydantic (pt. 2) | Example 4: Structured Output with Pydantic Classes (pt. 2) | `prompting` (line 257) |
| Esempio 4: output strutturato con classi Pydantic (pt. 3) | Example 4: Structured Output with Pydantic Classes (pt. 3) | `prompting` (line 281) |
| Esempio 4: output strutturato con classi Pydantic (pt. 4, continua) | Example 4: Structured Output with Pydantic Classes (pt. 4, cont.) | `prompting` (line 315) |
| Esercizio guidato 2: modificare lo schema di output | Guided exercise 2: changing the output schema | __new__ |
| Tool: il concetto generale | Tools: the general concept | `agents` (line 105) |
| Tool: un esempio intuitivo | Tools: an intuitive example | `agents` (line 130) |
| I tool come ponte verso i sistemi aziendali | Tools as the bridge to enterprise systems | __new__ |
| Il ciclo ReAct: il concetto generale | The ReAct loop: the general concept | `agents` (line 250) |
| Il ciclo ReAct: un esempio intuitivo | The ReAct loop: an intuitive example | `agents` (line 268) |
| Lo spettro dell'autonomia | The spectrum of autonomy | `agents` (line 83) |
| Workflow: il concetto generale | Workflows: the general concept | `workflows` (line 42) |
| Tipi di passi | Kinds of steps | `workflows` (line 56) |
| Workflow: un esempio intuitivo | Workflows: an intuitive example | `workflows` (line 72) |
| Pattern di workflow: un catalogo | Workflow patterns: a catalogue | `workflows` (line 376) |
| I tool cambiano il profilo di rischio | Tools change the risk profile | `agents` (line 699) |
| Prompt injection | Prompt injection | `agents` (line 717); trifecta Venn diagram __new__ |
| Mitigazioni | Mitigations | `agents` (line 742) |
| L'LLM propone, i critici verificano: LLM-Modulo | LLMs can't plan, but can help planning: LLM-Modulo | `agents` (line 674), merged with the previous slide |
| **Fondamenti di IA agentica** | **Fundamentals of Agentic AI** | __new__ |
| Agenti basati su LLM: la metafora agentica | Agents built around LLMs: the agentic metaphor | `agents` (line 56) |
| Gli LLM da soli non sono agenti | LLMs are not agents by themselves | `agents` (line 66) |
| Skill: il concetto generale | Skills: the general concept | `skills` (line 42) |
| Skill: un esempio intuitivo | Skills: an intuitive example | `skills` (line 63) |
| Skill a confronto con prompt, tool, server MCP e workflow | Skills vs. prompts, tools, MCP servers, and workflows | `skills` (line 76) |
| Anatomia di una skill | Anatomy of a skill | `skills` (line 152) |
| **Valutare una domanda di dottorato** | **Reviewing a PhD application** | __new__ |
| Caricamento progressivo del contesto delle skill | Skills as a context-loading mechanism: progressive disclosure | `skills` (line 209) |
| Skill e tool: script e istruzioni | Skills and tools: scripts vs. instructions | `skills` (line 223) |
| Dove si eseguono le skill? | Where do skills run? | __new__ |
| Skill: il panorama tecnologico | Skills: the technological landscape | `skills` (line 242) |
| Esempio 5: usare skill da OpenClaw | Example 5: Using Skills from OpenClaw | `skills` (line 346) |
| Skill nei propri agenti | Skills in your own agents | `skills` (line 399) |
| Sicurezza delle skill: minacce | Security of skills: threats | `skills` (line 896) |
| Sicurezza delle skill: mitigazioni | Security of skills: mitigations | `skills` (line 912) |
| Implicazioni architetturali e decisionali | Architectural and decision implications | __new__ |
| Quale tecnica? Compromessi | Which technique? Trade-offs | `governance` (line 639) |
| **Costruire un agente, passo dopo passo** | **Building an Agent, Incrementally** | __new__ |
| Esempio 6: un agente da zero con il client OpenAI (pt. 1) | Example 6: an Agent from Scratch, with OpenAI's Client (pt. 1) | `agents` (line 278) |
| Esempio 6: un agente da zero con il client OpenAI (pt. 2) | Example 6: an Agent from Scratch, with OpenAI's Client (pt. 2) | `agents` (line 295) |
| Esempio 6: un agente da zero con il client OpenAI (pt. 3) | Example 6: an Agent from Scratch, with OpenAI's Client (pt. 3) | `agents` (line 314) |
| Model Context Protocol (MCP): il concetto generale | The Model Context Protocol (MCP): the general concept | `agents` (line 437) |
| MCP: un esempio intuitivo | MCP: an intuitive example | `agents` (line 459) |
| Esempio 7: tool semplici come server MCP (pt. 1) | Example 7: Simple Tools as an MCP Server (pt. 1) | `agents` (line 485) |
| Esempio 7: tool semplici come server MCP (pt. 2) | Example 7: Simple Tools as an MCP Server (pt. 2) | `agents` (line 505) |
| Tool locali e server MCP a confronto | Local tools vs. MCP servers | `agents` (line 546) |
| Riepilogo | Wrap-up | __new__ |

## `content/governance/_index.md`

| Slide (Italian) | Original heading (English) | Origin |
|---|---|---|
| **Alternative sovrane e aperte ai grandi LLM** | **Sovereign and Open Alternatives to Large LLMs** | __new__ |
| Programma | Outline | __new__ |
| Riprendiamo: dove deve essere eseguito il modello? | Recall: where should the model run? | __new__ |
| **Il panorama dei modelli aperti e a pesi aperti** | **The Landscape of Open and Open-Weight Models** | __new__ |
| Cataloghi di modelli (pt. 1): il concetto generale | Model zoos (pt. 1): the general concept | `llmaas` (line 55) |
| Cataloghi di modelli (pt. 2): il panorama tecnologico | Model zoos (pt. 2): the technological landscape | `llmaas` (line 77) |
| Cataloghi di modelli (pt. 3): e poi? | Model zoos (pt. 3): what next? | `llmaas` (line 98) |
| I parametri chiave, a colpo d'occhio | Key parameters, at a glance | __new__ |
| Esplorazione guidata: lo stesso modello in tre cataloghi | Guided tour: the same model, in three zoos | __new__ |
| Cosa rende un modello _"aperto"_? Il concetto generale | What makes a model _"open"_? The general concept | `governance` (line 328) |
| L'apertura ha _gradi_, non è binaria | Openness is _graded_, not binary | `governance` (line 344) |
| La definizione OSI di IA open source (OSAID 1.0, ott. 2024) | The OSI Open Source AI Definition (OSAID 1.0, Oct. 2024) | `governance` (line 361) |
| Il panorama delle licenze (a settembre 2026) | The licensing landscape (as of September 2026) | `governance` (line 380) |
| Il panorama delle licenze (a settembre 2026) | The licensing landscape (as of September 2026) | `governance` (line 391) |
| Cosa significano in pratica le licenze restrittive? | What do restrictive licenses mean in practice? | `governance` (line 405) |
| Derivati: fine-tuning, distillazione, fusioni, dati, output | Derivatives: fine-tunes, distillation, merges, data, outputs | `governance` (line 420) |
| I modelli aperti nell'AI Act dell'UE | Open models under the EU AI Act | `governance` (line 438) |
| **Modelli, fornitori e piattaforme** | **Models, Providers, and Platforms** | __new__ |
| Fornitori primari, aggregatori e marketplace | Primary providers, aggregators, and marketplaces | __new__ |
| Open Router (cfr. <https://openrouter.ai/>), pt. 1 | About Open Router (cf. <https://openrouter.ai/>), pt. 1 | `llmaas` (line 414) |
| Open Router (cfr. <https://openrouter.ai/>), pt. 2 | About Open Router (cf. <https://openrouter.ai/>), pt. 2 | `llmaas` (line 432) |
| Esempio cloud con Open Router (pt. 1) | On-cloud Example with Open Router (pt. 1) | `llmaas` (line 454) |
| Esempio cloud con Open Router (pt. 2) | On-cloud Example with Open Router (pt. 2) | `llmaas` (line 467) |
| Esempio cloud con Open Router (pt. 3) | On-cloud Example with Open Router (pt. 3) | `llmaas` (line 475) |
| Esempio cloud con Open Router (pt. 4) | On-cloud Example with Open Router (pt. 4) | `llmaas` (line 488) |
| Esempio cloud con Open Router (pt. 4, continua) | On-cloud Example with Open Router (pt. 4, cont.) | `llmaas` (line 501) |
| Un client, _molti_ fornitori gratuiti | One client, _many_ free providers | `free-access` (line 71) |
| Portabilità delle applicazioni e dipendenza dal fornitore | Portability of applications, and dependency on the provider | __new__ |
| **Esecuzione locale e sovranità infrastrutturale** | **Local Execution and Infrastructural Sovereignty** | __new__ |
| Ollama (cfr. <https://ollama.com/>) | About Ollama (cf. <https://ollama.com/>) | `llmaas` (line 121) |
| Esempio on-premise con Ollama, pt. 1 | On-premise Example with Ollama pt. 1 | `llmaas` (line 137) |
| Esempio on-premise con Ollama, pt. 2 | On-premise Example with Ollama pt. 2 | `llmaas` (line 159) |
| Esempio on-premise con Ollama, pt. 3 | On-premise Example with Ollama pt. 3 | `llmaas` (line 169) |
| Esempio on-premise con Ollama, pt. 4 | On-premise Example with Ollama pt. 4 | `llmaas` (line 209) |
| Eseguire modelli localmente: cosa serve? (pt. 1) | Running models locally: what does it take? (pt. 1) | `llmaas` (line 264) |
| Eseguire modelli localmente: cosa serve? (pt. 2) | Running models locally: what does it take? (pt. 2) | `llmaas` (line 286) |
| Quantizzazione (pt. 1): il concetto | Quantization (pt. 1): the concept | `llmaas` (line 308) |
| Decifrare le etichette di quantizzazione GGUF (nei tag di Ollama) | Decoding GGUF quantization labels (as seen in Ollama's tags) | `llmaas` (line 337) |
| Eseguire modelli localmente: hardware e software (pt. 1) | Running models locally: hardware & software (pt. 1) | `llmaas` (line 351) |
| Eseguire modelli localmente: hardware e software (pt. 2) | Running models locally: hardware & software (pt. 2) | `llmaas` (line 371) |
| Opzioni di deployment: un esempio di calcolo dei costi | Deployment options: a worked cost example | `governance` (line 131) |
| Il punto di pareggio, a colpo d'occhio | — | __new__ |
| Quando l'esecuzione locale è _davvero_ sostenibile? | When is local execution _really_ sustainable? | __new__ |
| **Sovranità digitale europea** | **European Digital Sovereignty** | __new__ |
| "IA sovrana": una narrazione e i suoi limiti | "Sovereign AI": a framing, and its limits | `governance` (line 459) |
| Il divario, in numeri | The gap, in numbers | `governance` (line 477) |
| Iniziative europee | European initiatives | `governance` (line 508) |
| Infrastrutture e politiche pubbliche | Infrastructure and policy | `governance` (line 521) |
| Problemi ricorrenti dei modelli aperti europei | Recurring issues of European open models | `governance` (line 547) |
| **Il panorama italiano** | **The Italian Landscape** | __new__ |
| Iniziative italiane | Italian initiatives | `governance` (line 494) |
| Quanto sono validi i modelli italiani? | How good are Italian models? | `governance` (line 533) |
| **Come valutare un LLM** | **How to Evaluate an LLM** | __new__ |
| Quale modello? Il concetto generale | Which model? The general concept | `governance` (line 671) |
| Documentazione dei modelli: model card, system card, datasheet | Model documentation: model cards, system cards, datasheets | `governance` (line 685) |
| Come valutare i modelli? | How to evaluate models? | `governance` (line 817) |
| Cosa valutare: qualità, efficienza, robustezza, specializzazione | What to evaluate: quality, efficiency, robustness, specialisation | __new__ |
| Misurare capacità _generali_: benchmark di frontiera | Measuring _general_ capabilities: frontier benchmarks | `governance` (line 698) |
| Benchmark di frontiera: gli orizzonti temporali di METR | Frontier benchmarks: METR's time horizons | `governance` (line 726) |
| Benchmark di frontiera: la classifica ARC-AGI-3 | Frontier benchmarks: the ARC-AGI-3 leaderboard | `governance` (line 758) |
| Benchmark agentici: quanto sono validi gli agenti in generale? | Agentic benchmarks: how good are agents, in general? | `agents` (line 621) |
| Una valutazione realistica per un'organizzazione | A realistic evaluation, for an organisation | __new__ |
| Validare software generativo: il concetto generale | Validating generative software: the general concept | `validating` (line 30) |
| Validare software generativo: un esempio intuitivo | Validating generative software: an intuitive example | `validating` (line 54) |
| LLM-as-a-Judge | LLM-as-a-Judge | `validating` (line 62) |
| Quante evidenze servono? | How much evidence? | `validating` (line 84) |
| **Criteri di scelta per l'adozione** | **Decision Criteria for Adoption** | __new__ |
| Cloud commerciale, fornitore europeo o on-premise? | Commercial cloud, European provider, or on-premise? | __new__ |
| La regolamentazione: il quadro generale | Regulation: the general picture | `governance` (line 164) |
| AI Act dell'UE: un approccio basato sul rischio | EU AI Act: a risk-based approach | `governance` (line 219) |
| Come scegliere un modello per un compito? | How to choose a model for a given task? | `governance` (line 769) |
| Indicazioni per l'ingegneria: controlli prima dell'adozione | Engineering take-aways: pre-adoption checklist | `governance` (line 563) |
| Indicazioni per l'ingegneria: tre scenari | Engineering take-aways: three scenarios | `governance` (line 583) |
| Esercizio guidato 1: confrontare i modelli sull'esempio ricorrente | Guided exercise 1: comparing models on the running example | __new__ |
| Esercizio guidato 2: motivare una decisione per iscritto, in gruppi | Guided exercise 2: a decision record, in groups | __new__ |
| Riepilogo | Wrap-up | __new__ |
