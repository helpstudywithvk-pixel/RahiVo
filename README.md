# RAAHIVO — Native Flutter Frontend

**Brand line:** Har Kadam Par Tumhare Saath.  
**Mentor identity:** Bada Bhai ❤️  
**Frontend version:** 0.8.0

RAAHIVO is a native Flutter app frontend for Android and iOS. This package includes the student-facing screen flow, local-first persistence and Supabase-backed account/data sync. It is not a website, PWA or WebView.

## Included frontend screens and flows

### Entry and profile
- Three-step welcome/onboarding flow with English/Hindi choice, education level, exam goal, optional name and daily study target.
- Skip onboarding and continue without entering personal details.
- Edit learning profile after onboarding.
- Live Supabase email/password sign-up and sign-in, plus password-reset email request. Google sign-in still needs OAuth configuration.

### Main app
- Five-tab navigation: Home, Learn, Practice, Bada Bhai and Profile.
- Home dashboard based only on the student's own saved records.
- Learning topics list with add/delete.
- Exam track selector and course-library navigation.
- Course catalogue and course detail empty states; no fabricated lessons or syllabus content.
- Bada Bhai chat UI connected in source to the secured `mentor-chat` Edge Function; it returns an honest unavailable state until the function is deployed and an AI provider is configured.
- Mock-test configuration UI (exam, difficulty, question count and duration). It does not start a test until verified question content exists.
- Practice record entry for tests the student actually attempted.
- Mistake notebook with add/delete.
- Study planner with add/complete/delete tasks.
- Focus timer with selectable durations, pause/reset, app-lifecycle pause and local completed-session records.
- Revision centre with due dates, review status and delete.
- Progress view calculated from the student's own saved activity only.
- Mind Space with temporary mood selection and breathing pause; it is not clinical care.
- Digital wellbeing screen with Android UsageStatsManager and iOS FamilyControls / DeviceActivity report scaffolds. Nothing is read until the student explicitly grants OS permission.
- Notification preference screen and alarm/quiet-hour preference editor.
- Profile, appearance, language preference and privacy/data controls.

## Data and empty-state policy

- No seeded student names, sample marks, invented progress, sample timetable, fake question bank, fake lesson, fake chat reply or fake notification history is included.
- Lists start empty and become populated only when the student enters their own information.
- Practice accuracy and progress values are derived from locally entered records.
- `shared_preferences` keeps an offline local copy. When signed in, planner, topic, revision, mistake, focus, practice and reminder-preference records sync with Supabase.
- The preferred alarm time and quiet hours sync as preferences. Enabling the reminder schedules a real local OS notification on the device.

## Not connected yet — requires backend or native integration

These are deliberately not simulated:

- Google sign-in is wired to Supabase OAuth; Google Cloud and Supabase dashboard credentials/redirect allow-list must be configured using `docs/GOOGLE_SIGN_IN_SETUP.md`.
- Live AI Bada Bhai replies until the Edge Function is deployed and provider secrets are configured.
- Reviewed course catalogue, syllabus, lesson content, videos, notes and exam-wise question banks.
- Real mock-test delivery, answer evaluation and exam-specific scoring rules.
- Local scheduled notifications and a daily exact-alarm reminder are wired through the OS notification plugin. Remote push (FCM/APNs) is not configured.
- Android UsageStatsManager / Usage Access bridge is included. iOS FamilyControls authorization bridge, native weekly DeviceActivityReport host and monitor/report extension source scaffolds are included; Apple entitlement approval and Xcode extension targets are still required.
- Parent/teacher dashboards are not included. Guardian consent request/verification and account deletion Edge Function sources are included but require deployment and provider setup.

The app screens explain these limitations instead of pretending these services work.

## Privacy and safety boundaries

- Supabase Auth and core student-record cloud sync are configured in source. The Flutter app still needs local SDK/device testing before this integration can be called end-to-end verified.
- No contacts, microphone, camera, private-message or screen-content permission is requested. App-usage access is optional and requested only when the student enables Digital Wellbeing.
- Local `shared_preferences` storage is app-sandboxed but is not end-to-end encrypted. Avoid entering passwords, financial information or highly sensitive information.
- Mind Space mood selection is temporary and not persisted.
- Wellbeing features are general self-care tools, not diagnosis or treatment. Students in serious distress should contact a trusted adult, teacher, counsellor or local emergency support.
- Before public launch, add secure local secret storage where needed, a data-export flow, reviewed content, robust child-safety controls and India-specific legal review. Guardian consent and account-deletion source is included but not live until migration/functions are deployed.

## Project structure

```text
lib/
  main.dart                 # App state, navigation, screens and reusable UI
test/
  app_data_test.dart        # Empty-state, persistence and preference tests
```

## Run in Android Studio

1. Install Flutter SDK and Flutter/Dart plugins in Android Studio.
2. Extract this folder and open it in Android Studio.
3. In the project terminal run:

```bash
flutter create --platforms=android,ios --org com.raahivo --project-name raahivo .
flutter pub get
flutter analyze
flutter test
flutter run
```

To create a debug APK after successful analysis and device testing:

```bash
flutter build apk --debug
```

The APK output will be `build/app/outputs/flutter-apk/app-debug.apk`.

## Verification status

The authoring environment used to prepare this package does not have Flutter or Dart installed. Therefore `flutter pub get`, `dart format`, `flutter analyze`, automated Flutter tests, emulator testing and APK compilation have **not** been run here. Treat this as a source-code delivery, not a device-verified release. Run the commands above before using it as a production build.

## Supabase backend — v0.7.0

A live Supabase project has been created for RAAHIVO in the Mumbai region. The initial database schema is applied and includes:

- `profiles`
- `study_plans`
- `study_sessions`
- `revision_items`
- `practice_attempts`
- `mood_checkins`
- `notification_preferences`
- `mentor_threads`
- `mentor_messages`
- `study_topics`
- `learning_notes`

Every table has Row Level Security enabled. Student-owned rows are restricted to `auth.uid()`; the `anon` role has no table access. Explicit `authenticated` grants are included because newly created Supabase projects no longer expose public tables to the Data API automatically. The database starts empty; no demo student data is inserted.

### Connect this Flutter app

The app includes `supabase_flutter` and UUID-backed local records. Email/password sign-up, sign-in and password-reset requests use Supabase Auth. Profile details and learning records sync only after an age choice and (for under-18 users) verified guardian consent.

Use the RAAHIVO project URL and publishable key below as build-time values (the publishable key is designed for mobile/client use; never put a Supabase secret/service-role key in the app):

```bash
flutter pub get
flutter run \
  --dart-define=SUPABASE_URL=https://gpqwwtbagysvwgqtwggk.supabase.co \
  --dart-define=SUPABASE_PUBLISHABLE_KEY=sb_publishable_PtSRgW2QClbrt3iHKTmwFA_drYrF7pJ
```

For a debug APK after local testing:

```bash
flutter build apk --debug \
  --dart-define=SUPABASE_URL=https://gpqwwtbagysvwgqtwggk.supabase.co \
  --dart-define=SUPABASE_PUBLISHABLE_KEY=sb_publishable_PtSRgW2QClbrt3iHKTmwFA_drYrF7pJ
```

### Supabase dashboard settings still required

1. In Authentication settings, confirm Email provider is enabled and choose the email-confirmation policy you want for testing.
2. Configure a production SMTP provider before public launch; the built-in email service is rate-limited and is not intended for production delivery.
3. Google sign-in requires the Google Cloud Web OAuth client ID/secret in Supabase Auth and the mobile redirect allow-list. Follow `docs/GOOGLE_SIGN_IN_SETUP.md`.
4. Configure password-reset redirect URLs and Android/iOS app deep links before relying on password reset on real devices.
5. Review and deploy the included age/guardian-consent and account-deletion flows before public use. A data-export flow and legal sign-off are still outstanding.

### Important integration boundary

- Email/password authentication, password-reset request and signed-in profile upsert are wired to Supabase when the two build-time values are supplied.
- Planner tasks, study topics, revision items, mistake-notebook notes, focus sessions, practice attempts and reminder/quiet-hour preferences now sync with Supabase after sign-in. Local copies remain available offline.
- On sign-in/app launch, the app uploads local records, fetches cloud records and merges by stable UUID. If the same record ID exists on both sides, the local copy wins in this first sync version. Deletions are queued locally and retried when sync runs again.
- AI mentor calls use the `mentor-chat` Edge Function source. The Gemini-backed function requires server-side `GEMINI_API_KEY` (and optionally `GEMINI_MODEL`); no AI key is embedded in Flutter. The updated function source is included in this package but still needs an approved deployment and secret configuration.
- Course content and question banks remain empty until reviewed content is supplied and published.
- Local scheduled notifications and Android exact-alarm reminders are wired in source. Remote push requires Firebase/APNs credentials. Android usage summaries are read after explicit Usage Access approval. iOS opens a native weekly DeviceActivityReport after Family Controls entitlement approval and Xcode extension setup.

### Backend migration history

The live project has applied migrations:

- `20261031050711_initial_student_platform_schema`
- `20261031050717_index_mentor_message_owner`
- `20261031051054_add_topics_and_learning_notes`
- `20261031051220_store_study_task_duration`

The matching SQL files are in `supabase/migrations/` for source control.

## Verification

The live database was inspected after migration: all 11 tables exist, RLS is enabled on every table, and each newly added sync table has an owner-only policy. A privilege check confirmed `anon` cannot SELECT any of these tables while `authenticated` has the required table grants. Supabase Security Advisor returned no security lints. Performance Advisor flagged only unused indexes on a new empty database; a missing owner index was added. Flutter build/analyze/device tests still need to be run in an environment with Flutter SDK installed.

### Cloud sync behavior and limitations

- Sync starts after a saved Supabase session is detected, runs after successful sign-in, and can be triggered from Profile → Cloud sync → Sync now.
- Records are merged by UUID. During this first version, if both device and cloud contain the same UUID, the local copy is kept. This is a simple last-local-wins strategy, not a full multi-device conflict-resolution engine.
- Deletes are recorded as local tombstones and retried at the next sync if the device was offline.
- Do not clear app data or uninstall while offline if you need unsynced local changes; wait for the Profile sync status to show success first.
- Mood selection remains temporary and is not uploaded. AI conversations, lessons and notifications are not included in this sync milestone.


## v0.8.0 — AI, OAuth, notifications, screen time and privacy flows

### Included source changes
- `docs/AI_MENTOR_SETUP.md`: server-secret and deployment verification guide.
- `supabase/functions/mentor-chat`: Gemini-backed authenticated AI mentor endpoint, server-side `GEMINI_API_KEY`, 4,000-character input cap, latest 10-message history, daily quota RPC, UUID/thread ownership validation and safe error responses. The Gemini update in v0.8.1 is source-only until separately approved for deployment.
- `supabase/functions/guardian-consent`: authenticated guardian request, one-time SHA-256 token, 72-hour expiry and email delivery through Resend. Configure `RESEND_API_KEY`, `CONSENT_FROM_EMAIL`, `APP_PUBLIC_URL`.
- `supabase/functions/delete-account`: recent-session check, explicit typed confirmation, deletion audit and Supabase Auth user deletion (foreign-key cascades remove linked records).
- `supabase/migrations/20261031100000_raahivo_safety_and_privacy.sql`: quota, age band, consent status, guardian consent and restricted deletion audit schema. The matching migration has been applied to the live database (recorded as `20261031054422`).
- Google OAuth client flow in Flutter and callback configuration templates.
- Local daily notifications / exact-alarm reminder using `flutter_local_notifications`.
- Android UsageStatsManager bridge; iOS FamilyControls authorization bridge, native weekly DeviceActivityReport host and monitor/report extension source scaffolds.
- Guardian consent and account deletion screens; AI chat invokes the secured Edge Function and is gated for under-18 profiles until consent is verified.

### Required setup and limitations
1. Use a Flutter SDK compatible with `flutter_local_notifications 19.4.0`. Generate native platform projects: `flutter create --platforms=android,ios --org com.raahivo --project-name raahivo .`. Confirm Android application ID and iOS bundle ID are `com.raahivo.raahivo`; merge platform snippets from this package.
2. Complete Google OAuth in `docs/GOOGLE_SIGN_IN_SETUP.md`.
3. Deploy the reviewed migration and Edge Functions only after explicit approval. Add AI and Resend secrets in Supabase Dashboard → Edge Functions → Secrets; never add them to Flutter or Git.
4. Host `docs/guardian-consent.html` over HTTPS and replace its project URL/publishable-key placeholders.
5. Android Usage Access requires the student to grant a special OS permission. iOS weekly Screen Time reports require Apple Family Controls entitlement approval and the included Swift sources to be added to signed Xcode extension targets.
6. Local notifications are real device notifications. Remote push via FCM/APNs is not configured.
7. Guardian email-link verification is a technical flow, not proof of DPDP legal compliance. Obtain Indian legal review of age assurance, verifiable parental consent and data processing before any public release to minors.

### Verification status
The privacy migration and the `mentor-chat`, `guardian-consent` and `delete-account` Edge Functions were previously deployed and verified active. This v0.8.1 Gemini revision of `mentor-chat` is local source only and requires explicit approval before deployment. Flutter/Dart and Android SDK are unavailable in this authoring environment, so dependency resolution, analyzer/tests, APK build and physical-device tests remain unrun. An Android GitHub Actions workflow is included to generate platform scaffolding, run checks and build a downloadable debug APK after the source is pushed to GitHub.
