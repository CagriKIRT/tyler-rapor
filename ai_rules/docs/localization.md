# Localization

- **Why:** Replaced custom map-based strings with Flutter `gen-l10n` for compile-time keys and ARB files.
- **When:** June 2026.
- **What:** English strings in `mobile/lib/l10n/app_en.arb`; run `fvm flutter gen-l10n`; read strings via `context.l10n` or `localizedTextForDefaultLocale()` when there is no `BuildContext`.
