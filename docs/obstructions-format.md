# Obstruction Table Format

HorizonOverlay reads a plain text table. Each data row describes the obstruction altitude at one azimuth.

```text
Az Alt
0 16
45 24
90 34
180 10
270 46
360 16
```

## Fields

- `Az`: azimuth in degrees. North is `0`, east is `90`, south is `180`, and west is `270`.
- `Alt`: obstruction altitude in degrees. `0` means the true horizon; positive values mean buildings, trees, terrain, or other local obstructions block the sky up to that altitude.

## Parsing Rules

- Blank lines are ignored.
- Non-numeric header lines such as `Az Alt` are ignored.
- Spaces, tabs, commas, and semicolons can be used as separators.
- `#` starts a comment. Everything after `#` on that line is ignored.
- Azimuth values are normalized to `0..360`.
- A positive azimuth that is an exact multiple of `360` is kept as `360`, so `360 16` can explicitly close the table.
- Altitude values are clamped to `-90..90`.
- If the same azimuth appears more than once, the later row overrides the earlier row.
- If `0` is missing, the first sample's altitude is copied to `0`.
- If `360` is missing, the altitude at `0` is copied to `360`.

## Shader Limit

The GPU shader fill path accepts up to `256` parsed obstruction samples. If the table has more samples, HorizonOverlay keeps working and automatically uses the CPU screen-mask fallback for wide-field fill rendering.

## Example

```text
# East-side buildings are high, south is lower.
Az Alt
0   12
45  25
90  38
135 22
180 8
225 14
270 19
315 15
360 12
```
