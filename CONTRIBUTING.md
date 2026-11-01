# Zu Flunar-Locals beitragen

Vielen Dank, dass du die Übersetzungen der Flunar-Plugins verbessern möchtest. Beiträge sind über Pull Requests willkommen. Zum Übersetzen musst du das jeweilige Plugin nicht programmieren können.

Bitte lies diese Richtlinien vollständig, bevor du Änderungen einreichst.

## Wichtigste Beitragsregel

Ein Pull Request darf Änderungen für **genau ein Plugin** und **genau einen Zielbereich** enthalten:

- entweder den allgemeinen Fallback unter `fallback/<Plugin-ID>/`,
- oder ein einzelnes Serverprofil unter `profiles/<Profil>/<Plugin-ID>/`.

Änderungen an mehreren Plugins, Profilen oder Zielbereichen müssen in getrennten Pull Requests eingereicht werden. Das gilt auch dann, wenn dieselbe Sprache für mehrere Plugins ergänzt werden soll.

## Mögliche Beiträge

Du kannst insbesondere:

- eine neue Sprache hinzufügen,
- bestehende Übersetzungen korrigieren oder verbessern,
- fehlende Texte ergänzen,
- profilbezogene Formulierungen hinzufügen oder berichtigen,
- Fehler in den zugehörigen Einträgen der `manifest.yml` beheben.

## Anforderungen an Übersetzungen

- Übersetze nur die sichtbaren Texte, niemals die Nachrichtenschlüssel.
- Bewahre Einrückungen, YAML-Struktur, Anführungszeichen und Zeilenumbrüche.
- Verändere oder entferne keine Platzhalter, Formatierungen oder Sonderzeichen.
- Gib die Bedeutung des Ausgangstextes sachlich und vollständig wieder.
- Verwende einheitliche Begriffe innerhalb eines Plugins.
- Prüfe maschinell erstellte Übersetzungen sorgfältig. Ungeprüfte automatische Übersetzungen werden nicht übernommen.
- Beleidigende, diskriminierende, belästigende, versteckte oder sachfremde Inhalte sind nicht gestattet.

## Fallback oder Serverprofil?

Allgemein gültige Übersetzungen gehören nach:

```text
fallback/<Plugin-ID>/locale_<Sprachcode>.yml
```

Serverbezogene Abweichungen gehören nach:

```text
profiles/<Profil>/<Plugin-ID>/locale_<Sprachcode>.yml
```

Ein Serverprofil soll ausschließlich die Texte enthalten, die vom allgemeinen Fallback abweichen. Alle nicht angegebenen Dateien und Nachrichtenschlüssel werden aus dem Fallback übernommen.

## Eine neue Sprache hinzufügen

1. Wähle genau ein Plugin und einen Zielbereich.
2. Verwende eine vorhandene Sprachdatei desselben Plugins als Vorlage.
3. Erstelle die neue Datei nach dem Muster `locale_<Sprachcode>.yml`, beispielsweise `locale_fr.yml`.
4. Übersetze alle sichtbaren Texte und erhalte Schlüssel, Platzhalter sowie Formatierungen.
5. Ergänze ausschließlich den zugehörigen Eintrag in der `manifest.yml`.
6. Trage Plugin, Sprachcode, Dateipfad und SHA-256-Prüfsumme korrekt ein.

Falls du die SHA-256-Prüfsumme nicht selbst ermitteln kannst, erwähne das deutlich in der Beschreibung des Pull Requests. Die Maintainer können sie während der Prüfung ergänzen.

## Ablauf eines Beitrags

1. Erstelle einen Fork dieses Repositorys.
2. Erstelle in deinem Fork einen eigenen Branch, beispielsweise `translation/fr-flunar-core`.
3. Nimm die Änderungen gemäß diesen Richtlinien vor.
4. Prüfe die YAML-Dateien und die zugehörigen Manifest-Einträge.
5. Erstelle einen verständlichen Commit, beispielsweise `Französische Core-Übersetzung hinzufügen`.
6. Öffne einen Pull Request gegen den Hauptbranch dieses Repositorys.

Ändere das Original-Repository nicht direkt. Beiträge sollen über einen Fork und einen Pull Request eingereicht werden.

## Pull-Request-Beschreibung

Beschreibe im Pull Request kurz:

- das betroffene Plugin,
- die Sprache,
- den Zielbereich (`fallback` oder genau ein Profil),
- den Inhalt beziehungsweise Grund der Änderung,
- noch offene Punkte oder notwendige Prüfungen.

## Checkliste

Bitte prüfe vor dem Absenden:

- [ ] Der Pull Request betrifft genau ein Plugin.
- [ ] Der Pull Request betrifft genau einen Zielbereich.
- [ ] Alle sichtbaren Texte sind vollständig und sinngemäß übersetzt.
- [ ] Nachrichtenschlüssel, Platzhalter und Formatierungen wurden beibehalten.
- [ ] Die YAML-Struktur und Einrückungen sind gültig.
- [ ] Allgemeine Texte befinden sich im Fallback und nur echte Abweichungen im Profil.
- [ ] Neue oder geänderte Dateien sind korrekt in der `manifest.yml` eingetragen.
- [ ] Dateipfad, Sprachcode und SHA-256-Prüfsumme stimmen.
- [ ] Der Beitrag enthält keine sachfremden oder unangemessenen Inhalte.
- [ ] Ich habe den Abschnitt „Lizenz für Beiträge“ gelesen und akzeptiert.

## Prüfung und Übernahme

Die Maintainer prüfen jeden Pull Request vor der Übernahme. Sie können Änderungen verlangen, Rückfragen stellen oder Beiträge ablehnen beziehungsweise schließen, wenn sie diesen Richtlinien nicht entsprechen.

Auch unvollständige Beiträge können eingereicht werden, wenn in der Pull-Request-Beschreibung eindeutig erklärt wird, was noch fehlt oder geprüft werden muss.

## Lizenz für Beiträge

Dieses Repository steht unter der [MIT-Lizenz](LICENSE).

Mit dem Einreichen eines Pull Requests bestätigst du, dass:

1. du den Beitrag selbst erstellt hast oder zur Einreichung berechtigt bist,
2. der Beitrag keine Rechte Dritter verletzt,
3. du deinen Beitrag unter der MIT-Lizenz dieses Repositorys zur Verfügung stellst und
4. der Beitrag von Flunar.de und anderen Personen gemäß der MIT-Lizenz verwendet, verändert und weitergegeben werden darf.

Wenn du mit diesen Bedingungen nicht einverstanden bist, reiche bitte keinen Beitrag ein.

## Fragen

Bei Fragen kannst du diese direkt in deinem Pull Request stellen.
