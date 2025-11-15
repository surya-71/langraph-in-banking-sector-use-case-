# Understanding Compound Interest and Deposit Calculations

Meta:
- title: "Compound Interest & Deposit Calculations — Formulas, Examples, and Tools"
- description: "A practical guide to simple, compound, and continuous interest plus calculations with regular deposits. Includes formulas, numeric examples, spreadsheet formulas, and Python snippets."
- estimated_reading_time: "6 min"
- tags: ["personal finance", "interest", "compound interest", "savings", "finance math"]

## Introduction

Compound interest is one of the most powerful concepts in personal finance and investing. It describes how an initial sum grows over time when interest is periodically added to the principal, and interest itself earns interest in subsequent periods. This article explains the core formulas for simple, discrete-compounded, and continuously compounded interest, shows how to include regular deposits, and provides ready-to-use spreadsheet and Python examples.

Key notation
- P = initial principal (starting deposit)
- r = nominal annual interest rate (as a decimal; 5% → 0.05)
- n = compounding periods per year (12 for monthly, 4 for quarterly, 1 yearly)
- t = time in years
- PMT = periodic deposit (amount deposited each period)
- type = 0 if deposits at period end (ordinary annuity), 1 if at period beginning (annuity due)

---

## 1. Simple interest (no compounding)

- Interest = P * r * t  
- Amount A = P + Interest = P * (1 + r * t)

Use simple interest only for short-term loans or when explicitly stated—most savings/investments use compound interest.

---

## 2. Discrete compound interest (periodic compounding)

Amount after t years:
- A = P * (1 + r/n)^(n * t)

Examples for common n:
- Annual: n = 1
- Quarterly: n = 4
- Monthly: n = 12
- Daily (approx): n = 365

---

## 3. Continuous compounding

When interest compounds continuously:
- A = P * e^(r * t)  (e is the natural base ~2.71828)

Continuous compounding gives the theoretical upper bound for growth given a nominal annual rate r.

---

## 4. Future value with regular deposits (discrete compounding)

If you make equal deposits PMT each period (period aligned with compounding), the future value of those deposits (ordinary annuity — deposits at period end) is:
- FV_deposits = PMT * [ (1 + r/n)^(n*t) - 1 ] / (r/n)

Total future value including initial principal:
- A = P * (1 + r/n)^(n*t) + FV_deposits

If deposits are at the beginning of each period (annuity due), multiply FV_deposits by (1 + r/n).

Edge case: if r = 0, FV_deposits = PMT * (n*t)

---

## 5. Present value formulas (reverse calculation)

- Present value of a future lump sum A: PV = A / (1 + r/n)^(n*t)  
- PV of an ordinary annuity (payments PMT at period end):  
  PV = PMT * [1 - (1 + r/n)^(-n*t)] / (r/n)

---

## 6. Quick numeric examples

Example A — compound only:
- P = 1,000; r = 0.05; n = 12; t = 3  
- A = 1000 * (1 + 0.05/12)^(36) ≈ 1,161.62

Example B — monthly deposits:
- P = 1,000; PMT = 100 per month; r = 0.05; n = 12; t = 3  
- FV_deposits = 100 * [ (1 + 0.05/12)^(36) - 1 ] / (0.05/12) ≈ 3,878.40  
- Total A ≈ 1,161.62 + 3,878.40 ≈ 5,040.02  
- If deposits at beginning of each month (annuity due), multiply deposit term by 1.0041667 → total ≈ 5,056.20

Example C — continuous compounding:
- P = 1,000; r = 0.05; t = 3  
- A = 1000 * e^(0.15) ≈ 1,161.83

---

## 7. Spreadsheet formulas (Excel / Google Sheets)

- Discrete compound: =P * (1 + r / n)^(n * t)  
- Continuous compounding: =P * EXP(r * t)  
- FV with monthly deposits (ordinary annuity): =FV(r/12, n*t, -PMT, -P, 0)  
  Example: =FV(0.05/12, 36, -100, -1000, 0)  
- For deposits at beginning: set last arg to 1 (type=1) in the FV function.

Note: Excel/Sheets FV uses rate per period and number of periods. Signs convention: cash outflows vs inflows; using negatives avoids confusion.

---

## 8. Small Python calculator

Copy and run the following snippet to compute scenarios programmatically:

```python
import math

def future_value(P, r, n, t):
    return P * (1 + r / n) ** (int(n * t))

def future_value_continuous(P, r, t):
    return P * math.exp(r * t)

def future_value_with_deposits(P, r, n, t, PMT, deposit_at_beginning=False):
    periods = int(n * t)
    rate_per_period = r / n
    if rate_per_period == 0:
        fv_deposits = PMT * periods
    else:
        fv_deposits = PMT * ((1 + rate_per_period) ** periods - 1) / rate_per_period
        if deposit_at_beginning:
            fv_deposits *= (1 + rate_per_period)
    return P * (1 + rate_per_period) ** periods + fv_deposits

# Example usage
P = 1000
r = 0.05
n = 12
t = 3
PMT = 100

print("Compound only:", future_value(P, r, n, t))
print("Continuous compounding:", future_value_continuous(P, r, t))
print("Monthly deposits (end):", future_value_with_deposits(P, r, n, t, PMT))
```

---

## 9. Practical considerations & caveats

- Always use r as a decimal (5% → 0.05).
- For nominal quoted rates with compounding, convert to rate-per-period r/n.
- Fees, taxes, and inflation reduce your effective returns. For inflation-adjusted returns, use the Fisher equation: (1 + nominal) = (1 + real)*(1 + inflation).
- If interest rate changes over time or deposits vary, simulate period-by-period (a simple loop in a spreadsheet or code) rather than using closed-form annuity formulas.

---

## 10. FAQ (brief)

Q: Which grows faster — more frequent compounding or continuous?  
A: Continuous compounding yields the highest amount for a given nominal rate; more frequent discrete compounding approaches that limit.

Q: How do I compute required monthly deposit to reach a goal?  
A: Rearrange the FV_deposits formula to solve for PMT:
- PMT = [Target - P*(1 + r/n)^(n*t)] * (r/n) / [ (1 + r/n)^(n*t) - 1 ]

Q: What if interest rate is variable?  
A: Simulate each period with its own rate; compound step-by-step.

---

## Conclusion

Understanding these formulas makes it easy to model savings goals, loan balances, and investment growth. Use spreadsheets for quick scenarios and code for flexible, repeatable simulations. If you tell me your P, r (percent), n, t, PMT, and whether deposits are at the beginning or end of the period, I’ll calculate exact numbers and a schedule for you.

References and further reading:
- Basic finance textbooks or online calculators (bankrate, Investopedia) for additional examples and definitions.
