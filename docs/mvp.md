# MVP-Scope

Dieses Dokument legt fest, welcher Funktionsumfang im Semester als funktionsfähiger Prototyp umgesetzt wird und was bewusst nicht. Die einzelnen Anforderungen und Akzeptanzkriterien stehen in [requirements.md](requirements.md).

## Kernanwendungsfall

Das MVP deckt **einen vollständigen, demonstrierbaren Anwendungsfall** ab: die KI-gestützte Erfassung einer Mahlzeit bis zur aktualisierten Tagesbilanz.

**Demo-Szenario:**

1. Nutzer registriert sich, meldet sich an und legt ein Tagesziel von 2200 kcal fest. *(US-01, US-02)*
2. Er fotografiert eine Reis-Bowl und gibt dazu ein: „150 g Reis gewogen, + 1 EL Olivenöl". *(US-03, US-04)*
3. Die App zeigt die erkannten Positionen als KI-Schätzung. Reis steht mit genau 150 g und als „vom Nutzer angegeben" in der Liste, Olivenöl ist ergänzt. *(US-04)*
4. Der Nutzer korrigiert die geschätzte Menge Hähnchen von 120 g auf 100 g und fügt über die Suche eine Sauce hinzu, die die KI nicht erkannt hat. Die Summe wird sofort neu berechnet. *(US-06, US-08)*
5. Nach „Speichern" zeigt die Tagesübersicht den Eintrag sowie verbrauchte und offene kcal. *(US-12)*
6. Er scannt den Barcode eines Proteinjoghurts, gibt 200 g ein und speichert. Die Nährwerte stammen direkt aus der Datenbank. *(US-09)*
7. Fehlerfall: Ein Foto ohne Essen ergibt die Meldung „Keine Lebensmittel erkannt" und den Weg zur manuellen Suche. *(US-03)*
8. Zum Abschluss löscht der Nutzer sein Konto; danach ist keine Anmeldung mit diesen Daten mehr möglich. *(US-22)*

Dieser Ablauf adressiert das erste Problem aus dem [README](../README.md#problemstellung), den hohen Erfassungsaufwand, vollständig, und zwar mit dem Merkmal, das uns von bestehenden Apps unterscheidet: Foto **und** exakte Textangaben in einem Schritt, mit voller Kontrolle vor dem Speichern.

## Enthaltene Funktionen

### Must: Kern des MVP

Alle Must-Stories müssen mit ihren Akzeptanzkriterien erfüllt sein. Die ersten sieben bilden den Kernanwendungsfall; US-09 und US-22 sind begründete Ausnahmen vom Weglass-Test.

| ID | Funktion | Rolle im Kernanwendungsfall |
|---|---|---|
| US-01 | Registrierung und Login | schützt das Backend mit dem kostenpflichtigen KI-Dienst, Daten pro Nutzer |
| US-02 | Kalorienziel festlegen | Grundlage für das offene Budget |
| US-03 | Mahlzeit per Foto erfassen | Kernfunktion |
| US-04 | Foto mit Textangaben ergänzen | Kernfunktion, Alleinstellungsmerkmal |
| US-06 | KI-Ergebnis prüfen und korrigieren | fängt Ungenauigkeit der KI auf |
| US-08 | Lebensmittel manuell suchen | Ausweg bei Fehlerkennung, Ergänzen fehlender Positionen |
| US-12 | Tagesübersicht | macht das Ergebnis sichtbar |
| US-09 | Barcode scannen | exakte Erfassung verpackter Lebensmittel; Grundvoraussetzung für die Zielgruppe (Begründung in requirements.md) |
| US-22 | Konto löschen | Recht auf Löschung nach DSGVO, auch im Prototyp mit Testpersonen |

Dazu alle nichtfunktionalen Anforderungen NFR-01 bis NFR-08.

### Should: Ausbaustufe nach den Musts

Wird in dieser Reihenfolge umgesetzt, sobald alle Musts fertig und getestet sind:

| Reihenfolge | ID | Funktion | Warum diese Position |
|---|---|---|---|
| 1 | US-21 | Heutigen Eintrag bearbeiten/löschen | kleiner Aufwand, großer Nutzen im Alltag |
| 2 | US-14 + US-16 | Mahlzeitvorschlag aus häufig gegessenen Lebensmitteln, Vorschlag übernehmen | löst das zweite Problem, siehe Entscheidung unten |
| 3 | US-05 | Mahlzeit nur per Text | nutzt die vorhandene KI-Anbindung |
| 4 | US-10 | Eigenes Lebensmittel (privat) | |
| 5 | US-20 | Makroziele | |
| 6 | US-13 | Vergangene Tage | |

### Could: nur bei übriger Zeit

US-07 Mahlzeitkategorie · US-11 Öffentliche Lebensmittel · US-15 Vorschlag aus vorhandenen Lebensmitteln · US-19 Zuletzt verwendet

## Ausgeschlossene Funktionen (Won't this time)

| Funktion | Grund für den Ausschluss |
|---|---|
| Gewichtsverlauf (ehem. US-17) | hat nichts mit den zwei Problemen zu tun |
| Erinnerungen / Push-Benachrichtigungen (ehem. US-18) | Zielgruppe trackt bereits regelmäßig |
| Moderation öffentlicher Lebensmittel | eigenes Problemfeld (Missbrauch, Qualitätsprüfung), zu groß für ein Semester |
| Passwort zurücksetzen | im Prototyp nur Testnutzer; Zurücksetzen durch das Team |
| Offline-Erfassung und Synchronisation | hoher Aufwand (Konfliktbehandlung); KI braucht ohnehin eine Verbindung (NFR-08) |
| Automatische Zuordnung von KI-Positionen zu Datenbankeinträgen | Nutzer kann manuell ersetzen (US-06); Automatik erst bei Bedarf |
| Speichern und Anzeigen der Mahlzeitfotos | widerspricht der Datensparsamkeit (NFR-01), kein Mehrwert für das Problem |
| Mikronährstoffe (Vitamine, Mineralstoffe) | Zielgruppe arbeitet mit kcal und Makros |
| Anbindung von Wearables / Health Connect / Apple Health | kein Bezug zum Problem |
| Ernährungspläne, Gesundheits- oder Diätempfehlungen | App ist kein Medizinprodukt und gibt keine Beratung |
| iOS-Release und Tests | kein macOS-Gerät im Team garantiert; Flutter hält die Option offen |
| Mehrsprachigkeit | App nur auf Deutsch |

## Entscheidung Mahlzeitvorschlag

Der Mahlzeitvorschlag (US-14) löst das zweite Problem aus dem README (Planung mit dem Restbudget). Ob er Must oder Should ist, war im Team offen. Beides ist vertretbar:

- **Für Must:** Das Problem steht im README gleichrangig neben dem Erfassungsaufwand. Ohne Vorschlag ist das Produkt „nur" ein besserer Tracker.
- **Für Should:** Die Erfassung allein ist bereits ein vollständiger, demonstrierbarer Anwendungsfall. Der Vorschlag braucht außerdem eine Datenbasis aus mehreren Tagen echter Nutzung (mind. 5 häufig gegessene Lebensmittel, siehe Begriffe in requirements.md) und eine zweite, unabhängige KI-Anbindung mit eigenem Prompt und eigenen Tests.

**Entscheidung: Should, mit Reihenfolge 2 in der Ausbaustufe.** Ausschlaggebend ist der Weglass-Test: Der Kernanwendungsfall ist ohne Vorschlag vollständig vorführbar, umgekehrt aber nicht, denn ohne Erfassung gibt es keine Daten für einen Vorschlag. Damit der Vorschlag nicht unter den Tisch fällt, sind seine Akzeptanzkriterien schon jetzt messbar festgelegt (US-14) und er steht direkt hinter der kleinen Story US-21.

## Abhängigkeiten und Kosten

```
Flutter-App  ──HTTPS──▶  Backend  ──▶  KI-Dienst mit Bildverarbeitung (kostenpflichtig)
                           │
                           ├──▶  Lebensmitteldatenbank (Open Food Facts, kostenlos)
                           └──▶  Datenbank: Konten, Einträge, eigene Lebensmittel
```

| Abhängigkeit | Risiko | Umgang |
|---|---|---|
| KI-Dienst (kostenpflichtig pro Anfrage) | laufende Kosten, v. a. bei wiederholten Tests (FR-05: 5 Testfälle × 10 Durchläufe) | Bildverkleinerung und Tageslimit (NFR-03); monatliches Ausgabenlimit beim Anbieter; Konto des Anbieters gehört dem Team. Konkreter Anbieter und Preis werden in Phase 2 festgelegt. |
| KI-Dienst (Verfügbarkeit, Antwortqualität) | Ausfall oder unbrauchbare Antworten | Fehlerfälle in US-03, Schema-Prüfung in FR-04, manuelle Suche als Ausweg |
| Open Food Facts | Lücken und Fehler in den Daten, besonders bei offenen Lebensmitteln; nicht jeder Barcode ist erfasst | Nährwerte vor Übernahme sichtbar (US-08, US-09); bei unbekanntem Barcode Weg zur manuellen Suche (US-09); eigene Lebensmittel als Should (US-10) |
| Backend und Hosting | Betrieb und Datenschutz | Hosting in der EU (NFR-01); Auswahl in Phase 2 |

## Wann ist das MVP fertig?

Das MVP gilt als erreicht, wenn

1. das Demo-Szenario oben auf einem Android-Gerät durchgespielt werden kann,
2. alle Akzeptanzkriterien der 9 Must-Stories erfüllt sind (inkl. der Testserie aus FR-05) und
3. die Prüfungen aller NFRs (Spalte „Prüfung" in requirements.md) bestanden sind.
