---
icon: bolt
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
metaLinks:
  alternates:
    - https://app.gitbook.com/s/yE16Xb3IemPxJWydtPOj/getting-started/quickstart
---

# Installation

Um surf-api nutzen zu können, brauchst du zunächst ein Multi-Module Project oder ein Single Module Project mit Gradle als Build-Tool.
Füge in der `gradle.properties` Datei deines Projektes diese Zeilen hinzu. Wenn die Datei noch nicht existiert, erstelle sie im Root Verzeichnis deines Projektes.

```properties
kotlin.code.style=official
kotlin.stdlib.default.dependency=false
org.gradle.parallel=true
```

## Einzelnes Modul
Wenn du in einem Single-Module-Project surf-api nutzen möchtest, sollte deine Ordnerstruktur vorerst so aussehen:

```
└── my-project/
    ├── src/main
    ├── .gitignore
    ├── gradle.properties
    ├── build.gradle.kts
    └── settings.gradle.kts
  ```

Füge anschließend in deiner `settings.gradle.kts` Datei die Abhängigkeit für surf-api hinzu:

```kotlin
buildscript {
    repositories {
        gradlePluginPortal()
        maven("https://repo.slne.dev/repository/maven-public/") { name = "maven-public" }
    }
    dependencies {
        classpath("dev.slne.surf:surf-api-gradle-plugin:<VERSION>")
    }
}
```

Die neuste surf-api Version findest du hier [Maven Central](https://repo.slne.dev/#browse/browse:maven-public:dev%2Fslne%2Fsurf%2Fsurf-api-gradle-plugin). Um die neuste Version des Major Updates zu nutzen, nutze `<MC-VERSION>+` (z.b. 1.21.10+).

{% hint style="warning" %}
Bitte beachte, dass nur die neuste surf-api Version unterstützt wird.
{% endhint %}

## Mehrere Module

Wenn du in einem Multi-Module-Project surf-api nutzen möchtest, sollte deine Ordner Struktur vorerst so aussehen:

```
└── root-folder/
    ├── api/
    │   ├── src/main
    │   └── build.gradle.kts
    ├── core/
    │   ├── src/main
    │   └── build.gradle.kts
    ├── paper-plugin/
    │   ├── src/main
    │   └── build.gradle.kts
    ├── velocity-plugin/
    │   ├── src/main
    │   └── build.gradle.kts
    ├── .gitignore
    ├── gradle.properties
    ├── build.gradle.kts
    └── settings.gradle.kts
```

Füge anschließend in der `build.gradle.kts` Datei im Hauptverzeichnis deines Projektes die Abhängigkeit für surf-api hinzu:

```kotlin
buildscript {
    repositories {
        gradlePluginPortal()
        maven("https://repo.slne.dev/repository/maven-public/") { name = "maven-public" }
    }
    dependencies {
        classpath("dev.slne.surf:surf-api-gradle-plugin:<VERSION>")
    }
}
```

Die neuste surf-api Version findest du hier [Maven Central](https://repo.slne.dev/#browse/browse:maven-public:dev%2Fslne%2Fsurf%2Fsurf-api-gradle-plugin). Um die neuste Version des Major Updates zu nutzen, nutze `<MC-VERSION>+` (z.b. 1.21.10+).

{% hint style="warning" %}
Bitte beachte, dass nur die neuste surf-api Version unterstützt wird.
{% endhint %}

## Modul einrichten

Füge anschließend in deiner `build.gradle.kts` Datei des entsprechenden Moduls das surf-api Plugin hinzu, welches zu deinem Projekt passt.
Hier findest du ein Beispiel für ein Paper Projekt:

```kotlin
plugins {
    id("dev.slne.surf.surfapi.gradle.paper-plugin")
}

surfPaperPluginApi {
    mainClass("dev.slne.surf.example.PaperMain")
    authors.addAll("red", "example")
    foliaSupported(true) // Nur wenn Folia unterstützt wird

    generateLibraryLoader(false)
}
```

Verfügbare Plattformen:

```kotlin
plugins {
    id("dev.slne.surf.surfapi.gradle.core") // for core modules (applies kotlin and other plugins, adds surf-core-api compileOnly dependency)
}

plugins {
  id("dev.slne.surf.surfapi.gradle.paper-plugin") // for paper plugin modules (all core features, adds paper-api dependency, generation of paper-plugin.yml and runServer configuration)
}

plugins {
  id("dev.slne.surf.surfapi.gradle.paper-raw") // for paper plugin modules (all core features, adds paper-api dependency, NO generation of paper-plugin.yml nor runServer configuration)
}

plugins {
  id("dev.slne.surf.surfapi.gradle.standalone") //  for standalone modules (all core features, adds surf-core-api dependency, shades surf-api-standalone)
}

plugins {
  id("dev.slne.surf.surfapi.gradle.velocity") // for velocity modules (all core features, adds velocity-api dependency)
}

```
