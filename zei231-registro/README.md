# ZEI 231 — Registro

Plugin per Claude che collega l'assistente a ZEI 231 (https://zei.services), il gestionale del Modello di organizzazione, gestione e controllo previsto dal D.Lgs. 231/2001.

In conversazione l'assistente:

1. alla frase «voglio creare un modello 231» crea subito il tuo **gruppo** su ZEI e compila il **profilo** della società con quello che sa già;
2. avvia la **valutazione dei rischi** del processo **Amministrazione**: le attività che la società svolge, le famiglie di reato della mappatura di CO.DE, il consulente partner di ZEI, e un protocollo proposto per attività, con il lavoro che resta;
3. apre la **dashboard in ZEI**, da cui chiedi a CO.DE il preventivo per completare il Modello.

Ogni passo si vede in un riquadro di ZEI nella chat.

L'assistente ti dà sempre del tu.

Per i gruppi invitati, lo stesso connettore compila il registro completo: checklist dei documenti, schede di intervista, processi, attività sensibili, reati e protocolli.

La prima mappatura non è un parere legale e non sostituisce la valutazione dei rischi del Modello, che fa il consulente.

## Installazione

In Claude Code:

```
/plugin marketplace add vittoriozoldan/zei231-plugin
/plugin install zei231-registro@zei231
```

In Claude (web e desktop) e in ChatGPT si aggiunge come connettore con l'indirizzo `https://zei.services/api/mcp`. Al primo uso si accede a ZEI con la propria email. Guida completa: https://zei.services/guida

## English

ZEI 231 connects Claude to an Italian compliance register for the Modello 231 (Legislative Decree 231/2001). On «voglio creare un modello 231» it creates the company's group at once, fills its profile, maps its administrative process with the offence families and one proposed control per activity, opens the ZEI dashboard, and, after explicit consent, sends a request for the full Model to the partner consultancy CO.DE. Invited groups can maintain their full register. Privacy: https://zei.services/privacy · Terms: https://zei.services/termini · Support: https://zei.services/supporto

## Licenza

I file di questo plugin sono rilasciati con licenza MIT (vedi LICENSE). L'uso del servizio ZEI 231 è regolato dai termini di servizio: https://zei.services/termini
