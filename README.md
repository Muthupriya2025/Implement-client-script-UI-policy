# Implement Client Script & UI Policy – Incident

## Project Overview

This micro project demonstrates how ServiceNow UI Policies and Client Scripts can be used to improve data accuracy and enforce business rules on Incident records.

The project focuses on controlling field behavior, automatically updating values, validating form submissions, and preventing unauthorized list edits.

## Problem Statement

Incident records require consistent and accurate data entry for effective triage, routing, reporting, SLA compliance, and resolution.

Manual validation can result in incomplete or incorrect information. This project addresses this issue by implementing UI Policies and Client Scripts that enforce field behavior and validation directly on the Incident form.

## Objective

The objective of this project is to demonstrate how ServiceNow client-side controls can:

- Make fields mandatory based on conditions
- Control field behavior dynamically
- Automatically populate field values
- Validate information before saving an Incident
- Prevent unauthorized list-based updates
- Improve data quality and consistency

## Skills Demonstrated

- Incident Management
- UI Policies
- UI Policy Actions
- Client Scripts
- Form Validation
- Data Integrity
- ServiceNow Configuration

## Project Implementation

### 1. UI Policy – High Impact Control

A UI Policy named **High Impact Control** was created for the Incident table.

**Condition:**
- Impact is High (1)

**Configuration:**
- Assignment Group is made mandatory
- Reverse if false is enabled

This ensures that additional controls are applied when an Incident has High Impact.

### 2. UI Policy Action – Urgency

A UI Policy Action was created for the **Urgency** field.

**Configuration:**
- Field: Urgency
- Read-only: Enabled
- Visible: Unchanged

When Impact is High, the Urgency field becomes read-only.

### 3. onChange Client Script

An **onChange Client Script** was created on the Impact field.

When Impact is changed to High, the script automatically sets Urgency to High and displays an informational message.

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage('Urgency set to High for High impact incident.');
    }
}
4. onSubmit Client Script
An onSubmit Client Script was created to validate the Assigned To field.
When Impact is High and Assigned To is empty, the Incident cannot be submitted.
function onSubmit() {
    if (g_form.getValue('impact') == '1' &&
        g_form.getValue('assigned_to') == '') {

        g_form.showErrorBox(
            'assigned_to',
            'Assigned To is mandatory for High impact incidents.'
        );
        return false;
    }
    return true;
}
5. onCellEdit Client Script
An onCellEdit Client Script was created for the State field.
It prevents users from changing the State directly from the Incident list view.
function onCellEdit(sysIDs, table, oldValues, newValue, callback) {

    alert('State cannot be updated using list editing. Please open the Incident.');

    callback(false);
}
Testing
The following scenarios were tested:
Mandatory Enforcement
High Impact Incident with empty Assigned To
Record submission is prevented
Successful Save
Assigned To is provided
Incident is saved successfully
Reverse Condition
Impact changed from High to Medium
Conditional field behavior is reversed
List Edit Blocking
State edited from Incident list
Direct list edit is blocked
Form-Based Update
State changed from the Incident form
Update is allowed
Expected Outcome
The implementation ensures that Incident records follow defined business rules while improving data consistency and user interaction.
The combination of UI Policies and Client Scripts provides:
Conditional field control
Automatic value updates
Save-time validation
Restricted list editing
Improved Incident data integrity
Evidence
The project evidence includes:
ServiceNow Developer screenshot showing the implemented configuration/activity
SkillWallet screenshot showing project progress
Project implementation details documented in this README
Tools Used
ServiceNow Developer Instance
ServiceNow Incident Management
UI Policies
UI Policy Actions
Client Scripts
Conclusion
The Implement Client Script & UI Policy – Incident project demonstrates how ServiceNow client-side controls can be used to enforce business rules, improve data quality, automate field updates, and control user interactions.
