---

# 🧾 CleanCity Test Report

## 1. Summary

Manual testing was done using Chrome on Mac.  
Focus areas: form validation, dashboard filters, feedback form, accessibility, and admin updates.

## 2. Environment

- **Device:** MacBook Pro
- **Browser:** Chrome (latest)
- **OS:** macOS
- **Network:** Wi-Fi (home)

## 3. Test Cases

| ID  | Feature          | Test Steps          | Expected                     | Actual              | Status  |
| --- | ---------------- | ------------------- | ---------------------------- | ------------------- | ------- |
| TC1 | Form Validation  | Submit without date | Error: “Date is required”    | No error shown      | ❌ Fail |
| TC2 | Dashboard Filter | Filter by Eldoret   | Show Eldoret results only    | Shows Nairobi data  | ❌ Fail |
| TC3 | Feedback Page    | Enter “ABC123”      | Show invalid format error    | Accepted invalid ID | ❌ Fail |
| TC4 | Awareness Page   | Check alt-text      | All images have descriptions | Missing alt-text    | ❌ Fail |

## 4. Defect Summary

| Bug ID | Description                 | Severity | Status |
| ------ | --------------------------- | -------- | ------ |
| #1     | Date validation missing     | Medium   | Open   |
| #2     | Dashboard filter incorrect  | High     | Open   |
| #3     | Invalid request ID accepted | Medium   | Open   |
| #4     | Missing alt-text on images  | Low      | Open   |

https://github.com/Herwebcode/CleanCity-QA-Herwebcode/issues/1
https://github.com/Herwebcode/CleanCity-QA-Herwebcode/issues/2
https://github.com/Herwebcode/CleanCity-QA-Herwebcode/issues/3
https://github.com/Herwebcode/CleanCity-QA-Herwebcode/issues/4

### 💭 Reflection

**Challenges:** Understanding filter logic and testing across browsers.  
**Effective Techniques:** Black-box testing for validation and white-box for filtering logic.

## 5. Lessons Learned

- Some intentional bugs helped understand real QA workflows.
- Accessibility testing is easy to overlook but very important.
- White-box testing could verify logic for filters and form validation.
- Realized how minor issues like missing alt-text can affect accessibility and user trust.

**Tested by:** Bashirat Oyekunle  
**Date:** October 2025

```

```
