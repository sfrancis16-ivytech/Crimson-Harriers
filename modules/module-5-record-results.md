# Module 5 — Record Results and Summary

**Owner:** Steven Francis · **Status:** Draft 1 (Oct 3, 2026) · **Style:** textbook pseudocode (*Programming Logic and Design*)

## What this module does

This module owns everything about the output. Main never touches the results file directly.

| Sub-module | Called | Job |
|---|---|---|
| `startResults()` | Once, before the first employee | Sets all totals to zero, opens the results file, writes the header row |
| `recordResults()` | Once per employee | Checks the pay amounts make sense, writes one row, adds to the totals |
| `finishResults()` | Once, after the last employee | Writes the totals row, closes the file, shows the summary |
| `displaySummary()` | From `finishResults()` and from menu choice 2 | Shows the counts and totals on screen |

The results file is `PayrollResults.csv`. A .csv file opens in Excel or Google Sheets as a spreadsheet, one employee per row.

## Spreadsheet layout

| Column | Header | Comes from |
|---|---|---|
| A | Employee ID | `empID` |
| B | Last Name | `empLastName` |
| C | First Name | `empFirstName` |
| D | Dependents | `numDependents` |
| E | Hours Worked | `hoursWorked` |
| F | Hourly Rate | `hourlyRate` |
| G | Regular Pay | `regularPay` |
| H | Overtime Pay | `overtimePay` |
| I | Gross Pay (pre-tax) | `grossPay` |
| J | State Tax | `stateTax` |
| K | Federal Tax | `fedTax` |
| L | Net Pay (post-tax) | `netPay` |
| M | Status | `entryStatus` |

The last row is a `TOTALS` row with the sums of columns I through L.

## Security checks in this module

| Check | Where | What happens |
|---|---|---|
| Pay amounts must make sense: rate above 0, gross and net not negative, net not more than gross | `recordResults()` | The row is marked `ERROR`, a warning is shown, and the row is left out of the totals |
| Skipped and error rows never count as paid | `recordResults()` | They are saved for the record but not added to any total |
| No summary before a payroll run | `displaySummary()` | Shows "No payroll has been run yet" instead of empty totals |
| Totals always start at zero | `startResults()` | Running payroll a second time cannot double the totals |

## Pseudocode

```
startResults()
   processedCount = 0
   flaggedCount   = 0
   skippedCount   = 0
   errorCount     = 0
   totalGrossPay  = 0
   totalStateTax  = 0
   totalFedTax    = 0
   totalNetPay    = 0
   open payrollSheet RESULTS_FILE
   output "Employee ID", "Last Name", "First Name", "Dependents", "Hours Worked",
          "Hourly Rate", "Regular Pay", "Overtime Pay", "Gross Pay (pre-tax)",
          "State Tax", "Federal Tax", "Net Pay (post-tax)", "Status" to payrollSheet
return


recordResults()
   // Security check: do the calculated amounts make sense?
   if entryStatus <> "SKIPPED" then
      if hourlyRate <= 0 OR grossPay < 0 OR netPay < 0 OR netPay > grossPay then
         entryStatus = "ERROR"
         output "Warning: pay amounts for employee ", empID, " do not make sense. Row marked ERROR."
      endif
   endif

   // Every employee gets a row, whatever the status
   output empID, empLastName, empFirstName, numDependents, hoursWorked,
          hourlyRate, regularPay, overtimePay, grossPay,
          stateTax, fedTax, netPay, entryStatus to payrollSheet

   // Only good rows are added to the totals
   if entryStatus = "SKIPPED" then
      skippedCount = skippedCount + 1
      output "Employee ", employeeCount, " was skipped."
   else
      if entryStatus = "ERROR" then
         errorCount = errorCount + 1
      else
         processedCount = processedCount + 1
         totalGrossPay  = totalGrossPay + grossPay
         totalStateTax  = totalStateTax + stateTax
         totalFedTax    = totalFedTax + fedTax
         totalNetPay    = totalNetPay + netPay
         if entryStatus = "FLAGGED" then
            flaggedCount = flaggedCount + 1
         endif
         output "Saved: ", empFirstName, " ", empLastName, "   Net pay: $", netPay
      endif
   endif
return


finishResults()
   output "TOTALS", "", "", "", "", "", "", "",
          totalGrossPay, totalStateTax, totalFedTax, totalNetPay, "" to payrollSheet
   close payrollSheet
   payrollHasRun = "Y"
   displaySummary()
return


displaySummary()
   if payrollHasRun = "N" then
      output "No payroll has been run yet. Choose option 1 first."
   else
      output "PAYROLL SUMMARY"
      output "Employees paid:            ", processedCount
      output "   of those, flagged:      ", flaggedCount
      output "Employees skipped:         ", skippedCount
      output "Rows with errors:          ", errorCount
      output "Total gross pay (pre-tax): $", totalGrossPay
      output "Total state tax:           $", totalStateTax
      output "Total federal tax:         $", totalFedTax
      output "Total net pay (post-tax):  $", totalNetPay
      output "Results saved to ", RESULTS_FILE
   endif
return
```

## Notes on the design

- **A flagged employee is still paid.** `FLAGGED` means the entry was unusual but confirmed (for example, 80 hours). The row counts in the totals and also in the flagged count, so someone can review it.
- **Zero hours is not an error.** Gross pay of 0 and net pay of 0 pass the check. Whether zero hours gets flagged is decided in the entry module.
- **Reuse.** `displaySummary()` is written once and called from two places: the end of a payroll run and menu choice 2.

## Test scenarios

Tested by a teammate (we each test someone else's module). Tester fills in the last two columns.

| # | Scenario | Input | Expected result | Actual | Pass/Fail |
|---|---|---|---|---|---|
| M5-1 | Summary before any run | Menu `2` first | "No payroll has been run yet. Choose option 1 first." | | |
| M5-2 | Header row | Start a run | First row of the file has the 13 headers in order, A through M | | |
| M5-3 | Normal row | Status `OK`, gross 720.00, net 622.80 | Row written; paid count +1; totals go up by the row's amounts | | |
| M5-4 | Flagged row | Status `FLAGGED`, 80 hours | Row written; counts as paid and as flagged; added to totals | | |
| M5-5 | Skipped row | Status `SKIPPED` | Row written with zeros and status `SKIPPED`; skipped count +1; totals unchanged | | |
| M5-6 | Net more than gross | gross 720.00, net 770.00 | Warning shown; row marked `ERROR`; error count +1; totals unchanged | | |
| M5-7 | Negative pay or rate of 0 | net −50.00, or rate 0 | Row marked `ERROR`; totals unchanged | | |
| M5-8 | Zero hours | hours 0, gross 0.00, net 0.00 | Accepted, not an error | | |
| M5-9 | Totals add up | Three `OK` rows with gross 720.00, 672.00, 950.00 | Total gross 2,342.00; state 131.15; federal 185.02; net 2,025.83 | | |
| M5-10 | End of a full run | 10 employees | Paid + skipped + errors = 10; `TOTALS` row is last; total gross − state − federal = total net | | |
| M5-11 | Summary again | Menu `2` after a run | Same numbers as shown at the end of the run | | |
