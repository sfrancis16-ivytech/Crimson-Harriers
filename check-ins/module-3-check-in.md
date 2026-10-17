# Module 3 Check-In

**Submitted:** October 4, 2026, 9:42 PM, by Steven Francis (text submission in IvyLearn)

## What the check-in covered

- **Leader:** Steven Francis. **Team:** Shenease Guy and Andrea Wilson, working team name Crimson Harriers.
- **Tool:** pseudocode.
- **Modules and owners:** Steven owns the system design, Module 0 (main program) and Module 5 (record results). Shenease owns Module 1 (employee information with security checks) and Module 2 (hourly rate lookup). Andrea owns Module 3 (gross pay with overtime) and Module 4 (taxes and net pay).
- **Testing:** each module tested by someone other than its author; testing lead still to be chosen at that point.
- **Collaboration:** pseudocode and plans on GitHub; all team communication in IvyLearn Groups. The tentative plan was in the `before-first-meeting` branch.
- **Attendance:** no team meeting had been held yet.

## Questions asked, and the instructor's answers (Oct 11)

| Question | Answer |
|---|---|
| We collect the number of dependents, but none of the required calculations use it. Should dependents affect the tax, or are they only recorded? | Only recorded |
| For unusual hours such as 80, is it acceptable to have the clerk confirm it, pay it and flag it for review, or should the program reject it? | Either is fine as long as we explain it |

Both answers are now built into the design (`docs/system-design.md`, section 8) and the expected test results (`tests/test-log.md`).
