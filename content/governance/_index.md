
+++

title = "[AI-ER] Sovereign and Open Alternatives to Large LLMs"
description = "Open models, providers, local execution, European and Italian initiatives, model evaluation and selection"
outputs = ["Reveal"]

+++

# Sovereign and Open Alternatives to Large LLMs

{{% import path="reusable/footer.md" %}}

---

## Outline

1. [The landscape of open and open-weight models](#/open-landscape)
    + model zoos (Hugging Face, Ollama, Open Router); key parameters; what "open" means; licenses
2. [Models, providers, and platforms](#/providers-landscape): who offers what
    + primary providers, aggregators, marketplaces; Open Router; portability and lock-in
3. [Local execution](#/local) and infrastructural sovereignty
    + Ollama; hardware requirements; when is it sustainable?
4. [European digital sovereignty](#/sovereignty): initiatives, and their maturity
5. [The Italian landscape](#/italy)
6. [How to evaluate an LLM](#/evaluation)
    + benchmarks, and their limits; realistic evaluation for an organisation
7. [Decision criteria for adoption](#/decision)
    + cloud, European provider, or on-premise? How to build a shortlist

---

## Recall: where should the model run?

- From the [previous lecture](../llmaas/#/deployment-options), three _deployment options_:
    1. __proprietary cloud API__ (e.g. OpenAI, Anthropic, Google)
    2. __open model, hosted by a third party__ (e.g. via Open Router, an EU cloud provider)
    3. __on-premise__ (e.g. via Ollama, vLLM)
- ... to be chosen according to _five dimensions_: quality, cost, control, compliance, time-to-market (cf. the [comparison](../llmaas/#/deployment-options))
- Options 2 and 3 require __open-weight__ models: _which_ ones, from _whom_, under which _conditions_, and how _good_ are they?

---

{{< slide id="open-landscape" >}}

# The Landscape of Open and Open-Weight Models

---

{{% section %}}

{{< slide id="model-zoos" >}}

## Model zoos (pt. 1): the general concept

- A __model zoo__ (a.k.a. model _hub_, _catalog_, _library_) is a _searchable collection_ of models made available by some provider, each one with:
    + a __name__ (and _versions_/_variants_), to be used in API calls or downloads
    + a __model card__: a document describing the model's _purpose_, _training data_, _evaluation_ results, _limitations_, and _intended uses_ (cf. [Mitchell et al., 2019](https://arxiv.org/abs/1810.03993))
    + _technical metadata_: size, _context window_, supported _modalities_ (text, image, audio), support for _tools_ / _structured output_ / _reasoning_
    + _economic_ and _legal_ metadata: price per token (for on-cloud zoos), __license__ (for downloadable models)

- Zoos are the _entry point_ for __model selection__, which is a recurring activity in GenAI engineering (cf. [GenAI lifecycle](https://unibo-fc-isi-iag.github.io/slides-module2/genai/#/genai-workflow))


### Example

- [Hugging Face Hub](https://huggingface.co/models) is a model zoo with millions of models for any task and any modality,
    - each one with a model card, technical metadata, and license information
    - e.g. filter on "Gemma" to see the [Gemma family](https://huggingface.co/models?sort=trending&search=gemma) of LLMs, then click on one to see the card

    {{< image src="./hugging-face-model-zoo.png" max-h="28vh" alt="Hugging Face Gemma model card screenshot" >}}

---

## Model zoos (pt. 2): the technological landscape

| | [Hugging Face Hub](https://huggingface.co/models) | [Ollama library](https://ollama.com/models) | [Open Router](https://openrouter.ai/models) |
|---|---|---|---|
| __Kind__ | download (+ some hosted inference) | download, run locally | on-cloud, pay per token |
| __Size__ | millions of models (any task, any modality) | hundreds of curated LLMs | hundreds of LLMs from dozens of providers |
| __Naming__ | `organization/model-name` | `model:variant` (e.g. `gemma4:e2b`) | `provider/model[:tier]` (e.g. `google/gemma-4-26b-a4b-it:free`) |
| __Variants__ | separate repositories (e.g. `...-GGUF`, `...-AWQ`) | _tags_ for size and quantization | one entry per model, served by several providers |
| __Metadata__ | full model card, files, license, community | size, context, capabilities, quantization | price, context, latency/throughput stats, supported parameters |
| __License__ | per model (shown as a tag) | per model (in the model page) | provider's _terms of service_ apply |

### Recurring naming conventions (worth decoding)

- _family_ + _version_: `llama-3.1`, `qwen3`, `gemma-4`
- _size_: `8b`, `70b`, or `26b-a4b` (MoE: total vs. active parameters), or `e2b` (_effective_ 2B: Gemma's ~2.3B parameters used in computation, 5.1B including per-layer embeddings)
- _flavour_: `base` (pure next-token predictor) vs. `instruct` / `it` / `chat` (tuned to follow instructions: __this is what you want__ for Chat Completion), or `coder`, `vl` / `vision`, `thinking`
- _quantization_ / _format_: `Q4_K_M`, `GGUF`, `AWQ`, `fp8`
- _date_ / _snapshot_: `gpt-4o-2024-08-06` (pin it for _reproducibility_!)

---

## Model zoos (pt. 3): what next?

- Zoos tell you _which_ models exist, and what they _claim_ to be capable of
- Need a model supporting _tools_ or _structured output_?
    + Ollama: look for the `tools`, `thinking`, `vision` badges (e.g. [filter by tools](https://ollama.com/search?c=tools))
    + Open Router: filter the models page by _supported parameters_ (e.g. `tools`, `structured_outputs`, `response_format`)
- __Choosing__ the right model for a given task, and __evaluating__ it on _evidence_, are decisions with consequences on quality, cost, control, and compliance
    + hence, they are discussed [later](#/model-selection), along with a guided exercise on comparing models on the running example

---

{{< slide id="key-parameters" >}}

## Key parameters, at a glance

{{% small "85%" %}}

| Parameter | Where to find it | Why it matters |
|---|---|---|
| __Size__ (parameters; _total_ vs. _active_, for MoE) | name (e.g. `26b-a4b`), model card | quality (roughly), memory and hardware needed, speed |
| __Cost__ | price per 1M input / output tokens (cloud); hardware + energy (local) | operating budget; varies by provider, for the _same_ model |
| __License__ | model card, license file | what you may _legally_ do: commercial use, fine-tuning, hosting, geography |
| __Context window__ | model card, zoo metadata | how many documents / how much history fit in a request |
| __Modalities__ | model card, badges (e.g. `vision`) | text only? images (e.g. scanned documents)? audio? |
| __Operational capabilities__ | badges, _supported parameters_ (e.g. `tools`, `structured_outputs`) | can it call tools, produce structured output, _reason_? |
| __Languages__ | model card, technical report | is Italian supported _well_ (not just "supported")? |
| __Knowledge cut-off__, __release date__ | model card | how up to date it is; how long before it is _deprecated_ |

{{% /small %}}

- Zoo metadata is a _claim_ by the provider: __verify__ what matters for your use case (cf. [evaluation](#/evaluation))

---

{{< slide id="zoo-tour" >}}

## Guided tour: the same model, in three zoos

> __Goal__: learn to read model zoos, by looking for the _same_ open-weight model (e.g. Gemma 4) in all three

1. [Hugging Face Hub](https://huggingface.co/models?search=gemma-4): find the model card of `google/gemma-4-26b-a4b-it`
    + license? languages? context window? evaluations? how many _derived_ models (fine-tunes, quantizations) exist?
2. [Ollama library](https://ollama.com/library/gemma4): look at the _tags_ of `gemma4`
    + which sizes and quantizations? how many GB each? which _badges_ (`vision`, `tools`, `thinking`)?
3. [Open Router](https://openrouter.ai/models?q=gemma-4): look at `google/gemma-4-26b-a4b-it`
    + how many _providers_ serve it? at which prices, latency, throughput? is there a `:free` variant? with which limits?

{{% fragment %}}
> Same _weights_, three very different _offers_: download it (and run it yourself), or pay per token to one of _several_ providers. This is what __open weights__ buy: the freedom to choose _where_ the model runs
{{% /fragment %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="open-models" >}}

## What makes a model _"open"_? The general concept

A model release is a __bundle__ of separately _openable_ artifacts: downloading one of them (usually the weights) says little about the others

| Component | What a useful release contains | What it enables |
|---|---|---|
| __Weights__ | final parameters (ideally also intermediate checkpoints) | local inference, fine-tuning, independent hosting |
| __Inference code__ | architecture, tokenizer, generation code, configuration | actually _running_ the weights |
| __Training code & recipe__ | data processing, hyper-parameters, training and post-training (SFT, RL) pipelines | understanding _how_ the weights arose; re-training |
| __Training data__ | the actual training mixture, in training-ready form | data auditing, contamination analysis, exact reproduction |
| __Data information__ | provenance, sources, filtering and selection processes | a (weaker) substitute when data cannot be redistributed |
| __Documentation__ | model card, technical report, evaluations, limitations | auditability, downstream risk assessment |
| __Licenses__ | _separate_ licenses for weights, code, and data | knowing what you may _legally_ do with all the above |

---

## Openness is _graded_, not binary

{{< image src="./openness-bar.svg" max-h="30vh" alt="Openness of seven release components for a closed API model, an open-weights model (Llama 4) and a fully open model (OLMo 3): downloadable weights do not make a model open" >}}

| Framework | Question it answers |
|---|---|
| [Liesenfeld & Dingemanse, FAccT 2024](https://doi.org/10.1145/3630106.3659005) | how open is a release _in practice_? (14 dimensions, 40 text LLMs + 6 text-to-image models) |
| [European Open Source AI Index](https://osai-index.eu/database/) | which open-science artifacts were actually released? (graded score; top ranks in July 2026: YuLan-Mini, BLOOMZ, OLMo 3, Apertus) |
| [Stanford Foundation Model Transparency Index](https://crfm.stanford.edu/fmti/December-2025/index.html) (Dec. 2025) | how _transparent_ are providers? (mean score 41/100, down 17 from 2024; IBM 95, DeepSeek 32, Alibaba 26: open weights $\neq$ transparency) |
| [OSI Open Source AI Definition](https://opensource.org/ai/open-source-ai-definition) | does a system meet a _normative_ definition of "open source AI"? (next slide) |

- __Open-washing__: presenting a model as "open" or "open source" based on _one_ disclosed component (typically the weights), while the artifacts or legal freedoms needed to study, reproduce, modify, or freely use it are missing
    + e.g. _Llama 4_ (downloadable weights, restrictive license), _Gemma 3_ (custom terms with use restrictions), _Mistral 3_ (called ["open-source"](https://mistral.ai/news/mistral-3/) by its vendor: Apache 2.0 weights, undisclosed training data)
    + by contrast, OpenAI calls [gpt-oss](https://openai.com/index/introducing-gpt-oss/) "_open-weight_": the more accurate term

---

## The OSI Open Source AI Definition (OSAID 1.0, Oct. 2024)

- Users must be able, _without asking permission_, to:
    1. __use__ the system for _any_ purpose
    2. __study__ how it works
    3. __modify__ it for any purpose
    4. __share__ it, with or without modifications
- This requires the _preferred form for making modifications_:
    + __data information__: enough detail on training data for a skilled person to build a _substantially equivalent_ system
    + __code__: the _complete_ training and inference pipeline
    + __parameters__: weights and configuration, under open terms
- __Controversy__: OSAID does __not__ require releasing the training data itself (cf. [OSI's FAQ](https://opensource.org/ai/faq))
    + OSI: data may be personal, copyrighted, or confidential, so requiring it would exclude too many systems
    + critics: without the data, exact reproduction and data-level auditing are impossible
- OSI's [initial validation](https://opensource.org/ai/final-board-report) found that _Pythia_, _OLMo_, _Amber_, _CrystalCoder_, and _T5_ passed, while _Llama 2_, _Grok_, _Phi-2_, and _Mixtral_ did not
    + a _snapshot_ of 2024, __not__ a certification registry for later versions

---

## The licensing landscape (as of September 2026)

| Category | Examples | What is open | Main limitation |
|---|---|---|---|
| __Closed__ (API-only) | GPT, Claude, Gemini, Mistral's "Premier" models | API + documentation | no weights; usage governed by _terms of service_ |
| __Open weights, restrictive license__ | [Llama 4](https://www.llama.com/llama4/license/), [Gemma 3](https://ai.google.dev/gemma/terms), [Teuken v0.6](https://huggingface.co/openGPT-X/Teuken-7B-instruct-v0.6), BLOOM ([RAIL](https://huggingface.co/bigscience/bloom-560m/blob/main/LICENSE)) | weights, inference code | use-based, geographic, commercial, or downstream restrictions: _not_ "any purpose" |
| __Open weights, permissive license__ | [Gemma 4](https://ai.google.dev/gemma/docs/core/model_card_4), [Qwen3](https://huggingface.co/Qwen/Qwen3-8B), [DeepSeek-R1](https://huggingface.co/deepseek-ai/DeepSeek-R1), [gpt-oss](https://huggingface.co/openai/gpt-oss-120b), [Mistral Large 3 / Small 4](https://docs.mistral.ai/getting-started/models/) | weights under Apache 2.0 or MIT | training data and full pipeline _not_ released |
| __Fully (or nearly) open__ | [OLMo 3](https://allenai.org/blog/olmo3), [Apertus](https://www.apertus-ai.org/), [Minerva](https://huggingface.co/sapienzanlp/Minerva-7B-base-v1.0) | weights + code + data (or data pipeline) + docs | reproduction still needs _enormous_ compute |

---

## The licensing landscape (as of September 2026)

### Licenses change across versions: always check the __exact checkpoint__

| Model | Before | Now |
|---|---|---|
| __Gemma__ | Gemma 3: custom Gemma Terms, with cascading use restrictions | Gemma 4 (2026): __Apache 2.0__ |
| __DeepSeek-V3__ | original V3: custom DeepSeek Model Agreement | V3-0324: [relicensed under __MIT__](https://api-docs.deepseek.com/news/news250325) (March 2025) |
| __Mistral__ | historically mixed | _still_ mixed: Large 3 and Small 4 Apache 2.0, Medium 3.5 "Modified MIT", several models proprietary |
| __Teuken__ | v0.4-commercial: Apache 2.0 | v0.6: __non-commercial__ ("newer" does not mean "more permissive") |
| __iGenius → Domyn__ | Italia 9B: MIT; Italia 10B: [custom, non-open license](https://secure.igenius.ai/legal/iGenius%2BLicense%2BItalia%2B10B.pdf) | [rebranded Domyn](https://www.domyn.com/news/introducing-domyn) (June 2025); [Domyn Small 10B](https://www.domyn.com/news/introducing-domyn-small-an-open-european-reasoning-llm-built-for-ai-ownership) (May 2026): MIT |

---

## What do restrictive licenses mean in practice?

| License | On-premise use | Fine-tuning | Hosted service | Redistribution |
|---|---|---|---|---|
| __Llama 4__ ([license](https://github.com/meta-llama/llama-models/blob/main/models/llama4/LICENSE), [AUP](https://github.com/meta-llama/llama-models/blob/main/models/llama4/USE_POLICY.md)) | yes, __except__ multimodal models for individuals/companies _based in the EU_ | yes | yes (EU _end users_ of products are carved out) | _"Built with Llama"_ attribution; derived models named _"Llama..."_; separate license above 700M monthly active users |
| __Gemma 3__ ([terms](https://ai.google.dev/gemma/terms)) | yes, within the prohibited-use policy | yes, derivatives inherit the restrictions | counts as _distribution_ | restrictions must be passed to downstream users; Google may restrict use _"remotely or otherwise"_ (a contractual right, not a kill switch) |
| __OpenRAIL__ ([OpenRAIL-M](https://www.licenses.ai/blog/2022/8/26/bigscience-open-rail-m-license)) | yes, for permitted purposes | yes, for permitted purposes | yes, users bound to use restrictions | use restrictions must be passed on |
| __Apache 2.0 / MIT__ | yes | yes | yes | yes, with attribution/notices (and patent clauses, for Apache) |

{{% fragment %}}
> __Running example__: the admission assistant must read _passport pictures_, hence needs a __multimodal__ model. The University of Bologna is an EU organization: under Llama 4's acceptable use policy, it is __not__ granted the right to use Llama 4's multimodal models. Gemma 4, Qwen3, or Mistral's Apache models raise no such issue
{{% /fragment %}}

---

## Derivatives: fine-tunes, distillation, merges, data, outputs

- __Fine-tunes__ remain subject to the base model's license: easy for Apache/MIT, restrictions _propagate_ for Gemma 3 and Llama
- __Distillation__ is license-specific:
    + Gemma 3 counts models trained on Gemma's _outputs_ (synthetic data) as _Model Derivatives_
    + DeepSeek-R1 (MIT) allows distillation, but its published _distills_ are fine-tunes of other models, and __inherit their licenses__ ([Qwen](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B) → Apache 2.0, [Llama](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B) → Llama license)
- __Merges__: a merge of models A and B must (conservatively) satisfy __both__ licenses; a restrictive component is not "washed away"
    + whether weights are copyrightable at all is still legally _unsettled_
- __Training data__ has its _own_ license: e.g. OLMo 3 is Apache 2.0, but its dataset [Dolma 3](https://huggingface.co/datasets/allenai/dolma3_mix-6T) is ODC-By (attribution required), and its sources may carry further terms
- __Outputs__: "the vendor claims no rights on outputs" $\neq$ "outputs are free of restrictions"
    + e.g. using Llama _outputs_ to train a distributed model triggers Llama's _naming_ requirement
    + proprietary vendors offer _copyright indemnities_ for outputs (e.g. Microsoft's Copilot Copyright Commitment, Google's generative AI indemnification, OpenAI's Copyright Shield, Anthropic's commercial terms), usually for paid or enterprise tiers only, and with conditions; open-weights licenses give none
    + in Italy, only _human_ works are protected (L. 132/2025, Art. 25: cf. [below](https://unibo-fc-isi-iag.github.io/slides-module2/governance/#/regulation))

---

{{< slide id="open-models-ai-act" >}}

## Open models under the EU AI Act

| GPAI provider duty ([Art. 53](https://artificialintelligenceact.eu/article/53/)) | Ordinary GPAI | Free/open-source GPAI |
|---|---|---|
| (a) technical documentation (for the AI Office) | required | __exempt__ |
| (b) information for downstream providers | required | __exempt__ |
| (c) policy to comply with EU _copyright_ law (incl. text-and-data-mining opt-outs) | required | __still required__ |
| (d) public _summary of training content_ (AI Office template) | required | __still required__ |
| systemic-risk obligations ([Art. 55](https://artificialintelligenceact.eu/article/55/)) | if > 10<sup>25</sup> FLOPs (presumption of [Art. 51(2)](https://artificialintelligenceact.eu/article/51/)) | __no exemption__ |

- To qualify, the model must be released under a license allowing _access, use, modification, and distribution_, with weights, architecture, and usage information _public_
    + this is much __weaker__ than OSAID: no training data or training code needed
- Non-EU providers of open-source GPAI are also exempted from appointing an EU _authorized representative_ ([Art. 54(6)](https://artificialintelligenceact.eu/article/54/)), unless the model poses systemic risk
- cf. the Commission's [AI Act Service Desk FAQ](https://ai-act-service-desk.ec.europa.eu/en/ai-act/faq/how-does-ai-act-apply-general-purpose-ai-models-released-open-source) on open-source GPAI models

> The AI Act gives open-source GPAI a __documentation discount__: __not__ a copyright exemption, __not__ a training-data-summary exemption, and __not__ a systemic-risk exemption

{{% /section %}}

---

{{< slide id="providers-landscape" >}}

# Models, Providers, and Platforms

Who offers what

---

{{% section %}}

{{< slide id="provider-kinds" >}}

## Primary providers, aggregators, and marketplaces

| Kind | What they offer | Examples | Contract with |
|---|---|---|---|
| __Primary providers__ (model developers) | their _own_ models, via their own API (and apps) | OpenAI, Anthropic, Google, Mistral, DeepSeek | the developer |
| __Inference providers__ | _open-weight_ models (of others), run on their hardware | Together AI, Groq, Fireworks, DeepInfra, EU clouds (e.g. Scaleway, OVHcloud) | the host |
| __Cloud platforms__ (hyperscalers) | many models (proprietary _and_ open), inside a cloud ecosystem | Azure AI Foundry, AWS Bedrock, Google Vertex AI | the cloud provider |
| __Aggregators__ (routers) | _one_ API and _one_ bill, for models of _many_ providers | Open Router | the aggregator (and, indirectly, each upstream provider) |
| __Marketplaces__ / hubs | models to _download_, and (some) hosted inference | Hugging Face | depends on the model's license, and on the host |

- The _same_ open-weight model can be reached in _many_ ways: compare __price__, __latency__, __data location__, and __terms__ (e.g. data retention, training on your data)
- The more _intermediaries_, the more contracts (and jurisdictions) your data goes through

---

{{< slide id="open-router" >}}

## About Open Router (cf. <https://openrouter.ai/>), pt. 1

- Open Router (_OR_) is a company that provides an __on-cloud__ service for using _LLMs as services_, with a focus on _provider interchangeability_

- Its distinguishing feature is that it acts as an _aggregator_ of multiple service providers, allowing users to access a variety of models from different providers via a single API, and helping to avoid vendor lock-in
    + one can use OR for __free__, with _very limited rates_ (20 requests per minute, max 50 per day)
        * one may __buy credits__ to widen the limitations for free models (see <https://openrouter.ai/docs/api_reference/limits> for details), or to use _pay-per-use_ models
        * other free options are listed in the [Free Access appendix](https://unibo-fc-isi-iag.github.io/slides-module2/free-access/#/free-tiers)

- Pretty rich [model zoo](https://openrouter.ai/models), exposing models from <u>many</u> different providers, most notably: OpenAI, Anthropic, Google, xAI, Mistral, etc.
    + notice that models come with __price-per-token__, commonly expressed a `$/1M tokens` (some times the prices is different for _input_ or _output_ tokens)
        * this is very common for on-cloud services, and it is the main reason why we need to be careful when experimenting with them, to avoid unexpected costs
        * recall that _token_ $\neq$ _word_ (cf. [tokens](https://unibo-fc-isi-iag.github.io/slides-module2/genai/#/tokens))

    {{< image src="open-router-models.png" alt="Open Router's Model Zoo" max-h="40vh" >}}

---

## About Open Router (cf. <https://openrouter.ai/>), pt. 2

- Models in OR's zoo are __named__ according to the following convention: `provider/model` followed by _optional_ `:tier`, where:
    + `provider` is the name of the service provider (e.g. `openai`, `qwen`, `google`, etc.)
    + `model` is the name of the model _variant_ as provided by the provider (e.g. `gpt-oss-120b`, `qwen3-coder`, `gemma-4-26b-a4b-it`, etc.)
    + `tier` is an optional suffix that denotes a specific variant of the model (most commonly `free` for free models)


- __Dynamic routing__ of requests happens at two levels:
    + among __models__: `openrouter/auto` ([Auto Router](https://openrouter.ai/docs/guides/routing/routers/auto-router)) lets OR pick the model for each request, based on its content (__paid__: it may route to paid models, so it fails on accounts without credits)
        * the candidates can be restricted with _glob_ patterns, e.g. `anthropic/*` or `openai/gpt-5*`, in the router's `allowed_models` (these are __not__ valid `model` names by themselves)
    + among __providers__ of a _given_ model: OR picks one of the providers serving it ([provider selection](https://openrouter.ai/docs/guides/routing/provider-selection)); by default, load-balanced and price-weighted
        * override per request with `provider.sort` = `price` | `latency` | `throughput` (tokens per second), or in the [routing settings](https://openrouter.ai/workspaces/default/routing) of your OR account
        * or with model _suffixes_: `:floor` (cheapest), `:nitro` (fastest throughput), [`:exacto`](https://openrouter.ai/docs/guides/routing/model-variants/exacto) (providers with better tool-calling quality, e.g. `moonshotai/kimi-k2-0905:exacto`)

- __Bring your own API key__ (BYOK): OR allows you to _link_ your account with your API keys from _other providers_ so that you can use OR's API to access models from those providers, and therefore be charged according to the prices of those providers instead of OR's prices
    + this is a great feature to avoid vendor lock-in, and to have more control over the costs and the models you want to use

> Dynamic routing and BYOK are _peculiar_ features of OR, for the rest it's an "ordinary" on-cloud provider

---

## On-cloud Example with Open Router (pt. 1)

1. __Sign up__ & _log in_ for an account on Open Router: <https://openrouter.ai>
    + easier to re-use your GitHub account

2. Optionally add __payment methods__ and _credits_ to your account in <https://openrouter.ai/settings/credits>
    + no credit needed for the examples of these lectures
    + recall to set _expense limitation_ to avoid unexpected costs, in case you decide to experiment with actual money

    {{< image src="open-router-credits.png" alt="Open Router's Credits Settings" max-h="40vh" >}}

---

## On-cloud Example with Open Router (pt. 2)

3. From time to time, keep an eye on the __activity dashboard__ of your account, to check the usage and the costs of your requests: <https://openrouter.ai/activity>

    {{< image src="open-router-activity.png" alt="Open Router's Activity Dashboard" max-h="40vh" >}}

---

## On-cloud Example with Open Router (pt. 3)

4. To use OR's models programmatically, you need to create an __API key__ first (to be stored securely, and not shared with anyone else)
    * (i.e. an _authentication token_ to let clients authenticate on your behalf)
    * you can do that for the _default workspace_ at <https://openrouter.ai/workspaces/default/keys>
        + __workspaces__ are OR-wise _administration domains_, inside which configurations rules (e.g. routing rules, cost limits, etc.) apply

    {{< image src="open-router-apikeys.png" alt="Open Router's API Keys Settings" max-h="40vh" >}}

    + an API key is a string of the form: `sk-or-v1-XXXXXXXXXXXXXXXXXXXXXXXX`: it is sufficient to <u>consume your credit</u> and to make requests on your behalf

---

## On-cloud Example with Open Router (pt. 4)

5. You may test your API key with a simple `curl` request:

    ```bash
    curl -X POST "https://openrouter.ai/api/v1/chat/completions" \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer YOUR_API_KEY" \
        -d '{"model": "nvidia/nemotron-3-super-120b-a12b:free", "messages": [{"role": "user", "content": "What is the capital of France?"}]}'
    ```

---

## On-cloud Example with Open Router (pt. 4, cont.)

answer (converted in YAML for the sake of readability):

```yaml {class="print-2cols"}
id: gen-1779447979-077vepCYJjlgJfXKnK2q
object: chat.completion
created: 1779447979
model: openai/gpt-5-nano-2025-08-07
provider: OpenAI
system_fingerprint: null
service_tier: default
choices:
  - index: 0
    logprobs: null
    finish_reason: stop
    native_finish_reason: completed
    message:
      role: assistant
      content: Paris.
      refusal: null
      reasoning: '**Providing the capital of France**


        The user asked a straightforward question: "What is the capital of France?"
        The answer is simple: Paris. I should respond concisely. My best guess is
        to confirm: "The capital of France is Paris." While adding context could be
        fun, it’s not necessary since the user didn’t request it. If I wanted to be
        helpful, I could mention that Paris is the largest city and home to famous
        landmarks like the Eiffel Tower, but I’ll keep it brief.**Confirming details
        on Paris**


        I can keep my response short and straightforward. The user asked about the
        capital of France, and I can simply say: "Paris." If I want to offer more,
        I could say, "The capital of France is Paris." I think that covers it! But
        if they are interested, I could mention I''m happy to share more details about
        Paris if they''d like. For now, I’ll stick with the essential answer.'
usage:
  prompt_tokens: 13
  completion_tokens: 243
  total_tokens: 256
  cost: 9.785e-05
  is_byok: false
  prompt_tokens_details:
    cached_tokens: 0
    cache_write_tokens: 0
    audio_tokens: 0
    video_tokens: 0
  cost_details:
    upstream_inference_cost: 9.785e-05
    upstream_inference_prompt_cost: 6.5e-07
    upstream_inference_completions_cost: 9.72e-05
  completion_tokens_details:
    reasoning_tokens: 192
    image_tokens: 0
    audio_tokens: 0

```

---

## One client, _many_ free providers

- All the providers above expose an __OpenAI-compatible__ API: only `base_url`, key, and model name change

{{% code path="static/lab-snippets/snippets/lecture_free_access/example1/free_providers.py" from="12" to="21" %}}

```bash
poetry run python -m snippets -l free-access -e 1 groq                               # list the models available now
poetry run python -m snippets -l free-access -e 1 groq openai/gpt-oss-20b "Hello!"   # one chat request
```

- `PROVIDERS`: each provider's _endpoint_, and the __environment variable__ holding _its_ key (e.g. `GROQ_API_KEY`, asked interactively if missing)
    + `ollama`, `vllm`: open models served _locally_ (on your machine, see [below](#/local)), hence no key
- Arguments: `PROVIDER` (required), then optionally `MODEL` and `PROMPT` (without them, the script just lists the models)
- Full script [here](../lab-snippets/snippets/lecture_free_access/example1/free_providers.py) (cf. [how to run snippets](../#/lab-snippets-run)); model IDs are _listed live_, not hard-coded, since they __go stale quickly__
- The same trick works with the [LLM-as-a-Service](../llmaas/) scripts: just set `OPENAI_BASE_URL`, `OPENAI_API_KEY`, __and__ `OPENAI_MODEL` (the default, `nvidia/nemotron-3-super-120b-a12b:free`, exists only on OpenRouter: pick an ID from the listing above), e.g.
    + Ollama: `OPENAI_BASE_URL=http://localhost:11434/v1 OPENAI_API_KEY=unused OPENAI_MODEL=gpt-oss:20b`
    + Groq: `OPENAI_BASE_URL=https://api.groq.com/openai/v1 OPENAI_API_KEY=... OPENAI_MODEL=openai/gpt-oss-20b`

---

{{< slide id="lock-in" >}}

## Portability of applications, and dependency on the provider

| What | Portable? | How to keep it portable |
|---|---|---|
| __API__ (requests, responses) | yes, within the _common subset_ of OpenAI-compatible APIs | base URL, key, and model name in the _configuration_; a gateway |
| __Open weights__ | yes: any host, or your own hardware | prefer permissive licenses; pin the exact checkpoint |
| __Proprietary models__ | __no__: they can be changed, or _retired_, on the vendor's schedule | have an alternative _tested_ in advance |
| __Provider-specific features__ (server-side state, built-in tools, fine-tuning on the platform) | __no__ | use them consciously, behind an _adapter_ |
| __Prompts__ | partly: tuned to a model, they may work worse with another | an __evaluation suite__, to re-validate prompts upon a change of model |
| __Embeddings__ (vector indexes, cf. RAG) | __no__: vectors of a model are meaningless for another | plan for _re-indexing_ |
| __Data__ (logs, conversations, files stored by the provider) | depends on the terms | keep your own copy |

> The real anti-lock-in tool is an __evaluation suite__ of your own: it tells, in hours, whether _another_ model or provider is good enough

{{% /section %}}

---

{{< slide id="local" >}}

# Local Execution and Infrastructural Sovereignty

---

{{% section %}}

{{< slide id="ollama" >}}

## About Ollama (cf. <https://ollama.com/>)

- Ollama is a company that provides a _containerized_ solution for _hosting_ and _running_ LLMs __on-premise__, with a focus on ease of use and accessibility

- It offers a [model zoo](https://ollama.com/models) with several pre-trained models available for _download_ and use
    + plus a _native API_ that is _partially compatible_ with OpenAI's API, to facilitate migration between on-premise and on-cloud deployments

- Ollama's solutions can be used on various __hardware__ & __OS configurations__, including _Nvidia_ GPUs and _Apple Silicon_ as well as _Linux_, _MacOS_, and _Windows_

- Ollama's API supports both a native __CLI conversational interface__ and a __Web API__ for programmatic access, making it versatile for different use cases and user preferences

- Ollama may also be used an [on-cloud service](https://ollama.com/pricing), but here we will focus on the on-premise deployment option alone
    + three levels of premiumships (\\$0/month, \\$20/month, \\$100/month) with widening rate limitations

---

## On-premise Example with Ollama pt. 1

0. Ensure you have the __required hardware__ (e.g. Nvidia GPU, Apple Silicon, etc.)
    + details and configuration caveats: <https://docs.ollama.com/gpu>
    + how to tell whether a model _fits_ your machine: see [Running models locally](#/local-requirements) below

1. __Install__ Ollama on your machine:

    ```bash
    brew install ollama # For macOS (Apple Silicon)
    curl -fsSL https://ollama.com/install.sh | sh # For Linux
    irm https://ollama.com/install.ps1 | iex # For Windows (PowerShell)
    ```

2. Activate the Ollama __service__ in some shell (alternatively, configure a _deamon_ to run it in background):

    ```bash
    ollama serve
    ```

---

## On-premise Example with Ollama pt. 2

3. Have a look to Ollama's __model zoo__ on <https://ollama.com/models>, and __select__ some model you like among the many variants available
    * notice naming convention: `model:variant`, most commonly the variant denotes the size of the model

    {{< image src="ollama-models.png" alt="Ollama's Model Zoo" max-h="70vh" >}}
    {{< image src="ollama-models-gemma4.png" alt="Available variants for a Model on Ollama's Model Zoo" max-h="70vh" >}}

---

## On-premise Example with Ollama pt. 3

4. Pick one model, e.g. `gemma4:e2b`, and __pull__ it:

    ```bash
    ollama pull gemma4:e2b
    ```

5. You may use Ollama's native __CLI conversational interface__ to try the model out:

    ```bash
    ollama run gemma4:e2b
    ```

    example of conversation (notice that the model is also showing its _thinking process_):

    ```text
    >>> ciao!
    Thinking...
    Thinking Process:

    1.  **Analyze the input:** The input is "ciao!". This is an informal Italian greeting, meaning "hello!".
    2.  **Determine the appropriate response:** Since the user initiated a friendly greeting, the response
    should also be a greeting, ideally reciprocating the language or offering a friendly follow-up.
    3.  **Formulate the response (in Italian):** The simplest and most common reply is "ciao" (hello/hi) or
    a slightly expanded version.
    4.  **Add a standard conversational element (optional but good practice):** Asking how the user is doing
    is polite. ("Come stai?" / "Come va?")
    5.  **Final selection:** A friendly, standard reply.

    *Self-Correction/Refinement:* Keep it warm and open-ended.

    *Output Generation:* Ciao! Come stai? (Hello! How are you?)
    ...done thinking.

    Ciao! Come stai? 😊
    ```

---

## On-premise Example with Ollama pt. 4

6. Should you ever want to use the model _programmatically_, you can use Ollama's __API__, which is _partially_ compatible with OpenAI's API
    * e.g. it supports the same `/v1/chat/completions` endpoint, but not all the features of OpenAI's API, e.g. the `tool_choice`, `logit_bias`, `user`, and `n` request fields, nor the files, batches, or fine-tuning endpoints (cf. [compatibility page](https://docs.ollama.com/api/openai-compatibility))
    * in the client program, recall to set the __API base URL__ to point to your local Ollama service (by default: <http://localhost:11434/v1>)
    * to prove that the API is working, you can send a test request via `curl`:

        ```bash
        curl -X POST "http://localhost:11434/v1/chat/completions" \
            -H "Content-Type: application/json" \
            -d '{
                "model": "gemma4:e2b",
                "messages": [
                    {"role": "system", "content": "You are a helpful assistant."},
                    {"role": "user", "content": "What is the capital of France?"}
                ]
            }'
        ```

        result (converted in YAML for the sake of readability):

        ```yaml
        id: chatcmpl-85
        object: chat.completion
        created: 1779435598
        model: gemma4:e2b
        system_fingerprint: fp_ollama
        choices:
          - index: 0
            message:
              role: assistant
              content: The capital of France is **Paris**.
              reasoning: |-
                Thinking Process:

                1.  **Analyze the Request:** The user is asking a factual question: "What is the capital of France?"
                2.  **Identify the Knowledge Needed:** I need to retrieve the capital city of France.
                3.  **Recall/Retrieve the Fact:** The capital of France is Paris.
                4.  **Formulate the Answer:** Provide a direct and accurate answer.
                5.  **Final Review:** The answer is correct and directly addresses the query. (Capital of France = Paris).
            finish_reason: stop
        usage:
          prompt_tokens: 29
          completion_tokens: 121
          total_tokens: 150
        ```

{{% /section %}}

---

{{% section %}}

{{< slide id="local-requirements" >}}

## Running models locally: what does it take? (pt. 1)

### The general concept: a model is a (huge) array of numbers

- An LLM is essentially a collection of __parameters__ (a.k.a. _weights_), i.e. numbers learned during training
    + model _size_ is expressed in number of parameters: e.g. `7B` = 7 billion parameters, `70B` = 70 billion
- To __run__ a model (_inference_), all its weights must be loaded in the _memory_ of the device doing the computation
    + ideally, the GPU's memory (__VRAM__), as GPUs are much faster than CPUs at the matrix multiplications LLMs are made of
- Hence, the __memory footprint__ of a model is roughly:

> __memory__ ≈ (number of params × bits per param / 8) [_weights_] + _KV cache_ [grows with context length] + _overhead_

- the __key-value (KV) cache__ stores the attention _keys_ and _values_ of every token processed so far...
    + ... so that they do not need to be recomputed for every new token generated
    + its size _grows linearly_ with the _context length_
        + this is why local runtimes (e.g. Ollama) use a _smaller_ default context window than the model's maximum
- the _overhead_ covers runtime buffers, activations, and driver context: usually an extra ~10–20%

{{< image src="./memory-footprint.svg" max-h="20vh" alt="Stacked bars of the memory needed (weights plus 8k-token KV cache) by 7B, 14B, 32B and 70B models at 16-bit, Q8_0 and Q4_K_M precision, compared with 8/16/24/48/80 GB devices" >}}

---

## Running models locally: what does it take? (pt. 2)

### Rule of thumb for the weights alone

| Model size | 32-bit (`fp32`) | 16-bit (`fp16`/`bf16`) | 8-bit (`int8`, `Q8_0`) | ~4-bit (`Q4_K_M`) |
|-----------:|----------------:|-----------------------:|-----------------------:|------------------:|
| 1B  | 4 GB   | 2 GB   | ~1 GB  | ~0.6 GB |
| 7B  | 28 GB  | 14 GB  | ~7.5 GB | ~4.3 GB |
| 14B | 56 GB  | 28 GB  | ~15 GB | ~8.6 GB |
| 32B | 128 GB | 64 GB  | ~34 GB | ~20 GB  |
| 70B | 280 GB | 140 GB | ~74 GB | ~43 GB  |

- i.e. GB ≈ billions of params × bits per weight / 8 (`Q8_0` ≈ 8.5 bits, `Q4_K_M` ≈ 4.9 bits, cf. [llama.cpp's quantize docs](https://github.com/ggml-org/llama.cpp/blob/master/tools/quantize/README.md))
- add ~10–20% runtime overhead, plus the KV cache (it grows with the context: several GB at 32k+ tokens)
- models are _trained_ in 16/32 bits, but most local users run them at __8 or 4 bits__: this is called _quantization_

{{% fragment %}}
> Before pulling a model, __do the math__: if it does not fit in VRAM, it will either _fail_ to load, or be (partially) _offloaded_ to the CPU, becoming _much slower_
{{% /fragment %}}

---

## Quantization (pt. 1): the concept

- __Quantization__ = representing the model's weights with _fewer bits_ (e.g. 4-bit integers instead of 16-bit floats)
    + by mapping _ranges_ of real values onto a small set of _discrete levels_, plus some _scaling factors_ (stored per _block_ of weights)
- Why?
    + _less memory_ $\implies$ bigger models on the same hardware
    + _faster_ inference, as moving weights around in memory is the main bottleneck
- At what price?
    + _loss of precision_ $\implies$ some _quality degradation_, which is often negligible at 8 bits, noticeable at 4 bits, and severe below 3 bits
    + degradation is task-dependent (e.g. worse for maths, code, and non-English languages)

{{< image src="./quantization.svg" max-h="40vh" alt="A bell-shaped histogram of fp16 weights mapped onto 16 four-bit codes plus a per-block scale, yielding an approximate, stepped histogram" >}}

---

## Decoding GGUF quantization labels (as seen in Ollama's tags)

- `F16` / `BF16`: no quantization (16-bit)
- `Q8_0`: 8-bit (~8.5 bits per weight, including block scales), nearly lossless
- `Q4_K_M`: ~4.9 bits per weight on average
    - `K` = _k-quants_ (super-blocks of weights whose sub-block scales are quantized too)
    - `M`edium mix (a few sensitive tensors kept at higher precision): the usual __default__ trade-off
- `Q2_K`, `Q3_K_S`, ... : smaller and increasingly lossy (`S`mall, `M`edium, `L`arge mixes)
- `IQ*`: _importance-matrix_ quants, calibrated on sample text to lose less at very low bit-widths

e.g. `ollama pull gemma4:e2b` downloads the default quantization, whereas tags like `...-q8_0` or `...-fp16` select a specific one (see the _"Tags"_ tab of each model on [Ollama's zoo](https://ollama.com/models))

---

## Running models locally: hardware & software (pt. 1)

### Hardware

- __Nvidia GPUs__ (CUDA): the reference platform; the VRAM size is what matters most (e.g. 8, 12, 16, 24 GB for consumer cards; 40–80+ GB for data-center ones)
- __AMD GPUs__ (ROCm): supported, with fewer models of cards and more caveats
- __Apple Silicon__ (Metal): CPU and GPU share a _unified memory_, so the whole RAM (minus what the OS needs) is usable by the model
- __CPU only__: works for small models, but expect _few tokens per second_
- __Partial offloading__: layers that do not fit in VRAM run on the CPU, which works but is much slower

(cf. <https://docs.ollama.com/gpu> for the supported GPUs)

### Mixture-of-Experts (MoE) models

- Some models activate only a _subset_ of their parameters for each token
    + e.g. `gemma-4-26b-a4b-it` = 26B _total_ parameters, ~4B _active_ per token
- __Memory__ depends on the _total_ parameters (all experts must be loaded), __speed__ depends on the _active_ ones

---

## Running models locally: hardware & software (pt. 2)

### What fits where? (indicative, at ~4-bit quantization, moderate context)

| Machine | Comfortable model size |
|---------|-----------------------|
| laptop, 8 GB RAM, no GPU | ≤ 3B (slow) |
| laptop, 16 GB unified memory (Apple Silicon) | ≤ 8B |
| desktop, 12–16 GB VRAM GPU | ≤ 14B |
| workstation, 24 GB VRAM GPU (or 32–64 GB unified memory) | ≤ 32B |
| server, 2× 80 GB GPUs | ≤ 70B at 16-bit (140 GB, tight), ~120B+ at 8-bit, larger with 4-bit or MoE |

### Software

- up-to-date __GPU drivers__ (CUDA for Nvidia, ROCm for AMD; Metal is built into macOS)
- a __runtime__: [Ollama](https://ollama.com) (simplest), [LM Studio](https://lmstudio.ai/) (GUI), [llama.cpp](https://github.com/ggml-org/llama.cpp) (lowest level), [vLLM](https://docs.vllm.ai/) (server-grade, high throughput, many concurrent users)
    + all of them expose an _OpenAI-compatible_ Web API, so client code does not change
- check what is running, and _where_, with `ollama ps` (on an Apple Silicon laptop):

    ```text
    NAME          ID              SIZE      PROCESSOR    CONTEXT    RUNNER    UNTIL
    gemma4:e2b    7fbdbf8f5e45    1.7 GB    100% GPU     4096                 4 minutes from now
    ```

    + `SIZE` is the memory _actually allocated_ (weights in use + KV cache), whereas `ollama list` shows the size _on disk_ (7.2 GB for `gemma4:e2b`: all weights, incl. per-layer embeddings and vision/audio encoders)
    + `PROCESSOR` tells whether the model is fully on the GPU, or split between CPU and GPU (e.g. `48%/52% CPU/GPU`)
    + `CONTEXT` is the context window in use: Ollama's [default](https://docs.ollama.com/context-length) depends on the VRAM (4k tokens below 24 GiB, 32k up to 48 GiB, 256k above)

{{% /section %}}

---

{{% section %}}

{{< slide id="cost-example" >}}

## Deployment options: a worked cost example

Let's estimate the yearly cost of the running example, under explicit (and rough) __assumptions__:
- 500 applications per year; each one = passport picture + transcript + letter ≈ 6k input tokens, plus ~1k tokens of prompts
- 5 requests per application (extraction, validation, scoring, ...) $\implies$ ~35k _input_ + ~5k _output_ tokens per application
- hence, per year: __17.5M input__ + __2.5M output__ tokens

| Option | Price (USD per 1M input / output tokens) | Yearly cost |
|---|---|---|
| frontier proprietary model (e.g. [Claude Opus 5.5](https://platform.claude.com/docs/en/about-claude/pricing)) | \\$4 / \\$20 | 17.5 × 4 + 2.5 × 20 ≈ __\\$120__ |
| small proprietary model (e.g. [Claude Haiku 4.5](https://platform.claude.com/docs/en/about-claude/pricing)) | \\$1 / \\$5 | ≈ __\\$30__ |
| hosted open model, in the EU (e.g. Mistral Small 4, multimodal, Apache 2.0, on Mistral's [La Plateforme](https://mistral.ai/pricing)) | \\$0.15 / \\$0.60 | 17.5 × 0.15 + 2.5 × 0.6 ≈ __\\$4__ |
| rented GPU, on demand (e.g. 24 GB GPU on [RunPod](https://www.runpod.io/pricing)) | ~\\$0.35–0.75 per hour | a few hours per year ≈ __\\$5__ (+ setup effort) |
| on-premise (a 24 GB GPU workstation) | ~€2–2.5k for the GPU alone | __€2.5k+__ upfront, + energy, + maintenance |

(prices as of September 2026: they change every few months, always re-check them)

{{% fragment %}}
> At _this_ scale, __cost is not the deciding factor__: compliance and control are. Things change at scale: e.g. one 80 GB GPU rented 24/7 costs ~\\$2k/month, which buys ~100M _output_ tokens of a frontier model per month. The __break-even__ depends on _volume_, _utilization_, and _model size_
{{% /fragment %}}

---

{{< slide id="local-sustainability" >}}

## When is local execution _really_ sustainable?

{{% multicol %}}
{{% col %}}

### It is, when...

- data __must not__ leave the premises (by law, contract, or policy), and no trusted host is available
- the workload is __steady__ and __high-volume__: hardware is used most of the time (high _utilization_)
- __small/medium__ models suffice for the task (e.g. extraction, classification, summarisation), as measured on your own evaluation
- __skills__ are available in-house: GPUs, drivers, runtimes, updates, monitoring, security
- __offline__ operation, or _pinned_ versions (reproducibility) are required

{{% /col %}}
{{% col %}}

### It is not, when...

- the task needs _frontier_ quality (hard reasoning, long agentic tasks): open models lag behind, and the largest ones need _data-centre_ hardware
- the workload is __small__ or __bursty__: hardware sits idle, and pay-per-token is cheaper (cf. the [cost example](#/cost-example))
- many __concurrent__ users: one GPU serves few requests at a time; scaling means more GPUs
- nobody can _operate_ it: an unmaintained server is a security and continuity risk
- __time-to-market__ matters: procurement and setup take weeks

{{% /col %}}
{{% /multicol %}}

> A middle ground: _open-weight_ models, hosted by a provider __in the EU__ (or on a rented GPU), with the option to move them on-premise later: same weights, same behaviour

{{% /section %}}

---

{{< slide id="eu-sovereignty" >}}

# European Digital Sovereignty

Initiatives, and their maturity

---

{{% section %}}

{{< slide id="sovereignty" >}}

## "Sovereign AI": a framing, and its limits

- Several European and Italian actors frame open, locally-developed models as a matter of __digital sovereignty__
    + reduce dependency on US and Chinese providers; better support for EU _languages_; compliance with EU _law_
- The framing mixes (at least) three __distinct__ goals, which should _not_ be conflated:
    1. __where__ a model is _developed_ (nationality of the developer)
    2. __where__ it is _executed_, and data are processed (_operational_ control)
    3. whether its making can be _inspected_ and _reproduced_ (_auditability_)
- Goals 2 and 3 do __not__ require goal 1:
    + _any_ open-weights model (e.g. Qwen3, gpt-oss, OLMo 3) can run in an EU data centre
    + the most reproducible models are not necessarily European (e.g. OLMo 3, from the US), and a European vendor may disclose little (e.g. Mistral)

{{% fragment %}}
> Skeptical reading: for an engineer, _where the model runs_ and _what its license allows_ matter much more than _who trained it_. The evidence (next slides) suggests that EU and Italian models are mostly __small__, __late__, and __not competitive__ with the frontier, nor with the best global open-weights models
{{% /fragment %}}

---

## The gap, in numbers

Data from the [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report) (2025 figures):

| | United States | China | Europe | Italy |
|---|---|---|---|---|
| _Notable_ AI models released (Epoch AI, Fig. 1.1.13) | __59__ | 35 | 1 (France); 1 more in the UK | none listed |
| Private AI investment (Quid, Fig. 4.2.8, 4.2.11) | __\\$285.9B__ | \\$12.4B | \\$20.9B (France \\$4.4B, Germany \\$3.9B) | < \\$1B (not in the top 15) |
| ... of which _generative_ AI (Fig. 4.2.10) | __\\$163.6B__ | \\$1.5B | \\$3.2B | — |
| Newly funded AI companies (Fig. 4.2.9) | __1,953__ | 161 | e.g. Germany 92, France 84 | 38 |
| Growth of private AI investment, 2024 $\rightarrow$ 2025 (Fig. 4.2.11) | +160% | +32% | +7% | — |

- The gap is not closing: it is _widening_, at least for private investment
- Caveats: private investment understates _state_ funding (notably China's, and EU/national programmes such as EuroHPC AI Factories); counts by affiliation may double-count multinational teams

---

## European initiatives

| Initiative | Who | What | License | Status (Sep. 2026) |
|---|---|---|---|---|
| [EuroLLM](https://huggingface.co/utter-project/EuroLLM-22B-2512) | IST Lisbon, Univ. of Edinburgh, Unbabel, et al.; EuroHPC and Horizon Europe funding | 1.7B, 9B, 22B; 35 languages (all 24 EU ones), ~4T tokens | Apache 2.0 | 22B released Feb. 2026 |
| [OpenEuroLLM](https://openeurollm.eu/) | 20 members, led by Charles University and AMD Silo AI; Digital Europe grant (Feb. 2025, 3 years) | "truly open" models for all EU languages | open (planned) | so far only small _reference_ models (e.g. [38 monolingual 2.15B](https://openeurollm.eu/blog/hplt-oellm-38-reference-models), with HPLT); flagship still pending |
| [OpenLLM-France](https://openllm-france.fr/en/main-page-en/) | French consortium, Bpifrance funding | [Lucie-7B](https://huggingface.co/OpenLLM-France/Lucie-7B), then [Luciole](https://huggingface.co/collections/OpenLLM-France/luciole-llm) 1B/8B/23B (~30% French) | open data, weights, code | research project |
| [Teuken / OpenGPT-X](https://www.iais.fraunhofer.de/en/industries_and_cross-sector_solutions/cross-sector_solutions/generative-ai/opengpt-x.html) | Fraunhofer, FZ Jülich, et al.; ~€14M from the German government | 7B, 24 EU languages | v0.4: Apache 2.0; v0.6: non-commercial | project __ended__ in March 2025; models still downloadable |
| [Apertus](https://www.apertus-ai.org/) | ETH Zurich, EPFL, CSCS (Swiss AI Initiative: CHF 20M + 10M GPU-hours) | 8B, 70B; 1.5 (July 2026) adds images, reasoning, tools | Apache 2.0, fully open | the most complete open release in Europe; 2.0 planned for 2027 |
| [Mistral AI](https://docs.mistral.ai/getting-started/models/) | French private company | from small to large MoE models | mixed: Apache 2.0, Modified MIT, proprietary | the European vendor closest to the frontier; little training disclosure |

---

## Infrastructure and policy

- __EuroHPC AI Factories__: supercomputing centres opened to AI training and use
    + [IT4LIA](https://www.hpc.cineca.it/projects/it4lia/), at CINECA in Bologna (with Austria and Slovenia), was among the [first seven](https://eurohpc-ju.europa.eu/selection-first-seven-ai-factories-drive-europes-leadership-ai-2024-12-10_en) selected in Dec. 2024 (€1.5bn in total, EU + national funds)
    + beware: IT4LIA is an AI __Factory__, __not__ an AI _Gigafactory_: the [Gigafactories call](https://www.eurohpc-ju.europa.eu/eurohpc-joint-undertaking-launches-ai-gigafactories-call-2026-07-30_en) (for frontier-scale training) opened only in July 2026, with a Nov. 2026 deadline
- __Data__: [ALT-EDIC](https://www.alt-edic.eu/)'s [LLMs4EU](https://www.alt-edic.eu/projects/llms4eu/) (March 2025, 3 years, 20+ countries) collects language resources for all EU languages, and targets sector-specific models
- __Italian policy__: the [Italian AI Strategy 2024–2026](https://www.agid.gov.it/sites/agid/files/2024-07/Strategia_italiana_per_l_Intelligenza_artificiale_2024-2026.pdf) and [Law 132/2025](https://unibo-fc-isi-iag.github.io/slides-module2/governance/#/regulation) call for technology "tailored to the Italian context" and open research artifacts

{{< image src="./europe-timeline.svg" max-h="35vh" alt="Timeline 2023-2026 contrasting global frontier model releases with EU and Italian releases and projects, which are smaller and arrive later" >}}

---

## Recurring issues of European open models

1. __Data__: too little high-quality text in less-resourced languages (the very reason for LLMs4EU and for synthetic data)
2. __Compute__: every project depends on a public supercomputer allocation (Leonardo, MareNostrum 5, JUWELS, Alps); an "open" model is not _reproducible_ by anyone who lacks one
3. __Scale and timing__: models trained from scratch up to 14B parameters, released _months to years_ after comparable global open models; flagships "planned" or "pending"
4. __Maintenance__: projects have _finite_ funding (e.g. OpenGPT-X ended in 2025): who fixes, updates, and re-evaluates the models afterwards?
5. __Adoption__: public-administration use is mostly _aspirational_ in the sources, not a demonstrated large-scale deployment

{{% fragment %}}
> These initiatives are valuable for __research__, __openness__, and __language coverage__; they are (still) __not__ a production-grade alternative to global models. An EU organization can get operational control by running a _global_ open-weights model on _EU_ infrastructure
{{% /fragment %}}

{{% /section %}}

---

{{< slide id="italy" >}}

# The Italian Landscape

---

{{% section %}}

## Italian initiatives

| Model | Who | What | License | Status (Sep. 2026) |
|---|---|---|---|---|
| [Minerva](https://huggingface.co/sapienzanlp/Minerva-7B-base-v1.0) | Sapienza NLP, within the [FAIR](https://fondazione-fair.it/) PNRR project; trained on CINECA's Leonardo | 350M, 1B, 3B, and __7B__, from scratch; 1.14T Italian + 1.14T English + 200B code tokens | Apache 2.0, open data | 7B presented in Nov. 2024; still the flagship, research-grade |
| [LLaMAntino / ANITA](https://huggingface.co/swap-uniba/LLaMAntino-3-ANITA-8B-Inst-DPO-ITA) | Univ. of Bari | _fine-tunes_ of Llama, Mistral, gpt-oss for Italian (not trained from scratch) | inherited; [ANITA-NEXT](https://huggingface.co/m-polignano/ANITA-NEXT-24B-Magistral-2506-ITA)'s metadata says Apache 2.0, its card says "research only" | active research project; license hygiene is weak |
| [Velvet](https://huggingface.co/Almawave/Velvet-14B) | Almawave (private); trained on Leonardo | 2B and 14B, 6 languages (~23% Italian for the 14B) | Apache 2.0 | released Jan.–Feb. 2025, static since |
| [Italia](https://huggingface.co/sapienzanlp/modello-italia-9b) → [Domyn Small](https://huggingface.co/domyn/Domyn-Small-v1.0) | iGenius (private), renamed Domyn in June 2025 | Italia 9B (2024), then 10B; Domyn Small 10B (May 2026), 50+ languages | Italia 9B: MIT; Italia 10B: _custom, non-open_; Domyn Small: MIT | company focus moved to _proprietary_ large models |

- A "__national__" model says nothing about its _openness_: compare Minerva (Apache, open data) with Italia 10B (restrictive license)
- Italian models trained _from scratch_ range from 0.35B (Minerva) to 14B (Velvet); larger ones (e.g. ANITA-NEXT 24B) are _fine-tunes_ of foreign models: one or two orders of magnitude below the frontier

---

## How good are Italian models?

- Measuring it is hard: translated English benchmarks are _not_ enough
    + [ITA-Bench](https://aclanthology.org/2024.clicit-1.67/) (CLiC-it 2024) mixes translated tasks with native Italian datasets
    + [Evalita-LLM](https://arxiv.org/abs/2502.02289) (2025) uses _native_ Italian tasks, and multiple prompts per task, since rankings are _sensitive_ to prompt wording ([leaderboard](https://huggingface.co/spaces/evalitahf/evalita_llm_leaderboard))
- Published numbers show that small Italian models can be _useful_, e.g. [Velvet-2B](https://huggingface.co/Almawave/Velvet-2B) reports 39.6 on Italian MMLU and 61.9–86.6 on ITA-Bench tasks...
- ... but there is __no robust evidence__ that Italian models outperform large multilingual open models (e.g. Qwen3, Gemma 4), let alone frontier ones, _even on Italian_

{{% fragment %}}
> Leaderboard positions change every few months: when citing a rank, always write the __snapshot date__
{{% /fragment %}}

{{% /section %}}

---

{{< slide id="evaluation" >}}

# How to Evaluate an LLM

---

{{% section %}}

{{< slide id="model-selection" >}}

## Which model? The general concept

- Choosing a model is a _governance_ decision, since it affects all five dimensions at once:
    + _quality_ (capabilities), _cost_ (price per token, or hardware), _control_ (open weights? version pinning? deprecation?), _compliance_ (license, provider's jurisdiction and data policies), _time-to-market_ (availability, ecosystem)
- It requires _information_ about models, which comes from:
    1. their __documentation__ (model cards, system cards, licenses, terms of service)
    2. public __evaluations__ (benchmarks, leaderboards)
    3. __your own__ evaluations on the task at hand
- It is __not__ a one-off decision: models are _updated_, _deprecated_, and _surpassed_ every few months, so the choice must be _re-assessed_ periodically (and made _reproducible_)

(cf. [model zoos](#/model-zoos) and their [key parameters](#/key-parameters))

---

## Model documentation: model cards, system cards, datasheets

- __Model cards__: recall them from the [model zoos](#/model-zoos) ([Mitchell et al., 2019](https://arxiv.org/abs/1810.03993); [Hugging Face's guide](https://huggingface.co/docs/hub/model-cards)); here, what matters is what they say about _governance_ (below)
- __Datasheets for datasets__ ([Gebru et al., 2021](https://arxiv.org/abs/1803.09010)): the same idea, for training data (motivation, composition, collection, recommended uses)
- __System cards__: longer reports published by frontier providers for each model release, focusing on _safety_ evaluations, red-teaming, and mitigations
    + e.g. [OpenAI's](https://openai.com/index/gpt-oss-model-card/) and [Anthropic's](https://www.anthropic.com/system-cards) system cards

{{< image src="./model-card-anatomy.svg" max-h="35vh" alt="Anatomy of a model card: eight sections, from model details to license, each tagged with the governance dimensions it informs (quality, compliance, control)" >}}

- __What to look for__, as an engineer: _license_, _languages_, _context window_, _knowledge cut-off_, _intended uses_ (does yours fit?), known _limitations_, and _evaluations_ relevant to your task

---

## How to evaluate models?

### 1. Public __benchmarks__ and __leaderboards__ (generic, cheap to consult)

- _benchmarks_: fixed datasets + metrics for some capability
    + e.g. knowledge ([MMLU](https://arxiv.org/abs/2009.03300)), expert reasoning ([GPQA](https://arxiv.org/abs/2311.12022)), coding ([SWE-bench](https://www.swebench.com/)), tool use, long context...
    + holistic frameworks like [HELM](https://arxiv.org/abs/2211.09110) evaluate many models on many scenarios and metrics (not just accuracy)
- _leaderboards_: rankings aggregating benchmark results or human preferences
    + e.g. [Arena](https://arena.ai/) (formerly Chatbot Arena / LMArena: humans blindly compare pairs of answers, cf. [Chiang et al., 2024](https://arxiv.org/abs/2403.04132); but see also the critique by [Singh et al., 2025](https://arxiv.org/abs/2504.20879)), [Artificial Analysis](https://artificialanalysis.ai/) (quality vs. price vs. speed)
- __caveats__:
    + _data contamination_: benchmark items may have leaked into training data (cf. [Xu et al., 2024](https://arxiv.org/abs/2406.04244))
    + _saturation_: top models all score near 100%, so benchmarks stop discriminating (e.g. Hugging Face [retired](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions/1135) its Open LLM Leaderboard in 2025)
    + _Goodhart's law_: once a benchmark becomes a target, it stops being a good measure
    + benchmarks measure _generic_ skills, possibly _not_ the ones your task requires

### 2. __Task-specific__ evaluation (the one that really matters)

- run each shortlisted model on _your_ test dataset, with _your_ scorers, as [below](#/validation) (deterministic scorers first, [LLM-as-a-Judge](#/llm-as-a-judge) where code can't help)
- measure _latency_ and _cost_ too (both reported in the API responses' metadata, e.g. `usage`)

---

{{< slide id="eval-dimensions" >}}

## What to evaluate: quality, efficiency, robustness, specialisation

| Dimension | Questions | How to measure |
|---|---|---|
| __Quality__ | are outputs _correct_, _complete_, _grounded_? | test set with expected results; scorers (code first, then LLM-as-a-judge, then humans) |
| __Efficiency__ | how _fast_, how _expensive_? | latency (time to first token, tokens per second), tokens per task, cost per task (or per month) |
| __Robustness__ | same quality with _rephrased_ inputs, noisy documents, adversarial content? Same output across runs? | input variations, repeated runs (_variance_), prompt-injection tests |
| __Specialisation__ | good at _your_ domain, _your_ language, _your_ document formats? | domain-specific test cases (e.g. Italian administrative texts, scanned forms) |

- Public benchmarks cover (part of) the first dimension, on _generic_ tasks: the others are __your__ job
- Each dimension can rule a model out: e.g. a model 2% more accurate but 10× slower, or _inconsistent_ across runs

---

## Measuring _general_ capabilities: frontier benchmarks

As models _saturate_ classic benchmarks, new ones try to measure what models __cannot__ (yet) do:

{{% multicol %}}
{{% col class="col-8" %}}
{{% small "90%" %}}
- __ARC-AGI__ ([Chollet, 2019](https://arxiv.org/abs/1911.01547)): intelligence as _skill-acquisition efficiency_, measured on visual puzzles that are easy for humans, and require _abstracting_ rules from a few examples
    + [ARC-AGI-2](https://arxiv.org/abs/2505.11831) (2025) raised the difficulty; the [ARC Prize 2025](https://arcprize.org/blog/arc-prize-2025-results-analysis) grand prize went _unclaimed_
    + [ARC-AGI-3](https://arcprize.org/blog/arc-agi-3-launch) (March 2026) is _interactive_: cf. the [agentic benchmarks](#/agentic-benchmarks) below, and the [leaderboard](#/arc-agi-3-leaderboard) below
- __Humanity's Last Exam__ ([Phan et al., 2025](https://arxiv.org/abs/2501.14249)): 2,500 expert-written questions across disciplines, designed to be _un-googleable_
    + best models went from single digits (early 2025) to ~60% (cf. [Artificial Analysis](https://artificialanalysis.ai/evaluations/humanitys-last-exam), September 2026)
- __METR's time horizon__ ([Kwa et al., 2025](https://arxiv.org/abs/2503.14499)): the _length_ of tasks (in human-expert time) that an agent completes with 50% success
    + _doubling_ every ~7 months over 2019–2025 (212 days, in the original paper), and faster since 2023: ~4 months (129 days, METR's Time Horizon 1.1 fit, cf. [live measurements](https://metr.org/time-horizons))
- __Aggregated views__: the [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report) describes a _jagged frontier_: models winning maths olympiad gold medals, yet failing at reading analog clocks
{{% /small %}}
{{% /col %}}
{{% col class="col-4" %}}
{{< image src="./benchmark-saturation.png" max-h="48vh" alt="AI Index 2026, Figure 2.1.1: best model performance relative to the human baseline, 2012-2025, on ImageNet, SuperGLUE, MMLU, GPQA Diamond, MATH, MMMU, AIME, SWE-bench Verified, OSWorld and others: each benchmark climbs towards or past the human baseline, newer ones ever faster" >}}

<small>Source: [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report), ch. 2, Figure 2.1.1 ([CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/))</small>
{{% /col %}}
{{% /multicol %}}

> __Engineering take-away__: frontier benchmarks tell you where the _state of the art_ is, not whether a model is good for __your__ task

---

## Frontier benchmarks: METR's time horizons

{{< image src="./task-completion-time-horizon.png" max-h="50vh" alt="METR: length of software tasks (in human time) that LLMs complete 50% of the time, by release date, log scale: from seconds (GPT-2, 2019) to about 1 hour (o1, late 2024), several hours (Claude Opus 4.6, early 2026), and about 16 hours (Claude Mythos Preview, early, April 2026), along an exponential trend; measurements above 16 hours are unreliable with the current task suite" >}}

- E.g. the 50% time horizon of Claude Mythos Preview (April 2026) is ~17 hours, at the limit of what METR's task suite can measure
- Caveats: _software_ tasks only, 50% success (the 80% horizon is much shorter), wide confidence intervals

<small>Source: [METR, time horizons](https://metr.org/time-horizons), retrieved October 2026</small>

---

{{< slide id="arc-agi-3-leaderboard" >}}

## Frontier benchmarks: the ARC-AGI-3 leaderboard

{{< image src="./arc-agi-3-leaderboard.png" max-h="50vh" alt="ARC-AGI-3 verified leaderboard, score versus cost per run (log scale): the best models with the standard harness reach about 63% (GPT-6 Astra, Max) at tens of thousands of dollars; Claude Opus 5 (High) about 30%; with the Provider Adapter harness, GPT-6 Astra and GPT-6.1 Sol reach 90–100%; most other models stay below 10%" >}}

- From < 1% at launch (March 2026) to 62.7% with the _standard_ harness, and ~99% with a _general-purpose_ harness (keeping reasoning state, compacting the conversation), in 6 months
    + at a cost of thousands to tens of thousands of dollars per full run; humans solve all environments

<small>Source: [ARC Prize leaderboard](https://arcprize.org/leaderboard), retrieved October 2026</small>

---

{{< slide id="agentic-benchmarks" >}}

## Agentic benchmarks: how good are agents, in general?

{{% small "80%" %}}

| Benchmark | What the agent must do | At release |
|---|---|---|
| [GAIA](https://arxiv.org/abs/2311.12983) (2023) | answer real-world questions, combining Web search, files, and tools | humans 92%, GPT-4 with plugins 15% |
| [WebArena](https://arxiv.org/abs/2307.13854) (2023) | complete tasks on realistic (self-hosted) Web sites, keeping track of the task state | humans 78%, best GPT-4 agent 14% |
| [AgentBench](https://arxiv.org/abs/2308.03688) (2023) | pursue goals in 8 environments (OS, databases, games, Web, ...) over many turns | large gap between commercial and open models |
| [SWE-bench](https://www.swebench.com/) (2023) | fix _real_ GitHub issues, i.e. build a working model of a code base | a few % (now much higher, cf. coding agents) |
| [$\tau$-bench](https://arxiv.org/abs/2406.12045) (2024) | serve simulated users via tools, following domain _policies_ | pass$^8$ < 25% in retail |
| [ARC-AGI-3](https://arcprize.org/arc-agi/3) (2026) | explore _novel_ interactive environments, infer goals and dynamics, plan | humans 100%, frontier AI < 1% ([March 2026](https://arxiv.org/abs/2603.24621)); best verified in Oct 2026: 62.7% ([leaderboard](https://arcprize.org/leaderboard)) |

{{% /small %}}

- __Representation__ is the bottleneck: agents fail when they cannot _build_, _maintain_, _verify_, and _revise_ the right representation of the task (its state, goals, rules, and the effects of actions)
    + e.g. on a variant of the ARC-AGI-3 environment TR87, Claude Opus 4.6 scores 0% with no harness and 97.1% within a _hand-crafted_ harness, yet 0% under both on BP35 ([ARC-AGI-3 report](https://arxiv.org/abs/2603.24621), §4): a hand-crafted harness can make or break a task, but does not _generalise_
    + whereas a _general-purpose_ harness (keeping reasoning state, compacting the conversation) lifts GPT-6 Astra from 54.8% to 99.9% on the whole benchmark ([leaderboard](https://arcprize.org/leaderboard), Oct 2026): the surrounding software matters as much as the model
- __Success__ without governance is not trustworthy _agency_: success rates say nothing about _how_ the goal was reached (policies, side effects, costs)
- Benchmarks _saturate_ and leak into training data: they help _choosing_ models (cf. [how to choose](#/decision)), but __your own__ evaluations are what tells whether _your_ agent works

{{% /section %}}

---

{{% section %}}

{{< slide id="realistic-evaluation" >}}

## A realistic evaluation, for an organisation

1. Start from the __use case__: which decisions, or documents, will the model touch? What is a _wrong_ output, and how bad is it?
2. Build a __golden set__: 30–100 _real_ (anonymised) cases, with the expected results, written by the people who do the job today
    + include _hard_ and _borderline_ cases, not just typical ones
3. __Shortlist__ 3–5 models satisfying the _hard constraints_ (deployment, data protection, license, modalities, language)
4. Run each model on the golden set, __several times__, measuring quality, efficiency, and robustness
5. __Decide__ with a rule written _in advance_ (e.g. "the cheapest model above 90% accuracy, with no critical errors"), and __document__ it
6. __Re-run__ the evaluation whenever the model, the prompt, or the provider changes: it is a _regression test_

> The golden set is an __asset__ of the organisation: it outlives models, providers, and vendors

---

{{< slide id="validation" >}}

## Validating generative software: the general concept

- LLM-based software is hard to test:
    + outputs are _non-deterministic_ (same input, different outputs)
    + outputs are _open-ended_ (many correct answers, no single expected value)
    + behaviour changes whenever the _prompt_, the _model_, or the _provider_ changes, even silently
- Hence, rather than _only_ unit tests (pass/fail on one run), we need __evaluations__ (a.k.a. _evals_): measuring _how often_, and _how well_, the system behaves as expected, on a _dataset_ of representative inputs
    + evaluations can still _run on_ unit-test frameworks (e.g. `pytest`), if assertions are on _rates_ or _thresholds_, rather than on single exact outputs

- Ingredients of an evaluation:
    1. a __test dataset__: _known_ inputs, each with the _expected_ results (a.k.a. _expectations_), designed by _humans_ __before__ testing
    2. the __system under test__ (prompt + model + code)
    3. __scorers__ (a.k.a. _metrics_), mapping each (input, output, expectation) to a _score_ or a _pass/fail_:
        * __deterministic__ scorers: schema validity, exact match, ranges, regular expressions, invariants and _relations_ among outputs
        * __statistical__ scorers: similarity to a reference (e.g. [BLEU](https://aclanthology.org/P02-1040/), [ROUGE](https://aclanthology.org/W04-1013/), embedding similarity)
            - they need a _reference_ text, and correlate poorly with human judgement on open-ended outputs (cf. [G-Eval](https://arxiv.org/abs/2303.16634)): not used here
        * __LLM-as-a-Judge__: another LLM grades the output, according to a natural language _rubric_
        * __humans__: the most reliable, and the most expensive
    4. an __aggregation__ (e.g. pass rate, mean score), to be compared across _versions_ of the system (_regression testing_)

- Structured outputs make evaluation _much_ easier: most fields can be checked with _code_

---

## Validating generative software: an intuitive example

{{< image src="./evaluation-pipeline.svg" max-h="65vh" alt="An evaluation pipeline: a test dataset of three letters with expectations goes through the prompt template and model; outputs are checked by deterministic, relational and LLM-as-a-judge scorers, and the pass/fail report feeds back into the prompt" >}}

---

{{< slide id="llm-as-a-judge" >}}

## LLM-as-a-Judge

- Idea: use a (strong) LLM to _grade_ outputs, according to some _criteria_ (cf. [Zheng et al. (2023)](https://arxiv.org/abs/2306.05685), and the survey by [Gu et al. (2024)](https://arxiv.org/abs/2411.15594))
    + _pointwise_ (grade one output on a scale), or _pairwise_ (which of two outputs is better?), with or without a _reference_ answer
    + the judge's prompt is a _rubric_, and its answer is (ideally) a __structured output__: score + rationale
    + asking for the _rationale first_ helps (cf. [chain-of-thought](https://unibo-fc-isi-iag.github.io/slides-module2/prompting/#/letter-tone)); [G-Eval](https://arxiv.org/abs/2303.16634) goes further, by first generating _evaluation steps_ from the criteria

- Known __biases__ of judges:
    + _position_ bias (preferring the first, or the second, option in pairwise comparisons), cf. [Wang et al. (2023)](https://arxiv.org/abs/2305.17926)
    + _verbosity_ bias (preferring longer answers), and _self-preference_ (preferring outputs of the _same_ model family)

- Good practices:
    + use a _different_ (and possibly stronger) model as judge, with _low_ temperature
    + prefer _narrow_, _binary_ criteria (e.g. "is every strength supported by the letter?") over vague ones (e.g. "is it good?")
    + __validate the judge__: compare its grades with _human_ grades, on a small sample, before trusting it
        * measure _agreement_: raw accuracy, plus [Cohen's κ](https://en.wikipedia.org/wiki/Cohen%27s_kappa) $= (p_o - p_e)/(1 - p_e)$, which discounts _chance_ agreement (cf. [Thakur et al., 2024](https://arxiv.org/abs/2406.12624))
    + use judges only for what _cannot_ be checked with code

---

{{< slide id="evidence" >}}

## How much evidence?

- A pass rate is an _estimate_, and small datasets give __wide__ uncertainty:
    + e.g. 3 letters out of 3 passing $\Rightarrow$ the 95% [Wilson](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Wilson_score_interval) confidence interval of the true pass rate is $[0.44, 1]$
    + hence, report pass rates _with_ confidence intervals (cf. [Miller, 2024, "Adding Error Bars to Evals"](https://arxiv.org/abs/2411.00640))
- Outputs are _non-deterministic_: repeat $N$ runs per test case, and look at the _distribution_, not at a single run
- Comparing two versions (prompts, models) _fairly_:
    + same test cases, same number of runs, _one_ factor changed at a time
    + compare them _paired_, case by case (e.g. [McNemar's test](https://en.wikipedia.org/wiki/McNemar%27s_test) on pass/fail, or paired differences of scores), rather than comparing two averages

{{% /section %}}

---

{{< slide id="decision" >}}

# Decision Criteria for Adoption

---

{{% section %}}

{{< slide id="three-options" >}}

## Commercial cloud, European provider, or on-premise?

{{% small "85%" %}}

| | Commercial cloud (e.g. US providers' APIs) | European provider (proprietary or open-weight models, EU data centres) | On-premise (open-weight models) |
|---|---|---|---|
| __Quality__ | highest (frontier models) | good: best open models, or EU proprietary ones (e.g. Mistral) | limited by the hardware |
| __Cost__ | pay per token; cheap at low volume | pay per token, often lower for open models | upfront hardware + energy + staff; cheap only at high, steady volume |
| __Control__ | low: models change or are retired by the vendor | medium: open weights can be moved elsewhere | full |
| __Compliance__ | contracts (DPA), transfer safeguards, foreign jurisdiction (e.g. US CLOUD Act) | EU jurisdiction; DPA still needed | data stays in-house |
| __Time-to-market__ | minutes | hours to days | weeks |

{{% /small %}}

- Often the answer is a __mix__ (cf. [hybrid architectures](../llmaas/#/hybrid)), by data sensitivity and task difficulty
- Whatever the option, __you__ remain responsible: e.g. as _deployer_ under the AI Act, and as _controller_ under the GDPR

---

## EU AI Act: a risk-based approach

{{% multicol %}}
{{% col %}}

- __Unacceptable risk__ $\rightarrow$ _prohibited_ ([Art. 5](https://artificialintelligenceact.eu/article/5/))
    + e.g. social scoring, manipulative techniques, __emotion recognition__ in workplaces and __education__, untargeted scraping of facial images
- __High risk__ $\rightarrow$ strict _obligations_ ([Art. 6](https://artificialintelligenceact.eu/article/6/) and [Annex III](https://artificialintelligenceact.eu/annex/3/))
    + e.g. CV screening, credit scoring, and __education__: systems to _determine access or admission_ to educational institutions (Annex III, point 3(a)), to _evaluate learning outcomes_ (3(b)), or to _proctor_ exams (3(d))
- __Transparency risk__ $\rightarrow$ _disclosure_ obligations ([Art. 50](https://artificialintelligenceact.eu/article/50/))
    + e.g. chatbots must disclose they are AI; synthetic content must be _marked_; deepfakes must be _labelled_
- __Minimal risk__ $\rightarrow$ no specific obligations
    + e.g. spam filters, AI in video games
- Orthogonally: __general-purpose AI (GPAI) models__, with obligations for their _providers_ ([Art. 53](https://artificialintelligenceact.eu/article/53/); systemic risk: [Art. 51](https://artificialintelligenceact.eu/article/51/), [Art. 55](https://artificialintelligenceact.eu/article/55/))

{{% /col %}}
{{% col %}}

{{< image src="./ai-act-pyramid.png" max-h="60vh" alt="The AI Act risk pyramid, top to bottom: unacceptable risk (prohibited), high risk (regulated), limited risk (transparency), minimal risk (unregulated)" >}}

{{% /col %}}
{{% /multicol %}}

{{% fragment %}}
> Our __running example is high-risk__: it contributes to deciding _admission_ to a PhD programme. The same LLM, used to summarize news, would be _minimal_ risk: the Act regulates the __use__, not the technology
{{% /fragment %}}

---

## How to choose a model for a given task?

{{% multicol %}}
{{% col %}}

### Hard constraints (filter)

1. __Deployment__: on-premise (hardware available?) or on-cloud (budget available?)
2. __Data protection__: can the data leave your premises? which jurisdiction? (cf. GDPR)
3. __License__: is the intended use (e.g. commercial) permitted?
4. __Modalities__: text only? images? audio?
5. __Capabilities__: tool calling? structured output? reasoning?
6. __Context window__: will prompts + documents + history fit?
7. __Languages__: e.g. is Italian supported well enough?

{{% /col %}}
{{% col %}}

### Soft criteria (rank)

- _quality_ on __your__ task (see next slide)
- _latency_ (time to first token, tokens per second)
- _cost_ per request (tokens × price, or hardware amortization)
- _stability_ of the offer (will the model be deprecated soon?)
- _ecosystem_ (client libraries, documentation, community)

{{% fragment %}}
> __Rule of thumb__: start from the _smallest/cheapest_ model satisfying the hard constraints, measure its quality on your task, and move to _bigger_ models only if needed
{{% /fragment %}}

{{% /col %}}
{{% /multicol %}}

{{< mermaid >}}
%%{init: {'flowchart': {'htmlLabels': false}}}%%
flowchart LR
    Z[("All models
    in the zoo")] --> D[/"Deployment &
    data protection"\] --> L[/"License"\] --> C[/"Modalities &
    capabilities"\] --> X[/"Context window
    & language"\] --> S["Shortlist
    (3-5 models)"] --> E["Evaluate on
    your own test set"] --> P(["Pick the cheapest one
    above the quality threshold"])
{{< /mermaid >}}

---

{{< slide id="open-models-checklist" >}}

## Engineering take-aways: pre-adoption checklist

The compliance unit is __checkpoint + license version + code licenses + data licenses + deployment + intended use__, not a brand name

| Question | What to check |
|---|---|
| exactly _which_ artifact? | repository, checkpoint hash, release date, license version |
| commercial use? | Apache/MIT: yes; CC-BY-NC: no; custom licenses: read the use restrictions |
| on-premise use? | the license, not the availability of files (e.g. Llama 4 multimodal in the EU) |
| fine-tuning, distillation, merging? | whether derivatives inherit restrictions; license of _each_ parent model |
| hosted service or redistribution? | whether serving an API counts as distribution (it does for Gemma 3); attribution and naming duties |
| geography or scale limits? | e.g. Llama 4: EU multimodal clause, 700M monthly users |
| training and fine-tuning data? | provenance, licenses (e.g. ODC-By), personal data |
| AI Act role? | are _we_ becoming a GPAI provider (e.g. by fine-tuning and distributing)? does the open-source exemption apply? |
| reproducibility needed? | self-hosting: permissive weights suffice; audit or research: prefer fully open models (OLMo 3, Apertus) |

> Mnemonic: __weights → license → recipe → data → regulatory role__: a model is only as open as the layer that matters for your use

---

## Engineering take-aways: three scenarios

{{% multicol %}}
{{% col %}}

### A UniBo lab, on-premise

- wants: local RAG/chat, fine-tuning on university material, reproducible experiments
- good fits: OLMo 3, Apertus (reproducibility); Gemma 4, Qwen3, gpt-oss, Mistral Apache models (weights only suffice); Minerva (Italian-centric research)
- ruled out: Llama 4 _multimodal_ (EU clause)

{{% /col %}}
{{% col %}}

### An Italian startup reselling a fine-tune

- wants: fine-tune, host, sell the model
- good fits: Apache/MIT weights (Gemma 4, Qwen3, gpt-oss, DeepSeek-R1, Mistral Large 3, OLMo 3, Domyn Small)
- beware: R1 _distills_ (Llama-based ones inherit Llama's license); Gemma 3 (restrictions flow to customers); Teuken v0.6 (non-commercial)

{{% /col %}}
{{% col %}}

### A public administration

- wants: EU-controlled deployment and auditability
- _auditability_: Apertus, OLMo 3, Minerva
- _operational control_: any permissively licensed model on EU infrastructure (incl. Mistral, EuroLLM, Velvet, Domyn Small)
- beware: IT4LIA/CINECA provides _compute_, not a model license

{{% /col %}}
{{% /multicol %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="exercise-compare" >}}

## Guided exercise 1: comparing models on the running example

> __Goal__: build a _shortlist_, and compare its models on evidence: the letter-scoring system of the [previous lecture](../llmaas/#/letter-scoring), on all 3 letters, 5 times each

```bash
poetry run python -m snippets -l governance -x 1                                     # default shortlist
poetry run python -m snippets -l governance -x 1 MODEL1 MODEL2@http://localhost:11434/v1   # custom shortlist (e.g. a local model)
```

- each `MODEL` is an Open Router model name, or `MODEL@BASE_URL` for other providers (e.g. Ollama)
- the script prints one line per model: scores (mean ± standard deviation), error w.r.t. the committee's _reference_ scores, ranking of candidates, failures, latency, tokens, and cost
    + raw data (one row per run) in `output/model-comparison.csv`
- discuss the results:
    + which model is the most __accurate__? the most __consistent__? the __cheapest__? the __fastest__?
    + do models __agree__ on the ranking of the candidates?
    + which one would you pick, and with which _rule_? Would you trust a decision based on 3 letters? (cf. [how much evidence](#/evidence))

(full code [here](../lab-snippets/snippets/lecture_governance/exercise1/compare_models.py); try also `TEMPERATURE=0` vs. `TEMPERATURE=1`)

---

{{< slide id="exercise-decision" >}}

## Guided exercise 2: a decision record, in groups

> __Goal__: _justify_ the adoption of an LLM-based system, in writing, as an organisation would have to

- In groups of 3–5 (mixing managers and technicians), pick a use case: the [running example](../llmaas/#/running-example), or one from _your_ office (e.g. classifying citizens' requests, summarising case files, drafting answers)
- Write a one-page __decision record__ (cf. _Architecture Decision Records_, <https://adr.github.io/>), answering:
    1. __Classification__: is the system _high-risk_ under the AI Act? Which personal data does it process?
    2. __Deployment__: commercial cloud, European provider, or on-premise? Why? (use the five dimensions)
    3. __Shortlist__: which 3 models, and which _hard constraints_ did they pass? (apply the [pre-adoption checklist](#/open-models-checklist))
    4. __Evaluation__: what would the _golden set_ contain? Which _decision rule_?
    5. __Oversight__: what do humans review? Who is _accountable_? When is the decision _re-assessed_?
- Share and discuss: could a colleague _reproduce_ your choice from the record alone?

{{% /section %}}

---

## Wrap-up

1. "Open" is __graded__: weights, code, data, documentation, and licenses are _separate_ questions; check the __exact checkpoint__
2. Open weights buy the freedom to choose __where__ a model runs: many providers, aggregators, or your own hardware
3. __Local execution__ is sustainable for steady volumes, small/medium models, and in-house skills; otherwise, EU-hosted open models are a middle ground
4. European and Italian models are valuable for research and openness, but __not yet__ competitive with global ones: _operational_ sovereignty does not need a _national_ model
5. Public benchmarks tell where the state of the art is; __your own golden set__ tells which model is good for _you_
6. Decide with explicit __criteria__, write them down, and __re-assess__ periodically

---

{{% import path="reusable/back.md" %}}
