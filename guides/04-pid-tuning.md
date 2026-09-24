# 04 – Kontrola topení a PID tuning

PID kalibraci dělej až poté, co oba teplotní senzory ukazují správně.

> [!CAUTION]
> Pokud teplota při zapnutí topení neroste očekávaným způsobem, topení okamžitě vypni a hledej chybu v zapojení nebo konfiguraci.

## Hotend

Příklad kalibrace pro běžnou tiskovou teplotu:

```text
PID_CALIBRATE HEATER=extruder TARGET=210
```

Po dokončení:

```text
SAVE_CONFIG
```

## Bed

Například:

```text
PID_CALIBRATE HEATER=heater_bed TARGET=60
SAVE_CONFIG
```

Kalibruj kolem teplot, které skutečně používáš.

Hodnoty uložené v mém `SAVE_CONFIG` jsou výsledkem kalibrace mého konkrétního hotendu a bedu. Nepřebírej je jako univerzální.

Po restartu ověř, že regulace teploty je stabilní a nedochází k neobvyklým výkyvům.

➡️ **05 – Kalibrace extruderu**
