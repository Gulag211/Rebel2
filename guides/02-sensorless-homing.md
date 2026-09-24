# 02 – Sensorless homing X/Y

Můj Rebel 2 používá TMC2209 a virtuální endstopy.

Aktuální konfigurace:

```ini
[stepper_x]
endstop_pin: tmc2209_stepper_x:virtual_endstop
homing_retract_dist: 0

[tmc2209 stepper_x]
diag_pin: ^PC0
driver_sgthrs: 100

[stepper_y]
endstop_pin: tmc2209_stepper_y:virtual_endstop
homing_retract_dist: 0

[tmc2209 stepper_y]
diag_pin: ^PC1
driver_sgthrs: 100
```

> [!WARNING]
> Hodnota `driver_sgthrs: 100` je nastavení mého stroje, ne univerzální hodnota.

U TMC2209 vyšší `driver_sgthrs` znamená vyšší citlivost StallGuardu. Příliš vysoká hodnota může vyvolat falešný konec osy, příliš nízká může způsobit tvrdé tlačení do dorazu.

## Bezpečný postup

Nejdřív ověř `STEPPER_BUZZ` pro X a Y. Potom testuj osy odděleně:

```text
G28 X
```

a až po spolehlivém X:

```text
G28 Y
```

Citlivost lze pro test měnit například:

```text
SET_TMC_FIELD STEPPER=stepper_x FIELD=SGTHRS VALUE=100
SET_TMC_FIELD STEPPER=stepper_y FIELD=SGTHRS VALUE=100
```

Testuj opakovaně z různých míst osy. Sensorless homing nerozezná skutečný konec osy od jiné mechanické překážky.

## Co doporučuje současný Klipper

Sensorless homing je citlivý nejen na `driver_sgthrs`, ale také na rychlost homingu, proud motoru, mechanické zatížení a teplotu motoru.

Klipper doporučuje jako rozumný výchozí bod homing speed přibližně odpovídající jedné otáčce motoru za dvě sekundy, tedy zhruba:

```text
homing_speed ≈ rotation_distance / 2
```

Na tomto Rebelu je X `rotation_distance: 32`, protože používá GT2 řemenici 16T (16 × 2 mm = 32 mm). Běžná GT2 řemenice 20T má `rotation_distance: 40`. Osa Y zde používá právě hodnotu 40. Provozní `homing_speed: 50` je ale proti konzervativnímu výchozímu bodu pro ladění výrazně vyšší. **Neměním ji automaticky**, protože hodnoty StallGuardu byly laděné na konkrétním stroji. Pokud budeš sensorless homing znovu kalibrovat, začni současnou metodikou Klipperu a nalaď rychlost, proud a SGTHRS společně.

Po každém sensorless home je vhodné odjet několik milimetrů od dorazu a před dalším sensorless homingem nechat driver alespoň přibližně 2 sekundy v klidu, aby se vyčistil stall flag.

> [!NOTE]
> `homing_retract_dist: 0` už zde máme správně. Klipper druhý homing pohyb u sensorless homingu nedoporučuje.

> [!CAUTION]
> První pokusy dělej u tiskárny s možností okamžitě vypnout napájení.

Až X i Y fungují opakovaně a bez tvrdých nárazů, pokračuj na BLTouch.

➡️ **03 – BLTouch a Z-home**
