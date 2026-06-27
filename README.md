# Zephyr Neobank Transaction Health and Fraud Analysis

An end-to-end analytics project examining H1 2026 transaction health for **Zephyr Bank**, a UK digital-only neobank — built for the **Onyx Data DataDNA Challenge**. The project moves from raw transactional data to a five-page, decision-ready Power BI report, uncovering fraud concentration, fee leakage, and channel patterns that aren't visible in surface-level reporting.

---

## Overview

Zephyr Bank operates entirely digitally — every transaction flows through its mobile app, web browser, or automated payment rails. For its 2026 H1 review, leadership needed one consolidated view answering very different questions for four different teams:

- **Risk** — which customers and merchant categories concentrate fraud exposure?
- **Finance** — what is fee revenue, and where is it leaking?
- **Product** — which channels and transaction types are failing, and why?
- **Compliance** — which customers need closer KYC/AML attention?

This project answers all four from a single underlying data model.

---

## Dataset

The data model follows a standard star schema: three dimension tables joined to one fact table.

| Table | Grain | Key Fields |
|---|---|---|
| `dim_customer` | 20 customers | age band, region, customer segment, KYC status, account open date |
| `dim_transaction_type` | 15 types | channel, domestic/international flag, typical fee |
| `dim_merchant_category` | 18 categories | sector, risk classification |
| `fact_transactions` | ~1,500 transactions | amount, fee charged, status, fraud flag, FX rate, device type, failure reason |

> **Note:** This is a synthetic dataset built for the Onyx Data DataDNA Challenge, not real banking data. "Zephyr Bank" is a fictional case-study brand.

---

## Tools & Process

| Stage | Tool |
|---|---|
| Data cleaning & transformation | PostgreSQL |
| Data modeling & report build | Power BI Desktop |
| Calculation layer | DAX |

**PostgreSQL** was used to clean and reshape the raw exports before they ever reached Power BI — joining the fact table to its three dimensions, standardizing types, and engineering derived fields (week number, weekday, account-age buckets, amount-size buckets, fee-deviation flags, and FX-applied flags) so the Power BI layer could focus on analysis rather than cleanup.

**Power BI / DAX** handled everything downstream: the star-schema model, a dedicated measures table (~20 custom DAX measures covering fraud exposure, fee leakage, account tenure, and segment-level KPIs), a dynamic-title system so every page and chart relabels itself based on the active filter, and two independent field parameters — one to swap the KPI lens (Transaction Count / Amount / Fraud Rate / Declined Rate) across charts, and a second purpose-built for dynamic Top/Bottom-N ranking.

*(Implementation details — SQL and DAX logic — are intentionally not reproduced here; this README documents the approach, not the code.)*

---

## Dashboard Structure

The report is organized as five pages, each with a distinct job:

1. **Executive Overview** — headline KPIs, synthesized cross-page findings, top recommendations, and in-report navigation to the other four pages.
2. **Transaction & Channel Patterns** — temporal trends (monthly, weekly, day-of-week), device/channel behavior, and FX-rate compliance on international transactions.
3. **Customer Insights** — segment, region, and age-band composition, and how tenure and KYC status relate to behavior.
4. **Fraud & Risk Exposure** — fraud concentration by customer, region, merchant category, and device; KYC-vs-fraud validation; a ranked customer watchlist.
5. **Revenue & Fee Health** — total fee revenue, expected-vs-charged variance, leakage by transaction type, and fee distribution by customer segment.

---

## Key Findings

A few of the most significant, non-obvious results from the analysis:

- **One customer concentrates the majority of risk.** A single account represents 59% of total transaction value and 95% of all fraud exposure in the book.
- **Fraud is a customer-level attribute, not a transaction-level event.** Exactly 4 of 20 customers carry a 100% fraud rate; the remaining 16 carry 0% — there's no middle ground.
- **KYC verification didn't prevent fraud — all of it happened after verification.** 100% of fraud-flagged transactions belong to KYC-verified customers; unverified customers show a 0% fraud rate, the opposite of the standard assumption.
- **Channel carries no risk signal; device type carries all of it.** Every channel (Mobile App, Web Browser, ATM Network, Automated) splits identically across device types — device type, not channel, is what actually predicts outcome.
- **A near-zero net fee variance was hiding a real billing defect.** Total fee charged (£750.17) looked almost identical to the expected total (£750.00) — but that figure was masking £1,174 in overcharges and shortfalls happening to cancel each other out in opposite directions.
- **Two categories often grouped together behave oppositely.** Gambling shows elevated fraud risk as expected; Crypto Exchange — frequently assumed to carry similar risk — does not.

---

## Recommendations

1. Open an immediate manual review of the single highest-exposure customer account.
2. Correct the billing rule causing erroneous fee charges on declined, fee-free transaction types.
3. Fix the international fee/FX-rate logic, starting with the highest-leakage transaction type.
4. Shift fraud-screening priority from KYC status to behavioral signals — device type, transaction size, and region.

