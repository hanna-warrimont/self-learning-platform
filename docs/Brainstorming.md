# Self Learning Platform – Brainstorming

_Stand: 15.11.2026 · lebendes Dokument, wird laufend ergänzt_

## Entscheidungen
| Datum | Entscheidung |
|---|---|
| 15.11.2026 | Zielgruppe: **nur ich selbst** (kein öffentliches Produkt, kein Bezahlmodell) |
| 15.11.2026 | Level: **Fortgeschritten** |
| 15.11.2026 | Erster Kurs: **System Design** → Gliederung in `Kurs-System-Design.md` |
| 15.11.2026 | Coden: **lokal** (Starter-Repo, Tests lokal) |
| 15.11.2026 | Git-Repo angelegt, Konzeptdokumente liegen unter `docs/` |

## 1. Vision
Eine persönliche Lernumgebung, in der ich **durch eigenes Coden** lerne, ohne Videos. Jede Lektion endet mit einer Aufgabe, die automatisch geprüft wird.
- Kurse: **System Design** (zuerst), danach **FinTech Stack**
- Tech-Stack: **Nuxt** (Frontend) + **.NET** (Backend)

## 2. Lernformat (Kernidee)
- **Lektion** = kurzer Text + Diagramm → Aufgabe → automatisches Feedback
- **Projektbasiert:** Pro Kurs ein echtes System, das wächst (System Design: Ticketing-System)
- Aufgabentypen: Build · Break & Fix · Measure (Lasttests) · Decide (ADR schreiben)
- Hilfe in Stufen: Hinweis 1 → Hinweis 2 → Lösung (mit Erklärung)

## 3. Was „Plattform“ jetzt bedeutet
Weil sie nur für mich ist, kann der Start viel schlanker sein:
- **Stufe 1 – Kurs-Repo:** Monorepo mit Modulen (Markdown + Tests + Hinweise), `make check MODULE=03` führt die Prüfung aus. Lösungen auf eigenen Branches. Keine Web-App nötig.
- **Stufe 2 – Dashboard:** kleine Nuxt-App + .NET-API, die Lektionen anzeigt und Fortschritt speichert. Das CLI meldet Testergebnisse dorthin. Kein Login nötig (oder nur ein einfacher).
- **Stufe 3 – Extras:** KI-Feedback zu Code und ADRs, Statistiken, weitere Kurse.

## 4. Kursideen
### System Design (Kurs 1) → siehe `Kurs-System-Design.md`

### FinTech Stack (Kurs 2)
- Geldbeträge richtig behandeln (decimal, Währungen, Rundung)
- Ledger / doppelte Buchführung
- Idempotenz & Transaktionen
- Payment-Flow (Autorisierung, Capture, Refund)
- Reconciliation (Abgleich)
- Event Sourcing / Audit-Trail
- Open Banking / PSD2-APIs, KYC/AML-Grundlagen
- Sicherheit & Compliance-Basics

### Später denkbar
DevOps & CI/CD, Testing-Strategien, Cloud-Architektur

## 5. Technische Architektur (Dashboard, Stufe 2)
- **Frontend:** Nuxt 4, evtl. Nuxt Content für die Lektionen
- **Backend:** ASP.NET Core Web API, EF Core, PostgreSQL (oder SQLite für den Anfang)
- **Inhalte als Code:** Kurse liegen im Git-Repo
- **Datenmodell:** Course · Module · Exercise · Submission · Progress

## 6. Roadmap
1. **Phase 0:** Gliederung Kurs 1 ✅ (Entwurf), Git-Repo ✅
2. **Phase 1:** Kurs-Repo + Modul 0–2 schreiben und selbst durcharbeiten
3. **Phase 2:** Restliche Module, parallel kleines Dashboard
4. **Phase 3:** KI-Feedback, Kurs 2 (FinTech)

## 7. Offene Fragen
- Kursprojekt: Ticketing-System oder die Lernplattform selbst?
- Sprache der Inhalte: Deutsch oder Englisch?
- Wer schreibt die Inhalte? (Ich erstelle Modul-Texte, Aufgaben und Tests mit Claude, dann arbeite ich sie selbst durch → Spoiler-Problem: Lösungen getrennt halten)
- Zeitbudget pro Woche?
