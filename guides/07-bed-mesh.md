# 07 – Bed Mesh

Aktuální konfigurace mého Rebela:

```ini
[bed_mesh]
speed: 350
horizontal_move_z: 7
mesh_min: 30, 20
mesh_max: 230, 230
probe_count: 4,4
```

Tyto hranice nejsou automaticky použitelné na jiném stroji.

## Nejdůležitější pravidlo

Klipper pohybuje **tryskou**, ale měří **sondou**. Proto při výpočtu bezpečné oblasti musíš započítat `x_offset` a `y_offset` BLTouch.

Každý krajní bod meshe musí být dosažitelný bez toho, aby:
- sonda opustila bed,
- vozík narazil do mechanického limitu,
- kabeláž byla napnutá.

## Test

Po homingu:

```text
BED_MESH_CLEAR
BED_MESH_CALIBRATE
```

Sleduj celý první průběh měření. Neodcházej od tiskárny.

KAMP řeš až ve chvíli, kdy klasický `BED_MESH_CALIBRATE` funguje spolehlivě.

➡️ **08 – Pressure Advance**
