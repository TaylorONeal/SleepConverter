# SleepConverter

Convert **SleepON / Go2Sleep** ring data into a **Wellue/Viatom O2 Ring CSV** that **SleepHQ** accepts —
with one click, fully offline, and now with built-in sleep analysis and charts.

![Example night dashboard](docs/assets/example-night.svg)

> 📖 **[Read the detailed data-format & conversion write-up →](docs/DATA_FORMAT.md)** —
> the exact SleepON export layout, the UTC/timezone gotcha, the SleepHQ output spec,
> the conversion pipeline, and the insights/visualizations, all verified against a real export.

---

## Why it exists

SleepON/Go2Sleep rings record excellent overnight SpO₂ and pulse data, but the
export doesn't drop straight into [SleepHQ](https://sleephq.com). This is a single,
self-contained HTML file that reformats it correctly — no account, no upload, no
backend. **Your sleep data never leaves your device.**

## What it does

- **Reads real SleepON exports** — the cloud `RawDataSecond_…​.xlsx` export, ViHealth-style
  flat CSVs, and the "packed" `spo2Raw`/`heartRaw` array layout. `.csv`, `.xlsx`, `.xls`,
  comma/semicolon/tab-delimited. Fuzzy column matching; metadata rows above the header are skipped.

- **Converts correctly** — the parts the old version got wrong:
  - **Localises time.** SleepON stores `Time` in **UTC** with a separate `Time_Zone` column
    (e.g. `-05:00`). The converter applies the offset so timestamps are the real local time
    SleepHQ aligns against — deterministically, regardless of your computer's own timezone.
  - **Keeps the whole night, across midnight** (the old version silently dropped everything after 00:00).
  - Robust dates (ISO/`DD-MM`/`MM-DD`/AM-PM/Excel-serial/epoch, auto day-vs-month), range
    validation, `Validity` filtering, de-duplication to one reading per second.
  - Emits the exact Wellue/Viatom columns SleepHQ expects, named `YYYYMMDDHHMMSS.csv`.

- **Analyses your night** — computed and charted locally (inline SVG, no dependencies):
  - Avg / lowest SpO₂, **% of night below 90% / 88%**, **desaturation events** and approximate **ODI**
  - Pulse average and range
  - **Hypnogram** and sleep-stage breakdown (from the `Stage` column)
  - SpO₂-over-night (with 90% threshold + shaded dips), pulse trend, and time-in-band charts

- **Mobile-first UX**
  - Large, non-overlapping tap targets that stack cleanly on phones
  - Native picker via a real `<label>` + `<input type="file">` so uploads work reliably on
    **iOS and Android** (Files, iCloud Drive, Google Drive, Photos); drag-and-drop on desktop
  - Microinteractions and animations throughout, with full `prefers-reduced-motion` support
  - **Try it with sample data** button, and a **Conversion details** log for troubleshooting

## How to use

1. Open `SleepConverter.html` in any modern browser (or host it anywhere static).
2. **Add your file** — choose it, drag it in, or tap *Try it with sample data*.
3. **Convert** — cleaned and reformatted client-side; review the stats and charts.
4. **Download** the `YYYYMMDDHHMMSS.csv` and import it into SleepHQ's O2 Ring uploader.

## File formats (quick reference)

**Input — SleepON cloud `RawDataSecond` export** (`.xlsx`):

| Time (UTC) | Time_Zone | Heart_Rate(BPM) | Oxygen(%) | Active | Stage | Validity |
|---|---|---|---|---|---|---|
| `2025-10-04T00:59:04.000Z` | `-05:00` | `64` | `96` | `0` | `Wake` | `1` |

**Output — SleepHQ / Wellue O2 Ring CSV:**

```csv
Time,Oxygen Level,Pulse Rate,Motion,O2 Reminder,PR Reminder
03/10/2025 19:59:04,96,64,0,0,0
04/10/2025 03:39:59,95,66,0,0,0
```

Full details, the timezone worked-example, supported variants, and the pipeline
diagram are in **[docs/DATA_FORMAT.md](docs/DATA_FORMAT.md)**.

## Technology

- Vanilla HTML / CSS / JavaScript — no build step, no framework
- [SheetJS (xlsx)](https://github.com/SheetJS/sheetjs) for Excel parsing (the only external dependency, via CDN)
- Charts are hand-rendered inline SVG — runs 100% client-side and offline
