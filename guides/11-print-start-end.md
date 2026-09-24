# 11 – PRINT_START, PRINT_END a slicer

Hlavní logiku startu a konce tisku držíme v Klipperu. Slicer předá požadované teploty a zavolá makro.

## PRINT_START na mém Rebelu

Aktuální postup:

1. převezme `BED` a `EXTRUDER`,
2. začne zahřívat bed,
3. drží hotend na 150 °C během přípravy,
4. počká na bed a přípravnou teplotu hotendu,
5. provede `G28`,
6. smaže starý mesh,
7. vytvoří **nativní adaptivní mesh Klipperu**,
8. nahřeje hotend na tiskovou teplotu.

Důležitá část:

```ini
{% set BED_TEMP = params.BED|float %}
{% set EXTRUDER_TEMP = params.EXTRUDER|float %}

M104 S150
M140 S{BED_TEMP}
M190 S{BED_TEMP}
M109 S150

G90
M83
G28

BED_MESH_CLEAR
BED_MESH_CALIBRATE ADAPTIVE=1

M104 S{EXTRUDER_TEMP}
TEMPERATURE_WAIT SENSOR=extruder MINIMUM={EXTRUDER_TEMP}
G92 E0
```

Oproti starší verzi už start tisku nepotřebuje KAMP makra `SMART_PARK`, `SETUP_KAMP_MESHING` ani KAMP přepsání `BED_MESH_CALIBRATE`.

## Purge

Původní konfigurace používala KAMP `LINE_PURGE`. Po odstranění KAMP jako povinné závislosti jej `PRINT_START` automaticky nevolá.

Purge lze řešit:
- start G-code sliceru,
- vlastním jednoduchým Klipper makrem,
- nebo volitelným KAMP purge, pokud jej chce uživatel zachovat.

Důležité je mít purge pouze na jednom místě.

## Parametry ze sliceru

Slicer musí předat oba parametry, například výsledným příkazem:

```text
PRINT_START BED=60 EXTRUDER=210
```

Konkrétní názvy proměnných pro teploty se liší podle sliceru.

## PRINT_END

Aktuální `PRINT_END`:
- počká na dokončení pohybů,
- vypne topení a ofuk,
- provede malou retrakci,
- bezpečně zvedne Z pouze do povoleného maxima,
- zaparkuje na X0 Y200,
- vymaže mesh,
- až nakonec vypne motory.

Parkovací souřadnice patří konkrétnímu stroji a na jiné tiskárně je nutné je přizpůsobit.

➡️ **12 – První tisk a rychlost**
