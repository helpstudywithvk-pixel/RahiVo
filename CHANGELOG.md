# Changelog

## 0.8.0 — AI mentor, OAuth, device notifications and privacy
- Added authenticated Supabase Edge Function source for Bada Bhai with server-side provider configuration, thread ownership checks, bounded context, a daily request quota and server-side guardian-consent enforcement.
- Added Google OAuth browser flow and Android/iOS callback setup guide.
- Added local scheduled notifications and a daily exact-alarm reminder with OS permission prompts.
- Added Android UsageStatsManager bridge and iOS FamilyControls authorization, native weekly DeviceActivityReport host, and monitor/report extension source scaffolds.
- Added guardian-consent request and one-time confirmation flow, age-band fields, and account deletion endpoint/UI.
- Added migration source for quotas, consent metadata and deletion audit. Not applied to production.
- Added deployment and legal-readiness documentation. No production deployment was performed.

## 0.7.0
- Added study topics, learning notes and task duration fields to the Supabase-backed student platform.
- Added local-first cloud sync for core learning records.
\n## v0.8.1 — Gemini provider adapter\n- Replaced OpenAI-compatible endpoint configuration with a server-side Gemini Developer API adapter.\n- Added Gemini API key/model secret setup guide; no API key is embedded in the app or repository.\n

## v0.8.1 — Gemini source preparation
- Switched Bada Bhai provider integration to Google Gemini Developer API (`gemini-2.5-flash` default).
- Moved API key to `x-goog-api-key` request header; it is read only from the Supabase Edge Function secret.
- Added strict request-object and UUID thread ID validation.
- Validated requested reply language (Hindi, English, Hinglish; otherwise auto-detect).
- Fixed recent-history selection to use the latest 10 messages in chronological order.
- Moved quota consumption after ownership validation for existing threads.
- Updated Flutter package version to `0.8.1+9`.
- No production deployment or API key configuration was performed in this update.
