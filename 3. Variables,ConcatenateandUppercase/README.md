# Variables, Concatenate and Upper Case

## Overview

A manually triggered cloud flow that takes a first name and last name, stores them in variables, joins them with a space, and converts the result to upper case. The result is produced by a Compose action and is visible in the run history. The flow does not send, save, or return the value anywhere.

This flow is a simple reference example of working with trigger inputs, variables, and expressions in Power Automate.

## Flow details

| Item | Value |
|---|---|
| Display name | Variables and Concatinate |
| Package name | Variables, Concatenate and Upper case |
| Flow ID | `04b22f66-26b3-4fdc-81b0-c08423465774` |
| Flow type | Instant cloud flow (manual trigger, Button) |
| Connectors | None (built-in actions only) |
| Failure alert subscription | Enabled |
| Source | Created in the Power Automate portal |
| Owner / creator | User ID `e206b255-3547-4be7-bb17-6d06a9ee95f6` |
| Last modified | 2026-11-05 17:32:53 UTC |
| Description | None provided |

## Trigger

**Manual (Button)** runs when the user clicks Run in Power Automate.

| Input | Label shown to user | Type | Required |
|---|---|---|---|
| `text` | First Name | string | Yes |
| `text_1` | Last Name | string | Yes |

## Steps

| # | Action | Type | Runs after | Purpose |
|---|---|---|---|---|
| 1 | `Var1` | Initialize variable | Trigger | Creates string variable `FirstName` from the `text` input. |
| 2 | `Var2` | Initialize variable | `Var1` succeeded | Creates string variable `LastName` from the `text_1` input. |
| 3 | `Compose` | Compose | `Var2` succeeded | Outputs `toUpper(concat(FirstName, ' ', LastName))`. |

### Expression used

```
@toUpper(concat(variables('FirstName'), ' ', variables('LastName')))
```

### Example

| Input | Output |
|---|---|
| First Name: `Ashutosh` / Last Name: `Mishra` | `ASHUTOSH MISHRA` |

## Inputs and outputs

- **Inputs:** First Name and Last Name, both required text values.
- **Outputs:** None returned to a caller. The result is the Compose action's output, shown in the run details.

## Error handling and notifications

- No retry policies, scope blocks, or custom error handling are configured.
- Failure alert subscription is turned on, so the owner receives a notification if a run fails.
- Both inputs are marked required, so the flow cannot start with blank values.

## Environment and ownership

- Built and tested in the Power Automate portal. No environment-specific settings or connection references are used, so it should run unchanged in any environment.
- No credentials or connections are needed.

## Known limitations and notes

- The flow's internal display name ("Variables and Concatinate") differs from the package name ("Variables, Concatenate and Upper case"). Consider renaming for consistency.
- The Compose output is not stored or sent anywhere, so the result is only visible in the run history.
- Values are not trimmed, so extra spaces in an input will carry into the output.
- The variables are only used once each. Compose could use the trigger inputs directly if the variables are not needed for later steps.

## Possible improvements

- Use `trim()` on each input to remove stray spaces.
- Add a Respond to a PowerApp or flow action, or send the result by email or Teams, if the output needs to be used.
- Rename the flow so the display name matches the package name.
