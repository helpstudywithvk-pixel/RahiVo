# RAAHIVO v0.8.0 integration checklist

- [x] Supabase schema and owner-only RLS for existing student records (live project, previously verified).
- [x] Source for secure AI mentor Edge Function (JWT, server-side provider secret, bounded messages, daily quota, age/guardian server gate).
- [x] Google OAuth browser sign-in code and redirect setup instructions.
- [x] Local scheduled notification service and exact-alarm permission request.
- [x] Android UsageStatsManager permission bridge.
- [x] iOS FamilyControls authorization bridge, native weekly DeviceActivityReport host and monitor/report extension source scaffolds.
- [x] Guardian-consent schema, email-link Edge Function and student UI.
- [x] Self-service account deletion Edge Function and confirmation UI.
- [ ] Apply the new SQL migration to the live Supabase project (requires approval).
- [ ] Deploy the three Edge Functions (requires approval).
- [ ] Configure AI provider secrets and Resend email secrets in Supabase.
- [ ] Configure Google Cloud OAuth and Supabase provider settings.
- [ ] Host the guardian-consent confirmation page over HTTPS and set APP_PUBLIC_URL.
- [ ] Generate Android/iOS platform projects and verify application/bundle IDs.
- [ ] Obtain Apple's Family Controls entitlement; create and sign the DeviceActivityMonitor and DeviceActivityReport Xcode extension targets.
- [ ] Configure Firebase/APNs if remote push notifications are required.
- [ ] Run `flutter pub get`, `flutter analyze`, `flutter test`, Android build and physical-device tests.
- [ ] Complete Indian privacy/legal review before onboarding minors publicly.
