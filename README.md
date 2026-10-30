# Project 6 - TranslationMe

Submitted by: **Nafisa Nusha**

**TranslationMe** is an iOS app that translates words, phrases, and sentences with MyMemory's REST API and stores personal translation history in Firebase Cloud Firestore.

Time spent: **2** hours spent in total

## Required Features

The following **required** functionality is implemented in source. Firebase-backed features require the setup below and end-to-end verification before submission:

- [x] Users open the app to a TranslationMe home page with a place to enter a word, phrase or sentence, a button to translate, and another field that is initially empty.
- [x] Tapping Translate sends the upper field to MyMemory and displays the translation in the lower field.
- [x] Translation history is stored in Firebase and displayed in a scroll view on the same screen.
- [x] History can be erased, with a confirmation dialog.

The following **optional** features are implemented:

- [x] Choose among 12 source and target languages.
- [x] UI flair: purple accents, rounded cards, SF Symbols, loading states, dark mode support, and an empty history view.

The following **additional** features are implemented:

- [x] Swap source and target languages.
- [x] Copy a translation and reuse an entry from history.
- [x] Anonymous Firebase authentication and per-user Firestore access rules.
- [x] UTF-8 byte limit validation, API error messages, and separate history-save errors.
- [x] Erase history in batches, including histories with more than 500 entries.

## Open and run in Xcode

Requires Xcode 16.4 or newer and iOS 17 or newer. The project includes FirebaseCore, FirebaseAuth, and FirebaseFirestore through Swift Package Manager.

1. Open `TranslationMe.xcodeproj`. Allow Xcode to resolve Firebase packages.
2. Select the TranslationMe target. Under **Signing & Capabilities**, choose your team and a unique bundle identifier (the default is `com.example.TranslationMe`).
3. Complete Firebase setup below.
4. Choose an iPhone simulator or signed device, then press Run (Command-R).

Translation works without Firebase configuration; the app explicitly reports that history cannot be saved. It does not pretend to save cloud history locally.

## Firebase setup

1. Create a project in the [Firebase console](https://console.firebase.google.com/).
2. Register an Apple/iOS app using exactly the bundle identifier selected in Xcode.
3. Download **GoogleService-Info.plist** and copy it into the inner `TranslationMe/` folder beside `TranslationMeApp.swift`. The Xcode synchronized folder includes it automatically; confirm it appears in the target's Copy Bundle Resources. Use the real file, not placeholder values.
4. In **Authentication → Sign-in method**, enable **Anonymous**.
5. Create a **Cloud Firestore** database. Under **Rules**, paste the contents of `firestore.rules` and publish them. Do not use open test-mode rules.
6. Run the app. The history status should indicate a Firebase connection. Translate a phrase and verify a document appears under `users/{anonymousUid}/translations/{documentId}`.

No MyMemory API key is needed for basic translation. Firebase billing/quota settings belong to your own project. No Firebase credentials or project were supplied with this assignment.

## How it works

- `TranslationService.swift`: HTTPS GET to `https://api.mymemory.translated.net/get`, with safely encoded `q` and `langpair` parameters. Enforces MyMemory's 500-byte limit and handles HTTP and API errors.
- `TranslationModel.swift`: main-actor UI state, anonymous Firebase sign-in, server timestamps, live Firestore history, saving and batch deletion.
- `ContentView.swift`: SwiftUI translation screen, language menus, copy/reuse controls, history, and deletion confirmation.
- `TranslationMeApp.swift`: configures Firebase only when its real configuration file is present.

History belongs to the anonymous account on the current installation. Cross-device history is not provided; reinstalling or losing that anonymous account can lose access to its history. Input and translated output are sent to MyMemory; saved entries are also sent to your Firebase project. The service has usage limits and machine translations may be imperfect.

## Verification and walkthrough checklist

Source syntax parsing, Xcode project property-list validation, and mocked translation-service checks passed during creation. The mocked checks cover request query encoding, response/entity decoding, blank input, UTF-8 limits, and identical languages. Firebase package resolution was blocked by network restrictions; simulator access was blocked by sandbox restrictions. A full build, simulator walkthrough, API integration test, and Firebase integration test have not been verified in this environment. Before submitting:

1. Launch: input and output start empty; history shows an empty state on a new account.
2. Enter `Hello world`, choose English → Spanish, and tap Translate. Verify Spanish output appears and a Firestore document is saved.
3. Relaunch and verify history remains. Tap an entry to reuse it, copy the result, and try another language pair.
4. Try blank input, identical languages, and text over 500 UTF-8 bytes. Translation is disabled.
5. Turn off connectivity and check error feedback. Restore it and retry.
6. Tap **Erase all**, cancel once, then confirm. Verify both the screen and the Firestore collection are empty; relaunch to confirm.
7. Check dark mode, larger text, and landscape on your chosen device.

## Video Walkthrough

**TODO:** Record the implemented user stories after Firebase setup and add your actual Loom or GIF link here. No walkthrough recording has been fabricated.

## Notes

MyMemory limits input by UTF-8 bytes, which differs from character count for many languages. Firebase configuration is intentionally not fabricated. Separate error feedback keeps a successful translation visible if saving history fails. Erasing all history requests server data, so it requires connectivity. An anonymous account separates each installation's data using authenticated Firestore rules.

Replace the submission name, actual time, walkthrough link, and license owner before submitting. Confirm the feature checklist against your own running app.

## References

- [MyMemory REST API specification](https://mymemory.translated.net/doc/spec.php)
- [Add Firebase to your Apple project](https://firebase.google.com/docs/ios/setup)
- [Anonymous authentication on Apple platforms](https://firebase.google.com/docs/auth/ios/anonymous-auth)

## License

Copyright 2026 Nafisa Nusha

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
