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

> [!CAUTION]
> První pokusy dělej u tiskárny s možností okamžitě vypnout napájení.

Až X i Y fungují opakovaně a bez tvrdých nárazů, pokračuj na BLTouch.

➡️ **03 – BLTouch a Z-home**
