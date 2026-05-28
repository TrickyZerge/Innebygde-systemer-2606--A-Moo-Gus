# A-Moog-Us 🎹

**A-Moog-Us** er et egenutviklet, modulært kretskort og en funksjonell sanntids-synthesizer. Prosjektet er utviklet som en tverrfaglig læringsplattform i faget *AUT-2606 Innebygde Systemer* ved UiT Norges arktiske universitet.

Prosjektet demonstrerer verdien av en distribuert arkitektur ved å forene tre fundamentale teknologier: et høynivå operativsystem (Raspberry Pi), en mikrokontroller for digital signalbehandling (ESP32), og maskinvareakselerasjon (FPGA). Ved å utnytte hver arkitekturs unike styrker, fungerer systemet som et kreativt bevis på kompleks systemintegrasjon og maskinvarenær programmering.

## ⚙️ Systemarkitektur

Systemet bygger på en Master/Slave-arkitektur med følgende hovedkomponenter:

* **Raspberry Pi Zero (Master):** Kjører et multitrådet Python-program. Hovedtråden driver et grafisk brukergrensesnitt (GUI) i Tkinter, mens en bakgrunnstråd lytter til maskinvare-UART (PL011) for å tolke eksterne MIDI-signaler ved 31 250 baud. Data overføres videre i standardiserte pakker via SPI.
* **ESP32 (Slave / DSP):** Fungerer som systemets lydmotor. Koden utnytter mikrokontrollerens *dual-core* arkitektur: Kjerne 0 lytter etter asynkrone SPI-meldinger, mens Kjerne 1 kjører kontinuerlig generering av digitale bølgeformer.
* **Maskinvare (Custom PCB):** Et spesialdesignet kretskort som huser prosessorene, strømforsyning, en MIDI-inngang (galvanisk skilt med 6N138 optokobler), en PCM5100 DAC (drevet via I2S), og en integrert Klasse-D forsterker.

## 📂 Innhold i repository
*(Tips: Endre mappenavnene under slik at de stemmer med strukturen din)*

* `/RPi_GUI` - Python-koden for det grafiske brukergrensesnittet og MIDI-håndtering.
* `/ESP32_DSP` - C++ / Arduino-koden for I2S-lydgenerering og SPI-mottak.
* `/Hardware` - Altium Designer-filer (schematics, PCB-layout, Gerber-filer).
* `/Docs` - Prosjektrapport og referansedokumenter.

## 🚀 Kom i gang (Setup)

For å kjøre prosjektet kreves det spesifikke oppsett for både Raspberry Pi og ESP32.

### Raspberry Pi (Python)
1. Aktiver SPI-grensesnittet i `raspi-config`.
2. For å muliggjøre presis MIDI-avlesing, må operativsystemets innebygde Bluetooth deaktiveres for å frigjøre maskinvare-UARTen (PL011) til `/dev/serial0`.
3. Installer nødvendige biblioteker:
   ```bash
   pip install pyserial spidev
