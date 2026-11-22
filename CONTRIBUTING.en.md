# Contributing to Flunar-Locals

[Deutsche Version](CONTRIBUTING.md)

Thank you for wanting to improve the translations of the Flunar plugins. Contributions are welcome as pull requests. You do not need to be able to program the plugin to translate it.

Please read these guidelines in full before submitting changes.

## Most important contribution rule

A pull request may only contain changes for **exactly one plugin** and **exactly one target area**:

- either the general fallback under `fallback/<Plugin-ID>/`,
- or a single server profile under `profiles/<Profile>/<Plugin-ID>/`.

Changes to multiple plugins, profiles, or target areas must be submitted as separate pull requests. This also applies when the same language should be added for several plugins.

## Possible contributions

You can, in particular:

- add a new language,
- correct or improve existing translations,
- add missing texts,
- add or correct profile-specific wording,
- fix errors in the related entries of `manifest.yml`.

## Requirements for translations

- Translate only the visible text, never the message keys.
- Keep indentation, YAML structure, quotation marks, and line breaks.
- Do not change or remove placeholders, formatting, or special characters.
- Convey the meaning of the source text accurately and completely.
- Use consistent terms within a plugin.
- Check machine-generated translations carefully. Unchecked automatic translations will not be accepted.
- Insulting, discriminatory, harassing, hidden, or unrelated content is not allowed.

## Fallback or server profile?

General translations belong in:

```text
fallback/<Plugin-ID>/locale_<language-code>.yml
```

Server-specific differences belong in:

```text
profiles/<Profile>/<Plugin-ID>/locale_<language-code>.yml
```

A server profile should only contain the texts that differ from the general fallback. All files and message keys that are not specified are taken from the fallback.

## Adding a new language

1. Choose exactly one plugin and one target area.
2. Use an existing language file of the same plugin as a template.
3. Create the new file following the pattern `locale_<language-code>.yml`, for example `locale_fr.yml`.
4. Translate all visible texts and keep keys, placeholders, and formatting.
5. Add only the matching entry in `manifest.yml`.
6. Enter the plugin, language code, file path, and SHA-256 checksum correctly.

If you cannot calculate the SHA-256 checksum yourself, state this clearly in the pull request description. The maintainers can add it during review.

## Workflow for a contribution

1. Create a fork of this repository.
2. Create your own branch in your fork, for example `translation/fr-flunar-core`.
3. Make your changes according to these guidelines.
4. Check the YAML files and the related manifest entries.
5. Create a clear commit, for example `Add French Core translation`.
6. Open a pull request against the main branch of this repository.

Do not change the original repository directly. Contributions should be submitted through a fork and a pull request.

## Pull request description

Briefly describe in the pull request:

- the affected plugin,
- the language,
- the target area (`fallback` or exactly one profile),
- the content or reason for the change,
- open points or checks that are still needed.

## Checklist

Please check the following before submitting:

- [ ] The pull request affects exactly one plugin.
- [ ] The pull request affects exactly one target area.
- [ ] All visible texts are complete and translated in meaning.
- [ ] Message keys, placeholders, and formatting have been kept.
- [ ] The YAML structure and indentation are valid.
- [ ] General texts are in the fallback, and only real differences are in the profile.
- [ ] New or changed files are correctly listed in `manifest.yml`.
- [ ] File path, language code, and SHA-256 checksum are correct.
- [ ] The contribution contains no unrelated or inappropriate content.
- [ ] I have read and accepted the section "License for contributions".

## Review and acceptance

The maintainers review every pull request before it is accepted. They may request changes, ask questions, or reject or close contributions that do not follow these guidelines.

Incomplete contributions are also welcome, provided the pull request description clearly explains what is still missing or needs to be checked.

## License for contributions

This repository is licensed under the [MIT License](LICENSE).

By submitting a pull request, you confirm that:

1. you created the contribution yourself or are authorized to submit it,
2. the contribution does not violate any third-party rights,
3. you make your contribution available under this repository's MIT License, and
4. Flunar.de and other people may use, modify, and distribute the contribution in accordance with the MIT License.

If you do not agree with these terms, please do not submit a contribution.

## Questions

If you have questions, you can ask them directly in your pull request.
