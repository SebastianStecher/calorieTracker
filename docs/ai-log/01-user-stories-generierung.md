# AI-Einsatz 1: User Stories aus der Projektidee generieren

- **Datum:** 14.10.2026
- **Modell:** Claude Opus 5.5 (claude.ai)
- **Zweck:** Erste Sammlung von User Stories als Grundlage für die Anforderungsanalyse. Die Ausgabe unten ist **unverändert**. Prüfung und Überarbeitung erfolgen durch das Team (siehe Abschnitt „Bewertung durch das Team").

## Prompt

```
Du bist Requirements Engineer. Erstelle User Stories für folgende Produktidee.

Produkt: Mobile App (Flutter) zum Kalorien-Tracking.
Zielgruppe: Personen, die bereits Kalorien tracken.
Kernfunktionen:
1. KI-gestützte Erfassung per Foto, ergänzt um Freitext (z. B. gewogene Mengen).
   Die KI erkennt Lebensmittel und schätzt Nährwerte.
2. KI-Mahlzeitvorschlag passend zum offenen Kalorienbudget, basierend auf häufig
   gegessenen Lebensmitteln des Nutzers oder auf Lebensmitteln, die er zu Hause hat.
3. Externe Lebensmitteldatenbank, erweiterbar durch Nutzer: eigene Lebensmittel
   mit oder ohne Barcode, beim Anlegen wählbar als privat oder öffentlich.
4. Login mit Benutzerkonto.

Rahmen: 3 Studierende, ein Semester, Ziel ist ein funktionsfähiger Prototyp.

Anforderungen an die Ausgabe:
- Format: „Als <Rolle> möchte ich <Ziel>, damit <Nutzen>."
- Fortlaufende ID (US-01, US-02, ...)
- Je Story ein Prioritätsvorschlag (Must/Should/Could)
- Für Must-Stories 2–4 überprüfbare Akzeptanzkriterien
- Auf Deutsch
```

## Rohausgabe (unverändert)

### Konto und Profil

**US-01 – Registrierung und Login** · *Must*
Als Nutzer möchte ich mich mit E-Mail und Passwort registrieren und anmelden, damit meine Daten geräteübergreifend gespeichert sind.
- Ein neuer Nutzer kann ein Konto mit E-Mail und Passwort anlegen.
- Ein registrierter Nutzer kann sich an- und abmelden.
- Bei falschen Zugangsdaten wird eine Fehlermeldung angezeigt.

**US-02 – Kalorienziel festlegen** · *Must*
Als Nutzer möchte ich mein tägliches Kalorienziel und optional meine Makroziele festlegen, damit die App mein Restbudget berechnen kann.
- Der Nutzer kann ein Tagesziel in kcal eingeben und ändern.
- Optional können Ziele für Protein, Kohlenhydrate und Fett in Gramm gesetzt werden.
- Das geänderte Ziel wird sofort in der Tagesübersicht berücksichtigt.

### KI-Erfassung

**US-03 – Mahlzeit per Foto erfassen** · *Must*
Als Nutzer möchte ich ein Foto meiner Mahlzeit aufnehmen, damit die KI die enthaltenen Lebensmittel und deren Nährwerte erkennt.
- Der Nutzer kann ein Foto mit der Kamera aufnehmen oder aus der Galerie wählen.
- Die App zeigt eine Liste erkannter Lebensmittel mit geschätzter Menge und kcal.
- Ist keine Erkennung möglich, erhält der Nutzer eine verständliche Meldung und kann manuell weitermachen.

**US-04 – Foto mit Textangaben ergänzen** · *Must*
Als Nutzer möchte ich zum Foto zusätzlich Text eingeben (z. B. „150 g Reis gewogen"), damit genaue Angaben die Schätzung der KI ersetzen.
- Der Nutzer kann vor dem Absenden einen Freitext zum Foto eingeben.
- Im Text angegebene Mengen werden exakt übernommen und nicht geschätzt.
- Im Text genannte, auf dem Foto nicht sichtbare Lebensmittel (z. B. Öl) werden ergänzt.

**US-05 – Mahlzeit nur per Text erfassen** · *Should*
Als Nutzer möchte ich eine Mahlzeit nur per Text beschreiben, damit ich auch ohne Foto schnell erfassen kann.

**US-06 – KI-Ergebnis prüfen und korrigieren** · *Must*
Als Nutzer möchte ich das Ergebnis der KI vor dem Speichern bearbeiten, damit nur korrekte Werte in mein Tagebuch kommen.
- Der Nutzer kann Mengen ändern sowie Lebensmittel löschen und hinzufügen.
- Nährwerte werden bei Mengenänderung automatisch neu berechnet.
- Erst nach Bestätigung wird der Eintrag gespeichert.

**US-07 – Mahlzeit einer Kategorie zuordnen** · *Could*
Als Nutzer möchte ich einen Eintrag einer Mahlzeit zuordnen (Frühstück, Mittag, Abend, Snack), damit mein Tagebuch übersichtlich ist.

### Lebensmitteldatenbank

**US-08 – Lebensmittel manuell suchen** · *Must*
Als Nutzer möchte ich Lebensmittel in der Datenbank suchen und mit Menge hinzufügen, damit ich auch ohne KI erfassen kann.
- Die Suche liefert Treffer aus der externen Datenbank und aus eigenen bzw. öffentlichen Nutzer-Lebensmitteln.
- Der Nutzer kann eine Menge in Gramm angeben, die Nährwerte werden berechnet.

**US-09 – Barcode scannen** · *Should*
Als Nutzer möchte ich den Barcode eines Produkts scannen, damit ich verpackte Lebensmittel schnell finde.

**US-10 – Eigenes Lebensmittel anlegen** · *Should*
Als Nutzer möchte ich ein eigenes Lebensmittel mit Nährwerten (optional mit Barcode) anlegen und festlegen, ob es privat oder öffentlich ist, damit fehlende Produkte erfasst werden können.

**US-11 – Öffentliche Nutzer-Lebensmittel verwenden** · *Could*
Als Nutzer möchte ich Lebensmittel finden, die andere Nutzer öffentlich angelegt haben, damit ich von der Community profitiere.

### Übersicht

**US-12 – Tagesübersicht** · *Must*
Als Nutzer möchte ich sehen, wie viele kcal und Makros ich heute verbraucht habe und wie viele noch offen sind, damit ich meinen Tag steuern kann.
- Die Übersicht zeigt Ziel, verbrauchte und offene kcal.
- Makros werden angezeigt, sofern Ziele gesetzt sind.
- Neue oder geänderte Einträge werden sofort berücksichtigt.

**US-13 – Vergangene Tage ansehen und bearbeiten** · *Should*
Als Nutzer möchte ich Einträge früherer Tage ansehen und bearbeiten, damit ich vergessene oder falsche Einträge nachtragen kann.

### KI-Mahlzeitvorschlag

**US-14 – Vorschlag aus häufig gegessenen Lebensmitteln** · *Must*
Als Nutzer möchte ich einen Mahlzeitvorschlag für mein offenes Budget erhalten, der auf meinen häufig gegessenen Lebensmitteln basiert, damit ich schnell eine passende Mahlzeit finde.
- Der Vorschlag nutzt das aktuell offene Kalorienbudget.
- Der Vorschlag enthält Zutaten mit Mengen und Gesamt-kcal.
- Die Gesamt-kcal liegen nahe am offenen Budget.

**US-15 – Vorschlag aus vorhandenen Lebensmitteln** · *Should*
Als Nutzer möchte ich eingeben, welche Lebensmittel ich zu Hause habe, damit der Vorschlag nur daraus besteht.

**US-16 – Vorschlag übernehmen** · *Should*
Als Nutzer möchte ich einen Vorschlag mit einem Tipp ins Tagebuch übernehmen, damit ich ihn nicht erneut erfassen muss.

### Weitere Ideen

**US-17 – Gewichtsverlauf** · *Could*
Als Nutzer möchte ich mein Körpergewicht eintragen und als Verlauf sehen, damit ich meinen Fortschritt verfolgen kann.

**US-18 – Erinnerungen** · *Could*
Als Nutzer möchte ich Erinnerungen zum Tracken erhalten, damit ich keine Mahlzeit vergesse.

**US-19 – Favoriten und zuletzt verwendet** · *Could*
Als Nutzer möchte ich häufig gegessene Lebensmittel und Mahlzeiten schnell wiederfinden, damit wiederkehrende Einträge weniger Aufwand sind.

## Bewertung durch das Team

Die Rohausgabe war eine brauchbare Grundlage: Format, IDs und die thematische Gliederung haben wir übernommen. Bei Priorisierung und Akzeptanzkriterien war aber deutliche Nacharbeit nötig. Die AI hat 8 von 19 Stories als Must eingestuft, darunter den Mahlzeitvorschlag (US-14). Mit dem Weglass-Test (*Ist der Kernanwendungsfall ohne diese Story noch vorführbar?*) blieben zunächst 7 Musts übrig. Zwei Stories (US-09 Barcode, US-22 Konto löschen) haben wir danach bewusst und mit eigener Begründung auf Must hochgestuft, also insgesamt 9. Die überarbeitete Fassung steht in [requirements.md](../requirements.md).

| Story | Entscheidung | Begründung |
|---|---|---|
| US-01 Login | übernommen, ergänzt | Must bleibt, aber mit anderer Begründung: Das Backend ruft einen kostenpflichtigen KI-Dienst auf und muss geschützt sein. Kriterien ergänzt: Daten nach Neuanmeldung vorhanden, keine Funktion ohne Login, keine Preisgabe existierender E-Mail-Adressen. |
| US-02 Kalorienziel | geändert | Makroziele ausgelagert (→ US-20, Should), weil sie für den Kernanwendungsfall nicht nötig sind. Gültiger Wertebereich ergänzt, damit das Kriterium prüfbar ist. |
| US-03 Foto | geändert | „Verständliche Meldung" war nicht prüfbar. Ersetzt durch konkrete Fehlerfälle (nichts erkannt, Timeout 30 s, Dienst nicht erreichbar) mit jeweils definierter Meldung. Kennzeichnung als „KI-Schätzung" ergänzt. |
| US-04 Textangaben | übernommen, präzisiert | Kern unserer Idee. Konkrete Testfälle ergänzt; später durch AI-Einsatz 2 auf Grammangaben eingeschränkt. |
| US-05 Nur Text | übernommen | Should passt: nutzt dieselbe KI-Anbindung, ist aber nicht Teil des Kernablaufs. |
| US-06 Korrigieren | übernommen, ergänzt | Wichtigste Must-Story, weil sie die Ungenauigkeit der KI auffängt. Ergänzt: „Abbrechen" verwirft, kein leerer Eintrag. |
| US-07 Kategorie | übernommen | Could ist angemessen. |
| US-08 Suche | geändert | Must bleibt (einziger Ausweg bei Fehlerkennung). Öffentliche Nutzer-Lebensmittel aus den Kriterien entfernt, weil die AI hier eine Could-Funktion (US-11) in eine Must-Story eingebaut hatte. |
| US-09 Barcode | hochgestuft auf Must, Kriterien ergänzt | Zuerst hatten wir auf Could herabgestuft (kein Alleinstellungsmerkmal). Nach nochmaliger Diskussion umgekehrt: Für verpackte Lebensmittel ist der Barcode genauer als das Foto und schneller als die Suche; erfahrene Tracker würden ohne ihn nicht wechseln. Die AI hatte Should vorgeschlagen und keine Akzeptanzkriterien geliefert; diese haben wir selbst ergänzt. |
| US-10 Eigenes Lebensmittel | geändert | Die AI hatte „privat oder öffentlich" in eine Should-Story gepackt, die öffentliche Nutzung aber in US-11 als Could geführt. Das widerspricht sich. Jetzt: US-10 nur privat (Should), Veröffentlichen gehört zu US-11. |
| US-11 Öffentlich | geändert | Teilen und Finden in einer Story zusammengeführt (Could). Ungelöste Frage, wie falsche öffentliche Einträge verhindert werden, als offener Punkt dokumentiert. |
| US-12 Tagesübersicht | übernommen, ergänzt | Kriterien ergänzt: Darstellung bei Überschreitung, Liste der Einträge. |
| US-13 Vergangene Tage | übernommen | Should passt. |
| US-14 Vorschlag | herabgestuft auf Should, Kriterien neu | „Gesamt-kcal liegen nahe am offenen Budget" ist nicht prüfbar. Neu: KI wählt nur Lebensmittel und Mengen, die App rechnet; max. ±50 kcal Abweichung. Priorität siehe [mvp.md](../mvp.md#entscheidung-mahlzeitvorschlag). |
| US-15 Vorhandene Lebensmittel | herabgestuft auf Could | Setzt eine weitere Eingabe voraus und ist eine Variante von US-14. |
| US-16 Übernehmen | übernommen | Should; nur sinnvoll, wenn US-14 umgesetzt wird. |
| US-17 Gewichtsverlauf | gestrichen (Won't) | Scope Creep: hat nichts mit unseren zwei Problemen zu tun. |
| US-18 Erinnerungen | gestrichen (Won't) | Scope Creep, Zielgruppe trackt bereits regelmäßig. |
| US-19 Favoriten | übernommen, eingeschränkt | Could; auf „zuletzt verwendet zuerst" eingeschränkt. |

**Lücken, die die AI nicht abgedeckt hat und die wir ergänzt haben:**
- Gespeicherte Einträge bearbeiten oder löschen (→ US-21, Should)
- Verhalten, wenn der KI-Dienst nicht erreichbar ist (→ US-03 Kriterium 3, NFR-04)
- Nichtfunktionale Anforderungen (Datenschutz, Sicherheit, Kosten, Performance). Der Prompt fragte nur nach User Stories, das Fehlen war also erwartbar; wir haben sie anhand der vorab besprochenen Risiken selbst formuliert (→ NFR-01 bis NFR-08).
