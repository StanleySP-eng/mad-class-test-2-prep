# MAD Class Test 2 — Study Guide

> **Covers Chapters 5, 6 & 7:** Navigation & more widgets · Flutter plug-ins (WebView + Maps) · Microcontroller (Bluetooth) communication.
>
> **Goal:** By the end of this guide you should recognise every concept the test can ask about — and know *why* each correct answer is correct, not just memorise.

---

## How to Use This Guide

1. **Read Part 1** (Concepts) — grouped by chapter.
2. **Read Part 2** (Cheat Sheet) — keep it open while you practise.
3. **Read Part 3** (Night-Before Recap) — quick-fire bold rules to skim before the test.
4. **Try `QUESTIONS.md`** and **`PRACTICE_QUIZ.md`** closed-book.

---

# PART 1 — THE KEY CONCEPTS

---

# CHAPTER 5 — More Widgets & Basic Navigation

## Concept 1: What is Navigation?

**Navigation** = moving between different **screens** in an app.

- **Routing** is the *mechanism* that defines how navigation happens — path matching, transitions, and the rules that connect screens.
- In Flutter, **screens and pages are called `routes`.**
  - In **Android** a route ≈ an **Activity**.
  - In **iOS** a route ≈ a **ViewController**.

| Choose... | When |
|---|---|
| **Navigator** widget | Small apps **without** complex deep linking (e.g. named route with data) |
| **Router** widget | Apps **with** deep linking — keeps in sync with the address bar on the web, handles deep links on Android/iOS |

**Pitfall:** if the question mentions *deep linking / address-bar sync*, the answer is **Router**, not Navigator.

---

## Concept 2: The Navigator Widget

**`Navigator`** is a **child class of `StatefulWidget`**. It displays screens as a **stack**, using the correct transition animation for the target platform.

You navigate by calling **imperative methods** on the Navigator (accessed through the route's `BuildContext`):

```dart
// PUSH — add a route to the TOP of the stack
Navigator.push(
  context,                                  // BuildContext object
  MaterialPageRoute(                        // Route object
    builder: (context) => const NextScreen(),
  ),
);

// POP — remove the current route from the stack (go back)
Navigator.pop(context);

// EXIT the whole Flutter app (needs services.dart)
SystemNavigator.pop();   // import 'package:flutter/services.dart';
```

| Method | What it does |
|---|---|
| **`push`** | Adds a route to the **top** of the stack |
| **`pop`** | Removes the **current** (top) route — navigate back |
| `pushReplacement` | Replaces the current top route with a new one |
| `pushAndRemoveUntil` | Pushes a new route, then pops until a condition is met |
| `popUntil` | Pops the most recent routes until a condition is met |
| `removeRoute` / `replace` | Remove / swap a specific route on the stack |

**Key words:** *push = add to top*, *pop = remove current*, *stack*, *imperative*.

---

## Concept 3: BuildContext & MaterialPageRoute

**`BuildContext`** — an **abstract class**; a **handle to the location of a widget in the widget tree**.
- Each widget has its own `BuildContext`, which becomes the parent of the widget returned by its `build` function.
- Static functions like `showDialog`, `Theme.of`, `Navigator.of` take `context` so they can act on behalf of the calling widget.

**`MaterialPageRoute`** — a **modal route** that blocks interaction with previous routes and replaces the entire screen with a **platform-adaptive transition**.
- It is a **child class of `Route`** (an abstract class for an entry managed by a Navigator).
- It has a **required named parameter `builder`**, which takes a function that builds and returns the route's widget:

```dart
typedef Widget WidgetBuilder(BuildContext context);
```

---

## Concept 4: ListView

**`ListView`** is the **most commonly used scrolling widget**.
- Displays its children **one after another** in the scroll direction — vertically (default) or horizontally.
- Used to **avoid the render-overflow** problem (the yellow-and-black "BOTTOM OVERFLOWED" stripe) when the screen isn't big enough to show every widget at once.

### Two constructors — know when to use each

| | **Default `ListView( )`** | **`ListView.builder( )`** |
|---|---|---|
| Best for | **Small, fixed-size** lists, all loaded at once | **Large / dynamic** lists |
| Memory | Loads everything | **Efficient** — only loads what's visible (+ a few nearby) — *lazy loading* |
| Children come from | **`children:`** `[ ... ]` (a List of widgets) | **`itemBuilder:`** (required) + **`itemCount:`** (how many) |
| Common params | `scrollDirection` (default `Axis.vertical`), `padding` | `scrollDirection`, `padding` |

```dart
// Default — a fixed list
ListView(
  children: [ Text('A'), Text('B'), Text('C') ],
  scrollDirection: Axis.vertical,          // default
  padding: EdgeInsets.all(10),
);

// Builder — big / dynamic list, built on demand
ListView.builder(
  itemCount: numbers.length,               // how many items
  itemBuilder: (context, i) => Text('${numbers[i]}'),  // required
);
```

**Pitfall:** `ListView.builder` uses **`itemBuilder`** (the required one) **+ `itemCount`**, NOT `children`. *(Technically only `itemBuilder` is required — leave out `itemCount` and the list is treated as infinite.)*

---

## Concept 5: GestureDetector

**`GestureDetector`** is a widget that **detects gestures** — `onTap`, `onDoubleTap`, `onLongPress`, `onPanUpdate`, `onVerticalDragStart`, and many more (~64 in total).

```dart
GestureDetector(
  onTap: () { /* event-handling code when tapped */ },
  child: Container( child: Text('Tap me') ),
);
```

- If it **has a child**, it **defers to that child for its sizing** behaviour.
- If it has **no child**, it **grows to fit the parent** instead.
- By default a GestureDetector with an *invisible* child **ignores touches** — controlled by its `behavior` parameter.

**Times-Table example (Ch 5 practical):** a `ListView.builder` of numbers, each wrapped in a `GestureDetector`; tapping a number does `Navigator.push` to a second `ListView` showing that number's times table; tapping there does `Navigator.pop` to go back.

---

# CHAPTER 6 — Flutter Plug-ins & More Navigation

## Concept 6: API vs Plug-in

**API — Application Programming Interface** — a set of rules that lets two pieces of software talk to each other. Three parts: the **Application**, the **Interface** (the rules/protocols), and the **Documentation** (how to write code to talk to it).

**Two APIs you already use.** They are **two separate things** — the **Dart API is NOT part of the Flutter API.** Dart is the language's own API; Flutter is the UI SDK built *using* Dart. You can write Dart with no Flutter at all.

| **Dart API** | **Flutter API** |
|---|---|
| Built-in libraries of the Dart **language** itself | SDK **classes & widgets** that build the UI |
| Not tied to any UI | Also handle the widget lifecycle |
| `dart:core` → String, int, List, Map, print() | Platform-agnostic — same code everywhere |
| `dart:async`, `dart:convert`, `dart:math` | `StatelessWidget`, `Scaffold`, `Text`… |

**A plug-in = Dart + native code.** It extends your app to features the SDK alone can't reach, installed from **pub.dev**:

```
Your Dart code  →  Platform Channel  →  Native OS
 (the Flutter app)    (the bridge)      (Kotlin/Java · Swift)
```

Examples: **camera · geolocator (GPS) · Firebase · webview_flutter · google_maps_flutter.**

### Flutter API vs Flutter plug-in

| | **Flutter API** | **Flutter plug-in** |
|---|---|---|
| Code base | Written **entirely in Dart** | **Dart + native** (Kotlin/Java, Swift) |
| Purpose | Add Dart-level UI & logic | Access **platform APIs & device hardware** |
| Platform | Same on all platforms | Platform-specific — uses **Platform Channels** |
| Examples | `http`, `intl` date formatting | camera, geolocator, Firebase, webview |

---

## Concept 7: webview_flutter

Show live web content **inside** your Flutter app. **Two parts work together:**

| **WebViewWidget** | **WebViewController** |
|---|---|
| The **UI** — the body of the screen | The **logic** — the "brain" |
| Renders the web page's pixels | `loadRequest()`, `setJavaScriptMode()` |
| Android: WebView · iOS: WKWebView | Handles navigation, JS & cookies |
| Declared in `build()` | Created as a field / in `initState()` |

**Rule of thumb:** declare the **controller first**, then hand it to the **widget** in `build()`. On `WebViewWidget`, **`controller` is a required parameter** — you cannot create the widget without one.

```dart
import 'package:webview_flutter/webview_flutter.dart';

final WebViewController wvController = WebViewController()   // the brain
  ..loadRequest(Uri.parse('https://www.sp.edu.sg'))         // load the page
  ..setJavaScriptMode(JavaScriptMode.unrestricted);         // allow JavaScript

Widget build(BuildContext context) => Scaffold(
  appBar: AppBar(title: const Text('Flutter WebView')),
  body: WebViewWidget(controller: wvController),            // the body
);
```

### Three config steps before you code

1. **`pubspec.yaml`** — add the dependency: `webview_flutter: ^4.x` (or `flutter pub add webview_flutter`).
2. **Android `build.gradle`** — set `ndkVersion "27.0.12077973"` and `minSdk = 21`.
3. **`AndroidManifest.xml`** — add the INTERNET permission:
   `<uses-permission android:name="android.permission.INTERNET"/>`

---

## Concept 8: The `..` Cascade Operator (Dart recall)

The **two dots `..`** are the **cascade operator** — call several methods on the **same object**. The cascaded version **returns the object itself**, so you can create and configure it in one expression.

```dart
// Without cascade                        // With cascade
final c = WebViewController();            final c = WebViewController()
c.loadRequest(Uri.parse('...'));           ..loadRequest(Uri.parse('...'))
c.setJavaScriptMode(...);                  ..setJavaScriptMode(...);
```

Both do the same thing.

---

## Concept 9: google_maps_flutter

Embed an interactive Google Map. Again, **two parts:**

| **GoogleMap (widget)** | **GoogleMapController** |
|---|---|
| The UI / view in the widget tree | The controller / logic |
| **Declarative** — defined at build time | **Imperative** — called via methods |
| Shows tiles, handles zoom & drag | Move camera, query, add markers |

**Watch out:** the controller is **null until `onMapCreated` fires** — declare it **`late`** and don't touch it before then.

> **Contrast with WebView:** unlike `WebViewWidget` (where `controller` *is* required), the **`GoogleMap` widget does NOT take `controller` as a constructor parameter.** You don't pass a controller in — you *receive* one through the **`onMapCreated`** callback. The one parameter `GoogleMap` really requires is **`initialCameraPosition`**.

### GoogleMap — key parameters

| Parameter | What it does |
|---|---|
| **`initialCameraPosition`** | **Required.** Start position + zoom (a `CameraPosition`) |
| **`onMapCreated`** | Callback that hands you the `GoogleMapController` |
| **`markers`** | A **`Set<Marker>`** of points of interest |
| **`mapType`** | normal / satellite / terrain / hybrid |
| **`myLocationEnabled`** | Shows the blue "my location" dot & button |
| **`trafficEnabled`** | Overlays live traffic |
| **`zoomGesturesEnabled`** | Lets users pinch to zoom |

```dart
late GoogleMapController mapController;                      // null until ready
final LatLng _SG = const LatLng(1.3521, 103.8198);          // Singapore
void _onMapCreated(GoogleMapController c) => mapController = c;

body: GoogleMap(
  onMapCreated: _onMapCreated,
  initialCameraPosition: CameraPosition(target: _SG, zoom: 11),
  markers: {
    Marker(markerId: MarkerId('SG'), position: _SG),
  },
),
```

**Setup extras:** get a **Google Maps API key** (Google Cloud Console → APIs & Services → Credentials), then wire it into `AndroidManifest.xml` (`com.google.android.geo.API_KEY`) and, for web, `web/index.html`. **Security:** restrict the key; never commit an unrestricted key to a public repo.

---

## Concept 10: Passing Data During Navigation

**Four ways to pass data** between screens:

| Way | How |
|---|---|
| **★ Constructor arguments** (used in this course) | Pass data straight into the next screen's **constructor** |
| Named routes | `pushNamed` with an arguments map; read via `ModalRoute.of(context).settings.arguments` |
| Future results | Get data **back**: `await Navigator.push(...)`, return it with `Navigator.pop(context, result)` |
| State management | Provider / Riverpod / Bloc — any screen reads shared state |

### Constructor-argument pattern

```dart
// Screen A — send data forward
onTap: () => Navigator.push(context, MaterialPageRoute(
  builder: (context) => WebViewApp(loc: 'Portland'),   // pass it in
));

// Screen B — receive & STORE it
class WebViewApp extends StatefulWidget {
  WebViewApp({super.key, String loc = 'Singapore'}) {
    locationName = loc;                                 // FIX: actually store it
  }
}
```

**Common pitfall:** if you forget `locationName = loc;`, the argument is **discarded** and the screen always shows the default (Singapore). *(The idiomatic Flutter way is a `final` field read as `widget.locationName`.)*

Also in a `StatefulWidget`, in `initState()` **call `super.initState()` first**.

**Capstone (Ch 6):** GoogleMap **marker `onTap`** → `Navigator.push` passing `loc` → **WebViewApp** loads the matching URL.

---

# CHAPTER 7 — Microcontroller (Bluetooth) Communication

## Concept 11: The Goal & Bluetooth Basics

**Goal:** transfer data between a **microcontroller (Arduino)** and an **Android device** using **Bluetooth** wireless communication.

**Bluetooth** is a popular wireless protocol because it is **low power, low cost and lightweight.** It offers several communication methods called **"Profiles":**

| Profile | Meaning |
|---|---|
| **RFComm** | Radio-frequency serial communication |
| **GATT** | General Attribute Profile — for **Bluetooth Low Energy (BLE)** |
| **SPP** | **Serial Port Profile** — in **Classic Bluetooth** (this is what we use) |

---

## Concept 12: Requirements & Hardware

**Requirements:** an Android device that supports **Bluetooth Serial**; an **Arduino** (Nano / Micro / Uno) + USB cable; the **Arduino IDE**; **Android Studio**; and an **HC-05 / HC-06 Bluetooth transceiver**.

**Hardware setup:** wire the **HC-05 / HC-06** to the Arduino, using a **voltage divider (10 kΩ + 20 kΩ)** on the line into the module's RX. The **COM port** number in Device Manager depends on which USB port the Arduino is plugged into.

---

## Concept 13: The Arduino Sketch

```cpp
#include <SoftwareSerial.h>
SoftwareSerial mySerial(8, 9);   // Rx, Tx of Arduino
// Arduino Pin 8 (Rx) -> Tx of HC05/06 ; Pin 9 (Tx) -> Rx of HC05/06

void setup() {
  Serial.begin(9600);           // USB serial monitor
  while (!Serial) { ; }          // wait (native USB only)
  mySerial.begin(9600);         // talk to HC05/06
}

void loop() {
  if (mySerial.available()) {   // a byte arrived from the phone (via the HC05/06 Bluetooth link)
    char c = mySerial.read();
    Serial.print(c);            // show it on the PC Serial Monitor
  }
  if (Serial.available()) {     // a byte was typed into the PC Serial Monitor
    char c = Serial.read();
    mySerial.print(c);          // send it out to the phone (via HC05/06)
    Serial.print(c);            // echo it back on the monitor
  }
}
```

**Two serial ports:** `Serial` is the built-in **USB link to the PC** (the Serial Monitor); `mySerial` (via `SoftwareSerial` on pins 8 & 9) is the **link to the HC-05/06**, which reaches the phone over Bluetooth. Data hops from one to the other. **Key facts:** the data rate is **9600 baud** on both.

---

## Concept 14: Pairing & the Terminal App

- Install **Serial Bluetooth Terminal** (by Kai Morich) from the Google Play Store.
- Pair over **Bluetooth Classic**: scan, find the device named **`MAD_HC06_Addr1:Addr0`**, pair with **password `1234`** (some modules use `0000`).
- In the Arduino **Serial Monitor**: set **9600 baud** and **"Both NL & CR"**.
- After a **successful connection**, the **HC-06 red light remains on** (stops blinking).

> **Must pair first.** Two Bluetooth devices must **pair successfully before they can be connected** — pairing (exchanging the passcode) is the prerequisite for any data connection.

---

## Concept 15: The Android Bluetooth Stack

From top to bottom:

1. **Application Framework** — your Apps + the `android.bluetooth` package. (Connected via **Binder**.)
2. **Bluetooth Process** — the **Bluetooth Service** + **Bluetooth Profiles** (`packages/apps/Bluetooth`).
3. **JNI** — the Java ↔ native bridge (`packages/apps/Bluetooth/jni`).
4. **Bluetooth Stack** (`system/bt`) — Bluetooth App Layer + Bluetooth Embedded System, talking over **HIDL** to the **Vendor Implementation** → the **Bluetooth Controller** (the radio hardware).

### How a Flutter app reaches Bluetooth

A Flutter app communicates via platform-specific **MethodChannels** to the **Android Framework**, which then talks to the native Bluetooth stack:

| Layer | Component | Function |
|---|---|---|
| **App** | Flutter (Dart) | UI and business logic |
| **Bridge** | Platform Channels (MethodChannels) | Marshals data between Dart and Java/Kotlin |
| **Framework** | `BluetoothAdapter` | Java APIs to manage the local Bluetooth radio |
| **Profile** | SPP (RFCOMM) | The specific protocol for serial data transfer |
| **Native** | JNI | C++ stack that handles the radio and HCI commands |

> **Dart does NOT touch the hardware directly.** Flutter Dart code **cannot** reach the Bluetooth hardware / JNI layer directly. It always goes **through the bridge**: Dart → **Platform Channels** → **BluetoothAdapter (Java framework)** → native stack → JNI. JNI sits at the bottom; Dart only talks to it *indirectly*, several layers up.

---

## Concept 16: AT Commands (programming the HC-05 / HC-06)

To program the module you use a set of commands called **AT commands.** Every "set" command returns **`"OK"`** when it executes successfully.

| AT Command | What it does |
|---|---|
| `AT` | Check connection status |
| `AT+NAME="ModuleName"` | Set a name for the device |
| `AT+ADDR` | Check the MAC address |
| `AT+UART` | Check the baud rate |
| `AT+UART="9600"` | Set the baud rate to 9600 |
| `AT+PSWD` | Check the default passcode |
| `AT+PSWD="1234"` | Set the passcode to 1234 |

---

# PART 2 — CHEAT SHEET (Memorise These!)

| Fact | Answer |
|---|---|
| Screens/pages in Flutter are called | **routes** |
| A route ≈ … (Android / iOS) | **Activity / ViewController** |
| Small app, no deep linking → use | **Navigator** widget |
| Deep linking / web address bar → use | **Router** widget |
| `Navigator` is a child class of | **StatefulWidget** |
| `Navigator.push` | Adds a route to the **top** of the stack |
| `Navigator.pop` | Removes the **current** route (go back) |
| Exit the whole Flutter app | `SystemNavigator.pop();` (needs `services.dart`) |
| `BuildContext` is | An **abstract class** — handle to a widget's location in the tree |
| `MaterialPageRoute` is a child of | **`Route`**; its required param is **`builder`** |
| Most common scrolling widget | **`ListView`** — avoids **render overflow** |
| Small fixed list → | default **`ListView(children: [...])`** |
| Large / dynamic list → | **`ListView.builder(itemCount:, itemBuilder:)`** (memory-efficient) |
| Default `scrollDirection` | `Axis.vertical` |
| Detects taps/gestures | **`GestureDetector`** (`onTap`, `child`) |
| GestureDetector with a child | **defers to the child** for sizing (else fills parent) |
| API stands for | **Application Programming Interface** |
| Dart API examples | `dart:core`, `dart:async`, `dart:convert`, `dart:math` |
| Plug-in = | **Dart + native code**, bridged by a **Platform Channel**, from **pub.dev** |
| Is the Dart API part of the Flutter API? | **No** — two separate APIs |
| Is a Flutter plug-in platform-specific? | **Yes** — Dart + native, via Platform Channels |
| WebView: UI part / brain part | **WebViewWidget** / **WebViewController** |
| Is `controller` required on `WebViewWidget`? | **Yes** — required parameter |
| Controller methods | `loadRequest()`, `setJavaScriptMode()` |
| WebView needs (manifest) | INTERNET permission; build.gradle minSdk **21** |
| `..` means | **cascade** — many methods on the same object; returns the object |
| Map: UI widget / controller | **GoogleMap** / **GoogleMapController** |
| Is `controller` a param of `GoogleMap`? | **No** — you get it from `onMapCreated` |
| Map needs (manifest) | Google Maps **API key** (`com.google.android.geo.API_KEY`) |
| Map controller is null until | **`onMapCreated`** fires → declare it **`late`** |
| Required GoogleMap param | **`initialCameraPosition`** |
| `markers` type | **`Set<Marker>`** |
| Pass data forward (this course) | **Constructor arguments** |
| Get data back | `await Navigator.push(...)` + `Navigator.pop(context, result)` |
| Bluetooth we use (Classic) | **SPP** = Serial Port Profile (over **RFCOMM**) |
| BLE profile | **GATT** |
| Bluetooth profiles (know these) | SPP, RFComm, GATT — **WiFi is NOT one** |
| Must pair before connecting? | **Yes** |
| Bluetooth data rate | **9600 baud** |
| HC-05/06 pairing password | **1234** |
| Does Dart reach the BT hardware / JNI directly? | **No** — through Platform Channels first |
| AT commands return … on success | **`"OK"`** |
| Set module name / baud / passcode | `AT+NAME=` / `AT+UART="9600"` / `AT+PSWD="1234"` |
| Flutter ↔ native bridge | **Platform Channels / MethodChannels** |

---

# PART 3 — NIGHT-BEFORE RECAP

Read these out loud:

1. **Screens = routes. Navigator shows them as a stack.**
2. **`push` = add to top. `pop` = remove current (go back).**
3. **`Navigator` is a StatefulWidget. `SystemNavigator.pop()` exits the app.**
4. **`BuildContext` = abstract class, a handle to a widget's spot in the tree.**
5. **`MaterialPageRoute` extends `Route`; its required param is `builder`.**
6. **`ListView` is the go-to scrolling widget — it stops render overflow.**
7. **Fixed list → `ListView(children:)`. Big/dynamic → `ListView.builder(itemCount, itemBuilder)`.**
8. **`GestureDetector` detects `onTap`; with a child it sizes to the child.**
9. **API = Application Programming Interface. Plug-in = Dart + native (Platform Channel).**
10. **WebView = WebViewWidget (UI) + WebViewController (brain: loadRequest, setJavaScriptMode).**
11. **`..` is the cascade operator — config an object in one expression.**
12. **Map = GoogleMap (widget) + GoogleMapController; controller is `late`, ready on `onMapCreated`.**
13. **`initialCameraPosition` is required; `markers` is a `Set<Marker>`.**
14. **Pass data forward with constructor arguments; get it back with `Navigator.pop(context, result)`.**
15. **Bluetooth Classic = SPP over RFCOMM. BLE = GATT.**
16. **HC-05/06 talks at 9600 baud; pairing password is 1234.**
17. **AT commands configure the module and return "OK". `AT+NAME`, `AT+UART="9600"`, `AT+PSWD="1234"`.**
18. **Flutter reaches native Bluetooth through Platform Channels → BluetoothAdapter → SPP/RFCOMM → JNI.**

You've got this. 🚀
