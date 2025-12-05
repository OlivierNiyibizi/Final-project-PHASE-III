**# Final-project-PHASE-III**
**PHASE III: Logical Model Design**
**Integrated Insurance Services Project**
**1. Entity–Relationship Model (ERM)**

This project uses five core entities: CUSTOMER, POLICY, CUSTOMER_POLICY, PAYMENT, CLAIM.
These entities support all MIS functions for customer registration, policy subscription, payment tracking, and claims processing.

Entities & Keys
Entity	Primary Key	Description
CUSTOMER	Customer_id	Stores customer demographic and contact information
POLICY	Policy_id	Contains insurance policy definitions
CUSTOMER_POLICY	Customer_policy_id	Links each customer to a purchased policy
PAYMENT	Payment_id	Records all premium payments
CLAIM	Claim_id	Stores claim requests and approval results
Cardinalities

CUSTOMER 1 — N CUSTOMER_POLICY
A customer may purchase many policies.

POLICY 1 — N CUSTOMER_POLICY
A policy may be bought by many customers.

CUSTOMER_POLICY 1 — N PAYMENT
A customer policy may have multiple payments.

CUSTOMER_POLICY 1 — N CLAIM
A customer policy may have multiple claims.

ER Diagram (Text Layout for Draw.io)

<img width="1024" height="1024" alt="ChatGPT Image Dec 5, 2025, 07_56_50 AM" src="https://github.com/user-attachments/assets/c2b4743e-3ee1-4475-b9aa-c434fd02937d" />

2. Normalization (1NF → 3NF)
1NF

All tables contain atomic values.

No repeating groups.

Each record is uniquely identified by a primary key.

2NF

No partial dependencies because each table uses a single-column PK.

Non-key attributes depend fully on their table’s PK.

3NF

No transitive dependencies.

Customer contact data is stored only in CUSTOMER, not repeated in CUSTOMER_POLICY or PAYMENT.

Policy attributes stored only in POLICY.

Payment and claim details depend only on their respective PKs.

Conclusion:
The database schema is fully normalized to 3rd Normal Form (3NF).

3. Data Dictionary
CUSTOMER
Column	Data Type	Key	Description
Customer_id	NUMBER	PK	Unique customer identifier
Full_name	VARCHAR2(200)		Full names of the customer
Phone	VARCHAR2(50)		Contact phone
Email	VARCHAR2(150)	UNIQUE	Customer email address
Registration_date	DATE		Date customer registered
POLICY
Column	Data Type	Key
Policy_id	NUMBER	PK
Policy_name	VARCHAR2(200)	
Policy_type	VARCHAR2(100)	
Premium_amount	NUMBER(12,2)	
Coverage_amount	NUMBER(14,2)	
Duration_months	NUMBER	
CUSTOMER_POLICY
Column	Data Type	Key
Customer_policy_id	NUMBER	PK
Customer_id	NUMBER	FK
Policy_id	NUMBER	FK
Start_date	DATE	
End_date	DATE	
Status	VARCHAR2(20)	
PAYMENT
Column	Data Type	Key
Payment_id	NUMBER	PK
Customer_policy_id	NUMBER	FK
Payment_date	DATE	
Amount_paid	NUMBER(12,2)	
Payment_method	VARCHAR2(100)	
CLAIM
Column	Data Type	Key
Claim_id	NUMBER	PK
Customer_policy_id	NUMBER	FK
Claim_date	DATE	
Claim_description	CLOB	
Claim_status	VARCHAR2(20)	
Approved_amount	NUMBER(14,2)	
4. Constraints Summary

PKs ensure unique identification of records.

FKs maintain referential integrity:

CUSTOMER_POLICY → CUSTOMER

CUSTOMER_POLICY → POLICY

PAYMENT → CUSTOMER_POLICY

CLAIM → CUSTOMER_POLICY

CHECK constraints enforce valid status values.

UNIQUE constraint on CUSTOMER.Email.

Data type constraints ensure proper numeric and date handling.

5. Business Intelligence (BI) Considerations
Fact Tables

FACT_PAYMENT: amount, date, customer_policy_id

FACT_CLAIM: approved amount, date, claim status

Dimension Tables

DIM_CUSTOMER

DIM_POLICY

DIM_DATE

Slowly Changing Dimensions (SCD)

Customer details: SCD Type 2 recommended

Policy definitions: SCD Type 1, unless pricing changes historically

Aggregations

Monthly payments totals

Claims by policy type

Claims approved vs rejected

Revenue per customer or per policy

Audit Trail Recommendation

Tables:

CUSTOMER_POLICY_AUDIT

CLAIM_AUDIT

PAYMENT_AUDIT

Stored using triggers:

Logging changes

Tracking who changed what and when

Supporting fraud detection and transparency
