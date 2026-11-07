# Additional HK1 CanApiJson registers — hardware validation

This contribution extends the two heating-curve registers contributed in
[PR #9](https://github.com/BorgNumberOne/Weishaupt_CanApiJson/pull/9).
These findings were made on one **Weishaupt WTC-GW 15-B** with an enabled
Systemgerät CanApiJson interface. Only **read operations** were issued.
No claim is made that the mapping works on every firmware version.

## Register table

For all addresses below: **MI=0x02, MX=0x00, OS=0x02**. GET uses command
`0x01`; successful responses use command `0x02`. Data is big-endian.

| HK1 controller setting | OX | GET VS | Response VS | Raw observed | Decoded / observed | Validation |
|---|---|---:|---:|---:|---|---|
| Vorlaufsolltemperatur Heizgrenze | `0x25C7` | 2 | 2 | 80 | 8.0 °C | A-B-A 8→7→8 °C |
| Frostschutz Außentemperatur | `0x2554` | 2 | 2 | 0 | 0.0 °C | Display match |
| Aufheizoptimierung | `0x25A3` | 1 | 1 | 0 | Aus | Display match for 0 |
| Gebäudebauweise | `0x256E` | 1 | 1 | 2 | leicht | Display match for 2 |
| Vorlaufsolltemperatur minimal | `0x251B` | **1** | **2** | 150 | 15.0 °C | Repeated and display matched |
| Vorlaufsolltemperatur maximal | `0x251C` | **1** | **2** | 800 | 80.0 °C | Repeated and display matched |
| So/Wi Umschalttemperatur | `0x257B` | **1** | **2** | 160 | 16.0 °C | Repeated and display matched |
| Niveauanhebung Außentemperatur | `0x2581` | 2 | 2 | signed −200 | −20.0 °C / Aus | Display match |
| Priorität Warmwasser | `0x2537` | 1 | 1 | 0 | Vorrang | Display match for 0 |
| Raumsolltemperatur Heizgrenze | `0x257C` | 1 | 1 | 1 | Ein | Display match for 1 |

All temperature values above use a 0.1 scaling factor. For `0x2581`, the
observed two-byte unsigned word `0xFF38` represents −200 as signed int16.
The `−20.0 °C` representation is a special *Off* setting on the tested
controller, **not** evidence that the device is measuring −20 °C outdoors.
Other enum states have not all been checked against device displays.

## Important request-vs-response size mismatch

The following three registers only yielded valid data when requested with
**VS=1**, but the Systemgerät replied with **VS=2**:

| Parameter | Read request VG | Successful response VG |
|---|---|---|
| Minimum flow target | `010200251b02000100` | `020200251b0200020096` |
| Maximum flow target | `010200251c02000100` | `020200251c0200020320` |
| Summer/winter temperature | `010200257b02000100` | `020200257b02000200a0` |

For all three, a GET request using `VS=2` previously returned a `CMD_ERROR`
frame. Two independent GET requests with `VS=1` returned the same values as
shown above, matching the physical controller.

**Decoder advice:** do not globally disable response-address validation to
accommodate this quirk. Allow the outgoing GET VS to differ from the
canonical incoming VS for a specifically configured register, and continue
to check MI, MX, OX, OS and incoming VS. Writes are not validated here.

## Additional notes and limitations

- Read-only tests used the `DDC` request source and did not issue SET calls.
- Device response can occasionally be HTTP 200 with an empty body. Do not
  interpret this alone as proof that a register is unavailable.
- `0x25C8` gave raw 1 with a one-byte reply but its exact menu semantics
  were **not** verified; it is intentionally omitted from the table.
- HK1 slope `0x254B` has occasionally returned an unexplained raw zero in
  an otherwise valid reply. This requires further reliability investigation.
- Values are validated on **one installation**; not a universal conversion
  rule from the `0x26xx` mappings of other Weishaupt CAN modules.
- No writable control, automated heating change or safety-setting write is
  part of this contribution.
