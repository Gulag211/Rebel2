# Rebel 2 + Klipper

Praktický český výukový repozitář vycházející z **mé vlastní stavby 3D tiskárny Rebel 2** a z jejího současného provozu na firmware **Klipper**.

> [!IMPORTANT]
> **Nejsem autorem původního projektu Rebel 2.** Tato tiskárna je moje vlastní stavba vytvořená podle tehdy dostupných návodů, podkladů a zkušeností komunity kolem open-source projektu Rebel 2. Tento repozitář dokumentuje **moji konkrétní stavbu, její pozdější úpravy a konfiguraci Klipperu**.

Cílem repozitáře není vydávat tuto konfiguraci za původní dokumentaci projektu Rebel 2 ani nabídnout „zázračný“ `printer.cfg`, který stačí slepě zkopírovat. Chci ukázat reálně provozovanou tiskárnu a postupně z tohoto repozitáře vytvořit **český výukový materiál**, podobně jako v mém projektu Ender3-Klipper-Guide.

> [!WARNING]
> Rebel 2 je stavebnice / open-source konstrukce, u které se jednotlivé stroje mohou výrazně lišit použitou elektronikou, motory, extruderem, hotendem, sondou i mechanickými úpravami. Hodnoty v tomto repozitáři proto **nepovažuj za univerzální konfiguraci pro každý Rebel 2**.

## 📖 NEJDŘÍV README, POTOM CONFIG

Nejrychlejší cesta k funkční tiskárně není začít kopírováním souborů.

Nejdřív si projdi README, zjisti, co odpovídá tvému stroji, a teprve potom přebírej jednotlivé části konfigurace.

```text
README → pochopit → porovnat s vlastní tiskárnou → upravit → bezpečně otestovat
```

Ne:

```text
zkopírovat → restartovat → chyba → hledat proč 😁
```

Stejně jako u Ender3-Klipper-Guide má být hlavní výhodou tohoto projektu to, že důležité věci budou vysvětlené **česky** a v souvislostech.

## 🔧 Konkrétní tiskárna, ze které repozitář vychází

Aktuální konfigurace v repozitáři používá mimo jiné:

- kinematiku Cartesian,
- BTT SKR Mini E3 V3.0,
- drivery TMC2209,
- Klipper + Moonraker + Mainsail,
- Raspberry Pi jako host,
- BLTouch,
- sensorless homing os X a Y,
- filament senzor,
- ADXL345 a Input Shaper,
- KAMP,
- 12864 displej,
- NeoPixel LED,
- samostatně řízené ventilátory,
- vlastní PRINT_START / PRINT_END makra.

Aktuálně nastavený pracovní rozsah je přibližně:

```text
X: 235 mm
Y: 235 mm
Z: 240 mm
```

To jsou hodnoty **této konkrétní tiskárny**, ne obecná specifikace každého Rebela 2.

## ⚠️ Co rozhodně nekopírovat naslepo

Každý z následujících údajů ověř nebo znovu zkalibruj na své tiskárně:

- MCU serial,
- směry motorů a `dir_pin`,
- koncové spínače / sensorless homing,
- `driver_sgthrs`,
- proudy motorů,
- `rotation_distance`,
- typy termistorů,
- PID hotendu a podložky,
- BLTouch offset,
- Z-offset,
- rozměry a limity os,
- Pressure Advance,
- Input Shaper,
- Bed Mesh,
- rychlosti a akcelerace.

> [!CAUTION]
> Nesprávná konfigurace není jen otázka kvality tisku. Chybný pin, směr pohybu, typ teplotního senzoru nebo špatně nastavené limity mohou způsobit náraz mechaniky nebo nebezpečné chování topení. První testy prováděj pod dohledem.

## 🗂️ Co je nyní v repozitáři

```text
Rebel2/
├── README.md
├── printer.cfg
├── macros.cfg
├── mainsail.cfg
├── moonraker.conf
├── KAMP_Settings.cfg
├── KlipperScreen.conf
├── crowsnest.conf
├── sonar.conf
└── timelapse.cfg
```

### `printer.cfg`

Hlavní konfigurace konkrétního Rebela. Obsahuje nastavení elektroniky, motorů, BLTouch, sensorless homingu, extruderu, bedu, ventilátorů, filament senzoru, NeoPixel, ADXL345, Input Shaperu, displeje a tiskových maker.

### `macros.cfg`

Doplňková makra používaná tiskárnou.

### `KAMP_Settings.cfg`

Konfigurace KAMP pro adaptivní funkce kolem oblasti tisku.

### Ostatní soubory

Repozitář obsahuje také konfiguraci služeb, které s tiskárnou souvisejí, například Moonraker, Mainsail, KlipperScreen, Crowsnest, Sonar a Timelapse.

## 🧩 Důležité INCLUDE v printer.cfg

Doplňkové služby a makra nestačí pouze nainstalovat nebo uložit vedle konfigurace. Pokud je má Klipper používat, musí být správně zapojené také v `printer.cfg`.

V této konfiguraci jsou například:

```ini
[gcode_macro BED_MESH_CALIBRATE]
[include macros.cfg]
[include mainsail.cfg]
[include KAMP_Settings.cfg]
[exclude_object]
```

Pokud některý soubor nebo službu nepoužíváš, nekopíruj její include automaticky. Naopak pokud ji nainstaluješ a konfigurace ji vyžaduje, musí o ní `printer.cfg` vědět.

## 🚀 Doporučené pořadí zprovoznění

Postupuj po jednotlivých vrstvách:

```text
Raspberry Pi + Klipper
        ↓
firmware řídicí desky
        ↓
MCU komunikace
        ↓
teplotní senzory
        ↓
směry motorů
        ↓
endstopy / sensorless homing
        ↓
BLTouch a bezpečný Z-home
        ↓
topení + PID
        ↓
extruder
        ↓
Z-offset
        ↓
Bed Mesh
        ↓
Pressure Advance
        ↓
ADXL345 + Input Shaper
        ↓
KAMP
        ↓
PRINT_START / PRINT_END
        ↓
první tisk a postupné zvyšování rychlosti
```

Pokud ještě bezpečně nefunguje základní homing, nemá smysl řešit KAMP nebo ladit maximální rychlost.

## 💾 Firmware pro BTT SKR Mini E3 V3.0

Konfigurace v tomto repozitáři používá SKR Mini E3 V3.0. Klipper firmware pro tuto desku se běžně připravuje s nastavením:

```text
Micro-controller Architecture: STMicroelectronics STM32
Processor model: STM32G0B1
Bootloader offset: 8KiB bootloader
Communication interface: USB
```

Po kompilaci vznikne:

```text
~/klipper/out/klipper.bin
```

Pro flash přes microSD se soubor přejmenuje na:

```text
firmware.bin
```

a vloží do kořenového adresáře karty.

## 🔌 MCU ID

MCU serial uložený v mém `printer.cfg` patří **mojí konkrétní desce**.

Na své tiskárně zjisti vlastní ID například:

```bash
ls /dev/serial/by-id/*
```

a použij vlastní cestu:

```ini
[mcu]
serial: /dev/serial/by-id/usb-Klipper_...
```

**Nikdy nekopíruj cizí MCU ID.**

## 📐 Mechanika a kalibrace

Rebel 2 je vhodný příklad toho, proč se konfigurace nemá přebírat bez kontroly. Tiskárna mohla během let dostat jinou elektroniku, extruder, hotend, sondu, držáky nebo další mechanické úpravy.

Po každé významné mechanické změně proto znovu ověř:

- skutečný rozsah X/Y/Z,
- zda nic nenaráží před dosažením softwarového limitu,
- polohu sondy vůči trysce,
- bezpečnou oblast Bed Meshe,
- Z-offset,
- kalibraci extruderu,
- případně rezonance a Input Shaper.

## 📚 Český návod krok za krokem

Výuková část je rozdělena do samostatných kapitol. Doporučuji jít postupně:

| Krok | Návod | Co řeší |
|---|---|---|
| 00 | [Základy configu](guides/00-config-basics.md) | piny, `!`, `^`, mechanické limity |
| 01 | [První spuštění](guides/01-first-start.md) | MCU, teploty, BLTouch a motory |
| 02 | [Sensorless homing](guides/02-sensorless-homing.md) | TMC2209, StallGuard, X/Y |
| 03 | [BLTouch](guides/03-bltouch.md) | sonda a bezpečný první Z-home |
| 04 | [PID tuning](guides/04-pid-tuning.md) | hotend a vyhřívaná podložka |
| 05 | [Kalibrace extruderu](guides/05-extruder-calibration.md) | `rotation_distance` a základ PA |
| 06 | [Z-offset](guides/06-z-offset.md) | `PROBE_CALIBRATE`, `TESTZ` |
| 07 | [Bed Mesh](guides/07-bed-mesh.md) | bezpečná měřicí oblast |
| 08 | [Pressure Advance](guides/08-pressure-advance.md) | princip a podmínky kalibrace |
| 09 | [ADXL345 + Input Shaper](guides/09-input-shaper-adxl345.md) | rezonance a vlastní měření |
| 10 | [KAMP](guides/10-kamp.md) | adaptivní mesh, include, purge |
| 11 | [PRINT_START / PRINT_END](guides/11-print-start-end.md) | makra a komunikace se slicerem |
| 12 | [První tisk a rychlost](guides/12-first-print-speed.md) | bezpečné zvyšování výkonu |

```text
README → 00 → 01 → 02 → 03 → ... → 12
```

> [!TIP]
> Config ukazuje **jak je nastavený můj stroj**. Kapitoly v `guides/` vysvětlují **proč dané nastavení existuje a co musíš ověřit na svém stroji**.

## 🔗 Související projekt

Pokud s Klipperem začínáš a chceš podrobnější český postup krok za krokem, podívej se také na můj repozitář:

**Ender3-Klipper-Guide**

Je koncipovaný jako výukový návod a principy Klipperu jsou z velké části přenositelné i na jinou tiskárnu. Konkrétní piny, mechaniku a kalibrační hodnoty ale vždy přizpůsob svému stroji.

## 👤 O projektu

Tento repozitář dokumentuje **moji vlastní stavbu Rebel 2**, kterou jsem postavil podle veřejně dostupných podkladů tehdejšího open-source projektu Rebel 2 a následně v průběhu let upravoval.

Repozitář tedy není oficiální dokumentací původního projektu Rebel 2 a netvrdím, že jsem autorem původní konstrukce.

Je to praktická dokumentace mého stroje a český výukový materiál pro lidi, kteří chtějí pochopit, jak lze starší vlastní 3D tiskárnu modernizovat a provozovat na Klipperu.

Pokud najdeš chybu nebo máš užitečné vylepšení, můžeš otevřít Issue nebo Pull Request.
