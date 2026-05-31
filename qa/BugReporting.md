# The Ultimate Bug Reporting Guide & Template

A high-quality bug report is a love letter to your development team. It reduces friction, eliminates back-and-forth questioning, and drastically speeds up time-to-resolution. 

This guide details how to write flawless bug reports and provides a copy-pasteable markdown template.

---

## 1. Anatomy of a Perfect Bug Report

Every bug report should contain the following core blocks to ensure clarity:

| Section | Description | Example |
| :--- | :--- | :--- |
| **Title** | Concise, descriptive summary of the issue (What + Where + Under what condition). | *[Checkout] 500 error when applying 100% discount promo code.* |
| **Environment** | OS, browser/device version, app version, and test environment (Staging/Dev/Prod). | *Chrome 124.0 (macOS Sonoma), Staging v2.4.1* |
| **Steps to Reproduce** | A numbered, step-by-step recipe to trigger the bug. | *1. Navigate to /cart. 2. Add 'Product A' to cart...* |
| **Expected Behavior** | What the application *should* have done according to specifications. | *Promo code is applied, cart total reduces to $0.00.* |
| **Actual Behavior** | What the application *actually* did. | *Application spins indefinitely, console logs a 500 Server Error.* |
| **Visuals & Logs** | Screenshots, screen recordings, network trace files, or console logs. | *(Attach file or paste JSON payload)* |
| **Impact / Context** | Business or user impact of this bug. | *Prevents users from checking out with free/reward products.* |

---

## 2. Severity vs. Priority Matrix

One of the most common pitfalls in QA is confusing **Severity** (technical impact on the system) with **Priority** (business urgency to fix).

```
                      SEVERITY (Technical Impact)
                 High ───────────────► Low
              ┌───────────────────────────┐
         High │  Blocker  │  Quick-Fix    │
              │  (S1/P1)  │  (S3/P1)      │
    P PRIORITY│  e.g. Auth│  e.g. Typos in│
    R (Business│  is down  │  main header  │
    O Urgency) ├───────────┼───────────────┤
    D          │  Major    │  Cosmetic     │
    U    Low  │  (S1/P3)  │  (S4/P3)      │
    C          │  e.g. Edge│  e.g. Button  │
    T          │  OS crash │  padding off  │
              └───────────────────────────┘
```

### Severity Levels (Technical)
*   **S1 - Blocker / Critical**: Complete system outage, data corruption, or security vulnerability. No workaround exists. *(e.g., Payment portal crashes on load).*
*   **S2 - Major**: Core feature is broken, but a tedious workaround exists. *(e.g., Search filter doesn't work, but sorting does).*
*   **S3 - Minor**: Non-critical feature failure with an easy workaround. *(e.g., Profile photo crop tool fails, but uploading full image works).*
*   **S4 - Trivial / Cosmetic**: Visual/UI issues, typos, or minor alignment bugs. *(e.g., Logo is slightly off-center).*

### Priority Levels (Business Urgency)
*   **P1 - High (Must Fix Now)**: Critical business impact. Demands immediate patch deployment.
*   **P2 - Medium (Fix in Next Release)**: Needs to be scheduled in the current sprint or upcoming release.
*   **P3 - Low (Backlog)**: Nice-to-have fix. Will be resolved when resources allow.

---

## 3. Good vs. Bad Bug Reports: In Action

### ❌ The Bad Report
> **Title**: Cart page doesn't work.
> **Description**: I tried to buy something and it crashed. Please fix ASAP.

*Why this is bad:*
*   **No steps**: How do developers replicate it?
*   **No environment**: Is this on Chrome? Safari? iOS?
*   **No logs**: Is it a frontend crash, API timeout, or payment decline?

###  The Good Report
> **Title**: `[Checkout]` UI freezes and console logs CORS error when attempting to pay with Apple Pay on Safari iOS
> **Steps to Reproduce**:
> 1. Open Safari on iPhone 15 Pro (iOS 17.4).
> 2. Go to `https://staging.example.com/shop`.
> 3. Add any item to the cart and proceed to Checkout.
> 4. Select **Apple Pay** as the payment method.
> 5. Double-click to pay using Apple Pay.
>
> **Expected**: Apple Pay sheet closes and user is redirected to `/checkout/success`.
> **Actual**: Apple Pay sheet closes, checkout spinner spins infinitely. Console error: `Origin https://staging.example.com is not allowed by Access-Control-Allow-Origin.`
> **Severity**: S1 (Users cannot checkout via Apple Pay) | **Priority**: P1 (High revenue impact)

---

## 4. Copy-Pasteable Markdown Bug Template

Use the template below for your GitHub Issues, GitLab Issues, or markdown-based bug trackers:

```markdown
## Description
<!-- Provide a clear and concise description of the bug -->

## Environment
- **Environment:** [e.g., Staging, Production, Dev]
- **OS:** [e.g., macOS Sonoma, Windows 11, iOS 17.2]
- **Browser/Device:** [e.g., Chrome 124, Safari, iPhone 15]
- **App Version:** [e.g., v2.3.0]

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

## Expected Behavior
<!-- A clear description of what you expected to happen -->

## Actual Behavior
<!-- A clear description of what actually happened -->

## Screenshots / Video / Logs
<!-- Attach screenshots, screen recordings, or paste relevant logs here -->
<details>
<summary>Console Logs / API Payload</summary>

```json
// Paste logs here
```
</details>

## Impact & Severity
- **Severity:** [Blocker / Major / Minor / Trivial]
- **Priority:** [P1 (High) / P2 (Medium) / P3 (Low)]
- **Impact:** <!-- Describe how this affects users or business operations -->
```
