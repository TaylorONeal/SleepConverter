# SleepConverter

Convert SleepON / Go2Sleep ring data into a Wellue/Viatom **O2 Ring CSV** that **SleepHQ** accepts.

# Sleep Ring Converter 💤🔁📈

**Transform SleepON ring data into a SleepHQ-ready CSV — in your browser, fully offline.**

This is a single self-contained HTML file. Open it (or host it anywhere static) and convert
SleepON 3 / Go2Sleep `CSV` or `XLSX` exports into the format SleepHQ’s O2 Ring importer
recognises. No account, no upload, no backend — your sleep data never leaves your device.

---

## 🚀 What it does

- **Reads real-world SleepON exports**
  - Flat per-second/per-4s tables (`RawData-Second-…​.xlsx` and CSV exports)
  - “Packed” exports where `spo2Raw` / `heartRaw` hold comma-separated sample arrays per row (auto-expanded)
  - `.csv`, `.xlsx`, `.xls`, tab- or semicolon-delimited text
  - Fuzzy column matching (Time/Date, SpO₂/Oxygen, Pulse/PR/Heart Rate, Motion)
  - Metadata/title rows above the header are skipped automatically

- **Correct, lossless conversion**
  - **Keeps the whole night, including across midnight** (the old version silently dropped everything after 00:00)
  - Robust date parsing: ISO, `DD/MM/YYYY`, `MM/DD/YYYY`, AM/PM, Excel serial dates, epoch timestamps — with automatic Day/Month vs Month/Day detection (and a manual override)
  - Filters physiologically impossible readings and de-duplicates identical timestamps
  - Outputs the exact Wellue/Viatom O2 Ring column layout SleepHQ expects

- **Clear results & troubleshooting**
  - Animated summary stats (avg/min/max SpO₂ & pulse, reading count, duration)
  - Preview of the first rows
  - A **Conversion details** log showing detected columns, date order, skipped rows and the time range — so if a file ever doesn’t parse, you can see exactly why

- **Mobile-first UX**
  - Large, non-overlapping tap targets that stack cleanly on phones
  - Native file picker via a real `<label>` + `<input type="file">` so uploading works reliably on **iOS and Android** (Files, iCloud Drive, Google Drive, Photos)
  - Drag-and-drop on desktop, “Try it with sample data” for a quick demo
  - Microinteractions and animations throughout, with full `prefers-reduced-motion` support

---

## 🛠 How to use

1. Open `SleepConverter.html` in any modern browser (or visit your hosted copy).
2. **Add your file** — choose it, drag it in, or tap *Try it with sample data*.
3. **Convert** — the data is cleaned and reformatted client-side.
4. **Download** the `YYYYMMDDHHMMSS.csv` file and import it into SleepHQ’s O2 Ring uploader.

---

## 📁 File format

**Input (SleepON / Go2Sleep):** `.csv` or `.xlsx` containing some variation of
`Time`/`Date`, `SpO2`/`Oxygen`, `Pulse`/`PR`/`Heart Rate`, and optionally `Motion`
— or the packed `startTime` + `spo2Raw`/`heartRaw` array layout.

**Output (SleepHQ / Wellue O2 Ring CSV):**
```csv
Time,Oxygen Level,Pulse Rate,Motion,O2 Reminder,PR Reminder
28/03/2025 23:59:56,97,58,0,0,0
29/03/2025 00:00:00,96,57,0,0,0
...
```
- Time format: `DD/MM/YYYY HH:mm:ss` (seconds kept, since the ring samples sub-minute)
- Rows sorted chronologically and de-duplicated

---

## 🧰 Technology

- Vanilla HTML / CSS / JavaScript — no build step, no framework
- [SheetJS (xlsx)](https://github.com/SheetJS/sheetjs) for Excel parsing (only external dependency, loaded from CDN)
- Runs 100% client-side
