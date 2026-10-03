# Payroll Program — SDEV120 Group Project

**Team:** Red Group 1 (team name to be chosen) · **Course:** SDEV120 Computing Logic, Ivy Tech, Fall 2026

This repository holds our group project: a payroll "program" for 10 employees, written in **pseudocode**. Nothing here is real code that runs. The program is split into modules so each of us can write our part separately and have the parts fit together.

This repository is private. Please keep it within the team and our instructor.

## Team

| Member | Role | Modules |
|---|---|---|
| Steven Francis | Group leader | System design, Module 0 (main program), Module 5 (record results) |
| Shenease Guy | Team member | Module 1 (employee information), Module 2 (rate lookup) |
| Andrea Wilson | Team member | Module 3 (gross pay), Module 4 (taxes and net pay) |

Testing lead: to be decided. Each module is tested by someone other than the person who wrote it.

## What the program does

1. The payroll clerk enters an access code.
2. A menu appears: 1 Run payroll, 2 View summary, 3 Exit.
3. For each of the 10 employees, the program:
   1. takes the employee's name, ID, dependents and hours, and checks each entry
   2. looks up the hourly rate using the employee ID
   3. calculates gross pay, including overtime
   4. calculates state tax, federal tax and net pay
   5. saves the results as a row in a spreadsheet
4. A summary shows the totals.

![Hierarchy chart of the payroll program](docs/hierarchy-chart.png)

A printable copy of the chart is in [docs/hierarchy-chart.pdf](docs/hierarchy-chart.pdf).

## Files

Files marked "Not started" don't exist yet. Their links show GitHub's "not found" page until the owner creates the file.

### `docs/` — design and plan

| File | What it is | Owner | Status |
|---|---|---|---|
| [system-design.md](docs/system-design.md) | **Start here.** Shared variable list and what each module must do | Steven | Draft 1 |
| [project-plan.md](docs/project-plan.md) | Development plan: scope, owners, schedule, security and testing | Steven | Draft 1 |
| [hierarchy-chart.png](docs/hierarchy-chart.png) | Chart of all modules and who owns them | Steven | Draft 1 |
| [hierarchy-chart.pdf](docs/hierarchy-chart.pdf) | Printable copy of the chart | Steven | Draft 1 |

### `modules/` — the pseudocode

| File | What it is | Owner | Status |
|---|---|---|---|
| [module-0-main.md](modules/module-0-main.md) | Access code, menu, the 10-employee loop | Steven | Draft 1 |
| [module-1-employee-info.md](modules/module-1-employee-info.md) | Entry of name, ID, dependents and hours, with security checks | Shenease | Not started |
| [module-2-rate-lookup.md](modules/module-2-rate-lookup.md) | Hourly rate lookup by employee ID | Shenease | Not started |
| [module-3-gross-pay.md](modules/module-3-gross-pay.md) | Regular pay and overtime | Andrea | Not started |
| [module-4-taxes-net-pay.md](modules/module-4-taxes-net-pay.md) | State tax, federal tax, net pay | Andrea | Not started |
| [module-5-record-results.md](modules/module-5-record-results.md) | Spreadsheet rows, totals, summary | Steven | Draft 1 |

### `tests/` — test cases and results

| File | What it is | Owner | Status |
|---|---|---|---|
| [test-log.md](tests/test-log.md) | Test cases for the whole program, with expected and actual results | Testing lead | Not started |
| [test-data.csv](tests/test-data.csv) | The 10 made-up employees used for testing | Testing lead | Not started |

### `data/` — pay rates and results

| File | What it is | Owner | Status |
|---|---|---|---|
| [pay-rates.csv](data/pay-rates.csv) | Hourly rate for each employee ID. Becomes the database in Module 8 | Shenease | Not started |
| [payroll-results-sample.csv](data/payroll-results-sample.csv) | Sample of the results spreadsheet | Steven | Not started |

### `check-ins/` — meetings and submissions

| File | What it is | Owner | Status |
|---|---|---|---|
| [meeting-log.md](check-ins/meeting-log.md) | Attendance and decisions for each meeting | Steven | Started |
| [module-3-check-in.md](check-ins/module-3-check-in.md) | What we submitted for the Module 3 check-in | Steven | Not started |

## How to add your module

You can do everything in the browser. Nothing needs to be installed.

1. Accept the invitation email from GitHub, sign in, and open this repository.
2. Read [system-design.md](docs/system-design.md). Section 6 says what your module must use and set.
3. Click **Add file**, then **Create new file**.
4. Type the folder and the file name together, exactly as shown in the Files tables, for example `modules/module-1-employee-info.md`. Typing `modules/` first puts the file in the modules folder.
5. Type or paste your pseudocode. Put a line of three backticks above and below it so the spacing is kept:

   ````
   ```
   getEmployeeInfo()
      output "Enter the employee's first name: "
      input empFirstName
   return
   ```
   ````

6. Click **Commit changes**, write a short note about what you did, and confirm.

To change a file later, open it, click the pencil icon, make your edit, and commit again.

Would you rather not use GitHub? Send your pseudocode in our IvyLearn Inbox thread and Steven will add it here with your name in the note.

## Ground rules

- Use the shared variable names in [system-design.md](docs/system-design.md) exactly as spelled. All shared variables are declared once, in the main program, so do not declare them again in your module.
- If you need a new shared variable, tell Steven so it gets added to the list and to main.
- Keep the module names that main calls: `getEmployeeInfo()`, `getHourlyRate()`, `calculateGrossPay()`, `calculateTaxes()`.
- Write in the textbook's style: `input`, `output`, `if … then … else … endif`, `while … endwhile`, and `return` at the end of each module.
- Edit only your own files. If you spot a problem in someone else's, tell them.
- Every module needs its security checks and a table of test scenarios.

## Project requirements

- [ ] Entry of first name, last name, ID, number of dependents and hours worked (Module 1)
- [ ] Hourly rate pulled from a database, with the employee ID as the primary key (Module 2)
- [ ] Gross pay: hourly rate × hours up to 40, plus overtime at 1.5× for hours over 40 (Module 3)
- [ ] State tax at 5.6% and federal tax at 7.9%, both on the pre-tax amount (Module 4)
- [ ] Pre-tax and post-tax amounts (Modules 3 and 4)
- [ ] Results recorded in a spreadsheet (Module 5)
- [ ] All variables defined (system design)
- [ ] At least 3 modules
- [ ] Every team member writes some of the pseudocode
- [ ] Proof that security checks were applied
- [ ] At least 10 test cases, with results

## Key dates

| Date | What |
|---|---|
| Sun, Oct 4, 2026, 11:59 PM | Module 3 check-in: development plan, modules and owners |
| Later modules | More check-ins (dates in IvyLearn) |
| Module 8 | Rate lookup changes to a database lookup |
| End of semester | Final project: pseudocode plus test results, submitted by the group leader |

The full schedule is in [project-plan.md](docs/project-plan.md).

## Communication

- **IvyLearn group (red group 1):** the "Let's Get Started" discussion and our Inbox thread. Decisions and meeting notes are posted there so our instructor can see them.
- **This repository:** the pseudocode itself.
- **Meetings:** attendance and decisions are recorded in [check-ins/meeting-log.md](check-ins/meeting-log.md).
