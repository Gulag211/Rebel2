# 01 – První spuštění a bezpečná kontrola

Předpokládáme SKR Mini E3 V3.0, Klipper, Moonraker a Mainsail.

> [!CAUTION]
> Po prvním načtení konfigurace ještě nepouštěj `G28`. Nejdřív ověř komunikaci, teploty, sondu a motory.

## 1. MCU

Na Raspberry Pi zjisti vlastní ID:

```bash
ls /dev/serial/by-id/*
```

Cestu vlož do:

```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_...
```

MCU ID v mém `printer.cfg` patří mé desce a na jiné nebude fungovat.

## 2. Teploty

Bez zapnutí topení zkontroluj v Mainsailu teplotu hotendu a bedu. Obě mají při studeném stroji dávat fyzikálně smysluplné hodnoty blízké okolí.

Můj config používá:
- hotend: `EPCOS 100K B57560G104F`,
- bed: `ATC Semitec 104GT-2`.

Pokud máš jiné termistory, změň `sensor_type` před testem topení.

## 3. Motory

Ověř jednotlivé motory:

```text
STEPPER_BUZZ STEPPER=stepper_x
STEPPER_BUZZ STEPPER=stepper_y
STEPPER_BUZZ STEPPER=stepper_z
STEPPER_BUZZ STEPPER=extruder
```

Pokud reaguje jiný motor, oprav zapojení/config. Ještě nehomuj.

## 4. BLTouch

Vyzkoušej:

```text
BLTOUCH_DEBUG COMMAND=pin_down
BLTOUCH_DEBUG COMMAND=pin_up
```

Potom ověř stav sondy pomocí `QUERY_PROBE`. Pokud Klipper nedokáže spolehlivě rozlišit sepnutou a nesepnutou sondu, Z-home nedělej.

## 5. Další krok

Teprve po těchto kontrolách pokračuj nastavením sensorless homingu X/Y.

➡️ **02 – Sensorless homing**
