# PARCELMATE AI — MASTER PROTOTYPE IMPROVEMENT, DEBUGGING & VALIDATION PROMPT
## Hackerdorm Track 2: Agentic Solution | WorkBuddy AI

## 1. ROLE AND DIRECTIVE

Act as a senior full-stack developer, agentic AI architect, UX/UI designer, database engineer, and QA tester. You are working on an **existing ParcelMate AI prototype** in the current workspace.

Inspect the existing application, validate what already works, improve the current implementation, fix defects, run the application, test its real workflows, and repeat the inspect–implement–test–fix cycle until the core demo is verified or a genuine blocker remains.

**Do not create a new project unnecessarily. Do not return only a proposal, snippets, or a static mockup. Make changes directly in the existing workspace. Preserve working features and the existing stack where practical.** Do not ask for approval for ordinary implementation choices.

The core experience must be:

**Owner enters parcel details once → WorkBuddy coordinates the deposit workflow → Student authenticates with a numeric pickup PIN → Any overdue fee is handled → Collection is committed → The system verifies and reports the final outcome.**

Clearly distinguish real software operations from simulated hardware, notifications, and payments. Never claim an action succeeded without verifying its actual result.

---

## 2. INSPECT, RESEARCH, AND VALIDATE BEFORE CHANGING THINGS

Before modifying the code:

1. Inspect project structure, framework, dependencies, run scripts, existing components, routing, data model, persistence layer, APIs/backend functions, WorkBuddy setup, notification integrations, styles, seed data, and tests.
2. Run the existing app and tests if possible. Record startup/build/runtime errors and identify which workflows currently work.
3. Check actual WorkBuddy documentation and available tools for agent orchestration, tool calls, database access, notifications, background jobs, and logs. Do not assume unavailable capabilities exist.
4. Identify root causes of the issues listed below. Do not assume features are missing before inspecting them.
5. Present a short implementation plan, then continue immediately into implementation without waiting for approval.
6. Prefer small, testable changes over an unnecessary rewrite or new infrastructure.

Use the following development loop throughout:

**Inspect → Implement → Run → Test → Observe → Diagnose → Fix → Retest.**

After each major change, run the relevant checks. At the end, run the full supported regression suite and perform a complete deposit-to-collection test through the actual application interface. Do not claim to have run tests, inspected a browser, or verified a feature unless you actually did so. Do not disable failing tests to obtain a green result.

If browser automation is available, use it to inspect laptop and kiosk-sized layouts. Otherwise use the strongest available combination of run checks, automated tests, and manual smoke tests. If blocked by missing credentials, integrations, packages, or hardware, report the specific limitation and use a clearly labelled safe simulation where practical.

---

## 3. PRODUCT AND HACKATHON GOAL

ParcelMate AI is an autonomous campus parcel-management and collection system intended to reduce repetitive parcel registration, locker assignment, recipient notification, deadline monitoring, fee calculation, and collection-record work.

The owner should enter the information the system cannot know. The agent should coordinate the routine downstream tasks. The student should identify the parcel by tracking number and authenticate collection with a separate numeric PIN.

Demonstrate the six Track 2 capabilities:

1. **Understanding the goal:** interpret a parcel registration or collection request.
2. **Planning tasks:** create a relevant multi-step execution plan.
3. **Executing actions:** call actual application functions/tools.
4. **Collaborating and validating:** inspect tool results and adapt to errors or changing conditions.
5. **Integrating results:** keep parcel, locker, credential, fee, payment, and notification records consistent.
6. **Delivering the final outcome:** verify the final state and give a truthful completion report.

One primary WorkBuddy-powered agent coordinating reliable application tools is sufficient. Do not add multiple agents just for appearance.

---

## 4. DESIGN DIRECTION: DISTINCTIVE, WARM, NEUTRAL, AND FUNCTIONAL

Improve the existing design so it looks like a polished, distinctive campus service—not a generic dashboard, a plain form, or a flashy sci-fi UI.

### Visual style

Use a restrained and coherent palette such as:

- Warm ivory or soft off-white backgrounds.
- Charcoal or deep slate text.
- Muted sage or sea-glass green for successful states.
- Dusty terracotta or soft peach for secondary accents.
- Muted lavender or warm sand as an occasional supporting colour.
- Soft grey borders and subtle shadows.

Use a limited subset of colours consistently. Keep typography legible, spacing intentional, cards restrained, and button labels explicit. Avoid neon cyan, highly saturated gradients, glowing outlines, excessive motion, clutter, and decorative elements that do not help the user complete a task.

A bright red accent is specifically required for the pickup PIN on the authorised Deposit Confirmed receipt, and the PIN should be especially noticeable in the demo notification inbox. Use red selectively for the credential only; do not turn the entire interface into a high-alert design.

### Centered laptop layout

Across **all sections**, center the main content within a consistent responsive container using sensible maximum widths and automatic horizontal margins. On a typical laptop screen, avoid anchoring the whole interface to the far left or stretching cards edge-to-edge with large empty areas.

- Align page headings, forms, locker grids, detail panels, and activity content to a consistent centered grid.
- Use a balanced two-column layout where it improves the workflow.
- Prevent horizontal overflow, clipped controls, and uneven card heights.
- Keep key actions visible without unnecessary scrolling at a typical laptop viewport around 1366 × 768 where content permits.
- At narrower sizes, stack columns in a logical order and keep all controls accessible. Do not hide required fields or clip content merely to avoid scrolling.
- Keep the numeric kiosk keypad centered and touch-friendly.

### Main visual identity: the Campus Locker Wall

Make the functional 24-compartment locker wall the visual centrepiece of the main operations view. The wall must reflect actual locker records, not decorative hard-coded tiles. Group lockers by size and make each compartment selectable to display its current state and authorised details.

Do not use an oversized conventional dashboard. Do not display a **Total Parcels** statistic or summary tile on the main overview. Removing that card must not remove parcel registration, search, history, or underlying parcel data.

Avoid unnecessary pages, charts, sidebars, oversized headings, and redundant cards. Buttons must perform the action their label promises.

---

## 5. NAVIGATION AND FOCUSED SCREENS

Keep the product compact, with these logical areas (routes, tabs, or panels may follow the existing architecture):

1. **Locker Wall / Operations:** the interactive 24-locker wall, compact availability information, and clear actions to register a parcel or open the kiosk.
2. **Register Parcel:** short owner form plus the Deposit Confirmed receipt.
3. **Student Lookup:** tracking-number lookup and demo notification inbox.
4. **Kiosk Collection:** numeric PIN keypad, fee review, penalty-payment QR when applicable, and simulated unlock/collection.
5. **Activity / History:** concise agent execution history, audit events, notification outcomes, and exceptions.
6. **ParcelMate Help:** a compact contextual FAQ assistant available without creating a separate analytics dashboard.

Do not create a login screen. Use fictional demo data and visibly label demo/simulated functions.

---

## 6. EXACTLY 24 FUNCTIONAL LOCKER RECORDS

The demo inventory must contain exactly 24 authoritative locker records:

| Group | IDs | Count |
|---|---|---:|
| Small | S01–S08 | 8 |
| Medium | M01–M08 | 8 |
| Large | L01–L08 | 8 |
| **Total** | | **24** |

Each record needs a unique locker ID, supported size, status, assigned parcel ID if applicable, and updated timestamp. Use these states:

- `EMPTY`
- `RESERVED`
- `OCCUPIED`
- `OUT_OF_SERVICE`

The normal state sequence is `EMPTY → RESERVED → OCCUPIED → EMPTY`. If deposit fails before confirmation, release the reservation safely. PIN expiry or notification failure must never automatically mark an occupied compartment empty.

Seed a realistic mixture of available, occupied, and out-of-service lockers. Make the seed parcel records consistent with locker occupancy. The wall must be generated from and update against the same source of truth used by the deposit and collection workflows.

### Allocation rules

When a parcel is submitted:

1. Read the latest locker inventory.
2. Match the parcel size to compatible compartments.
3. Exclude reserved, occupied, and out-of-service compartments.
4. Choose an appropriate compartment using a deterministic policy (for example, the first compatible available locker in configured order).
5. Reserve it using a concurrency-safe operation.
6. Verify the reservation.

Never guess that a locker is available, assign the same locker twice, or assign an incompatible size. If no compatible locker is available, explain the problem and stop safely without leaving a partial or inconsistent parcel record.

---

## 7. REGISTER PARCEL FORM

Keep the owner form short, centered, and easy to complete.

### Required inputs

- **Tracking Number** — alphanumeric characters and permitted separators; not numeric-only.
- **Recipient Phone Number** — a fictional number for demo data, validated appropriately.
- **Parcel Size** — Small, Medium, or Large.

A recipient name may be retained as an optional or existing field only if the current workflow genuinely needs it. Do not use Student ID as a required identifier. The student-facing lookup must use the tracking number, not student-name search.

### Explicit form changes

- Completely remove the **Delivery Note** field from the interface, validation, form payload, persistence, receipt, and notification workflow if those elements exist solely for delivery notes. Do not leave a blank gap after removal. Safely handle any old stored field without deleting unrelated data.
- Use **Tracking Number** wherever the user-facing label currently says **Parcel Reference**.
- Remove the Student ID field.
- Do not ask the owner to enter the locker ID, PIN, payment QR, deposit time, expiry, fee, or collection time. These are assigned or calculated by the application.
- Configure the owner's notification phone once in demo settings or seeded configuration. Do not ask for it on every parcel.
- Keep recipient and owner notification destinations distinct.
- Remove excessive top padding and whitespace. Keep the main required inputs and Register Parcel button visible together at a typical laptop viewport where possible, without shrinking them excessively or hiding required content.

Validate all required fields and check tracking-number uniqueness before allocating a locker. Do not fabricate missing recipient data.

---

## 8. DEPOSIT WORKFLOW AND DEPOSIT CONFIRMED RECEIPT

After the owner submits the form, the WorkBuddy agent should coordinate the deposit workflow:

1. Understand the goal and create a relevant plan.
2. Validate tracking number, phone number, parcel size, and duplicate status.
3. Retrieve locker inventory and find a compatible compartment.
4. Reserve the compartment safely.
5. Generate a unique, unpredictable six-digit numeric pickup PIN.
6. Guide the operator to place the parcel in the assigned compartment.
7. In the simulation, require an explicit **Confirm Simulated Deposit** action. With real hardware, use actual sensor/controller confirmation.
8. Only after confirmed placement, persist the deposit timestamp and commit the parcel as `READY_FOR_COLLECTION` and the locker as `OCCUPIED`.
9. Start the free-collection timer from the confirmed deposit timestamp.
10. Record the credential expiry and notification outcomes.
11. Notify the student and owner through configured services, or through a clearly labelled persistent simulation.
12. Retrieve the saved records and verify the critical final state.

Do not show a success state merely because the owner clicked Submit. The parcel and locker records must be committed and the critical checks must pass.

### Deposit Confirmed layout

On the Register Parcel screen, use a centered, balanced two-column laptop layout:

- **Left column:** registration form or a compact completed-form summary.
- **Top-right column:** the Deposit Confirmed receipt tile, aligned next to the first/main tile rather than placed far below it.

Before a successful deposit, the right column may show a restrained placeholder such as “Your receipt will appear here after a successful deposit.” After success, replace it with the actual receipt. This should reduce negative space and eliminate unnecessary scrolling.

On smaller screens, stack the form and receipt naturally. Do not overlap or clip them.

### Receipt contents

Show the following data only after it has been saved and verified:

- Clear **Deposit Confirmed** heading/status.
- Tracking number.
- Assigned locker ID and size.
- Confirmed deposit timestamp.
- Six-digit pickup PIN.
- PIN expiry date/time.
- Student notification status.
- Owner notification status.
- Clear next action, such as “View Student Lookup” or “Register Another Parcel”.

### Pickup PIN emphasis

Make the six-digit PIN the most noticeable item on the receipt:

- Use a selectively bright red or strong red-accent text/container.
- Use large, bold, highly legible digits with clear spacing.
- Label it **PICKUP PIN**.
- Add a functional Copy PIN button if clipboard support is available. If not, omit the button rather than displaying an inert control.
- Keep the PIN as a string so leading zeroes are preserved.
- Do not display any pickup QR code on this receipt.

### Hide verification test feedback from the receipt

Do not show internal verification-test output, raw tool responses, debug logs, assertion text, or developer-oriented test feedback inside the customer-facing Deposit Confirmed receipt. Keep necessary validation and verification running in the backend and retain accurate results in the Activity/Audit view for judging and debugging. The receipt should show the clear business outcome and relevant notification status, not internal diagnostic chatter.

### QR restriction

**Do not display a pickup QR code on the Deposit Confirmed receipt. Do not create a pickup-QR collection flow in this version.** The only QR code requested in this improvement is for the overdue-penalty payment flow described in Section 11.

---

## 9. PICKUP PIN GENERATION, SECURITY, AND EXPIRY

Generate a unique, unpredictable six-digit numeric PIN with a cryptographically secure random generator where supported. Treat it as a string to preserve leading zeroes. Never derive it from the tracking number, use a sequential counter, or reuse an active PIN for another parcel.

Associate the PIN with exactly one parcel. Store a protected hash/reference where practical and log no plaintext PIN in general audit logs. For the authorised demo workflow, the PIN may appear on the post-deposit owner receipt and the corresponding fictional demo notification.

Set the demo default PIN expiry to 24 hours after confirmed deposit unless a different configured existing policy is already required. Keep expiry separate from the one-hour fee period.

Reject invalid, expired, revoked, and already-used PINs. PIN expiry must not delete a parcel, reset its deposit timestamp, mark it collected, or free its locker. Where supported, allow the owner to reissue an expired PIN through an authorised workflow that revokes the old PIN and records the action.

Tracking number identifies a parcel; the PIN authorises collection. Knowing the tracking number alone must not reveal a protected PIN outside the clearly labelled fictional demo notification flow. Do not present demo lookup as production-grade identity verification.

---

## 10. STUDENT LOOKUP AND DEMO NOTIFICATION INBOX

Use **Tracking Number** as the student lookup field. Do not use student name or Student ID as the identifying field.

A tracking-number lookup may show appropriate parcel status and instructions, but it must not by itself expose the protected PIN.

Provide a clearly labelled **Demo Notification Inbox** with fictional seed messages so the judge can retrieve the pickup message during the demo without real SMS delivery.

### Pickup PIN prominence in the inbox

Make the PIN easy to spot in each authorised pickup message:

- Place it in a dedicated high-contrast PIN block within the message.
- Use large, bold digits and the explicit label **PICKUP PIN**.
- Use a restrained red accent consistent with the deposit receipt.
- Keep other notification text visually secondary.
- Provide a working Copy PIN action if supported.
- Clearly distinguish the PIN from the tracking number, fee, and payment QR.

Only show the PIN in the intended fictional demo message or other appropriately authorised context. Never reveal it in general parcel search results merely because a tracking number was entered.

The notification should include parcel tracking number, assigned locker, PIN, deposit time, expiry, the one-hour free period, the fee policy, and collection instructions. For real integrations, only report `SENT` when the service returns evidence of sending; otherwise use `SIMULATED` or `FAILED` accurately.

---

## 11. KIOSK COLLECTION AND DUITNOW-STYLE PENALTY QR

The kiosk must provide a numeric-only touchscreen keypad for entering the six-digit pickup PIN. Include digits 0–9, `CLEAR`, `DELETE`, a masked entry display, and a submit button. Reject alphabetic and non-numeric input and preserve leading zeroes.

After submission:

1. Validate the PIN in application logic.
2. Check expiry/revocation, parcel status, assigned locker, and locker state.
3. Reject invalid or ineligible requests without changing records.
4. Retrieve the actual deposit timestamp and calculate the fee deterministically.
5. Display the fee and policy clearly.

### If there is no penalty

If the assessed fee is RM0, do not display a payment QR or payment panel. Proceed to the normal authorised collection step.

### If there is an overdue penalty

If the fee is greater than RM0, display a dedicated **Pay Overdue Fee** section showing:

- Tracking number or safe parcel reference.
- Exact assessed fee in RM.
- Calculation breakdown.
- Payment status.
- **DuitNow QR** payment method, when supported.

### Payment integration and simulation safety

The prototype must not process real money by default.

- If an approved, configured payment provider is already available, use its genuine provider-generated payment QR and validate the provider's result before marking payment complete.
- If no real payment integration exists, display a clearly labelled **DEMO — SIMULATED DUITNOW PAYMENT — NOT A REAL PAYMENT** experience. The QR may encode an internal demo payment reference or open a local simulated payment confirmation screen, but it must not pretend to be a real transferable bank-payment QR.
- Never invent a real DuitNow payment payload, merchant identifier, bank destination, transaction ID, or successful payment result.
- A simulated payment is only complete after the user performs the explicit demo confirmation action and the application records the simulated result.
- Keep assessed fee separate from payment status.
- Do not claim real funds were transferred.

Follow the existing configured collection policy for whether payment is required before unlock. Do not silently introduce a new fee-enforcement rule. If the prototype requires a fee to be marked paid before proceeding, enforce that consistently and explain the status clearly.

After the payment step, revalidate collection eligibility before authorising the unlock. Do not let payment confirmation bypass the PIN or parcel-status validation.

### Unlock and collect

Only after eligibility and any required demo payment step succeed:

1. Authorise the correct locker unlock operation.
2. Simulate the unlock unless compatible hardware is genuinely connected.
3. Ask the student to confirm simulated parcel removal.
4. Commit the collection transaction.
5. Mark the parcel `COLLECTED` and save the collection timestamp.
6. Revoke the pickup PIN.
7. Mark the assigned locker `EMPTY` and clear its active assignment, preserving historical records.
8. Write the audit event and update the locker wall.
9. Notify student and owner through the configured integration or clearly labelled simulation.
10. Retrieve the updated records and verify the final state before showing success.

Do not release an occupied locker before collection is committed. Prevent duplicate collection and concurrent double-release. If a critical operation fails, report the failure honestly and preserve consistent data. Do not claim physical unlock or parcel removal was detected when it was only simulated.

---

## 12. LATE-FEE POLICY

The first **60 minutes after confirmed deposit** are free. After the free period, charge **RM1 per additional started hour**.

Use deterministic application code, not an LLM estimate:

```text
elapsed_ms = current_time_ms - confirmed_deposit_time_ms
chargeable_hours = ceil(max(0, elapsed_ms - 3_600_000) / 3_600_000)
fee_rm = chargeable_hours * 1
```

Expected results:

| Elapsed time | Fee |
|---|---:|
| 45 minutes | RM0 |
| 60 minutes | RM0 |
| 61 minutes | RM1 |
| 120 minutes | RM1 |
| 121 minutes | RM2 |
| 180 minutes | RM2 |
| 181 minutes | RM3 |

Use the original confirmed deposit timestamp, not PIN expiry, as the starting point. Store timestamps consistently, preferably UTC; display Malaysian time using `Asia/Kuching`.

Show the free-period end, elapsed time, chargeable hours, rate, assessed total, and payment status. Recalculate fee from the authoritative timestamp whenever requested. Do not use browser countdown state as the source of truth. If background scheduling is unavailable, do not claim that an inactive app is continuously monitoring parcels.

---

## 13. EDIT AND DELETE/CANCEL REGISTERED PARCELS

Add owner-facing **Edit** and **Delete/Cancel** actions for registered parcels. Locate records by tracking number and apply state-aware validation.

### Edit rules

- Allow permitted details, such as the recipient phone number, to be edited when safe.
- Validate edits and preserve tracking-number uniqueness.
- If a parcel size changes while a locker is reserved but before confirmed deposit, verify a compatible replacement and safely update assignments.
- After confirmed deposit, do not silently alter fields that could invalidate the active locker/PIN transaction. Use a validated correction flow or restrict those fields.
- Do not silently rewrite completed collection records.
- Write an audit event for important changes.
- Provide Save Changes and Cancel controls and show success only after persistent update and verification.

### Delete/cancel rules

- Before deposit: allow cancellation/deletion according to the existing state model while preserving appropriate audit history.
- Reserved but not deposited: release the reservation safely when cancellation succeeds.
- After confirmed deposit: do not delete the record or free its occupied locker merely because the owner clicked Delete. Require a safe withdrawal/cancellation process and confirmation that the parcel has been removed; only then may the locker be released.
- After collection: preserve the completed transaction history. Do not offer a normal destructive delete that erases evidence of collection.

Use a confirmation dialog explaining the action's impact. Ensure the locker wall, parcel lookup, notifications where relevant, and history refresh after successful changes. Do not leave dangling assignments or active PINs after cancellation.

---

## 14. BUILT-IN PARCELMATE HELP FAQ ASSISTANT

Include a compact AI FAQ assistant named **ParcelMate Help**. It should feel like support for a finished parcel service, not an explanation of past prototype edits or development decisions. Do not mention prior interface changes in FAQs.

Provide a restrained floating help button (for example, `Need Help?`) and a compact chat panel. Show a few suggested questions, a text input, send button, loading/error states, and a clear close action. Match the warm-neutral visual style. Do not create another dashboard or interfere with the numeric PIN keypad.

Suggested questions and grounded knowledge categories:

**General:** What is ParcelMate AI? How does parcel collection work? How do I use the kiosk? What can the assistant help me with?

**Registration:** How do I register a parcel? What information is required? What is a tracking number? How can I tell registration succeeded? Can I update or cancel a registration?

**Collection:** How do I find my pickup instructions? How do I collect my parcel? Where can I find my pickup PIN? What happens after collection?

**Security:** What if my PIN is incorrect or expired? Can I reuse my PIN? Can someone else collect my parcel? How does the system protect my parcel?

**Lockers:** How is a locker selected? What do locker statuses mean? What if no suitable locker is available? Why is the locker still occupied after a PIN expires?

**Fees and payment:** How long is the free period? How are fees calculated? What is the fee after 61, 120, or 121 minutes? How do I pay an overdue fee? What if I think the fee is wrong?

**Notifications:** How will I know my parcel is ready? What if I do not receive a message? Does the owner receive a collection update?

**Troubleshooting:** Why can’t I find my tracking number? Why is my PIN not working? What if the locker cannot open? What if the collection process fails?

**Owner support:** How do I check available lockers, overdue parcels, notification results, or failed operations?

### FAQ behaviour

- Answer general questions from a structured FAQ knowledge base or equivalent grounded retrieval mechanism.
- Use actual application rules as the source of truth.
- If authorised and technically supported, call parcel-status or fee tools for questions about a specific record.
- Use deterministic tools for fees, locker availability, credential status, payment status, and parcel state.
- Keep answers concise and friendly.
- Ask for clarification only when necessary.
- Admit when information is unavailable; never fabricate data.
- Route supported requests to the real application workflow. Do not claim a registration, payment, unlock, or collection occurred merely because it was discussed in chat.
- Never reveal a protected PIN based only on a tracking number, expose private details without authorisation, bypass validation, or change records from an unauthorised chat command.
- Distinguish actual notification delivery and payment outcomes from simulations.

Keep the FAQ knowledge base synchronized with the final implemented policy and available capabilities. Test both suggested questions and free-text questions.

---

## 15. WORKBUDDY AGENT ORCHESTRATION AND AUDIT

Preserve and improve actual WorkBuddy orchestration where supported. Create or reuse application tools equivalent to:

- `register_parcel`
- `validate_parcel_information`
- `check_duplicate_tracking_number`
- `list_available_lockers`
- `find_compatible_locker`
- `assign_locker`
- `confirm_deposit`
- `generate_pickup_code`
- `get_parcel_status`
- `calculate_late_fee`
- `validate_pickup_code`
- `check_collection_eligibility`
- `create_payment_reference_or_demo_payment`
- `verify_payment_status`
- `authorise_locker_unlock`
- `collect_parcel`
- `release_locker`
- `send_notification`
- `write_audit_event`
- `verify_final_state`

Adapt names to the current project. Tools must execute actual validated operations and return structured results. The agent must inspect results, adapt to unavailable lockers or failed operations, and verify state before reporting success.

Display a compact Activity timeline that records real events, statuses, timestamps and validation results. Do not show raw verification-test output on the Deposit Confirmed receipt; retain useful internal verification details in Activity/Audit for debugging and judging. Avoid logging secrets, plaintext PINs, or unnecessary personal information.

If genuine WorkBuddy tool orchestration is unavailable, implement an honest local orchestration layer where feasible and clearly disclose that it does not fully meet the live WorkBuddy integration requirement.

---

## 16. DATA MODEL AND PERSISTENCE

Use the existing persistent database or the simplest supported data store that survives refreshes/restarts. Do not rely solely on frontend state.

Keep records for:

- **Parcels:** parcel ID, tracking number, recipient phone reference, optional recipient name if used, parcel size, locker assignment, confirmed deposit timestamp, status, PIN expiry reference, collection timestamp, fee assessment, and relevant timestamps.
- **Lockers:** locker ID, size, status, assigned parcel ID, updated timestamp.
- **Pickup credentials:** credential ID, parcel ID, protected PIN hash/reference, issue time, expiry, revocation, and status.
- **Payments:** payment reference, parcel ID, assessed amount, payment mode (`SIMULATED` or configured provider), status, timestamps, and provider reference if truly available.
- **Notifications:** parcel ID, destination type, message type, attempt timestamp, outcome, and error details.
- **Audit events:** parcel ID, locker ID, event type, timestamp, outcome, and non-sensitive details.

Keep referential integrity and uniqueness constraints wherever supported. Avoid storing unnecessary personal information. Related state changes should use transactions or equivalent concurrency-safe controls. Seed data must be internally consistent.

---

## 17. ERROR HANDLING AND RELIABILITY

Handle at least:

- Missing or invalid form details.
- Duplicate tracking number.
- No compatible available locker.
- Locker becoming unavailable during allocation.
- Failed deposit confirmation or database write.
- Invalid, expired, revoked, or reused PIN.
- Already-collected parcel.
- Fee-calculation failure.
- Simulated payment cancellation or failure.
- Notification failure.
- Inconsistent parcel and locker state.
- Edit conflicts and cancellation of occupied parcels.
- Repeated invalid PIN attempts.
- Duplicate/concurrent deposit or collection requests.

Retry only when safe and idempotent. Do not silently change security rules or free a locker without verified removal. When a critical state cannot be established, stop the affected workflow, preserve records, and flag an exception for owner attention.

All users should receive actionable but non-sensitive error messages. The prototype must use fictional student data and must not expose API keys, secrets, PINs in logs, or unnecessary personal information.

---

## 18. REQUIRED TESTS

Use the existing testing setup where possible. Run tests before and after changes. Add or update tests for:

1. Application startup and build.
2. Exactly 24 authoritative locker records and eight per size group.
3. Seed occupancy consistency.
4. Valid registration and correct locker allocation.
5. Missing details and duplicate tracking numbers.
6. No compatible locker and no double assignment.
7. Delivery Note field fully removed from the user workflow.
8. Deposit receipt appears top-right next to the form on laptop layout, with a placeholder before success.
9. Deposit receipt contains no pickup QR and no raw verification-test feedback.
10. Deposit receipt prominently highlights the six-digit pickup PIN in red.
11. Deposit timer starts only after confirmed placement.
12. Student notification inbox makes the pickup PIN easy to notice while keeping it out of unauthorised search results.
13. Six-digit PIN generation, leading zero preservation, validation, expiry, revocation, and single use.
14. Numeric-only kiosk input and tracking-number field remains alphanumeric.
15. Fee results at 45, 60, 61, 120, 121, 180, and 181 minutes.
16. No payment QR when fee is RM0.
17. A penalty greater than RM0 shows the appropriate DuitNow payment method or safe labelled demo simulation.
18. A simulated QR does not claim to transfer real money; payment status changes only after explicit demo confirmation.
19. Successful collection updates the parcel, revokes the PIN, releases the correct locker, writes audit history and verifies final state.
20. Duplicate collection is rejected.
21. Edit changes persist and preserve uniqueness/integrity.
22. Cancellation before deposit safely releases a reservation.
23. Cancellation after deposit does not empty a locker without verified removal.
24. Completed collection history cannot be destructively removed through ordinary delete.
25. Owner and recipient notification routing remains separate and outcomes are accurately labelled.
26. FAQ suggested questions and custom questions produce grounded answers.
27. FAQ cannot reveal protected PINs or claim unexecuted actions succeeded.
28. All main sections are centered and balanced at laptop and narrow viewports; no clipping or unintended horizontal overflow.
29. Buttons, Copy PIN, edit/cancel, fee payment simulation, and navigation perform their labelled actions.
30. Data persists after refresh/restart where the selected data store promises persistence.
31. WorkBuddy execution timeline reflects real tool calls and verified outcomes where supported.

Run each relevant test and record actual outcomes. Do not mark a test passed just because test code exists. Do not skip existing tests or remove assertions to hide failures.

---

## 19. FINAL END-TO-END DEMONSTRATION

After the changes, launch the application and test through the actual UI when possible.

### Deposit

1. Open Register Parcel.
2. Confirm Delivery Note is absent.
3. Enter a fictional tracking number, recipient phone number and parcel size.
4. Submit and observe the actual WorkBuddy/tool execution.
5. Confirm compatible locker reservation.
6. Confirm simulated deposit.
7. Verify the timer starts only after confirmation.
8. Verify a unique numeric PIN is generated and stored.
9. Confirm the centered two-column laptop layout places the Deposit Confirmed receipt at the top-right beside the form.
10. Confirm there is no pickup QR or raw verification-test feedback in the receipt.
11. Confirm the PIN is prominent in bright red and notification outcomes are accurate.

### Student lookup

1. Search by tracking number.
2. Verify the lookup alone does not disclose the PIN.
3. Open the fictional Demo Notification Inbox.
4. Confirm the PIN is visually prominent and easy to copy.

### Collection and payment

1. Enter the six-digit PIN using the numeric keypad.
2. Verify the credential against the stored record.
3. Advance a controlled test clock or use a fixture to create an overdue case.
4. Confirm the correct fee is calculated.
5. If fee is RM0, verify no payment QR appears.
6. If fee is greater than RM0, verify the DuitNow method or clearly labelled simulated payment panel appears.
7. Complete the demo payment action if required by the configured policy.
8. Simulate unlock and removal, then complete collection.
9. Verify parcel `COLLECTED`, locker `EMPTY`, credential revoked, audit entry present, notifications recorded, and final state re-read.

### Edit/delete and FAQ

1. Edit a permitted parcel detail and verify persistence.
2. Cancel a parcel before deposit and verify safe reservation release.
3. Verify an occupied parcel cannot be deleted in a way that frees its locker without removal confirmation.
4. Ask several suggested and custom FAQ questions and verify answers match the application.

After any fix, rerun the affected tests and the core regression suite. If an actual browser or external service is unavailable, clearly state which checks could not be performed and test everything else feasible.

---

## 20. FINAL REPORT AND DELIVERABLES

At the end, provide:

- Summary of the existing problems discovered.
- Changes actually implemented.
- Confirmation of exactly 24 locker records.
- Confirmation of the revised form, receipt placement, receipt QR removal, red PIN emphasis, hidden internal verification feedback, notification-inbox PIN emphasis, and centered laptop layouts.
- Edit/delete/cancellation rules and tested behaviour.
- FAQ assistant capabilities and privacy limits.
- DuitNow QR/payment implementation status: genuine configured integration or labelled demo simulation.
- WorkBuddy tools actually executed and how final state was verified.
- Tests executed with actual pass/fail results.
- Build/startup status and any blockers.
- Run instructions and known limitations.

Do not claim the app, WorkBuddy integration, notification delivery, payment, physical lock, or tests are working unless that was verified.

## FINAL NON-NEGOTIABLE DIRECTIVE

Inspect the existing project first, validate actual capabilities, make targeted improvements, run the app, test the workflows, diagnose failures, fix them and retest.

The product must remain a focused campus parcel kiosk with a distinctive centered 24-locker wall—not a generic dashboard. The owner enters parcel information once; the agent coordinates the routine workflow; students use a tracking number to find their parcel and a numeric PIN to collect it; a DuitNow-style QR appears only for a non-zero penalty and must be honestly simulated unless a genuine approved payment integration is configured; and all successful outcomes must be verified from actual application records.


---

# 21. LATEST REQUIRED CHANGES — TRACKING-SPECIFIC PIN DISPLAY AND PARCEL MANAGEMENT

**This section is the latest amendment to the master prompt. Where an earlier section conflicts with the specific behaviours below, follow this section for these three features while preserving all other requirements.** Implement these changes in the existing application, connect them to the same authoritative records, and test them. Do not create static placeholders or duplicate disconnected state.

## 21.1 Student Lookup: Filter the Demo Notification Inbox by the Entered Tracking Number

Update the Student Lookup flow so that after a user enters a tracking number and performs the lookup, the **Demo Notification Inbox displays only the pickup notification and pickup PIN associated with that exact tracking number**.

Required behaviour:

1. The student enters a tracking number and submits the lookup.
2. Normalize the submitted value safely for matching (trim accidental leading/trailing whitespace and apply the application's established case policy; do not alter meaningful characters).
3. Retrieve or filter notification records using the matching parcel/tracking-number relationship in the application data source.
4. Display only the notification(s) that belong to the matched tracking number. Most importantly, display only the pickup PIN belonging to that parcel; never show PINs from other seeded/demo parcels in the same result.
5. Make the matching PIN easy to notice using a dedicated high-contrast block labelled **PICKUP PIN**, large bold digits, and the restrained red accent used on the deposit receipt. Preserve leading zeroes and provide a working Copy PIN action if clipboard support exists.
6. Keep the tracking number visually distinct from the PIN. Make it clear that the tracking number is used to find the parcel and the PIN is used to authorise collection.
7. If no parcel matches, display a helpful “No parcel found for this tracking number” state and show no pickup PIN.
8. If the parcel matches but no valid pickup notification/PIN is available, explain that the pickup information is unavailable or expired and provide the correct next step. Do not fall back to showing every notification.
9. Clear or replace the previous result whenever another tracking number is submitted. Do not leave the previous parcel's PIN visible while displaying a different lookup result.
10. While the search is empty or has not been submitted, do not show a list of all pickup PINs by default.

This is a clearly labelled **demo notification inbox**. The prototype may reveal the matching fictional demo PIN after tracking-number lookup to support the hackathon demonstration. Do not present this as strong production authentication: a production service would need recipient verification before releasing a secret pickup credential. Do not disclose PINs in general search results, global activity logs, or unrelated locker details outside the authorised owner/operations view described below.

## 21.2 Register Parcel: Add Edit and Delete/Cancel Actions to the Deposit Confirmed Tile

After a parcel has been successfully registered and its deposit is confirmed, add visible **Edit** and **Delete/Cancel** actions directly inside the **Deposit Confirmed** receipt tile. Do not force the owner to navigate to a separate management screen to find the parcel they have just registered.

### Edit action

- Open a compact inline editor, modal, or side panel associated with the exact parcel shown on that receipt.
- Pre-fill it using the persisted parcel record, not stale form values.
- Permit safe fields such as the recipient phone number and other non-critical details to be edited.
- Validate all edited values and preserve tracking-number uniqueness.
- If the tracking number can be changed by the current business rules, validate that it is unique and update all references consistently.
- Do not silently change the physical parcel size or locker assignment after deposit. For changes that affect compartment compatibility, require a validated relocation/reassignment workflow; do not simply rewrite the database value.
- Do not alter the original confirmed deposit timestamp when editing contact details.
- Save changes only through the actual update function/API, persist them, update the receipt and related views, and write an audit event.
- Provide explicit **Save Changes** and **Cancel** buttons. Show success only after the save and follow-up read-back succeed.

### Delete/Cancel action

- Provide an obvious but visually restrained **Delete/Cancel Parcel** action on the same receipt tile.
- Before making changes, display a confirmation dialog identifying the tracking number and explaining the impact.
- If the parcel is only registered or reserved and not physically/simulated-deposited, cancel the operation and safely release any reservation.
- If the parcel is already confirmed as deposited and its locker is occupied, do not erase the parcel record or set the locker to empty immediately. Require a safe withdrawal/cancellation step and a clear confirmation that the parcel has been removed. In the simulated prototype, this can be an explicit “Confirm Simulated Parcel Removal” action; real hardware would require appropriate controller/sensor evidence.
- Only after removal/cancellation is successfully committed may the application revoke the active pickup PIN and release the locker.
- Preserve an audit/history record; use a cancelled/withdrawn status or soft deletion when appropriate instead of destroying transaction evidence.
- Never allow ordinary deletion of a collected parcel to erase completed collection history.
- If any critical operation fails, preserve a consistent state and show an error rather than a false success.

After edit or cancellation, update the receipt tile, locker wall, Student Lookup, Demo Notification Inbox and relevant history from the same persisted records. No stale PIN, duplicate assignment, dangling locker link, or contradictory status may remain.

## 21.3 Locker Wall: Enrich Locker Details for Every Occupied Locker

In the Locker Wall section, when the owner/operations user selects an `OCCUPIED` locker, its **Locker Details** tile/panel must show the parcel information associated with that exact compartment.

For every occupied locker, display:

- Locker ID.
- Locker size.
- Current status (`OCCUPIED`).
- Assigned parcel's tracking number.
- The pickup PIN associated with that parcel, displayed as a six-digit string with leading zeroes preserved.
- PIN expiry, if available.
- Deposit timestamp, if available.
- Relevant parcel status.
- Last-updated timestamp.
- Available owner/operations actions.

The tracking number and PIN must come from the parcel and credential records actually assigned to that locker. Never infer or generate a PIN just for display. If the occupied locker has no valid parcel/credential link, flag an inconsistency and do not invent missing details.

Show the PIN in a high-contrast, clearly labelled **PICKUP PIN** field. Provide a Copy PIN control only if it works. Keep the compact 24-locker wall visually clean; put detailed parcel data in the selected **Locker Details** tile/panel rather than cramming tracking numbers and PINs into every tiny compartment tile.

Because the prototype intentionally has no login screen, treat this as an owner/operations demo view and label the demo context clearly. Do not show all locker PINs in the student interface, general dashboard summaries, or public activity log. Keep the student-facing lookup constrained to the single tracking number entered, as defined in Section 21.1. Document that production deployment would require actual access control before exposing PINs to owners or students.

### Edit from Locker Details

Add an **Edit Parcel** action to the details of an occupied locker. It must open the editor for the parcel currently assigned to that exact locker, prefilled from persisted data. Apply the state-aware edit rules in Section 21.2. Validate the relationship again before saving so an old or stale details panel cannot edit a different parcel after the locker state has changed.

### Delete/Cancel from Locker Details

Add a **Delete/Cancel Parcel** action to the details of an occupied locker. It must operate on the parcel currently assigned to that exact locker, not delete the locker record itself. Show a confirmation step and require safe parcel-removal confirmation before releasing an occupied compartment. Revoke the pickup credential only after the cancellation/removal transaction has safely succeeded. Preserve parcel and audit history.

Do not allow this action to delete one of the 24 configured locker records. The locker remains part of the 24-compartment inventory and returns to `EMPTY` only when the parcel has been safely removed and the state change has been committed.

For an `EMPTY` locker, do not display a parcel tracking number or PIN. For `RESERVED` or `OUT_OF_SERVICE` lockers, display appropriate status and only actions valid for that state.

## 21.4 State Consistency and Security Requirements for These Actions

All new edit/delete and PIN-display features must use the same authoritative data source and validated application functions as deposit and collection.

- Do not duplicate parcel data in component-only state as a separate source of truth.
- Re-read the current locker-to-parcel assignment before every edit, cancellation, or collection operation.
- Use atomic or otherwise concurrency-safe state updates where supported.
- Prevent editing a parcel that has already changed state without revalidation.
- Prevent cancelling a parcel that is already collected through the ordinary delete flow.
- Keep completed collection records and audit history intact.
- Never free an occupied locker before a successful, confirmed removal/cancellation operation.
- Never expose a PIN from a different tracking number in Student Lookup.
- Never expose credential secrets in general debug logs or developer-facing verification output on the receipt.
- Keep actual tool outcomes and useful validation evidence in the protected Activity/Audit view.

## 21.5 Additional Acceptance Tests for the Latest Changes

Add and execute tests that verify:

1. Entering tracking number A in Student Lookup displays only the notification and PIN associated with parcel A.
2. The result for parcel A does not display PINs or notifications for parcels B or C.
3. A nonexistent tracking number shows a clear empty state and no PIN.
4. Submitting a new tracking number replaces the previous result and never leaves the old PIN visible.
5. Leading zeroes in a displayed PIN are preserved.
6. The Demo Notification Inbox does not show all pickup PINs before a tracking-number lookup.
7. The Deposit Confirmed tile has functioning Edit and Delete/Cancel controls associated with the just-registered parcel.
8. Editing a permitted field persists the change and updates the receipt, notification record/view, and related parcel displays as applicable.
9. Duplicate tracking-number edits are rejected without corrupting the record.
10. Cancelling before confirmed deposit releases a reservation safely.
11. Cancelling a deposited parcel requires removal confirmation and never frees the locker prematurely.
12. A completed/collected parcel's history cannot be erased through the normal delete action.
13. Selecting each occupied locker displays the correct tracking number and PIN belonging to that locker.
14. Each occupied locker detail panel shows its actual persisted PIN with leading zeroes preserved.
15. An empty locker shows no parcel tracking number or pickup PIN.
16. An inconsistent occupied-locker assignment is flagged instead of showing invented data.
17. Edit/Delete actions in Locker Details operate on the parcel assigned to the selected locker and revalidate that assignment before committing.
18. Cancelling a parcel from Locker Details safely revokes its active PIN, records the cancellation, updates the locker wall, and returns the compartment to `EMPTY` only after successful removal.
19. The 24 locker records remain exactly 24 after parcel edits, cancellations, and collections; cancelling a parcel never deletes a locker record.
20. Existing deposit, PIN validation, late-fee, payment simulation, FAQ, notification, collection, persistence, and agent-execution tests still pass.

Run the relevant tests after each fix, then rerun the core regression suite and perform a live UI smoke test if supported. Do not claim a test passed unless it was executed.

---

# 22. FINAL PRIORITY FOR THE NEXT ITERATION

Implement the latest changes in this order:

1. Correct tracking-number-filtered Demo Notification Inbox so it shows only the matching pickup PIN.
2. Add working Edit and Delete/Cancel actions on the Deposit Confirmed receipt.
3. Add tracking number, pickup PIN, and state-aware Edit/Delete/Cancel actions to each occupied Locker Details panel.
4. Verify state consistency across the receipt, Student Lookup, notification inbox, locker wall, parcel history, and database.
5. Run and fix the corresponding tests, then rerun the end-to-end deposit-to-collection demonstration.

Do not undo previously required behaviour: keep exactly 24 authoritative locker records, no Total Parcels overview card, no Delivery Note field, no pickup QR on the Deposit Confirmed receipt, prominent red pickup PIN, centered laptop layouts, a numeric-only kiosk PIN keypad, one-hour free collection period, RM1 per additional started hour, penalty QR only when a fee is due, a clearly labelled simulated payment when no approved real integration exists, generalized grounded FAQ support, and real WorkBuddy/application tool execution where supported.

The objective is a coherent, testable prototype—not merely more buttons. Every Edit/Delete/Cancel action must work on the correct record, every displayed PIN must belong to the intended tracking number or selected occupied locker, and every successful state change must be persisted and verified.
