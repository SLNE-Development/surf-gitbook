---
icon: copy
---

# Config Api

Die `SurfConfigApi` ist die zentrale Schnittstelle zur Verwaltung von Konfigurationsdateien innerhalb der Surf API.\
Sie unterstützt zwei Systeme:

* **Sponge Configs** (YAML & JSON)
* **DazzlConf Configs**

***

Unterstützte Formate:

* `.yml` / `.yaml`
* `.json`

***

## DazzlConf-Konfigurationen

### Konfiguration erstellen

```kotlin
fun <C> createDazzlConfig(
    configClass: Class<C>,
    configFolder: Path,
    configFileName: String
): C
```

| Parameter        | Beschreibung                      |
| ---------------- | --------------------------------- |
| `configClass`    | Ziel-Config-Klasse                |
| `configFolder`   | Pfad zum Config-Verzeichnis       |
| `configFileName` | YAML-Dateiname (`*.yml`/`*.yaml`) |

***

### Konfiguration abrufen

```kotlin
fun <C> getDazzlConfig(configClass: Class<C>): C
```

### Konfiguration neuladen

```kotlin
fun <C> reloadDazzlConfig(configClass: Class<C>): C
```

***

## Sponge-Konfigurationen

### YAML erstellen

```kotlin
fun <C> createSpongeYmlConfig(
    configClass: Class<C>,
    configFolder: Path,
    configFileName: String
): C
```

### JSON erstellen

```kotlin
fun <C> createSpongeJsonConfig(
    configClass: Class<C>,
    configFolder: Path,
    configFileName: String
): C
```

### Abrufen

```kotlin
fun <C> getSpongeConfig(configClass: Class<C>): C
```

### Neuladen

```kotlin
fun <C> reloadSpongeConfig(configClass: Class<C>): C
```

***

### SpongeConfigManager abrufen

```kotlin
fun <C> getSpongeConfigManagerForConfig(configClass: Class<C>): SpongeConfigManager<C>
```

***

## Reified Extensions

### DazzlConf

```kotlin
surfConfigApi.createDazzlConfig<MyConfig>(folder, "config.yml")
surfConfigApi.getDazzlConfig<MyConfig>()
surfConfigApi.reloadDazzlConfig<MyConfig>()
```

### Sponge (YML & JSON)

```kotlin
surfConfigApi.createSpongeYmlConfig<MyConfig>(folder, "config.yml")
surfConfigApi.createSpongeJsonConfig<MyConfig>(folder, "config.json")
surfConfigApi.getSpongeConfig<MyConfig>()
```

***

## Beispiel

```kotlin
data class MyConfig(val enabled: Boolean = true)

val api = surfConfigApi

val config = api.createSpongeYmlConfig<MyConfig>(
    configFolder = Path.of("configs"),
    configFileName = "myconfig.yml"
)

println(config.enabled)
```
