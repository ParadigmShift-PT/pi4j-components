# Pi4J Components

A catalogue of reusable Pi4J component wrappers for Raspberry Pi peripherals — LEDs, buttons, joysticks (digital and analogue), servos, ADCs (Ads1115), potentiometers, LCD displays, LED matrices and strips, buzzers, cameras, and serial GPS — built on top of [Pi4J v2](https://pi4j.com).

**Group ID:** `pt.paradigmshift.iot`
**Artifact ID:** `pi4j-components`
**Current version:** `0.0.7`
**License:** [Apache 2.0](LICENSE) — see also [NOTICE](NOTICE) for the provenance chain.

---

## Origin

This artifact is a **vendored copy** of third-party Apache-2.0-licensed code originally extracted from the [Pi4J project's](https://pi4j.com) example applications. ParadigmShift hosts the artifact on its Maven repository so the StoneFlux ecosystem does not depend on third-party Maven hosts that may become unavailable.

Provenance chain:

| Stage | Source | Coordinates |
|---|---|---|
| Pi4J framework | [pi4j.com](https://pi4j.com) — [Pi4J GitHub org](https://github.com/Pi4J) | `com.pi4j:pi4j-core` (and family) |
| Component catalogue | Pi4J example applications | `com.pi4j:pi4j-components:0.0.7` |
| ParadigmShift repackage | [paradigmshift.pt](https://www.paradigmshift.pt) | `pt.paradigmshift.iot:pi4j-components:0.0.7` |

The Java packages remain `com.pi4j.catalog.*` so existing consumers can switch by changing only their Maven coordinates — no source-level changes required. The Apache 2.0 licence is preserved in [LICENSE](LICENSE) verbatim; ParadigmShift makes no additional licensing claim.

---

## Usage

Add to your `pom.xml`:

```xml
<repositories>
    <repository>
        <id>paradigmshift-repository</id>
        <name>ParadigmShift Repository</name>
        <url>https://maven.paradigmshift.pt/releases</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>pt.paradigmshift.iot</groupId>
        <artifactId>pi4j-components</artifactId>
        <version>0.0.7</version>
    </dependency>
</dependencies>
```

`pi4j-components` brings `pi4j-core` and the standard set of Pi4J runtime plugins (`raspberrypi`, `linuxfs`, `gpiod`, `pigpio`, `mock`) in transitively, so this single dependency is sufficient to drive supported peripherals on a Raspberry Pi.

```java
Context pi4j = Pi4J.newAutoContext();

SimpleLed led = new SimpleLed(pi4j, PIN.D17);
led.on();

SimpleButton button = new SimpleButton(pi4j, PIN.D26, true);
button.onDown(() -> System.out.println("pressed"));
```

The full component catalogue lives under `com.pi4j.catalog.components` (concrete devices) and `com.pi4j.catalog.components.base` (`Component`, `DigitalActuator`, `DigitalSensor`, `I2CDevice`, `PwmActuator`, `SerialDevice`, `SpiDevice`, `PIN`).

This artifact must run on hardware that exposes Linux GPIO / I²C / SPI interfaces — typically a Raspberry Pi. The `pi4j-plugin-mock` dependency is included so that consumers can write unit tests against the component API without real hardware.

---

## Building

Requires Java 17 and Maven 3.6+.

```bash
mvn verify    # compile + (no tests yet)
mvn package   # produces JAR, sources JAR, and Javadoc JAR
mvn deploy    # publish to maven.paradigmshift.pt (requires REPOSILITE_TOKEN)
```

## Releasing

Push a version tag — the GitHub Actions CI workflow builds and deploys automatically:

```bash
git tag v0.0.7
git push origin v0.0.7
```

---

## License

Apache 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE). Original work © the Pi4J project; repackaged for the ParadigmShift ecosystem.
