+++

title = "About the lectures"
description = "LLM APIs, agents, and open models: two lectures of the 'AI Course — Regione Emilia-Romagna 2026'"
outputs = ["Reveal"]

+++

{{% import path="reusable/front.md" %}}

---

{{< slide id="toc" >}}

## Table of Contents

0. [About the lectures](./) (this page):
    + [the running example](llmaas/#/running-example) · [code of the examples](#/lab-snippets)
1. [LLM-as-a-Service: APIs, Tools, and Agents](llmaas) (27/10/2026, 4h)
    + [architectural choices](llmaas/#/llmaas) · [APIs](llmaas/#/apis) · [integration](llmaas/#/integration) · [OpenAI APIs in practice](llmaas/#/openai-in-practice) · [from text to processes](llmaas/#/processes) · [agentic AI](llmaas/#/agentic-ai) · [building an agent](llmaas/#/building-an-agent)
    + guided exercise 1. [same program, different providers](llmaas/#/exercise-providers) · guided exercise 2. [changing the output schema](llmaas/#/exercise-schema)
2. [Sovereign and Open Alternatives to Large LLMs](governance) (18/11/2026, 3h)
    + [open models](governance/#/open-landscape) · [providers](governance/#/providers-landscape) · [local execution](governance/#/local) · [EU sovereignty](governance/#/eu-sovereignty) · [Italy](governance/#/italy) · [evaluation](governance/#/evaluation) · [decision](governance/#/decision)
    + guided tour: [three zoos](governance/#/zoo-tour) · guided exercise 1. [comparing models](governance/#/exercise-compare) · guided exercise 2. [decision record](governance/#/exercise-decision)

---

## Teacher

### Giovanni Ciatto
* email: [`giovanni.ciatto@unibo.it`](mailto:giovanni.ciatto@unibo.it)
* homepage: <https://www.unibo.it/sitoweb/giovanni.ciatto/en>
* Department of Computer Science and Engineering (DISI), University of Bologna

---

## Goals

1. __LLM-as-a-Service__: how to _integrate_ LLMs into applications
    + architectural and strategic choices: cloud, on-premise, hybrid
    + the ecosystem of APIs, and the OpenAI API as the _de facto_ standard
    + from text generation to _processes_: structured output, tools, workflows, agents, skills

2. __Sovereign and open alternatives__: how to _choose_ models and providers
    + open and open-weight models, providers, aggregators, local execution
    + European and Italian initiatives, and their maturity
    + how to evaluate models, and how to decide

---

## Prerequisites

- For _everybody_: none, besides curiosity
    + slides are about __concepts__ and __trade-offs__, with short examples and guided exercises
- For _technicians_ willing to run the examples by themselves:
    + basic knowledge of __Python__, __Git__, and the __shell__; basic understanding of __HTTP__ APIs and __JSON__
    + a [Python](https://www.python.org/downloads/) 3.11+ installation, and [Git](https://git-scm.com/downloads)
    + an __API key__ for some LLM provider (e.g. a free one from [Open Router](https://openrouter.ai/)), or a _local_ LLM runtime (e.g. [Ollama](https://ollama.com/))

---

{{% section %}}

{{< slide id="lab-snippets" >}}

## The code of the examples: a repository

- All examples live in a single repository: [`unibo-fc-isi-iag/lab-snippets`]({{< github-url repo="lab-snippets" >}})
    + slides only show _excerpts_: the repository contains the _full_, _commented_, _runnable_ code
    + it is the repository of the University course these lectures are drawn from ("Intelligent Agents", Module 2), hence it contains _more_ than what we will see

- The [README]({{< github-url repo="lab-snippets" path="README.md" >}}) lists all examples, lecture by lecture

---

{{< slide id="lab-snippets-setup" >}}

## The code of the examples: setup

1. Requirements: [Python](https://www.python.org/downloads/) 3.11+, and [Git](https://git-scm.com/downloads)

2. Clone the repository, and install its dependencies _once_, via [Poetry](https://python-poetry.org) (a dependency manager for Python):

    ```bash
    git clone https://github.com/unibo-fc-isi-iag/lab-snippets.git
    cd lab-snippets
    pip install -r requirements.txt  # installs Poetry, if missing
    poetry install                   # creates a virtual environment in .venv/, with all dependencies
    ```

3. Configure the LLM provider via __environment variables__:
    + `OPENAI_API_KEY`: your API key (if missing, snippets ask for it interactively)
    + `OPENAI_BASE_URL` (default: `https://openrouter.ai/api/v1/`), `OPENAI_MODEL` (default: `nvidia/nemotron-3-super-120b-a12b:free`, free)

    ```bash
    export OPENAI_API_KEY="sk-or-v1-..."     # on Windows (PowerShell): $env:OPENAI_API_KEY="sk-or-v1-..."
    ```

4. Open the repository's directory in your IDE (e.g. VS Code), and select the interpreter in `.venv/`

---

{{< slide id="lab-snippets-run" >}}

## The code of the examples: running examples

- Always run snippets _from the root directory_ of the repository, via the __runner__, by lecture `NAME` and example `ID`:

    ```bash
    poetry run python -m snippets --lecture <NAME> --example <ID> [ARGS]
    poetry run python -m snippets -l <NAME> -e <ID> [ARGS]               # same, but shorter
    poetry run python -m snippets -l prompting -e 1bis data/letter-mario-rossi.txt   # e.g.
    ```

    - `poetry run`: runs the command _inside_ the project's virtual environment (`.venv/`), with all dependencies available
    - `python -m snippets`: runs the `snippets` package as a program (i.e. its `__main__.py`, the runner)
    - `ARGS` are passed to the snippet (each example's slides show which ones it accepts)
    - prefer the runner to `python path/to/snippet.py`: same command for every snippet, no paths to remember

- Don't remember the `ID`? List the snippets of a lecture (or all of them, omitting `-l`):

    ```text
    $ poetry run python -m snippets -l llmaas --list
    llmaas/example1: Example 1: Sync CLI Chat, with OpenAI's Chat Completions API.
         snippets/lecture_llmaas/example1/repl_chat_openai.py
    llmaas/example1bis: Example 1 (bis): the same CLI Chat, with Anthropic's Messages API (served by a local Ollama).
         snippets/lecture_llmaas/example1bis/repl_chat_anthropic.py
    llmaas/example2: Example 2: Async CLI Chat with Streaming, with OpenAI's Chat Completions API.
         snippets/lecture_llmaas/example2/repl_chat_openai_async.py
    llmaas/example3: Example 3: CLI Chat with Attachments, with OpenAI's Responses API (stateless or stateful).
         snippets/lecture_llmaas/example3/repl_chat_responses.py
    ...
    ```

{{% /section %}}

---

{{% import path="reusable/back.md" %}}
