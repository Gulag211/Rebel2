# 05 – Kalibrace extruderu

Aktuální config mého Rebela obsahuje:

```ini
[extruder]
rotation_distance: 21.673
pressure_advance: 0.026
pressure_advance_smooth_time: 0.08
```

Tyto hodnoty patří mému extruderu a jeho mechanice.

## Rotation distance

Cílem je, aby příkaz k posunu určité délky filamentu odpovídal skutečnému posunu.

Před měřením si označ filament, zahřej hotend na vhodnou teplotu a vytlač přesně známou délku. Z naměřené skutečné délky dopočítej novou `rotation_distance`.

Obecný vztah:

```text
nová rotation_distance =
stará rotation_distance × skutečně vytlačená délka / požadovaná délka
```

Příklad: pokud je stará hodnota 21.673, požadavek byl 100 mm a skutečný posun 95 mm:

```text
21.673 × 95 / 100 = 20.58935
```

Měř několikrát. Jedno nepřesné měření není dobrý základ kalibrace.

## Pressure Advance

PA kalibruj až po správné kalibraci extruderu a flow. Hodnota `0.026` v repozitáři není doporučení pro jiný extruder nebo filament.

➡️ **06 – Z-offset**
