# Module 0 — Main Program

**Owner:** Steven Francis · **Status:** Draft 1 (Oct 3, 2026) · **Style:** textbook pseudocode (*Programming Logic and Design*)

## What this module does

Main is the controller. It does none of the payroll math itself. It:

1. Declares every shared variable and constant (see `system-design.md`).
2. Checks the access code before anything else happens.
3. Shows a menu and makes sure the choice is valid.
4. Repeats the payroll steps for each of the 10 employees by calling the other modules in order.
5. Ends the program cleanly.

## Security checks in this module

| Check | Where | What happens on bad input |
|---|---|---|
| Access code, 3 tries maximum | `loginCheck()` | After the third wrong code the program says "Access denied" and ends. The menu never appears. |
| Menu choice must be 1, 2 or 3 | `getMenuChoice()` | Anything else (0, 5, letters, blank) gets "Invalid choice" and the program asks again. |
| Skipped employees are never calculated | `runPayroll()` | If entry or rate lookup marks an employee `SKIPPED`, the pay modules are not called for that employee. |
| No leftover data between employees | `resetEmployeeValues()` | Every value is cleared before each employee, so one person's pay can never show up on another person's row. |

## Pseudocode

```
start
   Declarations
      // ---------- Constants ----------
      num    NUM_EMPLOYEES  = 10
      num    MAX_ATTEMPTS   = 3
      string ACCESS_CODE    = "PAY2026"     // in a real system this would be stored securely, not in the program
      string RUN_CHOICE     = "1"
      string REPORT_CHOICE  = "2"
      string EXIT_CHOICE    = "3"
      string RESULTS_FILE   = "PayrollResults.csv"
      num    REG_HOURS      = 40            // used by Andrea
      num    OT_RATE        = 1.5           // used by Andrea
      num    STATE_TAX_RATE = 0.056         // used by Andrea
      num    FED_TAX_RATE   = 0.079         // used by Andrea
      num    MAX_HOURS      = 60            // used by Shenease (hours above this get flagged)

      // ---------- Main program variables (Steven) ----------
      string loggedIn      = "N"
      string codeEntered
      num    attemptCount  = 0
      string menuChoice
      num    employeeCount = 0
      string payrollHasRun = "N"

      // ---------- Employee entry and rate lookup (Shenease) ----------
      string empFirstName
      string empLastName
      string empID
      num    numDependents
      num    hoursWorked
      num    hourlyRate
      string entryStatus                    // "OK", "FLAGGED", "SKIPPED" or "ERROR"

      // ---------- Pay calculations (Andrea) ----------
      num regularPay
      num overtimePay
      num grossPay                          // the pre-tax amount
      num stateTax
      num fedTax
      num netPay                            // the post-tax amount

      // ---------- Results and totals (Steven) ----------
      OutputFile payrollSheet
      num processedCount
      num flaggedCount
      num skippedCount
      num errorCount
      num totalGrossPay
      num totalStateTax
      num totalFedTax
      num totalNetPay

   housekeeping()
   if loggedIn = "Y" then
      getMenuChoice()
      while menuChoice <> EXIT_CHOICE
         if menuChoice = RUN_CHOICE then
            runPayroll()
         else
            displaySummary()
         endif
         getMenuChoice()
      endwhile
   endif
   endOfJob()
stop


housekeeping()
   output "PAYROLL PROGRAM"
   loginCheck()
return


loginCheck()
   attemptCount = 0
   while loggedIn = "N" AND attemptCount < MAX_ATTEMPTS
      output "Enter the payroll access code: "
      input codeEntered
      attemptCount = attemptCount + 1
      if codeEntered = ACCESS_CODE then
         loggedIn = "Y"
      else
         output "Incorrect code. Attempts left: ", MAX_ATTEMPTS - attemptCount
      endif
   endwhile
   if loggedIn = "N" then
      output "Access denied. Too many incorrect attempts."
   endif
return


getMenuChoice()
   output "PAYROLL MENU"
   output "1 - Run payroll for ", NUM_EMPLOYEES, " employees"
   output "2 - View payroll summary"
   output "3 - Exit"
   output "Enter your choice (1-3): "
   input menuChoice
   while menuChoice <> RUN_CHOICE AND menuChoice <> REPORT_CHOICE AND menuChoice <> EXIT_CHOICE
      output "Invalid choice. Please enter 1, 2 or 3: "
      input menuChoice
   endwhile
return


runPayroll()
   startResults()                           // Module 5 (Steven)
   employeeCount = 0
   while employeeCount < NUM_EMPLOYEES
      employeeCount = employeeCount + 1
      resetEmployeeValues()
      output "Employee ", employeeCount, " of ", NUM_EMPLOYEES
      getEmployeeInfo()                     // Module 1 (Shenease)
      if entryStatus <> "SKIPPED" then
         getHourlyRate()                    // Module 2 (Shenease)
      endif
      if entryStatus <> "SKIPPED" then
         calculateGrossPay()                // Module 3 (Andrea)
         calculateTaxes()                   // Module 4 (Andrea)
      endif
      recordResults()                       // Module 5 (Steven)
   endwhile
   finishResults()                          // Module 5 (Steven)
return


resetEmployeeValues()
   empFirstName  = ""
   empLastName   = ""
   empID         = ""
   numDependents = 0
   hoursWorked   = 0
   hourlyRate    = 0
   regularPay    = 0
   overtimePay   = 0
   grossPay      = 0
   stateTax      = 0
   fedTax        = 0
   netPay        = 0
   entryStatus   = "OK"
return


endOfJob()
   output "End of program."
return
```

## Notes on the design

- **Why the menu choice is a string.** The choice is compared to "1", "2" and "3" as text. That way letters or a blank entry are simply "not a match" and get rejected, with no chance of a number-conversion error.
- **Why `getHourlyRate()` sits inside an `if`.** If entry failed, there is no valid ID to look up. The second `if` is separate because the rate lookup can also mark an employee `SKIPPED` (ID not in the database).
- **Running payroll twice.** Choosing 1 again starts a fresh run. `startResults()` sets the totals back to zero and starts a new results file.
- **Later modules.** In Module 8 the rate lookup changes to a real database. Only the inside of `getHourlyRate()` changes. Main stays the same.

## Test scenarios

Tested by a teammate (we each test someone else's module). Tester fills in the last two columns.

| # | Scenario | Input | Expected result | Actual | Pass/Fail |
|---|---|---|---|---|---|
| M0-1 | Correct code, first try | `PAY2026` | Menu appears | | |
| M0-2 | Wrong twice, then correct | `x`, `abc`, `PAY2026` | "Incorrect code" twice (attempts left 2, then 1), then menu appears | | |
| M0-3 | Wrong three times | `a`, `b`, `c` | "Access denied", menu never appears, "End of program." | | |
| M0-4 | Right code, wrong case | `pay2026` | Treated as incorrect | | |
| M0-5 | Invalid menu choices | `0`, `5`, `abc`, blank | "Invalid choice" each time, asks again | | |
| M0-6 | Exit right away | `3` | "End of program.", no payroll run, no results file | | |
| M0-7 | Run payroll | `1` | Counter shows "Employee 1 of 10" through "Employee 10 of 10", then summary, then menu again | | |
| M0-8 | Entry module skips an employee | Employee marked `SKIPPED` in entry | Rate lookup and pay modules are not called; row saved as `SKIPPED`; loop moves on | | |
| M0-9 | ID not in database | Rate lookup marks `SKIPPED` | Pay modules are not called; row saved as `SKIPPED` | | |
| M0-10 | No carry-over | A skipped employee right after a paid one | Skipped row shows zeros, not the previous employee's pay | | |
| M0-11 | Run payroll twice | `1`, then `1` again | Second run starts fresh; totals are not doubled | | |
