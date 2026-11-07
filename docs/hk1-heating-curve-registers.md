# HK1 Heating Curve Registers — Hardware Validation

## Overview

This document describes two hardware-validated CanApiJson
register mappings for heating circuit 1 (HK1) on a
**Weishaupt WTC-GW 15-B**.

The registers allow read-only monitoring of:

- Heating curve slope (Heizkurvensteilheit)
- Heating curve parallel shift (Parallelverschiebung)

The findings were confirmed through controlled changes
at the physical heating controller and corresponding
CanApiJson responses.

## Register definitions

| Field | Heating curve slope | Parallel shift |
|---|---|---|
| MI | `0x02` | `0x02` |
| MX | `0x00` | `0x00` |
| OX | `0x254B` | `0x25AD` |
| OS | `0x02` | `0x02` |
| VS | 2 | 2 |
| Scale | 0.1 | 0.1 |
| Unit | — | K |
| Access validated | Read-only | Read-only |

### Heating curve slope

CanApiJson read request VG:

```text
010200254b0200020000
```

The decoded integer value is multiplied by `0.1`.

For example, raw value `85` corresponds to the displayed
setting `8.5`.

The mapping was verified against the controller display.

### Heating curve parallel shift

CanApiJson read request VG:

```text
01020025ad0200020000
```

Example response VG:

```text
02020025ad0200020055
```

The returned raw value `0x0055` equals decimal `85`.

With scaling factor `0.1`, the result is **8.5 K**.

## Hardware validation

Controlled A-B-A measurements were performed on a
Weishaupt WTC-GW 15-B.

### Parallel shift measurements

| Controller setting | Raw decimal | Response VG |
|---|---:|---|
| 8.5 K | 85 | `02020025ad0200020055` |
| 8.0 K | 80 | `02020025ad0200020050` |
| 8.5 K | 85 | `02020025ad0200020055` |
| 9.0 K | 90 | `02020025ad020002005a` |
| 8.5 K | 85 | `02020025ad0200020055` |

All successful readings matched the physical controller
setting.

The A-B-A sequence reduces the likelihood that the
observed changes were caused by unrelated data.

The parallel shift value was also observed updating
successfully in a running Home Assistant installation.

### Heating curve slope

The slope register `0x254B` was independently verified
against physical controller settings including 8.5
and 9.0.

## Research background

The parallel shift candidate originated from the
WEM CAN object mapping `0x26AD` documented by:

[geronet1/wem-python](https://github.com/geronet1/wem-python)

Comparison with known CanApiJson mappings suggested
`0x25AD`, which was subsequently validated against
the physical controller.

This correspondence should not be treated as a
universal register address conversion rule.

## Safety and limitations

- Only read-only CanApiJson operations were validated.
- No write requests were performed.
- Results were validated on one WTC-GW 15-B installation.
- Negative parallel shift values have not been
  confirmed against negative controller settings.
- Compatibility with other Weishaupt models and
  firmware versions is not yet established.
- The exact permitted parameter ranges are unknown.
- Clients should tolerate occasional empty or
  invalid HTTP responses.

## Related open-source contributions

The same register mappings were integrated into
two independent Home Assistant projects:

- [JS-DE-Tech PR #9 — HK1 slope](https://github.com/JS-DE-Tech/hacs-weishaupt-lan/pull/9)
- [JS-DE-Tech PR #10 — HK1 parallel shift](https://github.com/JS-DE-Tech/hacs-weishaupt-lan/pull/10)
- [kraiz PR #21 — Both HK1 sensors](https://github.com/kraiz/hassio-weishaupt/pull/21)

Further investigation:

- [CanApiJson Issue #8](https://github.com/BorgNumberOne/Weishaupt_CanApiJson/issues/8)

This contribution documents register behavior only.
It does not modify any existing protocol implementation.
