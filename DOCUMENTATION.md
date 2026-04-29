# Documentation — CMITS

**Contributor Management and Issue Tracking System**
A plain-language guide to every function and section in `main.py`, including the full code for each.

---

## Overview

CMITS is a Python program that runs in the terminal. It asks you questions, collects your answers, organizes the data, and saves everything into files. There are no buttons or screens — just typed responses.

The program is built from **3 helper functions**, **2 data-collection functions**, and **4 sections** of logic that process and save everything.

---

## Shared Variables

Before any functions are defined, two reusable separator lines are created. These are just visual dividers used throughout the program when printing to the terminal — a long line of `=` signs and a long line of `-` signs.

```python
line = 70 * "="
dash = 70 * "-"
```

---

## Helper Functions

These are small, reusable tools built to solve one specific problem: making sure bad data never gets into the system. They are called repeatedly throughout the program whenever user input is needed.

---

### `get_validated_input(prompt, valid_options)`

**What it does:**
Asks the user a question and only accepts one of the pre-approved answers. It strips extra spaces from what the user typed, normalizes the capitalization (so "bug" and "BUG" are both treated as "Bug"), then checks if the answer is in the allowed list. If it isn't, it shows an error and loops back to ask again — it never moves forward with a wrong answer.

**Why it was created:**
Some fields only make sense with specific values. For example, an issue's priority can only be "Critical", "High", "Medium", or "Low" — anything else would break the system's analysis. This function enforces that rule in one place so it doesn't have to be rewritten every time.

**Used for:** Issue type (Bug/Feature), priority level (Critical/High/Medium/Low), and issue status (Open/In Progress/Resolved).

---

### `get_validated_alpha(prompt, field_name)`

**What it does:**
Asks the user for a text answer — like a name, role, or country — and rejects anything that contains numbers or special characters. It also rejects blank answers. Spaces are allowed (so "New Zealand" or "Team Lead" work fine). If the input passes, it's returned with proper capitalization (e.g., "jane doe" becomes "Jane Doe").

**Why it was created:**
Names and roles should only contain letters. Allowing numbers or symbols in a contributor's name field (e.g., "Dev123" or "!Admin") would produce messy, unprofessional reports. This function keeps all text fields clean automatically.

**Used for:** Contributor name, role, language, country, and issue title and reporter.

---

### `get_validated_int(prompt)`

**What it does:**
Asks the user to enter a number and keeps asking until they provide a valid whole number that is zero or greater. It uses Python's built-in `.isdigit()` check, which naturally rejects decimals, negative numbers, and any text. Once a valid number is entered, it converts it from text to an actual integer and returns it.

**Why it was created:**
The number of commits a contributor has made must be a real, countable number — not "many" or "-5" or "3.5". This function ensures the commits field always contains a usable integer that the program can work with.

**Used for:** Number of commits per contributor.

---

## Section 1 — Launch Your Project

### Project Data (Tuple)

The project's fixed details are stored in a tuple — an ordered, unchangeable list. Tuples are used here because this information (name, version, year, language, lead) should never be modified once set. The `contributors` list starts empty and gets filled as users are registered.

---

### `get_contributor()`

**What it does:**
Collects all the information needed for one contributor by asking five questions in order: name, role, programming language, number of commits, and country. Each answer is validated using the helper functions. Once all five answers pass validation, they are packaged together into a dictionary (a labeled collection of data) and returned. The `while` loop calls this function repeatedly until exactly 4 contributors are registered.

**Why it was created:**
Instead of writing the same five questions out four separate times, this function groups them into one reusable block. It also handles the case where the user interrupts the program midway (pressing Ctrl+C), so it exits cleanly instead of crashing.

**Fields collected:**

| Field | What it means |
|-------|--------------|
| `name` | The contributor's full name |
| `role` | Their job on the project (e.g., Developer, Designer) |
| `language` | The programming language they use |
| `commits` | How many times they've submitted code |
| `country` | Where they are from |

---

### Banner and List Operations

After all contributors are registered, the program prints a welcome banner using the project tuple, then performs several list operations on the contributor data:

These steps demonstrate sorting, dictionary updating with `update()`, and safe copying with `copy()` — all standard tools for managing lists of records.

---

## Section 2 — Track and Analyse Issues

### `get_issue()`

**What it does:**
Collects all the information needed to log one issue by asking six questions: the issue ID, title, type, priority, reporter, and status. The free-text fields (title, reporter) go through `get_validated_alpha()`, and the choice fields (type, priority, status) go through `get_validated_input()`. Once all six answers pass, they are packaged into a dictionary and returned. The `while` loop calls this five times to collect five issues.

**Why it was created:**
For the same reason as `get_contributor()` — to avoid repetition and keep the input process consistent and safe. Grouping all six questions into one function also makes the code easier to read and maintain.

**Fields collected:**

| Field | What it means |
|-------|--------------|
| `id` | A unique identifier for the issue (e.g., ISS-001) |
| `title` | A short description of the problem or request |
| `type` | Whether it's a Bug (broken) or Feature (new addition) |
| `priority` | How urgent it is — Critical, High, Medium, or Low |
| `reporter` | Who flagged this issue |
| `status` | Whether it's Open, In Progress, or Resolved |

---

### Analysis Operations

After all issues are collected, the program runs analysis automatically — no user input needed.

**What each part does:**
- **Open count** — loops through all issues and tallies how many are still unresolved
- **Priority escalation** — directly updates the first issue's priority to show how records can be changed by index
- **Reporters set** — collects all unique reporter names; sets automatically remove duplicates
- **Tech stack set** — collects all unique languages from contributors, then demonstrates `add()` and `discard()` to manually adjust the set
- **Set operations** — `union` combines both sets, `intersection` finds what they share, `difference` finds what's only in reporters
- **Status groups** — builds a dictionary where each key is a status and its value is a list of issue titles under that status
- **Top reporter** — manually loops through reporter counts to find who submitted the most, without relying on Python's `max()` shortcut

---

## Section 3 — Save, Read and Report

### Create Project Folder

**What it does:** Takes the project name from the tuple, converts it to lowercase, and creates a folder with that name (`cmits/`). It checks first whether the folder already exists so it never accidentally overwrites an existing one.

---

### Save `project_report.txt`

**What it does:** Opens a new text file and writes the entire project summary into it — project info, all contributors, all issues, and the analysis results. It's wrapped in a `try/except` block so that if something goes wrong during saving (e.g., the disk is full), the program prints a helpful error instead of crashing.

---

### Save `issues.csv`

**What it does:** Creates a spreadsheet-compatible `.csv` file. The first row is the header (column names), and each issue becomes one data row beneath it. This file can be opened directly in Excel or Google Sheets without any extra steps.

---

### Reading Files Back

**What it does:** After saving, the program reads the report back in three different ways to confirm the file was written correctly. `read()` loads the entire file at once. `readline()` reads one line at a time — here it's called twice to get the first two lines. `readlines()` loads every line into a list, then the program loops through and prints only the lines that mention "Critical" or "High".

---

## Bonus Section — Urgent Issue Detection

### Part 1: List Comprehension Filter

**What it does:** Uses a single line called a list comprehension to scan all five issues and collect only the titles of those marked Critical or High. It reads almost like plain English: *"give me the title of every issue where the priority is Critical or High."*

**Why this approach:** It's a compact and efficient way to filter data without writing a full multi-line loop. The result is a clean list of urgent issue titles ready to be used immediately.

---

### Part 2: Append Urgent Issues to Report

**What it does:** Opens the existing report file in append mode (`"a"`) — this means it adds to the bottom of the file without erasing anything already saved. It writes a new "URGENT ISSUES" section listing every critical or high-priority issue title. Then it reads the last 6 lines of the file and prints them to confirm the addition worked correctly.

---

## Input Rules Summary

| Field | Rule |
|-------|------|
| Names, roles, countries, titles, reporters | Letters and spaces only — no numbers or symbols |
| Commits | Whole numbers only, zero or greater |
| Issue type | Must be exactly "Bug" or "Feature" |
| Priority | Must be "Critical", "High", "Medium", or "Low" |
| Status | Must be "Open", "In Progress", or "Resolved" |

If any rule is broken, the program asks again. It does not crash.

---

## Output Files

| File | Location | Contents |
|------|----------|----------|
| `project_report.txt` | `cmits/` folder | Full report: project info, contributors, issues, analysis, urgent flags |
| `issues.csv` | `cmits/` folder | Spreadsheet of all 5 issues — ready to open in Excel |
