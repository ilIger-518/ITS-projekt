# Projekt To Do (Kurzfassung)

In Zweierteams entwickeln Sie ein vernetztes CPS-Modul auf Basis des **M5Stack AtomS3R**. Jedes Team:

*   richtet einen AtomS3R mit **ESPHome/YAML** ein,
*   liest Sensordaten des **ENV-III Moduls** über den I²C-Bus aus,
*   bindet einen **externen Taster** über die Bottom-Pins ein,
*   sendet alle Daten authentifiziert und optional verschlüsselt an einen MQTT-Broker,
*   abonniert die Daten eines Partnerteams und
*   steuert abhängig von deren Messwerten ein eigenes Aktor-Element (Displayfarbe oder externe RGB-LED).

---

# 1. Hardwareaufbau

Jedes Team erhält:

*   1× AtomS3R
*   1× ENV-III Sensor (I²C, Port A)
*   1× externer Taster (Unit Key oder Seeed Button v1.3)
*   Jumper-Kabel

## Aufgabe:
*   Schließen Sie den **ENV-III Sensor** über den Grove-Port (GPIO1/2) an.
*   Verbinden Sie den **Taster** über einen freien Bottom-Pin (empfohlen: GPIO5 oder GPIO6) und GND.
*   Dokumentieren Sie Ihren Aufbau mit einem Foto.

---

# 2. ESPHome-Konfiguration (YAML)

Erstellen Sie eine vollständige YAML-Konfiguration, die folgende Anforderungen erfüllt:

## 2.1 Grundstruktur
*   **esphome:** korrekt für AtomS3R (`esp32:`)
*   **wifi:** mit Zugangsdaten
*   **mqtt:** mit
    *   Benutzername/Passwort
    *   TLS-Verschlüsselung (falls möglich)
    *   eindeutigem `topic_prefix` (z. B. `team05/atoms3r`)
*   **logger:**
*   **ota:**

## 2.2 Sensorik
*   ENV-III Sensor über I²C einbinden
    *   Temperatur
    *   Luftfeuchtigkeit
    *   Luftdruck
*   Externer Taster als `binary_sensor`
*   Alle Werte regelmäßig publizieren (z. B. alle 20 s)

---

# 3. MQTT-Kommunikation

## 3.1 Publishing
Senden Sie folgende Daten an Ihren MQTT-Broker:

| Komponente | Topic | Beispielwert |
| :--- | :--- | :--- |
| Temperatur | `BSZ/E2FI3/cont01/env/temp` | 22.4 |
| Luftfeuchte | `BSZ/E2FI3/cont01/env/humidity` | 45.1 |
| Luftdruck | `BSZ/E2FI3/cont01/env/pressure` | 988.2 |
| Taster | `BSZ/E2FI3/cont01/button` | pressed / released |

## 3.2 Subscription
*   Abonnieren Sie die Temperatur eines Partnerteams, z. B.: `team07/env/temp`
*   Speichern Sie den Wert in einer globalen Variable oder nutzen Sie direkt eine `on_message`-Automation.

---

# 4. Aktorik: Reaktion auf Partnerdaten

Ihr AtomS3R soll abhängig von der Temperatur des Partnerteams reagieren:

| Temperatur Partner | Aktion |
| :--- | :--- |
| **< 25°C** | Display/LED grün |
| **25 – 28°C** | Display/LED gelb |
| **> 28°C** | Display/LED rot |

### Optionen für die Umsetzung:
*   Display-Hintergrundfarbe ändern
*   externe RGB-LED (z. B. Unit Key) ansteuern
*   interne Status-LED (GPIO35) nutzen

> **Hinweis:** Die Umsetzung muss als **Aktor** konfiguriert sein, nicht als Sensor.

---

# 5. Reflexionsfragen

Beantworten Sie die Fragen (siehe Projektauftrag) schriftlich in Ihrer Dokumentation.
