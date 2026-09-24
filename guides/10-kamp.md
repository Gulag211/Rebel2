# 10 – Nativní Adaptive Bed Mesh v Klipperu

Původní konfigurace této tiskárny používala **KAMP (Klipper Adaptive Meshing & Purging)**. KAMP byl velmi užitečný projekt, protože přinesl adaptivní měření oblasti tisku ještě předtím, než tuto funkci Klipper obsahoval přímo.

Dnešní Klipper už ale umí adaptivní Bed Mesh nativně. Proto je tento repozitář pro nové instalace postavený především na vestavěné funkci Klipperu.

## Co jsme zjednodušili

Pro samotný adaptivní mesh už nepotřebujeme:

```ini
[gcode_macro BED_MESH_CALIBRATE]
[include KAMP_Settings.cfg]
```

ani v `PRINT_START`:

```text
SETUP_KAMP_MESHING ...
```

Místo toho používáme přímo:

```text
BED_MESH_CALIBRATE ADAPTIVE=1
```

## Nastavení v printer.cfg

V této konfiguraci je:

```ini
[bed_mesh]
speed: 350
horizontal_move_z: 7
mesh_min: 30, 20
mesh_max: 230, 230
probe_count: 4,4
adaptive_margin: 5
```

`mesh_min` a `mesh_max` stále určují bezpečnou maximální oblast, ve které se může mesh vytvářet.

`probe_count` určuje základní hustotu. Při adaptivním měření Klipper počet bodů automaticky škáluje podle poměru mezi celou definovanou oblastí a oblastí aktuálního tisku.

`adaptive_margin: 5` přidává 5 mm okraj kolem oblasti obsazené tisknutými objekty.

> [!IMPORTANT]
> Hodnoty 30,20 až 230,230 patří tomuto konkrétnímu Rebelu. Před použitím na jiné tiskárně ověř mechanické limity a offset sondy.

## PRINT_START

Princip je nyní jednoduchý:

```text
G28
BED_MESH_CLEAR
BED_MESH_CALIBRATE ADAPTIVE=1
```

Adaptivní mesh se vytváří pro každý tisk znovu. Nemá smysl jej ukládat a znovu používat pro jiný model.

## Odkud Klipper ví, kde jsou objekty?

Nativní Adaptive Bed Mesh používá objekty definované v G-code souboru. Proto musí být G-code a jeho zpracování nastavené tak, aby Klipper informace o jednotlivých objektech skutečně dostal.

V `printer.cfg` proto ponecháváme:

```ini
[exclude_object]
```

Pokud nejsou objekty správně definované, adaptivní mesh nemá z čeho určit oblast aktuálního tisku.

## A co KAMP?

KAMP tímto nezmizel z historie projektu. Soubor `KAMP_Settings.cfg` v repozitáři ponechávám jako **legacy/reference konfiguraci**, protože na této tiskárně byl používán.

KAMP navíc obsahuje funkce jako Smart Park a adaptivní purge, které nejsou totéž jako samotný nativní Adaptive Bed Mesh.

Pro nový základní setup ale nechci kvůli meshi vytvářet zbytečnou externí závislost.

> [!NOTE]
> Starší konfigurace nebo uživatel, který chce konkrétně KAMP Smart Park / Line Purge, může KAMP používat dál. Není ale potřeba kvůli samotnému adaptivnímu meshi.

## Nejdřív musí fungovat klasický mesh

Pokud řešíš problém, nejdřív otestuj:

```text
G28
BED_MESH_CLEAR
BED_MESH_CALIBRATE
```

Až když klasické měření funguje správně, testuj:

```text
BED_MESH_CALIBRATE ADAPTIVE=1
```

Tím oddělíš problém sondy nebo geometrie od problému s definicí objektů.

➡️ **11 – PRINT_START / PRINT_END**
