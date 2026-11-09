# Anforderungen und User Stories

Dieses Dokument beschreibt, **was** der KI-Kalorientracker leisten muss. Projektidee, Problemstellung und Zielgruppe stehen im [README](../README.md), der Umfang des Prototyps in [mvp.md](mvp.md).

Die User Stories basieren auf einem AI-generierten Erstentwurf ([ai-log/01](ai-log/01-user-stories-generierung.md)), der vom Team überarbeitet wurde. Anschließend wurde die erste vollständige Fassung dieses Dokuments per AI auf Widersprüche und Lücken geprüft ([ai-log/02](ai-log/02-anforderungen-pruefen.md)); die übernommenen Funde sind hier bereits eingearbeitet. Eine Zusammenfassung beider AI-Einsätze steht in [ai-usage.md](ai-usage.md).

## 1. Begriffe

| Begriff | Bedeutung |
|---|---|
| **Eintrag** | Eine gespeicherte Mahlzeit im Tagebuch, bestehend aus einer oder mehreren Positionen und einem Datum. |
| **Position** | Ein Lebensmittel innerhalb eines Eintrags mit Menge in Gramm sowie kcal, Protein, Kohlenhydraten und Fett. |
| **Tag** | Kalendertag in der Zeitzone des Geräts (00:00–23:59). Ein Eintrag gehört zu dem Tag, den sein Datum angibt. Das Datum ist standardmäßig „heute" und kann nur beim Nachtragen (US-13) geändert werden. |
| **Tagesziel** | Vom Nutzer festgelegte kcal-Menge pro Tag. |
| **Offenes Budget** | Tagesziel minus Summe der kcal aller Einträge des aktuellen Tages. Kann negativ sein. |
| **KI-Analyse** | Übermittlung von Foto und/oder Text an den KI-Dienst (über das Backend). Ergebnis ist ein **Vorschlag** aus Positionen, der erst nach Bestätigung durch den Nutzer gespeichert wird. |
| **Datenbank-Lebensmittel** | Ein Lebensmittel aus der externen Datenbank oder ein eigenes Lebensmittel (US-10), eindeutig identifiziert über eine ID. KI-Positionen haben keine ID, solange der Nutzer sie nicht durch ein Datenbank-Lebensmittel ersetzt (US-06). |
| **Häufig gegessenes Lebensmittel** | Ein Datenbank-Lebensmittel, das in den letzten 14 Tagen in mindestens 2 Einträgen vorkam. Positionen ohne ID zählen nicht. |

## 2. Priorisierung (MoSCoW)

| Stufe | Bedeutung in diesem Projekt |
|---|---|
| **Must** | Ohne diese Funktion ist der Kernanwendungsfall (Mahlzeit per Foto + Text erfassen, prüfen, speichern, in der Tagesbilanz sehen) nicht vorführbar, **oder** sie ist rechtlich verpflichtend bzw. für die Zielgruppe eine Grundvoraussetzung, um die App statt einer bestehenden zu nutzen. |
| **Should** | Wichtig für die Zielgruppe, aber der Kernanwendungsfall funktioniert auch ohne. Wird umgesetzt, sobald alle Musts fertig und getestet sind. |
| **Could** | Nur bei übriger Zeit; fällt als Erstes weg. |
| **Won't (this time)** | Bewusst nicht Teil dieses Semesters, siehe [mvp.md](mvp.md). |

Leitfrage für jede Must-Story war der **Weglass-Test**: *Ist der Kernanwendungsfall ohne diese Story noch vollständig vorführbar?* Wenn ja, ist sie grundsätzlich kein Must. Ausnahmen sind nur mit eigener Begründung zulässig; das betrifft US-09 (Zielgruppe) und US-22 (DSGVO).

## 3. User Stories

### Übersicht

| ID | Titel | Priorität | Herkunft |
|---|---|---|---|
| US-01 | Registrierung und Login | Must | AI, Kriterien ergänzt |
| US-02 | Kalorienziel festlegen | Must | AI, Makroziele ausgelagert |
| US-03 | Mahlzeit per Foto erfassen | Must | AI, Kriterien präzisiert |
| US-04 | Foto mit Textangaben ergänzen | Must | AI, Kriterien präzisiert |
| US-05 | Mahlzeit nur per Text erfassen | Should | AI |
| US-06 | KI-Ergebnis prüfen und korrigieren | Must | AI, Kriterien ergänzt |
| US-07 | Eintrag einer Mahlzeitkategorie zuordnen | Could | AI |
| US-08 | Lebensmittel manuell suchen | Must | AI, Datenquelle eingeschränkt |
| US-09 | Barcode scannen | Must | AI, vom Team auf Must hochgestuft |
| US-10 | Eigenes Lebensmittel anlegen (privat) | Should | AI, „öffentlich" ausgelagert |
| US-11 | Eigene Lebensmittel öffentlich teilen und finden | Could | AI, zusammengeführt |
| US-12 | Tagesübersicht | Must | AI, Kriterien ergänzt |
| US-13 | Vergangene Tage ansehen und bearbeiten | Should | AI |
| US-14 | Vorschlag aus häufig gegessenen Lebensmitteln | Should | AI, von Must herabgestuft, Kriterien messbar gemacht |
| US-15 | Vorschlag aus vorhandenen Lebensmitteln | Could | AI, von Should herabgestuft |
| US-16 | Vorschlag übernehmen | Should | AI |
| US-19 | Favoriten und zuletzt verwendet | Could | AI |
| US-20 | Makroziele festlegen | Should | Team (aus US-02 ausgelagert) |
| US-21 | Heutigen Eintrag bearbeiten oder löschen | Should | Team (Lücke) |
| US-22 | Konto löschen | Must | AI-Review (Fund 5), vom Team auf Must hochgestuft |

**Summe:** 20 Stories: 9 Must · 7 Should · 4 Could. US-17 (Gewichtsverlauf) und US-18 (Erinnerungen) wurden gestrichen und sind als *Won't* in [mvp.md](mvp.md) dokumentiert. Die IDs wurden nicht neu vergeben, damit die Änderungen gegenüber dem AI-Entwurf nachvollziehbar bleiben.

### Konto und Ziele

**US-01 – Registrierung und Login** · **Must**
Als Nutzer möchte ich mich mit E-Mail und Passwort registrieren und anmelden, damit meine Einträge gespeichert bleiben und nur ich darauf zugreifen kann.

*Begründung Must:* Die KI-Analyse läuft über ein Backend mit kostenpflichtigem KI-Dienst. Ohne Anmeldung wäre dieser Endpunkt für jeden aufrufbar (Kostenrisiko, siehe NFR-03). Außerdem werden Einträge serverseitig dem Nutzer zugeordnet.

Akzeptanzkriterien:
1. Ein neuer Nutzer kann mit gültiger E-Mail-Adresse und einem Passwort mit mindestens 8 Zeichen ein Konto anlegen.
2. Ein registrierter Nutzer kann sich an- und abmelden; nach erneuter Anmeldung (auch auf einem anderen Gerät) sind seine Einträge und sein Tagesziel vorhanden.
3. Bei falschen Zugangsdaten erscheint eine Fehlermeldung; es wird nicht verraten, ob die E-Mail-Adresse existiert.
4. Ohne Anmeldung ist keine Funktion außer Registrierung und Login erreichbar.

**US-02 – Kalorienziel festlegen** · **Must**
Als Nutzer möchte ich mein tägliches Kalorienziel festlegen, damit die App mein offenes Budget berechnen kann.

Akzeptanzkriterien:
1. Der Nutzer kann ein Tagesziel als ganze Zahl zwischen 800 und 6000 kcal eingeben und jederzeit ändern; Werte außerhalb werden mit Hinweis abgelehnt. *(Die Grenzen fangen Tippfehler wie 250 statt 2500 ab; die Untergrenze verhindert zudem extrem niedrige Ziele, da die App keine Ernährungsberatung leistet.)*
2. Nach dem ersten Login wird der Nutzer aufgefordert, ein Tagesziel festzulegen, bevor er die Tagesübersicht sieht.
3. Eine Änderung wirkt sich sofort auf das offene Budget des aktuellen Tages aus.

**US-20 – Makroziele festlegen** · *Should*
Als Nutzer möchte ich optional Ziele für Protein, Kohlenhydrate und Fett in Gramm festlegen, damit ich auch meine Makros im Blick habe. Sind Makroziele gesetzt, zeigt die Tagesübersicht (US-12) zusätzlich gegessene und offene Gramm je Makronährstoff.

### KI-Erfassung (Kernfunktion)

**US-03 – Mahlzeit per Foto erfassen** · **Must**
Als Nutzer möchte ich ein Foto meiner Mahlzeit aufnehmen, damit die KI die enthaltenen Lebensmittel erkennt und Mengen und Nährwerte schätzt.

Akzeptanzkriterien:
1. Der Nutzer kann ein Foto mit der Kamera aufnehmen oder aus der Galerie wählen.
2. Nach der KI-Analyse zeigt die App eine Liste von Positionen mit Name, geschätzter Menge in g und kcal. Die Liste ist sichtbar als „KI-Schätzung" gekennzeichnet.
3. In folgenden Fällen zeigt die App eine Meldung mit dem jeweiligen Grund und bietet die manuelle Erfassung (US-08) an; es wird nichts gespeichert:
   - die KI erkennt keine Lebensmittel → „Keine Lebensmittel erkannt"
   - der KI-Dienst antwortet nicht innerhalb von 30 s oder ist nicht erreichbar → „Analyse derzeit nicht möglich"
   - das Tageslimit für KI-Anfragen (NFR-03) ist erreicht → „Tageslimit erreicht, morgen wieder verfügbar"
4. Während der Analyse ist ein Ladezustand sichtbar und die Analyse kann abgebrochen werden.

**US-04 – Foto mit Textangaben ergänzen** · **Must**
Als Nutzer möchte ich zum Foto zusätzlich Text eingeben (z. B. „150 g Reis gewogen"), damit meine genauen Angaben die Schätzung der KI ersetzen.

Akzeptanzkriterien:
1. Vor dem Absenden kann ein optionaler Freitext (max. 500 Zeichen) zum Foto eingegeben werden.
2. Eine im Text genannte Menge **in Gramm** wird für das betreffende Lebensmittel exakt übernommen. *Testfall:* Foto einer Reis-Bowl + „150 g Reis gewogen" → Position „Reis" hat 150 g.
3. Im Text genannte, auf dem Foto nicht sichtbare Lebensmittel werden als eigene Position ergänzt. Mengen in anderen Einheiten (ml, EL, Stück) rechnet die KI in Gramm um; diese gelten als Schätzung. *Testfall:* „+ 1 EL Olivenöl" → Position „Olivenöl" ist mit einer Menge zwischen 5 und 20 g vorhanden.
4. Positionen, deren Grammangabe aus dem Text stammt, sind in der Ergebnisliste als „vom Nutzer angegeben" erkennbar.
5. AK2 und AK3 gelten als erfüllt, wenn die Testfälle aus FR-05 die dort festgelegte Schwelle erreichen.

**US-05 – Mahlzeit nur per Text erfassen** · *Should*
Als Nutzer möchte ich eine Mahlzeit nur per Text beschreiben (z. B. „2 Eier, 1 Scheibe Vollkornbrot"), damit ich auch ohne Foto schnell erfassen kann.

**US-06 – KI-Ergebnis prüfen und korrigieren** · **Must**
Als Nutzer möchte ich das Ergebnis der KI vor dem Speichern bearbeiten, damit nur Werte in mein Tagebuch kommen, die ich geprüft habe.

Akzeptanzkriterien:
1. Der Nutzer kann bei jeder Position die Menge ändern sowie Positionen löschen und über die Suche (US-08) neue Positionen hinzufügen.
2. Der Nutzer kann eine KI-Position durch ein Datenbank-Lebensmittel aus der Suche ersetzen; die Menge bleibt erhalten, die Nährwerte kommen aus der Datenbank.
3. Bei einer Mengenänderung werden kcal und Makros der Position und die Summe des Eintrags sofort neu berechnet (Grundlage: Nährwerte pro 100 g der Position).
4. Der Eintrag wird erst nach Tippen auf „Speichern" gespeichert; „Abbrechen" verwirft das Ergebnis vollständig.
5. Ein Eintrag mit 0 Positionen kann nicht gespeichert werden.

**US-07 – Eintrag einer Mahlzeitkategorie zuordnen** · *Could*
Als Nutzer möchte ich einen Eintrag einer Kategorie (Frühstück, Mittagessen, Abendessen, Snack) zuordnen, damit mein Tagebuch übersichtlicher ist.

### Lebensmitteldaten

**US-08 – Lebensmittel manuell suchen** · **Must**
Als Nutzer möchte ich Lebensmittel in der Datenbank suchen und mit Menge hinzufügen, damit ich auch ohne KI oder bei falscher Erkennung erfassen kann.

*Begründung Must:* Ohne manuelle Suche gibt es keinen Ausweg, wenn die KI ein Lebensmittel nicht oder falsch erkennt (US-03 Kriterium 3, US-06 Kriterium 1).

Akzeptanzkriterien:
1. Ab 3 eingegebenen Zeichen zeigt die Suche Treffer aus der externen Lebensmitteldatenbank.
2. Jeder Treffer zeigt Name und kcal pro 100 g.
3. Nach Auswahl und Eingabe einer Menge in g werden kcal und Makros berechnet und als Position übernommen.
4. Findet die Suche nichts, wird „Keine Treffer" angezeigt.

**US-09 – Barcode scannen** · **Must**
Als Nutzer möchte ich den Barcode eines verpackten Produkts scannen, damit ich es schnell und mit exakten Nährwerten erfasse.

*Begründung Must:* Verpackte Lebensmittel (Joghurt, Riegel, Proteinpulver) sind bei der Zielgruppe ein großer Teil der Ernährung. Für sie ist der Barcode genauer als jede Foto-Schätzung, weil die Herstellerangaben direkt übernommen werden, und schneller als die Suche. Alle bestehenden Tracking-Apps bieten ihn an; ohne Barcode wäre unsere App für erfahrene Tracker bei Packungsware ein Rückschritt, und sie würden nicht wechseln. Das widerspricht dem Ziel „Zeit sparen, ohne Genauigkeit zu verlieren".

Akzeptanzkriterien:
1. Der Barcode-Scanner ist sowohl als eigener Erfassungsweg als auch im Prüfdialog (US-06) zum Hinzufügen einer Position erreichbar.
2. Ein EAN-13- oder EAN-8-Barcode wird mit der Kamera erkannt und in der externen Lebensmitteldatenbank nachgeschlagen. *Testfall:* 5 vom Team festgelegte Produkte aus österreichischen Supermärkten werden jeweils gefunden.
3. Bei einem Treffer zeigt die App Produktname und kcal pro 100 g; nach Eingabe einer Menge in g wird die Position mit den Nährwerten der Datenbank übernommen (Datenbank-Lebensmittel mit ID).
4. Ist das Produkt nicht in der Datenbank oder fehlen Nährwerte, erscheint „Produkt nicht gefunden" und die manuelle Suche (US-08) wird angeboten. Es wird keine Position mit leeren Werten angelegt.

**US-10 – Eigenes Lebensmittel anlegen (privat)** · *Should*
Als Nutzer möchte ich ein eigenes Lebensmittel mit Name, kcal und Makros pro 100 g (optional mit Barcode) anlegen, damit ich Produkte erfassen kann, die in der Datenbank fehlen. Das Lebensmittel ist nur für mich sichtbar und erscheint in meiner Suche (US-08) zusammen mit den Treffern der externen Datenbank.

**US-11 – Eigene Lebensmittel öffentlich teilen und finden** · *Could*
Als Nutzer möchte ich beim Anlegen festlegen können, dass mein Lebensmittel öffentlich ist, und öffentliche Lebensmittel anderer Nutzer in der Suche finden, damit fehlende Produkte nicht jeder selbst anlegen muss.

### Übersicht und Tagebuch

**US-12 – Tagesübersicht** · **Must**
Als Nutzer möchte ich sehen, wie viele kcal ich heute gegessen habe und wie viele noch offen sind, damit ich meinen Tag steuern kann.

Akzeptanzkriterien:
1. Die Übersicht zeigt Tagesziel, Summe der heutigen kcal und offenes Budget. Ein negatives Budget wird als Überschreitung dargestellt (z. B. „120 kcal über Ziel").
2. Die Übersicht listet alle heutigen Einträge mit Uhrzeit und kcal.
3. Nach dem Speichern eines Eintrags ist die Übersicht ohne manuelles Neuladen aktualisiert.

**US-21 – Heutigen Eintrag bearbeiten oder löschen** · *Should*
Als Nutzer möchte ich einen bereits gespeicherten Eintrag nachträglich ändern oder löschen, damit ein Fehler nicht dauerhaft in meiner Bilanz bleibt.

**US-13 – Vergangene Tage ansehen und bearbeiten** · *Should*
Als Nutzer möchte ich Einträge früherer Tage ansehen und für einen früheren Tag nachtragen (Datum wählbar), damit ich vergessene Mahlzeiten ergänzen kann.

### KI-Mahlzeitvorschlag

**US-14 – Vorschlag aus häufig gegessenen Lebensmitteln** · *Should*
Als Nutzer möchte ich für mein offenes Budget einen Mahlzeitvorschlag erhalten, der aus meinen häufig gegessenen Lebensmitteln besteht, damit ich abends schnell eine passende Mahlzeit finde.

*Begründung Should statt Must:* siehe [mvp.md](mvp.md#entscheidung-mahlzeitvorschlag).

Akzeptanzkriterien (für die Umsetzung bereits festgelegt):
1. Die KI wählt nur Lebensmittel und Mengen aus; die kcal berechnet die App aus den Nährwerten der Datenbank-Lebensmittel.
2. Der Vorschlag besteht nur aus häufig gegessenen Lebensmitteln (Definition siehe Begriffe) und zeigt jede Zutat mit Menge in g sowie die Gesamt-kcal.
3. Die Gesamt-kcal weichen um höchstens ±50 kcal vom offenen Budget ab. Gelingt das nicht, wird kein Vorschlag angezeigt, sondern eine Meldung.
4. Ist das offene Budget kleiner als 150 kcal oder gibt es weniger als 5 häufig gegessene Lebensmittel, wird statt eines Vorschlags eine Meldung mit dem Grund angezeigt.

**US-15 – Vorschlag aus vorhandenen Lebensmitteln** · *Could*
Als Nutzer möchte ich für einen Vorschlag eingeben, welche Lebensmittel ich gerade zu Hause habe, damit der Vorschlag nur daraus besteht.

**US-16 – Vorschlag übernehmen** · *Should*
Als Nutzer möchte ich einen Vorschlag mit einem Tipp als Eintrag übernehmen und vorher wie in US-06 anpassen, damit ich ihn nicht erneut erfassen muss.

### Konto verwalten

**US-22 – Konto löschen** · **Must**
Als Nutzer möchte ich mein Konto löschen können, damit alle meine Ernährungsdaten entfernt werden.

*Begründung Must:* Ernährungsdaten können Rückschlüsse auf die Gesundheit zulassen. Das Recht auf Löschung (Art. 17 DSGVO) muss der Nutzer selbst ausüben können, ohne das Team kontaktieren zu müssen. Da die App mit echten Testpersonen erprobt wird (NFR-05), gilt das schon für den Prototyp.

Akzeptanzkriterien:
1. In den Einstellungen gibt es „Konto löschen"; die Löschung muss durch erneute Eingabe des Passworts bestätigt werden.
2. Nach der Löschung sind alle Einträge, Ziele und eigenen Lebensmittel des Nutzers endgültig aus der Datenbank entfernt. *Prüfung:* Eine Abfrage der Datenbank nach der Nutzer-ID liefert keine Datensätze.
3. Der Nutzer wird abgemeldet; eine Anmeldung mit den alten Zugangsdaten schlägt fehl, die E-Mail-Adresse kann neu registriert werden.

### Komfort

**US-19 – Favoriten und zuletzt verwendet** · *Could*
Als Nutzer möchte ich zuletzt verwendete Lebensmittel in der Suche zuerst sehen, damit wiederkehrende Einträge schneller gehen.

## 4. Funktionale Anforderungen

Die funktionalen Anforderungen beschreiben das Systemverhalten hinter den User Stories. Die Priorität ergibt sich aus der zugehörigen Story.

| ID | Anforderung | Story | Prio |
|---|---|---|---|
| FR-01 | Das System muss Konten mit E-Mail und Passwort anlegen und Nutzer authentifizieren. Alle Daten sind dem Konto zugeordnet und serverseitig gespeichert. | US-01 | Must |
| FR-02 | Das System muss pro Nutzer ein Tagesziel (800–6000 kcal) speichern. | US-02 | Must |
| FR-03 | Die App muss ein Foto (Kamera oder Galerie) und optional einen Freitext (max. 500 Zeichen) an das Backend senden. Das Backend leitet die Anfrage an den KI-Dienst weiter. | US-03, US-04 | Must |
| FR-04 | Der KI-Dienst muss eine strukturierte Antwort (JSON) liefern: Liste von Positionen mit Name, Menge in g, kcal, Protein, Kohlenhydrate und Fett jeweils pro 100 g sowie einer Markierung, ob die Menge aus dem Nutzertext stammt. Antworten, die nicht diesem Schema entsprechen, werden wie „keine Lebensmittel erkannt" behandelt. | US-03, US-04 | Must |
| FR-05 | Im Freitext angegebene Grammangaben haben Vorrang vor der KI-Schätzung; das wird im Prompt an den KI-Dienst vorgegeben. Da KI-Antworten nicht deterministisch sind, gilt: Es gibt 5 feste Testfälle (Foto + Text). Jeder Testfall wird 10-mal ausgeführt; mindestens 9 von 10 Durchläufen müssen die Kriterien von US-04 erfüllen. | US-04 | Must |
| FR-06 | Das System muss nach einer Mengenänderung die Werte einer Position als Menge × Wert pro 100 g / 100 berechnen. | US-06, US-08 | Must |
| FR-07 | Das System muss die externe Lebensmitteldatenbank durchsuchen und die Nährwerte pro 100 g übernehmen. | US-08 | Must |
| FR-08 | Das System muss Einträge mit Datum, Uhrzeit, Positionen und Herkunft je Position (KI / Datenbank) speichern. | US-06 | Must |
| FR-09 | Das System muss Tagessumme und offenes Budget für den aktuellen Tag berechnen. | US-12 | Must |
| FR-10 | Das System muss gespeicherte Einträge ändern und löschen können. | US-13, US-21 | Should |
| FR-11 | Das System muss eigene Lebensmittel mit Sichtbarkeit „privat" speichern und in der Suche des Erstellers anzeigen. | US-10 | Should |
| FR-12 | Für den Mahlzeitvorschlag muss das Backend dem KI-Dienst das offene Budget und die Liste der häufig gegessenen Lebensmittel mit Nährwerten übergeben; die KI liefert Lebensmittel-IDs und Mengen, die App berechnet die kcal. | US-14 | Should |
| FR-13 | Das System muss ein Konto mit allen zugehörigen Einträgen und eigenen Lebensmitteln endgültig löschen können. | US-22 | Must |
| FR-14 | Die App muss EAN-13- und EAN-8-Barcodes per Kamera lesen und das Produkt über den Barcode in der externen Lebensmitteldatenbank abfragen. | US-09 | Must |

## 5. Nichtfunktionale Anforderungen

| ID | Kategorie | Anforderung | Prüfung |
|---|---|---|---|
| NFR-01 | Datenschutz | Fotos werden nur zur Analyse an den KI-Dienst übertragen und im Backend **nicht gespeichert**. Es wird nur ein KI-Anbieter gewählt, der vertraglich zusichert, API-Daten nicht zum Training zu verwenden und höchstens befristet zu speichern. Ernährungsdaten werden ausschließlich in der EU gespeichert. Grundlage: DSGVO (Ernährungsdaten können Rückschlüsse auf Gesundheit zulassen). | Code-Review Backend (kein Speichern von Bilddaten); Nutzungsbedingungen des Anbieters bei der Auswahl in Phase 2 dokumentieren |
| NFR-02 | Sicherheit | Der API-Schlüssel des KI-Dienstes liegt ausschließlich im Backend. Die gesamte Kommunikation läuft über HTTPS. Backend-Endpunkte sind nur mit gültiger Anmeldung aufrufbar. | Suche nach dem Schlüssel im App-Build; Aufruf der Endpunkte ohne Token → HTTP 401 |
| NFR-03 | Kosten | Pro Nutzer sind höchstens 30 KI-Anfragen pro Tag möglich. Fotos werden vor dem Upload auf max. 1024 px an der längeren Seite verkleinert. | Test: 31. Anfrage wird abgelehnt; Größe des übertragenen Bildes prüfen |
| NFR-04 | Performance | Zielwert: Eine KI-Analyse (Foto + Text) liefert in mindestens 9 von 10 Fällen innerhalb von 15 s ein Ergebnis (WLAN oder 4G). Harte Grenze: Nach 30 s wird abgebrochen und die Fehlermeldung aus US-03 angezeigt. | Messung mit 10 Testfotos |
| NFR-05 | Usability | Eine Mahlzeit mit 3 Komponenten lässt sich per Foto + Text in unter 60 s erfassen, gemessen vom Öffnen der Kamera bis „Speichern", **inklusive** Wartezeit auf die KI und einer Mengenkorrektur. Die Erfassung ist damit schneller als die manuelle Erfassung derselben Mahlzeit über die Suche. | Test mit 3 Personen außerhalb des Teams: beide Wege stoppen |
| NFR-06 | Transparenz | KI-geschätzte Werte sind bis zum Speichern als Schätzung gekennzeichnet. Die App enthält einen Hinweis, dass sie keine Ernährungs- oder Gesundheitsberatung ist. | Sichtprüfung |
| NFR-07 | Plattform | Die App läuft auf Android ab Version 10. iOS wird durch Flutter grundsätzlich unterstützt, im MVP aber nicht getestet. | Installation auf mind. 2 Android-Geräten |
| NFR-08 | Verfügbarkeit | Die App setzt eine Internetverbindung voraus; Offline-Erfassung wird nicht unterstützt. Ohne Verbindung zeigt die App einen Hinweis statt eines Fehlers oder Absturzes; bereits eingegebene Daten im Prüfdialog (US-06) gehen nicht verloren, solange die App geöffnet bleibt. | Test im Flugmodus |

## 6. Annahmen und offene Punkte

- **Genauigkeit der KI-Erkennung** wird nicht als Anforderung formuliert, da sie nicht zuverlässig messbar ist und vom KI-Anbieter abhängt. Stattdessen wird sichergestellt, dass der Nutzer jeden Wert vor dem Speichern prüfen und korrigieren kann (US-06).
- **Auswahl des KI-Dienstes und der Datenbank** erfolgt in Phase 2. Voraussetzung: Bildverarbeitung, strukturierte Ausgabe (JSON), Datenverarbeitung DSGVO-konform. Open Food Facts ist als Datenbank vorgesehen.
- **Qualität öffentlicher Nutzer-Lebensmittel** (US-11): Wie falsche Einträge verhindert werden, ist offen. Deshalb ist die Story Could; Moderation ist Won't.
