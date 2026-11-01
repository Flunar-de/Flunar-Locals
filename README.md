# Flunar-Locals

Dieses Repository sammelt Übersetzungen für die Flunar-Plugins. Spieler und andere Community-Mitglieder können neue Sprachen beitragen oder bestehende Texte verbessern. Änderungen werden als Pull Request vorgeschlagen und vor der Übernahme geprüft.

Zum Übersetzen musst du das Plugin nicht programmieren. Wichtig ist, die vorhandenen Nachrichtenschlüssel und die Struktur der Dateien beizubehalten.

## Fallbacks und Serverprofile

Der Ordner `fallback/` enthält die allgemeinen Übersetzungen eines Plugins. Sie gelten auf allen Servern, solange ein Serverprofil keine abweichende Übersetzung bereitstellt.

Ein Profil ist nur nötig, wenn ein Servertyp eigene Formulierungen braucht. Solche Abweichungen liegen unter `profiles/<Profil>/<Plugin-ID>/`. Ein Profil muss nicht alle Texte enthalten: Nicht angegebene Dateien und Nachrichtenschlüssel werden aus dem Fallback übernommen. Wenn ein Profil keine eigenen Übersetzungen braucht, verwendet das Plugin direkt `fallback/`.

Jedes Plugin hat einen eigenen Ordner. Aktuell liegen die Core-Dateien unter `fallback/Flunar-Core/` und die Lobby-System-Dateien unter `fallback/Flunar-LobbySystem/`. Für beide Plugins sind derzeit Deutsch und Englisch vorhanden. Die Lobby verwendet ihre Fallback-Dateien; eigene Lobby-Profilübersetzungen gibt es aktuell nicht.

## Eine allgemeine Sprache hinzufügen

Neue Sprachen gehören in den Fallback, damit sie grundsätzlich auf allen Servern verfügbar sind.

1. Öffne im Repository für jedes betroffene Plugin eine vorhandene Fallback-Datei in der gewünschten Sprache als Vorlage. Wenn es diese Sprache noch nicht gibt, nimm eine vorhandene Sprache desselben Plugins.
2. Lege im Fallback-Ordner des Plugins eine Datei für die neue Sprache an. Der Dateiname folgt dem Muster `locale_<Sprachcode>.yml`, zum Beispiel `locale_fr.yml` für Französisch.
3. Übersetze die sichtbaren Texte. Behalte die Nachrichtenschlüssel, Einrückungen, Platzhalter, Formatierungen und Sonderzeichen bei. Übersetze keine Schlüssel und entferne keine Platzhalter.
4. Ergänze die neue Datei im `manifest.yml`. Das Manifest ist das Verzeichnis, über das die Core die Übersetzungen findet; für jede Datei müssen Plugin, Sprachcode, Dateipfad und die passende SHA-256-Prüfsumme eingetragen sein.
5. Wenn du die Prüfsumme nicht selbst ermitteln kannst, erwähne das im Pull Request. Die Maintainer können den Manifest-Eintrag beim Prüfen vervollständigen.

Eine Sprache muss für jedes Plugin ergänzt werden, dessen Texte in dieser Sprache angezeigt werden sollen. Übersetze nur Plugins, deren Texte du zuverlässig prüfen kannst, und liste im Pull Request alle bearbeiteten Plugins auf.

## Eine Übersetzung für ein Serverprofil hinzufügen

Ein Profil ist für serverbezogene Abweichungen gedacht, zum Beispiel für eine besondere Lobby, ein Event oder einen Spielmodus. Allgemeine Texte und Übersetzungen bleiben im Fallback.

1. Wähle einen kurzen, eindeutigen Namen für das Serverprofil.
2. Lege nur für betroffene Plugins Übersetzungsdateien unter `profiles/<Profil>/<Plugin-ID>/` an.
3. Übernimm nur die Nachrichtenschlüssel, deren Text sich auf diesem Profil vom allgemeinen Fallback unterscheiden soll. Alle übrigen Texte werden aus dem Fallback verwendet.
4. Trage die neuen Dateien und das Profil im `manifest.yml` ein. Jede Datei braucht den richtigen Plugin-Namen, Sprachcode, Pfad und SHA-256-Prüfsumme. Wenn du Hilfe mit der Prüfsumme brauchst, schreib es in den Pull Request.

Ein Profil ohne eigene Abweichungen muss nicht angelegt werden. In diesem Fall reicht der Fallback.

## Änderungen über GitHub vorschlagen

1. Öffne dieses Repository auf GitHub und wähle **Fork**, um eine eigene Kopie unter deinem GitHub-Konto anzulegen.
2. Erstelle in deinem Fork einen Branch für deine Übersetzung.
3. Füge die Sprachdateien hinzu oder verbessere bestehende Übersetzungen. Passe bei neuen Dateien auch das Manifest an oder notiere im Pull Request, dass die Prüfsumme noch ergänzt werden muss.
4. Speichere deine Änderungen mit einem aussagekräftigen Commit, zum Beispiel „Französische Lobby-Übersetzung hinzufügen“.
5. Erstelle über GitHub einen Pull Request von deinem Branch in den Hauptbranch dieses Repositorys.
6. Beschreibe kurz die Sprache, die betroffenen Plugins und gegebenenfalls das Serverprofil. Beantworte Rückfragen und passe die Änderungen an, falls die Maintainer darum bitten.

Bitte ändere das Original-Repository nicht direkt. Der Pull Request gibt den Maintainer:innen Gelegenheit, Vollständigkeit, Formatierung und Manifest-Einträge gemeinsam mit dir zu prüfen.

## Vor dem Pull Request prüfen

- Sind alle sichtbaren Texte übersetzt und ist die Bedeutung erhalten geblieben?
- Stimmen Nachrichtenschlüssel, Platzhalter und Formatierungen mit der Vorlage überein?
- Liegen allgemeine Texte im Fallback und nur echte Abweichungen im Serverprofil?
- Sind alle neu hinzugefügten Dateien im Manifest aufgeführt?
- Sind Plugin, Sprachcode, Dateipfad und SHA-256-Prüfsumme für jeden Manifest-Eintrag korrekt?

Bei Fragen kannst du sie direkt im Pull Request stellen. Auch unvollständige Übersetzungsbeiträge sind willkommen, wenn im Pull Request klar steht, welche Texte oder Dateien noch geprüft werden müssen.
