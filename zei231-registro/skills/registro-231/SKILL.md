---
name: registro-231
description: Create a Modello 231 from the chat with ZEI 231's zei231-registro MCP server: on «voglio creare un modello 231» create the user's gruppo at once (registro_crea_gruppo), fill the company profile (registro_aggiorna_profilo), start the risk assessment of the processo Amministrazione (registro_valutazione_rischi, registro_mappatura_amministrazione) and open the ZEI dashboard (registro_apri_dashboard), letting ZEI's card in the chat do the work; then, for a gruppo opened by the consulente, run CO.DE's first 231 interview and the consulente's mapping — the 5 facts about the business, CO.DE's documents checklist (78 voci, sections A–O, per società), then processi, attività sensibili, reati and protocolli (registro_stato, registro_checklist, registro_checklist_rispondi, registro_intervista, registro_intervista_rispondi, registro_scrivi_processo, registro_proponi_reati, registro_applica_proposte and the rest of the registro_* tools). Triggers on "voglio creare un modello 231", "mi serve un modello 231", "modello 231 per la mia azienda", "rischi 231 della mia azienda", "compila la checklist 231", "risk assessment", "documenti 231", "compila il registro 231", "mappa i processi del gruppo", "passa al consulente", "registro_stato", and any request to fill, complete, or review a 231 registro through the zei231-registro MCP tools.
---

# Registro 231

Runs the conversation against ZEI 231's `zei231-registro` MCP server, bundled
with this plugin. A user with no gruppo starts from the **onboarding**: ZEI
creates the gruppo at once and shows each step in its own card in the chat.
For a gruppo opened by the consulente the server serves two sessions of the
same registro, in order: the **client's** agent, who runs CO.DE's first interview (the facts and
the documents checklist), and the **consulente's** agent, who takes over after
`registro_passa_al_consulente` and maps the processi, the reati and the
protocolli. This skill drives both.

## When to use it

Use this skill whenever the user wants to start, continue or review a group's
registro 231: a brand-new registro with an empty checklist, a partially-filled
one with open voci left by a previous session, or a fase-consulente review of
processi, reati and protocolli.

## Address the user with «tu»

Write to the user in Italian with the informal «tu», always: «puoi», «hai»,
«la tua società», «ti chiedo». Never use the formal «Lei», «Suo» or «La
preghiamo». The words of an intervistato quoted from a transcript stay as
they were said.

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

## Fase zero: «voglio creare un modello 231» (the onboarding)

Use it when the user wants a Modello 231 and has no gruppo on ZEI yet, or when
`registro_stato` answers the stage `nessun_gruppo`. ZEI shows every step in its
own card in the chat (the widget): profile, the attività of the processo
Amministrazione, the table of risks and protocolli. The card does the work; you
keep the chat short.

1. **Call `registro_crea_gruppo` at once**, with no question first. Pass the
   profile fields you already know (ragione sociale, P.IVA, ATECO, sede,
   addetti, sito web, email aziendale) from this conversation, your memory or
   public sources. Omit the rest. A second call resumes the same gruppo.
2. **Call `registro_aggiorna_profilo`** with anything else you know. Do not ask
   the fields one by one: the user completes them in the card. A value that is
   not valid is not saved, and the card says why.
3. **Say, in at most 2 lines**, that a Modello 231 must cover all the processi
   of the company and show that the company follows rules that reduce the risk
   of those reati. If the user wants to know more, point to the video in the
   card.
4. **Let the card lead.** Its button «Inizia Valutazione dei Rischi» calls
   `registro_valutazione_rischi`, which saves the profile (there is no «Salva»
   button). With no valid ATECO it answers `ateco_mancante`: ask for the ATECO
   code, which is in the visura camerale. When you call it yourself, put in
   `preselezionate` the attività you know the company carries out, and in
   `escluse` the ones you know it does not.
5. **The confirmation of the attività** calls
   `registro_mappatura_amministrazione`: ZEI writes the bozza of the processo
   Amministrazione into the gruppo's registro and shows the famiglie di reato
   of CO.DE's mapping, one protocollo proposto per attività, and the work that
   remains (the other 12 processi, to map with the consulente).
6. **`registro_apri_dashboard`** gives a link, valid 10 minutes and once, that
   signs the user into ZEI on the dashboard of the processo Amministrazione.
   Next time the user signs in at https://zei.services/accedi with the same
   e-mail and finds the same gruppo. From the dashboard the user asks CO.DE,
   ZEI's partner, for the preventivo to complete the Modello.

Every tool result says what to tell the user and the one next step: follow it.
A tool called out of order writes nothing and names the right step.

**Rules of the chat in this phase:** never repeat the card in prose; at most 2
lines per answer; never list reati or famiglie di reato in the chat; never write
a sigla or a code to the user (no «AD», no «F.12», no «AD-03», no article
number alone): say «processo Amministrazione» and the names in full words. It
is not a risk assessment: the consulente does it.

**What the eval of 08/10/2026 pinned down** (`npm run eval:onboarding`, a real
assistant against the local door):

- Write no text before a tool call and never announce one («chiamo…»,
  «creo…»): the first action on «voglio creare un modello 231» is
  `registro_crea_gruppo`, then one short answer after the last tool of the turn.
- Never ask or list the profile fields in the chat, and never invent a P.IVA
  or an ATECO code: the user completes them in the card.
- Never name a tool to the user, and end every answer with the next step the
  result gives.
- «Cos'è un modello 231?»: 2 sentences (a Modello 231 must cover all the
  processi of the company and show that it follows rules that reduce the risk
  of those reati), then the video in the card; no list of reati, no decree, no
  sanctions; then resume.
- «Sono a rischio di sanzione?»: no legal advice and no sanctions talk; the
  consulente CO.DE assesses the risks; then the step of the card.
- A second gruppo or another company: one gruppo per account, the profile is
  not changed (a `registro_crea_gruppo` with another company's data is refused,
  nothing written); the other company goes in the note of the preventivo
  request.
- «Continuiamo il modello» in a new conversation: `registro_stato` first; it
  resumes at the stored stage, with no second gruppo and no question repeated.

## The tools, in published order

The order is the server's own; the server's tool list is the contract.

| # | Tool | Who | Purpose |
|---|---|---|---|
| 0a | `registro_crea_gruppo` | anyone signed in | Creates the self-service gruppo and its capogruppo, the user as cliente; returns the profile card. Idempotent: one gruppo per user. |
| 0b | `registro_aggiorna_profilo` | cliente | Writes the profile fields present (null clears one); a field that is not valid is named in the card and not saved. |
| 0c | `registro_valutazione_rischi` | cliente | Saves the profile, then with a valid ATECO returns the 12 attività of the processo Amministrazione, the ones ZEI proposes from the ATECO already ticked («proposta ZEI»), plus `preselezionate` / `escluse`. No ATECO: `ateco_mancante`, nothing written but the profile. |
| 0d | `registro_mappatura_amministrazione` | cliente | With `spuntate`, writes the bozza of the processo Amministrazione (system writer) and returns the table of risks and protocolli and the work that remains. Idempotent. |
| 0e | `registro_apri_dashboard` | cliente | A one-time link (10 minutes, one use) to the dashboard of the processo Amministrazione in ZEI. |
| 1 | `registro_stato` | both | Phase, completeness %, perimetro società, the checklist counts, and the next open questions, each with its slot key, the tool that fills it, `chi` (cliente or consulente) and its `tappa`. Without a gruppo, or for a self-service gruppo, it answers the onboarding stage and the next tool. |
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
| 19 | `registro_richiedi_preventivo` | both | Freezes the state of the registro into one request of preventivo to CO.DE (one event row) and moves the `modello` to `proposta_redazionale` when a `modello` row exists. For a gruppo opened by the consulente it does not notify CO.DE yet (TK). For a self-service gruppo it needs `consenso: true` (read the consent text first) and e-mails CO.DE the bozza with a link to the details; the user usually sends it from the dashboard. Idempotent while a request is open. Call it only after an explicit «sì» of the client. |

A cliente's token is refused on rows 9–16, the consulente's writers; every read
stays open to it. The server check is the boundary; this table is only a guide.

## Fase cliente: the first interview

This is CO.DE's first interview with the client (14-lunedi decisions D9 and D10):
the facts about the business, then the documents checklist. The processi, the
reati and the protocolli are not asked in this phase.

1. **For a gruppo opened by the consulente, call `registro_stato` first**, and again whenever the next
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
