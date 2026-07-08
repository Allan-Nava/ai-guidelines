# CLAUDE.md

Istruzioni operative per agent AI (Claude/Copilot o equivalenti) che lavorano su questa repository.

## Missione

Mantenere la repo come riferimento affidabile e aggiornato sulle best practice di gestione progetto.

Ogni contributo deve migliorare almeno uno di questi aspetti:

- chiarezza delle linee guida,
- qualita operativa,
- tracciabilita delle decisioni,
- coerenza tra documenti.

## Regole Obbligatorie

1. Aggiornare sempre documentazione correlata quando si modifica contenuto sostanziale.
2. Aggiornare sempre `CHANGELOG.md` per modifiche rilevanti.
3. Non introdurre contenuti non verificabili o non motivati.
4. Non inserire segreti, token o credenziali in file versionati.
5. Non creare duplicati: preferire estendere sezioni esistenti.
6. Tracciare sempre tutto in `TODO.md` con stato e evidenze aggiornate.
7. Non pushare mai (ne manualmente ne via automazioni) senza richiesta esplicita dell'utente.

## Qualita Del Contenuto

Ogni modifica deve essere:

- specifica (evitare frasi vaghe),
- azionabile (indicazioni applicabili),
- misurabile (criteri o checklist),
- consistente con il resto della repo.

## Stile Editoriale

1. Scrittura tecnica chiara, orientata all'azione.
2. Titoli descrittivi e sezioni corte.
3. Liste con punti concreti.
4. Evitare buzzword senza pratica operativa.
5. Esplicitare trade-off quando presenti.

## Priorita Quando Si Propongono Miglioramenti

1. Definition of Done, quality gate, test strategy.
2. Incident management, runbook, rollback.
3. Changelog, release process, ownership.
4. Sicurezza e gestione segreti.
5. Onboarding e mantenibilita della documentazione.

## Checklist Pre-Consegna

Prima di considerare completata una modifica:

1. Verifica coerenza tra `README.md`, `guidelines.md`, `CLAUDE.md`.
2. Aggiorna `CHANGELOG.md` in `Unreleased`.
3. Controlla che esempi/template siano validi e riusabili.
4. Riduci ridondanze e frasi ripetitive.
5. Conferma che il contributo sia utile a un team reale, non solo teorico.

## Non Fare

- Non riscrivere l'intero documento senza bisogno.
- Non cambiare tono o struttura senza motivo.
- Non rimuovere regole esistenti senza sostituzione equivalente.
- Non aggiungere policy in conflitto con quanto gia definito.

## Output Atteso Da Un Agente

Quando applicabile, l'agente deve consegnare:

1. modifica ai file interessati,
2. aggiornamento changelog,
3. breve sintesi delle scelte fatte,
4. eventuali gap rimasti e prossimi passi suggeriti.
5. aggiornamento di `TODO.md` con stato dei task toccati.
