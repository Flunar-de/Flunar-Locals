# Flunar-Locals

[Deutsche Version](README.md)

This repository collects translations for Flunar plugins. Players and other community members can contribute new languages or improve existing text. Changes are proposed through pull requests and reviewed before they are merged.

You do not need to program the plugin to contribute a translation. Keep the existing message keys and file structure intact.

## Important contribution rule

**Each pull request may change translations for exactly one plugin and one target area: either the general fallback or a single server profile.** Changes to multiple plugins or profiles must be submitted as separate pull requests. This also applies when adding the same language to multiple plugins. In `manifest.yml`, only change entries that belong to that plugin and target area.

This separation makes every contribution easier to review. Translations must accurately and completely reflect the original text. Insults, discriminatory content, harassment, hidden messages, and unrelated text will be rejected. Maintainers review every pull request before merging and may request changes or close a contribution.

## Fallbacks and server profiles

The `fallback/` directory contains a plugin's general translations. They are used on all servers unless a server profile provides different text.

A profile is only needed when a server type requires different wording. These overrides belong under `profiles/<Profile>/<Plugin-ID>/`. A profile does not need to contain every message: missing files and message keys are taken from the fallback. If a profile does not need its own translations, the plugin uses `fallback/` directly.

Each plugin has its own directory. The Core files are currently under `fallback/Flunar-Core/`, and the LobbySystem files are under `fallback/Flunar-LobbySystem/`. German and English are currently available for both plugins. The LobbySystem uses its fallback files and has no profile-specific translations at this time. German and English fallback translations for Flunar-Claims are under `fallback/Flunar-Claims/`; it has no separate server profile.

## Adding a general language

New languages belong in the fallback so they can be used on all servers by default.

1. Choose exactly one plugin and open one of its existing fallback files as a template. If the language is not available yet, use another language file for the same plugin.
2. Add a file for the new language in that plugin's fallback directory. Use the filename format `locale_<language-code>.yml`, for example `locale_fr.yml` for French.
3. Translate the visible text. Keep message keys, indentation, placeholders, formatting, and special characters intact. Do not translate keys or remove placeholders.
4. Add the new file to `manifest.yml`. The manifest is how the Core finds translations; each file needs the plugin ID, language code, file path, and matching SHA-256 checksum.
5. Only add a manifest entry for this plugin and language. If you cannot calculate the checksum, mention that in the pull request. Maintainers can complete it during review.

If the same language should also be available for another plugin, submit a separate pull request for that plugin.

## Adding a translation for a server profile

A profile is for server-specific wording, such as a special lobby, an event, or a game mode. General text and translations stay in the fallback. A profile pull request may only change one profile and one plugin.

1. Choose a short, clear name for the server profile.
2. Add translation files only for the affected plugin under `profiles/<Profile>/<Plugin-ID>/`.
3. Include only the message keys whose wording differs from the general fallback. All other text will come from the fallback.
4. Add the new files and profile to `manifest.yml`. Each file needs the correct plugin ID, language code, path, and SHA-256 checksum. If you need help calculating the checksum, mention it in the pull request.

You do not need to create a profile if it has no overrides. The fallback is enough.

## Proposing changes through GitHub

1. Open this repository on GitHub and select **Fork** to create your own copy under your GitHub account.
2. Create a branch in your fork for the translation.
3. Add language files or improve existing translations. For new files, update the manifest too, or note in the pull request that the checksum still needs to be added.
4. Commit your changes with a clear message, such as “Add French LobbySystem translation”.
5. Create a pull request on GitHub from your branch to this repository's default branch.
6. Briefly describe the one plugin, language, and target area (fallback or profile). Respond to review questions and update your changes if maintainers ask.

Do not make changes directly in the original repository. Before merging, maintainers check that the pull request follows the contribution rule, that the translation is complete and appropriate, and that the manifest entries are correct. Changes to additional plugins or profiles will be rejected or must be submitted as separate pull requests.

## Scoreboards

Scoreboards are stored in the `scoreboards/` directory and provided through `scoreboards/manifest.yml`. The Core downloads the manifest and the files from GitHub and caches them locally. If the download fails, the Core uses the last valid version or the files bundled with the Core.

- `scoreboards/fallback/<id>.yml` contains scoreboards that apply on all servers.
- `scoreboards/profiles/<profile>/<id>.yml` contains scoreboards only for one server profile (for example `smp`, `lobby`, `citybuild`). A profile always extends `fallback`. An entry with the same `id` replaces the fallback entry.
- Paths in the manifest are relative to `scoreboards/manifest.yml`, for example `fallback/default.yml` or `profiles/smp/sidebar_pvp.yml`.
- To create a new server profile, add an entry with `extends: fallback` and `scoreboards: []` under `profiles:` in the manifest, and create the folder `scoreboards/profiles/<name>/`. The name may only contain lowercase letters, digits, `_` and `-`. The `Profile` setting in the Core's `scoreboard-settings.yml` determines which profile a server uses.
- The `id` is the name a scoreboard is shown with (for example `default`). Language variants are named `<id>_<language>`, for example `default_de`. Allowed characters are lowercase letters, digits, `_` and `-`.
- Every file needs an entry in the manifest with its path and SHA-256 checksum. Calculate the checksum with `sha256sum scoreboards/<path>`.

The `ignore` files in `scoreboards/profiles/smp/`, `scoreboards/profiles/lobby/`, `scoreboards/profiles/citybuild/` and in the locale folders `profiles/smp/` and `profiles/lobby/` are only placeholders. Git does not store empty folders, so they make sure the folders exist locally and are pushed to GitHub. They have no function, so you can ignore them.

Example for a new scoreboard in the `smp` profile:

1. Create the file `scoreboards/profiles/smp/sidebar_pvp.yml`.
2. Calculate the checksum: `sha256sum scoreboards/profiles/smp/sidebar_pvp.yml`
3. In the manifest, replace `scoreboards: []` under the profile `smp` with a list, or add the entry to the existing list:

```yaml
      - id: sidebar_pvp
        path: profiles/smp/sidebar_pvp.yml
        sha256: <checksum from step 2>
```

A pull request with scoreboard changes affects only one profile or the fallback. The same content rules apply as for translations.

## Before submitting a pull request

- Is all visible text translated, with the original meaning preserved?
- Do message keys, placeholders, and formatting match the source files?
- Are general translations in the fallback, with only genuine overrides in the server profile?
- Does the pull request affect exactly one plugin and one target area?
- Does the translation contain no insults, discriminatory content, hidden messages, or unrelated text?
- Are all new files listed in the manifest?
- Are the plugin ID, language code, file path, and SHA-256 checksum correct for the related manifest entries?

If you have questions, ask them in the pull request. Partial translations are also welcome if the pull request clearly states which text or files still need review.
