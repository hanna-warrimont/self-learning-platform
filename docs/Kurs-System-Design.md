# Kurs 1: System Design (Fortgeschritten) – Gliederung

_Stand: 15.11.2026 · Entwurf_

## Kursprojekt: Event-Ticketing-System („TicketRush“)
Über den ganzen Kurs hinweg baust du **ein** System, das Schritt für Schritt wächst. Ticketing passt gut, weil es alle harten System-Design-Probleme enthält:
- sehr viele Lesezugriffe (Events ansehen) → Caching
- Konkurrenz um knappe Plätze (kein Doppelverkauf) → Konsistenz und Locking
- Reservierungen mit Ablaufzeit und Zahlung, die scheitern kann → Sagas und Kompensation
- Lastspitzen beim Flash Sale → Queues, Rate Limiting, Skalierung
- Bezahlung und Benachrichtigung → asynchrone Abläufe, Idempotenz

_Entschieden am 15.11.2026. Verworfene Alternative: die Lernplattform selbst als Kursprojekt (Runner, Queue, Fortschritt)._

## Aufbau jedes Moduls
```
modules/03-caching/
  README.md      # Theorie: Konzept, Diagramm, Trade-offs
  TASK.md        # Aufgabe + Akzeptanzkriterien
  tests/         # automatische Prüfung (xUnit, Testcontainers, k6)
  hints/         # Hinweis 1, 2, 3
  ADR.md         # Vorlage: deine Design-Entscheidung schriftlich begründen
```
- Lösung liegt auf einem eigenen Branch (`solution/03`).
- Abschluss = Tests grün **und** ADR geschrieben.

## Stack
.NET (ASP.NET Core, EF Core) · Nuxt 4 · PostgreSQL · Redis · RabbitMQ (MassTransit) · Docker Compose · xUnit + Testcontainers · k6 (Lasttests) · OpenTelemetry, Prometheus, Grafana (+ Tempo für Traces) · YARP oder nginx · Toxiproxy (Fehler simulieren)

## Module
| # | Thema | Was du baust | Wie es geprüft wird |
|---|---|---|---|
| 0 | Setup & Baseline | Solution, Docker Compose, einfache Events-API, Fake-Zahlungsdienst (Stub), OpenTelemetry-Grundsetup (Traces + Metriken in Grafana) | Smoke-Tests, Trace eines Requests sichtbar, erster k6-Lauf als Ausgangswert |
| 1 | Anforderungen & Kapazität | Grobe Rechnung (Nutzer, RPS, Speicher), SLOs, API-Vertrag (OpenAPI) inkl. `Idempotency-Key`-Header | Design-Doc + Contract-Tests |
| 2 | Daten & Konsistenz | Schema, Indizes, Transaktionen, Isolation Levels, optimistisches Locking, **Reservierung mit Ablaufzeit** (Hold → Freigabe abgelaufener Holds) | Test mit 500 parallelen Käufen: **kein Platz doppelt verkauft**; abgelaufene Reservierung wird wieder frei |
| 3 | Caching | Redis Cache-Aside, Invalidierung, Schutz vor Cache Stampede | k6: p95-Latenz besser als Baseline, Update innerhalb von X ms sichtbar |
| 4 | Asynchrone Verarbeitung | RabbitMQ, Outbox-Pattern, idempotente Consumer, Retries, Dead Letter Queue | Test: Nachrichten doppelt zugestellt → Wirkung nur einmal |
| 5 | Kaufablauf als Saga | Reservieren → Zahlen (Fake-Zahlungsdienst) → Bestätigen oder Kompensieren; Idempotency Keys an der API | Zahlung schlägt fehl → Platz wird freigegeben; gleicher Request mit gleichem Key zweimal → genau ein Kauf |
| 6 | Observability (vertieft) | Eigene Spans, Trace-Propagation über die Queue, Dashboards, SLO-Alerts | Break & Fix: gezielter Bug, Ursache allein über Traces finden |
| 7 | Resilienz | Timeouts, Retry, Circuit Breaker (Polly), Rate Limiting | Toxiproxy macht Zahlungsdienst langsam → API bleibt stabil |
| 8 | Horizontale Skalierung | Zustandslose API, mehrere Instanzen hinter Load Balancer; Decide: Wann braucht man verteilte Locks – und wann nicht? (inkl. Redlock-Kritik) | k6 mit 3 Instanzen, eine Instanz wird gestoppt → Fehlerquote im Budget |
| 9 | Warteraum & Echtzeit | Virtueller Warteraum im Backend (Token, Zulassungsrate), SignalR für Live-Verfügbarkeit, Nuxt-UI mit SSR + Caching | Lasttest: Zulassung entspricht der Rate, Reihenfolge fair; E2E-Test (Playwright) |
| 10 | Datenwachstum | CQRS-Lesemodell, Partitionierung, Read Replica; Replikationsverzögerung und Read-your-writes | Lasttest Lese-/Schreib-Mix; Test: eigener Kauf direkt nach dem Schreiben sichtbar |
| 11 | Capstone: Flash Sale | 10.000 Käufer in 60 Sekunden, Fairness, Überlastschutz | Alle SLOs erfüllt, abschließendes Design-Doc (inkl. Grenzen der lokalen Messung) |

## Aufgabentypen im Kurs
- **Build:** Feature umsetzen, bis die Tests grün sind
- **Break & Fix:** Ein vorbereiteter Bug oder Engpass muss gefunden werden
- **Measure:** Vorher/Nachher-Messung mit k6, Ergebnis im ADR festhalten
- **Decide:** Zwei Ansätze vergleichen (z. B. pessimistisches vs. optimistisches Locking) und begründen
