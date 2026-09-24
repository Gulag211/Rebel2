# 03 – BLTouch a první bezpečný Z-home

Můj Rebel používá BLTouch jako virtuální Z-endstop.

Aktuální část configu:

```ini
[bltouch]
sensor_pin: ^PC14
control_pin: PA1
x_offset: 23
y_offset: 3
pin_move_time: 0.1
speed: 10

[stepper_z]
endstop_pin: probe:z_virtual_endstop
```

Offsety 23/3 patří konkrétnímu držáku na mém stroji. Změř si vlastní.

## 1. Mechanická kontrola

Při zasunutém pinu musí být sonda bezpečně nad špičkou trysky. Při vysunutém pinu musí sepnout dřív, než tryska narazí do bedu.

## 2. Elektrický test

```text
BLTOUCH_DEBUG COMMAND=pin_down
QUERY_PROBE
```

Jemně pin ručně sepni a znovu spusť `QUERY_PROBE`. Stav se musí změnit. Potom:

```text
BLTOUCH_DEBUG COMMAND=pin_up
```

## 3. Safe Z Home

Můj config používá:

```ini
[safe_z_home]
home_xy_position: 117,117
speed: 350
z_hop: 10
z_hop_speed: 16
```

Tyto hodnoty odpovídají mému stroji. Před použitím ověř, že při dané poloze je **sonda** skutečně nad bedem.

První Z-home dělej s dostatečnou rezervou a připraveným vypínačem. Jakmile je sonda ověřená, můžeš testovat kompletní `G28`.

➡️ **04 – PID tuning**
