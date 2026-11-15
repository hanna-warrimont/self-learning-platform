# CLAUDE.md

## Projekt
Persönliche Self-Learning-Plattform: Ich lerne durch eigenes Coden, ohne Videos. Jede Lektion endet mit einer Aufgabe, die automatisch geprüft wird.

- Zielgruppe: nur ich selbst, Level fortgeschritten
- Erster Kurs: System Design, Kursprojekt „TicketRush“ (Event-Ticketing-System)
- Coden: lokal, Prüfung mit `make check MODULE=XX`
- Stack: .NET (ASP.NET Core, EF Core), Nuxt 4, PostgreSQL, Redis, RabbitMQ, Docker Compose, xUnit + Testcontainers, k6

Konzept und Entscheidungen: `docs/Brainstorming.md`. Kursgliederung: `docs/Kurs-System-Design.md`.

## Regeln für Claude
- **Keine Spoiler:** Musterlösungen nie auf `main` und nie ungefragt zeigen. Lösungen kommen auf Branches `solution/XX`.
- Wenn ich an einer Aufgabe arbeite: Hinweise nur stufenweise (Hinweis 1 → 2 → 3), keine fertige Lösung, außer ich frage ausdrücklich.
- Jedes Modul unter `modules/XX-name/` mit `README.md` (Theorie), `TASK.md` (Aufgabe + Akzeptanzkriterien), `tests/`, `hints/`, `ADR.md` (Vorlage).
- Neue Entscheidungen in die Tabelle „Entscheidungen“ in `docs/Brainstorming.md` eintragen.
- Kommunikation auf Deutsch.

## Offene Fragen
Siehe Abschnitt „Offene Fragen“ in `docs/Brainstorming.md`.

## Agent skills

### Issue tracker

Issues liegen in GitHub Issues (hanna-warrimont/self-learning-platform), Zugriff über `gh`. See `docs/agents/issue-tracker.md`.

### Triage labels

Standard-Labels: needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix. See `docs/agents/triage-labels.md`.

### Harness

`/harness <Idee | #Issue>` führt Arbeit durch architect → developer → verifier → reviewer (`.claude/agents/`). Läufe landen in `.harness/runs/`.

### Domain docs

Single-context: `GLOSSARY.md` + `docs/adr/` im Repo-Root. See `docs/agents/domain.md`.
