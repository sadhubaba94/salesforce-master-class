# [**Salesforce Master Class**](https://salesforcemasterclass.netlify.app/)

> **Visit the live portal:** [**https://salesforcemasterclass.netlify.app/**](https://salesforcemasterclass.netlify.app/)

---

<p align="center">
  <a href="https://salesforcemasterclass.netlify.app/">
    <img src="https://img.shields.io/badge/Salesforce-Master%20Class-00A1E0?style=for-the-badge&logo=salesforce" alt="Salesforce Master Class"/>
  </a>
  <br>
  <em>The ultimate resource hub for Salesforce developers, admins, and architects.</em>
</p>

---

## Table of Contents

- [Validation Rules in Salesforce – A Deep Dive](#validation-rules-in-salesforce--a-deep-dive)
- [Roll-Up Summary Fields in Salesforce – A Complete Guide](#roll-up-summary-fields-in-salesforce--a-complete-guide)
- [Flow vs Apex in Salesforce – A Developer's Deep Dive Guide](#flow-vs-apex-in-salesforce--a-developers-deep-dive-guide)
- [Formula Fields in Salesforce – A Complete Guide](#formula-fields-in-salesforce--a-complete-guide)
- [Governor Limits in Apex](#governor-limits-in-apex)
- [SOQL & SOSL in Salesforce – Deep Dive for Developers](#soql--sosl-in-salesforce--deep-dive-for-developers)
- [Callouts in Apex – A Deep Dive for Salesforce Developers](#callouts-in-apex--a-deep-dive-for-salesforce-developers)
- [Async Apex in Salesforce – Deep Dive for Developers](#async-apex-in-salesforce--deep-dive-for-developers)
- [Apex Triggers in Salesforce](#apex-triggers-in-salesforce)
- [Events & Component Communication in LWC](#events--component-communication-in-lwc)
- [Lightning Datatable in Lightning Web Components (LWC)](#lightning-datatable-in-lightning-web-components-lwc)
- [Salesforce Integration – Deep Dive](#salesforce-integration--deep-dive)
- [Ultimate Salesforce Integration Guide (All-In-One)](#ultimate-salesforce-integration-guide-all-in-one)
- [Streaming API vs Pub/Sub API vs REST Hooks in Salesforce](#streaming-api-vs-pubsub-api-vs-rest-hooks-in-salesforce)

---

## Validation Rules in Salesforce – A Deep Dive

### 1. What Are Validation Rules?

Validation Rules in Salesforce are declarative mechanisms used to enforce business rules by validating input data before it's saved to the database. They prevent users from saving records that do not meet the specified conditions by returning custom error messages. These rules are crucial for maintaining data integrity and consistency across the system.

### 2. Why Use Validation Rules?

- 🔐 Enforce field-level data validation without Apex code
- 🧹 Maintain clean, high-quality, and reliable data
- ✅ Ensure business logic is followed by all users
- 🧭 Provide real-time feedback to guide data entry
- 📊 Support audit and compliance by enforcing strict data rules

### 3. Anatomy of a Validation Rule

Each validation rule includes:

- **Rule Name**: A unique and descriptive name
- **Active Status**: Toggle to enable or disable the rule
- **Formula Expression**: Boolean formula that returns TRUE when an error should occur
- **Error Message**: Clear and user-friendly message shown when the formula evaluates to TRUE
- **Error Location**: Can be shown at the field level or at the top of the page

**Basic Syntax Example:**

```
AND(ISBLANK(Email), ISPICKVAL(Contact_Type__c, "Primary"))
```

Error Message: "Email is required."

### 4. Common Functions Used in Validation Rules

| Function | Description | Example |
|---|---|---|
| `ISBLANK()` | Checks if a field is blank | `ISBLANK(Phone)` |
| `ISPICKVAL()` | Evaluates picklist values | `ISPICKVAL(Status, "Open")` |
| `AND()` / `OR()` | Combines multiple conditions | `AND(ISBLANK(Email), ISBLANK(Phone))` |
| `NOT()` | Negates a condition | `NOT(ISPICKVAL(Type, "New"))` |
| `REGEX()` | Validates format of text | Valid email format |
| `LEN()` | Checks string length | `LEN(Name) > 80` |
| `ISCHANGED()` | Detects change in value | `ISCHANGED(Status)` |
| `PRIORVALUE()` | Gets old value before edit | `PRIORVALUE(Status) = "Approved"` |

### 5. Real-World Validation Rule Examples

🔸 **Prevent Close Date in the Past** — Ensure future-oriented pipeline
🔸 **Require Phone Number for "Phone Inquiry" Leads** — Ensure agents can follow up
🔸 **Minimum Opportunity Amount** — Prevent invalid revenue records
🔸 **Validate Email Format** — Prevent invalid email addresses
🔸 **Conditional Field Requirement** — Ensure required fields are filled only under certain statuses

### 6. Tips for Writing Effective Validation Rules

- ✍ **Write clear error messages**: Avoid technical jargon; explain the issue in user-friendly terms
- 🧪 **Test in Sandbox**: Always verify behavior and edge cases before moving to production
- 🚫 **Avoid Over-Validation**: Too many rules may frustrate users
- 🔄 **Combine with Workflow or Flow Logic**: For complex use cases
- 📘 **Documentation**: Maintain internal docs explaining purpose and logic behind each rule

### 7. Execution Order & Limits

- Executed after `before` triggers and before `after` triggers & workflows
- Validation rules can prevent records from being saved
- **Governor Limits**:
  - Max **500** active rules per object
  - Formula compile size: **5,000 characters**
  - Formula character length limit: **3,900 characters**
  - Avoid using complex nested logic that could hit parsing or runtime limits

### 8. Scenario-Based Interview Questions

#### Q1: Enforce selection of a checkbox before record submission
✅ Enforces user acknowledgment

#### Q2: Restrict Discount greater than 30% on Opportunities
✅ Prevents over-discounting during sales

#### Q3: Lock a field once record is marked as "Approved"
✅ Protects data integrity post-approval

#### Q4: Block future birthdates on Contact records
✅ Validates accurate personal information

#### Q5: Ensure dependent picklist field is not blank when parent picklist is selected
✅ Validates completeness of hierarchical data

### 9. Advanced Use Cases

- 🧩 **Cross-Object Validation** (only possible in formula fields, not validation rules)
- 🧾 **Using VLOOKUP()** (only works in custom formula fields, not validation)
- 🔐 **Conditional Mandatory Fields**: Based on user profile or record type
- 🌍 **Localization**: Support multilingual error messages using Custom Labels

### 10. Conclusion

Validation Rules are an essential part of declarative development in Salesforce. They offer powerful guardrails to prevent bad data and ensure business logic is always followed, even without writing a single line of code. *"A strong org isn't built just with great features—it's built with clean, trustworthy data."* Mastering validation rules is one of the easiest yet most impactful ways to level up your Salesforce development.

---

## Roll-Up Summary Fields in Salesforce – A Complete Guide

### 1. What Are Roll-Up Summary Fields?

**Roll-Up Summary Fields** in Salesforce are read-only fields on a master object that aggregate values from related detail records in a master-detail relationship. They perform calculations like SUM, MIN, MAX, and COUNT on related child records. These calculations are stored in the database and automatically updated when related child records are added, modified, or deleted.

> *"Think of Roll-Up Summary Fields as your built-in Excel formulas for related records."*

### 2. Where Can You Use Them?

Roll-up summary fields can only be created on master records in a **master-detail relationship**. They are not available for lookup relationships (unless using tools like DLRS). Examples:

- Total Amount of all Opportunities on an Account
- Number of Cases under a Customer
- Earliest Order Date for a Customer
- Total Number of Invoices linked to a Project

**Objects commonly involved:**
- Account → Opportunity
- Opportunity → Opportunity Product
- Custom Master → Custom Detail

### 3. Types of Roll-Up Summaries

You can perform the following operations:

| Type | Description | Use Case Example |
|---|---|---|
| **SUM** | Adds up values of a numeric field | Total sales revenue per customer |
| **MIN** | Smallest value among related records | First login date of a customer |
| **MAX** | Largest value among related records | Last support ticket creation date |
| **COUNT** | Number of related records | Number of quotes on an opportunity |

### 4. How to Create a Roll-Up Summary Field

1. Go to the master object (e.g., Account).
2. Create a new custom field.
3. Select **Roll-Up Summary** as the field type.
4. Provide a name and label for the field.
5. Choose the child object (detail record).
6. Choose the summary type: **COUNT, SUM, MIN, MAX**.
7. Add optional filter criteria.
8. Save the field and add it to the page layout if needed.

### 5. Use Cases

| Use Case | Description |
|---|---|
| Total Order Value | Sum of Order Amounts on a Customer |
| Oldest Invoice | Min Invoice Date on a Billing Account |
| Number of Active Cases | COUNT where Status = 'Open' |
| Highest Transaction Value | MAX of Transaction.Amount__c |
| Average Sales (via formula) | SUM(Order_Amount) / COUNT(Order) using formula fields |

### 6. Filtering Records in Roll-Ups

You can apply filter criteria to child records while rolling up:

> **Advanced Tip:** Combine filters with logical operators to create precise calculations.

These filters ensure only relevant records are included in the roll-up summary calculation.

### 7. Limitations

- Only available on master-detail relationships
- Not available on lookup relationships (unless using DLRS)
- Cannot reference grandchild records (child of child)
- Max **25** roll-up summary fields per object (can be increased via support)
- Cannot use text fields for SUM, MIN, MAX
- Cannot use cross-object formulas in roll-up filters
- May not support complex filters like ISCHANGED() or multi-level logic

### 8. Alternatives to Roll-Up Summary Fields

- **DLRS (Declarative Lookup Rollup Summaries Tool):**
  - Open-source managed package
  - Allows roll-ups on lookup relationships
  - Supports scheduled and real-time calculations
- **Apex Trigger or Batch Apex:**
  - Custom roll-up logic with full control
  - Can handle complex conditions, grandchild relationships, and large data volumes
  - Requires development and testing

### 9. Real-World Interview Questions

#### Q1. Can you create a roll-up summary field on a lookup relationship?
**A:** Not natively. Use the DLRS tool or triggers for this.

#### Q2. How do you calculate the total 'Closed Won' Opportunities on an Account?
**A:** Create a roll-up summary field on Account for Opportunities. Use SUM on Amount with filter Stage = 'Closed Won'.

#### Q3. How would you count only Active child records in a roll-up?
**A:** Use the COUNT summary type with a filter: `IsActive__c = TRUE`.

#### Q4. What are the governor limits related to roll-ups?
**A:** While execution limits don't govern the creation of roll-ups, the DLRS tool (or Apex-based roll-ups) can count toward CPU time, DML limits, or SOQL limits.

#### Q5. Can you roll-up from grandchild records?
**A:** No. You'd need DLRS and intermediate fields or Apex triggers.

#### Q6. Can Roll-Up Summary Fields reference formula fields?
**A:** Yes, but performance may vary, especially if the formula is complex.

### 10. Best Practices

- Use filters to improve relevance and performance
- Label roll-ups clearly to reflect the business intent
- Keep roll-up summary logic simple and avoid relying heavily on formula fields
- Use DLRS if your use case requires lookup or cross-object rollups
- Avoid rolling up frequently updated fields to prevent performance bottlenecks
- Validate roll-ups using report cross-checks

### 11. Summary

Roll-Up Summary Fields are essential for summarizing child data without writing code. They bring reporting-level insight directly into record layouts. Used wisely, they simplify automation, support decision-making, and improve UI responsiveness.

> *"If a formula field is smart, a roll-up summary is strategic."*

---

## Flow vs Apex in Salesforce – A Developer's Deep Dive Guide

### 1. Introduction

Salesforce empowers developers and admins with two distinct automation approaches: **Flow**, a declarative tool for low-code solutions, and **Apex**, a programmatic tool for writing robust logic. The key to building efficient and maintainable applications lies in understanding their differences, strengths, weaknesses, and when to combine them.

### 2. What is Flow?

Flow is part of Salesforce's low-code automation toolbox, designed for building business processes using a visual UI.

🔹 **Types of Flows:**
- **Record-Triggered Flow**: Executes on record insert/update/delete (before or after context)
- **Scheduled Flow**: Executes in batch at a scheduled time
- **Screen Flow**: Includes user interaction (form inputs, guided wizards)
- **Autolaunched Flow**: Invoked via Process Builder, Apex, or another Flow
- **Platform Event-Triggered Flow**: Responds to published events

🔹 **Core Flow Concepts:**
- Variables & Constants
- Record Variables vs. Single Variables
- Collections & Loops
- Subflows for modular logic
- Fault Paths for exception handling

🔹 **Enterprise Flow Use Cases:**
- Guided lead qualification wizard (Screen Flow)
- Mass record updates every night (Scheduled Flow)
- Before-save validation on custom object fields
- Quick automations that replace old Workflow Rules or Process Builders

### 3. What is Apex?

Apex is Salesforce's strongly typed, object-oriented programming language, designed to run on the multi-tenant Lightning Platform.

🔹 **Where Apex Shines:**
- Custom Triggers on DML events
- Rich Class structures for abstraction
- Asynchronous jobs (Future, Batch, Queueable, Scheduled)
- Callouts to external APIs
- Building reusable business logic (trigger handlers, utils, frameworks)
- Fine-grained control over governor limits, transactions, error handling

🔹 **Apex in the Enterprise:**
- Integrating Salesforce with payment gateways
- Creating nightly sync jobs with ERP/legacy systems
- Complex commission or discount logic
- Security logic spanning multiple objects or layers

### 4. Flow vs Apex – Deep Comparison Table

| Feature / Concern | Flow | Apex |
|---|---|---|
| Setup Time | Very fast; UI-based | Slower; needs class/trigger + test class |
| Performance in Bulk | Limited; prone to hitting limits | Highly optimized when bulkified |
| Reusability | Difficult beyond subflows | High; reusable methods, utils, testable units |
| Debugging | Visual debugger (limited stack trace) | Full stack trace, logs, breakpoints |
| Test Automation | Manual or 3rd-party tools | Required via test classes; full CI/CD integration |
| CI/CD Integration | Challenging | Supported via SFDX/Git/Copado |
| Callouts/External Integration | Not supported directly | Supported (HTTP, REST, SOAP) |
| UI Forms | Yes (Screen Flow) | ❌ None |
| Recursion Management | No built-in handling | Can manage recursion depth/context via code |
| Dynamic Behavior | Limited dynamic DML/SOQL | Fully dynamic via Apex |

### 5. Hybrid Approach: Best of Both Worlds

Many real-world solutions use **Flow for orchestration** and **Apex for heavy lifting**.

**Pattern:**
1. Record-triggered Flow (after save)
2. Decision element checks for specific criteria
3. Call Apex using an **Apex Action** element for custom processing

### 6. Performance & Governor Limits

🔹 **Flow Limitations:**
- SOQL queries: Max **100** per transaction
- DML operations: Max **150**
- CPU time: Max **10,000ms**
- Flows lack query optimization tools like Maps/Sets

🔹 **Apex Capabilities:**
- Leverages collections and Maps for bulk operations
- Can split large operations across Queueable/Batch
- Allows developer to manage transaction boundaries and retry logic

### 7. Testing & Debugging Best Practices

**Flow:**
- Use Debug on Canvas
- Build test cases manually using test records
- Always configure Fault Paths for critical steps

**Apex:**
- Write test classes with:
  - Positive/negative scenarios
  - Bulk execution
  - Assertions for every business rule
- Use Developer Console, Logs, and Test Run UI

### 8. DevOps, Versioning, Collaboration

| Aspect | Flow | Apex |
|---|---|---|
| Version control | UI-based | Text-based, fully Git-friendly |
| Git merging | Difficult | Full support |
| CLI tooling | Limited | VS Code + Salesforce CLI |
| Peer reviews & CI/CD | Challenging | Pull requests, CI/CD workflows |

### 9. Real-World Scenarios

| Scenario | Recommended Tool | Notes |
|---|---|---|
| Multi-step onboarding form | **Screen Flow** | Use Subflows for modularity |
| Update parent record based on child aggregate | **Apex Trigger** | Use SOQL with Aggregates and Trigger Handler |
| Bulk account enrichment using external API | **Queueable Apex** | Avoid limits, control callout retries |
| Case routing based on logic and custom object thresholds | **Flow + Invocable** | Best of both worlds |
| Daily export of records to external FTP | **Apex + Batch** | Use Scheduled Apex + FTP integration |

### 10. Best Practices Recap

**For Flow:**
- Use Before Save Flows to reduce DML
- Split complex logic using Subflows
- Monitor flows with Flow Error Emails
- Avoid complex nesting of loops

**For Apex:**
- Apply Trigger Frameworks (1 trigger per object)
- Write reusable service classes
- Handle limits using `Limits.get...()` methods
- Catch exceptions and use meaningful error messages

### 11. Real-World Scenario-Based Interview Questions

**1. You need to update a parent Account record whenever a related Contact is inserted or updated. What tool would you use and why?**
- ✅ **Apex Trigger** – Because child-to-parent updates based on aggregate logic require custom SOQL and are not handled efficiently by Flow.

**2. A business team wants a guided wizard for customer onboarding with conditional steps. What should you choose?**
- ✅ **Screen Flow** – Ideal for user-driven, step-by-step form-based input with decision branching.

**3. You need to call a third-party REST API after a record is created. Can you use Flow?**
- ❌ **No.** Use Apex (Queueable or Future) because Flows don't support callouts directly.

**4. How would you design a solution that sends a weekly digest email summarizing closed Cases?**
- ✅ **Scheduled Flow** or **Scheduled Apex** – Flow for simple use, Apex for advanced aggregation/templating.

**5. Which automation tool would you use if you want to modularize logic and reuse it across multiple triggers?**
- ✅ **Apex** – Allows creation of service classes and handlers that are reusable and testable.

### 12. Conclusion

- ✅ Use **Flows** when you need simple, admin-manageable logic
- ✅ Use **Apex** when you need performance, reuse, or integration
- 🚀 Use **both together** for layered, scalable solutions

---

## Formula Fields in Salesforce – A Complete Guide

### 1. What are Formula Fields?

**Formula Fields** in Salesforce are read-only custom fields that automatically calculate values based on other fields, expressions, or logic. These fields are dynamically updated in real-time.

> *"Think of Formula Fields as mini-calculators on your records."*

### 2. Key Features

- Real-time evaluation
- Read-only and auto-calculated
- Support for arithmetic, logical, text, and date operations
- Can reference fields from related objects (cross-object)
- Evaluated on-the-fly – no data is stored
- Used in validation rules, workflows, reports, and dashboards

### 3. Formula Field Syntax

Formula fields use a combination of:

- **Operators**: `+`, `-`, `*`, `/`, `=`, `!=`, `AND()`, `OR()`
- **Functions**: `IF()`, `CASE()`, `ISBLANK()`, `TEXT()`, `DATE()`, `NOW()`, `LEN()`, `CONTAINS()`, `ROUND()`, `BLANKVALUE()`, `VALUE()`
- **Field References**: Use merge fields like `Account.Name`, `Opportunity.Amount`, etc.
- **Literal Values**: Numbers, text (in quotes), dates, etc.

**Example:**

```
IF(Amount > 100000, "High Value", "Standard")
```

### 4. Cross-Object Formulas

You can reference fields from parent objects (lookup or master-detail relationships).

**Example:** Display the Account Phone number on the Opportunity object:
```
Account.Phone
```

> **Limitation:** You can only go **one level up** (parent-to-child not allowed). Use custom roll-up summaries or Apex for child-to-parent logic.

### 5. Use Cases of Formula Fields

| Use Case | Formula Sample | Explanation |
|---|---|---|
| Sales Commission | `Amount * 0.05` | Calculates 5% of Opportunity amount |
| Status Flag | `IF(IsActive__c, "Active", "Inactive")` | Visual status for users |
| Days Since Created | `TODAY() - CreatedDate` | Tracks how old the record is |
| Discount Check | `IF(Discount__c > 0.2, "Check Discount", "OK")` | Enforces discount policy |
| Age Calculation | `YEAR(TODAY()) - YEAR(Date_of_Birth__c)` | Approximate age from birthdate |
| Fiscal Year Category | `IF(MONTH(CloseDate) <= 3, "Q4", "FY")` | Derive fiscal period |

### 6. Advanced Techniques

- **Nested IFs vs CASE()**: Use `CASE()` for cleaner conditional logic
- **HTML in Formula Fields**: Display colored labels (e.g., use `IMAGE()` with status icons)
- **BLANKVALUE and NULLVALUE**: Handle null values gracefully
- **ISCHANGED()** inside validation rules can complement formula fields for tracking changes

### 7. Limitations

- Cannot perform DML or callouts
- Cannot reference child objects
- Max formula size: **3,900 characters** (compiled), **5,000** (display)
- Cannot trigger automation like Flows or Triggers directly
- Limited functions compared to Apex

### 8. Best Practices

- Use `TEXT()` to convert picklist and boolean fields for display
- Avoid overly complex formulas for readability and performance
- Use `CASE()` over nested `IF`s for multiple conditions
- Use emojis or symbols for intuitive UI in formula results
- Comment your formulas using notes (inside `/* */` blocks)

### 9. Real-World Interview Questions

#### Q1. Can you give an example of a cross-object formula you used?
**A:** I used `Account.Industry` on a custom object to classify records for reporting without creating duplicate fields.

#### Q2. What would you use to calculate the number of days an Opportunity was in a specific stage?
**A:** Create a formula field: `TODAY() - LastModifiedDate` (assuming the stage update was the last change).

#### Q3. How do you create a formula field that shows a colored status text?
**A:** Use `CASE()` with HTML IMAGE() tags or use `ISPICKVAL()` with conditional logic.

#### Q4. What is the difference between a formula field and a workflow field update?
**A:** A formula field is real-time and read-only. Workflow field updates are stored in the database and can be edited.

#### Q5. Can a formula field reference a field from a related object?
**A:** Yes, but only if it's a parent object in a lookup or master-detail relationship.

#### Q6. How do you show today's date in a formula field?
**A:** Use `TODAY()` or `NOW()` depending on whether you need time precision.

#### Q7. How can you handle a null picklist in a formula?
**A:** Use `ISBLANK()` or wrap with `TEXT()` before evaluating.

### 10. Conclusion

Formula Fields are a powerful declarative tool in Salesforce that enable automation, cleaner UI, and smarter reporting without writing Apex. When used thoughtfully, they reduce complexity and improve user experience.

> *"A single formula field can save you hours of development if used right."*

---

## Governor Limits in Apex

### 1. What Are Governor Limits?

Salesforce runs on a **multi-tenant architecture**, where resources are shared among multiple customers. To ensure fair usage and prevent any single tenant from monopolizing server resources, Salesforce enforces **Governor Limits** on code execution. These limits apply to Apex, SOQL, DML, callouts, and more.

### 2. Categories of Governor Limits

🔹 **Per-Transaction Limits** (Synchronous vs Asynchronous)

| Resource | Synchronous Limit | Asynchronous Limit |
|---|---|---|
| Total SOQL Queries | 100 | 200 |
| Records Retrieved by SOQL | 50,000 | 50,000 |
| Total DML Statements | 150 | 150 |
| Records Processed by DML | 10,000 | 10,000 |
| CPU Time (ms) | 10,000 | 60,000 |
| Future Methods per Txn | 0 | 50 |
| Callouts per Txn | 1 (sync) | 100 (async) |

🔹 **Per-Org or Static Limits** — These apply to the entire org:
- Max Batch Apex Jobs: **5** queued/active
- Max Queueable Jobs Chained: **50**
- Max Daily Email Invocations: **5,000/org**
- Max Scheduled Apex Jobs: **100**

### 3. Types of Limits

**🧮 Query & DML Limits**
- SOQL: 100 queries (sync)
- DML: 150 statements (sync)
- *Tip:* Use `Map<Id, Object>` patterns to reduce redundant queries

**🧠 Heap Size Limit**
- Max Heap Size: **6MB** (sync), **12MB** (async)
- Use `Limits.getHeapSize()` to track

**🪵 CPU Time Limit**
- Use `Limits.getCpuTime()` to avoid hitting 10,000ms / 60,000ms cap

**🔁 Looping Limit**
- Max loop iterations: **1,000,000**
- *Tip:* Avoid nested loops; use sets and maps instead

**📞 Callouts**
- Max **100** callouts per async transaction
- Must annotate methods with `@future(callout=true)` or implement `Database.AllowsCallouts`

### 4. Tools to Monitor Governor Limits

- `Limits.getLimitDmlStatements()` / `getDmlStatements()`
- `Limits.getQueryRows()`
- Developer Console Debug Logs (highlight governor usage)
- Apex `System.debug()` to log execution stats

### 5. Best Practices to Avoid Hitting Limits

1. **Bulkify Code**: Always process collections (List, Set, Map)
2. **Avoid SOQL/DML in Loops**: Query first, then process
3. **Use Maps for Data Association**
4. **Cache Results Where Needed**
5. **Queueable/Batch for Heavy Lifting**
6. **Use Limits API to Monitor**

### 6. Real-World Scenario-Based Questions

#### Q1. What happens if your trigger exceeds 150 DML statements?
- **Answer:** Governor limit exception: *Too many DML statements: 151*

#### Q2. You're hitting 100 SOQL limit. How to fix?
- **Answer:** Move SOQL outside loops; use one query with WHERE IN clause

#### Q3. Need to update 100,000 records. What's your approach?
- **Answer:** Use Batch Apex to process in manageable chunks of 200

#### Q4. How can you reduce CPU time usage in a trigger with complex logic?
- **Answer:** Optimize logic, avoid recursion, use helper classes, and minimize field updates

#### Q5. A trigger is failing with a "Too many SOQL queries" error. What would you do?
- **Answer:** Refactor the trigger to avoid SOQL in loops. Use a map or list to retrieve data in bulk.

#### Q6. Your code works in sandbox but fails in production with "Too many DML rows". Why?
- **Answer:** Sandbox often has less data. Production might be hitting the 10,000 DML row limit. Use batch processing instead.

#### Q7. You need to send 150 callouts in one process. What's your strategy?
- **Answer:** Use Batch Apex with 100 callouts per `execute()` iteration or split into multiple queueable jobs.

#### Q8. How can you monitor usage of limits during execution?
- **Answer:** Use `Limits.getLimitX()` and `Limits.getX()` methods to monitor and act accordingly.

### 7. DevOps Considerations

- Integrate Limit Tracking into Apex test classes
- Add Logging for Limits in Error Logs or Custom Object
- Create Alert Mechanisms (Email on limit breach)
- Mock Limits in Unit Tests using `Test.startTest()`/`Test.stopTest()`

### 8. Anti-Patterns That Break Limits

- ❌ SOQL inside for-loop
- ❌ DML inside nested for-loop
- ❌ Inefficient filtering (querying unnecessary fields)
- ❌ Multiple triggers on same object without coordination

### 9. Conclusion

Governor Limits are essential for writing scalable, efficient, and compliant Salesforce applications. Instead of fearing limits, embrace them as a guide for writing high-performance code. The more you understand and anticipate them, the more future-proof your applications become.

> **Pro Tip:** Use bulk patterns, async Apex, and helper classes to stay below limits.

---

## SOQL & SOSL in Salesforce – Deep Dive for Developers

---

## Callouts in Apex – A Deep Dive for Salesforce Developers

### 1. What is a Callout in Apex?

A **callout** in Salesforce refers to an HTTP request sent from Apex code to an external service. This allows Salesforce to interact with APIs, retrieve or send data, and integrate with third-party systems.

### 2. Types of Callouts

🔹 **HTTP Callouts (REST & SOAP)**
- **REST**: Use `HttpRequest`, `HttpResponse`, `Http`, JSON-based payloads
- **SOAP**: Use WSDL2Apex-generated classes

🔹 **Named Credentials**
- Simplifies authentication and endpoint management
- Secure way to store credentials

🔹 **Continuation**
- For long-running requests that may exceed timeout limits (e.g., over 10 seconds)

🔹 **Callout from Flow**
- Possible via Flow Actions + External Services or Apex Invocable Methods

### 3. Anatomy of a REST Callout

```
HttpRequest req = new HttpRequest();
req.setEndpoint('https://api.example.com/endpoint');
req.setMethod('GET');
req.setHeader('Content-Type', 'application/json');

Http http = new Http();
HttpResponse res = http.send(req);
System.debug(res.getBody());
```

### 4. Key Annotations and Interfaces

| Feature | Purpose |
|---|---|
| `@future(callout=true)` | Used for async callouts |
| `Queueable` | Enables async callouts with chaining |
| `Continuation` | Handles long-running requests (>10 seconds) |
| `HttpCalloutMock` | Used in test methods for mocking responses |
| `@InvocableMethod` | Make callouts accessible via Flow if needed |

### 5. Callout Limits and Restrictions

| Limit | Value |
|---|---|
| Max callouts per transaction | 100 |
| Max timeout | 120 seconds |
| Callout after DML | ❌ Not Allowed |
| Callout in Test Class | ✔ Requires Mocking |
| HTTP methods supported | GET, POST, PUT, PATCH, DELETE |

### 6. Error Handling in Callouts

- Use try-catch blocks to catch `CalloutException`
- Always check `HttpResponse.getStatusCode()`
- Log and monitor errors (store in custom object/logs)
- Retry logic (preferably exponential backoff for retrying)

### 7. Best Practices for Callouts

1. Use **Named Credentials** for managing endpoints & auth securely
2. Avoid hardcoded URLs – use **Custom Metadata or Settings**
3. Callouts should be **async** wherever possible (Queueable/Future)
4. **Always handle errors** – no blind trust in external services
5. Timeouts & retries should be handled gracefully
6. Log responses & errors to help with debugging
7. Design for resilience – implement fallback logic where needed

### 8. Testing Callouts in Apex

You must **mock** callouts in Apex tests to avoid real API calls:

```apex
Test.setMock(HttpCalloutMock.class, new MyMockClass());
```

- Use `implements HttpCalloutMock`
- Return a simulated `HttpResponse`

### 9. Real-World Scenario-Based Interview Questions

#### Q1. You need to make a POST callout from a trigger. What's your approach?
**A:** Triggers can't perform callouts directly. Use Platform Events or Queueable with `@future(callout=true)` to move logic to async context.

#### Q2. How do you secure sensitive endpoint credentials in Apex?
**A:** Use **Named Credentials**. They abstract authentication and securely store credentials.

#### Q3. How would you handle a 500 Internal Server Error from an API?
**A:** Implement retry logic with backoff, log the issue, and alert stakeholders. Use monitoring tools and catch `CalloutException`.

#### Q4. Can you use DML and Callout in the same method?
**A:** Not in the same context. Split logic or use Queueable/Future methods to avoid mixed-context errors.

#### Q5. How would you monitor API failures in production?
**A:** Log callout failures in a custom object. Send alerts via Flow or Email. Use Platform Events or Event Monitoring.

#### Q6. How do you perform callouts from Flow?
**A:** Use **External Services** with OpenAPI schema or call an Apex method using `@InvocableMethod`.

### 10. Advanced Topics

- **Chaining Callouts**: In Queueable Apex using chained jobs
- **Streaming API + Callouts**: Listen to external system changes and trigger outbound actions
- **Webhooks Simulation**: Use a public service like webhook.site to test external callouts
- **API Management**: Rate limiting, caching, and batching strategies

### 11. Conclusion

Callouts allow Salesforce to act beyond its borders. When done right, they enable real-time, secure, and robust integrations.

> *"Well-designed integrations don't just fetch data—they strengthen your architecture."*

✅ Use async callouts with proper error handling.
✅ Secure your integrations with Named Credentials.
✅ Always test with mocks and design for failure.

---

## Async Apex in Salesforce – Deep Dive for Developers

### 1. Introduction

**Asynchronous Apex** in Salesforce allows developers to run processes in the background, separate from the main transaction. This is crucial for improving performance, handling large data volumes, and performing time-intensive operations such as callouts, batch updates, or scheduled jobs.

### 2. Types of Asynchronous Apex

🔹 **Future Methods**
- **Use Case:** Lightweight async processing like sending emails, simple updates
- **Limitations:**
  - No return values
  - Cannot be chained
  - No callouts if `@future(callout=false)`

🔹 **Queueable Apex**
- **Use Case:** Flexible jobs with chaining and state management
- **Supports:** Callouts, job chaining, complex logic

🔹 **Batch Apex**
- **Use Case:** Massive record processing (millions), large data sets
- **Supports:** Parallelism, query locator, restartability
- **Structure:** `start`, `execute`, `finish`

🔹 **Scheduled Apex**
- **Use Case:** Run Apex logic at defined intervals

### 3. Governor Limits – Async vs Sync

| Operation | Synchronous Limit | Asynchronous Limit |
|---|---|---|
| SOQL Queries | 100 | 200 |
| DML Statements | 150 | 150 |
| CPU Time (ms) | 10,000 | 60,000 |
| Future Calls | ❌ | 50 per transaction |
| Callouts | 1 (sync) | 100 (async) |

### 4. Choosing the Right Async Type

| Requirement | Recommended Tool |
|---|---|
| External API Callout | Queueable |
| Scheduled Data Cleanup | Scheduled Apex |
| Record Updates in Bulk | Batch Apex |
| Lightweight Background Operation | Future Method |
| Chained Dependent Operations | Queueable |
| Exporting Data to External System | Batch + Callout |

### 5. Real-world Enterprise Use Cases

- 🔄 **Data Sync with ERP System:** Queueable Apex + Platform Events
- 📤 **Exporting 1M+ records to FTP:** Batch Apex with 2000 record scope
- ⏰ **Monthly Email Summary Reports:** Scheduled Apex + Email service
- 📞 **Queue Callouts to Payment Gateway:** Queueable Apex with retry

### 6. Error Handling Patterns

- Wrap logic in try-catch blocks
- Send email to admins via `Messaging.sendEmail`
- Store errors in custom object (`Async_Job_Log__c`)
- Use `Limits.getCPUTime()` for graceful fallback

### 7. Test Class Strategies

- **Batch Apex:** Call `executeBatch` with scope
- **Queueable Apex:** `Test.startTest()` / `Test.stopTest()`
- **Scheduled Apex:** Schedule and execute with `Test.stopTest()`
- Use assertions to verify DML operations or field updates
- Always use `Test.stopTest()` to ensure async job execution

### 8. DevOps & CI/CD Considerations

- Apex async code is testable and version-controllable
- Integrates seamlessly with SFDX, Git, Jenkins, Copado
- Create modular jobs to isolate failures
- Use feature flags to disable async logic via custom metadata

### 9. Common Pitfalls & Anti-Patterns

- ❌ Chaining too many Queueables (>50)
- ❌ Running long-running jobs synchronously
- ❌ Not bulkifying logic inside `execute()`
- ❌ Not checking limits before DML
- ❌ Using Future when Queueable fits better

### 10. Scenario-Based Interview Questions

#### Q1. You need to sync 1 million contact records to an external system nightly. What approach will you use?
**A:** Use Batch Apex with scope of 2000. In each execute, make an HTTP callout (with `Database.AllowsCallouts`). Schedule the batch using Scheduled Apex.

#### Q2. You need to retry failed API sync jobs. How would you handle that?
**A:** Use Queueable Apex with a custom retry mechanism and exponential backoff stored in custom metadata.

#### Q3. Why would you prefer Queueable over Future methods?
**A:** Queueable supports chaining, job tracking, state passing via constructor, and allows callouts — none of which Future supports.

#### Q4. How do you avoid hitting governor limits in async jobs?
**A:** Use Maps to avoid redundant queries, always bulkify DML/logic, and monitor Limits API to stop execution if near threshold.

### 11. Conclusion

Asynchronous Apex is the backbone of scalable Salesforce apps. It is crucial to choose the right async tool, understand limits, and implement proper error handling.

---

## Apex Triggers in Salesforce

### What Are Apex Triggers?

Apex Triggers allow you to:

- Respond to DML events (Insert, Update, Delete)
- Run custom logic before/after data changes
- Automate processes at the database level

Think of them as *event handlers* that listen and react when data is created, changed, or removed. If Flows are declarative automation, Triggers are programmatic.

### Trigger Lifecycle & Context

Triggers execute during the **Save Order of Execution**.

**Common Context Variables:**

| Variable | Purpose |
|---|---|
| `Trigger.new` | New version of the records |
| `Trigger.old` | Previous version (only update/delete) |
| `Trigger.isInsert` | True if it's an Insert operation |
| `Trigger.isUpdate` | True if it's an Update operation |
| `Trigger.isDelete` | True if it's a Delete operation |
| `Trigger.isBefore` | Executes before saving to DB |
| `Trigger.isAfter` | Executes after saving to DB |
| `Trigger.newMap` | Id → Record (new) |
| `Trigger.oldMap` | Id → Record (old) |

### Trigger Events

**Supported Trigger Events:**
- `before insert`
- `before update`
- `before delete`
- `after insert`
- `after update`
- `after delete`
- `after undelete`

**Choose wisely:**
- **before:** modify fields or validate input
- **after:** use IDs or create related records

### Trigger Use Cases – Real Examples

- **Auto-create child records:** When a new Account is inserted, auto-create a default Contact
- **Cross-object updates:** When Opportunity is marked Closed-Won, update Account.Status
- **Custom validation:** Prevent deleting Case if related Tasks exist
- **Data normalization:** Auto-format phone number fields to a standard format

### Trigger Best Practices (Expanded)

- ✅ **Bulkify** all logic (Triggers always run in bulk)
- ✅ Use **Maps/Sets** instead of nested loops
- ✅ Avoid **DML/SOQL inside loops**
- ✅ Check `Trigger.isInsert`, `isUpdate` properly
- ✅ Prevent **recursive triggers** (use static flags)
- ✅ Keep trigger logic **minimal**
- ✅ Use **Try-Catch** blocks for error resilience
- ✅ Write test methods with **multiple records & edge cases**

> Clean triggers = fewer production bugs.

### Preventing Recursive Triggers

**Problem:** A trigger update causes another update → infinite loop
**Solution:** Use a **static variable**

Essential when doing DML inside an "after" trigger.

### Testing Triggers Like a Pro

Every Trigger MUST have a Test Class:
- Insert, Update, Delete tests
- Negative test cases (assert that errors are thrown)
- Bulk tests with 200+ records
- Assert that logic executed as expected

### Summary – Trigger Takeaways

- Triggers allow precise, powerful automation
- Use **1 trigger per object**
- Move logic to **handler classes**
- Always **bulkify**
- Avoid **recursion**
- Write **meaningful tests**
- Follow naming conventions (`ObjectNameTrigger`, `ObjectNameTriggerHandler`)

---

## Events & Component Communication in LWC

### 1. Introduction

Component communication is a core skill in Lightning Web Components development. It enables the exchange of data and events between components — whether they are related (parent-child) or completely unrelated (across the DOM).

### 2. Parent to Child Communication

This is done using the **`@api`** decorator.

✅ **Use Case:** A parent passes data to a child component for display.

✅ **Key Points:**
- Only `@api` properties can be passed from parent
- Properties are reactive (if updated in parent, child sees changes)

### 3. Child to Parent Communication

Use **`CustomEvent`** to notify the parent of an action or data.

✅ **Use Case:** Child sends selected record ID to parent when a row is clicked.

✅ **Key Points:**
- Event name should be lowercase with no special characters (e.g., `onselected`)
- You can pass data using `detail`
- Events can bubble and be composed

### 4. Child ➝ Parent ➝ Grandparent (Bubbling Events)

Use `bubbles: true, composed: true` in `CustomEvent` for the event to travel up the DOM tree.

✅ **Key Points:**
- **Bubble** allows event to move up DOM tree
- **Composed** allows it to pass through shadow DOM boundaries

### 5. Sibling to Sibling Communication

Handled indirectly in 2 ways:

**a) Via Parent Component**
Parent acts as mediator to relay data between siblings.
*[Child A] → event → [Parent] → update prop → [Child B]*

**b) Via Lightning Message Service (LMS)**
Use LMS when siblings are unrelated or on separate DOM trees (like in Utility Bar or App Page).

### 6. Lightning Message Service (LMS)

LMS enables communication between **unrelated components**, across pages or even in different tabs.

✅ **Setup:**
1. Create Message Channel in `messageChannels/` folder
2. Publish from one component
3. Subscribe in another component

✅ **When to Use LMS:**
- Communicating between Utility Bar and Record Page
- Sharing state across tabs in Console App
- Sibling communication without a common parent

### 7. Event Object Structure in CustomEvent

- `detail`: payload (object, primitive, etc.)
- `bubbles`: enables parent/grandparent listening
- `composed`: allows breaking out of shadow DOM
- `cancelable`: allows the parent to prevent default behavior

### 8. Real-World Interview Scenarios

#### Q1. How do you send data from a child to a grandparent in LWC?
**A:** Fire a `CustomEvent` with `bubbles: true, composed: true`. Grandparent listens for the event and receives it.

#### Q2. What's the difference between LMS and CustomEvent?
**A:** LMS is for unrelated components across the app. CustomEvent is for components within the same DOM hierarchy.

#### Q3. Why might @api properties not reflect changes?
**A:** If the property is reassigned in child (instead of reading only), it breaks reactivity. `@api` is one-way only.

#### Q4. Can you communicate between an Aura and LWC component?
**A:** Yes, using events or LMS if encapsulated properly.

### 9. Best Practices

- Use `@api` only to expose what child needs from parent
- Always include a `detail` object in `CustomEvent`
- Don't tightly couple child-parent — prefer events
- For complex flows, use LMS or Redux-style state mgmt
- Avoid deeply nested event chains — instead use services

### 10. Summary

Whether you're building multi-step wizards, dashboards, or dynamic forms — understanding component communication is critical to building scalable and maintainable LWC applications.

> *"In LWC, how you communicate is as important as what you build."*

---

## Lightning Datatable in Lightning Web Components (LWC)

### 1. Introduction

`<lightning-datatable>` is one of the most versatile Lightning Base Components. It offers rich, spreadsheet-like capabilities — sortable columns, inline editing, row actions, mass selection, and custom cell rendering — all while adhering to Salesforce Lightning Design System (SLDS).

### 2. Basic Anatomy

```html
<lightning-datatable
  key-field="Id"
  data={data}
  columns={columns}
  onsave={handleSave}
  onrowaction={handleRowAction}>
</lightning-datatable>
```

### 3. Column Definition

| Property | Purpose |
|---|---|
| `label` | Column header |
| `fieldName` | API name in data array |
| `type` | text, number, currency, date, boolean, url, button, action |
| `editable` | Enables inline editing |
| `sortable` | Enables column sorting |

### 4. Custom Data Types

Define a custom type via `type: 'custom'` and provide a template. Create a separate LWC (e.g., `datatableBadge`) and register in the datatable. Custom cells unlock icons, progress bars, and dynamic badges.

### 5. Sorting (Client & Server)

**Client-side:** Track `sortBy` and `sortDirection`; sort in JS.
**Server-side:** Pass `fieldName` and `direction` to Apex, let SOQL handle ordering.

### 6. Pagination Strategies

| Strategy | Pros | Cons |
|---|---|---|
| Client (slice array) | Simple | Loads all records, heavy for large sets |
| Server (LIMIT/OFFSET) | Scales to thousands | Requires state + query |
| Lazy Loading (virtual scroll) | Fast UI | More JS & Apex logic |

> **Tip:** Combine OFFSET with `ORDER BY Id` for stable pagination.

### 7. Row Actions

Handle in `onrowaction` to navigate or DML.

### 8. Inline Editing & Save Flow

1. Add `editable: true` to column
2. Listen for `onsave` → draft values
3. Perform DML via Apex
4. Use `refreshApex` to reload data

### 9. Mass Selection & Bulk Actions

- Enable row selection via data-table default checkbox
- Use `getSelectedRows()` to fetch selected

### 10. Integration Patterns

| Pattern | Wire | Imperative |
|---|---|---|
| Initial load | `@wire(getAccounts)` | — |
| Query by filter | — | `getAccounts({ industry })` |
| Inline save | — | `updateAccounts()` + `refreshApex` |

### 11. Performance Considerations

- Retrieve only necessary fields
- Use **lazy load** for lists > 2k rows
- Cache data client-side; invalidate on save
- Debounce user filters before Apex call

### 12. Accessibility & Styling

- Datatable is SLDS-compliant out of box
- Add `row-number-column` for easy scanning
- Use `cell-class` & `row-class` for conditional styling

### 13. Testing Datatables with Jest

- Mock `<lightning-datatable>` using `jest.mock()`
- Assert row actions dispatch custom events

### 14. Real-World Scenarios

1. **Pipeline Board:** Opportunities datatable with drag-to-stage quick edits
2. **Order Fulfillment:** Inline edit quantities + mass commit
3. **Support Console:** Lazy-loaded case list with row actions (View / Escalate)

### 15. Common Interview Questions

1. **How do you handle 10k rows in datatable?** → Server-side pagination or virtual scrolling
2. **Difference between inline editing and `lightning-record-edit-form`?**
3. **How to create a custom datatable type?** → Create cell LWC + typeAttributes

### 16. Best Practices

- Let Apex calculate totals; don't loop large data in JS
- Always show spinners during save calls
- Use `draftValues` pattern for inline edits
- Componentize large configs (columns in JSON)

> *"A datatable is more than rows and columns — it's a UI powerhouse when coupled with Apex and smart patterns."*

---

## Salesforce Integration – Deep Dive

### 1. Introduction to Salesforce Integration

Integration is the backbone of any enterprise-grade Salesforce implementation. It allows Salesforce to work in tandem with ERP systems, databases, payment gateways, and custom apps, enabling organizations to maintain a single source of truth while automating processes across systems.

**Why Integrate?**
- Avoid redundant data entry
- Real-time data availability
- Unified reporting
- Improved customer experience

### 2. Types of Integration

| Type | Description | Tools/Techniques |
|---|---|---|
| Data Integration | Synchronize or replicate data across systems | MuleSoft, ETL tools, External Objects |
| Process Integration | Trigger processes based on events or actions | Platform Events, Flow, Callouts |
| UI Integration | Seamlessly embed user interfaces | Lightning Out, Canvas, Web Components |
| Event-driven Integration | Decoupled, scalable communication using events | Platform Events, CDC, Pub/Sub API |
| Middleware Integration | Use of iPaaS or enterprise service buses (ESB) | MuleSoft, Dell Boomi, Informatica |

### 3. REST vs SOAP Web Services

| Feature | REST API | SOAP API |
|---|---|---|
| Format | JSON | XML |
| Ease of use | High | Moderate |
| Tooling | Postman, cURL | SoapUI, WSDL2Apex |
| Integration Style | Lightweight, Modern APIs | Legacy Systems |
| Use in Mobile Apps | Highly suitable | Not preferred |

### 4. Authentication & Authorization in Depth

**OAuth 2.0 Flows Explained**

| Flow | Use Case |
|---|---|
| Authorization Code | Browser-based apps with redirect URLs |
| JWT Bearer | Backend integrations using certificates |
| Username-Password | CLI, scripts (least secure) |
| Client Credentials | Server-to-server API access (secure & scalable) |

**Named Credentials in Practice**
- One-click endpoint and auth storage
- Prevents hardcoding sensitive info in code
- Supports multiple auth mechanisms (Basic, OAuth 2.0, JWT)
- Enables per-user vs named principal access

**Additional Security Controls**
- IP Restrictions & Login Hours
- Session Timeout Policies
- High Assurance Sessions
- Mutual TLS & Certificate Pinning

### 5. Integration Patterns with Examples

| Pattern | Description | Example Scenario |
|---|---|---|
| Remote Call-In | External system calls Salesforce API | Mobile app querying Case records |
| Request & Reply | Salesforce makes synchronous callout | Get real-time product availability from SAP |
| Fire & Forget | No response expected from API | Send SMS via third-party from Process Builder |
| Batch Data Sync | Scheduled/bulk data load | Nightly sync of orders from ERP |
| Event-Driven Integration | Event triggers communication | New Lead triggers webhook via Platform Event |

### 6. Monitoring & Logging Integrations

- Use Custom Logging Objects for tracking success/failure
- Enable Debug Logs for API users to trace issues
- Event Monitoring licenses help track API calls in real-time
- Named Credential Logs (Enable Debug Mode)
- Use External Monitoring Tools (New Relic, Datadog, Splunk)

### 7. Real-World Scenario-Based Interview Questions

#### Q1: You're building a two-way sync between Salesforce and an external inventory system. How would you ensure data consistency?
- Use Platform Events and Change Data Capture
- Employ external IDs and UPSERT logic
- Implement middleware to ensure transactional integrity

#### Q2: Your callout fails randomly due to API rate limits. How do you handle it?
- Implement exponential backoff
- Monitor headers for rate-limit info
- Use batch processing with retry capability

#### Q3: You want to expose a custom Apex REST API to your frontend app. How?
- Create an `@RestResource` class
- Use `@HttpGet`, `@HttpPost`, etc.
- Handle auth via Connected App + OAuth 2.0

#### Q4: You need a secure, scalable way for Salesforce to call an Azure function.
- Use Named Credential (with certificate)
- Use JWT Bearer flow for auth
- Enable Mutual TLS if required

#### Q5: You must call an external API, then perform a DML. What approach?
- Use `@future(callout=true)` or Queueable Apex
- Callout happens in async context; DML in a separate transaction

#### Q6: External API requires authentication via OAuth, but tokens expire every hour.
- Use Named Credential with auto-refresh token
- Monitor and refresh as needed

#### Q7: What happens if the endpoint URL changes?
- Store endpoint in Custom Metadata or Custom Setting
- Avoid hardcoding in Apex

#### Q8: Can Salesforce act as an Identity Provider?
- Yes. Use Salesforce Identity features with Auth Providers & SAML

#### Q9: How would you manage different API versions across environments?
- Use environment-specific Custom Metadata
- Externalize versions and use in logic

#### Q10: You're limited by Salesforce API governor limits. What's your strategy?
- Combine requests (Composite API)
- Use Bulk API for high-volume data
- Cache results when possible

### 8. Integration Tools and Technologies (Extended)

- **MuleSoft:** Native Salesforce integration platform
- **Salesforce Connect:** Access external data via External Objects (OData)
- **Heroku Connect:** Bi-directional sync between Salesforce and Heroku Postgres
- **Event Bus:** For asynchronous event-driven architecture
- **Pub/Sub API:** For streaming integration use cases

### 9. Conclusion

Salesforce integration is a broad and powerful domain that blends technical skill, architectural thinking, and business insight. Knowing how and when to integrate can make or break the reliability and scalability of your implementation.

> *"A good developer writes APIs. A great one integrates them with foresight."*

🎯 Master the fundamentals. Secure your endpoints. Automate wisely.

---

## Ultimate Salesforce Integration Guide (All-In-One)

### 🧠 1. What is Salesforce Integration?

Salesforce Integration is the process of connecting external systems to Salesforce to send and receive data using various mechanisms like APIs, events, or declarative tools. It enables seamless data flow and business process automation between Salesforce and other applications.

### 🔁 2. Integration Approaches

| Direction | Type | Description |
|---|---|---|
| **Inbound** | REST API | External system pushes JSON-based data into Salesforce. Modern and lightweight. |
| **Inbound** | SOAP API | External system pushes XML-based data into Salesforce, often used for legacy systems with WSDL contracts. |
| **Outbound** | Apex Callouts | Salesforce initiates sending data to external systems using various protocols. |
| **Outbound** | Platform Events | Real-time, push-based integration model where Salesforce publishes events that external systems can subscribe to. |
| **Bi-Directional** | Named Credentials + External Services | Provides secure, two-way API connectivity, often leveraging OpenAPI specifications for declarative integration via Flows. |
| **Streaming** | Change Data Capture (CDC) | Notifies external systems in real-time about data changes (create, update, delete, undelete) in Salesforce. |
| **Outbound** | Outbound Message | A declarative workflow rule-triggered SOAP message that sends data to a specified external endpoint. |

### 📦 3. Salesforce Integration Types

- **REST API:** Ideal for modern, lightweight, and flexible integrations using JSON-based data
- **SOAP API:** Best suited for legacy systems requiring WSDL contracts and XML-based data exchange
- **Apex Callouts:** Used when Salesforce needs to initiate communication and push data to external systems programmatically
- **Platform Events:** Enables real-time, event-driven integrations following a publish-subscribe model
- **Outbound Message:** A no-code option for sending a SOAP message based on a workflow rule
- **Named Credential:** Securely manages authentication and callout URLs
- **External Services:** Allows Salesforce to declaratively integrate with external APIs defined by OpenAPI specifications

### 🛠 4. Connected App + OAuth Setup

A **Connected App** in Salesforce is a framework that allows external applications to integrate with Salesforce using APIs and standard protocols like OAuth.

**Steps for Connected App Setup:**
1. Navigate to Setup → App Manager
2. Click **New Connected App**
3. Fill in Basic Information (Name, API Name, Contact Email)
4. Enable OAuth Settings
5. Set Callback URL and OAuth Scopes
6. Save and capture **Consumer Key (Client ID)** and **Consumer Secret (Client Secret)**

### 🔐 5. Named Credentials + Auth Provider + External Credentials

**Overview of Callout Process:**
1. Create a **Named Credential** (recommended for authentication)
2. Configure a **Remote Site Setting** (required if not using Named Credential)
3. Get **Access Token** from the token endpoint
4. Use **Access Token** to call the actual API
5. **Handle Response and Errors** properly
6. Set up **Apex Classes** for callouts

### 📑 6. JSON Serialization & Deserialization

**Typed Serialization** — Uses Apex wrapper classes for strong type checking.

**Untyped Serialization** — Uses `Map<String, Object>` and `List<Object>` for dynamic JSON structures.

### 🔨 7. Full REST API Endpoints – All Methods

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/services/apexrest/AccountTest` | Create Account + Contacts |
| GET | `/services/apexrest/AccountTest/{id}` | Retrieve Account |
| PUT | `/services/apexrest/AccountUpdate/{id}` | Update Account (Upsert) |
| PATCH | `/services/apexrest/AccountUpdate/{id}` | Update Partial Field |
| DELETE | `/services/apexrest/AccountUpdate/{id}` | Delete Account |

### 📤 8. Apex Callout to External API

**Where Can You Perform Callouts?**

| Context | Allowed? | Notes |
|---|---|---|
| Trigger (sync) | ❌ | Not allowed directly, must use future/queueable methods |
| Future Method | ✅ | Asynchronous, limited to one callout per transaction |
| Queueable Apex | ✅ | Preferred async option, more flexibility |
| Batch Apex | ✅ | In `execute()` method only |
| Schedulable Apex | ✅ | Good for recurring callouts |
| LWC | ✅ | Client-side callout via Apex methods |
| Flow/Process Builder | ✅ | Only via External Services/HTTP Callout |
| Apex Test Class | ✅ (mocked) | Use `HttpCalloutMock` for testing |

### 🧃 9. Platform Event (Push Model)

```apex
// Publish a platform event
Order_Event__e evt = new Order_Event__e(
  Order_Id__c='1234',
  Status__c='Shipped'
);
EventBus.publish(evt);
```

### 📎 10. File Upload (Base64)

REST endpoint in Salesforce to receive a file encoded in Base64 and create a ContentVersion record.

### 🧩 11. Flow + Apex Integration Pattern

Expose Apex methods to Salesforce Flows using the `@InvocableMethod` annotation.

### 📬 12. Postman Mock Examples

| Name | Method | Endpoint |
|---|---|---|
| Create Account Contact | POST | `/services/apexrest/AccountTest` |
| Update Account | PUT | `/services/apexrest/AccountUpdate/{id}` |
| Partial Update Account | PATCH | `/services/apexrest/AccountUpdate/{id}` |
| Delete Account | DELETE | `/services/apexrest/AccountUpdate/{id}` |
| Bulk Insert | POST | `/services/apexrest/BulkDataWebService` |
| File Upload | POST | `/services/apexrest/FileUploadService` |
| Push Event | POST | `/services/data/v59.0/sobjects/Event__e` |
| Zoho Token | POST | `https://accounts.zoho.com/oauth/v2/token` |

---

## Streaming API vs Pub/Sub API vs REST Hooks in Salesforce

### 1. Introduction: Why Real-Time Eventing Matters

Salesforce processes mission-critical data in many business workflows — from orders, cases, and leads to external integrations like ERP, analytics, and mobile platforms. Real-time communication mechanisms help deliver:

- ⚡ **Low-latency updates** (milliseconds to seconds)
- 🛠 **Reduced polling & batch inefficiencies**
- 📡 **External system reactivity** (e.g., data lakes, bots, services)
- 💬 **Bi-directional messaging** for true event-driven architecture (EDA)

### 2. Streaming API – Legacy but LWC-Friendly

**🔧 Protocol:** CometD (Bayeux over long polling)
- Based on HTTP + JSON
- Uses `replayId` for at-least-once delivery within 24 hrs
- Subscriptions maintained with `/meta/connect` and `/meta/subscribe`

**🧩 Supported Channel Types:**
1. **PushTopic:** Query-based event (SOQL with selected fields)
2. **Generic Events:** Lightweight, no schema enforcement
3. **Platform Events**
4. **Change Data Capture (CDC)**

**🔍 Replay IDs:**
- Persisted for **24 hours**
- Enable client resubscription after disconnect
- `ReplayId = -1` (new events), `-2` (latest), or specific value

✅ **Key Benefits:**
- Native browser support (via `lightning/empApi`)
- Simple to implement for UI-driven real-time needs
- Built-in filtering via PushTopics

⚠ **Challenges:**
- JSON over long polling = higher overhead
- Difficult to scale under high-volume multi-topic ingestion
- Requires reconnection logic & heartbeat checks

### 3. Pub/Sub API – Enterprise-Grade Eventing

**🚀 Protocol:** gRPC (HTTP/2 + Protocol Buffers)
- High-throughput, efficient binary streaming
- Bi-directional streams over single HTTP/2 connection

**🧩 Supported Event Types:**
- Platform Events
- Change Data Capture
- Generic Events (future roadmap)

**⚡ Advanced Capabilities:**
- Bi-directional messaging (subscribe + publish)
- Backpressure-aware delivery
- ReplayId-based resume
- Full acknowledgment model (ack after processing)

✅ **Ideal For:**
- Event-driven microservices & ETL systems
- Real-time processing pipelines (e.g., analytics, ML scoring)
- High-volume, multi-tenant subscriber models

⚠ **Limitations:**
- No LWC/browser support (gRPC unsupported)
- Higher setup complexity: gRPC clients, certificates, auth flows
- Requires Named Credential or JWT bearer flow for auth

### 4. REST Hooks – Webhook Simplicity

**🔔 Protocol:** HTTPS (POST callbacks)
- External system registers endpoint
- Salesforce sends a POST on record change

**🔧 Configuration:**
- Setup > Change Data Capture > Select Object
- Register webhook URL + verification token

✅ **Best For:**
- Light integrations (e.g., notify Zapier, Google Sheets)
- Systems without persistent connections
- Events with low frequency and simple retry models

⚠ **Considerations:**
- No ordering guarantee
- Retry logic must be implemented by consumer
- No replay/reconnect; missed events = lost

### 5. Feature Comparison: Deep View

| Feature | Streaming API | Pub/Sub API | REST Hooks |
|---|---|---|---|
| **Protocol** | HTTP + JSON (CometD) | HTTP/2 + Protobuf (gRPC) | HTTPS POST + JSON |
| **Transport** | Long polling | Persistent bidirectional | Stateless, async POST |
| **Direction** | Receive only | Receive + publish | Receive only |
| **Browser Support (LWC)** | ✅ Yes | ❌ No | ❌ No |
| **Backpressure Handling** | ❌ Manual | ✅ Built-in | ❌ None |
| **Replay Support** | ✅ ReplayId (24 hrs) | ✅ ReplayId (24 hrs) | ❌ |
| **Delivery Guarantee** | ✅ At least once | ✅ At least once | ❌ Best effort |
| **Complex Routing** | ❌ Limited | ✅ Stream multiplexing | ❌ Manual endpoint logic |
| **Authentication** | Session ID / OAuth | OAuth JWT / Named Creds | Bearer token |
| **Ideal Subscriber** | Browser UI, LWC | Microservices, Middleware | Webhooks / REST endpoints |

### 6. Choosing the Right Eventing Tool

| Scenario | Recommended Tool | Reason |
|---|---|---|
| Real-time UI update on Case escalation in LWC | **Streaming API** | Native LWC support via empApi |
| ETL sync from Salesforce to AWS Redshift | **Pub/Sub API** | Scalable, high throughput + binary |
| Send Slack message when Opportunity is won | **REST Hook** | Simple, fire-and-forget integration |
| Internal event-driven orchestrations | **Pub/Sub API** | Handles multiple topics with backpressure |
| Mobile app receives Account updates | **REST Hook / PubSub** | Depends on mobile platform |

### 7. Best Practices

- Prefer **Pub/Sub** for high-scale systems; **Streaming API** for UI triggers
- Always implement **replay logic** to avoid event loss (Streaming & Pub/Sub)
- Use **Named Credentials** or **JWT Auth Flow** for secure external access
- Avoid using **REST Hooks** for critical data unless you implement retries
- Audit event logs & volumes via **Event Monitoring** in Salesforce Shield

### 8. Interview Q&A

#### 1. What's the key difference between Streaming API and Pub/Sub API?
**A:** Streaming uses CometD over HTTP; Pub/Sub uses gRPC, is faster, supports publish + subscribe.

#### 2. When would you use REST Hook over Streaming?
**A:** For simple, non-critical notifications to REST endpoints that don't support persistent connection.

#### 3. How do you ensure no events are missed using Streaming API?
**A:** Track `replayId` and re-subscribe with offset. Consider using durable subscribers.

#### 4. Is Pub/Sub API supported in Lightning Web Components?
**A:** No. Pub/Sub is backend only due to lack of gRPC in browsers.

#### 5. Which API would you choose for scalable Kafka integration?
**A:** Pub/Sub API – binary transport, backpressure, replay, and native protocol for brokers.

### 9. Summary

| Use Case | Recommended API |
|---|---|
| Real-time UI updates | **Streaming API** |
| High-volume backend sync | **Pub/Sub API** |
| Quick alert via REST POST | **REST Hook** |
| LWC component push notifications | **Streaming API** |
| Microservices event backbone | **Pub/Sub API** |

> *"In event-driven design, your integration is only as good as your pipe. Choose wisely."*

---

<p align="center">
  <strong>Salesforce Master Class</strong> — Your ultimate guide to mastering Salesforce development.<br>
  <a href="https://salesforcemasterclass.netlify.app/">🔗 Visit the Portal</a>
</p>
