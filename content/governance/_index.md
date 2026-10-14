
+++

title = "[AI-ER] Alternative sovrane e aperte ai grandi LLM"
description = "Modelli aperti, fornitori, esecuzione locale, iniziative europee e italiane, valutazione e scelta dei modelli"
outputs = ["Reveal"]

+++

# Alternative sovrane e aperte ai grandi LLM

{{% import path="reusable/footer.md" %}}

---

## Programma

1. [Il panorama dei modelli aperti e a pesi aperti](#/open-landscape)
    + cataloghi di modelli (Hugging Face, Ollama, Open Router); parametri chiave; cosa significa "aperto"; licenze
2. [Modelli, fornitori e piattaforme](#/providers-landscape): chi offre cosa
    + fornitori primari, aggregatori, marketplace; Open Router; portabilità e lock-in
3. [Esecuzione locale](#/local) e sovranità infrastrutturale
    + Ollama; requisiti hardware; quando è sostenibile?
4. [Sovranità digitale europea](#/sovereignty): iniziative e loro maturità
5. [Il panorama italiano](#/italy)
6. [Come valutare un LLM](#/evaluation)
    + benchmark e loro limiti; valutazione realistica per un'organizzazione
7. [Criteri di scelta per l'adozione](#/decision)
    + cloud, fornitore europeo o on-premise? Come costruire una rosa di candidati

---

## Riprendiamo: dove deve essere eseguito il modello?

- Dalla [lezione precedente](../llmaas/#/deployment-options), tre _opzioni di deployment_:
    1. __API cloud proprietaria__ (es. OpenAI, Anthropic, Google)
    2. __modello aperto, ospitato da terzi__ (es. tramite Open Router, un fornitore cloud UE)
    3. __on-premise__ (es. tramite Ollama, vLLM)
- ... da scegliere secondo _cinque dimensioni_: qualità, costo, controllo, conformità, time-to-market (cfr. il [confronto](../llmaas/#/deployment-options))
- Le opzioni 2 e 3 richiedono modelli __a pesi aperti__: _quali_, di _chi_, a quali _condizioni_ e quanto sono _validi_?

---

{{< slide id="open-landscape" >}}

# Il panorama dei modelli aperti e a pesi aperti

---

{{% section %}}

{{< slide id="model-zoos" >}}

## Cataloghi di modelli (pt. 1): il concetto generale

- Un __model zoo__ (detto anche _hub_, _catalogo_ o _libreria_ di modelli) è una _raccolta consultabile_ di modelli messi a disposizione da un fornitore, ciascuno con:
    + un __nome__ (e _versioni_/_varianti_), da usare nelle chiamate API o per il download
    + una __model card__: un documento che descrive _scopo_, _dati di addestramento_, risultati della _valutazione_, _limiti_ e _usi previsti_ del modello (cfr. [Mitchell et al., 2019](https://arxiv.org/abs/1810.03993))
    + _metadati tecnici_: dimensione, _finestra di contesto_, _modalità_ supportate (testo, immagini, audio), supporto per _tool_ / _output strutturati_ / _ragionamento_
    + metadati _economici_ e _giuridici_: prezzo per token (per i cataloghi cloud), __licenza__ (per i modelli scaricabili)

- I cataloghi sono il _punto di partenza_ per la __scelta del modello__, un'attività _ricorrente_ nei progetti di GenAI: si ripete a ogni _nuovo modello_ disponibile, o quando cambiano _compito_ o _dati_


{{% multicol %}}
{{% col class="col-5" %}}
- __Esempio__: [Hugging Face Hub](https://huggingface.co/models) è un catalogo con milioni di modelli per qualsiasi compito e modalità,
    - ciascuno con una model card, metadati tecnici e informazioni sulla licenza
    - es. filtrare per "Gemma" per vedere la [famiglia Gemma](https://huggingface.co/models?sort=trending&search=gemma) di LLM, poi selezionare un modello per aprirne la scheda
{{% /col %}}
{{% col class="col-7" %}}
{{< image src="./hugging-face-model-zoo.png" max-w="100%" max-h="34vh" alt="Schermata della model card di Gemma su Hugging Face" >}}
{{% /col %}}
{{% /multicol %}}

---

## Cataloghi di modelli (pt. 2): il panorama tecnologico

{{% small "80%" %}}

| | [Hugging Face Hub](https://huggingface.co/models) | [Libreria Ollama](https://ollama.com/models) | [Open Router](https://openrouter.ai/models) |
|---|---|---|---|
| __Tipo__ | download (+ alcune opzioni di inferenza ospitata) | download, esecuzione locale | cloud, pagamento per token |
| __Dimensione__ | milioni di modelli (qualsiasi compito e modalità) | centinaia di LLM selezionati | centinaia di LLM da decine di fornitori |
| __Nomi__ | `organization/model-name` | `model:variant` (es. `gemma4:e2b`) | `provider/model[:tier]` (es. `google/gemma-4-26b-a4b-it:free`) |
| __Varianti__ | repository separati (es. `...-GGUF`, `...-AWQ`) | _tag_ per dimensione e quantizzazione | una voce per modello, servito da più fornitori |
| __Metadati__ | model card completa, file, licenza, comunità | dimensione, contesto, capacità, quantizzazione | prezzo, contesto, statistiche di latenza e throughput (token al secondo), parametri supportati |
| __Licenza__ | per modello (mostrata come tag) | per modello (nella pagina del modello) | si applicano i _termini di servizio_ del fornitore |

{{% /small %}}

{{% small "85%" %}}

### Convenzioni ricorrenti nei nomi (da saper decifrare)

- _famiglia_ + _versione_: `llama-3.1`, `qwen3`, `gemma-4`
- _dimensione_: `8b`, `70b` o `26b-a4b` (MoE: parametri totali vs. attivi), oppure `e2b` (2B _effettivi_: ~2,3 miliardi di parametri di Gemma usati nel calcolo, 5,1 miliardi includendo gli embedding per strato)
- _specializzazione_: `base` (puro predittore del token successivo) vs. `instruct` / `it` / `chat` (adattato a seguire istruzioni: __è ciò che serve__ per Chat Completion), oppure `coder`, `vl` / `vision`, `thinking`
- _quantizzazione_ / _formato_: `Q4_K_M`, `GGUF`, `AWQ`, `fp8`
- _data_ / _snapshot_: `gpt-4o-2024-08-06` (fissare la versione per la _riproducibilità_!)

{{% /small %}}

---

## Cataloghi di modelli (pt. 3): e poi?

- I cataloghi indicano _quali_ modelli esistono e cosa _dichiarano_ di saper fare
- Serve un modello che supporti _tool_ o _output strutturati_?
    + Ollama: cercare i badge `tools`, `thinking`, `vision` (es. [filtro per tool](https://ollama.com/search?c=tools))
    + Open Router: filtrare la pagina dei modelli per _parametri supportati_ (es. `tools`, `structured_outputs`, `response_format`)
- __Scegliere__ il modello adatto a un compito e __valutarlo__ sulla base di _evidenze_ sono decisioni che incidono su qualità, costo, controllo e conformità
    + ne parleremo [più avanti](#/model-selection), con un esercizio guidato di confronto dei modelli sull'esempio ricorrente

---

{{< slide id="key-parameters" >}}

## I parametri chiave, a colpo d'occhio

{{% small "85%" %}}

| Parametro | Dove trovarlo | Perché conta |
|---|---|---|
| __Dimensione__ (parametri; _totali_ vs. _attivi_, per MoE) | nome (es. `26b-a4b`), model card | qualità (indicativamente), memoria e hardware necessari, velocità |
| __Costo__ | prezzo per 1M di token in ingresso / uscita (cloud); hardware + energia (locale) | budget operativo; varia tra fornitori per lo _stesso_ modello |
| __Licenza__ | model card, file della licenza | cosa è _legalmente_ consentito: uso commerciale, fine-tuning, hosting, territorio |
| __Finestra di contesto__ | model card, metadati del catalogo | quanti documenti / quanta cronologia entrano in una richiesta |
| __Modalità__ | model card, badge (es. `vision`) | solo testo? immagini (es. documenti scansionati)? audio? |
| __Capacità operative__ | badge, _parametri supportati_ (es. `tools`, `structured_outputs`) | può chiamare tool, produrre output strutturati, _ragionare_? |
| __Lingue__ | model card, rapporto tecnico | l'italiano è supportato _bene_ (non soltanto "supportato")? |
| __Limite temporale delle conoscenze__, __data di rilascio__ | model card | quanto è aggiornato; quanto manca alla sua _dismissione_ |

{{% /small %}}

- I metadati del catalogo sono _dichiarazioni_ del fornitore: __verificare__ ciò che conta per il proprio caso d'uso (cfr. [valutazione](#/evaluation))

---

{{< slide id="zoo-tour" >}}

## Esplorazione guidata: lo stesso modello in tre cataloghi

> __Obiettivo__: imparare a leggere i cataloghi cercando lo _stesso_ modello a pesi aperti (es. Gemma 4) in tutti e tre

1. [Hugging Face Hub](https://huggingface.co/models?search=gemma-4): trovare la model card di `google/gemma-4-26b-a4b-it`
    + licenza? lingue? finestra di contesto? valutazioni? quanti modelli _derivati_ (fine-tuning, quantizzazioni) esistono?
2. [Libreria Ollama](https://ollama.com/library/gemma4): osservare i _tag_ di `gemma4`
    + quali dimensioni e quantizzazioni? quanti GB ciascuno? quali _badge_ (`vision`, `tools`, `thinking`)?
3. [Open Router](https://openrouter.ai/models?q=gemma-4): osservare `google/gemma-4-26b-a4b-it`
    + quanti _fornitori_ lo offrono? a quali prezzi, con quale latenza e throughput? esiste una variante `:free`? con quali limiti?

{{% fragment %}}
> Stessi _pesi_, tre _offerte_ molto diverse: scaricare il modello ed eseguirlo in autonomia, oppure usarlo pagando un fornitore per token. I __pesi aperti__ offrono la libertà di scegliere _dove_ eseguirlo
{{% /fragment %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="open-models" >}}

## Cosa rende un modello _"aperto"_? Il concetto generale

Quando viene rilasciato un modello, possono essere pubblicati diversi __artefatti__, ciascuno con un proprio grado di _apertura_. La disponibilità dei pesi dice poco sugli altri componenti.

| Componente | Cosa contiene un rilascio utile | Cosa permette |
|---|---|---|
| __Pesi__ | parametri finali (idealmente anche checkpoint intermedi) | inferenza locale, fine-tuning, hosting indipendente |
| __Codice di inferenza__ | architettura, tokenizer, codice di generazione, configurazione | _eseguire_ concretamente i pesi |
| __Codice e ricetta di addestramento__ | elaborazione dei dati, iperparametri, pipeline di addestramento e post-addestramento (SFT, RL) | capire _come_ sono stati ottenuti i pesi; riaddestrare |
| __Dati di addestramento__ | l'effettiva miscela di dati, pronta per l'addestramento | audit dei dati, analisi della contaminazione, riproduzione esatta |
| __Informazioni sui dati__ | provenienza, fonti, processi di filtraggio e selezione | un sostituto (più debole) quando i dati non sono redistribuibili |
| __Documentazione__ | model card, rapporto tecnico, valutazioni, limiti | verificabilità, valutazione dei rischi a valle |
| __Licenze__ | licenze _distinte_ per pesi, codice e dati | sapere cosa è _legalmente_ consentito fare con tutto quanto sopra |

---

## L'apertura ha _gradi_, non è binaria

{{< image src="./openness-bar.svg" max-h="30vh" alt="Apertura di sette componenti del rilascio per un modello chiuso via API, un modello a pesi aperti (Llama 4) e un modello completamente aperto (OLMo 3): i pesi scaricabili non bastano a rendere aperto un modello" >}}

{{% small "80%" %}}

| Quadro di riferimento | A quale domanda risponde |
|---|---|
| [Liesenfeld & Dingemanse, FAccT 2024](https://doi.org/10.1145/3630106.3659005) | quanto è aperto un rilascio _in pratica_? (14 dimensioni, 40 LLM testuali + 6 modelli testo-immagine) |
| [European Open Source AI Index](https://osai-index.eu/database/) | quali artefatti di scienza aperta sono stati effettivamente pubblicati? (punteggio su una scala; ai primi posti a luglio 2026: YuLan-Mini, BLOOMZ, OLMo 3, Apertus) |
| [Stanford Foundation Model Transparency Index](https://crfm.stanford.edu/fmti/December-2025/index.html) (dic. 2025) | quanto sono _trasparenti_ i fornitori? (media 41/100, 17 punti in meno del 2024; IBM 95, DeepSeek 32, Alibaba 26: pesi aperti $\neq$ trasparenza) |
| [Definizione OSI di IA open source](https://opensource.org/ai/open-source-ai-definition) | un sistema soddisfa una definizione _normativa_ di "IA open source"? (prossima slide) |

- __Open-washing__: presentare un modello come "aperto" o "open source" sulla base di _un solo_ componente pubblicato (tipicamente i pesi), mentre mancano gli artefatti o le libertà giuridiche necessari per studiarlo, riprodurlo, modificarlo o usarlo liberamente
    + es. _Llama 4_ (pesi scaricabili, licenza restrittiva), _Gemma 3_ (termini specifici con restrizioni d'uso), _Mistral 3_ (definito ["open-source"](https://mistral.ai/news/mistral-3/) dal produttore: pesi Apache 2.0, dati di addestramento non divulgati)
    + OpenAI invece definisce [gpt-oss](https://openai.com/index/introducing-gpt-oss/) "_open-weight_": il termine più preciso

{{% /small %}}

---

## La definizione OSI di IA open source (OSAID 1.0, ott. 2024)

- Gli utenti devono poter, _senza chiedere il permesso_:
    1. __usare__ il sistema per _qualsiasi_ scopo
    2. __studiare__ come funziona
    3. __modificarlo__ per qualsiasi scopo
    4. __condividerlo__, con o senza modifiche
- Serve quindi la _forma preferita per apportare modifiche_:
    + __informazioni sui dati__: dettagli sufficienti sui dati di addestramento affinché una persona competente possa costruire un sistema _sostanzialmente equivalente_
    + __codice__: la pipeline _completa_ di addestramento e inferenza
    + __parametri__: pesi e configurazione distribuiti con condizioni che ne consentano uso, studio, modifica e condivisione
- __Controversia__: OSAID __non__ richiede di pubblicare i dati di addestramento stessi (cfr. [FAQ di OSI](https://opensource.org/ai/faq))
    + OSI: i dati possono essere personali, protetti dal diritto d'autore o riservati; richiederli escluderebbe troppi sistemi
    + critici: senza i dati, non sono possibili la riproduzione esatta né la verifica dei dati di addestramento
- Nella [validazione iniziale](https://opensource.org/ai/final-board-report) di OSI, _Pythia_, _OLMo_, _Amber_, _CrystalCoder_ e _T5_ risultavano conformi; _Llama 2_, _Grok_, _Phi-2_ e _Mixtral_ no
    + una _fotografia_ del 2024, __non__ un registro di certificazione delle versioni successive

---

## Il panorama delle licenze (a settembre 2026)

| Categoria | Esempi | Cosa è aperto | Limite principale |
|---|---|---|---|
| __Chiuso__ (solo API) | GPT, Claude, Gemini, modelli "Premier" di Mistral | API + documentazione | nessun peso; uso regolato dai _termini di servizio_ |
| __Pesi aperti, licenza restrittiva__ | [Llama 4](https://www.llama.com/llama4/license/), [Gemma 3](https://ai.google.dev/gemma/terms), [Teuken v0.6](https://huggingface.co/openGPT-X/Teuken-7B-instruct-v0.6), BLOOM ([RAIL](https://huggingface.co/bigscience/bloom-560m/blob/main/LICENSE)) | pesi, codice di inferenza | restrizioni d'uso, geografiche, commerciali o a valle: _non_ "qualsiasi scopo" |
| __Pesi aperti, licenza permissiva__ | [Gemma 4](https://ai.google.dev/gemma/docs/core/model_card_4), [Qwen3](https://huggingface.co/Qwen/Qwen3-8B), [DeepSeek-R1](https://huggingface.co/deepseek-ai/DeepSeek-R1), [gpt-oss](https://huggingface.co/openai/gpt-oss-120b), [Mistral Large 3 / Small 4](https://docs.mistral.ai/getting-started/models/) | pesi con licenza Apache 2.0 o MIT | dati di addestramento e pipeline completa _non_ pubblicati |
| __Completamente (o quasi) aperto__ | [OLMo 3](https://allenai.org/blog/olmo3), [Apertus](https://www.apertus-ai.org/), [Minerva](https://huggingface.co/sapienzanlp/Minerva-7B-base-v1.0) | pesi + codice + dati (o pipeline dei dati) + documentazione | la riproduzione richiede comunque risorse di calcolo _enormi_ |

---

## Il panorama delle licenze (a settembre 2026)

### Le licenze cambiano tra versioni: verificare sempre il __checkpoint esatto__

| Modello | Prima | Ora |
|---|---|---|
| __Gemma__ | Gemma 3: termini Gemma specifici, con restrizioni d'uso a cascata | Gemma 4 (2026): __Apache 2.0__ |
| __DeepSeek-V3__ | V3 originale: DeepSeek Model Agreement specifico | V3-0324: [rilasciato nuovamente con licenza __MIT__](https://api-docs.deepseek.com/news/news250325) (marzo 2025) |
| __Mistral__ | storicamente eterogeneo | _ancora_ eterogeneo: Large 3 e Small 4 Apache 2.0, Medium 3.5 "Modified MIT", diversi modelli proprietari |
| __Teuken__ | v0.4-commercial: Apache 2.0 | v0.6: __non commerciale__ ("più recente" non significa "più permissivo") |
| __iGenius → Domyn__ | Italia 9B: MIT; Italia 10B: [licenza specifica, non aperta](https://secure.igenius.ai/legal/iGenius%2BLicense%2BItalia%2B10B.pdf) | [rinominata Domyn](https://www.domyn.com/news/introducing-domyn) (giugno 2025); [Domyn Small 10B](https://www.domyn.com/news/introducing-domyn-small-an-open-european-reasoning-llm-built-for-ai-ownership) (maggio 2026): MIT |

---

## Cosa significano in pratica le licenze restrittive?

| Licenza | Uso on-premise | Fine-tuning | Servizio ospitato | Redistribuzione |
|---|---|---|---|---|
| __Llama 4__ ([licenza](https://github.com/meta-llama/llama-models/blob/main/models/llama4/LICENSE), [AUP](https://github.com/meta-llama/llama-models/blob/main/models/llama4/USE_POLICY.md)) | sì, __tranne__ i modelli multimodali per persone/aziende _con sede nell'UE_ | sì | sì (gli _utenti finali_ UE dei prodotti sono esclusi dal divieto) | attribuzione _"Built with Llama"_; modelli derivati denominati _"Llama..."_; licenza separata oltre 700M di utenti attivi mensili |
| __Gemma 3__ ([termini](https://ai.google.dev/gemma/terms)) | sì, rispettando la policy sugli usi vietati | sì, i derivati ereditano le restrizioni | conta come _distribuzione_ | le restrizioni vanno trasmesse agli utenti a valle; Google può limitare l'uso _"da remoto o altrimenti"_ (un diritto contrattuale, non un interruttore di spegnimento) |
| __OpenRAIL__ ([OpenRAIL-M](https://www.licenses.ai/blog/2022/8/26/bigscience-open-rail-m-license)) | sì, per scopi consentiti | sì, per scopi consentiti | sì, utenti vincolati alle restrizioni d'uso | le restrizioni d'uso vanno trasmesse a valle |
| __Apache 2.0 / MIT__ | sì | sì | sì | sì, con attribuzione/avvisi (e clausole sui brevetti, per Apache) |

{{% fragment %}}
> __Esempio ricorrente__: l'assistente alle ammissioni deve leggere _immagini di passaporti_, quindi serve un modello __multimodale__. L'Università di Bologna è un'organizzazione UE: la policy di uso accettabile di Llama 4 __non__ le concede il diritto di usare i modelli multimodali Llama 4. Gemma 4, Qwen3 o i modelli Apache di Mistral non presentano questo problema
{{% /fragment %}}

---

## Derivati: fine-tuning, distillazione, fusioni, dati, output

- I modelli ottenuti con __fine-tuning__ restano soggetti alla licenza del modello di base: semplice con Apache/MIT, le restrizioni si _propagano_ con Gemma 3 e Llama
- La __distillazione__ dipende dalla licenza:
    + Gemma 3 considera _Model Derivatives_ i modelli addestrati sugli _output_ di Gemma (dati sintetici)
    + DeepSeek-R1 (MIT) consente la distillazione, ma i suoi modelli _distillati_ pubblicati sono fine-tuning di altri modelli e __ne ereditano le licenze__ ([Qwen](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Qwen-32B) → Apache 2.0, [Llama](https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B) → licenza Llama)
- __Fusioni__: una fusione dei modelli A e B deve (prudenzialmente) rispettare __entrambe__ le licenze; una componente restrittiva non viene "cancellata"
    + la possibilità stessa di proteggere i pesi con il diritto d'autore è giuridicamente _ancora incerta_
- I __dati di addestramento__ hanno una licenza _propria_: es. OLMo 3 è Apache 2.0, ma il suo dataset [Dolma 3](https://huggingface.co/datasets/allenai/dolma3_mix-6T) è ODC-By (attribuzione obbligatoria) e le fonti possono prevedere ulteriori condizioni
- __Output__: "il produttore non rivendica diritti sugli output" $\neq$ "gli output sono privi di restrizioni"
    + es. usare _output_ Llama per addestrare un modello distribuito fa scattare il requisito Llama sul _nome_
    + i fornitori proprietari offrono _tutele contrattuali in caso di contestazioni per violazione del diritto d'autore_ relative agli output (es. Copilot Copyright Commitment di Microsoft, indennizzo per IA generativa di Google, Copyright Shield di OpenAI, termini commerciali di Anthropic), solitamente solo per piani a pagamento o enterprise e a determinate condizioni; le licenze a pesi aperti non ne offrono
    + in Italia sono protette soltanto le opere _umane_ ([L. 132/2025](https://www.gazzettaufficiale.it/eli/id/2025/09/25/25G00143/sg), Art. 25)

---

{{< slide id="open-models-ai-act" >}}

## I modelli aperti nell'AI Act dell'UE

{{% small "85%" %}}

| Obbligo del fornitore GPAI ([Art. 53](https://artificialintelligenceact.eu/article/53/)) | Modelli GPAI ordinari | Modelli GPAI liberi/open source |
|---|---|---|
| (a) documentazione tecnica (per l'AI Office) | obbligatoria | __esenzione__ |
| (b) informazioni per i fornitori a valle | obbligatorie | __esenzione__ |
| (c) policy di conformità al _diritto d'autore_ UE (incl. riserve sul text and data mining) | obbligatoria | __ancora obbligatoria__ |
| (d) _sintesi dei contenuti di addestramento_ pubblica (modello dell'AI Office) | obbligatoria | __ancora obbligatoria__ |
| obblighi per rischio sistemico ([Art. 55](https://artificialintelligenceact.eu/article/55/)) | se > 10<sup>25</sup> FLOPs (presunzione dell'[Art. 51(2)](https://artificialintelligenceact.eu/article/51/)) | __nessuna esenzione__ |

{{% /small %}}

- Per rientrare nell'esenzione, il modello deve avere una licenza che consenta _accesso, uso, modifica e distribuzione_, con pesi, architettura e informazioni d'uso _pubblici_
    + un requisito molto __più debole__ di OSAID: non servono dati né codice di addestramento
- I fornitori extra-UE di GPAI open source sono anche esentati dal nominare un _rappresentante autorizzato_ nell'UE ([Art. 54(6)](https://artificialintelligenceact.eu/article/54/)), salvo modelli con rischio sistemico
- cfr. le [FAQ dell'AI Act Service Desk](https://ai-act-service-desk.ec.europa.eu/en/ai-act/faq/how-does-ai-act-apply-general-purpose-ai-models-released-open-source) della Commissione sui modelli GPAI open source

> Per i modelli GPAI open source sono previste __esenzioni da alcuni obblighi documentali__. Restano gli obblighi sul __diritto d'autore__ e sulla __sintesi dei dati di addestramento__; non sono previste esenzioni per i modelli con __rischio sistemico__

{{% /section %}}

---

{{< slide id="providers-landscape" >}}

# Modelli, fornitori e piattaforme

Chi offre cosa

---

{{% section %}}

{{< slide id="provider-kinds" >}}

## Fornitori primari, aggregatori e marketplace

{{% small "90%" %}}

| Tipo | Cosa offrono | Esempi | Contratto con |
|---|---|---|---|
| __Fornitori primari__ (sviluppatori dei modelli) | i _propri_ modelli tramite la propria API (e app) | OpenAI, Anthropic, Google, Mistral, DeepSeek | lo sviluppatore |
| __Fornitori di inferenza__ | modelli _a pesi aperti_ (altrui), eseguiti sul proprio hardware | Together AI, Groq, Fireworks, DeepInfra, cloud UE (es. Scaleway, OVHcloud) | chi ospita il modello |
| __Piattaforme cloud__ (hyperscaler) | molti modelli (proprietari _e_ aperti) in un ecosistema cloud | Azure AI Foundry, AWS Bedrock, Google Vertex AI | il fornitore cloud |
| __Aggregatori__ (router) | _una_ API e _una_ fattura per modelli di _molti_ fornitori | Open Router | l'aggregatore (e indirettamente ogni fornitore a monte) |
| __Marketplace__ / hub | modelli da _scaricare_ e (alcune opzioni di) inferenza ospitata | Hugging Face | dipende dalla licenza del modello e da chi lo ospita |

- Lo _stesso_ modello a pesi aperti è accessibile in _molti_ modi: confrontare __prezzo__, __latenza__, __localizzazione dei dati__ e __condizioni__ (es. conservazione dei dati, addestramento sui propri dati)
- Più _intermediari_ ci sono, più contratti e giurisdizioni: chiedersi _dove_ vengono elaborati i dati, non solo _chi_ ha addestrato il modello

{{% /small %}}

{{< image src="./provider-chain.svg" max-h="20vh" alt="Catena dei fornitori: l'organizzazione (titolare del trattamento) invia i dati a un aggregatore (es. Open Router, USA; ToS e DPA firmati dall'organizzazione), che li inoltra a un fornitore di inferenza (es. Together AI, USA, o Scaleway, UE; contratto con l'aggregatore), che esegue i pesi di uno sviluppatore (es. Qwen3, Alibaba, Cina; solo la licenza dei pesi). I dati transitano dai primi tre, lo sviluppatore non li vede" >}}

---

{{< slide id="open-router" >}}

## Open Router (cfr. <https://openrouter.ai/>), pt. 1

- Open Router (_OR_) offre un servizio __cloud__ per accedere a LLM di diversi fornitori, facilitando il _passaggio dall'uno all'altro_

- La sua caratteristica distintiva è il ruolo di _aggregatore_ di più fornitori: permette di accedere a modelli diversi tramite un'unica API e aiuta a evitare il lock-in
    + OR è utilizzabile __gratuitamente__, con _limiti molto stretti_ (20 richieste al minuto, massimo 50 al giorno)
        * si possono __acquistare crediti__ per aumentare i limiti dei modelli gratuiti (dettagli in <https://openrouter.ai/docs/api_reference/limits>) o usare modelli _a consumo_
        * altre API con quote gratuite, tutte compatibili con l'API OpenAI (ottobre 2026): [Google Gemini API](https://ai.google.dev/gemini-api/docs/pricing), [Mistral](https://mistral.ai/pricing/) (piano Free), [Groq](https://console.groq.com/docs/rate-limits), [Cohere](https://docs.cohere.com/docs/rate-limits) (chiave di prova, solo uso non commerciale)

- Un [catalogo di modelli](https://openrouter.ai/models) piuttosto ricco, con modelli di <u>molti</u> fornitori: in particolare OpenAI, Anthropic, Google, xAI, Mistral ecc.
    + i modelli hanno un __prezzo per token__, normalmente espresso in `$/1M tokens` (talvolta diverso per i token in _ingresso_ e in _uscita_)
        * è molto comune nei servizi cloud: occorre tenerne conto durante gli esperimenti per evitare costi inattesi
        * ricordare che _token_ $\neq$ _parola_: i token sono frammenti di testo definiti dal _tokenizzatore_ del modello (cfr. [esempio interattivo](https://platform.openai.com/tokenizer)), e la stessa frase può costare token diversi su modelli diversi

    {{< image src="open-router-models.png" alt="Catalogo dei modelli di Open Router" max-h="40vh" >}}

---

## Open Router (cfr. <https://openrouter.ai/>), pt. 2

- I modelli nel catalogo OR sono __denominati__ secondo la convenzione `provider/model`, seguita da un eventuale `:tier`, dove:
    + `provider` è il nome del fornitore del servizio (es. `openai`, `qwen`, `google` ecc.)
    + `model` è il nome della _variante_ indicato dal fornitore (es. `gpt-oss-120b`, `qwen3-coder`, `gemma-4-26b-a4b-it` ecc.)
    + `tier` è un suffisso opzionale che indica una variante specifica (di solito `free` per i modelli gratuiti)


- L'__instradamento dinamico__ delle richieste avviene su due livelli:
    + tra __modelli__: `openrouter/auto` ([Auto Router](https://openrouter.ai/docs/guides/routing/routers/auto-router)) lascia scegliere a OR il modello per ogni richiesta in base al contenuto (__a pagamento__: può scegliere modelli a pagamento, quindi non funziona su account senza crediti)
        * i candidati possono essere limitati con pattern _glob_, es. `anthropic/*` o `openai/gpt-5*`, in `allowed_models` del router (da soli __non__ sono nomi `model` validi)
    + tra __fornitori__ di un _dato_ modello: OR sceglie il fornitore che lo esegue ([selezione del fornitore](https://openrouter.ai/docs/guides/routing/provider-selection)); per impostazione predefinita, bilancia il carico e assegna le richieste tenendo conto del prezzo
        * modificabile per richiesta con `provider.sort` = `price` | `latency` | `throughput` (token al secondo), oppure nelle [impostazioni di routing](https://openrouter.ai/workspaces/default/routing) del proprio account OR
        * oppure con _suffissi_ del modello: `:floor` (più economico), `:nitro` (throughput più alto), [`:exacto`](https://openrouter.ai/docs/guides/routing/model-variants/exacto) (fornitori con migliore qualità nel tool calling, es. `moonshotai/kimi-k2-0905:exacto`)

- __Bring your own API key__ (BYOK, utilizzo delle proprie chiavi API): OR permette di _collegare_ all'account le proprie chiavi di _altri fornitori_, accedendo ai loro modelli tramite l'API OR e pagando secondo i loro prezzi anziché quelli di OR
    + utile per evitare il lock-in e avere più controllo su costi e modelli da usare

> Instradamento dinamico e BYOK sono caratteristiche _distintive_ di OR; per il resto è un "normale" fornitore cloud

---

## Esempio cloud con Open Router (pt. 1)

1. __Creare__ un account Open Router e _accedere_ al servizio: <https://openrouter.ai>
    + è più semplice riutilizzare l'account GitHub

2. Facoltativamente, aggiungere __metodi di pagamento__ e _crediti_ all'account in <https://openrouter.ai/settings/credits>
    + per gli esempi di queste lezioni non servono crediti
    + se si sperimenta spendendo denaro, impostare un _limite di spesa_ per evitare costi inattesi

    {{< image src="open-router-credits.png" alt="Impostazioni dei crediti di Open Router" max-h="40vh" >}}

---

## Esempio cloud con Open Router (pt. 2)

3. Controllare periodicamente la __dashboard delle attività__ dell'account per verificare utilizzo e costi delle richieste: <https://openrouter.ai/activity>

    {{< image src="open-router-activity.png" alt="Dashboard delle attività di Open Router" max-h="40vh" >}}

---

## Esempio cloud con Open Router (pt. 3)

4. Per usare i modelli OR da un programma occorre creare una __chiave API__ (da conservare in modo sicuro e non condividere)
    * (ossia un _token di autenticazione_ che consente ai client di autenticarsi a nome del titolare della chiave)
    * per il _workspace predefinito_, si crea in <https://openrouter.ai/workspaces/default/keys>
        + in OR, i __workspace__ sono _domini amministrativi_ al cui interno si applicano regole di configurazione (es. routing, limiti di costo ecc.)

    {{< image src="open-router-apikeys.png" alt="Impostazioni delle chiavi API di Open Router" max-h="40vh" >}}

    + una chiave API è una stringa del tipo `sk-or-v1-XXXXXXXXXXXXXXXXXXXXXXXX`: basta per <u>spendere il credito dell’account</u> e inviare richieste a nome del titolare

---

## Esempio cloud con Open Router (pt. 4)

5. Si può provare la chiave API con una semplice richiesta `curl`:

    ```bash
    curl -X POST "https://openrouter.ai/api/v1/chat/completions" \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer YOUR_API_KEY" \
        -d '{"model": "nvidia/nemotron-3-super-120b-a12b:free", "messages": [{"role": "user", "content": "What is the capital of France?"}]}'
    ```

---

## Esempio cloud con Open Router (pt. 4, continua)

risposta (convertita in YAML per facilitarne la lettura):

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


        The user asked a straightforward question: "What is the
        capital of France?" The answer is simple: Paris. I
        should respond concisely. My best guess is to confirm:
        "The capital of France is Paris." While adding context
        could be fun, it’s not necessary since the user didn’t
        request it. If I wanted to be helpful, I could mention
        that Paris is the largest city and home to famous
        landmarks like the Eiffel Tower, but I’ll keep it
        brief.**Confirming details on Paris**


        I can keep my response short and straightforward. The
        user asked about the capital of France, and I can simply
        say: "Paris." If I want to offer more, I could say, "The
        capital of France is Paris." I think that covers it! But
        if they are interested, I could mention I''m happy to
        share more details about Paris if they''d like. For now,
        I’ll stick with the essential answer.'
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

## Un client, _molti_ fornitori gratuiti

{{% small "90%" %}}

- Tutti i fornitori citati espongono un'API __compatibile con OpenAI__: cambiano solo `base_url`, chiave e nome del modello

{{% code path="static/lab-snippets/snippets/lecture_free_access/example1/free_providers.py" from="12" to="21" %}}

```bash
poetry run python -m snippets -l free-access -e 1 groq                               # list the models available now
poetry run python -m snippets -l free-access -e 1 groq openai/gpt-oss-20b "Hello!"   # one chat request
```

- `PROVIDERS`: l'_endpoint_ di ogni fornitore e la __variabile d'ambiente__ che contiene la _sua_ chiave (es. `GROQ_API_KEY`, richiesta interattivamente se manca)
    + `ollama`, `vllm`: modelli aperti serviti _localmente_ (sulla propria macchina, cfr. [sotto](#/local)), quindi senza chiave
- Argomenti: `PROVIDER` (obbligatorio), poi opzionalmente `MODEL` e `PROMPT` (senza questi, lo script elenca solo i modelli)
- Script completo [qui](../lab-snippets/snippets/lecture_free_access/example1/free_providers.py) (cfr. [come eseguire gli snippet](../#/lab-snippets-run)); gli ID dei modelli sono _elencati in tempo reale_, non fissati nel codice, perché __diventano obsoleti rapidamente__
- Lo stesso accorgimento funziona con gli script [LLM-as-a-Service](../llmaas/): basta impostare `OPENAI_BASE_URL`, `OPENAI_API_KEY` __e__ `OPENAI_MODEL` (il modello predefinito, `nvidia/nemotron-3-super-120b-a12b:free`, esiste solo su OpenRouter: scegliere un ID dall'elenco precedente), es.
    + Ollama: `OPENAI_BASE_URL=http://localhost:11434/v1 OPENAI_API_KEY=unused OPENAI_MODEL=gpt-oss:20b`
    + Groq: `OPENAI_BASE_URL=https://api.groq.com/openai/v1 OPENAI_API_KEY=... OPENAI_MODEL=openai/gpt-oss-20b`

{{% /small %}}

---

{{< slide id="lock-in" >}}

## Portabilità delle applicazioni e dipendenza dal fornitore

| Cosa | Portabile? | Come mantenerlo portabile |
|---|---|---|
| __API__ (richieste, risposte) | sì, nel _sottoinsieme comune_ delle API compatibili con OpenAI | URL di base, chiave e nome del modello nella _configurazione_; un gateway |
| __Pesi aperti__ | sì: qualsiasi host o hardware proprio | preferire licenze permissive; fissare il checkpoint esatto |
| __Modelli proprietari__ | __no__: possono essere modificati o _ritirati_ secondo i tempi del produttore | avere un'alternativa _già valutata_ |
| __Funzionalità specifiche del fornitore__ (stato lato server, tool integrati, fine-tuning sulla piattaforma) | __no__ | usarle consapevolmente tramite un _adattatore_ |
| __Prompt__ | in parte: ottimizzati per un modello, possono funzionare peggio con un altro | una __suite di valutazione__ per rivalidare i prompt quando cambia il modello |
| __Embedding__ (indici vettoriali, cfr. RAG) | __no__: i vettori di un modello non hanno significato per un altro | prevedere la _reindicizzazione_ |
| __Dati__ (log, conversazioni, file conservati dal fornitore) | dipende dalle condizioni | conservarne una copia propria |

> Il vero strumento contro il lock-in è una __suite di valutazione__ propria: indica in poche ore se un _altro_ modello o fornitore è abbastanza valido

{{% /section %}}

---

{{< slide id="local" >}}

# Esecuzione locale e sovranità infrastrutturale

---

{{% section %}}

{{< slide id="ollama" >}}

## Ollama (cfr. <https://ollama.com/>)

- Ollama è un'azienda che offre una soluzione _containerizzata_ per _ospitare_ ed _eseguire_ LLM __on-premise__, orientata alla facilità d'uso e all'accessibilità

- Offre un [catalogo di modelli](https://ollama.com/models) preaddestrati, che si possono _scaricare_ e utilizzare
    + e un'_API nativa_ _parzialmente compatibile_ con quella di OpenAI, per facilitare la migrazione tra deployment on-premise e cloud

- Le soluzioni Ollama funzionano con varie __configurazioni hardware__ e __sistemi operativi__: GPU _Nvidia_, _Apple Silicon_, _Linux_, _MacOS_ e _Windows_

- Ollama supporta sia un'__interfaccia conversazionale CLI__ nativa sia un'__API Web__ per usare i modelli nei programmi

- Ollama è disponibile anche come [servizio cloud](https://ollama.com/pricing), ma qui ci concentriamo sul deployment on-premise
    + tre piani (\\$0/mese, \\$20/mese, \\$100/mese) con limiti d'uso progressivamente più ampi

---

## Esempio on-premise con Ollama, pt. 1

0. Verificare di avere l'__hardware necessario__ (es. GPU Nvidia, Apple Silicon ecc.)
    + dettagli e avvertenze di configurazione: <https://docs.ollama.com/gpu>
    + per capire se la memoria disponibile è _sufficiente_ a eseguire un modello: cfr. [Eseguire modelli localmente](#/local-requirements) più avanti

1. __Installare__ Ollama sulla propria macchina:

    ```bash
    brew install ollama # For macOS (Apple Silicon)
    curl -fsSL https://ollama.com/install.sh | sh # For Linux
    irm https://ollama.com/install.ps1 | iex # For Windows (PowerShell)
    ```

2. Avviare il __servizio__ Ollama in una shell (in alternativa configurare un _demone_ che lo esegua in background):

    ```bash
    ollama serve
    ```

---

## Esempio on-premise con Ollama, pt. 2

3. Consultare il __catalogo dei modelli__ di Ollama su <https://ollama.com/models> e __scegliere__ un modello tra le molte varianti disponibili
    * convenzione dei nomi: `model:variant`; di solito la variante indica la dimensione del modello

    {{< image src="ollama-models.png" alt="Catalogo dei modelli di Ollama" max-h="70vh" >}}
    {{< image src="ollama-models-gemma4.png" alt="Varianti di un modello disponibili nel catalogo di Ollama" max-h="70vh" >}}

---

## Esempio on-premise con Ollama, pt. 3

{{% small "90%" %}}

4. Scegliere un modello, es. `gemma4:e2b`, e __scaricarlo__:

    ```bash
    ollama pull gemma4:e2b
    ```

5. Si può provare il modello tramite l'__interfaccia conversazionale CLI__ nativa di Ollama:

    ```bash
    ollama run gemma4:e2b
    ```

    esempio di conversazione (notare che il modello mostra anche il proprio _processo di ragionamento_):

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

{{% /small %}}

---

## Esempio on-premise con Ollama, pt. 4

{{% small "90%" %}}

6. Per usare il modello _da un programma_, si può utilizzare l'__API__ di Ollama, _parzialmente_ compatibile con quella di OpenAI
    * es. supporta lo stesso endpoint `/v1/chat/completions`, ma non tutte le funzionalità OpenAI: mancano i campi `tool_choice`, `logit_bias`, `user` e `n`, e gli endpoint per file, batch e fine-tuning (cfr. [pagina sulla compatibilità](https://docs.ollama.com/api/openai-compatibility))
    * nel client, impostare come __URL di base dell'API__ l'indirizzo del servizio Ollama locale (predefinito: <http://localhost:11434/v1>)
    * per verificare che l'API funzioni, inviare una richiesta di prova con `curl`:

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

        risultato (convertito in YAML per facilitarne la lettura):

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

{{% /small %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="local-requirements" >}}

## Eseguire modelli localmente: cosa serve? (pt. 1)

### Il concetto generale: un modello è un’enorme raccolta di numeri

- Un LLM è essenzialmente una raccolta di __parametri__ (o _pesi_), cioè numeri appresi durante l'addestramento
    + la _dimensione_ del modello è il numero di parametri: es. `7B` = 7 miliardi di parametri, `70B` = 70 miliardi
- Per __eseguire__ un modello (_inferenza_), tutti i pesi devono essere caricati nella _memoria_ del dispositivo che effettua il calcolo
    + idealmente nella memoria della GPU (__VRAM__): le GPU sono molto più veloci delle CPU nelle moltiplicazioni matriciali su cui si basano gli LLM
- Quindi l'__occupazione di memoria__ di un modello è approssimativamente:

> __memoria in byte__ ≈ (numero di parametri × bit per parametro / 8) [_pesi_] + _cache KV_ [cresce con la lunghezza del contesto] + _overhead_

- La __cache key-value (KV)__ conserva le _chiavi_ e i _valori_ dell'attenzione di ogni token elaborato finora...
    + ... evitando di ricalcolarli per ogni nuovo token generato
    + la sua dimensione _cresce linearmente_ con la _lunghezza del contesto_
        + per questo i runtime locali (es. Ollama) usano una finestra di contesto predefinita _più piccola_ del massimo del modello
- L'_overhead_ comprende buffer del runtime, attivazioni e contesto del driver: di solito circa il 10–20% in più

{{< image src="./memory-footprint.svg" max-h="20vh" alt="Barre impilate della memoria richiesta (pesi più cache KV di 8mila token) da modelli 7B, 14B, 32B e 70B a precisione 16 bit, Q8_0 e Q4_K_M, confrontate con dispositivi da 8/16/24/48/80 GB" >}}

---

## Eseguire modelli localmente: cosa serve? (pt. 2)

### Regola pratica per i soli pesi

| Dimensione del modello | 32 bit (`fp32`) | 16 bit (`fp16`/`bf16`) | 8 bit (`int8`, `Q8_0`) | ~4 bit (`Q4_K_M`) |
|-----------:|----------------:|-----------------------:|-----------------------:|------------------:|
| 1B  | 4 GB   | 2 GB   | ~1 GB  | ~0.6 GB |
| 7B  | 28 GB  | 14 GB  | ~7.5 GB | ~4.3 GB |
| 14B | 56 GB  | 28 GB  | ~15 GB | ~8.6 GB |
| 32B | 128 GB | 64 GB  | ~34 GB | ~20 GB  |
| 70B | 280 GB | 140 GB | ~74 GB | ~43 GB  |

- cioè GB ≈ miliardi di parametri × bit per peso / 8 (`Q8_0` ≈ 8,5 bit, `Q4_K_M` ≈ 4,9 bit, cfr. [documentazione sulla quantizzazione di llama.cpp](https://github.com/ggml-org/llama.cpp/blob/master/tools/quantize/README.md))
- aggiungere ~10–20% di overhead del runtime e la cache KV (cresce con il contesto: diversi GB da 32mila token in su)
- i modelli sono _addestrati_ a 16/32 bit, ma la maggior parte degli utenti locali li esegue a __8 o 4 bit__: è la _quantizzazione_

{{% fragment %}}
> Prima di scaricare un modello, __fare i conti__: se non entra in VRAM, il caricamento _fallirà_ oppure il modello sarà (parzialmente) _trasferito_ alla CPU, diventando _molto più lento_
{{% /fragment %}}

---

## Quantizzazione (pt. 1): il concetto

- __Quantizzazione__ = rappresentare i pesi del modello con _meno bit_ (es. interi a 4 bit anziché numeri in virgola mobile a 16 bit)
    + associando _intervalli_ di valori reali a pochi _livelli discreti_, più alcuni _fattori di scala_ (memorizzati per _blocco_ di pesi)
- Perché?
    + _meno memoria_ $\implies$ modelli più grandi sullo stesso hardware
    + inferenza _più veloce_: lo spostamento dei pesi in memoria è il principale collo di bottiglia
- A quale prezzo?
    + _perdita di precisione_ $\implies$ un certo _peggioramento della qualità_, spesso trascurabile a 8 bit, visibile a 4 bit e forte sotto i 3 bit
    + il peggioramento dipende dal compito (es. maggiore per matematica, codice e lingue diverse dall'inglese)

{{< image src="./quantization.svg" max-h="40vh" alt="Istogramma a campana dei pesi fp16 mappato su 16 codici a quattro bit più un fattore di scala per blocco, ottenendo un istogramma approssimato a gradini" >}}

---

## Decifrare le etichette di quantizzazione GGUF (nei tag di Ollama)

- `F16` / `BF16`: nessuna quantizzazione (16 bit)
- `Q8_0`: 8 bit (~8,5 bit per peso, inclusi i fattori di scala dei blocchi), quasi senza perdita
- `Q4_K_M`: in media ~4,9 bit per peso
    - `K` = _k-quants_ (superblocchi di pesi con fattori di scala dei sottoblocchi, anch'essi quantizzati)
    - mix `M`edium (medio: alcuni tensori sensibili restano a precisione maggiore): il compromesso normalmente usato come __impostazione predefinita__
- `Q2_K`, `Q3_K_S`, ...: più piccoli e con perdite crescenti (mix `S`mall = piccolo, `M`edium = medio, `L`arge = grande)
- `IQ*`: quantizzazioni con _matrice d'importanza_, calibrate su testo campione per ridurre la perdita con pochissimi bit

Es. `ollama pull gemma4:e2b` scarica la quantizzazione predefinita; tag come `...-q8_0` o `...-fp16` ne selezionano una specifica (cfr. scheda _"Tags"_ di ogni modello nel [catalogo Ollama](https://ollama.com/models))

---

## Eseguire modelli localmente: hardware e software (pt. 1)

### Hardware

- __GPU Nvidia__ (CUDA): la piattaforma di riferimento; conta soprattutto la VRAM (es. 8, 12, 16, 24 GB nelle schede consumer; 40–80+ GB per data center)
- __GPU AMD__ (ROCm): supportate, con meno modelli di schede e più vincoli
- __Apple Silicon__ (Metal): CPU e GPU condividono _memoria unificata_, quindi tutta la RAM (meno quella del sistema operativo) è utilizzabile dal modello
- __Solo CPU__: funziona per modelli piccoli, ma con _pochi token al secondo_
- __Offloading parziale__: gli strati che non entrano in VRAM vengono eseguiti sulla CPU; funziona, ma è molto più lento

(cfr. <https://docs.ollama.com/gpu> per le GPU supportate)

### Modelli Mixture-of-Experts (MoE)

- Alcuni modelli attivano soltanto un _sottoinsieme_ dei parametri per ogni token
    + es. `gemma-4-26b-a4b-it` = 26B parametri _totali_, ~4B _attivi_ per token
- La __memoria__ dipende dai parametri _totali_ (tutti gli esperti devono essere caricati), la __velocità__ da quelli _attivi_

---

## Eseguire modelli localmente: hardware e software (pt. 2)

{{% small "85%" %}}

### Quali modelli si possono eseguire sui diversi dispositivi? (indicativamente, quantizzazione a ~4 bit, contesto moderato)

| Macchina | Dimensione del modello gestibile |
|---------|-----------------------|
| portatile, 8 GB RAM, senza GPU | ≤ 3B (lento) |
| portatile, 16 GB di memoria unificata (Apple Silicon) | ≤ 8B |
| desktop, GPU con 12–16 GB VRAM | ≤ 14B |
| workstation, GPU con 24 GB VRAM (o 32–64 GB di memoria unificata) | ≤ 32B |
| server, 2 GPU da 80 GB | ≤ 70B a 16 bit (140 GB, al limite), ~120B+ a 8 bit, più grandi con 4 bit o MoE |

### Software

- __Driver GPU__ aggiornati (CUDA per Nvidia, ROCm per AMD; Metal è integrato in macOS)
- un __runtime__: [Ollama](https://ollama.com) (più semplice), [LM Studio](https://lmstudio.ai/) (GUI), [llama.cpp](https://github.com/ggml-org/llama.cpp) (a un livello più basso, con maggiore controllo sui dettagli di esecuzione), [vLLM](https://docs.vllm.ai/) (per server, alto throughput, molti utenti simultanei)
    + tutti espongono un'API Web _compatibile con OpenAI_, quindi il codice client non cambia
- verificare cosa è in esecuzione e _dove_ con `ollama ps` (su un portatile Apple Silicon):

    ```text
    NAME          ID              SIZE      PROCESSOR    CONTEXT    RUNNER    UNTIL
    gemma4:e2b    7fbdbf8f5e45    1.7 GB    100% GPU     4096                 4 minutes from now
    ```

    + `SIZE` è la memoria _effettivamente allocata_ (pesi in uso + cache KV); `ollama list` mostra invece la dimensione _su disco_ (7,2 GB per `gemma4:e2b`: tutti i pesi, inclusi embedding per strato ed encoder di immagini/audio)
    + `PROCESSOR` indica se il modello è interamente sulla GPU o suddiviso tra CPU e GPU (es. `48%/52% CPU/GPU`)
    + `CONTEXT` è la finestra di contesto in uso: il [valore predefinito](https://docs.ollama.com/context-length) di Ollama dipende dalla VRAM (4mila token sotto 24 GiB, 32mila fino a 48 GiB, 256mila oltre)

{{% /small %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="cost-example" >}}

## Opzioni di deployment: un esempio di calcolo dei costi

{{% small "80%" %}}

Stimiamo il costo annuo dell'esempio ricorrente, con __ipotesi__ esplicite (e approssimative):
- 500 candidature all'anno; ciascuna = immagine del passaporto + certificato degli esami + lettera ≈ 6mila token in ingresso, più ~mille token di prompt
- 5 richieste per candidatura (estrazione, validazione, valutazione, ...) $\implies$ ~35mila token in _ingresso_ + ~5mila in _uscita_ per candidatura
- quindi, all'anno: __17,5M token in ingresso__ + __2,5M in uscita__

| Opzione | Prezzo (USD per 1M di token in ingresso / uscita) | Costo annuo |
|---|---|---|
| modello proprietario di frontiera (es. [Claude Opus 5.5](https://platform.claude.com/docs/en/about-claude/pricing)) | \\$4 / \\$20 | 17.5 × 4 + 2.5 × 20 ≈ __\\$120__ |
| modello proprietario piccolo (es. [Claude Haiku 4.5](https://platform.claude.com/docs/en/about-claude/pricing)) | \\$1 / \\$5 | ≈ __\\$30__ |
| modello aperto ospitato nell'UE (es. Mistral Small 4, multimodale, Apache 2.0, su [La Plateforme](https://mistral.ai/pricing) di Mistral) | \\$0.15 / \\$0.60 | 17.5 × 0.15 + 2.5 × 0.6 ≈ __\\$4__ |
| GPU a noleggio, su richiesta (es. GPU da 24 GB su [RunPod](https://www.runpod.io/pricing)) | ~\\$0.35–0.75 all'ora | poche ore all'anno ≈ __\\$5__ (+ lavoro di configurazione) |
| on-premise (workstation con GPU da 24 GB) | ~€2–2,5mila per la sola GPU | __€2,5mila+__ iniziali, + energia, + manutenzione |

(prezzi a settembre 2026: cambiano ogni pochi mesi, verificarli sempre)

{{% /small %}}

{{% fragment %}}
> A _questa_ scala, __il costo non è il fattore decisivo__: contano conformità e controllo. Su larga scala cambia tutto: es. una GPU da 80 GB noleggiata 24/7 costa ~\\$2mila/mese, un importo che permette di acquistare ~100M di token in _uscita_ di un modello di frontiera al mese. Il __punto di pareggio__ dipende da _volume_, _utilizzo_ e _dimensione del modello_
{{% /fragment %}}

---

{{< slide id="cost-breakeven" >}}

## Il punto di pareggio, a colpo d'occhio

{{< image src="./cost-breakeven.svg" max-h="55vh" alt="Costo annuo in funzione dei token elaborati all'anno, in scala logaritmica: le opzioni a consumo (modello proprietario di frontiera, piccolo proprietario, aperto ospitato nell'UE) sono rette crescenti, le opzioni a costo fisso (GPU da 80 GB a noleggio 24/7, circa 24mila dollari all'anno; workstation on-premise, circa 1.200 dollari all'anno) sono orizzontali. L'esempio ricorrente, circa 20 milioni di token all'anno, sta molto a sinistra di tutti i punti di pareggio: la workstation conviene rispetto al modello di frontiera da circa 200 milioni di token all'anno, rispetto al modello aperto ospitato nell'UE solo da circa 6 miliardi" >}}

- I costi fissi non crescono con il volume, ma hanno una _capacità_ massima e costi nascosti (_personale_, manutenzione)
- Rispetto a un modello aperto ospitato nell'UE, l'on-premise conviene solo a volumi __centinaia di volte__ maggiori dell'esempio ricorrente

---

{{< slide id="local-sustainability" >}}

## Quando l'esecuzione locale è _davvero_ sostenibile?

{{% multicol %}}
{{% col %}}

### Lo è quando...

- i dati __non devono__ uscire dall'organizzazione (per legge, contratto o policy) e non c'è un host affidabile disponibile
- il carico è __costante__ e __elevato__: l'hardware è usato per gran parte del tempo (alto _tasso di utilizzo_)
- modelli __piccoli/medi__ bastano per il compito (es. estrazione, classificazione, sintesi), come verificato con valutazioni proprie
- ci sono __competenze__ interne: GPU, driver, runtime, aggiornamenti, monitoraggio, sicurezza
- sono richiesti funzionamento __offline__ o versioni _fissate_ (riproducibilità)

{{% /col %}}
{{% col %}}

### Non lo è quando...

- il compito richiede qualità _di frontiera_ (ragionamento difficile, lunghe attività agentiche): i modelli aperti sono indietro e i più grandi richiedono hardware _da data center_
- il carico è __ridotto__ o __discontinuo__: l'hardware resta inutilizzato e il pagamento per token costa meno (cfr. [esempio sui costi](#/cost-example))
- ci sono molti utenti __simultanei__: una GPU serve poche richieste per volta; scalare significa aggiungere GPU
- nessuno sa _gestire il sistema_: un server senza manutenzione è un rischio per la sicurezza e la continuità del servizio
- conta il __time-to-market__: acquisto e configurazione richiedono settimane

{{% /col %}}
{{% /multicol %}}

> Una via intermedia: modelli _a pesi aperti_, ospitati da un fornitore __nell'UE__ (o su una GPU a noleggio), con la possibilità di spostarli on-premise in seguito: stessi pesi, stesso comportamento

{{% /section %}}

---

{{< slide id="eu-sovereignty" >}}

# Sovranità digitale europea

Iniziative e loro maturità

---

{{% section %}}

{{< slide id="sovereignty" >}}

## "IA sovrana": una narrazione e i suoi limiti

- Diversi attori europei e italiani presentano i modelli aperti sviluppati localmente come una questione di __sovranità digitale__
    + ridurre la dipendenza da fornitori statunitensi e cinesi; supportare meglio le _lingue_ dell’UE; rispettare il _diritto_ dell’UE
- Questa narrazione mescola (almeno) tre obiettivi __distinti__, da _non_ confondere:
    1. __dove__ un modello è _sviluppato_ (nazionalità dello sviluppatore)
    2. __dove__ viene _eseguito_ e vengono elaborati i dati (controllo _operativo_)
    3. se il processo di realizzazione è _ispezionabile_ e _riproducibile_ (_verificabilità_)
- Gli obiettivi 2 e 3 __non__ richiedono l'obiettivo 1:
    + _qualsiasi_ modello a pesi aperti (es. Qwen3, gpt-oss, OLMo 3) può essere eseguito in un data center UE
    + i modelli più riproducibili non sono necessariamente europei (es. OLMo 3, statunitense), e un fornitore europeo può divulgare poco (es. Mistral)

{{% fragment %}}
> Lettura scettica: per un ingegnere, _dove gira il modello_ e _cosa consente la licenza_ contano molto più di _chi lo ha addestrato_. Le evidenze (prossime slide) suggeriscono che i modelli UE e italiani sono perlopiù __piccoli__, __tardivi__ e __non competitivi__ con la frontiera né con i migliori modelli globali a pesi aperti
{{% /fragment %}}

---

## Il divario, in numeri

Dati dallo [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report) (cifre del 2025):

| | Stati Uniti | Cina | Europa | Italia |
|---|---|---|---|---|
| Modelli di IA _di rilievo_ pubblicati (Epoch AI, Fig. 1.1.13) | __59__ | 35 | 1 (Francia); un altro nel Regno Unito | nessuno elencato |
| Investimenti privati in IA (Quid, Fig. 4.2.8, 4.2.11) | __\\$285.9B__ | \\$12.4B | \\$20.9B (Francia \\$4.4B, Germania \\$3.9B) | < \\$1B (fuori dai primi 15) |
| ... di cui IA _generativa_ (Fig. 4.2.10) | __\\$163.6B__ | \\$1.5B | \\$3.2B | — |
| Nuove aziende di IA finanziate (Fig. 4.2.9) | __1.953__ | 161 | es. Germania 92, Francia 84 | 38 |
| Crescita degli investimenti privati in IA, 2024 $\rightarrow$ 2025 (Fig. 4.2.11) | +160% | +32% | +7% | — |

- Il divario non si riduce: si _allarga_, almeno negli investimenti privati
- Cautele: gli investimenti privati sottostimano i finanziamenti _pubblici_ (soprattutto cinesi, e programmi UE/nazionali come le EuroHPC AI Factories); nei conteggi basati sull’affiliazione, i gruppi multinazionali possono comparire più volte

---

## Iniziative europee

{{% small "80%" %}}

| Iniziativa | Chi | Cosa | Licenza | Stato (set. 2026) |
|---|---|---|---|---|
| [EuroLLM](https://huggingface.co/utter-project/EuroLLM-22B-2512) | IST Lisbona, Univ. di Edimburgo, Unbabel e altri; fondi EuroHPC e Horizon Europe | 1.7B, 9B, 22B; 35 lingue (tutte le 24 lingue ufficiali dell’UE), ~4mila miliardi di token | Apache 2.0 | 22B pubblicato a feb. 2026 |
| [OpenEuroLLM](https://openeurollm.eu/) | 20 membri, guidati da Università Carolina e AMD Silo AI; finanziamento Digital Europe (feb. 2025, 3 anni) | modelli "davvero aperti" per tutte le lingue UE | aperta (prevista) | finora solo piccoli modelli _di riferimento_ (es. [38 modelli monolingui da 2,15 miliardi di parametri](https://openeurollm.eu/blog/hplt-oellm-38-reference-models), con HPLT); modello principale ancora atteso |
| [OpenLLM-France](https://openllm-france.fr/en/main-page-en/) | consorzio francese, fondi Bpifrance | [Lucie-7B](https://huggingface.co/OpenLLM-France/Lucie-7B), poi [Luciole](https://huggingface.co/collections/OpenLLM-France/luciole-llm) 1B/8B/23B (~30% francese) | dati, pesi e codice aperti | progetto di ricerca |
| [Teuken / OpenGPT-X](https://www.iais.fraunhofer.de/en/industries_and_cross-sector_solutions/cross-sector_solutions/generative-ai/opengpt-x.html) | Fraunhofer, FZ Jülich e altri; ~€14M dal governo tedesco | 7B, 24 lingue UE | v0.4: Apache 2.0; v0.6: non commerciale | progetto __concluso__ a marzo 2025; modelli ancora scaricabili |
| [Apertus](https://www.apertus-ai.org/) | ETH Zurigo, EPFL, CSCS (Swiss AI Initiative: CHF 20M + 10M di ore GPU) | 8B, 70B; 1.5 (luglio 2026) aggiunge immagini, ragionamento, tool | Apache 2.0, completamente aperto | il rilascio aperto più completo in Europa; 2.0 prevista per il 2027 |
| [Mistral AI](https://docs.mistral.ai/getting-started/models/) | azienda privata francese | dai modelli piccoli ai grandi MoE | eterogenea: Apache 2.0, Modified MIT, proprietaria | il fornitore europeo più vicino alla frontiera; poche informazioni sull'addestramento |

{{% /small %}}

---

## Infrastrutture e politiche pubbliche

- __EuroHPC AI Factories__: centri di supercalcolo aperti all'addestramento e all'uso dell'IA
    + [IT4LIA](https://www.hpc.cineca.it/projects/it4lia/), al CINECA di Bologna (con Austria e Slovenia), era tra le [prime sette](https://eurohpc-ju.europa.eu/selection-first-seven-ai-factories-drive-europes-leadership-ai-2024-12-10_en) selezionate a dic. 2024 (€1,5 miliardi totali, fondi UE + nazionali)
    + attenzione: IT4LIA è una AI __Factory__, __non__ una AI _Gigafactory_: il [bando Gigafactories](https://www.eurohpc-ju.europa.eu/eurohpc-joint-undertaking-launches-ai-gigafactories-call-2026-07-30_en) (per addestramento alla scala della frontiera) è stato aperto solo a luglio 2026, con scadenza a nov. 2026
- __Dati__: [LLMs4EU](https://www.alt-edic.eu/projects/llms4eu/) di [ALT-EDIC](https://www.alt-edic.eu/) (marzo 2025, 3 anni, oltre 20 paesi) raccoglie risorse linguistiche per tutte le lingue UE e punta a modelli settoriali
- __Politiche italiane__: la [Strategia italiana per l'IA 2024–2026](https://www.agid.gov.it/sites/agid/files/2024-07/Strategia_italiana_per_l_Intelligenza_artificiale_2024-2026.pdf) e la [Legge 132/2025](https://www.gazzettaufficiale.it/eli/id/2025/09/25/25G00143/sg) promuovono tecnologie "adattate al contesto italiano" e artefatti di ricerca aperti

{{< image src="./europe-timeline.svg" max-h="35vh" alt="Linea temporale 2023-2026 che confronta i rilasci dei modelli globali di frontiera con rilasci e progetti UE e italiani, più piccoli e più tardivi" >}}

---

## Problemi ricorrenti dei modelli aperti europei

1. __Dati__: poco testo di qualità nelle lingue con meno risorse (la ragione stessa di LLMs4EU e dei dati sintetici)
2. __Calcolo__: ogni progetto dipende da un'assegnazione di risorse di supercalcolo pubbliche (Leonardo, MareNostrum 5, JUWELS, Alps); un modello "aperto" non è _riproducibile_ da chi non vi ha accesso
3. __Scala e tempi__: modelli addestrati da zero fino a 14B parametri, pubblicati _mesi o anni_ dopo modelli globali aperti comparabili; modelli principali "previsti" o "attesi"
4. __Manutenzione__: i progetti hanno finanziamenti _a termine_ (es. OpenGPT-X concluso nel 2025): chi corregge, aggiorna e rivaluta i modelli dopo?
5. __Adozione__: nelle fonti, l'uso nella pubblica amministrazione è soprattutto un'_aspirazione_, non un deployment su larga scala dimostrato

{{% fragment %}}
> Queste iniziative sono preziose per __ricerca__, __apertura__ e __copertura linguistica__; (ancora) __non__ sono un'alternativa ai modelli globali pronta per l'uso in produzione. Un'organizzazione UE può ottenere controllo operativo eseguendo un modello _globale_ a pesi aperti su infrastrutture _UE_
{{% /fragment %}}

{{% /section %}}

---

{{< slide id="italy" >}}

# Il panorama italiano

---

{{% section %}}

## Iniziative italiane

| Modello | Chi | Cosa | Licenza | Stato (set. 2026) |
|---|---|---|---|---|
| [Minerva](https://huggingface.co/sapienzanlp/Minerva-7B-base-v1.0) | Sapienza NLP, nel progetto PNRR [FAIR](https://fondazione-fair.it/); addestrato su Leonardo del CINECA | 350M, 1B, 3B e __7B__, da zero; 1.140 miliardi di token in italiano + 1.140 miliardi in inglese + 200 miliardi di token di codice | Apache 2.0, dati aperti | 7B presentato a nov. 2024; ancora il modello principale, per ricerca |
| [LLaMAntino / ANITA](https://huggingface.co/swap-uniba/LLaMAntino-3-ANITA-8B-Inst-DPO-ITA) | Univ. di Bari | _fine-tuning_ di Llama, Mistral, gpt-oss per l'italiano (non addestrati da zero) | ereditata; i metadati di [ANITA-NEXT](https://huggingface.co/m-polignano/ANITA-NEXT-24B-Magistral-2506-ITA) indicano Apache 2.0, la scheda dice "solo ricerca" | progetto di ricerca attivo; gestione delle licenze poco rigorosa |
| [Velvet](https://huggingface.co/Almawave/Velvet-14B) | Almawave (privata); addestrato su Leonardo | 2B e 14B, 6 lingue (~23% italiano per il 14B) | Apache 2.0 | pubblicato a gen.–feb. 2025, poi invariato |
| [Italia](https://huggingface.co/sapienzanlp/modello-italia-9b) → [Domyn Small](https://huggingface.co/domyn/Domyn-Small-v1.0) | iGenius (privata), rinominata Domyn a giugno 2025 | Italia 9B (2024), poi 10B; Domyn Small 10B (maggio 2026), oltre 50 lingue | Italia 9B: MIT; Italia 10B: _specifica, non aperta_; Domyn Small: MIT | l'azienda si è orientata verso grandi modelli _proprietari_ |

- La qualifica di modello "__nazionale__" non dice nulla sul suo grado di _apertura_: confrontare Minerva (Apache, dati aperti) con Italia 10B (licenza restrittiva)
- I modelli italiani addestrati _da zero_ vanno da 0,35B (Minerva) a 14B (Velvet); quelli più grandi (es. ANITA-NEXT 24B) sono _fine-tuning_ di modelli stranieri. Il numero di parametri resta di uno o due ordini di grandezza inferiore a quello dei modelli di frontiera

---

## Quanto sono validi i modelli italiani?

- Misurarlo è difficile: i benchmark inglesi tradotti _non_ bastano
    + [ITA-Bench](https://aclanthology.org/2024.clicit-1.67/) (CLiC-it 2024) combina compiti tradotti e dataset nativi italiani
    + [Evalita-LLM](https://arxiv.org/abs/2502.02289) (2025) usa compiti _nativi_ italiani e più prompt per compito: le classifiche sono _sensibili_ alla formulazione del prompt ([classifica](https://huggingface.co/spaces/evalitahf/evalita_llm_leaderboard))
- I risultati pubblicati mostrano che i piccoli modelli italiani possono essere _utili_: es. [Velvet-2B](https://huggingface.co/Almawave/Velvet-2B) riporta 39,6 su MMLU italiano e 61,9–86,6 nei compiti ITA-Bench...
- ... ma __non ci sono evidenze robuste__ che i modelli italiani superino i grandi modelli aperti multilingue (es. Qwen3, Gemma 4), tantomeno quelli di frontiera, _nemmeno in italiano_

{{% fragment %}}
> Le posizioni in classifica cambiano ogni pochi mesi: quando si cita una posizione, indicare sempre la __data della rilevazione__
{{% /fragment %}}

{{% /section %}}

---

{{< slide id="evaluation" >}}

# Come valutare un LLM

---

{{% section %}}

{{< slide id="model-selection" >}}

## Quale modello? Il concetto generale

- La scelta del modello è una decisione di _governance_ e influenza tutte e cinque le dimensioni contemporaneamente:
    + _qualità_ (capacità), _costo_ (prezzo per token o hardware), _controllo_ (pesi aperti? versioni fissate? dismissione?), _conformità_ (licenza, giurisdizione e policy sui dati del fornitore), _time-to-market_ (disponibilità, ecosistema)
- Richiede _informazioni_ sui modelli, provenienti da:
    1. __documentazione__ (model card, system card, licenze, termini di servizio)
    2. __valutazioni__ pubbliche (benchmark, classifiche)
    3. valutazioni __proprie__ sul compito da svolgere
- __Non__ è una decisione una tantum: i modelli vengono _aggiornati_, _dismessi_ e _superati_ ogni pochi mesi; la scelta va _riesaminata_ periodicamente (e resa _riproducibile_)

(cfr. [cataloghi di modelli](#/model-zoos) e relativi [parametri chiave](#/key-parameters))

---

## Documentazione dei modelli: model card, system card, datasheet

{{% small "90%" %}}

- __Model card__: già viste nei [cataloghi](#/model-zoos) ([Mitchell et al., 2019](https://arxiv.org/abs/1810.03993); [guida di Hugging Face](https://huggingface.co/docs/hub/model-cards)); qui interessa ciò che riportano sugli aspetti di _governance_
- __Datasheet per dataset__ ([Gebru et al., 2021](https://arxiv.org/abs/1803.09010)): stessa idea per i dati di addestramento (motivazione, composizione, raccolta, usi consigliati)
- __System card__: rapporti più lunghi pubblicati dai fornitori di frontiera a ogni rilascio, centrati sulle valutazioni di _sicurezza_, il red teaming e le mitigazioni
    + es. le system card di [OpenAI](https://openai.com/index/gpt-oss-model-card/) e [Anthropic](https://www.anthropic.com/system-cards)

{{< image src="./model-card-anatomy.svg" max-h="35vh" alt="Anatomia di una model card: otto sezioni, dai dettagli del modello alla licenza, associate alle dimensioni di governance su cui forniscono informazioni (qualità, conformità, controllo)" >}}

- __Cosa cercare__, da ingegneri: _licenza_, _lingue_, _finestra di contesto_, _limite temporale delle conoscenze_, _usi previsti_ (il proprio rientra?), _limiti_ noti e _valutazioni_ pertinenti al compito

{{% /small %}}

---

## Come valutare i modelli?

### 1. __Benchmark__ e __classifiche__ pubblici (generici, economici da consultare)

- _benchmark_: dataset fissi + metriche per misurare una capacità
    + es. conoscenza ([MMLU](https://arxiv.org/abs/2009.03300)), ragionamento esperto ([GPQA](https://arxiv.org/abs/2311.12022)), programmazione ([SWE-bench](https://www.swebench.com/)), uso dei tool, contesto lungo...
    + framework complessivi come [HELM](https://arxiv.org/abs/2211.09110) valutano molti modelli su molti scenari e metriche (non solo accuratezza)
- _classifiche_: graduatorie che aggregano risultati di benchmark o preferenze umane
    + es. [Arena](https://arena.ai/) (prima Chatbot Arena / LMArena: confronti umani alla cieca tra coppie di risposte, cfr. [Chiang et al., 2024](https://arxiv.org/abs/2403.04132); si veda anche la critica di [Singh et al., 2025](https://arxiv.org/abs/2504.20879)), [Artificial Analysis](https://artificialanalysis.ai/) (qualità vs. prezzo vs. velocità)
- __cautele__:
    + _contaminazione dei dati_: elementi del benchmark possono essere finiti nei dati di addestramento (cfr. [Xu et al., 2024](https://arxiv.org/abs/2406.04244))
    + _saturazione_: tutti i migliori modelli si avvicinano al 100%, quindi il benchmark smette di distinguerli (es. Hugging Face ha [ritirato](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions/1135) l'Open LLM Leaderboard nel 2025)
    + _legge di Goodhart_: quando una misura diventa un obiettivo, smette di essere una buona misura
    + i benchmark misurano capacità _generiche_, che potrebbero _non_ essere quelle richieste dal proprio compito

### 2. Valutazione __specifica per il compito__ (quella che conta davvero)

- eseguire ogni modello candidato sul _proprio_ dataset di test, con i _propri_ valutatori, come illustrato nella [sezione sulla validazione](#/validation) (prima valutatori deterministici, [LLM-as-a-Judge](#/llm-as-a-judge) dove il codice non basta)
- misurare anche _latenza_ e _costo_ (entrambi riportati nei metadati delle risposte API, es. `usage`)

---

{{< slide id="eval-dimensions" >}}

## Cosa valutare: qualità, efficienza, robustezza, specializzazione

| Dimensione | Domande | Come misurare |
|---|---|---|
| __Qualità__ | gli output sono _corretti_, _completi_, _fondati sui dati_? | test set con risultati attesi; valutatori (prima codice, poi LLM-as-a-Judge, poi persone) |
| __Efficienza__ | quanto è _veloce_, quanto _costa_? | latenza (tempo al primo token, token al secondo), token per compito, costo per compito (o mese) |
| __Robustezza__ | stessa qualità con input _riformulati_, documenti con errori o informazioni irrilevanti, contenuti costruiti per ingannare il sistema? Stesso output tra esecuzioni? | variazioni degli input, esecuzioni ripetute (_varianza_), test di prompt injection |
| __Specializzazione__ | funziona bene nel _proprio_ dominio, nella _propria_ lingua e con i _propri_ formati di documento? | casi di test del dominio (es. testi amministrativi italiani, moduli scansionati) |

- I benchmark pubblici coprono (parte della) prima dimensione su compiti _generici_: la valutazione delle altre dimensioni spetta all’__organizzazione__
- Ogni dimensione può escludere un modello: es. con un’accuratezza superiore del 2% ma 10 volte più lento, oppure _incostante_ tra esecuzioni

---

## Misurare capacità _generali_: benchmark di frontiera

Mentre i modelli _saturano_ i benchmark classici, nuovi benchmark provano a misurare ciò che i modelli __non__ sanno (ancora) fare:

{{% multicol %}}
{{% col class="col-8" %}}
{{% small "90%" %}}
- __ARC-AGI__ ([Chollet, 2019](https://arxiv.org/abs/1911.01547)): intelligenza come _efficienza nell'acquisire capacità_, misurata su rompicapi visivi facili per gli esseri umani, che richiedono di _astrarre_ regole da pochi esempi
    + [ARC-AGI-2](https://arxiv.org/abs/2505.11831) (2025) ha alzato la difficoltà; il premio principale dell'[ARC Prize 2025](https://arcprize.org/blog/arc-prize-2025-results-analysis) _non è stato assegnato_
    + [ARC-AGI-3](https://arcprize.org/blog/arc-agi-3-launch) (marzo 2026) è _interattivo_: cfr. [benchmark agentici](#/agentic-benchmarks) e [classifica](#/arc-agi-3-leaderboard) più avanti
- __Humanity's Last Exam__ ([Phan et al., 2025](https://arxiv.org/abs/2501.14249)): 2.500 domande scritte da esperti di varie discipline, pensate per _non essere risolvibili con Google_
    + i migliori modelli sono passati da percentuali a una cifra (inizio 2025) a ~60% (cfr. [Artificial Analysis](https://artificialanalysis.ai/evaluations/humanitys-last-exam), settembre 2026)
- __Orizzonte temporale di METR__ ([Kwa et al., 2025](https://arxiv.org/abs/2503.14499)): la _durata_ dei compiti (misurata dal tempo necessario a un esperto umano) che un agente completa con un tasso di successo del 50%
    + _raddoppio_ ogni ~7 mesi nel 2019–2025 (212 giorni nel lavoro originale), più veloce dal 2023: ~4 mesi (129 giorni, stima METR Time Horizon 1.1, cfr. [misurazioni aggiornate](https://metr.org/time-horizons))
- __Quadro d’insieme__: lo [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report) descrive una _frontiera irregolare_: modelli da medaglia d'oro alle olimpiadi di matematica, che però falliscono nel leggere orologi analogici
{{% /small %}}
{{% /col %}}
{{% col class="col-4" %}}
{{< image src="./benchmark-saturation.png" max-h="48vh" alt="AI Index 2026, Figura 2.1.1: prestazioni del miglior modello rispetto al riferimento umano, 2012-2025, su ImageNet, SuperGLUE, MMLU, GPQA Diamond, MATH, MMMU, AIME, SWE-bench Verified, OSWorld e altri: ogni benchmark si avvicina o supera il riferimento umano, quelli più recenti sempre più rapidamente" >}}

<small>Fonte: [Stanford AI Index 2026](https://hai.stanford.edu/ai-index/2026-ai-index-report), cap. 2, Figura 2.1.1 ([CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/))</small>
{{% /col %}}
{{% /multicol %}}

> __Da ricordare__: i benchmark di frontiera mostrano dove si trova lo _stato dell'arte_, non se un modello è adatto al __proprio__ compito

---

## Benchmark di frontiera: gli orizzonti temporali di METR

{{< image src="./task-completion-time-horizon.png" max-h="50vh" alt="METR: durata dei compiti software (misurata dal tempo necessario a una persona) completati dagli LLM nel 50% dei casi, per data di rilascio, in scala logaritmica: da secondi (GPT-2, 2019) a circa un'ora (o1, fine 2024), diverse ore (Claude Opus 4.6, inizio 2026) e circa 16 ore (Claude Mythos Preview, aprile 2026), con andamento esponenziale; sopra 16 ore le misurazioni sono inaffidabili con l'attuale insieme di compiti" >}}

- Es. l'orizzonte temporale al 50% di Claude Mythos Preview (aprile 2026) è ~17 ore, al limite di ciò che la suite di compiti METR può misurare
- Cautele: solo compiti _software_, successo al 50% (l'orizzonte all'80% è molto più breve), intervalli di confidenza ampi

<small>Fonte: [METR, orizzonti temporali](https://metr.org/time-horizons), consultato a ottobre 2026</small>

---

{{< slide id="arc-agi-3-leaderboard" >}}

## Benchmark di frontiera: la classifica ARC-AGI-3

{{< image src="./arc-agi-3-leaderboard.png" max-h="50vh" alt="Classifica verificata ARC-AGI-3, punteggio rispetto al costo per esecuzione (scala logaritmica): i migliori modelli con harness standard raggiungono circa 63% (GPT-6 Astra, Max) a decine di migliaia di dollari; Claude Opus 5 (High) circa 30%; con harness Provider Adapter, GPT-6 Astra e GPT-6.1 Sol raggiungono 90–100%; quasi tutti gli altri restano sotto il 10%" >}}

- In sei mesi il punteggio è passato da meno dell’1% al lancio (marzo 2026) al 62,7% con l'harness _standard_ (il software che gestisce l’agente) e a circa il 99% con un harness _generale_ (conservazione dello stato di ragionamento, compattazione della conversazione)
    + a un costo di migliaia o decine di migliaia di dollari per esecuzione completa; gli esseri umani risolvono tutti gli ambienti

<small>Fonte: [classifica ARC Prize](https://arcprize.org/leaderboard), consultata a ottobre 2026</small>

---

{{< slide id="agentic-benchmarks" >}}

## Benchmark agentici: quanto sono validi gli agenti in generale?

{{% small "80%" %}}

| Benchmark | Cosa deve fare l'agente | Al rilascio |
|---|---|---|
| [GAIA](https://arxiv.org/abs/2311.12983) (2023) | rispondere a domande reali combinando ricerca Web, file e tool | esseri umani 92%, GPT-4 con plugin 15% |
| [WebArena](https://arxiv.org/abs/2307.13854) (2023) | completare compiti su siti Web realistici (ospitati localmente), mantenendo traccia dello stato del compito | esseri umani 78%, miglior agente GPT-4 14% |
| [AgentBench](https://arxiv.org/abs/2308.03688) (2023) | perseguire obiettivi in 8 ambienti (sistemi operativi, database, giochi, Web, ...) per molti turni | ampio divario tra modelli commerciali e aperti |
| [SWE-bench](https://www.swebench.com/) (2023) | risolvere issue GitHub _reali_, cioè costruire una rappresentazione operativa di una base di codice | pochi punti percentuali (ora molto di più, cfr. agenti di programmazione) |
| [$\tau$-bench](https://arxiv.org/abs/2406.12045) (2024) | assistere utenti simulati tramite tool, rispettando le _policy_ del dominio | pass$^8$ < 25% nel commercio al dettaglio |
| [ARC-AGI-3](https://arcprize.org/arc-agi/3) (2026) | esplorare ambienti interattivi _nuovi_, inferire obiettivi e dinamiche, pianificare | esseri umani 100%, IA di frontiera < 1% ([marzo 2026](https://arxiv.org/abs/2603.24621)); miglior risultato verificato a ott. 2026: 62,7% ([classifica](https://arcprize.org/leaderboard)) |

{{% /small %}}

{{% small "80%" %}}

- La __rappresentazione__ è il collo di bottiglia: gli agenti falliscono quando non sanno _costruire_, _mantenere_, _verificare_ e _rivedere_ la corretta rappresentazione del compito (stato, obiettivi, regole, effetti delle azioni)
    + es. in una variante dell'ambiente ARC-AGI-3 TR87, Claude Opus 4.6 ottiene 0% senza harness e 97,1% con un harness _costruito ad hoc_, ma 0% in entrambi i casi su BP35 ([rapporto ARC-AGI-3](https://arxiv.org/abs/2603.24621), §4): un harness ad hoc può determinare il successo o il fallimento, ma non _generalizza_
    + un harness _generale_ (conservazione dello stato di ragionamento, compattazione della conversazione) porta invece GPT-6 Astra da 54,8% a 99,9% sull'intero benchmark ([classifica](https://arcprize.org/leaderboard), ott. 2026): il software che circonda il modello conta quanto il modello stesso
- Il __successo__ senza governance non rende affidabile l'_azione autonoma_: i tassi di successo non dicono _come_ sia stato raggiunto l'obiettivo (policy, effetti collaterali, costi)
- I benchmark _saturano_ e finiscono nei dati di addestramento: aiutano a _scegliere_ i modelli (cfr. [come scegliere](#/decision)), ma sono le valutazioni __proprie__ a dire se il _proprio_ agente funziona

{{% /small %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="realistic-evaluation" >}}

## Una valutazione realistica per un'organizzazione

1. Partire dal __caso d'uso__: su quali decisioni interverrà il modello e quali documenti elaborerà? Che cosa rende _sbagliato_ un output e quanto è grave l'errore?
2. Costruire un __golden set__: 30–100 casi _reali_ (anonimizzati), con risultati attesi definiti da chi svolge oggi il lavoro
    + includere casi _difficili_ e _al limite_, non solo quelli tipici
3. __Selezionare__ 3–5 modelli che soddisfino i _vincoli inderogabili_ (deployment, protezione dei dati, licenza, modalità, lingua)
4. Eseguire ogni modello sul golden set __più volte__, misurando qualità, efficienza e robustezza
5. __Decidere__ con una regola scritta _in anticipo_ (es. "il modello più economico sopra il 90% di accuratezza, senza errori critici") e __documentarla__
6. __Ripetere__ la valutazione ogni volta che cambia modello, prompt o fornitore: è un _test di regressione_

> Il golden set è un __patrimonio__ dell'organizzazione: dura più di modelli e fornitori

---

{{< slide id="validation" >}}

## Validare software generativo: il concetto generale

- Il software basato su LLM è difficile da testare:
    + gli output sono _non deterministici_ (stesso input, output diversi)
    + gli output sono _aperti_ (molte risposte corrette, nessun unico valore atteso)
    + il comportamento cambia quando cambia _prompt_, _modello_ o _fornitore_, anche senza preavviso
- Oltre ai _soli_ test unitari (superato/fallito su un'esecuzione), servono __valutazioni__ (o _eval_): misurare _quanto spesso_ e _quanto bene_ il sistema si comporta come previsto su un _dataset_ di input rappresentativi
    + le valutazioni possono comunque _usare_ framework di test unitari (es. `pytest`), con asserzioni su _tassi_ o _soglie_ anziché su singoli output esatti

- Ingredienti di una valutazione:
    1. un __dataset di test__: input _noti_, ciascuno con risultati _attesi_, progettato da _persone_ __prima__ del test
    2. il __sistema da valutare__ (prompt + modello + codice)
    3. __valutatori__ (o _metriche_), che associano a ogni (input, output, risultato atteso) un _punteggio_ o un esito _superato/fallito_:
        * valutatori __deterministici__: validità dello schema, corrispondenza esatta, intervalli, espressioni regolari, invarianti e _relazioni_ tra output
        * valutatori __statistici__: somiglianza rispetto a un testo di riferimento (es. [BLEU](https://aclanthology.org/P02-1040/), [ROUGE](https://aclanthology.org/W04-1013/), similarità degli embedding)
            - richiedono un testo _di riferimento_ e hanno una correlazione debole con il giudizio umano sugli output aperti (cfr. [G-Eval](https://arxiv.org/abs/2303.16634)): qui non li usiamo
        * __LLM-as-a-Judge__: un altro LLM valuta l'output secondo una _griglia di valutazione_ descritta in linguaggio naturale
        * __persone__: i valutatori più affidabili e più costosi
    4. un'__aggregazione__ (es. tasso di superamento, punteggio medio), da confrontare tra _versioni_ del sistema (_test di regressione_)

- Gli output strutturati rendono la valutazione _molto_ più semplice: gran parte dei campi è verificabile con _codice_

---

## Validare software generativo: un esempio intuitivo

{{< image src="./evaluation-pipeline.svg" max-h="65vh" alt="Pipeline di valutazione: un dataset di tre lettere con risultati attesi attraversa il template del prompt e il modello; gli output sono verificati da valutatori deterministici, relazionali e LLM-as-a-Judge; il rapporto superato/fallito alimenta la revisione del prompt" >}}

---

{{< slide id="llm-as-a-judge" >}}

## LLM-as-a-Judge

- Idea: usare un LLM (capace) per _valutare_ gli output secondo _criteri_ definiti (cfr. [Zheng et al. (2023)](https://arxiv.org/abs/2306.05685) e la rassegna di [Gu et al. (2024)](https://arxiv.org/abs/2411.15594))
    + valutazione _puntuale_ (assegnare un punteggio a un singolo output su una scala) o _a coppie_ (quale dei due output è migliore?), con o senza risposta _di riferimento_
    + il prompt del giudice descrive una _griglia di valutazione_; la risposta è (idealmente) un __output strutturato__: punteggio + motivazione
    + chiedere _prima la motivazione_ aiuta (_chain-of-thought_: far _ragionare_ il modello passo per passo prima della risposta, cfr. [Wei et al. (2022)](https://arxiv.org/abs/2201.11903)); [G-Eval](https://arxiv.org/abs/2303.16634) va oltre, generando prima _passi di valutazione_ a partire dai criteri

- __Distorsioni__ note dei giudici:
    + distorsione di _posizione_ (preferenza per la prima o la seconda opzione nei confronti a coppie), cfr. [Wang et al. (2023)](https://arxiv.org/abs/2305.17926)
    + distorsione di _verbosità_ (preferenza per risposte più lunghe) e preferenza per gli output della _propria_ famiglia di modelli

- Buone pratiche:
    + usare come giudice un modello _diverso_ (e possibilmente più capace), con temperatura _bassa_
    + preferire criteri _circoscritti_ e _binari_ (es. "ogni punto di forza è sostenuto dalla lettera?") a quelli vaghi (es. "è buono?")
    + __validare il giudice__: confrontare i suoi giudizi con quelli _umani_ su un piccolo campione, prima di fidarsi
        * misurare l'_accordo_: percentuale di accordo, più [κ di Cohen](https://en.wikipedia.org/wiki/Cohen%27s_kappa) $= (p_o - p_e)/(1 - p_e)$, che tiene conto dell'accordo _atteso per caso_ (cfr. [Thakur et al., 2024](https://arxiv.org/abs/2406.12624))
    + usare i giudici solo per ciò che _non_ è verificabile con codice

---

{{< slide id="evidence" >}}

## Quante evidenze servono?

- Un tasso di superamento è una _stima_; dataset piccoli comportano un'incertezza __ampia__:
    + es. 3 lettere su 3 superano la valutazione $\Rightarrow$ l'intervallo di [Wilson](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Wilson_score_interval) al 95% per il vero tasso di superamento è $[0.44, 1]$
    + quindi riportare i tassi _con_ gli intervalli di confidenza (cfr. [Miller, 2024, "Adding Error Bars to Evals"](https://arxiv.org/abs/2411.00640))
- Gli output sono _non deterministici_: ripetere $N$ esecuzioni per caso di test e osservare la _distribuzione_, non una sola esecuzione
- Confrontare due versioni (prompt, modelli) _in condizioni omogenee_:
    + stessi casi di test, stesso numero di esecuzioni, _un_ fattore cambiato alla volta
    + confronti _appaiati_, caso per caso (es. [test di McNemar](https://en.wikipedia.org/wiki/McNemar%27s_test) su superato/fallito o differenze appaiate dei punteggi), anziché confrontare due medie

{{% /section %}}

---

{{< slide id="decision" >}}

# Criteri di scelta per l'adozione

---

{{% section %}}

{{< slide id="three-options" >}}

## Cloud commerciale, fornitore europeo o on-premise?

{{% small "85%" %}}

| | Cloud commerciale (es. API di fornitori USA) | Fornitore europeo (modelli proprietari o a pesi aperti, data center UE) | On-premise (modelli a pesi aperti) |
|---|---|---|---|
| __Qualità__ | massima (modelli di frontiera) | buona: migliori modelli aperti o proprietari UE (es. Mistral) | limitata dall'hardware |
| __Costo__ | per token; economico con volumi bassi | per token, spesso inferiore per modelli aperti | acquisto iniziale dell’hardware + energia + personale; economico solo con volumi alti e costanti |
| __Controllo__ | basso: il produttore modifica o ritira i modelli | medio: i pesi aperti si possono spostare altrove | completo |
| __Conformità__ | contratti (DPA), garanzie sui trasferimenti, giurisdizione estera (es. CLOUD Act USA) | giurisdizione UE; serve comunque un DPA | dati all'interno dell'organizzazione |
| __Time-to-market__ | minuti | da ore a giorni | settimane |

{{% /small %}}

- Spesso la risposta è una __combinazione__ (cfr. [architetture ibride](../llmaas/#/hybrid)), in base a sensibilità dei dati e difficoltà del compito
- Qualunque sia l'opzione, la responsabilità resta __vostra__: es. come _deployer_ secondo l'AI Act e come _titolare del trattamento_ secondo il GDPR

---

{{< slide id="regulation" >}}

## La regolamentazione: il quadro generale

Il software basato sull'IA è soggetto ad (almeno) quattro _livelli_ di regole, ciascuno con un _oggetto_ diverso:

1. il __GDPR__ ([Reg. (UE) 2016/679](https://eur-lex.europa.eu/eli/reg/2016/679/oj)) disciplina il trattamento dei __dati personali__, qualunque sia la tecnologia
2. l'__AI Act dell'UE__ ([Reg. (UE) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)) disciplina i __sistemi di IA__ (in base al _rischio_ del loro _uso_) e i __modelli di IA per finalità generali__
3. le __leggi nazionali__, es. la legge italiana sull'IA ([Legge 132/2025](https://www.gazzettaufficiale.it/eli/id/2025/09/25/25G00143/sg))
4. le __policy dell'organizzazione__, es. le regole interne sull'uso dell'IA generativa

più i _contratti_ (es. termini di servizio dei fornitori, accordi sul trattamento dei dati) e le _licenze_ (dei modelli e dei dati)

{{< image src="./regulation-layers.svg" max-h="35vh" alt="Livelli annidati di regole attorno a un'applicazione basata su LLM (licenze e termini di servizio, policy dell'organizzazione, Legge 132/2025, AI Act dell'UE, GDPR), ciascuno associato alla domanda a cui risponde" >}}

> Avvertenza: è una panoramica da _ingegneri_, __non__ una consulenza legale: nei progetti reali coinvolgere l'_ufficio legale_ e il _Responsabile della protezione dei dati_ (DPO) della propria organizzazione

---

## AI Act dell'UE: un approccio basato sul rischio

{{% multicol %}}
{{% col %}}

{{% small "85%" %}}

- __Rischio inaccettabile__ $\rightarrow$ _vietato_ ([Art. 5](https://artificialintelligenceact.eu/article/5/))
    + es. social scoring, tecniche manipolative, __riconoscimento delle emozioni__ sul lavoro e nell'__istruzione__, raccolta indiscriminata di immagini facciali
- __Alto rischio__ $\rightarrow$ _obblighi_ rigorosi ([Art. 6](https://artificialintelligenceact.eu/article/6/) e [Allegato III](https://artificialintelligenceact.eu/annex/3/))
    + es. selezione dei CV, valutazione del credito e __istruzione__: sistemi per _determinare l’accesso o l’ammissione_ agli istituti (Allegato III, punto 3(a)), _valutare i risultati dell'apprendimento_ (3(b)) o _sorvegliare_ gli esami (3(d))
- __Rischio di trasparenza__ $\rightarrow$ obblighi di _informazione_ ([Art. 50](https://artificialintelligenceact.eu/article/50/))
    + es. i chatbot devono dichiarare di essere IA; i contenuti sintetici devono essere _marcati_; i deepfake devono essere _etichettati_
- __Rischio minimo__ $\rightarrow$ nessun obbligo specifico
    + es. filtri antispam, IA nei videogiochi
- Su un piano distinto: __modelli di IA per finalità generali (GPAI)__, con obblighi per i _fornitori_ ([Art. 53](https://artificialintelligenceact.eu/article/53/); rischio sistemico: [Art. 51](https://artificialintelligenceact.eu/article/51/), [Art. 55](https://artificialintelligenceact.eu/article/55/))

{{% /small %}}

{{% /col %}}
{{% col %}}

{{< image src="./ai-act-pyramid.png" max-h="60vh" alt="Piramide dei rischi dell'AI Act, dall'alto: rischio inaccettabile (vietato), alto rischio (regolamentato), rischio limitato (trasparenza), rischio minimo (non regolamentato)" >}}

{{% /col %}}
{{% /multicol %}}

{{% fragment %}}
> Il nostro __esempio ricorrente è ad alto rischio__: contribuisce a decidere l'_ammissione_ a un dottorato. Lo stesso LLM, usato per riassumere notizie, sarebbe a rischio _minimo_: il regolamento disciplina l'__uso__, non la tecnologia
{{% /fragment %}}

---

## Come scegliere un modello per un compito?

{{% small "85%" %}}

{{% multicol %}}
{{% col %}}

### Vincoli inderogabili (filtrare)

1. __Deployment__: on-premise (hardware disponibile?) o cloud (budget disponibile?)
2. __Protezione dei dati__: i dati possono uscire dall'organizzazione? quale giurisdizione si applica al trattamento dei dati? (cfr. GDPR)
3. __Licenza__: l'uso previsto (es. commerciale) è consentito?
4. __Modalità__: solo testo? immagini? audio?
5. __Capacità__: tool calling? output strutturati? ragionamento?
6. __Finestra di contesto__: entrano prompt + documenti + cronologia?
7. __Lingue__: es. l'italiano è supportato abbastanza bene?

{{% /col %}}
{{% col %}}

### Criteri di preferenza (ordinare)

- _qualità_ sul __proprio__ compito (prossima slide)
- _latenza_ (tempo al primo token, token al secondo)
- _costo_ per richiesta (token × prezzo o ammortamento hardware)
- _stabilità_ dell'offerta (il modello verrà dismesso a breve?)
- _ecosistema_ (librerie client, documentazione, comunità)

{{% fragment %}}
> __Regola pratica__: partire dal modello _più piccolo/economico_ che soddisfa i vincoli inderogabili, misurare la qualità sul proprio compito e passare a modelli _più grandi_ solo se necessario
{{% /fragment %}}

{{% /col %}}
{{% /multicol %}}

{{% /small %}}

{{< mermaid >}}
%%{init: {'flowchart': {'htmlLabels': false}}}%%
flowchart LR
    Z[("Tutti i modelli
    nel catalogo")] --> D[/"Deployment e
    protezione dati"\] --> L[/"Licenza"\] --> C[/"Modalità e
    capacità"\] --> X[/"Finestra di contesto
    e lingua"\] --> S["Rosa di candidati
    (3-5 modelli)"] --> E["Valutare sul
    proprio test set"] --> P(["Scegliere il più economico
    sopra la soglia di qualità"])
{{< /mermaid >}}

---

{{< slide id="open-models-checklist" >}}

## Indicazioni per l'ingegneria: controlli prima dell'adozione

La conformità va verificata per la specifica combinazione di __checkpoint, versione della licenza, licenze del codice e dei dati, deployment e uso previsto__, anziché sulla base del nome del marchio

{{% small "85%" %}}

| Domanda | Cosa verificare |
|---|---|
| esattamente _quale_ artefatto? | repository, hash del checkpoint, data di rilascio, versione della licenza |
| uso commerciale? | Apache/MIT: sì; CC-BY-NC: no; licenze specifiche: leggere le restrizioni d'uso |
| uso on-premise? | la licenza, non la disponibilità dei file (es. Llama 4 multimodale nell'UE) |
| fine-tuning, distillazione, fusione? | se i derivati ereditano restrizioni; licenza di _ogni_ modello di partenza |
| servizio ospitato o redistribuzione? | se esporre un'API conta come distribuzione (sì per Gemma 3); obblighi di attribuzione e denominazione |
| limiti geografici o di scala? | es. Llama 4: clausola UE sui multimodali, 700M utenti mensili |
| dati di addestramento e fine-tuning? | provenienza, licenze (es. ODC-By), dati personali |
| ruolo nell'AI Act? | stiamo diventando _noi_ fornitori GPAI (es. con fine-tuning e distribuzione)? si applica l'esenzione open source? |
| serve riproducibilità? | hosting autonomo: bastano pesi distribuiti con una licenza permissiva; audit o ricerca: preferire modelli completamente aperti (OLMo 3, Apertus) |

{{% /small %}}

> Da ricordare: __pesi → licenza → ricetta → dati → ruolo normativo__: l’apertura va valutata rispetto ai componenti e alle libertà necessari per l’uso previsto

---

## Indicazioni per l'ingegneria: tre scenari

{{% multicol %}}
{{% col %}}

### Un laboratorio UniBo, on-premise

- vuole: RAG/chat locale, fine-tuning su materiale universitario, esperimenti riproducibili
- adatti: OLMo 3, Apertus (riproducibilità); Gemma 4, Qwen3, gpt-oss, modelli Apache di Mistral (bastano i pesi); Minerva (ricerca centrata sull'italiano)
- escluso: Llama 4 _multimodale_ (clausola UE)

{{% /col %}}
{{% col %}}

### Una startup italiana che rivende un modello adattato

- vuole: fare fine-tuning, ospitare e vendere il modello
- adatti: pesi Apache/MIT (Gemma 4, Qwen3, gpt-oss, DeepSeek-R1, Mistral Large 3, OLMo 3, Domyn Small)
- attenzione: modelli _distillati_ R1 (quelli basati su Llama ne ereditano la licenza); Gemma 3 (restrizioni trasmesse ai clienti); Teuken v0.6 (non commerciale)

{{% /col %}}
{{% col %}}

### Una pubblica amministrazione

- vuole: deployment sotto controllo UE e verificabilità
- _verificabilità_: Apertus, OLMo 3, Minerva
- _controllo operativo_: qualsiasi modello con licenza permissiva su infrastrutture UE (incl. Mistral, EuroLLM, Velvet, Domyn Small)
- attenzione: IT4LIA/CINECA offre _risorse di calcolo_, non una licenza del modello

{{% /col %}}
{{% /multicol %}}

{{% /section %}}

---

{{% section %}}

{{< slide id="exercise-compare" >}}

## Esercizio guidato 1: confrontare i modelli sull'esempio ricorrente

> __Obiettivo__: selezionare una _rosa di modelli_ e confrontarli sulla base di evidenze, usando il sistema di valutazione delle lettere della [lezione precedente](../llmaas/#/letter-scoring): tutte e 3 le lettere, 5 esecuzioni per lettera e per modello

```bash
poetry run python -m snippets -l governance -x 1                                     # default shortlist
poetry run python -m snippets -l governance -x 1 MODEL1 MODEL2@http://localhost:11434/v1   # custom shortlist (e.g. a local model)
```

- ogni `MODEL` è il nome di un modello Open Router, oppure `MODEL@BASE_URL` per altri fornitori (es. Ollama)
- lo script stampa una riga per modello: punteggi (media ± deviazione standard), errore rispetto ai punteggi _di riferimento_ della commissione, graduatoria dei candidati, fallimenti, latenza, token e costo
    + dati grezzi (una riga per esecuzione) in `output/model-comparison.csv`
- discutere i risultati:
    + quale modello è il più __accurato__? il più __costante__? il più __economico__? il più __veloce__?
    + i modelli __concordano__ sulla graduatoria dei candidati?
    + quale scegliereste e con quale _regola_? Vi fidereste di una decisione basata su 3 lettere? (cfr. [quante evidenze](#/evidence))

(codice completo [qui](../lab-snippets/snippets/lecture_governance/exercise1/compare_models.py); provare anche `TEMPERATURE=0` vs. `TEMPERATURE=1`)

---

{{< slide id="exercise-decision" >}}

## Esercizio guidato 2: motivare una decisione per iscritto, in gruppi

> __Obiettivo__: _motivare_ per iscritto l'adozione di un sistema basato su LLM, come dovrebbe fare un'organizzazione

- In gruppi di 3–5 persone (mescolando manager e tecnici), scegliere un caso d'uso: l'[esempio ricorrente](../llmaas/#/running-example) o uno del _proprio_ ufficio (es. classificare richieste dei cittadini, riassumere fascicoli, preparare risposte)
- Scrivere un __documento di una pagina che motivi la decisione__ (cfr. _Architecture Decision Records_, <https://adr.github.io/>), rispondendo alle seguenti domande:
    1. __Classificazione__: il sistema è _ad alto rischio_ secondo l'AI Act? Quali dati personali tratta?
    2. __Deployment__: cloud commerciale, fornitore europeo o on-premise? Perché? (usare le cinque dimensioni)
    3. __Rosa di candidati__: quali 3 modelli e quali _vincoli inderogabili_ hanno superato? (applicare i [controlli prima dell'adozione](#/open-models-checklist))
    4. __Valutazione__: cosa conterrebbe il _golden set_? Quale _regola di decisione_?
    5. __Supervisione__: cosa controllano le persone? Chi ne _risponde_? Quando viene _riesaminata_ la decisione?
- Condividere e discutere: un collega potrebbe _riprodurre_ la scelta leggendo solo il documento?

{{% /section %}}

---

## Riepilogo

1. L'apertura ha __gradi__: pesi, codice, dati, documentazione e licenze sono questioni _distinte_; verificare il __checkpoint esatto__
2. I pesi aperti offrono la libertà di scegliere __dove__ gira un modello: molti fornitori, aggregatori o hardware proprio
3. L'__esecuzione locale__ è sostenibile con volumi costanti, modelli piccoli/medi e competenze interne; altrimenti i modelli aperti ospitati nell'UE sono una via intermedia
4. I modelli europei e italiani sono preziosi per ricerca e apertura, ma __non ancora__ competitivi con quelli globali: la sovranità _operativa_ non richiede un modello _nazionale_
5. I benchmark pubblici mostrano lo stato dell'arte; il __proprio golden set__ indica quale modello è adatto a _voi_
6. Decidere con __criteri__ espliciti, metterli per iscritto e __riesaminarli__ periodicamente

---

{{% import path="reusable/back.md" %}}
