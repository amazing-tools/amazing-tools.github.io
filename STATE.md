# Current Focus & Objective
1. **Loan EMI & Prepayment Calculator Accuracy & Dynamic UI/UX Overhaul**:
   - Fixed core calculation discrepancies where prepayment tenure reduction was displaying excessively large reductions due to a hidden ghost monthly prepayment (defaulting to ₹5,000) and missing floating rate revision arguments in isolated counterfactual simulations.
   - Restructured Step 1 input flow so that the **First EMI Date / Month picker** is placed immediately after the **Loan Amount**, matching user expectations.
   - Replaced abstract "Year 1, Year 2 ... Month 1, Month 2" dropdowns in both Floating Interest Rate Timeline and Custom Prepayment Scheduler with **real dynamic calendar years (e.g. 2024, 2025...) and calendar months (e.g. July, November...)** derived from the First EMI Date and Loan Tenure.
   - Implemented exact Indian bank-standard amortization formulas where at any prepayment month, remaining tenure drop ($N_{before} - N_{after}$) is calculated accurately on active principal, current EMI, and effective floating interest rate.
   - Added Quick 1x, 2x, 3x, 4x, 5x EMI multiplier chips directly into the prepayment scheduler.
   - Added "Remaining Tenure" column to the full amortization schedule (both monthly & yearly views, as well as CSV export).

# Architecture Context
- `tools-website/pages/emi-calculator.html`:
  - Relocated First EMI Date right after Loan Amount with quick presets (`This Month`, `Next Month`, `Jan Next Year`, `1 Yr Ago (Running Loan)`).
  - Cleaned up Rate Revision selects (`newRateYear`, `newRateMonth`) with dynamic calendar timing preview.
  - Reset default Extra Monthly EMI from 5,000 to 0 to prevent phantom deductions.
  - Added Quick EMI Multipliers (`1x EMI`, `2x EMI`, `3x EMI`, `4x EMI`, `5x EMI`) and live date preview in Prepayment Scheduler.
  - Updated Scheduler Table and Amortization Table headers to include Bank Tenure Reduced, Balances, and Remaining Tenure.
- `tools-website/js/emi-calculator.js`:
  - Added `computeBankRemainingTenure()` helper matching standard banking annuity equations.
  - Added `updateDropdownsForLoanTiming()` populating real calendar years and months across both timeline and prepayment dropdowns.
  - Updated `handleAddLumpsum()` and `handleAddRateRevision()` to map real calendar dates to exact loan months.
  - Fixed `renderSchedulerTable()` to pass full rate revisions and calculate isolated bank remaining tenure reduction and lifetime interest savings.
  - Enhanced Amortization table and CSV download with "Remaining Tenure" column.

# Active Files & Dependencies
- `tools-website/pages/emi-calculator.html`
- `tools-website/js/emi-calculator.js`

# Completed Steps
1. Investigated root causes of mathematical inflation in tenure reduction:
   - Default `monthlyPrepay` value of ₹5,000 in HTML was running silently in background simulations.
   - `currentParams` in `renderSchedulerTable()` was missing `rateRevisions` and `baseRate`, comparing incompatible simulations.
2. Relocated First EMI Date to follow Loan Amount in HTML.
3. Implemented dynamic calendar year and month generation based on First EMI date and tenure.
4. Implemented Quick EMI multiple buttons (`1x`, `2x`, `3x`, `4x`, `5x EMI`).
5. Implemented bank-standard remaining tenure reduction calculation formula in `renderSchedulerTable()`.
6. Automated verification:
   - Validated JS syntax with `node -c`.
   - Verified user scenario (40L, 17 Yrs, 8.75%, 3x EMI in July 2025 -> exact ~11-12 months bank tenure reduction).
   - Captured and reviewed headless Chrome visual layout screenshots.

# Current Issues / Risks
- None. All calculations, UI dropdowns, and layouts verified.

# Next Immediate Actions
- Present concise summary and explanations to user in clear Hindi/English.
