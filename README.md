# Assessment Test: Software Tester (QA)

## Candidate Details

- **Full Name:** Surya Pratap Singh
- **Email ID:** surya.pratap.cs.2023@miet.ac.in
- **Contact Number:** 9389533233

---

# 1. Test Case Design

## Application Description

The application is a Task Management Application. It provides User Registration, User Login, and Create/View/Edit/Delete for tasks. Tasks are stored in a database and shown as a list.

## Testing Assumption

*The application is close to release and there's no existing test documentation, and no build was available to try things out on directly. A few details aren't mentioned in the requirements (password rules, field limits, whether there's a confirm dialog, session timeout, etc). Where a test case depends on one of these, I've noted it as something to confirm rather than assumed it as fixed behaviour.*

---

# 2. Registration Test Cases

## 2.1 Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| REG-001 | Register with valid details | Enter a valid name, a new email, and a password. Submit the form. | Should register successfully and show some confirmation (exact message/redirect to be confirmed). |
| REG-002 | Login right after registering | Register, then try logging in with the same email/password. | Should be able to log in with the details just used to register. |
| REG-003 | Register with minimum valid input | Use the shortest name/password that should still be acceptable. | Should register successfully if it meets whatever the minimum rule is. |

## 2.2 Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| REG-004 | Invalid email format | Enter an email without "@", e.g. `riya.sharma.com`. | Should show a validation error and not create the account. |
| REG-005 | Missing required field | Leave the email (or name) field empty and submit. | Should show a "required" type error; form should not submit. |
| REG-006 | Duplicate email | Try registering again with an email that's already used. | Should show an error that the email is already registered. |

## 2.3 Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| REG-007 | Very long input in name/email | Enter an unusually long string in the name or email field. | Should not crash. (Exact max length isn't specified needs to be confirmed.) |
| REG-008 | Password / confirm password mismatch | Enter different values in Password and Confirm Password, if that field exists. | Should show a mismatch error, assuming a confirm-password field is part of the form. |
| REG-009 | Special characters in name | Enter something like `<script>alert(1)</script>` or symbols in the name field. | Should not break the page or run any script should just be treated as plain text. |

---

# 3. Login Test Cases

## 3.1 Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| LOGIN-001 | Login with valid details | Enter a correct, registered email and password. | Should log in and take the user to the task list/dashboard. |
| LOGIN-002 | Stay logged in while browsing | Log in, then move between task list and other pages. | Should not get logged out unexpectedly. |

## 3.2 Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| LOGIN-003 | Wrong password | Enter a correct email with an incorrect password. | Should show an error and not log the user in. |
| LOGIN-004 | Unregistered email | Enter an email that was never registered. | Should show an error (ideally without confirming whether the email exists). |
| LOGIN-005 | Empty fields | Leave email and password blank and submit. | Should show required-field errors. |

## 3.3 Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| LOGIN-006 | Repeated failed logins | Enter the wrong password several times in a row. | Not sure if there's a lockout/CAPTCHA after repeated attempts this needs to be checked, since the requirements don't mention it. |
| LOGIN-007 | Different letter case in email | Register with mixed-case email, then try logging in using lowercase. | Behaviour should be consistent either way to be confirmed. |

---

# 4. Task CRUD Test Cases

## 4.1 Create Task

### Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| CREATE-001 | Create task with valid data | Enter a title and description, then save. | Task should be saved and show up in the list. |
| CREATE-002 | Create task with only a title | Enter just a title, leave other fields blank, save. | Should still save if title is the only required field (needs to be confirmed). |

### Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| CREATE-003 | Empty title | Leave the title blank and try to save. | Should show an error and not save the task. |
| CREATE-004 | Very long title | Enter an unusually long string as the title. | Should not crash; exact limit isn't specified, so this needs checking. |

### Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| CREATE-005 | Special characters/emoji in title | Enter symbols or an emoji in the task title. | Should save and display without breaking the page. |
| CREATE-006 | Double-click Save | Click Save twice quickly while creating a task. | Should not create two copies of the same task. |

---

## 4.2 View Task List

### Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| VIEW-001 | View task list | Log in as a user who has tasks and open the list. | Should show that user's tasks correctly. |
| VIEW-002 | List reflects changes | Add or edit a task, then check the list. | New/updated task should appear without needing a manual refresh. |

### Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| VIEW-003 | Access another user's task | Try opening a task that belongs to a different user (e.g. by changing an ID in the link). | Should not be allowed this is worth checking carefully since it's not stated how task access is restricted. |
| VIEW-004 | View list after session ends | Let the session expire (or log out), then try to open the task list. | Should ask the user to log in again rather than showing old data. |

### Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| VIEW-005 | No tasks yet | Log in as a new user with no tasks and open the list. | Should show some kind of "no tasks" message instead of a blank/broken page. |
| VIEW-006 | Large number of tasks | Open the list for a user with a lot of saved tasks. | Should still load in reasonable time (performance not specified, worth a quick check). |

---

## 4.3 Edit Task

### Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| EDIT-001 | Edit task with valid data | Change the title of an existing task and save. | Updated title should be saved and shown in the list. |
| EDIT-002 | Edit one field only | Update only the description, leave the rest as is, save. | Only that field should change; the rest should stay the same. |

### Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| EDIT-003 | Edit with invalid data | Clear the title field and try to save. | Should show an error and not save the change. |
| EDIT-004 | Edit another user's task | Try editing a task that belongs to another user (e.g. by changing the ID). | Should not be allowed. |

### Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| EDIT-005 | Edit an already-deleted task | Delete a task in one tab, then try editing it from another tab, and save. | Should show a proper error (like "task not found") instead of crashing. |
| EDIT-006 | Cancel an edit | Change some fields, then click Cancel instead of Save. | Changes should be discarded. |

---

## 4.4 Delete Task

### Positive

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| DELETE-001 | Delete a task | Click delete on a task and confirm. | Task should be removed from the list. |
| DELETE-002 | Cancel a delete | Click delete, then choose Cancel if a confirmation shows up. | Task should remain if there's a confirmation step and it's cancelled. |

### Negative

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| DELETE-003 | Delete another user's task | Try deleting a task that belongs to another user (e.g. by changing the ID). | Should not be allowed. |
| DELETE-004 | Delete without confirmation | Try sending a delete action directly without going through the normal confirmation step (if one exists). | Should still check permissions before deleting anything. |

### Edge Cases

| ID | Scenario | Steps / Input | Expected Result |
|---|---|---|---|
| DELETE-005 | Delete the same task twice quickly | Click delete twice fast, or try deleting from two tabs at once. | Second attempt should fail gracefully, not throw an error to the user. |
| DELETE-006 | Delete the last task | Delete the only task remaining in the list. | List should then show the empty state correctly. |

---

# 5. Input Validation & Error Handling

| ID | Area | Scenario | Input / Action | Expected Result |
|---|---|---|---|---|
| VAL-001 | Registration | Script tag in name field | `<script>alert('x')</script>` | Should be handled safely; no script should run. |
| VAL-002 | Registration | SQL-like text in any field | `'; DROP TABLE users;--` | Should not cause a database error or break the form. |
| VAL-003 | Login | SQL-like text in login fields | `' OR '1'='1` | Login should still fail normally; no bypass. |
| VAL-004 | Login | Extra spaces around the email | `" user@test.com "` | Should probably be trimmed before checking against the stored email (to confirm). |
| VAL-005 | Create Task | Extra spaces around the title | `" Prepare report "` | Should be trimmed before saving. |
| VAL-006 | Edit Task | Non-English text/emoji in title | Title with Hindi text and an emoji | Should display correctly, not as broken characters. |
| VAL-007 | Delete Task | Network drops mid-delete | Turn off network right after clicking delete | Should show a clear error rather than leaving things in an unclear state. |
| VAL-008 | General | Check wording of error messages across forms | Trigger any validation error | Messages should be specific to the field, not a generic "error occurred". |

---

# 6. Potential Bugs / Risk Areas

*Identified by reading through the requirements, without running the actual application.*

| # | Bug / Risk Description | Severity | Reason / Impact |
|---|---|---|---|
| 1 | No mention of password rules for registration weak passwords may be accepted. | Major | Makes accounts easier to guess or break into. |
| 2 | Email check might be case-sensitive, so `Test@mail.com` and `test@mail.com` could both register. | Major | Could create duplicate accounts and confuse users. |
| 3 | No mention of a limit on repeated wrong login attempts. | Critical | Someone could keep guessing a password with no restriction. |
| 4 | Task IDs might be visible in the edit/delete links and possibly guessable. | Critical | Could let a user reach another user's task if IDs aren't checked properly. |
| 5 | Not clear if delete always asks for confirmation, or if that can be skipped. | Major | Risk of accidentally deleting a task with no way to undo it. |
| 6 | Not clear if validation is also checked on the server, not just on the screen. | Critical | If only the screen checks input, bad data could still get saved by going around the UI. |
| 7 | Not clear how long a login session lasts or what happens on logout. | Major | Sessions that don't expire are risky on shared computers. |
| 8 | No mention of how the task list behaves with a large number of tasks. | Minor | Could become slow to load once a user has many tasks. |
| 9 | Login error messages might differ for "wrong password" vs "email not found". | Minor | Could let someone figure out which emails are registered. |
| 10 | Not clear what happens if two tabs/sessions edit the same task at once. | Minor | One person's changes could get overwritten without any warning. |

---

# 7. Testing Approach / Summary

### Testing Approach

I went through each feature and first wrote the normal, expected-to-work case (positive), then thought about what happens with wrong or missing input (negative), and added a few edge cases like special characters, long input, and doing the same action twice quickly. Input validation is listed separately since a few checks like blocking script tags apply to more than one form.

A few details weren't in the requirements (password rules, field limits, session timeout, whether delete needs confirmation, etc). For those, I've written the test case around what should probably happen, but marked it as something to confirm rather than treating it as a fixed rule.

### Key Risks Identified

- Whether a user can access or change another user's task by changing the task ID not addressed in the requirements, so worth checking early.
- Whether there's any limit on repeated wrong login attempts.
- Whether input is checked again on the server side, not just on the screen.

These three would be my priority to verify first, since they affect whether other users' data is safe, not just whether a form behaves as expected.

---

# 8. Conclusion

These test cases cover the main features listed registration, login, and task create/view/edit/delete along with basic input checks. Some of the expected results depend on rules that weren't given in the requirements, so those would need to be confirmed against the actual application before this is treated as final. This isn't a complete test suite, but it should be a reasonable first pass given only the feature list to work from.
