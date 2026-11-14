# Sanad Android — 0.13

A native Kotlin Android app for family care and doctor follow-up, with Arabic and English interfaces.

## Open and build

1. Clone this repository or download and extract its ZIP.
2. In Android Studio, open the directory containing `settings.gradle.kts`.
3. Install Android SDK 36 and select JDK 17 or newer for Gradle.
4. Wait for Gradle Sync, then run the `app` configuration on Android 9 (API 28) or newer.

The Gradle 8.11.1 wrapper is included. From the project directory:

```powershell
.\gradlew.bat assembleDebug
```

On macOS/Linux:

```sh
chmod +x gradlew
./gradlew assembleDebug
```

APK output: `app/build/outputs/apk/debug/app-debug.apk`.

## Language

Tap **English** on the sign-in screen or in **My account**. Tap **العربية** to switch back. The choice is saved on the device. Arabic uses right-to-left layout; English uses left-to-right layout. Account type, patient data and user-authored messages do not change when the language changes. Existing measurement records retain their storage identifiers.

## Features

- Google sign-in or email/password authentication.
- A family or doctor role saved once on the server; the same account cannot switch roles in the app.
- Doctor codes, family link requests, acceptance and revocation.
- Patient profiles, family members, tasks with dates and notes, medications and text lab results.
- Manual blood pressure, oxygen saturation and blood glucose entries; a watch is optional.
- Read-only shared family records for the accepted doctor.
- Text conversations between the linked family and doctor.
- Optional Health Connect reading on the device.

## Firebase setup

The included `app/google-services.json` is client configuration for the existing `sanad-345dd` demo project. It is not an administrator credential. No service-account private key, user password or signing key is included.

Email/password authentication is available. A build signed on another computer needs that signing key's SHA-1 registered in Firebase to use Google sign-in. The previously supplied APK uses the configured signing key.

For an independent backend:

1. Create a Firebase project and register Android package `com.sanad.health`.
2. Enable Email/Password and Google authentication.
3. Create a Firestore database.
4. Replace `app/google-services.json` with your own configuration.
5. Publish `firestore.rules` to your Firebase project. If using Firebase CLI, explicitly select your own project when deploying.

Use synthetic data for evaluation. Do not upload real medical records to the shared demo backend.

## Try the family/doctor flow

Use two different accounts. The doctor registers a code. The family saves the patient's name, searches for that code and confirms sharing. The doctor accepts the request and opens the shared family file. New family entries synchronize while the app is online. Open the conversation to exchange messages. Revoking the link blocks subsequent server access; it cannot recall copies saved outside the app.

Existing registered doctor accounts retain their doctor role. Accounts with an existing family link retain the family role. Other legacy accounts select a role once after signing in.

## Source map

| File | Purpose |
| --- | --- |
| `app/src/main/java/com/sanad/health/MainActivity.kt` | Programmatic native UI, authentication flow, care records and conversations |
| `app/src/main/java/com/sanad/health/CloudLink.kt` | Firestore transactions, roles, links, shared records and messages |
| `app/src/main/java/com/sanad/health/I18n.kt` | Saved language preference, localized context and stable measurement labels |
| `app/src/main/res/values/strings.xml` | Arabic interface strings |
| `app/src/main/res/values-en/strings.xml` | English interface strings |
| `app/src/main/java/com/sanad/health/HealthActivity.kt` | Optional Health Connect integration |
| `app/src/main/java/com/sanad/health/PermissionsRationaleActivity.kt` | Health permission explanation |
| `app/src/main/AndroidManifest.xml` | Activities, permissions and application configuration |
| `app/src/main/res` | Launcher icon and resources |
| `app/build.gradle.kts` | App version and dependencies |
| `firestore.rules` | Backend authorization rules |

Most interface strings use Android resources. Dynamic sentences use paired Arabic/English templates in Kotlin. User-entered content is displayed as authored.

## Verification and limitations

Version 0.12 passed 38 live backend checks, covering role immutability, linking, data sharing, two-way messages, unauthorized access and revocation. See `cloud-v012-test-results.json`. Version 0.13 adds localization without changing the backend rules. Its debug build passed, along with checks for 297 matching Arabic/English resource pairs, translated template parameters, stable measurement identifiers and explicit locale direction configuration.

No end-to-end UI run on two physical phones has been completed. There are no background message notifications, automated medical alerts, message attachments or tested direct watch connections. Shared health records are read-only for the doctor. Automatic family-data restoration on a new device and multi-device conflict merging are not implemented. Conversations display the latest 100 messages, but read the matching conversation history without server pagination.

Doctor registration is automatic for demonstration and does not verify professional credentials. Server access still requires an accepted family link.

This README is the current setup guide for version 0.13.


