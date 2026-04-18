# Contributor Management and Issue Tracking System (CMITS)

A Python-based command-line application for managing project contributors and tracking issues — built as a course project for Introduction to Open Source Software.

---

## About

CMITS is a simple, interactive system that lets you register contributors, log issues, and generate project reports — all from the terminal. It demonstrates core Python concepts including data structures, file I/O, and input validation.

---

## What It Does

- Register up to **4 contributors** with their name, role, language, commits, and country
- Log up to **5 issues** with type, priority, reporter, and status
- Automatically generates a **project report** (`.txt`) and an **issues export** (`.csv`)
- Displays a **final summary** and highlights urgent (Critical/High) issues

---

## Requirements

- Python 3.x
- No external libraries required — uses only Python built-ins (`os`, `csv`)

---

## How to Use

**1. Clone the repository**
```bash
git clone https://github.com/your-username/OSS-week-6.git
cd OSS-week-6
```

**2. Run the program**
```bash
python main.py
```

**3. Follow the prompts**

The program will guide you step by step:
- Enter contributor details (4 contributors)
- Enter issue details (5 issues)
- Output files will be saved automatically in a `cmits/` folder

---

## Output Files

| File | Description |
|------|-------------|
| `cmits/project_report.txt` | Full project summary including contributors, issues, and analysis |
| `cmits/issues.csv` | Exportable spreadsheet of all logged issues |

---

## Project Structure

```
OSS-week-6/
├── main.py           # Main application file
├── README.md         # You are here
├── CONTRIBUTING.md   # How to contribute
├── CODE_OF_CONDUCT.md
└── LICENSE
```

---

## Author

**Marie Criz Zaragoza** — Student ID: 26040022  
Introduction to Open Source Software
