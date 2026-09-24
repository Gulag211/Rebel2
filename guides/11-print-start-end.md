# 11 – PRINT_START, PRINT_END a slicer

Hlavní logiku startu a konce tisku je praktické držet v Klipperu. Slicer pak pouze předá teploty a zavolá makro.

## PRINT_START na mém Rebelu

Makro:
1. převezme `BED` a `EXTRUDER`,
2. začne zahřívat bed,
3. drží hotend na 150 °C pro přípravu,
4. čeká na bed,
5. provede `G28`,
6. připraví KAMP,
7. smaže starý mesh a vytvoří nový,
8. nahřeje hotend na cílovou teplotu,
9. spustí `LINE_PURGE`.

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
SMART_PARK
SETUP_KAMP_MESHING DISPLAY_PARAMETERS=-1 LCD_ENABLE=-1 FUZZ_ENABLE=-1
BED_MESH_CLEAR
BED_MESH_CALIBRATE
M104 S{EXTRUDER_TEMP}
TEMPERATURE_WAIT SENSOR=extruder MINIMUM={EXTRUDER_TEMP}
G92 E0
LINE_PURGE
```

Slicer musí předat oba parametry, například výsledným příkazem ve stylu:

```text
PRINT_START BED=60 EXTRUDER=210
```

Konkrétní proměnné sliceru se liší podle programu.

## PRINT_END

Současné makro vypne topení, zvedne Z, zaparkuje hlavu, provede retrakci, vypne ventilátor a motory a smaže aktivní mesh.

> [!WARNING]
> Parkovací souřadnice i Z-zvednutí musí odpovídat mechanickým limitům. U vysokého výtisku může slepé `G1 Z10` překročit maximum Z. To je vhodné před dalším rozšiřováním repozitáře upravit na podmíněný bezpečný zdvih.

Nedělej stejnou operaci současně ve sliceru i v makru. Dvojitý homing, purge nebo čekání na teplotu jen komplikuje diagnostiku.

➡️ **12 – První tisk a rychlost**
