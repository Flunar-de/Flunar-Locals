# Flunar-Locals

Dieses Repository ist die gemeinsame Quelle für Sprachdateien der Flunar-Plugins. Die Flunar-Core wählt pro Server ein Profil aus und stellt die Texte über ihre Locale-API bereit. Jedes Plugin liefert seine eigenen Nachrichtenschlüssel und unterstützten Sprachen.

**Stand dieses Repositorys:** Die Ordner `fallback/` und `profiles/` sowie `manifest.yml` sind vorbereitet. Locale-Dateien sind noch nicht angelegt. Dieses Dokument beschreibt die geplante Struktur und legt keine Locale-Dateien an.

## Aufbau

```text
Flunar-Locals/
├── manifest.yml
├── fallback/
│   ├── Flunar-Core/
│   │   ├── locale_de.yml
│   │   └── locale_en.yml
│   └── Flunar-LobbySystem/
│       ├── locale_de.yml
│       └── locale_en.yml
└── profiles/
    ├── lobby/
    │   └── Flunar-LobbySystem/
    │       └── locale_de.yml
    └── survival/
        └── Flunar-Claims/
            └── locale_de.yml
```

Die gezeigten Locale-Dateien sind Beispiele für die spätere Struktur; sie sind aktuell nicht vorhanden.

### Fallback

`fallback/` enthält die allgemeinen Texte. Für jedes Plugin und jede Sprache, die das Netzwerk anbietet, sollte dort eine vollständige Standarddatei liegen. Zum Beispiel:

- `fallback/Flunar-LobbySystem/locale_de.yml`
- `fallback/Flunar-LobbySystem/locale_en.yml`

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
      - plugin: Flunar-LobbySystem
        language: de
        path: fallback/Flunar-LobbySystem/locale_de.yml
        sha256: "<sha256>"
      - plugin: Flunar-LobbySystem
        language: en
        path: fallback/Flunar-LobbySystem/locale_en.yml
        sha256: "<sha256>"

  lobby:
    extends: fallback
    files:
      - plugin: Flunar-LobbySystem
        language: de
        path: profiles/lobby/Flunar-LobbySystem/locale_de.yml
        sha256: "<sha256>"
```

Das Manifest ist hier nur dokumentiert und bleibt vorerst leer. Wenn die Locale-Dateien später hinzugefügt werden, müssen auch die passenden Manifest-Einträge und SHA-256-Prüfsummen ergänzt werden. In der echten Fallback-Liste stehen alle verwendeten Plugins und alle für sie angebotenen Sprachen.

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

- Allgemeine Lobby-System-Texte liegen in `fallback/Flunar-LobbySystem/locale_de.yml` und `locale_en.yml`.
- Lobby-spezifische Überschreibungen liegen in `profiles/lobby/Flunar-LobbySystem/`.
- Die Lobby verwendet das in der Core eingestellte Profil. Eine zweite Einstellung in der Lobby-System-Config ist nicht erforderlich.
- Die Core muss `de` und `en` unter `Language.AvailableLanguages` freigeben. Das Lobby-Sprachmenü bietet derzeit genau diese beiden Sprachen an.
- Wird zum Beispiel Französisch ergänzt, müssen Core-Konfiguration, Lobby-Sprachmenü, `loadPluginLocales`-Sprachen, gebündelte Plugin-Datei und Fallback-Datei zusammen angepasst werden.

Die Datei unter `fallback/` enthält den allgemeinen Schlüssel; eine Profil-Datei überschreibt ihn nur für diesen Servertyp:

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

Dieses Repository enthält derzeit noch keine Locale-Dateien. Auch das Manifest ist noch leer. Die obigen Strukturen und Beispiele dokumentieren den Zielaufbau; sie sind keine bereits von Flunar-Core unterstützte GitHub-Synchronisierung. Die Core- und Plugin-Anpassungen müssen implementiert werden, bevor die Remote-Einstellungen wirksam sind.
