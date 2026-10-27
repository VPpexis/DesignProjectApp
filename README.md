# Plant Shop Application

The Flutter mobile-app archive for **IoT-based Plant Guide, Inventory and Billing System for a Plant Shop**, a 2022 Computer Engineering design project at Mapúa University.

The project combines a handheld barcode-scanning device with customer and seller app interfaces. Customers can explore plant information and their shopping list; sellers can access inventory and transaction views.

This fork preserves the original application source from [ckdirecto/DesignProjectApp](https://github.com/ckdirecto/DesignProjectApp). It is an earlier development snapshot, with unfinished integrations, rather than a verified release of the complete system documented in the final report.

## Project at a glance

| Part | Technology / purpose |
| --- | --- |
| Mobile application | Flutter and Dart |
| Cloud integration | Firebase Authentication and Realtime Database; Cloud Firestore is also declared as a dependency |
| Hardware companion | ATmega328P, ESP8266 / NodeMCU, EAN-8 barcode scanner, 20×4 LCD, and operation buttons |
| System operations | View plant information, add items, remove items, and confirm an order for billing |
| Firmware archive | [VPpexis/plant-shop-firmware](https://github.com/VPpexis/plant-shop-firmware) — separate private repository; access required |

The hardware used **ESP8266**, not ESP32. The report recommends ESP32 as a possible future upgrade. The `web/` folder is a Flutter platform scaffold; this repository is primarily the mobile-app source.

## How the system fits together

```text
Barcode + operation buttons
             |
       ATmega328P + LCD
             |
       ESP8266 / NodeMCU
             |
           Firebase
             |
   Customer / seller mobile app
```

The final report describes a generated PIN for the customer's shopping session, detailed plant information, a running bill, and seller inventory and order records. Confirming checkout creates the order/receipt and updates stock; the customer still pays at the cashier.

These are full-system design goals and documented prototype behavior. Some corresponding screens and data flows in this archived app remain incomplete.

## What is in this repository

```text
lib/
  main.dart                 App entry point and Firebase initialization
  main1.dart                Seller navigation and database-view entry points
  Screens/
    Welcome/                Customer / seller selection
    LoginCustomer/          Customer PIN interface
    LoginSeller/            Seller authentication
    PlantList/              Plant catalog and shopping-list screens
    Inventory/              Inventory-related screens
    Transactions/           Transaction-related screens
    Receipt/                Receipt-related source
  realtime_db/              Firebase inventory and transaction queries
  components/               Shared UI and data helpers
assets/                     Plant images, icons, and branding
fonts/                      Bundled typefaces
android/  ios/  web/         Flutter platform projects
test/                       Original template widget test
```

Microcontroller sketches, circuit files, and the complete Firebase database are not included here. The firmware is maintained separately.

## Local setup

### 1. Prepare a compatible toolchain

This is a legacy Flutter project. `pubspec.yaml` specifies **Dart `>=2.12.0 <3.0.0`**, so use a compatible Flutter SDK that bundles Dart 2. The archive does not pin an exact Flutter release or commit a dependency lockfile.

For Android, install the Android SDK and an emulator or connect a development device. The checked-in project targets Android API 30, with minimum API 21, and uses Android Gradle Plugin 4.1.0. Newer Flutter and Android toolchains may require a separate migration.

```sh
git clone https://github.com/VPpexis/DesignProjectApp.git
cd DesignProjectApp
flutter doctor
flutter pub get
```

### 2. Configure your own Firebase project

- Register an Android app matching `com.example.firebase_trial_app`, or update the application ID and Firebase registration together.
- Replace `android/app/google-services.json` with the configuration for your development project. The checked-in file belongs to the historical setup.
- Enable Email/Password authentication and create your own seller test account.
- Create a Realtime Database and supply development data compatible with the queries in `lib/realtime_db/`. The archive references `Plant_Inventory` and `Transaction`; it does not include a database export or rules file.
- Configure iOS separately before attempting an iOS build. No `GoogleService-Info.plist` is included.

Use your own development accounts and data. Do not rely on historical credentials in comments or commit passwords or service-account private keys. Configuration alone will not complete the unfinished app flows.

### 3. Run on an Android device

```sh
flutter devices
flutter run -t lib/main.dart -d <device-id>
```

The intended entry point is `lib/main.dart`. Once the legacy environment and integrations are working, an Android APK can be built with `flutter build apk`. The archived release build still uses debug signing and needs a proper signing configuration before distribution.

## Archive status and limitations

- The plant catalog in `lib/Screens/PlantList/plant_model.dart` is temporary local data.
- Customer PIN verification currently routes to the shop screen without validating the PIN against the fetched transaction IDs.
- Firebase inventory and transaction list builders contain placeholder `ListTile` output.
- The original widget test still expects Flutter's counter-template UI and does not validate this application.
- The Drive firmware variants and this app snapshot use different transaction paths in places. Reconcile the message protocol and database schema before connecting them.
- The report mentions a later NativeScript-Vue implementation, while this repository contains Flutter/Dart. This archive does not establish the source of that later build.

The code has not been rebuilt or tested against a live Firebase project as part of the README update. To assess a restored environment, start with `flutter analyze`, replace the template test with meaningful app tests, and verify the device-to-cloud workflow with development data.

## Team and provenance

**Marie Krystine D. Anticuando · Claudee Khiarra R. Directo · Van Philip M. Panugan**
BS Computer Engineering, Mapúa University — May 2022
Design adviser and client: Engr. Analyn N. Yumang

- [Original application repository](https://github.com/ckdirecto/DesignProjectApp)
- [Firmware archive](https://github.com/VPpexis/plant-shop-firmware) — private; access required

The original history and team attribution are preserved. No new license is asserted by this README; the archive currently has no license file granting reuse rights.

