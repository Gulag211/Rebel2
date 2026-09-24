# 09 – ADXL345 a Input Shaper

Můj Rebel používá ADXL345 připojený k Raspberry Pi:

```ini
[mcu rpi]
serial: /tmp/klipper_host_mcu

[adxl345]
cs_pin: rpi:None

[resonance_tester]
accel_chip: adxl345
probe_points:
    110, 100, 20
```

Aktuálně uložený Input Shaper:

```ini
[input_shaper]
shaper_freq_x: 46
shaper_type_x: ei
shaper_freq_y: 52
shaper_type_y: ei
```

Tyto frekvence jsou měření mého stroje. **Nekopíruj je.**

## Kontrola akcelerometru

Nejdřív:

```text
ACCELEROMETER_QUERY
```

Potom lze provést měření rezonancí:

```text
SHAPER_CALIBRATE
```

Při měření se tiskárna výrazně rozkmitá. Zkontroluj dotažení mechaniky, kabeláž a pevné uchycení akcelerometru.

Po změně hmotnosti toolheadu, napnutí řemenů nebo významné mechanické úpravě měření zopakuj.

Input Shaper neodstraňuje mechanické problémy. Nejdřív oprav vůle, špatná ložiska nebo řemeny.

➡️ **10 – KAMP**
