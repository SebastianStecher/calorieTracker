# KI-Kalorientracker (Arbeitstitel)

Projekt im Rahmen der LV *Principles of AI-based Software Engineering*, FH Technikum Wien.
Team: 3 Personen · Plattform: Mobile App (Flutter) · Stand: Phase 1 – Anforderungsanalyse

## Projektidee

Eine Kalorien-Tracking-App für Menschen, die bereits regelmäßig tracken und dabei Zeit sparen wollen, ohne an Genauigkeit zu verlieren. Die App kombiniert zwei KI-Funktionen:

1. **KI-gestützte Erfassung:** Nutzer fotografieren ihre Mahlzeit und können zusätzlich Text eingeben, z. B. „150 g Reis gewogen, 1 EL Olivenöl". Die KI erkennt die Lebensmittel, schätzt Mengen und Nährwerte und berücksichtigt dabei die Textangaben. Das Ergebnis wird vor dem Speichern angezeigt und kann korrigiert werden.
2. **KI-Mahlzeitvorschlag für das Restbudget:** Nutzer fragen z. B. am Abend „Ich habe noch 700 kcal offen – was kann ich kochen?". Die App schlägt eine Mahlzeit vor, die zum offenen Budget passt. Grundlage sind Lebensmittel, die der Nutzer häufig isst, oder Lebensmittel, die er gerade zu Hause hat.

Die Nährwertdaten stammen aus einer externen Lebensmitteldatenbank. Nutzer können sie um eigene Lebensmittel (mit oder ohne Barcode) erweitern, die sie entweder privat oder für alle Nutzer sichtbar anlegen.

## Problemstellung

Bestehende Tracking-Apps (z. B. MyFitnessPal, Yazio) setzen auf manuelle Suche in einer Datenbank. Für regelmäßige Nutzer entstehen dabei zwei wiederkehrende Probleme:

- **Hoher Erfassungsaufwand:** Jede Zutat muss einzeln gesucht, der passende Eintrag gewählt und die Menge eingegeben werden. Bei zusammengesetzten Gerichten (z. B. Bowl, Pfanne, Restaurantessen) dauert das mehrere Minuten pro Mahlzeit. Reine Foto-Erkennung ist zwar schneller, aber ungenau, und gewogene Mengen lassen sich meist nicht einbringen.
- **Planung mit dem Restbudget:** Gegen Ende des Tages ist oft unklar, welche Mahlzeit noch in das verbleibende Kalorienbudget passt. Nutzer probieren dann Kombinationen im Kopf oder in der App durch, was aufwendig ist und oft zu Über- oder Unterschreitung des Ziels führt.

## Zielgruppe

**Primär:** Personen, die bereits Kalorien tracken bzw. Erfahrung damit haben, z. B. Kraftsportler in Diät- oder Aufbauphasen oder gesundheitsbewusste Personen mit einem festen Kalorienziel.

Typische Eigenschaften und Bedürfnisse:
- kennen Kalorien- und Makronährstoffziele und tracken täglich
- wiegen Lebensmittel teilweise ab und erwarten, dass diese Angaben exakt übernommen werden
- wollen schnell erfassen, aber die Kontrolle über die Werte behalten
- essen häufig ähnliche Lebensmittel und planen Mahlzeiten rund um ihr Tagesziel

**Nicht Zielgruppe:** Tracking-Einsteiger, die erst an Kalorienzählen herangeführt werden müssen, sowie Personen, die medizinische Ernährungsberatung benötigen.

## Wichtigste Anwendungsfälle

| # | Anwendungsfall | Ablauf (kurz) | Im MVP |
|---|---|---|---|
| UC-1 | **Mahlzeit per Foto und Text erfassen** | Foto aufnehmen, gewogene Mengen als Text ergänzen, KI-Ergebnis prüfen und korrigieren, speichern | ja (Kern) |
| UC-2 | **Tagesbilanz prüfen** | verbrauchte und offene kcal des Tages ansehen | ja |
| UC-3 | **Lebensmittel manuell oder per Barcode erfassen** | Lebensmittel suchen oder Barcode einer Packung scannen, Menge eingeben, hinzufügen; Ausweg, wenn die KI etwas nicht oder falsch erkennt | ja |
| UC-4 | **Mahlzeit für das Restbudget finden** | Vorschlag aus häufig gegessenen Lebensmitteln anfordern, übernehmen | Ausbaustufe (Should) |
| UC-5 | **Fehlendes Lebensmittel anlegen** | eigenes Lebensmittel mit Nährwerten anlegen, privat oder öffentlich | Ausbaustufe (Should/Could) |

## Projektziel

Bis zum Ende der LV entsteht ein **funktionsfähiger Prototyp** als Flutter-App, der mindestens einen vollständigen Anwendungsfall demonstrierbar abdeckt: Eine Mahlzeit wird per Foto und Text mit KI erfasst, geprüft, gespeichert und in der Tagesbilanz angezeigt (UC-1 bis UC-3). Der KI-Mahlzeitvorschlag (UC-4) ist die erste Ausbaustufe nach dem MVP; die Begründung steht in [docs/mvp.md](docs/mvp.md#entscheidung-mahlzeitvorschlag).

## Rahmenbedingungen und Abhängigkeiten

- **Team:** 3 Personen, Umsetzung innerhalb eines Semesters
- **Plattform:** Mobile App mit Flutter
- **Externe Dienste:**
  - Lebensmitteldatenbank (z. B. Open Food Facts, kostenlos)
  - KI-Modell mit Bildverarbeitung (kostenpflichtig pro Anfrage)
- **Backend:** Wird für Benutzerkonten, öffentlich geteilte Lebensmittel und die sichere Anbindung des KI-Dienstes benötigt. Der API-Schlüssel darf nicht in der App liegen.
- **Abgrenzung:** Die App ist kein Medizinprodukt und gibt keine Ernährungs- oder Gesundheitsberatung. KI-Werte sind Schätzungen und werden als solche gekennzeichnet.

## Dokumentation

| Dokument | Inhalt |
|---|---|
| [docs/requirements.md](docs/requirements.md) | User Stories, funktionale und nichtfunktionale Anforderungen, Priorisierung, Akzeptanzkriterien |
| [docs/mvp.md](docs/mvp.md) | MVP-Scope, enthaltene und ausgeschlossene Funktionen |
| [docs/ai-usage.md](docs/ai-usage.md) | Einsatz von KI im Requirements Engineering, Prompts, Ergebnisse, kritische Bewertung |
| [docs/ai-log/](docs/ai-log/) | Vollständige Prompts und unveränderte AI-Ausgaben mit Bewertung durch das Team |
