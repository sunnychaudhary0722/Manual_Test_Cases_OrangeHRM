# OrangeHRM — Manual QA Test Suite

A self-directed QA portfolio project: manual functional, negative, and exploratory testing of [OrangeHRM](https://opensource-demo.orangehrmlive.com), an open-source Human Resource Management System, performed against its public demo instance.

## Why this project

Built to demonstrate practical manual testing skills on a real, feature-rich web application — test planning, test case design, execution, and defect reporting — end to end.

## Results at a glance

- **25 test cases** executed across Login, Dashboard, PIM, Leave, Admin, and My Info
- **22 Pass / 3 Fail**
- **3 defects logged**, spanning Low and Medium severity

## Contents

| File | Purpose |
|---|---|
| `QA_Manual_Test_Cases_OrangeHRM.xlsx` | 25 documented test cases with steps, test data, expected results, and execution status, plus a Defect Log tab |

## Test Plan

### 1. Objective
Verify the functional correctness of core OrangeHRM workflows — authentication, employee record management (PIM), leave application, and admin user management — through structured manual testing.

### 2. Scope

**In scope:**
- Login — valid/invalid credentials, empty fields, session handling
- Dashboard — widget load, navigation
- PIM — add employee, search employee, edit employee details, delete employee
- Leave — apply for leave, view leave list, cancel a pending leave request
- Admin — add system user, search user, assign user role
- My Info — view and edit logged-in user's own profile

**Out of scope:**
- Performance/load testing
- Security/penetration testing
- Payroll and Recruitment modules
- Mobile responsiveness testing

### 3. Approach
- **Scripted functional testing** against expected system behavior.
- **Negative testing** deliberately paired with every form-based workflow — empty required fields, invalid formats, duplicate data, boundary values — since these are where real defects tend to surface.
- **Exploratory testing** — a time-boxed session per module to catch issues scripted cases miss.
- **Cross-checking** — repeating key actions (e.g. add + delete employee) to confirm state changes persist correctly.

### 4. Environment
- URL: `https://opensource-demo.orangehrmlive.com`
- Credentials: `Admin` / `admin123` (public demo)
- Browser: Latest Chrome (primary), Firefox spot-check
- Note: this is a shared public demo instance that resets periodically — data created during testing may not persist, and that alone isn't a defect.

### 5. Severity Definitions

| Severity | Definition |
|---|---|
| Critical | Blocks core functionality; no workaround (e.g. cannot log in) |
| High | Major feature broken or produces incorrect data; workaround exists |
| Medium | Feature partially works or behaves inconsistently; limited impact |
| Low | Cosmetic, wording, or minor usability issue; no functional impact |

## Findings

**DEF-01 — Password policy not communicated before submission (Low)**
- **Module / TC:** Admin, TC-19
- **Steps:** Navigate to Admin > Add User → fill required fields with a password containing no numeric character → click Save
- **Expected:** The form indicates the password requirement up front, or accepts the entered password
- **Actual:** Save is rejected with an error stating the password must contain at least 1 number — but this requirement isn't shown anywhere on the form before submission
- **Severity:** Low — validation works, but the UX around it doesn't

**DEF-02 — Session does not time out after extended inactivity (Medium)**
- **Module / TC:** Session handling, TC-25
- **Steps:** Log in → leave the session idle for an extended period → attempt an action
- **Expected:** Session expires after a reasonable period of inactivity and requires re-authentication
- **Actual:** Session remained active with no re-authentication prompt, even after prolonged inactivity
- **Severity:** Medium — a session-management gap worth flagging, since indefinite sessions are a security-adjacent concern

**DEF-03 — Employee ID not assigned/displayed after employee creation (Medium)**
- **Module / TC:** PIM, TC-09
- **Steps:** Navigate to PIM > Add Employee → enter First Name and Last Name → click Save
- **Expected:** System auto-generates and displays an Employee ID immediately upon successful creation
- **Actual:** Employee record is created and the user is redirected to the Personal Details tab, but no Employee ID is generated or shown
- **Severity:** Medium — a data-integrity concern, since the ID is presumably referenced elsewhere in the system (leave, payroll, etc.)

## How to review

1. Open `QA_Manual_Test_Cases_OrangeHRM.xlsx`.
2. **Test Cases** tab — all 25 cases with steps, test data, expected results, and Pass/Fail status.
3. **Defect Log** tab — the 3 findings above with full repro steps.
4. **Summary** tab — quick totals by priority and execution result.
