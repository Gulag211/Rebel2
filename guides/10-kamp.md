# 10 – KAMP

KAMP je v tomto stroji doplňková vrstva nad funkčním Klipperem a Bed Meshem.

V `printer.cfg` jsou důležité zejména:

```ini
[gcode_macro BED_MESH_CALIBRATE]
[include KAMP_Settings.cfg]
[exclude_object]
```

a samozřejmě musí být dostupné soubory KAMP, které tvoje instalace očekává.

> [!IMPORTANT]
> Nestačí něco pouze nainstalovat. Pokud konfigurace funkci používá, musí být správně zapojená přes include a odpovídající sekce v Klipperu.

## Nejdřív klasický mesh

Před KAMP musí spolehlivě fungovat:

```text
G28
BED_MESH_CLEAR
BED_MESH_CALIBRATE
```

Teprve potom řeš adaptivní oblast a purge.

Můj `PRINT_START` používá mimo jiné:

```text
SETUP_KAMP_MESHING DISPLAY_PARAMETERS=-1 LCD_ENABLE=-1 FUZZ_ENABLE=-1
BED_MESH_CLEAR
BED_MESH_CALIBRATE
...
LINE_PURGE
```

Pokud KAMP selže, vrať se ke klasickému meshi. Tím rychle zjistíš, zda je problém v základní konfiguraci tiskárny, nebo v doplňkové vrstvě.

➡️ **11 – PRINT_START / PRINT_END**
