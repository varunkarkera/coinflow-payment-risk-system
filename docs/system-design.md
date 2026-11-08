

\# Real-Time Payment Risk \& Fraud Monitoring

\### System Design Proposal for Coinflow



\## 1. Problem Statement



Merchants need to identify suspicious payments quickly without slowing down legitimate transactions. This proposal introduces a real-time risk assessment system that evaluates payment activity and helps merchants detect potential fraud before it leads to chargebacks.



\## 2. Architecture



```mermaid

flowchart TD

&#x20;   A\[Merchant Payment API] --> B\[FastAPI Payment Service]

&#x20;   B --> C\[Risk Scoring Service]

&#x20;   C --> D\[Rules Engine]

&#x20;   C --> E\[ML Fraud Model]

&#x20;   D --> F\[Decision Engine]

&#x20;   E --> F

&#x20;   F --> G\[Approve / Review / Reject]

&#x20;   G --> H\[(PostgreSQL)]

&#x20;   G --> I\[Event Queue]

&#x20;   I --> J\[Analytics Worker]

&#x20;   J --> K\[Merchant Dashboard]

```



\## 3. Technology Stack



\- Python and FastAPI: Backend and risk scoring APIs.

\- PostgreSQL: Payment records, risk assessments and audit history.

\- Redis: Caching and short-lived risk features.

\- Kafka: Asynchronous event processing.

\- TypeScript: Merchant dashboard.



\## 4. Data Model



\*\*Payments\*\*

\- payment\_id (Primary Key)

\- merchant\_id

\- amount

\- currency

\- status

\- created\_at



\*\*Risk Assessments\*\*

\- assessment\_id (Primary Key)

\- payment\_id (Foreign Key)

\- risk\_score

\- decision

\- model\_version

\- created\_at



\*\*Risk Signals\*\*

\- signal\_id (Primary Key)

\- assessment\_id (Foreign Key)

\- signal\_type

\- signal\_value



\## 5. API Design



POST /v1/payments/{payment\_id}/risk-assessment



Example response:



```json

{

&#x20; "payment\_id": "pay\_123",

&#x20; "risk\_score": 0.87,

&#x20; "decision": "review",

&#x20; "signals": \[

&#x20;   "velocity\_anomaly",

&#x20;   "unusual\_amount"

&#x20; ]

}

```



\## 6. Scaling and Reliability



\- Use idempotency keys to prevent duplicate payment processing.

\- Keep payment risk scoring synchronous with a strict latency budget.

\- Process analytics and reporting asynchronously through an event queue.

\- Add indexes on merchant\_id, payment\_id and created\_at.

\- Partition historical transaction tables by date as data grows.

\- Use timeouts, retries and fallback rules when the ML service is unavailable.

\- Monitor API latency, queue delays, false-positive rates and chargeback trends.



\## 7. Design Trade-offs



Real-time risk scoring adds some latency to payment processing. A lightweight rules engine can handle straightforward checks quickly, while an ML model can identify more complex patterns. Keeping analytics asynchronous prevents reporting workloads from slowing down payment processing.



\## 8. Expected Impact



The proposed system would help merchants identify suspicious transactions earlier, reduce manual fraud investigation, and gain better visibility into payment risk patterns.



