# Data Challenge

## 🎯 Your Role

Step into the role of a data analyst embedded within the risk and product team at **Zephyr Bank**, a UK-based fintech neobank. Your remit covers transaction health monitoring — identifying friction, fraud exposure, and revenue leakage across the bank's digital transaction pipeline. You've been provided with a dataset spanning January 2026 to June 2026, covering ~1,500 individual customer transactions across all channels and product types. Your mission is to create an analytical report that uncovers patterns, drives insights, and delivers actionable recommendations to stakeholders across Risk, Product, and Finance.

## 📊 The Scenario

Zephyr Bank launched its full product suite in 2018 and has grown to serve over 20,000 active customers across the UK. Unlike traditional banks, Zephyr operates entirely digitally — all transactions flow through its mobile app, web browser, or automated payment rails. This creates rich transactional data, but also unique challenges: fraud patterns skew towards digital channels, FX fees are a meaningful revenue lever, and customer segments behave very differently.

As part of the bank's 2026 H1 review (target date: **1 June 2026**), leadership wants a consolidated view of transaction health. Finance wants to understand fee revenue and leakage. Risk wants to know which customer segments and merchant categories concentrate fraud exposure. Product wants to understand channel usage and where transactions are failing — and why.

The dataset covers three interlinked dimensions: customers (with demographic and KYC data), transaction types (with channel and fee metadata), and merchant categories (with sector and risk classification). Together they power a fact table of ~1,500 transactions with amount, status, fraud flags, FX rates, and failure reasons.

### Dataset Overview

- **Dimensions**: 3 dimension tables tracking customers, transaction types, and merchant categories
- **Facts**: 1 fact table recording individual digital transactions
- **Scope**: 2026-01-01 to 2026-05-31 (H1 2026 pre-review window)
- **Volume**: ~1,500 transaction rows

### Key Business Metrics Available

- **Transaction volume & value**: Count and GBP amount by date, customer segment, channel, and merchant sector
- **Fraud & risk exposure**: Fraud flag rate and Declined/Reversed transaction rate by customer, category, and region
- **Fee revenue**: Actual fees charged vs. expected fees by transaction type and channel

## 🔍 Analysis Areas

Look for patterns and insights across these core areas:

### 📅 Temporal Trends
- How has total transaction value trended month-on-month through H1 2026?
- Are Declined or fraud-flagged transactions clustering in specific weeks or months?
- Is there a day-of-week or time-of-day pattern in high-risk transactions? time week day
- How has fee revenue evolved over the period?
- Are there any anomalous spikes in large-value transactions that warrant investigation?

### 👤 Customer & Segment Analysis
- Which customer segments (Starter, Standard, Premium, Business) drive the highest transaction volumes and values?
- Do unverified (non-KYC) customers show disproportionate rates of Declined or fraud-flagged transactions?
- Which UK regions generate the most transaction activity, and where are fraud events concentrated?
- Are older account holders behaving differently from recently onboarded customers?
- Which individual customers are generating outsized risk or fee revenue?

### 💳 Transaction & Merchant Patterns
- Which transaction types have the highest failure (Declined + Reversed) rates?
- Which merchant categories attract the most fraud flags — and does the risk_flag rating align with actual fraud events?
- How do international transactions compare to domestic in terms of value, fees, and risk?
- Which device type is associated with the most failed or fraudulent transactions?
- Are there merchant categories where actual fees charged deviate from the typical fee — suggesting waivers or errors?

## ❓ Guiding Questions

Use these to structure your analysis — they're starting points, not a checklist. Explore further where you find interesting signal.

### 💰 Revenue & Fee Health
- What is Zephyr Bank's total fee revenue for H1 2026, and which transaction types contribute most?
- Are international transfer fees being consistently applied, or is there evidence of fee waivers or system errors?
- Which customer segments generate the highest fee revenue per transaction?

### 🚨 Fraud & Risk Exposure
- What percentage of all transactions were flagged as fraudulent, and how does this vary by merchant category?
- Do fraud-flagged transactions correlate with non-KYC-verified customers?
- Which single transaction represents the highest risk-adjusted exposure (high amount + fraud flag + Declined)?
- Is the Gambling or Crypto merchant category driving a disproportionate share of fraud flags relative to its transaction volume?

### 📲 Channel & Operational Health
- What is the overall Declined rate, and which channel (Mobile App, Web, Automated) has the worst failure rate?
- Are ATM Reversals or international Declines pointing to a systemic issue in a specific channel or region?
- How does device type (iOS, Android, Web, N/A) split across customer segments?

## 💡 Impact & Purpose

Your analysis should help stakeholders:

- **Risk team**: Prioritise fraud investigations by identifying the highest-exposure customer + merchant combinations
- **Finance team**: Quantify fee revenue, detect leakage from waived or missed fees, and project H2 outlook
- **Product team**: Identify channels and transaction types with unacceptably high failure rates and target UX improvements
- **Compliance team**: Flag non-KYC customers with elevated transaction values or risk-category activity for remediation

## 📋 Deliverables

Create a professional analytical report that includes:

1. **Executive Summary**
   - 3–5 key findings with business impact stated clearly
   - Top 3 recommendations in plain language
   - Headline numbers that matter

2. **Detailed Analysis**
   - Visualisations of key trends and patterns
   - Statistical summaries by segment
   - Answers to the guiding questions with supporting evidence

3. **Actionable Insights**
   - What you discovered and why it matters to the business
   - Recommended next steps with expected impact
   - Areas for deeper investigation or additional data

## 📁 Data Dictionary

See `DATA_DICTIONARY.md` for full table and column descriptions.

## 🚀 Getting Started

1. **Explore the data**: Start with summary statistics, distributions, and simple aggregations
2. **Check data quality**: The data is generally clean, but keep an eye out for outliers or anomalies worth flagging
3. **Visualise relationships**: Use charts to probe temporal and cross-dimensional patterns
4. **Synthesise insights**: Connect findings across dimensions into a coherent business story

## Scoring Hints (for your own benefit)

A strong submission will:
- Lead with business impact, not table dumps
- Quantify findings in percentages or currency amounts, not just row counts
- Use cohort and over-index analysis, not just top-N rankings
- Distinguish signal from noise — call out where findings are indicative vs conclusive
- Propose next actions that are specific and measurable
