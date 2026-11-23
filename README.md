# Mikrocontroller- & Systemprogrammierung (FH Aachen)

Praktikumsprojekte aus dem Modul **Architektur von Rechnersystemen und Betriebssystemkonzepte** an der FH Aachen.
Hardwarenahe Programmierung eines **ATmega328P** (Arduino Nano) in **C** direkt über Register – ohne Arduino-Bibliotheken – sowie Grundlagen der Nebenläufigkeit mit **Threads, Mutex und Semaphoren** in C++.

## Teil 1: ATmega328P – Hardwarenahe Programmierung in C

**Hardware:** Arduino Nano (ATmega328P, 16 MHz), 10er-LED-Bar, 2-stellige 7-Segment-Anzeige, Taster, Potentiometer
**Toolchain:** PlatformIO, avr-gcc

### Aufgabe 2 – Lauflicht
- Ein Licht läuft über die 10er-LED-Bar hin und her
- Mit jedem Richtungswechsel wird die Geschwindigkeit erhöht
- Start per Tastendruck (Taster mit internem Pull-up-Widerstand)

### Aufgabe 3 – Reaktionsspiel
- Der Spieler muss den Taster genau dann drücken, wenn das Licht an einem der äußeren LEDs ist
- Bei Erfolg: Richtungswechsel, Level +1, das Spiel wird schneller
- **Game Over**, wenn zu früh gedrückt wird oder das Licht über den Rand läuft – mit eigener Animation je nach Fehlerart
- Aktuelles Level wird live auf der **7-Segment-Anzeige** angezeigt, der erreichte Score am Ende als Balken auf der LED-Bar

### Aufgabe 4 – Spannungsmessung mit dem ADC
- Ein Potentiometer liefert eine Spannung zwischen 0 und 5 V
- Der **Analog-Digital-Wandler (ADC)** wandelt sie in einen Wert von 0–1023 (10 Bit, Free-Running-Mode, 125 kHz)
- Anzeige der Spannung in Volt auf der 7-Segment-Anzeige und als Füllstand auf der LED-Bar

### Technische Highlights
| Thema | Umsetzung |
|---|---|
| Registerprogrammierung | Direkter Zugriff auf `DDRx`, `PORTx`, `PINx` über Bitmasken |
| Timer-Interrupt | Timer0-Overflow-Interrupt (~244 Hz) für das Multiplexing der 7-Segment-Anzeige |
| ADC | Konfiguration von Referenzspannung, Kanal und Prescaler über `ADMUX`/`ADCSRA` |
| Taster | Eingang mit internem Pull-up, Warten auf Drücken/Loslassen mit Timeout |
| Modularer Aufbau | Eigene Treiber-Module für LED-Bar, 7-Segment, Taster und ADC |

### Projektstruktur
```
arbk/a1/src/
├── main.c          # Auswahl der Aufgabe (TASK 2–4)
├── task2.c         # Lauflicht
├── task3.c         # Reaktionsspiel
├── task4.c         # ADC-Spannungsmessung
├── ledBar.c/.h     # Treiber LED-Bar
├── sevenSeg.c/.h   # Treiber 7-Segment-Anzeige (Timer-Interrupt)
├── button.c/.h     # Treiber Taster
└── adc.c/.h        # Treiber ADC
```

## Gelernte Konzepte
- Hardwarenahe Programmierung ohne Abstraktionsschicht
- Interrupts und Timer
- Analog-Digital-Wandlung
- Thread-Zustände (ready, running, blocked) und Scheduling
- Synchronisation mit Mutex und Semaphoren
