# MAD Class Test 2 — Practice Questions

A 30-question set covering **Chapters 5, 6 & 7**. Same concepts you'll be tested on, **different wording, code and values** — so you practise *understanding*, not memorising. Each question has a collapsible answer: try it closed-book first, then click **Answer**.

> Best viewed on GitHub or any Markdown viewer that supports `<details>`.

---

## Chapter 5 — Navigation & More Widgets

###### 1. In Flutter, what are the individual screens/pages of an app called?

- A: Activities
- B: Routes
- C: Frames
- D: Scenes

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

In Flutter, screens and pages are called **routes**. The `Navigator` manages these routes as a stack.

| Platform | A route is like… |
|---|---|
| Android | an **Activity** |
| iOS | a **ViewController** |

So "Activity" (A) describes the *Android* equivalent — but the Flutter term itself is **route**.

</p>
</details>

---

###### 2. What does `Navigator.push(context, route)` do?

- A: Removes the current screen and goes back
- B: Adds a route to the **top** of the navigation stack
- C: Exits the app
- D: Replaces every route on the stack

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`Navigator.push(...)` **adds a route to the top of the stack** — it moves the user *forward* to a new screen.

| Method | Effect |
|---|---|
| `push` | Add a route to the **top** |
| `pop` | Remove the **current** (top) route — go back |
| `pushReplacement` | Swap the current top route for a new one |
| `pushAndRemoveUntil` | Push, then pop until a condition is met |

To **exit the whole Flutter app**, use `SystemNavigator.pop();` (needs `import 'package:flutter/services.dart';`).

</p>
</details>

---

###### 3. The `Navigator` widget is a child class of which type?

- A: `StatelessWidget`
- B: `State`
- C: `StatefulWidget`
- D: `MaterialApp`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`Navigator` is a **child class of `StatefulWidget`**. It displays screens as a **stack**, using the correct transition animation for the target platform, and you drive it with **imperative** methods like `push()` and `pop()`.

</p>
</details>

---

###### 4. Which class must you provide a `builder` to when pushing a new screen, and what is its parent class?

```dart
Navigator.push(context, __________(
  builder: (context) => const DetailScreen(),
));
```

- A: `PageRoute` — parent is `Widget`
- B: `MaterialPageRoute` — parent is `Route`
- C: `MaterialApp` — parent is `StatelessWidget`
- D: `Scaffold` — parent is `Route`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`MaterialPageRoute` is a **modal route** with a platform-adaptive transition. It is a **child class of `Route`** (the abstract class for an entry managed by a `Navigator`).

Its **required named parameter `builder`** takes a function that builds and returns the route's widget:

```dart
typedef Widget WidgetBuilder(BuildContext context);
```

</p>
</details>

---

###### 5. `BuildContext` is best described as…

- A: A widget that draws pixels on screen
- B: An abstract class — a handle to a widget's location in the widget tree
- C: The list of all routes in the app
- D: A plug-in for accessing native code

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`BuildContext` is an **abstract class** — a **handle to the location of a widget in the widget tree**. Every widget has its own `BuildContext`, and static helpers like `Navigator.of(context)`, `Theme.of(context)` and `showDialog(...)` take it so they can act on behalf of the calling widget.

</p>
</details>

---

###### 6. You have 40 items to show in a scrolling list and the screen isn't tall enough. Which widget avoids the "BOTTOM OVERFLOWED" render error?

- A: `Column`
- B: `Row`
- C: `ListView`
- D: `Center`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`ListView` is the **most commonly used scrolling widget**. It lays its children out one after another in the scroll direction and **avoids the render-overflow** problem (the yellow-and-black striped bar) when everything can't fit on screen at once. `Column` and `Row` do **not** scroll, so they overflow.

</p>
</details>

---

###### 7. Which pair of parameters do you supply to `ListView.builder` (for a large / dynamic list)?

- A: `children:` and `scrollDirection:`
- B: `itemCount:` and `itemBuilder:`
- C: `child:` and `builder:`
- D: `count:` and `children:`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`ListView.builder` is built for **large / dynamic** lists and is memory-efficient — it only builds the items currently visible (plus a few nearby). It uses **`itemCount`** (how many) and **`itemBuilder`** (how to build each one) — **not** `children`.

```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, i) => Text(items[i]),
);
```

The **default** `ListView(children: [...])` is the one that takes a fixed list of `children` and loads them all at once.

> *Precise note:* of the two, **only `itemBuilder` is strictly required** — `itemCount` just tells the list how many items exist (omit it and the list is treated as infinite). But among the options here, `itemCount` + `itemBuilder` is the correct pairing, because `children` belongs to the *default* `ListView`, not `.builder`.

</p>
</details>

---

###### 8. About `GestureDetector`, which statement is TRUE?

- A: It can only detect `onTap`, nothing else
- B: If it has a child, it defers to that child for its sizing behaviour
- C: It must always be the root widget of a screen
- D: It draws its own visible border by default

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`GestureDetector` detects many gestures (`onTap`, `onDoubleTap`, `onLongPress`, drag/pan/scale, …).

- **If it has a child**, it **defers to that child for its sizing** behaviour.
- **If it has no child**, it **grows to fit the parent**.
- It is invisible — it only listens for gestures; it doesn't paint anything itself.

</p>
</details>

---

## Chapter 6 — Plug-ins & More Navigation

###### 9. What does **API** stand for?

- A: Apple Interface
- B: Application Programming Interface
- C: Android Plug-in Integration
- D: Automated Package Installer

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

**API = Application Programming Interface** — a set of rules that lets two pieces of software talk to each other (the *Application*, the *Interface* rules, and the *Documentation*).

</p>
</details>

---

###### 10. **True or False:** The Dart API is a part of the Flutter API.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B — False

They are **two separate APIs**. The **Dart API** belongs to the Dart *language* (`dart:core`, `dart:async`, `dart:convert`, `dart:math`), and works with no UI at all. The **Flutter API** is the *UI SDK* (widgets like `Scaffold`, `Text`) built **using** Dart. One is not a subset of the other.

</p>
</details>

---

###### 11. **True or False:** A Flutter plug-in is platform-specific.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

A **plug-in = Dart + native code** and reaches platform features through a **Platform Channel**, so it is **platform-specific** (needs Kotlin/Java for Android, Swift for iOS). Contrast this with a pure **Flutter API** package (e.g. `http`, `intl`), which is written entirely in Dart and behaves the **same on all platforms**.

</p>
</details>

---

###### 12. A plug-in bridges your Dart code to the native OS. What is that bridge called?

- A: A REST endpoint
- B: A Platform Channel
- C: A Widget tree
- D: A Route

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

```
Your Dart code  →  Platform Channel  →  Native OS
 (the Flutter app)    (the bridge)      (Kotlin/Java · Swift)
```

Plug-ins are installed from **pub.dev**. Examples: camera, geolocator (GPS), Firebase, `webview_flutter`, `google_maps_flutter`.

</p>
</details>

---

###### 13. In the `webview_flutter` plug-in, which class is the **UI (view)** and which is the **controller (logic)**?

- A: (UI) `WebViewController`  ·  (Controller) `WebViewWidget`
- B: (UI) `WebViewWidget`  ·  (Controller) `WebViewController`
- C: Both are `WebViewController`
- D: (UI) `webview`  ·  (Controller) `flutter`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

| `WebViewWidget` (UI) | `WebViewController` (brain) |
|---|---|
| The body of the screen; renders the page's pixels | `loadRequest()`, `setJavaScriptMode()` |
| Android: WebView · iOS: WKWebView | Handles navigation, JS & cookies |
| Declared in `build()` | Created as a field / in `initState()` |

Declare the **controller first**, then hand it to the **widget** in `build()`.

</p>
</details>

---

###### 14. **True or False:** When you create a `WebViewWidget`, the `controller` argument is required.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

`WebViewWidget` **requires a `controller`** — the widget is just the view; the `WebViewController` holds all the logic (which URL to load, JavaScript mode, etc.), so the widget can't function without one.

```dart
body: WebViewWidget(controller: wvController),   // controller is required
```

</p>
</details>

---

###### 15. **True or False:** To use `webview_flutter` on Android, you must add `<uses-permission android:name="android.permission.INTERNET" />` to `AndroidManifest.xml`.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

A WebView loads content over the network, so the app needs the **INTERNET permission** declared in `AndroidManifest.xml`. Without it the page won't load on a device.

</p>
</details>

---

###### 16. Which steps are needed to use a Flutter plug-in that accesses Android features? *(Select all that apply.)*

- A: Add the plug-in as a dependency in `pubspec.yaml`
- B: Configure `ndkVersion` and `minSdk` in `android/app/build.gradle`
- C: Add the plug-in's required info (e.g. permissions / API key) in `AndroidManifest.xml`
- D: Import the plug-in's Dart file in your source code

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A, B, C **and** D — all four

To use a plug-in that accesses Android features, you need **all four** steps:

1. **`pubspec.yaml`** — add the dependency (or `flutter pub add <plugin>`). **(A)**
2. **`android/app/build.gradle`** — set `ndkVersion` and `minSdk` (e.g. 21). **(B)**
3. **`AndroidManifest.xml`** — add the plug-in's required info: INTERNET permission for WebView, or the Maps **API key** for `google_maps_flutter`. **(C)**
4. **`import`** the plug-in's Dart file(s) in your source code so you can call the API. **(D)**

Steps 1–3 are the project-configuration steps; step 4 is the code step. **All four are required** — leave any one out and the plug-in won't work.

</p>
</details>

---

###### 17. What does the Dart cascade operator `..` do?

```dart
final c = WebViewController()
  ..loadRequest(Uri.parse('https://sp.edu.sg'))
  ..setJavaScriptMode(JavaScriptMode.unrestricted);
```

- A: Divides two numbers
- B: Calls several methods on the **same object** and returns that object
- C: Creates a range of values
- D: Declares a nullable type

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The **two dots `..`** are the **cascade operator** — they let you call several methods on the **same object** without repeating its name. The cascade **returns the object itself**, so you can create *and* configure it in one expression.

</p>
</details>

---

###### 18. In the `google_maps_flutter` plug-in, which class is the **widget** and which is the **controller**?

- A: (Widget) `GoogleMapView`  ·  (Controller) `GoogleMapController`
- B: (Widget) `GoogleMap`  ·  (Controller) `GoogleMapViewController`
- C: (Widget) `GoogleMap`  ·  (Controller) `GoogleMapController`
- D: (Widget) `GoogleMapViewController`  ·  (Controller) `GoogleMapWidget`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

| `GoogleMap` (widget) | `GoogleMapController` |
|---|---|
| The UI/view in the tree; **declarative** | The logic; **imperative** |
| Shows tiles, handles zoom & drag | Move camera, query, add markers |

The controller is **null until `onMapCreated` fires** — declare it **`late`**.

</p>
</details>

---

###### 19. **True or False:** `controller` is **not** a required parameter of the `GoogleMap` widget constructor.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

Unlike `WebViewWidget`, the **`GoogleMap` widget doesn't take a `controller` at all** — you don't pass one in. Instead you **receive** the `GoogleMapController` through the **`onMapCreated`** callback after the map is built. The parameter `GoogleMap` really **requires** is **`initialCameraPosition`**.

```dart
GoogleMap(
  initialCameraPosition: CameraPosition(target: _SG, zoom: 11),  // required
  onMapCreated: (c) => mapController = c,                        // controller arrives here
);
```

</p>
</details>

---

###### 20. **True or False:** To use `google_maps_flutter` on Android, a valid Google Maps API key must be added to `AndroidManifest.xml`.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

The Android build needs the key as a `meta-data` entry in `AndroidManifest.xml`:

```xml
<meta-data
  android:name="com.google.android.geo.API_KEY"
  android:value="YOUR-KEY"/>
```

You obtain the key from the **Google Cloud Console → APIs & Services → Credentials**. Keep it **restricted** and never commit an unrestricted key.

</p>
</details>

---

###### 21. What is the type of the `markers` parameter on `GoogleMap`?

- A: `List<Marker>`
- B: `Map<String, Marker>`
- C: `Set<Marker>`
- D: `Marker`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`markers` takes a **`Set<Marker>`** — a set of points of interest. Each marker needs a unique `markerId` and a `position` (a `LatLng`):

```dart
markers: {
  Marker(markerId: MarkerId('SG'), position: LatLng(1.3521, 103.8198)),
},
```

</p>
</details>

---

###### 22. Which of these are ways Flutter offers to **pass data during navigation**? *(Select all that apply.)*

- A: Constructor arguments
- B: Named routes (arguments map)
- C: Future results (`Navigator.pop(context, result)`)
- D: State management (Provider / Riverpod / Bloc)

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A, B, C, D — all four

| Way | How |
|---|---|
| **Constructor arguments** ★ | Pass data straight into the next screen's constructor (used in this course) |
| **Named routes** | `pushNamed` with an arguments map; read via `ModalRoute.of(context).settings.arguments` |
| **Future results** | `await Navigator.push(...)`, then return with `Navigator.pop(context, result)` |
| **State management** | Provider / Riverpod / Bloc — shared state any screen can read |

</p>
</details>

---

###### 23. Screen A opens Screen B and needs to send it a location string. Using the constructor-argument approach, which is correct?

- A: `Navigator.push(context, MaterialPageRoute(builder: (context) => DetailScreen(loc: 'Batam')));`
- B: `Navigator.pop(context, 'Batam');`
- C: `runApp(DetailScreen('Batam'));`
- D: `Navigator.push('Batam');`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

To pass data **forward**, put it in the next screen's constructor inside the route `builder`:

```dart
onTap: () => Navigator.push(context, MaterialPageRoute(
  builder: (context) => DetailScreen(loc: 'Batam'),   // pass it in
));
```

The receiving screen must **store** the value (e.g. `locationName = loc;` or read it as `widget.locationName`). If you forget to store it, the argument is discarded and the screen shows its default.

`Navigator.pop(context, result)` (B) sends data **backward**, not forward.

</p>
</details>

---

## Chapter 7 — Microcontroller (Bluetooth) Communication

###### 24. Which of these is **NOT** a Bluetooth profile?

- A: SPP
- B: RFComm
- C: WiFi
- D: GATT

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

**WiFi** is a completely separate wireless technology, **not** a Bluetooth profile.

| Profile | Meaning |
|---|---|
| **SPP** | Serial Port Profile — Classic Bluetooth (what we use) |
| **RFComm** | Radio-frequency serial communication |
| **GATT** | General Attribute Profile — for Bluetooth Low Energy (BLE) |

</p>
</details>

---

###### 25. To connect an Android phone to the HC-06 over Classic Bluetooth for serial data, which profile is used, and what is BLE's profile?

- A: Classic uses GATT; BLE uses SPP
- B: Classic uses SPP (RFComm); BLE uses GATT
- C: Both use WiFi
- D: Classic uses HCI; BLE uses RFComm

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Serial data over **Classic Bluetooth** uses the **SPP (Serial Port Profile)**, which runs over **RFComm**. **BLE (Bluetooth Low Energy)** uses **GATT** instead.

</p>
</details>

---

###### 26. **True or False:** Two Bluetooth devices must be paired successfully before they can be connected to exchange data.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A — True

**Pairing comes first.** The two devices must pair (exchange the passcode — e.g. `1234` for the HC-06) before any data connection can be established.

</p>
</details>

---

###### 27. **True or False:** Flutter Dart code accesses the Android device's Bluetooth hardware layer directly through JNI.

- A: True
- B: False

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B — False

Dart **cannot** reach the hardware / JNI directly. It goes **through the bridge**:

```
Flutter (Dart)  →  Platform Channels  →  BluetoothAdapter (Java framework)
                →  SPP / RFCOMM  →  JNI (native C++ stack)  →  radio
```

**JNI is the bottom native layer.** Dart only talks to it *indirectly*, several layers up — the first hop is always a **Platform Channel**.

</p>
</details>

---

###### 28. When you send AT commands to configure an HC-05 / HC-06, what does a successful command return?

- A: `DONE`
- B: `OK`
- C: `1`
- D: `SUCCESS`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Every "set" AT command returns **`"OK"`** on success.

| Command | Purpose |
|---|---|
| `AT` | Check connection status |
| `AT+NAME="ModuleName"` | Set the device name |
| `AT+ADDR` | Check the MAC address |
| `AT+UART="9600"` | Set the baud rate to 9600 |
| `AT+PSWD="1234"` | Set the passcode to 1234 |

</p>
</details>

---

###### 29. In the Arduino sketch and Serial Monitor for the HC-06, what baud rate is used?

- A: 115200
- B: 300
- C: 9600
- D: 4800

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The course uses **9600 baud**: `Serial.begin(9600);` and `mySerial.begin(9600);`, and the Serial Monitor is set to **9600 baud** with **"Both NL & CR"**. `SoftwareSerial mySerial(8, 9);` creates a second serial port on pins 8 (Rx) and 9 (Tx).

</p>
</details>

---

###### 30. How does a Flutter app reach the native Android Bluetooth stack?

- A: By importing `dart:bluetooth`
- B: Through Platform Channels (MethodChannels) to the Android framework
- C: By writing Kotlin directly inside the Dart file
- D: Over WiFi

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

A Flutter app communicates via platform-specific **MethodChannels / Platform Channels** to the **Android Framework** (`BluetoothAdapter`), which then talks down to the native Bluetooth stack.

| Layer | Component | Function |
|---|---|---|
| App | Flutter (Dart) | UI and business logic |
| **Bridge** | **Platform Channels (MethodChannels)** | Marshals data between Dart and Java/Kotlin |
| Framework | `BluetoothAdapter` | Manages the local Bluetooth radio |
| Profile | SPP (RFCOMM) | Serial data transfer protocol |
| Native | JNI | C++ stack for the radio & HCI commands |

</p>
</details>

---

## Concept Coverage

| Concept | Questions |
|---|---|
| Routes / Navigator / push-pop / exit | 1, 2, 3 |
| MaterialPageRoute & builder | 4 |
| BuildContext | 5 |
| ListView / render overflow / builder | 6, 7 |
| GestureDetector | 8 |
| API meaning · Dart vs Flutter API | 9, 10 |
| Plug-in = platform-specific / Platform Channel | 11, 12 |
| webview_flutter (classes, controller, permission) | 13, 14, 15 |
| Plug-in config steps | 16 |
| Cascade `..` operator | 17 |
| google_maps_flutter (classes, controller, API key, markers) | 18, 19, 20, 21 |
| Passing data during navigation | 22, 23 |
| Bluetooth profiles (SPP/RFComm/GATT, not WiFi) | 24, 25 |
| Pairing before connection | 26 |
| Dart → native (not direct JNI) / Platform Channels | 27, 30 |
| AT commands / baud rate | 28, 29 |

---

## How to use this file

- Try each question closed-book, then expand **Answer**.
- For a mock exam, use **`PRACTICE_QUIZ.md`** (questions grouped, answer key at the bottom).
- Review anything you miss in **`STUDY_GUIDE.md`**.
