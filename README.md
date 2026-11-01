# Flunar-Locals

[English version](README.en.md)

Dieses Repository sammelt Übersetzungen für die Flunar-Plugins. Spieler und andere Community-Mitglieder können neue Sprachen beitragen oder bestehende Texte verbessern. Änderungen werden als Pull Request vorgeschlagen und vor der Übernahme geprüft.

Zum Übersetzen musst du das Plugin nicht programmieren. Wichtig ist, die vorhandenen Nachrichtenschlüssel und die Struktur der Dateien beizubehalten.

## Wichtige Beitragsregel

**Ein Pull Request darf Übersetzungen nur für ein Plugin und genau einen Zielbereich enthalten: entweder den allgemeinen Fallback oder ein einzelnes Serverprofil.** Änderungen an mehreren Plugins oder Profilen müssen in getrennten Pull Requests eingereicht werden. Das gilt auch, wenn dieselbe neue Sprache für mehrere Plugins ergänzt werden soll. Passe im `manifest.yml` nur Einträge an, die zu diesem Plugin und Zielbereich gehören.

Diese Trennung macht jeden Beitrag einzeln prüfbar. Übersetzungen müssen den vorhandenen Text sachlich und vollständig wiedergeben. Beleidigungen, diskriminierende Inhalte, Belästigungen, versteckte Nachrichten und sachfremde Texte werden abgelehnt. Die Maintainer prüfen jeden Pull Request vor dem Merge und können Änderungen anfordern oder Beiträge schließen.

## Fallbacks und Serverprofile

Der Ordner `fallback/` enthält die allgemeinen Übersetzungen eines Plugins. Sie gelten auf allen Servern, solange ein Serverprofil keine abweichende Übersetzung bereitstellt.

Ein Profil ist nur nötig, wenn ein Servertyp eigene Formulierungen braucht. Solche Abweichungen liegen unter `profiles/<Profil>/<Plugin-ID>/`. Ein Profil muss nicht alle Texte enthalten: Nicht angegebene Dateien und Nachrichtenschlüssel werden aus dem Fallback übernommen. Wenn ein Profil keine eigenen Übersetzungen braucht, verwendet das Plugin direkt `fallback/`.

Jedes Plugin hat einen eigenen Ordner. Aktuell liegen die Core-Dateien unter `fallback/Flunar-Core/` und die Lobby-System-Dateien unter `fallback/Flunar-LobbySystem/`. Für beide Plugins sind derzeit Deutsch und Englisch vorhanden. Die Lobby verwendet ihre Fallback-Dateien; eigene Lobby-Profilübersetzungen gibt es aktuell nicht. Für Flunar-Claims liegen deutsche und englische Fallback-Übersetzungen unter `fallback/Flunar-Claims/`; ein eigenes Serverprofil gibt es dafür nicht.

## Eine allgemeine Sprache hinzufügen

Neue Sprachen gehören in den Fallback, damit sie grundsätzlich auf allen Servern verfügbar sind.

1. Wähle genau ein Plugin und öffne darin eine vorhandene Fallback-Datei als Vorlage. Wenn es die gewünschte Sprache noch nicht gibt, nimm eine vorhandene Sprache desselben Plugins.
2. Lege im Fallback-Ordner des Plugins eine Datei für die neue Sprache an. Der Dateiname folgt dem Muster `locale_<Sprachcode>.yml`, zum Beispiel `locale_fr.yml` für Französisch.
3. Übersetze die sichtbaren Texte. Behalte die Nachrichtenschlüssel, Einrückungen, Platzhalter, Formatierungen und Sonderzeichen bei. Übersetze keine Schlüssel und entferne keine Platzhalter.
4. Ergänze die neue Datei im `manifest.yml`. Das Manifest ist das Verzeichnis, über das die Core die Übersetzungen findet; für jede Datei müssen Plugin, Sprachcode, Dateipfad und die passende SHA-256-Prüfsumme eingetragen sein.
5. Ergänze im Manifest nur den Eintrag für dieses Plugin und diese Sprache. Wenn du die Prüfsumme nicht selbst ermitteln kannst, erwähne das im Pull Request. Die Maintainer können sie beim Prüfen vervollständigen.

Soll dieselbe Sprache auch für ein weiteres Plugin angeboten werden, reiche dafür einen separaten Pull Request ein.

## Eine Übersetzung für ein Serverprofil hinzufügen

Ein Profil ist für serverbezogene Abweichungen gedacht, zum Beispiel für eine besondere Lobby, ein Event oder einen Spielmodus. Allgemeine Texte und Übersetzungen bleiben im Fallback. Ein Profil-Pull-Request darf nur ein Profil und ein Plugin betreffen.

1. Wähle einen kurzen, eindeutigen Namen für das Serverprofil.
2. Lege nur für dieses Plugin Übersetzungsdateien unter `profiles/<Profil>/<Plugin-ID>/` an.
3. Übernimm nur die Nachrichtenschlüssel, deren Text sich auf diesem Profil vom allgemeinen Fallback unterscheiden soll. Alle übrigen Texte werden aus dem Fallback verwendet.
4. Trage die neuen Dateien und das Profil im `manifest.yml` ein. Jede Datei braucht den richtigen Plugin-Namen, Sprachcode, Pfad und SHA-256-Prüfsumme. Wenn du Hilfe mit der Prüfsumme brauchst, schreib es in den Pull Request.

Ein Profil ohne eigene Abweichungen muss nicht angelegt werden. In diesem Fall reicht der Fallback.

## Änderungen über GitHub vorschlagen

1. Öffne dieses Repository auf GitHub und wähle **Fork**, um eine eigene Kopie unter deinem GitHub-Konto anzulegen.
2. Erstelle in deinem Fork einen Branch für deine Übersetzung.
3. Füge die Sprachdateien hinzu oder verbessere bestehende Übersetzungen. Passe bei neuen Dateien auch das Manifest an oder notiere im Pull Request, dass die Prüfsumme noch ergänzt werden muss.
4. Speichere deine Änderungen mit einem aussagekräftigen Commit, zum Beispiel „Französische Lobby-Übersetzung hinzufügen“.
5. Erstelle über GitHub einen Pull Request von deinem Branch in den Hauptbranch dieses Repositorys.
6. Beschreibe kurz das eine Plugin, die Sprache und den Zielbereich (Fallback oder Profil). Beantworte Rückfragen und passe die Änderungen an, falls die Maintainer darum bitten.

Bitte ändere das Original-Repository nicht direkt. Die Maintainer prüfen vor dem Merge, ob der Pull Request die Beitragsregel einhält, die Übersetzung vollständig und angemessen ist und die Manifest-Einträge stimmen. Änderungen an weiteren Plugins oder Profilen werden abgelehnt oder müssen als separate Pull Requests eingereicht werden.

## Vor dem Pull Request prüfen

- Sind alle sichtbaren Texte übersetzt und ist die Bedeutung erhalten geblieben?
- Stimmen Nachrichtenschlüssel, Platzhalter und Formatierungen mit der Vorlage überein?
- Liegen allgemeine Texte im Fallback und nur echte Abweichungen im Serverprofil?
- Betrifft der Pull Request genau ein Plugin und genau einen Zielbereich?
- Enthält die Übersetzung keine Beleidigungen, diskriminierenden Inhalte, versteckten Nachrichten oder sachfremden Texte?
- Sind alle neu hinzugefügten Dateien im Manifest aufgeführt?
- Sind Plugin, Sprachcode, Dateipfad und SHA-256-Prüfsumme für die zugehörigen Manifest-Einträge korrekt?

Bei Fragen kannst du sie direkt im Pull Request stellen. Auch unvollständige Übersetzungsbeiträge sind willkommen, wenn im Pull Request klar steht, welche Texte oder Dateien noch geprüft werden müssen.
