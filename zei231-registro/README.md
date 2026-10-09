# ZEI 231 — Registro

Questo plugin dà al tuo assistente quattro skill e il collegamento a ZEI 231 (https://zei.services), il registro del Modello di organizzazione, gestione e controllo previsto dal D.Lgs. 231/2001. Le skill rispondono alle domande sul Modello 231 con i testi di legge verificati su Normattiva il 09/10/2026. Il collegamento MCP permette di creare e compilare il registro 231 della tua società dalla chat.

## Le quattro skill

| Skill | Si attiva quando | Che cosa fa |
|---|---|---|
| `orientamento-231` | chiedi cos'è il Modello 231, se è obbligatorio, a chi si applica, le sanzioni, chi lo redige e quanto costa, ogni quanto si aggiorna, se basta un fac simile | Risponde in 2-4 frasi con l'articolo di legge, rimanda alla pagina della guida e offre di iniziare il registro. |
| `reati-presupposto-231` | chiedi quali reati rientrano nel decreto o una famiglia (corruzione, reati societari, sicurezza sul lavoro, ambientali, tributari, ...) | Risponde dall'elenco di 28 famiglie e 219 voci (`references/famiglie.md`) con articolo e link. |
| `organismo-di-vigilanza-e-codice-etico` | chiedi dell'organismo di vigilanza, del codice etico, del whistleblowing (D.Lgs. 24/2023), della parte speciale e dei protocolli | Risponde dalle pagine della guida; dove la legge tace o la questione è discussa, lo dice. |
| `registro-231` | vuoi creare o compilare il registro 231 della tua società | Guida la conversazione con gli strumenti `registro_*` del server MCP. |

Le skill informative non promettono la conformità, non danno pareri legali sul caso concreto e non usano gli strumenti senza il tuo consenso.

## Il server MCP e i suoi strumenti

Il server `zei231-registro` è remoto: `https://zei.services/api/mcp`. Gli strumenti principali:

- `registro_crea_gruppo` e `registro_aggiorna_profilo`: creano il tuo gruppo e salvano il profilo della società;
- `registro_valutazione_rischi` e `registro_mappatura_amministrazione`: propongono le attività del processo Amministrazione, le famiglie di reato e un protocollo per attività;
- `registro_apri_dashboard`: dà un link, valido 10 minuti e una volta, alla dashboard in ZEI;
- `registro_richiedi_preventivo`: invia la bozza al consulente partner CO.DE, solo dopo il tuo «sì» esplicito;
- per i gruppi invitati dal consulente: checklist dei documenti, schede di intervista, processi, attività sensibili, reati e protocolli (`registro_stato`, `registro_checklist`, `registro_intervista`, `registro_scrivi_reato` e gli altri `registro_*`).

La prima mappatura è un'analisi automatica. Non è un parere legale e non sostituisce la valutazione dei rischi del Modello.

## Cosa fa e cosa invia

- Le skill sono file di testo con istruzioni e un elenco. Non eseguono codice.
- Il plugin non ha hook, comandi, agenti, script locali né analytics.
- L'unico collegamento di rete è il server MCP `https://zei.services/api/mcp`. Si connette quando usi uno strumento `registro_*`.
- Al primo uso accedi a ZEI con OAuth 2.1 (PKCE) e la tua email. Il plugin non salva password né chiavi.
- Gli strumenti inviano a ZEI i dati che tu fornisci o confermi in chat (per esempio ragione sociale, partita IVA, codice ATECO, risposte della checklist).
- I dati sono conservati nell'UE, a Francoforte, tranne i fornitori elencati nell'informativa privacy.
- Il plugin non legge file del tuo computer e non apre altri indirizzi.
- Privacy: https://zei.services/privacy · Termini: https://zei.services/termini · Supporto: https://zei.services/supporto

## Installazione in Claude Code

```
/plugin marketplace add ZEI-231/zei231-plugin
/plugin install zei231-registro@zei231
```

Al primo uso di uno strumento accedi a ZEI. Guida: https://zei.services/guida

## English

This plugin bundles four skills and a remote MCP server for ZEI 231, a register for the Italian Modello 231 (Legislative Decree 231/2001). Three skills answer general questions (what the Model is, who must adopt it, offence families, the supervisory body, code of ethics, whistleblowing, structure) from legal texts checked against Normattiva on 09/10/2026, and say so when the law is silent or debated. The fourth skill starts or continues the company's register through the MCP tools. The plugin has no hooks, no local code and no analytics. It connects only to https://zei.services/api/mcp, after you sign in with OAuth. Data is stored in the EU (Frankfurt). Install in Claude Code with `/plugin marketplace add ZEI-231/zei231-plugin`. Guide: https://zei.services/guida

## Licenza

I file di questo plugin sono rilasciati con licenza MIT (vedi LICENSE). L'uso del servizio ZEI 231 è regolato dai termini di servizio: https://zei.services/termini
