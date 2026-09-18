# Rechnenübung

Eine Android-App (Kotlin, Jetpack Compose) für Grundschulkinder zum Üben der vier
Grundrechenarten – mit einem Prüfungsmodus und einem PIN-geschützten
Eltern-Bereich mit Statistiken.

## Funktionen

### Für das Kind
- **Üben**: 10 Aufgaben pro Runde, sofortiges Feedback (richtig/falsch inkl.
  korrekter Lösung), Eingabe über einen großen Ziffernblock.
- **Prüfung**: 5, 10 oder 20 Aufgaben, mit Timer, ohne Zwischenfeedback; am
  Ende eine detaillierte Auswertung mit jeder einzelnen Aufgabe, der eigenen
  Antwort und der Musterlösung.
- Auswahl der Rechenart(en): Plus, Minus, Mal, Geteilt (einzeln oder
  kombiniert).
- Drei Schwierigkeitsstufen (siehe unten).

### Für die Eltern
- PIN-geschützter Bereich (Standard-PIN `1234`, änderbar).
- Statistik-Dashboard: Anzahl absolvierter Prüfungen/Übungsrunden,
  Gesamt-Trefferquote, Genauigkeit je Rechenart, Verlauf aller Sitzungen mit
  Datum und Ergebnis.
- Einstellungen: PIN ändern, Anzahl der Prüfungsaufgaben festlegen, Verlauf
  löschen.

## Schwierigkeitsstufen – der Rahmen

Die App generiert Aufgaben zufällig innerhalb fester Zahlenbereiche. Diese
orientieren sich grob an den Lehrplänen der deutschen Grundschule, sind aber
bewusst einfach gehalten (reines Kopfrechnen, keine Textaufgaben, keine
schriftlichen Rechenverfahren):

| Stufe  | Plus / Minus (Zahlenraum) | Mal / Geteilt (Faktoren) | entspricht etwa |
|--------|---------------------------|--------------------------|-----------------|
| Leicht | 0–20                      | 1–5 (Ergebnis max. 25)   | Klasse 1        |
| Mittel | 0–100                     | 1–10 (kleines Einmaleins)| Klasse 2        |
| Schwer | 0–1000                    | 2–12 (großes Einmaleins) | Klasse 3/4      |

Regeln für kindgerechte Aufgaben:
- **Subtraktion** ergibt nie ein negatives Ergebnis (der erste Operand ist
  immer ≥ dem zweiten).
- **Division** geht immer ohne Rest auf (es werden nur "glatte" Aufgaben
  erzeugt, z. B. 42 ÷ 6, nie 43 ÷ 6).
- Bei kombinierten Rechenarten wird pro Aufgabe zufällig eine der
  ausgewählten Arten gezogen.

**Was die App bewusst nicht abdeckt:** Textaufgaben/Sachrechnen, schriftliche
Rechenverfahren (schriftliche Addition/Subtraktion/Multiplikation/Division),
Rechnen mit Kommazahlen oder Brüchen, Größen (Geld, Zeit, Längen). Die App ist
ein Kopfrechen-Trainer für die vier Grundrechenarten im jeweiligen
Zahlenraum – kein vollständiger Mathe-Lehrgang.

Die genauen Zahlenbereiche lassen sich bei Bedarf leicht anpassen in
[`ProblemGenerator.kt`](app/src/main/java/com/rechnenueben/app/domain/ProblemGenerator.kt).

## Technik

- Kotlin, Jetpack Compose, Material 3
- Navigation Compose für die Bildschirm-Navigation
- Room (SQLite) für den Übungs-/Prüfungsverlauf
- DataStore Preferences für Einstellungen (PIN, Prüfungslänge)
- Kein Backend, keine Internetverbindung nötig – alle Daten bleiben auf dem
  Gerät.
- Min-SDK 24 (Android 7.0), Target-/Compile-SDK 34

## Projekt öffnen / bauen

Voraussetzung: [Android Studio](https://developer.android.com/studio)
(aktuelle Version) oder ein installiertes Android SDK (`compileSdk 34`,
`build-tools 34.0.0`).

```bash
# Debug-APK bauen (zum Testen, unsigniert/mit Debug-Zertifikat)
./gradlew assembleDebug
# Ergebnis: app/build/outputs/apk/debug/app-debug.apk

# Unit-Tests ausführen
./gradlew testDebugUnitTest
```

Alternativ: Repository in Android Studio öffnen und über „Run“ direkt auf
einem Gerät/Emulator starten.

### Signierte Release-APK

Für eine Release-APK wird ein eigener Signier-Schlüssel benötigt. Lokal eine
Datei `keystore.properties` im Projekt-Wurzelverzeichnis anlegen (diese Datei
wird nicht eingecheckt, siehe `.gitignore`):

```properties
storeFile=/pfad/zum/release.keystore
storePassword=...
keyAlias=...
keyPassword=...
```

Anschließend:

```bash
./gradlew assembleRelease
# Ergebnis: app/build/outputs/apk/release/app-release.apk
```

Ohne vorhandene `keystore.properties` lässt sich das Projekt weiterhin bauen,
die Release-Variante ist dann aber unsigniert.

## Projektstruktur

```
app/src/main/java/com/rechnenueben/app/
├── domain/        Aufgabenlogik (Rechenarten, Schwierigkeit, Generator)
├── data/          Room-Datenbank, Repository, Einstellungen (DataStore)
├── navigation/    Navigation zwischen den Screens
└── ui/
    ├── home/      Startbildschirm
    ├── practice/  Übungsmodus
    ├── exam/      Prüfungsmodus
    ├── result/    Prüfungsauswertung
    ├── parent/    Eltern-Bereich (PIN, Statistik, Einstellungen)
    └── components/ wiederverwendbare Compose-Komponenten
```

## Lizenz

Privates Projekt – keine Lizenzangabe.
