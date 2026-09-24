# 12 – První testovací tisk a bezpečné zvyšování rychlosti

Aktuální provozní config mého Rebela obsahuje:

```ini
[printer]
kinematics: cartesian
max_velocity: 400
max_accel: 6000
max_accel_to_decel: 6000
max_z_velocity: 30
max_z_accel: 200
```

> [!WARNING]
> To jsou limity mé konkrétní upravené tiskárny. **Nejsou to doporučené startovní hodnoty pro jiný Rebel 2.**

## První tisk

Po nové konfiguraci začni výrazně konzervativněji. Cílem prvního tisku není rychlost, ale ověření:
- první vrstvy,
- extruze,
- chlazení,
- meshe,
- maker,
- mechaniky,
- spolehlivého ukončení tisku.

Rychlost a akceleraci zvyšuj až potom a vždy postupně.

## Objemový průtok

Rychlost pohybu sama o sobě neříká, zda hotend stíhá tavit materiál.

Přibližně:

```text
objemový průtok = šířka čáry × výška vrstvy × rychlost
```

Například 0,45 × 0,20 × 100 mm/s = 9 mm³/s.

Při zvyšování rychlosti tedy může být limitem hotend nebo extruder dřív než mechanika.

## Co sledovat

Při zvyšování výkonu kontroluj ztracené kroky, posuny vrstev, vibrace, řemeny, teplotu motorů/driverů, kvalitu extruze a chlazení.

Input Shaper ani Pressure Advance nejsou „turbo“. Pomáhají s dynamikou pohybu a extruze, ale neodstraňují fyzikální limity stroje.

Cílem není nejvyšší číslo, které tiskárna jednou přežije. Cílem je nastavení, na kterém **opakovaně a spolehlivě tiskne**.

---

Tím končí základní cesta od konfigurace až k prvnímu bezpečně naladěnému tisku.
