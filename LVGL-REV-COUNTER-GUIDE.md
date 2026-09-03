# LVGL Gauge Guide: Rev Counter Background

This guide shows how to build a **rev counter (tachometer) background** in LVGL, then place a needle on top.

## 1. Pick your gauge style

Decide:

- Min and max RPM (example: `0` to `8000`)
- Redline start (example: `6500`)
- Tick count (example: every `500` RPM, major tick every `1000`)
- Label positions (for major ticks only)

Keeping these values fixed early helps the layout stay consistent.

## 2. Create the gauge container

Create a parent object to hold all gauge elements:

- Background arc(s)
- Tick marks
- Labels
- Needle
- Center cap

Set a square size so the gauge remains circular.

## 3. Draw the background ring

Use one or more `lv_arc` objects for:

- Base ring color (normal RPM range)
- Warning/redline segment

Tips:

- Disable arc interactivity so it behaves as decoration.
- Use rounded arc ends for a polished look.
- Keep ring width proportional to gauge size (about 8–14% of diameter works well).

## 4. Add tick marks

Build tick marks from short line segments or narrow objects rotated around the center.

- Minor ticks: thinner/shorter
- Major ticks: thicker/longer
- Redline ticks: change color near redline range

Map each RPM value to an angle (for example, from `-120°` to `120°`) and position ticks with trig (`sin/cos`) relative to center.

## 5. Add RPM labels

Place text labels near major ticks only (`0, 1, 2, ... 8` or full values).

- Use a readable font size
- Keep labels inside the ring but away from the needle path
- Align each label by center point to avoid jitter between digits

## 6. Create and style the needle

Use a thin rectangle/line object anchored at gauge center, or an image needle.

- Set transform pivot to needle base
- Rotate from the same angle map used by ticks
- Optionally add animation for smooth movement

## 7. Add center cap and optional effects

Place a small center circle over the needle base. Optional enhancements:

- Gradient or darker inner disc
- Glow/shadow for the needle
- Subtle border around the gauge

## 8. Update RPM at runtime

When RPM changes:

1. Clamp value to min/max
2. Convert RPM to angle
3. Rotate needle object
4. Optionally color-shift ring/needle near redline

For smoother visuals, animate the angle change instead of snapping.

## 9. Performance notes

- Pre-create all objects once; only update needle angle
- Avoid frequent label redraws
- Use image assets for complex backgrounds on low-power MCUs
- Keep anti-aliased elements balanced with frame-rate goals

## 10. Common pitfalls

- Mismatched angle mapping between ticks and needle
- Labels overlapping the ring at small sizes
- Redline arc not aligned to corresponding RPM value
- Over-updating UI from a high-frequency sensor callback

## Quick checklist

- [ ] Background ring visible and centered
- [ ] Major/minor ticks evenly spaced
- [ ] Labels readable and aligned
- [ ] Needle pivoted at exact center
- [ ] Needle angle matches RPM values
- [ ] Redline zone visually distinct

