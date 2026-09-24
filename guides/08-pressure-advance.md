# 08 – Pressure Advance

Pressure Advance kompenzuje změny tlaku v trysce při změnách rychlosti extruze.

V mém configu je:

```ini
pressure_advance: 0.026
pressure_advance_smooth_time: 0.08
```

Neber `0.026` jako univerzální hodnotu. Výsledek ovlivňuje extruder, Bowden/direct drive, hotend, tryska, materiál, teplota i rychlost.

## Před kalibrací

Musí být správně:
- `rotation_distance`,
- flow/extrusion multiplier,
- teplota filamentu,
- mechanika extruderu.

PA potom kalibruj podle oficiálního postupu Klipperu nebo kalibračních nástrojů sliceru, který používáš.

Kalibraci dělej pro reálnou kombinaci materiálu a tiskových podmínek. Extrémní PA může způsobovat problémy s extruzí a není náhradou za mechanicky správný extruder.

➡️ **09 – ADXL345 a Input Shaper**
