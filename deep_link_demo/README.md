# deep_link_demo

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

# deep_link_demo

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## Deep link notes (concise answers)

### Concept Check
- Route inside Flutter vs Android-level deep link:
	- A Flutter route is an in-app navigation concept (Widget tree + Navigator) handled by Dart at runtime. It maps URIs/strings to screens inside the running Flutter app.
	- An Android-level deep link is an OS-level intent/URI matching rule declared in the AndroidManifest that tells Android which app/activity should handle an external URI. It lets the OS launch or route an app when a link or intent is triggered outside the app (browser, other apps, adb, OS).

- Why Android needs an intent filter:
	- The Android system uses intent filters to match incoming intents (action/data/category) to apps. The intent filter declares which URIs and actions the activity can handle so the OS can resolve and deliver the intent to the correct app/activity.

### Technical Understanding
- Role of the `app_links` package:
	- `app_links` provides a cross-platform Dart API to receive platform deep-link events (initial link that launched the app and link events while running). It abstracts the native platform intent/callback plumbing so your Flutter code can get URIs reliably without writing platform-specific code.

- What happens if a deep link is opened while the app is already running:
	- The OS delivers an Intent to the existing activity (depending on activity launchMode/config). The Flutter side receives a link event (via the plugin stream API) or the activity's `onNewIntent` is called; the plugin forwards the URI to Dart so you can navigate to the appropriate Flutter route.

### Debugging Insight
- If `adb shell am start -d "myapp://details/123"` opens the app but it doesn't navigate to the detail page, check these:
	 Dart link handling code (`lib/main.dart`) — ensure you subscribe to the plugin's link stream (and handle the initial URI) and that the handler actually calls Navigator.push or updates state. This is where the app translates an incoming URI into a route.

These checks are ordered by likelihood: most issues are in the Dart handler (missing stream subscription or initial URI handling), then native forwarding, then manifest mismatch.
