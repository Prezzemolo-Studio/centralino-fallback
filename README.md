# Centralino — rete di sicurezza

Due file XML statici. Esistono per un caso solo: **quando n8n non risponde.**

Il centralino d'agenzia vive su n8n (`automazioni/n8n/centralino/` nel repo `agenzia`). Se quel servizio è giù, lento o irraggiungibile, Telnyx abbandona la richiesta e chiama il `voice_fallback_url` dell'applicazione TeXML, che punta qui.

| File | Cosa fa |
|---|---|
| `fallback.xml` | annuncia che non possiamo rispondere e registra un messaggio |
| `chiudi.xml` | saluta e chiude, ed è l'`action` del `<Record>` |

## Perché due file e non uno

Un `<Record>` senza `action` fa ri-richiedere al fornitore il documento corrente: l'annuncio ripartirebbe da capo, all'infinito, finché il chiamante non riaggancia. Il secondo file esiste per chiudere la chiamata in modo esplicito, senza dipendere da un comportamento che la documentazione Telnyx non dichiara.

## Perché sta su GitHub e non sulla nostra VPS

La rete di sicurezza non può vivere sulla macchina che deve proteggere. n8n gira su una VPS: un file di riserva ospitato lì cadrebbe insieme a ciò che dovrebbe coprire.

## Perché non ci sono numeri di telefono

Il fallback **non fa squillare nessuno**, registra e basta. Questo repo è pubblico, e qualunque cosa scritta qui dentro è leggibile da chiunque: i cellulari dei soci restano nelle Data Table di n8n, che è il loro posto.

Il prezzo è che, durante un disservizio, il telefono non suona. La chiamata però non si perde, e quella è la proprietà che conta.

## Cosa succede al messaggio

La registrazione resta su Telnyx: la notifica la manda n8n, che in quel momento è giù. **Quando n8n torna su, vanno controllate le registrazioni orfane.**

## Come si modifica

Un commit su `main`. GitHub Pages ripubblica da solo in un minuto. Dopo ogni modifica, riascoltare il messaggio chiamando il numero con n8n spento: è l'unico modo di sapere se funziona.
