# SleepConverter
Convert SleepOn 3 Data Format to Wellue O2Ring/SleepHQ O2 Ring Pro Format

# Sleep Ring Converter 💤🔁📈

**Transform SleepON Ring Data into SleepHQ-Compatible CSVs — in One Click, All Offline.**

This tool allows users to instantly convert SleepON 3 Ring CSV or XLSX files into a format compatible with SleepHQ uploads. The app runs 100% in-browser (client-side), preserving privacy and ensuring smooth, fast processing — no account, upload, or backend needed.

---

## 🚀 Features

- **📂 Upload Options:**
  - Drag-and-drop interface
  - Manual file picker
  - Sample data available for testing

- **🔁 Conversion Logic:**
  - Converts time to `DD/MM/YYYY HH:MM` (24-hour format, no seconds)
  - Cleans and renames columns (e.g. SpO2, PR, Motion)
  - Filters invalid rows and removes duplicates
  - Ensures SleepHQ compatibility with default filler values

- **📊 Preview + Stats:**
  - Beautifully styled preview of top 5 converted rows
  - Summary stats: Min/Max/Average SpO2 and Pulse, row counts, duplicates

- **💾 Download:**
  - Auto-generates a downloadable file in `YYYYMMDDHHMMSS.csv` format
  - Fully compliant with SleepHQ upload structure

---

## 🧰 Technology

- **Frontend:** Vanilla HTML, CSS, JavaScript
- **Libraries:**
  - [SheetJS (xlsx)](https://github.com/SheetJS/sheetjs) for Excel parsing
  - Font Awesome + IBM Plex fonts for UI styling
- **No dependencies or servers beyond browser CDN links**

---

## 🛠 How It Works

1. **Upload your SleepON CSV/XLSX file** via drag-drop or button
2. **Click "Convert"** — the app detects headers like `Time`, `SpO2`, `PR`, and `Motion`
3. **Preview** the converted results in a table and stats panel
4. **Download** the new SleepHQ-ready CSV file instantly

---

## 📁 File Format

**Input (SleepON Ring):**
- `.csv` or `.xlsx`
- Must include some variation of:
  - `Time`, `SpO2` or `Oxygen`, `Pulse` or `PR`, optionally `Motion`

**Output (SleepHQ-Compatible):**
```csv
Time,Oxygen Level,Pulse Rate,Motion,O2 Reminder,PR Reminder
28/03/2025 23:52,98,53,8,0,0
...
