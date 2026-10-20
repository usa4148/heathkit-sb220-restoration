# Heathkit SB-220 Restoration Plan

A restoration plan for the Heathkit SB-220 2 kW grounded-grid linear amplifier (two 3-500Z tubes). It covers which parts fail with age, the 3 kV supply rebuild, the bias zener and grid-protection issues, a phased restoration and first power-up procedure, tune-up readings, a symptom quick-reference, optional modifications (clearly separated from factory practice) and maintenance.

**Status:** Rev. 1. It is built from the Heath service bulletins and field sources (AF6C, AG6K, VA7JW, KV5V, KA2C, the Heath list, Heathkit-HFAmps), checked against the original Heath manuals (595-682-07 and 595-1122-16).

## Files

- [`sb220-restoration-plan.md`](sb220-restoration-plan.md) — source (pandoc markdown)
- [`sb220-restoration-plan.pdf`](sb220-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc sb220-restoration-plan.md -o sb220-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Safety

**Lethal voltage.** The plate supply is about 2500 V DC (CW/TUNE) to 3000 V DC (SSB), stored in a 200 µF × 8 capacitor bank. The top-cover interlock is a backup, not a discharge procedure. Unplug the amplifier, verify zero volts with a separate meter, and discharge with a grounded shorting stick before touching anything. Never power the amplifier with the cover off.

## Sources

- Heath SB-220 Assembly Manual 595-682-07 (1969) and SB-220 Manual 595-1122-16
- Heath SB-220 service bulletins SB-220-1 to -30, transcribed on the Heath mailing list
- AF6C *Heathkit of the Month #49*; AG6K *Circuit Improvements for the SB-220*; VA7JW *SB-220 Compendium*; KV5V *Notes on Rebuilding an SB-220*; KA2C restoration notes
- Heathkit-HFAmps and Heathkit-Radios groups on groups.io

See also: [heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration) · [heathkit-hw101-restoration](https://github.com/usa4148/heathkit-hw101-restoration) · [heathkit-sb102-restoration](https://github.com/usa4148/heathkit-sb102-restoration) · [heathkit-sb110-restoration](https://github.com/usa4148/heathkit-sb110-restoration)
