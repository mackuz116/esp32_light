======================================================================
DOKUMENTACJA STEROWNIKA OŚWIETLENIA AKWARIUM 300L - "ESP-LIGHT"
======================================================================
Data utworzenia: 2026-03-10
Platforma: ESP32 DevKit V1 (Framework: Arduino/ESPHome)
Lokalizacja projektu: ~/Documents/esp32_lights

----------------------------------------------------------------------
1. KONFIGURACJA PINÓW (GPIO) - WYJŚCIA PWM (CZĘSTOTLIWOŚĆ 1000Hz)
----------------------------------------------------------------------
PIN GPIO  | NAZWA W SYSTEMIE | PRZEZNACZENIE / OPIS
----------|------------------|---------------------------------------
GPIO 12   | Lampa 1          | Sekcja Biała 1 (UWAGA: Strapping Pin*)
GPIO 13   | Lampa 2          | Sekcja Biała 2
GPIO 14   | Lampa 3          | Sekcja Biała 3
GPIO 26   | Lampa 4          | Sekcja Biała 4
GPIO 25   | Lampa 5          | Sekcja Biała 5
GPIO 33   | Lampa 6          | Sekcja Biała 6
GPIO 32   | Lampa 7          | Panel Tło (Niezależny harmonogram)
GPIO 16   | out_red          | Kanał RGB - Czerwony
GPIO 17   | out_green        | Kanał RGB - Zielony
GPIO 18   | out_blue         | Kanał RGB - Niebieski

*UWAGA: GPIO12 może blokować start ESP32, jeśli przy włączaniu 
zasilania stan na pinie będzie wysoki.

----------------------------------------------------------------------
2. MAGISTRALA I2C (ADRESACJA)
----------------------------------------------------------------------
PIN GPIO  | FUNKCJA | ADRES I2C | URZĄDZENIE
----------|---------|-----------|-------------------------------------
GPIO 21   | SDA     | 0x68      | RTC DS1307 / DS3231 (Czas)
GPIO 22   | SCL     | 0x3C      | OLED SSD1306 128x64 (Wyświetlacz)

----------------------------------------------------------------------
3. PARAMETRY STEROWANIA (LOGIKA INTERWAŁU 1 MIN)
----------------------------------------------------------------------
- INDYWIDUALNA MOC: Każda z 7 lamp posiada własny "Box" (0-100%).
- DYNAMICZNY CZAS: Czas wschodu/zachodu wyliczany z różnicy minut.
- ZAPIS PAMIĘCI: Wszystkie ustawienia (Moc, Godziny, Minuty) są
  przechowywane w pamięci Flash (Restore Value: YES).
- BEZPIECZEŃSTWO: Soft-start (1s domyślnie) i płynne przejścia.

----------------------------------------------------------------------
4. DANE SIECIOWE (SECRETS.YAML)
----------------------------------------------------------------------
- WiFi SSID: MacKuz_20
- Hasło API/OTA: mercedes
- Hostname: esp-light.local
----------------------------------------------------------------------

----------------------------------------------------------------------
SPECYFIKACJA PINU GPIO 12 (Lampa 1)
----------------------------------------------------------------------
- FUNKCJA: MTDI (Strapping Pin)
- RYZYKO: Jeśli stan przy BOOT > 0V, ESP32 nie wystartuje poprawnie.
- ROZWIĄZANIE: Jeśli sterownik nie wstaje po zaniku prądu, należy 
  zastosować rezystor Pull-Down (10k Ohm) do masy na tym pinie.
----------------------------------------------------------------------