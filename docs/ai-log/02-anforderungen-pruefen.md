# AI-Einsatz 2: Anforderungen auf Widersprüche, Lücken und Unklarheiten prüfen

- **Datum:** 08.11.2026
- **Modell:** Claude Opus 5.5 (Claude, Agent-Sitzung)
- **Eingabe:** `docs/requirements.md` in der ersten vollständigen Fassung (nach Überarbeitung der User Stories aus [AI-Einsatz 1](01-user-stories-generierung.md)) sowie `README.md`
- **Zweck:** Unabhängige Prüfung der fertigen Anforderungen, bevor MVP-Scope und Abgabe finalisiert werden. Die Ausgabe unten ist **unverändert**. Die Bewertung durch das Team folgt am Ende.

## Prompt

```
Du bist ein kritischer Reviewer für Software-Anforderungen. Im Anhang findest du
README.md und requirements.md eines Studierendenprojekts (Flutter-App zum
Kalorien-Tracking mit KI-Fotoerfassung, 3 Personen, 1 Semester).

Prüfe die Anforderungen auf:
1. Widersprüche zwischen Stories, Akzeptanzkriterien, FR, NFR und Begriffen
2. Lücken: fehlende Fälle, Fehlerfälle, Anforderungen ohne zugehörige Story
3. Unklarheiten: Formulierungen, die nicht eindeutig testbar sind
4. Priorisierung: Hängen Must-Anforderungen von Should/Could ab? Sind Musts
   wirklich unverzichtbar?

Ausgabeformat: nummerierte Liste. Pro Fund: Kategorie, betroffene IDs,
Beschreibung des Problems, konkreter Verbesserungsvorschlag, Schweregrad
(hoch/mittel/niedrig). Erfinde keine Probleme – wenn etwas in Ordnung ist,
lass es weg. Keine Einleitung, keine Zusammenfassung.
```

## Rohausgabe (unverändert)

**1. Widerspruch · Begriffe „Tag" ↔ US-13 · hoch**
Laut Begriffsdefinition gehört ein Eintrag zu dem Tag, an dem er gespeichert wird. US-13 verlangt aber, Mahlzeiten für frühere Tage nachzutragen. Das ist mit der Definition nicht möglich.
*Vorschlag:* Jeder Eintrag erhält ein Datum, das standardmäßig „heute" ist und beim Nachtragen gewählt werden kann.

**2. Widerspruch · US-04 AK2 ↔ Begriff „Position" · hoch**
US-04 AK2 übernimmt Mengen in „g oder ml" exakt, eine Position hat laut Begriffen aber nur eine Menge in Gramm. Für Flüssigkeiten wie Öl ist ml ≠ g (Dichte ca. 0,92). Ebenso ist unklar, was mit „1 EL Olivenöl" aus AK3 passiert.
*Vorschlag:* Exakte Übernahme auf Grammangaben beschränken. Andere Einheiten (ml, EL, Stück) rechnet die KI in g um; diese Mengen gelten als Schätzung.

**3. Unklarheit · US-04 AK2/AK3, FR-05 · hoch**
Die Akzeptanzkriterien einer Must-Story hängen vom Verhalten eines nicht-deterministischen Sprachmodells ab. Ein einzelner Testlauf kann zufällig bestehen oder scheitern. „In Tests mit festen Testfotos geprüft" legt weder Anzahl der Durchläufe noch Bestehensgrenze fest.
*Vorschlag:* Testmenge und Schwelle festlegen (z. B. 10 Durchläufe je Testfall, mind. 9 bestanden). Alternativ die Grammangaben aus dem Text deterministisch in der App/im Backend nachbearbeiten.

**4. Lücke · US-14, Begriff „häufig gegessenes Lebensmittel" · hoch**
„Häufig gegessen" setzt voraus, dass dasselbe Lebensmittel über mehrere Einträge hinweg wiedererkannt wird. KI-Positionen haben aber nur einen frei generierten Namen („Reis", „Basmatireis", „gekochter Reis") und keine Datenbank-ID. US-14 AK1 („aus den gespeicherten Nährwerten") ist für solche Positionen ebenfalls unklar.
*Vorschlag:* Festlegen, wie Lebensmittel identifiziert werden: z. B. nur Positionen mit Datenbank-ID zählen, oder KI-Positionen beim Speichern einem Datenbankeintrag zuordnen.

**5. Lücke · NFR-01 · mittel**
NFR-01 fordert, dass Nutzer ihr Konto inklusive aller Daten löschen können. Dafür gibt es weder eine User Story noch eine funktionale Anforderung, und die Priorität ist unklar.
*Vorschlag:* Eigene Story/FR „Konto löschen" mit Priorität ergänzen und NFR-01 auf die nichtfunktionalen Aspekte beschränken.

**6. Lücke · US-03, NFR-03 · mittel**
NFR-03 begrenzt KI-Anfragen auf 30 pro Tag. Was der Nutzer beim Erreichen des Limits sieht, ist nicht beschrieben.
*Vorschlag:* In US-03 AK3 als weiteren Fehlerfall aufnehmen.

**7. Priorisierung · US-12 AK4 → US-20 · mittel**
Die Must-Story US-12 enthält ein Akzeptanzkriterium, das sich auf die Should-Story US-20 bezieht. Damit ist unklar, ob US-12 ohne US-20 als erfüllt gilt.
*Vorschlag:* AK4 aus US-12 entfernen und als Kriterium zu US-20 verschieben.

**8. Priorisierung · US-08 AK1 → US-10 · niedrig**
Gleiches Muster: Must-Story US-08 verweist auf Should-Story US-10.
*Vorschlag:* Verweis entfernen; Anzeige eigener Lebensmittel in der Suche als Kriterium von US-10 formulieren.

**9. Unklarheit · NFR-01 · mittel**
„sofern konfigurierbar" macht die Anforderung unprüfbar: Wenn der Anbieter keine Konfiguration erlaubt, ist sie automatisch erfüllt.
*Vorschlag:* Als Auswahlkriterium für den KI-Anbieter formulieren (Anbieter muss zusichern, API-Daten nicht zum Training zu verwenden und nur befristet zu speichern).

**10. Unklarheit · NFR-05 · niedrig**
„Zum Vergleich: manuelle Suche derselben Mahlzeit" ist kein prüfbares Kriterium. Unklar ist auch, ob die 60 s die Wartezeit der KI (NFR-04, bis 15 s) enthalten.
*Vorschlag:* Vergleich entweder streichen oder messbar machen (z. B. „schneller als die manuelle Erfassung derselben Mahlzeit"). Klarstellen, dass die Wartezeit enthalten ist.

**11. Widerspruch · NFR-04 ↔ US-03 AK3 · niedrig**
NFR-04 nennt 15 s, US-03 AK3 einen Abbruch nach 30 s. Zwei verschiedene Zeitgrenzen für dieselbe Aktion.
*Vorschlag:* Einen einzigen Wert verwenden.

**12. Lücke · US-01 · mittel**
Es gibt keine Möglichkeit, ein vergessenes Passwort zurückzusetzen.
*Vorschlag:* Story „Passwort zurücksetzen" ergänzen.

**13. Lücke · nichtfunktional · niedrig**
Barrierefreiheit (Schriftgrößen, Screenreader) und Offline-Verhalten sind nicht adressiert. Für eine Tracking-App, die unterwegs verwendet wird, ist Offline-Erfassung relevant.
*Vorschlag:* NFR zu Offline-Verhalten und Barrierefreiheit ergänzen.

**14. Unklarheit · US-02 AK1 · niedrig**
Die Grenzen 800–6000 kcal sind nicht begründet.
*Vorschlag:* Begründung ergänzen oder Grenzen entfernen.

## Bewertung durch das Team

| # | Entscheidung | Begründung / Änderung |
|---|---|---|
| 1 | **übernommen** | Echter Widerspruch, den wir übersehen hatten. Begriff „Tag" geändert: Eintrag hat ein Datum, Standard „heute", in US-13 wählbar. |
| 2 | **übernommen** | Richtig. US-04 AK2 auf Grammangaben beschränkt; andere Einheiten rechnet die KI um, die Position gilt dann als Schätzung. Testfall in AK3 angepasst. |
| 3 | **übernommen (erste Variante)** | Wichtigster Fund: Ein Must-Kriterium, das an einem LLM hängt, ist sonst nicht reproduzierbar testbar. Wir legen 5 feste Testfälle × 10 Durchläufe mit mind. 9 bestandenen fest (FR-05). Die deterministische Nachbearbeitung wäre robuster, aber das Parsen beliebiger Freitexte ist für das MVP zu aufwendig. Falls die Schwelle in Phase 2 nicht erreicht wird, ist sie die Rückfallebene. |
| 4 | **übernommen (erste Variante)** | Berechtigte Lücke. „Häufig gegessen" zählt nur Positionen mit Datenbank-ID. Damit KI-Positionen trotzdem einfließen, kann der Nutzer in US-06 eine KI-Position durch einen Datenbankeintrag ersetzen (neues AK). Eine automatische Zuordnung wäre komfortabler, ist aber zusätzlicher Aufwand für eine Should-Story. |
| 5 | **übernommen** | Neue Story US-22 „Konto löschen" und FR-13, NFR-01 entsprechend gekürzt. Zunächst als Should eingestuft (Kernanwendungsfall auch ohne vorführbar). Später vom Team auf **Must** hochgestuft: Da echte Testpersonen die App nutzen (NFR-05), müssen sie ihr Recht auf Löschung nach DSGVO selbst ausüben können. |
| 6 | **übernommen** | Als Fehlerfall in US-03 AK3 ergänzt. |
| 7 | **übernommen** | AK4 nach US-20 verschoben. |
| 8 | **übernommen** | Verweis entfernt, Kriterium bei US-10 ergänzt. |
| 9 | **übernommen** | NFR-01 als Auswahlkriterium für den Anbieter umformuliert. |
| 10 | **übernommen** | Vergleich messbar gemacht, Wartezeit ausdrücklich eingeschlossen. |
| 11 | **verworfen** | Kein Widerspruch: 15 s ist das Ziel für 9 von 10 Anfragen, 30 s die harte Abbruchgrenze für Ausreißer. Das sind zwei verschiedene Anforderungen. Wir haben NFR-04 aber umformuliert, damit der Unterschied klarer wird. |
| 12 | **teilweise übernommen** | Lücke ist berechtigt, aber für den Prototyp mit Testnutzern nicht nötig (Reset durch das Team). Als *Won't* in [mvp.md](../mvp.md) dokumentiert statt als neue Story. |
| 13 | **teilweise übernommen** | Offline-Verhalten als NFR-08 ergänzt, aber nur als Festlegung, dass Offline-Erfassung **nicht** unterstützt wird und was dann passiert. Offline-Synchronisation ist für ein Semester zu aufwendig (Won't). Barrierefreiheit verworfen: als allgemeiner Anspruch sinnvoll, aber ohne konkreten Bezug zu unserem Problem und ohne Kapazität für Tests. |
| 14 | **verworfen, Begründung ergänzt** | Die Grenzen sind bewusst gesetzt: Sie fangen Tippfehler ab (z. B. 250 statt 2500). Die Untergrenze verhindert zusätzlich extrem niedrige Ziele, da die App keine Ernährungsberatung ist. Begründung steht jetzt in US-02. |

**Fazit:** 10 von 14 Funden vollständig übernommen, 2 teilweise, 2 verworfen. Besonders wertvoll waren die Funde 1–4: Sie betreffen Zusammenhänge *zwischen* Dokumentteilen (Begriffe ↔ Stories, Must ↔ LLM-Verhalten), die beim Schreiben einzelner Abschnitte leicht übersehen werden. Die verworfenen bzw. nur teilweise übernommenen Funde (11–14) sind typische „Vollständigkeits"-Vorschläge, die das Dokument größer, aber nicht besser für unser Problem gemacht hätten.
