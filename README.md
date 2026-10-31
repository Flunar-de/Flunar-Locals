# Flunar-Locals

Dieses Repository ist die gemeinsame Quelle für Sprachdateien der Flunar-Plugins. Die Flunar-Core wählt pro Server ein Profil aus und stellt die Texte über ihre Locale-API bereit. Jedes Plugin liefert seine eigenen Nachrichtenschlüssel und unterstützten Sprachen.

**Aktueller Stand:** Die drei Core-Dateien `locale.yml`, `locale_de.yml` und `locale_en.yml` liegen unter `fallback/Flunar-Core/`. Die deutschen und englischen Lobby-System-Dateien wurden aus dem Plugin-Projekt in `profiles/lobby/Flunar-LobbySystem/` kopiert. Beide Quellprojekte bleiben unverändert. Die Lobby-Profil-Dateien sind zunächst vollständige Kopien; ein allgemeiner Lobby-System-Fallback und weitere Profile fehlen noch.

## Aufbau

```text
Flunar-Locals/
├── manifest.yml
├── fallback/
│   ├── Flunar-Core/
│   │   ├── locale.yml
│   │   ├── locale_de.yml
│   │   └── locale_en.yml
└── profiles/
    ├── lobby/
    │   └── Flunar-LobbySystem/
    │       ├── locale_de.yml
    │       └── locale_en.yml
```

Für das Lobby-System sind aktuell nur die Lobby-Profil-Dateien vorhanden. Weitere Serverprofile und Plugin-Fallbacks werden ergänzt, wenn deren Dateien bereitgestellt werden.

### Fallback

`fallback/` enthält die allgemeinen Texte. Für jedes Plugin und jede Sprache, die das Netzwerk anbietet, sollte dort eine vollständige Standarddatei liegen.

Aktuell enthält dieser Ordner nur Flunar-Core. Das Lobby-System verwendet seine gebündelten Plugin-Ressourcen als allgemeinen Fallback, bis `fallback/Flunar-LobbySystem/` separat hinzugefügt wird.

Für Flunar-Core gibt es zusätzlich `fallback/Flunar-Core/locale.yml`. Diese Datei ist nicht an eine bestimmte Sprache gebunden und wird im Manifest mit `language: default` als letzte Core-Fallback-Datei geführt.

### Serverprofile

`profiles/<Profil>/<Plugin-ID>/` enthält nur die Texte, die von der allgemeinen Fassung abweichen. Profile wie `lobby`, `survival` oder `minigame` werden pro Server einmal in der Core eingestellt.

Fehlt ein Profil im Repository, greift `fallback/`. Fehlt in einem vorhandenen Profil eine Plugin-Datei, Sprachdatei oder ein einzelner Schlüssel, wird der entsprechende Eintrag aus dem Fallback verwendet. Ein neu eingerichteter Server kann daher starten, bevor ein eigenes Profil existiert.

Die Ordnernamen der Plugins müssen exakt dem `name` in ihrer `plugin.yml` oder `paper-plugin.yml` entsprechen. Beispiele sind `Flunar-Core` und `Flunar-LobbySystem`.

## Manifest

`manifest.yml` beschreibt, welche Dateien zu welchem Profil, Plugin und Sprachcode gehören. Die Core sollte die GitHub-Ordner nicht selbst durchsuchen. Ein vereinfachtes Beispiel:

```yaml
schema-version: 1

profiles:
  fallback:
    files:
      - plugin: Flunar-Core
        language: default
        path: fallback/Flunar-Core/locale.yml
        sha256: d3493de225700bb39751f1aafab9b0d0ef0c84bcd4a39ea780563b206bc996dc
      - plugin: Flunar-Core
        language: de
        path: fallback/Flunar-Core/locale_de.yml
        sha256: f0d847db6873732ee1dc761273ed42b128ed0f9a29fdc28b49ecb02105260db4
      - plugin: Flunar-Core
        language: en
        path: fallback/Flunar-Core/locale_en.yml
        sha256: 234fa6357b7c5b84540d94e4df7b97cb4662f3b1fc353db2c08c00d5ad673e9e

  lobby:
    extends: fallback
    files:
      - plugin: Flunar-LobbySystem
        language: de
        path: profiles/lobby/Flunar-LobbySystem/locale_de.yml
        sha256: 7cad45f1c049807206c2e6e60a485e9f875b262522f61b489421247d153a9121
      - plugin: Flunar-LobbySystem
        language: en
        path: profiles/lobby/Flunar-LobbySystem/locale_en.yml
        sha256: a9c05d2ef9e2d02de850576fc2bcbd5a4e111910bf3bb709658f45dac70caad4
```

Das aktuelle Manifest enthält die Core-Fallback-Dateien und die beiden kopierten Lobby-Profil-Dateien. Der allgemeine Lobby-System-Fallback unter `fallback/Flunar-LobbySystem/` fehlt noch. SHA-256-Werte müssen bei jeder Dateiänderung neu berechnet werden.

## Core pro Server einstellen

Das Serverprofil wird zentral in der Core gesetzt, zum Beispiel in `plugins/Flunar-Core/locale-settings.yml`:

```yaml
RemoteLocales:
  Enabled: true
  ManifestUrl: "https://raw.githubusercontent.com/YOUR_GITHUB_ORG/Flunar-Locals/main/manifest.yml"
  Profile: lobby
```

Ersetze `YOUR_GITHUB_ORG` durch den GitHub-Benutzer oder die Organisation, unter der das Repository liegt. `ManifestUrl` muss direkt auf die Raw-Datei zeigen. Verwende daher `raw.githubusercontent.com` und keinen normalen GitHub-Weblink mit `/blob/`. In einem Link für einen festen Release wird `main` durch den Tag ersetzt, zum Beispiel `v1.2.0`.

Die Core lädt das Manifest über diesen Link. Die Locale-Dateien werden anschließend über die relativen Pfade abgerufen, die im Manifest stehen, zum Beispiel `fallback/Flunar-LobbySystem/locale_de.yml` oder `profiles/lobby/Flunar-LobbySystem/locale_de.yml`.

Die verfügbaren Spielersprachen stehen weiterhin in der `config.yml` der Core:

```yaml
Language:
  DefaultLanguage: de
  AvailableLanguages:
    - de
    - en
```

Fehlt die Profileinstellung oder kennt das Manifest den eingestellten Profilnamen noch nicht, verwendet die Core `fallback`. Sobald das Profil unter der eingestellten Repository-Revision veröffentlicht ist, wird es beim nächsten erfolgreichen Abruf genutzt.

## Plugins an die Locale-API anbinden

Ein Plugin, das Flunar-Core als Locale-System nutzt:

1. Deklariert Flunar-Core als erforderliche Abhängigkeit und lädt danach, damit die API vor seinem `onEnable()` verfügbar ist.
2. Packt gebündelte Standarddateien unter `src/main/resources/locales/` in das Plugin-JAR. Sie dienen als Offline- und Erststart-Fallback.
3. Holt die Locale-API, registriert sich und lädt seine unterstützten Sprachen.
4. Verwendet im Code stabile Nachrichtenschlüssel statt fest eingebauter Spielertexte.
5. Liefert für jede unterstützte Sprache eine Fallback-Datei in diesem Repository.

Beispiel für den Locale-Teil eines Plugin-Starts:

```java
FlunarCore core = (FlunarCore) getServer().getPluginManager().getPlugin("Flunar-Core");
FlunarLocaleAPI localeAPI = core.getLocaleAPI();
localeAPI.registerPlugin(this);
localeAPI.loadPluginLocales(this, "de", "en");
```

Eine Nachricht wird über die API mit dem Plugin-Objekt, Spieler und Schlüssel gelesen:

```java
String title = localeAPI.getMessage(this, player, "quick-menu.language-menu.title");
```

Das Plugin setzt **kein eigenes Serverprofil und keine GitHub-URL**. Die Core wählt das Profil einmal für den Server und kombiniert darin die Locale-Dateien aller Plugins.

### Beispiel: Flunar-LobbySystem

Das Lobby-System heißt in seiner Plugin-Beschreibung `Flunar-LobbySystem`, benötigt Flunar-Core mit früherem Ladezeitpunkt und lädt bereits Deutsch und Englisch mit `loadPluginLocales(this, "de", "en")`. Seine gebündelten Standarddateien sind `src/main/resources/locales/locale_de.yml` und `locale_en.yml`.

Für die zentralen Locals bedeutet das:

- Die gebündelten Lobby-System-Dateien sind nach `profiles/lobby/Flunar-LobbySystem/` kopiert und dort unverändert.
- Ein allgemeiner GitHub-Fallback unter `fallback/Flunar-LobbySystem/` fehlt noch. Bis er ergänzt ist, liefern die im Plugin-JAR enthaltenen Dateien die allgemeinen Texte für andere Profile.
- Sobald der allgemeine Fallback hinzugefügt ist, sollten die Profil-Dateien nur die Lobby-spezifischen Abweichungen enthalten.
- Die Lobby verwendet das in der Core eingestellte Profil. Eine zweite Einstellung in der Lobby-System-Config ist nicht erforderlich.
- Die Core muss `de` und `en` unter `Language.AvailableLanguages` freigeben. Das Lobby-Sprachmenü bietet derzeit genau diese beiden Sprachen an.
- Wird zum Beispiel Französisch ergänzt, müssen Core-Konfiguration, Lobby-Sprachmenü, `loadPluginLocales`-Sprachen, gebündelte Plugin-Datei und Fallback-Datei zusammen angepasst werden.

Zielbild nach Ergänzung des allgemeinen Fallbacks: Die Datei unter `fallback/` enthält den allgemeinen Schlüssel; eine Profil-Datei überschreibt ihn nur für diesen Servertyp. Die folgende Fallback-Datei ist noch nicht angelegt:

```yaml
# fallback/Flunar-LobbySystem/locale_de.yml
quick-menu:
  language-menu:
    title: "<yellow><bold>Sprache ändern"
```

```yaml
# profiles/lobby/Flunar-LobbySystem/locale_de.yml
quick-menu:
  language-menu:
    title: "<gold><bold>Lobby-Sprache"
```

## Neue Texte und Übersetzungen hinzufügen

### Vorhandenen Text übersetzen oder ändern

1. Bearbeite die Sprache in `fallback/<Plugin-ID>/locale_<sprache>.yml`.
2. Wenn der Text nur auf einem Profil anders lauten soll, bearbeite stattdessen oder zusätzlich `profiles/<Profil>/<Plugin-ID>/locale_<sprache>.yml`.
3. Passe den SHA-256-Wert und die Manifest-Revision an.
4. Prüfe, dass alle vom Plugin verwendeten Platzhalter enthalten sind, zum Beispiel `%player%` oder `%language%`.
5. Veröffentliche die Änderung auf dem Ref, den die Core verfolgt.

### Neuen Nachrichtenschlüssel hinzufügen

1. Füge im Plugin-Code einen stabilen Schlüssel hinzu oder verwende einen vorhandenen Schlüssel.
2. Ergänze diesen Schlüssel in der gebündelten Plugin-Standarddatei und in jeder Fallback-Sprache.
3. Ergänze den Schlüssel nur dann in einem Serverprofil, wenn dieser Server eine eigene Formulierung benötigt.
4. Aktualisiere Manifest und Prüfsummen.
5. Achte darauf, dass der Code denselben Schlüssel und dieselben Platzhalternamen verwendet wie die Locale-Dateien.

Ein Profil soll normalerweise keine vollständigen Kopien aller Plugin-Locals enthalten. So bleiben allgemeine Übersetzungen an einem Ort und Profile enthalten nur ihre Unterschiede.

## Abruf, Cache und Offline-Verhalten

Die Core lädt beim Start zuerst den zuletzt gültigen lokalen Cache. Danach prüft sie GitHub asynchron und übernimmt eine neue Revision erst, wenn Manifest, Dateien, Hashes und YAML gültig sind. Schlägt der Abruf fehl, bleibt der Cache aktiv. Beim ersten Start ohne Cache liefern die gebündelten Plugin-Locals die Grundversorgung.

Plugins greifen weiterhin über die Locale-API zu. Sie laden nicht selbst von GitHub. Die Core veröffentlicht aktualisierte Texte über ihren Locale-Manager, damit neue API-Abfragen den neuen Profilstand verwenden.

## Aktueller Umsetzungsstand

Das Repository enthält derzeit den Core-Fallback und die beiden deutschen und englischen Locale-Dateien im Lobby-Profil. Die Dateien sind im Manifest mit Prüfsummen eingetragen. Ein allgemeiner Lobby-System-Fallback sowie Locals weiterer Plugins fehlen noch.

Die GitHub-Synchronisierung selbst ist noch nicht in Flunar-Core implementiert. Die Dateien und das Manifest sind vorbereitet, werden aber erst von Servern genutzt, wenn die Remote-Locale-Unterstützung in der Core ergänzt ist.
