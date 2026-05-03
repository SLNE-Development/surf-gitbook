# Component System

### Übersicht

Das Component System ist ein modulares System zur Organisation und Verwaltung von Plugin-Funktionalitäten. Es ermöglicht die automatische Erkennung, bedingte Aktivierung und Lebenszyklus-Verwaltung von Komponenten zur Compile-Zeit und Runtime.

{% hint style="info" %}
Komponenten werden zur Compile-Zeit automatisch erkannt und zur Runtime basierend auf ihren Abhängigkeiten und Bedingungen instanziiert. Du musst Komponenten nicht manuell registrieren!
{% endhint %}

### Grundkonzepte

#### Was ist eine Component?

Eine Component ist eine modulare Funktionseinheit, die einem definierten Lebenszyklus folgt. Komponenten können:

* Automatisch zur Compile-Zeit entdeckt werden
* Bedingt basierend auf Konfiguration, Umgebung oder anderen Faktoren geladen werden
* Abhängigkeiten zu anderen Komponenten oder Plugins deklarieren
* In einer bestimmten Reihenfolge initialisiert werden

#### Lebenszyklus

Jede Component durchläuft drei Phasen:

1. **Load** - Initiale Ladephase für Konfiguration und Ressourcen
2. **Enable** - Aktivierungsphase für die eigentliche Funktionalität
3. **Disable** - Deaktivierungsphase beim Shutdown für Aufräumarbeiten

{% hint style="warning" %}
Components werden in umgekehrter Reihenfolge ihrer Initialisierung deaktiviert. Das stellt sicher, dass Abhängigkeiten korrekt aufgelöst werden.
{% endhint %}

### Component erstellen

#### Verwendung von AbstractComponent (Empfohlen)

Die einfachste Methode ist, von `AbstractComponent` zu erben:

```kotlin
import dev.slne.surf.api.core.component.AbstractComponent
import dev.slne.surf.api.shared.api.component.SurfComponentMeta

@SurfComponentMeta
class MyComponent : AbstractComponent() {
    
    override suspend fun onLoad() {
        // Konfiguration laden, Ressourcen initialisieren
        println("MyComponent wird geladen")
    }
    
    override suspend fun onEnable() {
        // Funktionalität aktivieren
        println("MyComponent ist aktiv")
    }
    
    override suspend fun onDisable() {
        // Aufräumen
        println("MyComponent wird deaktiviert")
    }
}
```

**Vorteile von AbstractComponent:**

* Automatische Phasen-Verkettung (`enable()` ruft automatisch `load()` auf)
* Lifecycle-Management ist bereits implementiert
* Jede Phase wird nur einmal ausgeführt

#### Direktes Implementieren von Component

Für mehr Kontrolle kannst du auch direkt das `Component` Interface implementieren:

```kotlin
import dev.slne.surf.api.shared.api.component.SurfComponent
import dev.slne.surf.api.shared.api.component.SurfComponentMeta

@SurfComponentMeta
class MyBasicComponent : SurfComponent {
    
    override suspend fun load() {
        // Manuelle Lifecycle-Verwaltung erforderlich
        println("Load-Phase")
    }
    
    override suspend fun enable() {
        // Keine automatische Verkettung!
        println("Enable-Phase")
    }
    
    override suspend fun disable() {
        println("Disable-Phase")
    }
}
```

{% hint style="danger" %}
Bei direkter Implementierung von `SurfComponent` (ohne `AbstractComponent`) erfolgt **KEINE** automatische Phasen-Verkettung. Du musst die Phasen manuell in der richtigen Reihenfolge aufrufen, sofern du nicht sicher stellst du in deinem Plugin nicht alle Phasen aufgerufen werden (siehe [#plugin-integration](component-system.md#plugin-integration "mention")).
{% endhint %}

### Plugin-Integration <a href="#plugin-integration" id="plugin-integration"></a>

#### SurfComponentApi verwenden

In deinem Plugin rufst du die entsprechenden Lifecycle-Methoden auf:

```kotlin
import com.github.shynixn.mccoroutine.folia.SuspendingJavaPlugin
import dev.slne.surf.api.core.component.surfComponentApi

class MyPlugin : SuspendingJavaPlugin() {
    
    override suspend fun onLoadAsync() {
        // Alle Components dieses Plugins laden
        surfComponentApi.load(this)
    }
    
    override suspend fun onEnableAsync() {
        // Alle Components dieses Plugins aktivieren
        surfComponentApi.enable(this)
    }
    
    override suspend fun onDisableAsync() {
        // Alle Components dieses Plugins deaktivieren
        surfComponentApi.disable(this)
    }
}

```

#### Components abfragen

Du kannst geladene Components auch zur Runtime abfragen:

```kotlin
// Alle Components eines bestimmten Typs erhalten
val services: List<MyService> = surfComponentApi.componentsOfTypeLoaded(this, MyService::class.java)

// Alle Components des Plugins erhalten
val allComponents = surfComponentApi.componentsLoaded(this)

// Lazy-Loading: Components werden bei Bedarf geladen (es wird kein Lifecycle aufgerufen, nur die Instanzen werden initialisiert)
val lazyServices = surfComponentApi.componentsOfType(this, MyService::class.java)

```

### Annotations & Metadata <a href="#annotations--metadata" id="annotations--metadata"></a>

#### `@SurfComponentMeta`

Die `@SurfComponentMeta` Annotation markiert eine Klasse als Component und ermöglicht die automatische Erkennung:

```kotlin
@SurfComponentMeta
class MyComponent : AbstractComponent() {
    // ...
}

```

{% hint style="info" %}
`@SurfComponentMeta` kann auch als Meta-Annotation verwendet werden, um eigene Component-Typen zu definieren!
{% endhint %}

#### `@Priority`&#x20;

Steuert die Initialisierungsreihenfolge. Niedrigere Werte werden zuerst initialisiert:

```kotlin
@ComponentMeta
@Priority(10)  // Wird früh initialisiert
class EarlyComponent : AbstractComponent() {
    // ...
}

@ComponentMeta
@Priority(100) // Wird später initialisiert
class LateComponent : AbstractComponent() {
    // ...
}

```

**Standard-Priorität:** `0` (wenn nicht angegeben)

### Component-Typen

Es gibt vordefinierte Meta-Annotations für verschiedene Component-Typen:

```kotlin
import dev.slne.surf.api.shared.api.component.types.Service

@Service  // Impliziert @SurfComponentMeta
class MyService : AbstractComponent() {
    // ...
}

```

Du kannst auch eigene Component-Typen erstellen:

```kotlin
@Service
annotation class DatabaseService

@DatabaseService  // Impliziert @Service und damit @SurfComponentMeta
class MyDatabaseService : AbstractComponent() {
    // ...
}

```

### Bedingte Aktivierung <a href="#bedingte-aktivierung" id="bedingte-aktivierung"></a>

#### `@ConditionalOnProperty`

Aktiviert die Component nur, wenn eine bestimmte Property gesetzt ist:

```kotlin
@SurfComponentMeta
@ConditionalOnProperty(key = ["feature", "enabled"], havingValue = "true")
class OptionalFeature : AbstractComponent() {
    // Wird nur geladen, wenn in properties.yml (im plugin data path):
    // feature:
    //   enabled: true
}

```

#### `@ConditionalOnEnvironment`

Die Basis-Annotation zur Steuerung der Component-Aktivierung basierend auf der Laufzeitumgebung:

```kotlin
@SurfComponentMeta
@ConditionalOnEnvironment(environments = ["development", "test"])
class DebugComponent : AbstractComponent() {
    // Wird nur in development und test Umgebungen geladen
}
```

**Umgebung konfigurieren:**

Die aktuelle Umgebung wird über die Environment-Variable (`.env`   Datei) `SURF_ENVIRONMENT` gesetzt:
