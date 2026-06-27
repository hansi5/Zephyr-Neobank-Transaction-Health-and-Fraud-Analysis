# Data Dictionary

**Domain:** UK Fintech Neobank — Digital Transaction Health Monitoring (Zephyr Bank)
**Scope:** 2026-01-01 to 2026-05-31 (H1 2026 pre-review window)
**Currency:** GBP (Great British Pounds)
**Grain:** 3 dimension tables (dim_customer, dim_transaction_type, dim_merchant_category), 1 fact table (fact_transactions)

---

## Model Diagram

```
dim_customer                dim_transaction_type       dim_merchant_category
┌──────────────────┐        ┌──────────────────────┐   ┌──────────────────────┐
│ customer_id (PK) │        │ transaction_type_id   │   │ merchant_category_id │
│ customer_name    │        │   (PK)                │   │   (PK)               │
│ age_band         │        │ type_name             │   │ category_name        │
│ region           │        │ channel               │   │ sector               │
│ customer_segment │        │ is_domestic           │   │ risk_flag            │
│ kyc_verified     │        │ typical_fee_gbp       │   └──────────┬───────────┘
│ account_open_date│        └──────────┬────────────┘              │
└──────────┬───────┘                   │                           │
           │                           │                           │
           │              ┌────────────▼───────────────────────────▼──────────┐
           └─────────────►│                  fact_transactions                 │
                          │  transaction_id (PK)                               │
                          │  customer_id (FK → dim_customer)                   │
                          │  transaction_type_id (FK → dim_transaction_type)   │
                          │  merchant_category_id (FK → dim_merchant_category) │
                          │  transaction_date                                   │
                          │  transaction_datetime                               │
                          │  amount_gbp                                         │
                          │  fee_charged_gbp                                    │
                          │  transaction_status                                 │
                          │  is_flagged_fraud                                   │
                          │  fx_rate_used                                       │
                          │  device_type                                        │
                          │  failed_reason                                      │
                          └────────────────────────────────────────────────────┘
```

---

## Dimension Tables

### dim_customer
Stores demographic and account-level details for each registered Zephyr Bank customer.

| Column | Type | Description |
|---|---|---|
| **customer_id** (PK) | integer | Surrogate key uniquely identifying each customer |
| customer_name | string | Full name of the customer |
| age_band | string | Age group bracket: 18-24, 25-34, 35-44, 45-54, 55-64 |
| region | string | UK region of customer registration (e.g. London, Scotland) |
| customer_segment | string | Product tier: Starter, Standard, Premium, Business |
| kyc_verified | boolean | TRUE if the customer has completed KYC identity verification |
| account_open_date | date | Date the customer's Zephyr Bank account was opened |

*Rows: 20*

---

### dim_transaction_type
Classifies each transaction by its type and the channel through which it was initiated.

| Column | Type | Description |
|---|---|---|
| **transaction_type_id** (PK) | integer | Surrogate key for the transaction type |
| type_name | string | Descriptive category: Purchase, Transfer - Outbound, ATM Withdrawal, Direct Debit, Salary Credit, Card Refund, Crypto Purchase, Savings Pot Transfer, Loan Repayment |
| channel | string | Originating channel: Mobile App, Web Browser, ATM Network, Automated |
| is_domestic | boolean | TRUE if the transaction is within the UK; FALSE if international |
| typical_fee_gbp | decimal | Standard fee in GBP for this transaction type (0.00 if no fee) |

*Rows: 15*

---

### dim_merchant_category
Groups transactions by the type of merchant or financial activity, with an associated risk classification.

| Column | Type | Description |
|---|---|---|
| **merchant_category_id** (PK) | integer | Surrogate key for the merchant category |
| category_name | string | Name of the spending category (e.g. Groceries, Gambling, Crypto Exchange) |
| sector | string | Broad grouping: Retail, Services, Financial, Leisure |
| risk_flag | string | Risk classification: Low, Medium, or High |

*Rows: 18*

---

## Fact Tables

### fact_transactions
Each row represents a single digital transaction executed by a Zephyr Bank customer during H1 2026.

| Column | Type | Description |
|---|---|---|
| **transaction_id** (PK) | integer | Surrogate key uniquely identifying each transaction |
| customer_id (FK) | integer | → dim_customer: the customer who initiated the transaction |
| transaction_type_id (FK) | integer | → dim_transaction_type: classification of the transaction |
| merchant_category_id (FK) | integer | → dim_merchant_category: merchant sector of the transaction |
| transaction_date | date | Calendar date the transaction occurred (YYYY-MM-DD) |
| transaction_datetime | datetime | Full timestamp of the transaction (YYYY-MM-DD HH:MM:SS) |
| amount_gbp | decimal | Transaction value in GBP; positive = debit/payment, negative = refund/credit |
| fee_charged_gbp | decimal | Fee actually charged in GBP; may differ from typical_fee_gbp if waived or in error |
| transaction_status | string | Outcome: Completed, Declined, Pending, Reversed |
| is_flagged_fraud | boolean | TRUE if the Zephyr fraud engine flagged this transaction |
| fx_rate_used | decimal | FX exchange rate applied to international transactions; NULL for domestic |
| device_type | string | Device used: iOS, Android, Web, N/A (for automated/ATM transactions) |
| failed_reason | string | Reason for non-completion if Declined or Reversed (e.g. Insufficient Funds, Fraud Suspected); NULL for completed/pending transactions |

*Rows: ~1,500 (expanded from 20 seed rows)*
