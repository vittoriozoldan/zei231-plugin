# ZEI 231 — Registro

Plugin per Claude che collega l'assistente a ZEI 231 (https://zei.services), il gestionale del Modello di organizzazione, gestione e controllo previsto dal D.Lgs. 231/2001.

In conversazione l'assistente:

1. prepara la **diagnosi preliminare 231** della società: famiglie di reato più vicine all'attività, sanzioni, numeri ufficiali e casi reali, ciascuno con la fonte;
2. mappa con il cliente il processo **«Amministrativo e affari generali»**: le attività sensibili svolte, i reati configurabili e i protocolli consigliati dal catalogo di CO.DE, il consulente partner di ZEI;
3. dopo un sì esplicito, invia la **richiesta di implementazione** del Modello a CO.DE.

Per i gruppi invitati, lo stesso connettore compila il registro completo: checklist dei documenti, schede di intervista, processi, attività sensibili, reati e protocolli.

La diagnosi non è un parere legale e non sostituisce la valutazione dei rischi del Modello.

## Installazione

In Claude Code:

```
/plugin marketplace add vittoriozoldan/zei231-plugin
/plugin install zei231-registro@zei231
```

In Claude (web e desktop) e in ChatGPT si aggiunge come connettore con l'indirizzo `https://zei.services/api/mcp`. Al primo uso si accede a ZEI con la propria email. Guida completa: https://zei.services/guida

## English

ZEI 231 connects Claude to an Italian compliance register for the Modello 231 (Legislative Decree 231/2001). It gives a company a sourced preliminary diagnosis, maps its administrative process with the applicable offences and recommended controls, and, after explicit consent, sends a request for the full Model to the partner consultancy CO.DE. Invited groups can maintain their full register. Privacy: https://zei.services/privacy · Terms: https://zei.services/termini · Support: https://zei.services/supporto

## Licenza

I file di questo plugin sono rilasciati con licenza MIT (vedi LICENSE). L'uso del servizio ZEI 231 è regolato dai termini di servizio: https://zei.services/termini
