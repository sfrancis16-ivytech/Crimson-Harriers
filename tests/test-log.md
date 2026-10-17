# Test Log

**Testing lead:** Steven Francis (no one else volunteered, so the role defaulted to the leader as planned) · **Started:** October 17, 2026

This log holds every test for the payroll program, the result we expect, and the result we actually got.

## How we test pseudocode

Pseudocode can't be run on a computer, so each test is a **desk check**. The tester walks through the pseudocode by hand with the test input, writes down what each variable holds, and compares the final output with the expected result. A test passes when every value matches to the cent and the status (OK, FLAGGED, SKIPPED or ERROR) matches.

Two rules from our instructor's check-in feedback (Oct 11) are built into the expected results:

- **Dependents are only recorded.** They are saved in the spreadsheet but do not change any tax.
- **Unusual hours are confirmed and flagged, not rejected.** Hours above 60 are paid once the clerk confirms them, and the row is marked FLAGGED for review.

## 1. Integration test: full run of 10 employees

The input for each employee is in the table below. The expected spreadsheet is [`../data/payroll-results-sample.csv`](../data/payroll-results-sample.csv). The employees are made up, and the expected results were calculated in advance from the project's pay and tax rules.

Taxes are 5.6% (state) and 7.9% (federal) of gross pay, each rounded to the cent. Net pay = gross − state − federal.

| # | Scenario | Input | Expected status | Gross | State | Federal | Net | Actual | Pass/Fail |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Standard week | 1001, 40 hrs, $18.00 | OK | $720.00 | $40.32 | $56.88 | $622.80 | | |
| 2 | Part-time | 1002, 32 hrs, $21.00 | OK | $672.00 | $37.63 | $53.09 | $581.28 | | |
| 3 | Overtime | 1003, 45 hrs, $20.00 | OK | $950.00 | $53.20 | $75.05 | $821.75 | | |
| 4 | Just past 40 hours | 1004, 40.5 hrs, $25.00 | OK | $1,018.75 | $57.05 | $80.48 | $881.22 | | |
| 5 | Heavy overtime, at the 60-hour limit | 1005, 60 hrs, $16.00 | OK | $1,120.00 | $62.72 | $88.48 | $968.80 | | |
| 6 | Zero hours | 1006, 0 hrs, $30.00 | OK | $0.00 | $0.00 | $0.00 | $0.00 | | |
| 7 | Unusual hours, confirmed | 1007, 80 hrs, $22.00 | FLAGGED | $2,200.00 | $123.20 | $173.80 | $1,903.00 | | |
| 8 | Negative hours | 1008, −5 (rejected), then 38 hrs, $19.50 | OK | $741.00 | $41.50 | $58.54 | $640.96 | | |
| 9 | ID not in the rate table | 9999, 40 hrs | SKIPPED | $0.00 | $0.00 | $0.00 | $0.00 | | |
| 10 | Letters in the hours field | 1010, "forty" (rejected), then 40 hrs, $17.25 | OK | $690.00 | $38.64 | $54.51 | $596.85 | | |

**Expected summary after the run:** 9 employees paid (1 of them flagged), 1 skipped, 0 errors. Total gross $8,111.75, state tax $454.26, federal tax $640.83, net pay $7,016.66.

**Can run when:** Modules 1–4 have a draft in the repository. Until then, Modules 0 and 5 can only be tested on their own (section 2).

## 2. Unit tests: each module on its own

Each module file ends with its own table of test scenarios. Each module is tested by someone other than its author.

| Module | Scenarios | Author | Tester | Status |
|---|---|---|---|---|
| [Module 0 — Main program](../modules/module-0-main.md) | 11 (M0-1 to M0-11) | Steven | Shenease | Ready to test |
| Module 1 — Employee information | Not written yet | Shenease | Steven | Waiting for draft |
| Module 2 — Rate lookup | Not written yet | Shenease | Steven | Waiting for draft |
| Module 3 — Gross pay | Not written yet | Andrea | To be decided | Waiting for draft |
| Module 4 — Taxes and net pay | Not written yet | Andrea | To be decided | Waiting for draft |
| [Module 5 — Record results](../modules/module-5-record-results.md) | 11 (M5-1 to M5-11) | Steven | Shenease | Ready to test |

## 3. Security tests

These tests prove the security checks work. Each one is also listed in a table above.

| Check | Tests |
|---|---|
| Access code, 3 tries | M0-1, M0-2, M0-3, M0-4 |
| Menu choice must be 1, 2 or 3 | M0-5 |
| Bad hours rejected (negative, letters) | Integration 8, 10 |
| Unusual hours confirmed and flagged | Integration 7 |
| Employee ID must be on file | Integration 9, M0-9 |
| Pay amounts sanity-checked before saving | M5-6, M5-7 |
| No values carry over between employees | M0-10 |

## 4. Run log

Add one row each time a test is run. Newest at the top.

| Date | Test(s) | Tester | Result | Notes |
|---|---|---|---|---|
| | | | | |
