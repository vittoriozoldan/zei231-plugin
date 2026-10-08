---
name: registro-231
description: Show a company that is considering a Modello 231 a factual diagnosi preliminare first (registro_diagnosi_domande, registro_diagnosi_preliminare — the exposed famiglie di reato, a typical scenario, the sanctions, official numbers and real cases with sources, the art. 6–7 exemption) and then offer to start the Model; then run CO.DE's first 231 interview with a company group and then the consulente's mapping — the 5 facts about the business, CO.DE's documents checklist (78 voci, sections A–O, per società), then processi, attività sensibili, reati and protocolli — through this plugin's zei231-registro MCP server (registro_stato, registro_checklist, registro_checklist_rispondi, registro_scrivi_processo, registro_proponi_reati, registro_intervista, registro_intervista_rispondi, registro_applica_proposte, registro_mappatura_amministrazione, registro_richiedi_implementazione, and the rest of the registro_* tools). Use when the user wants to fill CO.DE's risk-assessment checklist, interview the client on the documents a società already has, hand a registro from the client to CO.DE, the consulente partner, or, as the consulente, map the group's processi in CO.DE's own order (AD, AF, FP, AP … WB), pre-populate an attività sensibile's typical famiglie di reato and assess P, I, K_AD and SCE. Triggers on "mi serve un modello 231?", "diagnosi 231", "rischi 231 della mia azienda", "compila la checklist 231", "risk assessment", "documenti 231", "compila il registro 231", "mappa i processi del gruppo", "propone i reati tipici", "passa al consulente", "registro_stato", and any request to fill, complete, or review a 231 registro through the zei231-registro MCP tools.
---

# Registro 231

Runs the conversation against ZEI 231's `zei231-registro` MCP server, bundled
with this plugin. Before any registro there is the **diagnosi preliminare**: a
company that is only considering a Modello 231 first sees why it needs one.
Then the server serves two sessions of the same registro, in
order: the **client's** agent, who runs CO.DE's first interview (the facts and
the documents checklist), and the **consulente's** agent, who takes over after
`registro_passa_al_consulente` and maps the processi, the reati and the
protocolli. This skill drives both.

## When to use it

Use this skill whenever the user wants to start, continue or review a group's
registro 231: a brand-new registro with an empty checklist, a partially-filled
one with open voci left by a previous session, or a fase-consulente review of
processi, reati and protocolli.

## Never guess a tool's registered name or its parameters

A host may list this plugin's tools under a name you have not seen before —
never assume it follows a pattern from a different session or a different
plugin. Before calling any `registro_*` tool for the first time in a
conversation, load its exact registered name and parameter schema (through
the host's own tool list or tool search), and call it with that name and
those parameter names exactly as loaded. A guessed name or a guessed
parameter shape fails with an error naming the real one — read that error and
retry with the exact name and parameters it gives, rather than guessing
again.

## Fase zero: the diagnosi preliminare (before any modelling)

Use it when the company has not started a Modello 231 yet: the user asks whether
the company needs one, you suggested a 231, or `registro_stato` answers that the
account has no gruppo. The aim is that the client understands the problem, with
facts: a company without a Modello has nothing to put forward in a 231
proceeding against it. The diagnosis writes nothing and needs no gruppo.

1. **Call `registro_diagnosi_domande`.** It lists the questions ZEI needs, each
   with where the answer is usually found.
2. **Answer every question you can yourself, without asking the client**: from
   this conversation, your memory of this user, earlier conversations if the
   host lets you search them, and public sources (visura, registro imprese, the
   company website; keep the URL and the date). General knowledge about a sector
   is not a fact about this company.
3. **Ask the client only the rest**, at most three questions per message. A
   question nobody can answer stays out: an absent field is reported as unknown,
   never guessed. `attivita` is the only required field.
4. **Call `registro_diagnosi_preliminare`** with the profile and, in `fonti`, the
   source of each field (`cliente`, `memoria`, `documento`, `fonte_aperta` with
   the URL in `citazione`).
5. **Show the report to the client exactly as it comes back.** Do not shorten
   the sources, do not add a number, a case, a sentence or a source that the
   tool did not return, and do not soften or sharpen it. If the client asks for
   a figure the report does not hold, say that ZEI has not verified it.
6. **Offer the first part of the Model**: «Vuole vedere quali attività del
   processo amministrativo vi espongono e come si proteggono?». A «no» or a
   «ci penso» is an answer: thank the client and write nothing.
7. **Map ONE processo, «Amministrativo e affari generali» (AD).** Call
   `registro_mappatura_amministrazione` with no `spuntate`: it returns the 12
   attività sensibili. Ask the client which ones the company carries out — one
   at a time, or as a list to tick, as the client prefers; answer yourself only
   what you know for certain. Then call it again with the codes the client
   confirmed in `spuntate`, and show the result as it comes back: for each
   attività, the famiglie di reato configurabili and the protocolli consigliati
   of CO.DE's catalogue. Say that it is the start of the mapping, not a risk
   assessment. Never map a second processo and never give a risk assessment:
   the rest is the consulente's work.
8. **Offer the implementation**: the rest of the Model (the other processi, the
   risk assessment, the definitive protocolli, Parte generale, codice etico,
   sistema disciplinare and the Organismo di Vigilanza) is done by a dedicated
   consulente of CO.DE, ZEI's partner, under a separate agreement. Ask: «Vuole
   chiedere il preventivo per completare il Modello?».
9. **Only after an explicit «sì»**, collect the referente's name, role, e-mail
   and (optional) phone, read the consent text of
   `registro_richiedi_implementazione` to the client word for word, and call it
   with `consenso: true` only if the client accepts it in this conversation.
   Tell the client the request was sent and that ZEI and CO.DE will contact the
   referente. A client who belongs to a gruppo on ZEI can also use
   `registro_richiedi_preventivo` (below) from the registro.

The same path exists on the web, at https://zei.services/modello-231.

## The tools, in published order

The order is the server's own; the server's tool list is the contract.

| # | Tool | Who | Purpose |
|---|---|---|---|
| 0a | `registro_diagnosi_domande` | anyone | The questions of the diagnosi preliminare, with where to find each answer. No gruppo needed, writes nothing. |
| 0b | `registro_diagnosi_preliminare` | anyone | The diagnosi preliminare: from the company profile, the report to show as it is (exposed famiglie with reasons, scenario, sanctions, official numbers and real cases with sources, art. 6–7, unknowns, the offer) plus `primaParte` (T4 facts and ONE processo with its attività sensibili). No gruppo needed, writes nothing. |
| 0c | `registro_mappatura_amministrazione` | anyone | The 12 attività sensibili of «Amministrativo e affari generali»; with `spuntate`, the famiglie di reato configurabili and up to three CO.DE protocolli per attività. No gruppo needed, writes nothing. |
| 0d | `registro_richiedi_implementazione` | anyone | Stores the request of implementation (company, ticked attività, referente, consent) and e-mails it to ZEI for CO.DE. Only after an explicit «sì» and the consent read to the client. No gruppo needed. |
| 1 | `registro_stato` | both | Phase, completeness %, perimetro società, the checklist counts, and the next open questions, each with its slot key, the tool that fills it, `chi` (cliente or consulente) and its `tappa`. Call first, always. |
| 2 | `registro_leggi` | both | The registro as the pages show it: the processi × società matrix, or one processo's rows with P, I, RL, K_AD, SCE, RN (rischio netto) and the protocollo. |
| 3 | `registro_catalogo` | consulente | CO.DE's own albero standard: 13 processi (11 with a standard attività list, 2 condizionali), each attività a candidate attività sensibile, and the typical famiglie di reato per processo by `seed_key`. No P × I. Needs no gruppo. |
| 4 | `registro_checklist` | both | CO.DE's documents checklist (DOCUMENTI_231 rev. 1): 78 voci in 13 sections A–O (no J, no K), each with numero, titolo, descrizione and, per società of the perimetro, the esito and the nota already written; the 6 header fields; the counts. At most 40 voci per call: with no `sezione` it returns A–F and names the next section (G); `sezione` returns one section; `soloAperte: true` skips the voci every società has already answered or handed over. |
| 5 | `registro_checklist_rispondi` | both | Writes 1 to 30 checklist answers in one call. A voce: `{ societa: [slug…], voce: 1–78, esito: si \| no \| non_pertinente \| non_so, nota? }`. A header field: `{ societa: [slug…], campo, valore }` (`valore: null` clears it). `fonte` and `citazione` are per call: no `fonte` = `cliente`; any other `fonte` needs a `citazione`. One `risposta` row per item and società, one event per call; an item the registro refuses (a società outside the perimetro) is refused alone and the others stay written; an item of a bad shape (voce outside 1–78, an unknown esito or campo) refuses the whole call, which writes nothing. |
| 6 | `registro_intervista` | both | CO.DE's interview form (SCHEDA RISK ASSESSMENT rev.01), one scheda per società × processo × funzione intervistata. With no `intervista`: the list of the gruppo's schede (id, società, processo, funzione, data, stato, answers out of 78). With `intervista`: one page of the scheda at a time (`sezione` A, A1, A2, A3, B or C; default A), every voce with its id, its options and scores, the answer already written, and the proposals of SCE (from A1), K_AD (from A2 and A3) and P/G per famiglia (from C); the text names the next page. |
| 7 | `registro_intervista_rispondi` | both | Creates or updates a scheda (`scheda: { societa, processo, funzione, intervistato?, data?, registrazioneConsenso?, verbaleFirmato?, stato?, note? }`; `processo` is a registro code or a CO.DE sigla) and/or writes 1 to 40 answers `{ voce, valore }`: an open question `{ testo }`, an A1/A2/A3/B item `{ opzione, punteggio? }` (the option index; `punteggio` only for a range option such as 1-2), a famiglia of C `{ realizzabile, na, p, g, commento }`. With the consent the recording is deleted 7 days after the interview date. Append-only, one event per writer; a bad item is refused alone. A transcript the user pastes is a source: `fonte: documento`, `citazione` = the quoted passage. It never changes P, I, K_AD or SCE of the registro. |
| 8 | `registro_dichiara` | both | Writes up to 10 answers on the gruppo facts: each item is `chiave` (the slot key `registro_stato` gives, for example `risposta:{società}:T4-01`), `valore` (any JSON value), `fonte` (`memoria` \| `cliente` \| `documento` \| `fonte_aperta`) and `citazione`, required whenever `fonte` is not `cliente`. Refused for a `checklist:` key: use `registro_checklist_rispondi`. |
| 9 | `registro_scrivi_processo` | consulente | Creates or updates a processo, the full set of società that run it, and its attività. |
| 10 | `registro_segna_processo_non_applicabile` | consulente | Marks a catalogue processo the gruppo never runs. Refused once the processo exists. |
| 11 | `registro_scrivi_attivita_sensibile` | consulente | Creates or updates one società's attività sensibile: responsabile, dipartimento, stato. |
| 12 | `registro_proponi_reati` | consulente | Pre-populates an attività sensibile with one reato per famiglia tipicamente associata al processo, F.1 → F.28 (ZEI's own editorial proposal, not a CO.DE artifact), unassessed (null P, I, K_AD and SCE, no protocollo): a system proposal, not a fact. |
| 13 | `registro_scrivi_reato` | consulente | Associates a reato with an attività sensibile: P, I, K_AD, SCE (each 1–4), protocollo (the `presidio` field), nota. The database computes RL = P × I and RN = max(1, RL / media(K_AD, SCE), 0,5 per eccesso). Consulente only. |
| 14 | `registro_scrivi_presidio` | consulente | Creates or updates a PS/PT/PV protocollo di controllo interno and its implementazione. |
| 15 | `registro_rimuovi` | consulente | Removes an association, an attività sensibile, a protocollo (`oggetto: presidio`) or a società from a processo. The only destructive tool; call it only on an explicit request. |
| 16 | `registro_applica_proposte` | consulente | Applies the proposals of one scheda: for every association attività sensibile × reato of the scheda's società in its processo, whose reato belongs to a famiglia with P or G in section C, writes P, I (the scheda's G), K_AD and SCE, one event for the call. Without `sovrascrivi` it fills only the empty fields; `sovrascrivi: true` replaces the values already written. Refused when the processo of the scheda is not in the registro. The aggregation of A1 → SCE and A2/A3 → K_AD (mean of the group means, rounded half up) is ZEI's proposal, still to validate with CO.DE. |
| 17 | `registro_rimanda_al_consulente` | both | Hands one open slot to the consulente with a note, when the client does not know the answer: a checklist voce is `checklist:{società}:{NN}`, NN 01–78. Writes no value. A voce handed over counts as answered for the hand-off. |
| 18 | `registro_passa_al_consulente` | both | Closes the fase cliente. Refused while any (società of the perimetro × voce) of the checklist has neither an esito nor a rimando: the refusal gives the count and the first 3 keys. From here every write is authored consulente. A second call writes nothing. |
| 19 | `registro_richiedi_preventivo` | both | Freezes the state of the registro into one request of preventivo to CO.DE (one event row) and moves the `modello` to `proposta_redazionale` when a `modello` row exists. It does not notify CO.DE yet: that channel is still TK. Idempotent while a request is open. Call it only after an explicit «sì» of the client. |

A cliente's token is refused on rows 9–16, the consulente's writers; every read
stays open to it. The server check is the boundary; this table is only a guide.

## Fase cliente: the first interview

This is CO.DE's first interview with the client (14-lunedi decisions D9 and D10):
the facts about the business, then the documents checklist. The processi, the
reati and the protocolli are not asked in this phase.

1. **Call `registro_stato` first, always** (after the diagnosi preliminare, when it ran), and again whenever the next
   question is unclear. In fase cliente it returns, in this order: the 5 facts
   about the capogruppo's business (tappa T4), the 6 header fields of the
   checklist for each società, then the open voci 1 → 78 (tappa T1), voce by
   voce for all società at once.
2. **The 5 facts first**: public tenders, waste, foreign trade, shareholdings,
   and the main activity of the company. Each one has its slot key
   (`risposta:{società}:T4-01` …); write the client's answer with
   `registro_dichiara`, using that key as `chiave`.
3. **Then the documents checklist, section by section, A to O.** Read the
   section with `registro_checklist` (the first call also gives the header
   fields), ask, and write with `registro_checklist_rispondi`, in the client's
   own words in the `nota`.
4. **Pre-fill the obvious answers** from what is already known: the facts
   already declared, the visura, the website, the documents in the
   conversation. Write each one with `fonte` (`memoria`, `documento` or
   `fonte_aperta`) and a `citazione`, and have the client confirm it.
5. **A voce that cannot apply** is `non_pertinente`, with the reason in the
   `nota` (for example, listed financial instruments for a private S.r.l.).
   Never invent an answer.
6. **Ask at most three or four voci of the same section per turn.** One
   question per voce holds for all società; write the per-società exceptions
   as separate items.
7. **When the client does not know**, call `registro_rimanda_al_consulente`
   with the voce key `checklist:{società}:{NN}` and a note that says what is
   missing and who knows it. `non_so` («Da verificare») is an answer too: use
   it when the client knows the document exists but must check it.
8. **After the documents checklist, the interviews per processo** (D14): one
   scheda per processo with the funzione responsible for it, read with
   `registro_intervista` and written with `registro_intervista_rispondi`,
   section by section (A, A1, A2, A3, B, C), in the interviewee's own words
   and scores. A transcript the user pastes is a source: `fonte: documento`,
   `citazione` = the quoted passage. Never invent a punteggio: a voce the
   interviewee did not score stays empty. The proposals of P, I, K_AD and SCE
   are the consulente's to apply. The hand-off does not wait for the schede.
9. **Never ask the client about processi, attività, reati, protocolli, P, I,
   K_AD or SCE.** They are the consulente's work after the hand-off.
   `registro_proponi_reati` refuses in fase cliente.
10. **When every voce has an esito or a rimando for every società**, call
    `registro_passa_al_consulente` once. A refusal names how many cells are
    still open and the first 3 keys: answer them, then call it again.
11. **Then ask the client whether to hand the rest of the work to CO.DE**, and
    call `registro_richiedi_preventivo` only after an explicit «sì».

## Fase consulente: the processi, the reati and the protocolli

After the hand-off every write is authored consulente. This is CO.DE's own
order of work (assessment-231, Fase 3 and Fase 4).

1. **Walk the albero, one processo at a time, in CO.DE's numero order**:
   AD, AF, FP, AP, CM, TE, IT, HR, SL, GA, SG, WB. TR is never asked: it is
   CO.DE's own fallback bucket. FP comes from a fact (contributi pubblici,
   finanziamenti agevolati, crediti d'imposta, fondi per la formazione). For
   each processo, start from the catalogue's standard attività
   (`registro_catalogo`). If the gruppo runs it, write it with
   `registro_scrivi_processo` (società and attività). If not, call
   `registro_segna_processo_non_applicabile` with the reason. The answers of
   the checklist and the T4 facts are the evidence to start from.
2. **Once a processo exists**, map its attività sensibili per società with
   `registro_scrivi_attivita_sensibile`.
3. **For each attività sensibile with reati still missing**, call
   `registro_proponi_reati` to pre-populate one reato per famiglia tipica of
   its processo, F.1 → F.28, then walk the proposed rows with
   `registro_scrivi_reato` to set P and I (1–4), K_AD and SCE (1–4 each), and
   link a protocollo. Add any other reato the same way. When a scheda of
   that società and processo exists (`registro_intervista`), read its
   proposals, then call `registro_applica_proposte` to fill the empty P, I,
   K_AD and SCE, and review each proposed value before you rely on it. K_AD is the
   «coefficiente di adeguatezza del Modello» (1 controllo assente · 2 carente ·
   3 adeguato · 4 pieno controllo); SCE is the «sistema di controllo
   esistente» of the ente (1 assente · 2 basso · 3 medio · 4 alto).
4. **Read the rischio netto (RN)** back from `registro_leggi`: «da sanare» is
   RN over 2 (CO.DE default; the soglia per ente is TK).
5. **The implementazione of the protocolli and any residual gap** follow the
   per-slot flow: call `registro_stato` again and work through what it still
   lists as open.

## The `tappa` argument

`registro_stato` takes an optional `tappa` (T0–T9, the stages of
`docs/revamp-code/08-sequenza-domande-cliente.md`). The stages that hold slots
today: T1 «I documenti che la società ha già» (the documents checklist), T4
«Domande sull'attività» (the 5 facts), T5 «Processi e attività» and T7
«Controlli già in essere» (both of the consulente). In fase cliente the filter
keeps only the cliente's own questions, so T5 and T7 return none.

## The memory-first rule

1. Before you ask the client, look for the answer in these places, in this
   order: this conversation; your memory of this user, if the host gives you
   memory; earlier conversations with this user, if the host lets you search
   them; the documents that the client uploaded in this conversation; public
   sources (keep the URL and the date of each fact).
2. Make two lists: the voci with an answer that you found, and the voci with
   no answer.
3. Show the first list for confirmation, at most 5 items in one message, one
   line each with its value and its source. Then ask one question: «Mi
   conferma questi dati, oppure c'è qualcosa da correggere?»
4. Write only the items that the client confirms, with `fonte: "memoria"`,
   `"documento"` or `"fonte_aperta"` and a `citazione` naming the source. If
   the client corrects an item, write the corrected value with
   `fonte: "cliente"` (no `citazione` needed). If the client does not confirm
   an item, do not write it: ask it again as an open question.
5. Then ask the voci of the second list, in the order of `registro_checklist`.
6. After each write, read the checklist line of the tool result («… da
   chiedere») and tell the client how many voci are left.

## Rules for every answer

- Never write a value from memory without the confirmation of the client.
- General knowledge is not memory. Never give a fact of your general
  knowledge as a fact about this company.
- A `documento` answer needs the file name and the page or the section.
- A `fonte_aperta` answer needs the URL and the date of the visit.
- Never ask a question that `registro_stato` marks «del consulente». Never
  ask for a value of P, I, K_AD or SCE.
- Write the name of a person only if the client says it or confirms it.
- Never write «albero validato», «checklist reati validata» or «mappatura
  validata». These sentences belong to the consultant.
- Never print «in regola», «conforme» or «protetto».
- Call `registro_richiedi_preventivo` only after an explicit «sì» of the
  client.

## House rules

- **Never invent** an esito, a responsabile, a protocollo, an implementazione,
  or a value of P, I, K_AD or SCE. Write only what the person in the
  conversation states, in their own words.
- **A reato the app catalogue does not hold** is refused by
  `registro_scrivi_reato` with its three nearest matches; read them back to
  the person and use the exact `seed_key` they confirm. Never drop the reato
  silently and never invent a `seed_key`.
- **`registro_proponi_reati` writes a proposal, not a valuation.** Say so
  when presenting it: P, I, K_AD and SCE stay null until the consulente sets
  them with `registro_scrivi_reato`.
- **Never print** «in regola», «conforme», or «protetto»: a measurement of
  what was assessed, when, and by whom is the honest statement; a compliance
  claim is not.

## Done checklist

- Fase cliente: `registro_checklist` with `soloAperte: true` answers «Nessuna
  voce aperta», `registro_passa_al_consulente` was accepted, and the client
  answered the preventivo question.
- Fase consulente: every catalogue processo is accounted for (live, or marked
  non applicabile); every attività sensibile the consulente has reached has
  run through `registro_proponi_reati` at least once, and every reato it
  proposed has P, I, K_AD and SCE from `registro_scrivi_reato` or an explicit
  `registro_rimanda_al_consulente`.
