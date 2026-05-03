---
icon: rocket
---

# Build, Publish and Release Gradle Modules

### Übersicht

Der **Build, Publish and Release Gradle Modules** Workflow ist ein wiederverwendbarer GitHub Actions Workflow, der in [`SLNE-Development/surf-workflows`](https://github.com/SLNE-Development/surf-workflows) gepflegt wird. Er standardisiert den gesamten Lebenszyklus eines Gradle-basierten Moduls in allen SLNE-Repositories — vom Kompilieren und Veröffentlichen von Maven-Artefakten bis hin zur Erstellung eines versionierten GitHub Releases.

Durch den Aufruf dieses einzelnen Workflows profitiert jedes Repository von einer konsistenten, bewährten CI/CD-Pipeline, ohne dass Workflow-Logik dupliziert werden muss.

***

### Features

* Baut alle Shadow JARs mit `shadowJar`
* Veröffentlicht optional Maven-Artefakte via `publish`
* Validiert optional die Kotlin ABI via `checkKotlinAbi`
* Liest die Projektversion direkt aus den Gradle Properties
* Hängt automatisch `-SNAPSHOT` an, wenn die Gradle Property `snapshot` auf `true` gesetzt ist
* Erstellt einen GitHub Release mit automatisch generierten Release Notes
* Markiert den Release als **Pre-Release** bei Snapshot-Versionen
* Markiert einen Release nur dann als **latest**, wenn er auf dem Default-Branch mit einer Nicht-Snapshot-Version läuft
* Fügt kompilierte JAR-Dateien über konfigurierbare Glob-Muster an den Release an
* Gradle-Setup und Caching werden automatisch übernommen — keine manuelle Konfiguration erforderlich

***

### Verwendung

Der Workflow wird aus einem beliebigen Repository über `uses:` innerhalb einer Job-Definition aufgerufen. `secrets: inherit` sollte immer angegeben werden, damit die benötigten Zugangsdaten automatisch weitergegeben werden.

```yaml
name: Publish

on:
  push:
    branches:
      - version/**
  workflow_dispatch:

jobs:
  build:
    uses: SLNE-Development/surf-workflows/.github/workflows/build-publish-release-gradle.yml@master
    with:
      modules: "surf-redis-paper-*-all.jar;surf-redis-velocity-*-all.jar"
    secrets: inherit
```

{% hint style="info" %}
`secrets: inherit` leitet alle im aufrufenden Repository (oder der Organisation) verfügbaren Secrets automatisch an den wiederverwendbaren Workflow weiter. Dies ist der empfohlene Ansatz — einzelne Secrets müssen nicht explizit aufgelistet werden.
{% endhint %}

***

### Inputs

| Input              | Typ       | Pflicht | Standard | Beschreibung                                                                                                                                                                                                      |
| ------------------ | --------- | ------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `maven-publish`    | `boolean` | Nein    | `true`   | Gibt an, ob nach dem Build alle Maven-Publikationen veröffentlicht werden sollen.                                                                                                                                 |
| `modules`          | `string`  | Nein    | `""`     | Semikolon-getrennte JAR-Dateinamenmuster, die an den GitHub Release angehängt werden (z. B. `surf-redis-paper-*-all.jar;surf-redis-velocity-*-all.jar`). Leer lassen, um einen Release ohne Anhänge zu erstellen. |
| `java-version`     | `string`  | Nein    | `"25"`   | Java-Version für den Gradle-Build (GraalVM Distribution).                                                                                                                                                         |
| `check-kotlin-abi` | `boolean` | Nein    | `false`  | Gibt an, ob der Gradle-Task `checkKotlinAbi` vor der Veröffentlichung ausgeführt werden soll.                                                                                                                     |

***

### Secrets

| Secret                        | Pflicht | Beschreibung                                                                                |
| ----------------------------- | ------- | ------------------------------------------------------------------------------------------- |
| `SLNE_RELEASES_REPO_USERNAME` | Ja      | Benutzername, den Gradle zur Authentifizierung am SLNE Maven Releases Repository verwendet. |
| `SLNE_RELEASES_REPO_PASSWORD` | Ja      | Passwort, das Gradle zur Authentifizierung am SLNE Maven Releases Repository verwendet.     |

Diese Secrets müssen in den **Repository Secrets** oder **Organisations-Secrets** des aufrufenden Repositories hinterlegt sein. Bei Verwendung von `secrets: inherit` (empfohlen) werden sie automatisch weitergeleitet — keine zusätzliche Konfiguration notwendig.

***

### Verhalten / Funktionsweise

Der Workflow führt folgende Schritte der Reihe nach aus:

1. **Checkout** — Der vollständige Verlauf wird ausgecheckt (`fetch-depth: 0`), um eine korrekte Versionierung zu gewährleisten.
2. **Java-Setup** — GraalVM JDK wird mit der konfigurierten `java-version` installiert.
3. **Gradle-Setup** — [`gradle/actions/setup-gradle`](https://github.com/gradle/actions) konfiguriert Gradle inklusive automatischem Build-Caching. Eine manuelle Cache-Konfiguration ist nicht erforderlich.
4. **`shadowJar`** — Alle Shadow JARs werden mit `--parallel --no-scan` gebaut.
5. **`checkKotlinAbi`** _(optional)_ — Wird nur ausgeführt, wenn `check-kotlin-abi` auf `true` gesetzt ist.
6. **`publish`** _(optional)_ — Veröffentlicht Maven-Artefakte im SLNE Releases Repository. Wird nur ausgeführt, wenn `maven-publish` auf `true` gesetzt ist.
7. **Versionsermittlung** — Die Version wird direkt über `./gradlew properties` aus Gradle ausgelesen. Ist die Property `snapshot` auf `true` gesetzt und enthält die Version noch kein `SNAPSHOT`, wird das Suffix `-SNAPSHOT` automatisch angehängt.
8. **Release-Optionen** — Es wird ermittelt, ob der Release als `prerelease` und/oder als `latest` markiert werden soll:
   * `prerelease: true` → Version enthält `SNAPSHOT`
   * `make_latest: true` → Version enthält **kein** `SNAPSHOT` **und** der Workflow läuft auf dem Default-Branch
9. **JAR-Sammlung** _(optional)_ — Ist `modules` nicht leer, durchsucht der Workflow alle `build/libs/`-Verzeichnisse nach Dateien, die den angegebenen Mustern entsprechen. Werden keine Dateien gefunden, schlägt der Build fehl.
10. **GitHub Release** — Ein Release wird mit dem Tag `v<VERSION>` und automatisch generierten Release Notes erstellt. Gefundene JARs werden als Anhänge hinzugefügt.

***

### Beispiele

#### Mit Modulen — JARs an den Release anhängen

Geeignet für Plugins oder Anwendungen, die verteilbare JARs erzeugen.

```yaml
jobs:
  build:
    uses: SLNE-Development/surf-workflows/.github/workflows/build-publish-release-gradle.yml@master
    with:
      modules: "surf-redis-paper-*-all.jar;surf-redis-velocity-*-all.jar"
    secrets: inherit
```

Der `modules`-Input akzeptiert mehrere Muster, getrennt durch `;`. Jedes Muster wird gegen Dateien in allen `build/libs/`-Verzeichnissen des Repositories abgeglichen. Die Muster verwenden Shell-Glob-Syntax (z. B. `*` als Platzhalter) — **keine** regulären Ausdrücke.

#### Ohne Module — Release für Bibliotheken/APIs

Geeignet für Bibliotheken, die als Maven-Abhängigkeiten genutzt werden. Ein GitHub Release wird weiterhin erstellt (mit automatischen Release Notes), es werden jedoch keine JAR-Dateien angehängt.

```yaml
jobs:
  build:
    uses: SLNE-Development/surf-workflows/.github/workflows/build-publish-release-gradle.yml@master
    with:
      maven-publish: true
    secrets: inherit
```

#### Mit Kotlin ABI Prüfung

Geeignet für Projekte, die eine öffentliche API bereitstellen, die sich ohne einen Major-Version-Bump nicht ändern darf.

```yaml
jobs:
  build:
    uses: SLNE-Development/surf-workflows/.github/workflows/build-publish-release-gradle.yml@master
    with:
      check-kotlin-abi: true
      modules: "surf-example-*-all.jar"
    secrets: inherit
```

***

### Hinweise & Best Practices

* **Auf Versions-Branches triggern** — Der Workflow wird typischerweise bei Pushes auf `version/**`-Branches sowie über `workflow_dispatch` ausgelöst.
* **Snapshot-Versionen** — Jede Version, die `SNAPSHOT` enthält (direkt gesetzt oder über die Gradle Property `snapshot` abgeleitet), erzeugt einen Pre-Release auf GitHub und wird niemals als `latest` markiert.
* **`latest`-Release** — Ein Release wird nur dann als `latest` markiert, wenn er auf dem Default-Branch des Repositories läuft **und** die Version kein Snapshot ist. So wird verhindert, dass Entwicklungs-Builds den letzten stabilen Release überschreiben.
* **Modul-Muster sind Globs, keine Regex** — `*` als Platzhalter verwenden (z. B. `surf-redis-paper-*-all.jar`). Regex-Syntax wie `.+` oder `\d+` wird nicht unterstützt.
* **Leeres `modules` ist gültig** — Das Weglassen oder Leer-Lassen von `modules` erstellt einen Release ohne Datei-Anhänge. Dies ist die korrekte Einstellung für API- und Bibliotheks-Module.
* **Kein Cache-Setup erforderlich** — `gradle/actions/setup-gradle` übernimmt das Gradle Build-Caching automatisch. Separate Caching-Schritte sollten nicht hinzugefügt werden.
* **Self-hosted Runner** — Der Workflow läuft auf einem `self-hosted` Runner. Es muss sichergestellt werden, dass der Runner verfügbar ist und Netzwerkzugriff auf das SLNE Maven Repository hat.
