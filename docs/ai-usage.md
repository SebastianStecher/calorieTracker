# AI-Einsatz und Reflexion

AI wurde in Phase 1 an vier Stellen eingesetzt. Zwei davon sind die vorgeschriebenen, unterschiedlichen Anwendungen im Requirements Engineering (Generierung und Prüfung). Prompts und unveränderte Ausgaben liegen im Ordner [ai-log](ai-log/), damit nachvollziehbar ist, was von der AI stammt und was vom Team.

| # | Zweck | Modell | Ergebnis | Log |
|---|---|---|---|---|
| 0 | Projektidee analysieren, Risiken finden | Claude Opus 5.5 (claude.ai) | Risikoliste, die wir für Anforderungen und MVP genutzt haben | – (Gesprächsnotizen) |
| 1 | **User Stories generieren** | Claude Opus 5.5 (claude.ai) | 19 Stories: 17 übernommen bzw. überarbeitet, 2 gestrichen; 2 eigene ergänzt | [01](ai-log/01-user-stories-generierung.md) |
| 2 | **Anforderungen auf Widersprüche, Lücken, Unklarheiten prüfen** | Claude Opus 5.5 (Claude, Agent-Sitzung) | 14 Funde: 10 übernommen, 2 teilweise, 2 verworfen | [02](ai-log/02-anforderungen-pruefen.md) |
| 3 | Unterstützung beim Ausformulieren von requirements.md, mvp.md und dieses Dokuments | Claude Opus 5.5 | Textentwürfe auf Basis der Team-Entscheidungen; vom Team gelesen und angepasst | – |

## Einsatz 0: Analyse der Projektidee

Bevor User Stories entstanden, haben wir die grobe Idee („Kalorien-App mit KI") mit einer AI durchgesprochen. Die AI hat dabei vor allem Fragen gestellt und Risiken benannt. Übernommen haben wir:

- **Scope-Warnung:** Die Idee besteht eigentlich aus vier Feature-Blöcken (KI-Erfassung, KI-Vorschlag, Datenbank mit eigenen Lebensmitteln, Basisfunktionen). Nicht alles kann Must sein.
- **Testbarkeit:** „KI erkennt richtig" ist nicht prüfbar. Prüfbar ist, dass der Nutzer jeden Wert vor dem Speichern korrigieren kann. Daraus wurde US-06 die zentrale Must-Story.
- **Rechnen statt Schätzen:** Beim Mahlzeitvorschlag wählt die KI nur Lebensmittel und Mengen, die App rechnet die kcal. Erst dadurch wird ein Kriterium wie „±50 kcal" überhaupt messbar.
- **Backend nötig** (API-Schlüssel darf nicht in die App), **Kosten pro Bild**, **Datenschutz** bei Fotos und Ernährungsdaten. Daraus wurden NFR-01 bis NFR-03.

Die Festlegung der **Zielgruppe** (Personen, die schon tracken, keine Einsteiger) und die Idee, gewogene Mengen per Text einzubringen, kamen vom Team.

## Einsatz 1: User Stories generieren

**Prompt (Kurzfassung, vollständig im Log):** Rolle Requirements Engineer; Produktidee mit den vier Kernfunktionen, Zielgruppe und Rahmen (3 Personen, ein Semester); Vorgaben zu Format, IDs, Prioritätsvorschlag und 2–4 überprüfbaren Akzeptanzkriterien pro Must-Story.

**Ergebnis:** 19 sauber formatierte User Stories, davon 8 als Must.

**Was gut war:**
- Vollständige Abdeckung der genannten Funktionen und sinnvolle Gliederung. Als Ausgangspunkt hat das viel Zeit gespart.
- Einige Kriterien waren direkt brauchbar (z. B. US-06: „Erst nach Bestätigung wird der Eintrag gespeichert").

**Was wir korrigieren mussten:**
- **Zu viele Musts.** Die AI hat praktisch jede Funktion, die im Prompt vorkam, als Must eingestuft, auch den Mahlzeitvorschlag. Sie kann nicht beurteilen, was für *unseren* Kernanwendungsfall unverzichtbar ist. Mit dem Weglass-Test haben wir auf 7 Musts reduziert. Zwei Stories haben wir danach bewusst wieder auf Must gesetzt (Barcode wegen der Zielgruppe, Konto löschen wegen DSGVO), jeweils mit eigener Begründung. Das ist der Unterschied zur AI-Priorisierung: Auch unsere Ausnahmen sind begründet, nicht bloß übernommen.
- **Nicht prüfbare Kriterien,** obwohl im Prompt ausdrücklich „überprüfbar" verlangt war: „nahe am offenen Budget" (US-14), „verständliche Meldung" (US-03).
- **Versteckte Widersprüche:** Öffentliche Lebensmittel standen als Could in US-11, gleichzeitig aber in den Must-Kriterien von US-08 und in der Should-Story US-10.
- **Scope Creep:** Gewichtsverlauf und Erinnerungen (US-17, US-18) wurden ohne Bezug zum Problem ergänzt, als „weitere Ideen".
- **Lücken:** Gespeicherte Einträge löschen und das Verhalten bei Ausfall des KI-Dienstes fehlten.

Die Entscheidung zu jeder einzelnen Story steht im Log unter „Bewertung durch das Team".

## Einsatz 2: Anforderungen prüfen

**Prompt (Kurzfassung):** Rolle kritischer Reviewer; Eingabe README und erste vollständige Fassung von requirements.md; Prüfung auf Widersprüche, Lücken, Unklarheiten und Priorisierung; pro Fund betroffene IDs, Vorschlag und Schweregrad; ausdrücklich „Erfinde keine Probleme".

**Ergebnis:** 14 Funde.

**Übernommen (Auswahl der wichtigsten):**
- **Begriff „Tag" widerspricht US-13** (Nachtragen unmöglich, wenn ein Eintrag immer zum Speichertag gehört). Diesen Fehler hatten wir beim Schreiben selbst eingebaut und nicht bemerkt.
- **Must-Kriterien hängen an einem nicht-deterministischen Modell.** Ein Testlauf von US-04 kann zufällig bestehen oder scheitern. Wir haben in FR-05 eine Testserie mit Schwelle festgelegt (5 Testfälle × 10 Durchläufe, mind. 9 bestanden). Das ist für uns die wichtigste Erkenntnis aus Phase 1, weil sie direkt beeinflusst, wie wir in Phase 2 testen.
- **„Häufig gegessen" ist für KI-Positionen nicht definierbar,** weil diese keine Datenbank-ID haben. Gelöst durch eine eigene Begriffsdefinition und ein neues Kriterium in US-06 (KI-Position durch Datenbankeintrag ersetzen).
- **Einheitenproblem g/ml** in US-04, fehlende Story zu „Konto löschen", Must-Stories mit Verweisen auf Should-Stories.

**Verworfen bzw. nur teilweise übernommen:**
- **Fund 11 (zwei Zeitgrenzen 15 s / 30 s):** Kein Widerspruch, sondern Zielwert und harte Grenze. Die AI hat hier Formulierung und Inhalt verwechselt; wir haben nur die Formulierung geschärft.
- **Fund 12 (Passwort zurücksetzen):** Berechtigt, aber für einen Prototyp mit Testnutzern nicht nötig → Won't.
- **Fund 13 (Barrierefreiheit, Offline):** Typischer Vollständigkeits-Vorschlag. Offline haben wir als *Nicht*-Unterstützung festgeschrieben (NFR-08), Barrierefreiheit verworfen.
- **Fund 14 (Grenzen 800–6000 kcal):** Grenzen bewusst beibehalten, Begründung ergänzt.

## Reflexion

**Wo AI geholfen hat.** Am meisten beim *Prüfen*, nicht beim *Erzeugen*. Die generierten Stories hätten wir mit etwas mehr Zeit auch selbst geschrieben. Die Prüfung hat aber Fehler gefunden, die über mehrere Abschnitte verteilt sind (Begriffe ↔ Stories ↔ FR) und beim Schreiben einzelner Abschnitte nicht auffallen. Der Hinweis auf den Nicht-Determinismus des Modells hat außerdem unser Testverständnis für Phase 2 verändert.

**Wo AI nicht helfen konnte.** Alle Entscheidungen, die vom Ziel des Produkts abhängen, mussten wir treffen:
- welche Zielgruppe wir bedienen (und welche nicht),
- was der Kernanwendungsfall ist und damit, was Must ist,
- ob der Mahlzeitvorschlag Must oder Should ist ([mvp.md](mvp.md#entscheidung-mahlzeitvorschlag)),
- welche Prüf-Funde relevant sind. Etwa ein Drittel der Review-Vorschläge hätte das Dokument größer, aber nicht besser gemacht.

**Muster, die uns aufgefallen sind.**
1. Die AI priorisiert nach *Erwähnung im Prompt*, nicht nach *Bedeutung für das Problem*. Was im Prompt steht, wird eher Must.
2. Die AI ergänzt gerne Funktionen, die „typisch" für die Produktkategorie sind (Gewicht, Erinnerungen), statt bei unserem Problem zu bleiben.
3. Auch wenn der Prompt „überprüfbar" verlangt, entstehen weiche Formulierungen. Akzeptanzkriterien müssen einzeln gegengelesen werden.
4. Beim Review schlägt die AI eher zu viel als zu wenig vor. Der Hinweis „Erfinde keine Probleme" hat geholfen, aber nicht alle Scheinprobleme verhindert (Fund 11).

**Grenzen unseres Vorgehens.** Generierung und Prüfung liefen mit derselben Modellfamilie. Ein anderes Modell hätte womöglich andere blinde Flecken gefunden. Außerdem kann die AI fachlich falsche Aussagen sehr überzeugend formulieren. Wir haben deshalb jede übernommene Änderung einzeln begründet (siehe Bewertungstabellen in den Logs), statt Vorschläge gesammelt zu übernehmen.

**Fazit.** AI kann Anforderungen erzeugen und prüfen, aber nicht entscheiden, was unser Produkt sein soll. Den größten Wert hatte sie als kritischer zweiter Blick auf bereits durchdachte Anforderungen. Verantwortlich für die Anforderungen ist das Team.
