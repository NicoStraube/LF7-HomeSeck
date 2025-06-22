# LF7-HomeSeck
Lernfeld 7 - Cyber-physische Systeme ergänzen - Projekt "HomeSeck"

--

# Technische Projektdokumentation: HomeSeck-Sicherheitssystem

## 1. Einleitung

Das HomeSeck-Sicherheitssystem ist darauf ausgelegt, eine robuste und flexible Lösung für die Haussicherheit zu bieten. Der Name „HomeSeck“ kombiniert „Home“ (Zuhause), „Security“ (Sicherheit) und Seck als Referenz zu (Home)-Sick und spielt auf die Sorge um den Schutz des Zuhauses an, wenn man abwesend ist. Durch die Integration von Hardware- und Softwarekomponenten gewährleistet HomeSeck, dass Ihr Zuhause sicher bleibt. Das System basiert auf einem Raspberry Pi 5 mit Home Assistant OS, der als zentrale Steuereinheit für die Verwaltung von Sicherheitsgeräten wie RFID-Scannern und Bewegungsmeldern dient.

## 2. Systemarchitektur

Das HomeSeck-System besteht aus folgenden Hauptkomponenten:

- **Raspberry Pi 5**: Die zentrale Steuereinheit, ausgestattet mit aktiver Kühlung und einer 32 GB SD-Karte, auf der Home Assistant OS läuft.
- **ESP32-Module**: Mikrocontroller, die spezifische Sicherheitsaufgaben wie das Lesen von RFID-Tags und das Erkennen von Bewegungen übernehmen.
- **RFID-Scanner (RC522)**: Ermöglicht physische Authentifizierung durch Scannen von RFID-Tags.
- **PIR-Bewegungsmelder (FOLLOWING)**: Erkennt Bewegungen im überwachten Bereich.
- **Summer**: Bietet akustisches Feedback bei RFID-Scans und Alarmsignalen.

Alle Komponenten kommunizieren über ein Wi-Fi-Netzwerk, was eine flexible Platzierung und einfache Integration ermöglicht. Die ESP32-Module und der Raspberry Pi sind mit demselben WLAN verbunden, das entweder über einen Telefon-Hotspot oder ein dediziertes Sub-WLAN bereitgestellt wird.

## 3. Hardware-Setup

### 3.1 Raspberry Pi 5

- **Modell**: Raspberry Pi 5
- **Kühlung**: Aktive Kühlung mit Lüfter
- **Speicher**: 32 GB SD-Karte mit Home Assistant OS
- **Netzwerk**: Konfiguriert für die Verbindung mit einem Wi-Fi-Netzwerk

### 3.2 ESP32-Module

- **Typ**: ESP32
- **Funktionalität**: Jeder ESP32 steuert spezifische Sensoren (RFID oder PIR)
- **Verbindung**: Mit demselben Wi-Fi-Netzwerk wie der Raspberry Pi verbunden

### 3.3 RFID-Scanner

- **Modell**: RC522
- **Verbindung**: Über SPI mit einem ESP32 verbunden

### 3.4 PIR-Bewegungsmelder

- **Modell**: FOLLOWING
- **Verbindung**: Über GPIO mit einem ESP32 verbunden

### 3.5 Summer

- **Verbindung**: An einen GPIO-Pin des Raspberry Pi angeschlossen (z. B. GPIO17)

| Komponente          | Modell/Typ       | Verbindung                     | Funktion                              |
|---------------------|------------------|--------------------------------|---------------------------------------|
| Raspberry Pi 5      | Raspberry Pi 5   | Wi-Fi, GPIO (Summer)           | Zentrale Steuereinheit                |
| ESP32-Modul (RFID)  | ESP32            | Wi-Fi, SPI (RC522)             | RFID-Tag-Lesen                        |
| ESP32-Modul (PIR)   | ESP32            | Wi-Fi, GPIO (FOLLOWING)        | Bewegungserkennung                    |
| RFID-Scanner        | RC522            | SPI zu ESP32                   | Authentifizierung                     |
| PIR-Bewegungsmelder | FOLLOWING        | GPIO zu ESP32                  | Bewegungserkennung                    |
| Summer              | -                | GPIO17 am Raspberry Pi         | Akustisches Feedback und Alarm        |

## 4. Software-Konfiguration

### 4.1 Installation von Home Assistant OS auf Raspberry Pi 5

1. Laden Sie das [Raspberry Pi Imager](https://www.raspberrypi.com/software/) herunter und installieren Sie es auf Ihrem Computer.
2. Öffnen Sie das Raspberry Pi Imager und wählen Sie den Raspberry Pi 5 aus.
3. Wählen Sie das Betriebssystem: Other specific-purpose OS > Home assistants and home automation > Home Assistant > Version für Raspberry Pi 5.
4. Wählen Sie die SD-Karte aus und schreiben Sie das Image.
5. Stecken Sie die SD-Karte in den Raspberry Pi 5, schließen Sie ihn an die Stromversorgung an und starten Sie ihn.

### 4.2 Installation des ESPHome-Add-ons in Home Assistant

1. Gehen Sie in Home Assistant zum Add-on Store und suchen Sie nach „ESPHome“.
2. Installieren Sie das ESPHome-Add-on.
3. Starten Sie das Add-on und öffnen Sie die Web-Benutzeroberfläche.

### 4.3 Konfiguration der ESP32-Module mit ESPHome

Für jedes ESP32-Modul erstellen Sie eine neue Konfiguration in ESPHome:

#### Für den RFID-Scanner:

```yaml
FOLLOWING
```

#### Für den PIR-Bewegungsmelder:

```yaml
FOLLOWING
```

### 4.4 Konfiguration des Raspberry Pi GPIO für den Summer

Installieren Sie die „Raspberry Pi GPIO“-Integration über HACS oder manuell ([Raspberry Pi GPIO Integration](https://github.com/thecode/ha-rpi_gpio)) und fügen Sie die folgende Konfiguration zu Ihrer Home Assistant-Konfigurationsdatei hinzu:

```yaml
FOLLOWING
```

## 5. Integration und Funktionalität

### 5.1 ESPHome-Integration

Nach der Konfiguration und dem Flashen der ESP32-Module erscheinen diese in Home Assistant unter der ESPHome-Integration. Entitäten wie der Bewegungsmelder und die zuletzt gescannte UID werden automatisch erkannt.

### 5.2 Automatisierungen

Richten Sie die folgenden Automatisierungen in Home Assistant ein:

#### Scharf-/Unscharfschalten:

Erstellen Sie einen input_boolean für den Scharfzustand:

```yaml
FOLLOWING
```

#### Alarm bei Bewegungserkennung auslösen:

```yaml
FOLLOWING
```

#### Unscharfschalten mit autorisiertem RFID-Tag:

```yaml
FOLLOWING
```

#### Feedback bei RFID-Scan:

```yaml
FOLLOWING
```

## 6. Benutzeroberfläche

Erstellen Sie Dashboards in Home Assistant, um das Sicherheitssystem zu überwachen und zu steuern.

### 6.1 Haupt-Dashboard

- Entitäten-Karte mit:
  - input_boolean.alarm_armed
  - binary_sensor.pir_bewegungsmelder
  - text_sensor.zuletzt_gescannte_uid

### 6.2 Sicherheits-Dashboard

- Schalter-Karte für input_boolean.alarm_armed zum Scharf-/Unscharfschalten
- Entitäten-Karte für den Status des Bewegungsmelders

## 7. Bedienung und Nutzung

- **System scharfschalten**: Schalten Sie den alarm_armed-Schalter im Sicherheits-Dashboard ein.
- **System unscharfschalten**: Scannen Sie ein autorisiertes RFID-Tag, um das System unscharf zu schalten.
- **Bewegungserkennung**: Wenn das System scharfgeschaltet ist, löst jede vom PIR-Sensor erkannte Bewegung den Alarm aus, indem der Summer ertönt.
- **RFID-Feedback**: Bei jedem Scan eines RFID-Tags ertönt ein kurzer Piepton als Feedback.

## 8. Fazit

Das HomeSeck-Sicherheitssystem bietet eine umfassende Lösung für die Haussicherheit durch die Nutzung von Raspberry Pi und ESP32-Modulen. Dank seines modularen Designs kann das System leicht erweitert und an spezifische Sicherheitsanforderungen angepasst werden. Mögliche zukünftige Erweiterungen könnten zusätzliche Sensoren oder eine dynamische Verwaltung autorisierter RFID-UIDs umfassen.
