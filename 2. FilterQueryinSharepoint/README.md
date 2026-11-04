# Filter Query in SharePoint

## Overview

**Filter Query in SharePoint** is a manually triggered Microsoft Power Automate cloud flow. It accepts an employee gender and a minimum age, retrieves matching items from a SharePoint list using an OData filter query, and creates a corresponding item in a second SharePoint list for every match.

The flow demonstrates how to use the **Get items** action with a dynamic SharePoint **Filter Query**, followed by **Apply to each** and **Create item**.

## How the flow works

1. **Manual trigger** – The user runs the flow and supplies both required inputs:
   - **Gender**: a choice of `Male`, `Female`, or `Other`.
   - **Age**: a number representing the minimum age criterion.
2. **Get items** – The flow queries the source SharePoint list and filters records using the supplied values.
3. **Apply to each** – The flow iterates over the items returned by the query.
4. **Create item** – For each matching source item, the flow creates an item in the destination SharePoint list, mapping the fields below.

## Filter query

The exported flow definition contains this filter expression:

```text
Gender eq '@{triggerBody()?['text']}' and Age ge '@{triggerBody()?['number']}'
```

In plain language, it selects records where:

- `Gender` equals the gender selected at runtime; **and**
- `Age` is greater than or equal to the age entered at runtime.

Both conditions must be true for an item to be returned.

## Field mapping

The flow copies the following values from each matching source item into the destination list:

| Destination field | Source value |
|---|---|
| Title | `Title` |
| EmpName | `EmpName` |
| Age | `Age` |
| Gender | `Gender/Value` |

The source and destination lists are identified in the exported definition by SharePoint list IDs. Their display names are not included in the package.

## Prerequisites

- Access to the SharePoint site configured in the flow.
- A SharePoint connection in Power Automate with permission to read from the source list and add items to the destination list.
- A source list containing the fields used by the filter and mapping: `Gender`, `Age`, `Title`, and `EmpName`.
- A destination list containing compatible fields for `Title`, `EmpName`, `Age`, and `Gender`.
- The SharePoint Gender field must support the choice/value structure expected by the flow.

## Setup and configuration

1. Import the flow package into the intended Power Automate environment, or recreate the flow using the steps above.
2. When prompted, select or create the SharePoint connection and authenticate with an account that has the required list permissions.
3. Verify the SharePoint site and select the intended source and destination lists. The exported definition currently points to a site and list IDs from the original environment; these may need to be replaced when moving the flow to another environment or site.
4. Confirm that the internal column names and data types match the fields used in the filter query and field mapping.
5. Save the flow and test it with a gender and minimum age for which you know matching records exist.

## Running the flow

1. Open the flow in Power Automate and select **Run**.
2. Choose a Gender value (`Male`, `Female`, or `Other`).
3. Enter the minimum Age.
4. Submit the run.
5. Review the run history. Check the **Get items** output to see which records matched, and confirm that the corresponding items were created in the destination list.

## Important notes and limitations

- The flow creates a new destination item for every matching source item each time it runs. There is no duplicate-checking or upsert logic in the exported definition; running the same criteria again can create duplicate destination records.
- The flow does not contain an explicit no-results message, summary, or custom error-handling branch.
- The filter uses the SharePoint internal field names `Gender` and `Age`. If the actual internal names differ, update the query accordingly.
- The exported filter expression places the dynamic Age value inside single quotes. Since the trigger defines Age as a number and SharePoint `Age` is expected to be numeric, verify the filter in the target environment; if the connector or column rejects the quoted value, use an unquoted numeric expression.
- The source and destination list IDs and site URL are environment-specific. Reconfigure them when importing into a different SharePoint site.
- The package contains the flow definition and connection references; it does not include the SharePoint lists or their data.

## Main components in the export

- `Microsoft.Flow/flows/<flow-id>/definition.json` – Trigger, SharePoint actions, filter expression, field mappings, and connection reference.
- `Microsoft.Flow/flows/<flow-id>/apisMap.json` – SharePoint connector API mapping.
- `Microsoft.Flow/flows/<flow-id>/connectionsMap.json` – Connection mapping used by the exported package.
- `manifest.json` – Package metadata and resource dependencies.

---

*Documentation is based on the exported Power Automate flow definition. Environment-specific site/list configuration and the actual SharePoint list schemas should be verified before deployment.*
