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

Aktuálně uložené hodnoty tohoto konkrétního stroje jsou:

```ini
[input_shaper]
shaper_freq_x: 46
shaper_type_x: ei
shaper_freq_y: 52
shaper_type_y: ei
```

Tyto hodnoty **nekopíruj na jinou tiskárnu**.

## Kontrola akcelerometru

Nejdřív ověř komunikaci:

```text
ACCELEROMETER_QUERY
```

a potom šum:

```text
MEASURE_AXES_NOISE
```

Extrémně vysoký šum může ukazovat na problém se senzorem, napájením, připojením nebo mechanickými vibracemi.

## Současný jednoduchý postup

Klipper umí automatickou kalibraci přímo:

```text
SHAPER_CALIBRATE
```

Případně pouze jednu osu:

```text
SHAPER_CALIBRATE AXIS=X
SHAPER_CALIBRATE AXIS=Y
```

Výsledek neposuzuj jen podle doporučené frekvence. Klipper vypisuje také očekávané zbývající vibrace, smoothing a doporučený limit akcelerace.

Po kalibraci lze hodnoty uložit pomocí:

```text
SAVE_CONFIG
```

## TEST_RESONANCES

Pro podrobnější diagnostiku lze použít:

```text
TEST_RESONANCES AXIS=X
TEST_RESONANCES AXIS=Y
```

Měření vytváří výrazné vibrace. První test vždy sleduj a buď připraven použít `M112`.

> [!WARNING]
> Automatická kalibrace není něco, co má smysl spouštět před každým tiskem. Klipper výslovně upozorňuje, že dlouhodobé opakované buzení rezonancí zvyšuje mechanické namáhání tiskárny.

## Kdy měřit znovu

Měření zopakuj po změně:
- hmotnosti toolheadu,
- napnutí řemenů,
- významné části mechaniky,
- uchycení akcelerometru,
- nebo pokud se charakter rezonancí viditelně změnil.

Input Shaper neopravuje vůle, špatná ložiska ani povolené šrouby. Mechanika musí být nejdřív v pořádku.

➡️ **10 – Adaptive Bed Mesh**
