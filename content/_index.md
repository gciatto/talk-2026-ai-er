+++

title = "Informazioni sulle lezioni"
description = "API per LLM, agenti e modelli aperti: due lezioni del 'Corso di IA — Regione Emilia-Romagna 2026'"
outputs = ["Reveal"]

+++

{{% import path="reusable/front.md" %}}

---

{{< slide id="toc" >}}

## Indice

0. [Informazioni sulle lezioni](./) (questa pagina):
    + [l'esempio ricorrente](llmaas/#/running-example) · [il codice degli esempi](#/lab-snippets)
1. [LLM-as-a-Service: API, strumenti e agenti](llmaas) (27/10/2026, 4h)
    + [scelte architetturali](llmaas/#/llmaas) · [API](llmaas/#/apis) · [integrazione](llmaas/#/integration) · [le API OpenAI in pratica](llmaas/#/openai-in-practice) · [dal testo ai processi](llmaas/#/processes) · [IA agentica](llmaas/#/agentic-ai) · [costruire un agente](llmaas/#/building-an-agent)
    + esercizio guidato 1. [stesso programma, fornitori diversi](llmaas/#/exercise-providers) · esercizio guidato 2. [cambiare lo schema dell'output](llmaas/#/exercise-schema)
2. [Alternative sovrane e aperte ai grandi LLM](governance) (18/11/2026, 3h)
    + [modelli aperti](governance/#/open-landscape) · [fornitori](governance/#/providers-landscape) · [esecuzione locale](governance/#/local) · [sovranità europea](governance/#/eu-sovereignty) · [Italia](governance/#/italy) · [valutazione](governance/#/evaluation) · [decisione](governance/#/decision)
    + visita guidata: [tre cataloghi di modelli](governance/#/zoo-tour) · esercizio guidato 1. [confrontare i modelli](governance/#/exercise-compare) · esercizio guidato 2. [motivare e documentare una decisione](governance/#/exercise-decision)

---

## Docente

### Giovanni Ciatto
* email: [`giovanni.ciatto@unibo.it`](mailto:giovanni.ciatto@unibo.it)
* pagina personale: <https://www.unibo.it/sitoweb/giovanni.ciatto/en>
* Dipartimento di Informatica — Scienza e Ingegneria (DISI), Università di Bologna

---

## Obiettivi

1. __LLM-as-a-Service__: come _integrare_ gli LLM nelle applicazioni
    + scelte architetturali e strategiche: architetture cloud, on-premise e ibride
    + l'ecosistema delle API e le API OpenAI come standard _de facto_
    + dalla generazione di testo ai _processi_: output strutturato, strumenti, workflow, agenti, skill

2. __Alternative sovrane e aperte__: come _scegliere_ modelli e fornitori
    + modelli aperti e a pesi aperti, fornitori, aggregatori, esecuzione locale
    + iniziative europee e italiane e loro maturità
    + come valutare i modelli e come decidere

---

## Prerequisiti

- Per _tutti_: nessuno, oltre alla curiosità
    + le slide trattano __concetti__ e __compromessi__, con brevi esempi ed esercizi guidati
- Per i _tecnici_ che vogliono eseguire gli esempi in autonomia:
    + conoscenze di base di __Python__, __Git__ e della __shell__; nozioni di base sulle API __HTTP__ e su __JSON__
    + [Python](https://www.python.org/downloads/) 3.11+ e [Git](https://git-scm.com/downloads) installati
    + una __chiave API__ di un fornitore di LLM (ad es. una chiave gratuita ottenuta da [Open Router](https://openrouter.ai/)), oppure un ambiente di esecuzione _locale_ per LLM (ad es. [Ollama](https://ollama.com/))

---

{{% section %}}

{{< slide id="lab-snippets" >}}

## Il codice degli esempi: un repository

- Tutti gli esempi sono in un unico repository: [`unibo-fc-isi-iag/lab-snippets`]({{< github-url repo="lab-snippets" >}})
    + le slide mostrano solo _estratti_: il repository contiene il codice _completo_, _commentato_ ed _eseguibile_
    + è il repository del corso universitario da cui sono tratte queste lezioni ("Agenti intelligenti", Modulo 2, Università di Bologna), quindi contiene _più_ di quanto vedremo
    + gli esempi sono organizzati per _lezione di quel corso_ (`snippets/lecture_<NOME>/`): i nomi delle cartelle (es. `lecture_prompting`, `lecture_agents`, `lecture_skills`) __non__ corrispondono alle due lezioni di questo corso, e le slide indicano sempre il percorso esatto

- Il [README]({{< github-url repo="lab-snippets" path="README.md" >}}) elenca tutti gli esempi, lezione per lezione

---

{{< slide id="lab-snippets-setup" >}}

## Il codice degli esempi: configurazione

1. Requisiti: [Python](https://www.python.org/downloads/) 3.11+ e [Git](https://git-scm.com/downloads)

2. Clonare il repository e installare le dipendenze _una sola volta_ con [Poetry](https://python-poetry.org) (un gestore di dipendenze per Python):

    ```bash
    git clone https://github.com/unibo-fc-isi-iag/lab-snippets.git
    cd lab-snippets
    pip install -r requirements.txt  # installs Poetry, if missing
    poetry install                   # creates a virtual environment in .venv/, with all dependencies
    ```

3. Configurare il fornitore di LLM tramite __variabili d'ambiente__:
    + `OPENAI_API_KEY`: la propria chiave API (se manca, gli snippet la chiedono interattivamente)
    + `OPENAI_BASE_URL` (valore predefinito: `https://openrouter.ai/api/v1/`), `OPENAI_MODEL` (valore predefinito: `nvidia/nemotron-3-super-120b-a12b:free`, gratuito)

    ```bash
    export OPENAI_API_KEY="sk-or-v1-..."     # on Windows (PowerShell): $env:OPENAI_API_KEY="sk-or-v1-..."
    ```

4. Aprire la directory del repository nel proprio IDE (ad es. VS Code) e selezionare l'interprete in `.venv/`

---

{{< slide id="lab-snippets-run" >}}

## Il codice degli esempi: esecuzione

- Eseguire sempre gli snippet _dalla directory principale_ del repository, tramite il __runner__, indicando la lezione `NAME` e l'esempio `ID`:

    ```bash
    poetry run python -m snippets --lecture <NAME> --example <ID> [ARGS]
    poetry run python -m snippets -l <NAME> -e <ID> [ARGS]               # same, but shorter
    poetry run python -m snippets -l prompting -e 1bis data/letter-mario-rossi.txt   # e.g.
    ```

    - `poetry run`: esegue il comando _nell'ambiente virtuale_ del progetto (`.venv/`), con tutte le dipendenze disponibili
    - `python -m snippets`: esegue il pacchetto `snippets` come programma (cioè il suo `__main__.py`, il runner)
    - gli argomenti `ARGS` vengono passati allo snippet (le slide di ciascun esempio indicano quali argomenti accetta)
    - preferire il runner a `python path/to/snippet.py`: lo stesso comando per ogni snippet, senza percorsi da ricordare

- Non si ricorda l'`ID`? Elencare gli snippet di una lezione (o tutti, omettendo `-l`):

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
