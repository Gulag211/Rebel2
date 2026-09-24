# 00 – Než začneš upravovat printer.cfg

Tato kapitola je výchozí bod pro práci s konfigurací mého **Rebelu 2**. Nejde o univerzální config pro všechny Rebely.

## Modifikátory pinů

Klipper používá před názvem pinu znaky, které mění jeho chování:

- `!` invertuje logiku pinu. U `dir_pin` tím obrátíš směr motoru.
- `^` zapne interní pull-up.
- `~` zapne interní pull-down, pokud jej MCU podporuje.
- `#` označuje komentář.

Příklad z této tiskárny:

```ini
[stepper_x]
dir_pin: !PB12

[tmc2209 stepper_x]
diag_pin: ^PC0
```

Nekopíruj tyto znaky automaticky mezi osami. Každý pin posuzuj samostatně.

## Změř skutečnou mechaniku

Aktuální config mého stroje používá přibližně X 235, Y 235 a Z 240 mm. To neznamená, že stejné hodnoty platí pro jiný Rebel 2.

Po změně toolheadu, hotendu, sondy nebo jiné mechaniky znovu ověř:
- skutečný rozsah X/Y/Z,
- polohu trysky vůči bedu,
- polohu sondy,
- `position_min` a `position_max`,
- oblast `bed_mesh`,
- `safe_z_home`,
- parkovací pozice maker.

> [!CAUTION]
> Rozměr podložky není automaticky bezpečný rozsah pohybu. Limity musí odpovídat skutečné mechanice.

## Proč nezačínat kopírováním

Nejdřív pochop konkrétní část configu, potom ji přenes na svůj stroj a otestuj. Zvlášť opatrně pracuj s piny topení, termistory, směry motorů a endstopy.

Další krok: **01 – První spuštění a bezpečná kontrola**.


## Poznámka k rotation_distance osy X

Na tomto Rebelu je na ose X použita **GT2 řemenice 16T**, proto je:

```ini
rotation_distance: 32
```

GT2 má rozteč zubů 2 mm, takže jedna otáčka 16zubé řemenice posune řemen o:

```text
16 × 2 mm = 32 mm
```

U běžnější **GT2 řemenice 20T** vychází:

```text
20 × 2 mm = 40 mm
```

a tedy typicky:

```ini
rotation_distance: 40
```

Proto není rozdíl mezi X=32 a Y=40 v této konfiguraci překlep. Osy používají rozdílné řemenice. Před kopírováním konfigurace vždy ověř počet zubů řemenice na vlastní tiskárně.
