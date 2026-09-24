# 06 – Kalibrace Z-offsetu

Z-offset určuje vztah mezi okamžikem sepnutí BLTouch a skutečnou výškou trysky nad bedem.

Hodnotu z mého `SAVE_CONFIG` nekopíruj.

## Postup

Nejdřív zahomuj:

```text
G28
```

Potom spusť:

```text
PROBE_CALIBRATE
```

Klipper přesune trysku do kalibrační polohy. Pomocí příkazů `TESTZ` ji opatrně přibližuj k papírku na podložce, například:

```text
TESTZ Z=-0.1
```

Pro jemnější krok:

```text
TESTZ Z=-0.01
```

Po nalezení správné výšky:

```text
ACCEPT
SAVE_CONFIG
```

> [!CAUTION]
> Nedělej velké záporné kroky těsně nad bedem. Z-offset dolaďuj pomalu.

Po změně trysky, hotendu, držáku sondy nebo jiné mechaniky Z-offset znovu ověř.

➡️ **07 – Bed Mesh**
