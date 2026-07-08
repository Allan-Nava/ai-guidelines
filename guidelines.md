# Project Guidelines

Linee guida complete per gestire un progetto software in modo scalabile, leggibile e operabile.

Base pratica: principi ricavati anche da workflow documentati nelle repository `devops_hiway` (runbook, incident, reporting operativo) e `compress-bot` (chatops, ownership, affidabilita, backlog idempotente).

Questo documento incorpora pratiche emerse da contesti reali orientati a operations e affidabilita, con particolare attenzione a:

- repository come fonte di verita,
- standardizzazione di processi e output,
- qualita del delivery,
- gestione incident e miglioramento continuo.

## 1. Principi Fondativi

1. La repository e il sistema ufficiale di lavoro: codice, documentazione, decisioni e runbook devono vivere insieme.
2. Ogni cambiamento rilevante deve lasciare traccia: cosa e stato fatto, perche, come validarlo.
3. Automatizzare dove possibile, ma rendere sempre verificabile manualmente ogni passaggio critico.
4. Separare chiaramente fatti, ipotesi e decisioni.
5. Green build non equivale automaticamente a sistema sano: verificare sempre il comportamento reale.

## 2. Struttura Minima Del Repository

Struttura consigliata:

```text
/
|- README.md
|- CHANGELOG.md
|- guidelines.md
|- CLAUDE.md
|- docs/
|  |- architecture/
|  |- runbooks/
|  |- incidents/
|  |- reports/
|  |- decisions/
|- src/
|- test/
|- ci/
```

Regole:

- `README.md`: onboarding rapido e mappa repository.
- `CHANGELOG.md`: Keep a Changelog + SemVer.
- `TODO.md`: backlog operativa unica e tracciata di tutto il lavoro.
- `docs/runbooks/`: procedure operative pianificate.
- `docs/incidents/`: post-mortem datati (`YYYY-MM-DD-slug.md`).
- `docs/reports/`: output periodici o ad-hoc (health-check, audit, verifiche).
- `docs/decisions/`: ADR o note decisionali tecniche.

## 3. Governance E Ownership

1. Ogni area deve avere owner esplicito (team o persona).
2. Ogni servizio deve avere:
	- owner tecnico,
	- canale alert,
	- runbook di triage,
	- check di salute.
3. Le responsabilita devono essere documentate, non implicite.
4. TODO e debito tecnico in una sola backlog di progetto, con ID stabili.

## 4. Convenzioni Di Sviluppo

### 4.1 Utilities

- Shared helpers in moduli dedicati.
- Utility piccole e focalizzate, evitare classi omnibus.
- No duplicazione di logica generica.

### 4.2 Constants

- Vietate stringhe di dominio hardcoded se esiste una costante.
- Literals centralizzati in un modulo unico.
- Confronti su valori noti via costanti/enumerazioni.

### 4.3 DTO E Contratti API

- DTO nei package/moduli API dedicati.
- No DTO pubblici annidati dentro servizi.
- Mapping esplicito ai boundary (handler/service).

### 4.4 Layering

- Handler/API dipendono da servizi, non da repository.
- Business logic nei servizi/dominio.
- Handler sottili: parsing, validazione, mapping response, error handling.

### 4.5 Dipendenze

- Dipendenze obbligatorie non nullable.
- Vietati fallback silenziosi che saltano persistenza o integrazioni critiche.
- Le dipendenze opzionali devono essere modellate esplicitamente.

### 4.6 Cache E Stato Runtime

- Definire ownership di ogni cache.
- Separare cache di configurazione da stato runtime.
- Definire invalidazione/refresh espliciti.
- Rendere evidente se uno stato e locale nodo o cluster-wide.

### 4.7 Unita E Conversioni

- Non mescolare unita (ms/s, bytes/MB, UTC/localtime).
- Conversioni ai boundary con test dedicati.

## 5. Testing E Quality Gates

1. Ogni bug fix deve includere almeno un test che avrebbe catturato il bug.
2. Ogni nuova feature deve avere:
	- test unitari della logica,
	- test integrazione nei boundary critici,
	- smoke test end-to-end se coinvolge workflow principali.
3. I test devono coprire sia path nominali sia error path.
4. Definire esplicitamente cosa e considerato warning vs failure.
5. Inserire gate minimi in CI:
	- lint,
	- test,
	- build,
	- security scan base.

## 6. CI/CD E Rilascio

### 6.1 Strategia Di Deploy

Pipeline consigliata per modifiche infrastrutturali o ad alto rischio:

1. Preflight: baseline dello stato corrente.
2. Dry-run: verifica differenze attese.
3. Canary: rollout su 1 nodo/istanza.
4. Rolling: rollout progressivo.
5. Post-check: validazione completa.
6. Smoke test: verifica funzionale reale.
7. Chiusura: report + changelog + follow-up.

### 6.2 Regole Operative

- Ogni release deve avere note di rilascio in changelog.
- Taggare release in SemVer quando applicabile.
- Rollback strategy obbligatoria per cambiamenti irreversibili.
- Evitare deployment opachi: i passaggi devono essere ricostruibili.

## 7. Documentazione Operativa

### 7.1 Standard Documentazione

Ogni intervento significativo deve produrre un documento in `docs/` con:

1. Contesto.
2. Obiettivo.
3. Azioni eseguite.
4. Evidenze (log/output principali).
5. Esito.
6. Rischi residui e follow-up.

### 7.2 Runbook

Un runbook deve includere:

- prerequisiti,
- comandi step-by-step,
- criteri di successo,
- rollback,
- troubleshooting rapido.

### 7.3 Incident Post-Mortem

Formato minimo consigliato:

1. Data e severita.
2. Sintomo osservato.
3. Impatto.
4. Root cause.
5. Timeline.
6. Mitigazione immediata.
7. Azioni preventive.
8. Link a evidenze/log.

## 8. Osservabilita E Monitoring

1. Ogni servizio deve avere health-check ripetibile.
2. Gli script di check devono avere interfaccia coerente:
	- `--list` per target,
	- `--mode` per categoria check,
	- codici di uscita documentati.
3. Distinguere chiaramente:
	- errore sistemico del checker,
	- warning operativo,
	- failure reale del servizio.
4. Archiviare report periodici con naming datato.
5. Introdurre metriche di affidabilita (uptime, MTTR, MTTA) dove utile.

## 9. Sicurezza E Segreti

1. Mai committare segreti o credenziali.
2. Token/chiavi solo via secret manager o variabili protette.
3. Sanitizzare log e report da dati sensibili.
4. Minimo privilegio per ogni integrazione.
5. Documentare rotazione credenziali e procedure di emergenza.

## 10. Gestione Backlog

1. Una sola fonte di verita per backlog (file o board con ID stabili).
2. Classificare ogni item (bug, feature, hardening, tech debt).
3. Definire priorita, owner, milestone.
4. Tenere separati lavoro pianificato e finding operativi.
5. Mantenere sync idempotente se esiste automazione issue.

### 10.1 Regola Operativa Obbligatoria: TODO E Tracciamento Totale

1. Tutto il lavoro deve essere tracciato in `TODO.md` con stato aggiornato.
2. Ogni task deve avere almeno: ID, descrizione, stato, ultima modifica, evidenza/link.
3. Nessuna attivita fuori traccia (no task in chat senza registrazione nel backlog).
4. Ogni modifica ai contenuti deve aggiornare anche `CHANGELOG.md`.
5. E vietato eseguire push automatici da agent o automazioni non richieste esplicitamente dall'utente.
6. Regola permanente: tracciare sempre tutto, non pushare mai senza istruzione esplicita.

## 11. Processo Decisionale

1. Decisioni architetturali importanti devono essere registrate (ADR).
2. Ogni ADR deve esplicitare:
	- contesto,
	- opzioni valutate,
	- decisione,
	- conseguenze.
3. Le decisioni obsolete vanno segnate come superseded, non cancellate.

## 12. Onboarding E Collaborazione

Checklist onboarding minima:

1. Leggere `README.md`, `guidelines.md`, `CLAUDE.md`.
2. Configurare ambiente locale.
3. Eseguire test e lint.
4. Simulare un flusso standard (modifica + test + changelog).
5. Leggere almeno un incident e un runbook recenti.

Regole collaborazione:

- PR piccole e focused.
- Commit con messaggi chiari e intent-based.
- Review con focus su rischi, regressioni e verificabilita.

## 13. Definition Of Done

Una modifica e considerata completa solo se:

1. codice aggiornato,
2. test pertinenti aggiornati/verdi,
3. documentazione aggiornata,
4. changelog aggiornato,
5. rischi e rollback chiariti,
6. evidenze di validazione disponibili.

## 14. Anti-Pattern Da Evitare

- Documentazione disallineata rispetto al codice.
- Decisioni critiche solo in chat, non tracciate.
- Hotfix non documentati.
- Fallback silenziosi che nascondono errori reali.
- Monitoring che segnala solo stati nominali senza contesto.

## 15. Template Rapidi

### 15.1 Template Report Operativo

```md
# <Titolo>

## Contesto

## Obiettivo

## Azioni Eseguite

## Evidenze

## Esito

## Follow-up
```

### 15.2 Template Incident

```md
# YYYY-MM-DD - <slug>

## Sintomo

## Impatto

## Root Cause

## Timeline

## Mitigazione

## Azioni Preventive
```

### 15.3 Template ADR

```md
# ADR-<numero> - <titolo>

## Contesto

## Opzioni

## Decisione

## Conseguenze
```

## 16. Regola Finale

Preferire sempre chiarezza, tracciabilita e ripetibilita alla velocita apparente.

Quando un cambiamento non e spiegabile e verificabile da chi non era presente, il processo non e ancora maturo.
