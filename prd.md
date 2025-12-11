# 🛡️ Product Requirements Document (PRD): VoterCall App

| Document Version | Date | Product Manager | Status |
| :--- | :--- | :--- | :--- |
| **1.0 (Final Draft)** | December 2025 | AI Agent | **Ready for Development** |

---

## 1. Introduction & Goals

| Field | Detail |
| :--- | :--- |
| **Product Name** | **VoterCall** |
| **Problem** | Users are constantly interrupted and financially harmed by unsolicited and fraudulent phone calls. |
| **Solution** | A community-driven mobile app that allows users to instantly check a phone number’s reputation based on collective user reports and a weighted scoring system. |
| **Core Value Proposition** | **Privacy-Focused & Community-Driven:** Get trusted spam reports without requiring access to the user's private contact list. |
| **Scope** | Define the MVP features necessary for a successful, scalable, and trustworthy launch. |

---

## 2. Success Metrics (MVP)

| Metric Type | Metric Name | Goal (Target) |
| :--- | :--- | :--- |
| **North Star** | **Daily Number Searches (DNS)** | High Volume |
| **Quality** | **Average Confidence Score** | >85% |
| **Engagement** | **Active Users Contributing Reports** | 10% of DNS submit at least one report per week. |
| **Business** | **Premium Conversion Rate** | 1-2% of Free Users converting to a paid plan. |

---

## 3. Target Users & Personas

* **Primary Persona:** **The Cautious Consumer:** A smartphone user who values privacy and seeks instant verification for unknown numbers.
* **Key Behavior:** Must be able to quickly input or copy a number for instant status feedback.

---

## 4. User Stories & Features

### Core P1 Features (MVP)

| ID | User Story | Rationale |
| :--- | :--- | :--- |
| **US-1.1** | **As a user, I want to quickly search a number** so that I can see its instant reputation status. | Core Value Proposition. |
| **US-1.2** | **As a user, I want to submit a report** and tag a number (using nuanced tags) so that I can contribute to the community database and earn reputation. | Data collection mechanism. |
| **US-1.3** | **As a high-reputation user, I want my votes to carry more weight** than a new user’s votes so that the final score is trustworthy and protected from fraud. | Integrity & Anti-Fraud. |

### Data Quality & Integrity P2 Features

| ID | User Story | Rationale |
| :--- | :--- | :--- |
| **US-2.1** | **As a subject user, I want to submit a formal Request for Exoneration** so that a community review is triggered to clear my number's reputation. | Required for ethical fairness. |
| **US-2.2** | **As a high-reputation user, I want to vote on an Exoneration Request** so that the community is responsible for confirming the number's clearance. | Scalable integrity mechanism. |

---

## 5. Technical Requirements & Architecture

### 5A. Technology Stack (Supabase-Only)

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Frontend** | **Flutter** | Fast development, native-like performance, single codebase (iOS/Android). |
| **Backend & Database** | **Supabase (PostgreSQL + Edge Functions)** | Handles Auth/User data, and is configured to host the core lookup logic. |

### 5B. Core Integrity & Performance

| Requirement | Implementation Detail | Constraint |
| :--- | :--- | :--- |
| **High-Speed Lookup** | Use a **Materialized View (`mv_phone_status`)** in Supabase Postgres as the primary lookup index. | **MUST** deliver search results in $<100ms$. |
| **Weighted Score** | Score computation must be executed in a **Postgres Function**, relying on the stored reputation of the contributor ($\text{VoterReputation}_{\text{Score}}$). |
| **Data Freshness** | Use a **Database Trigger** to asynchronously initiate a `REFRESH MATERIALIZED VIEW CONCURRENTLY` upon high-reputation report submission. | Essential for keeping scores up-to-date without database contention. |

### 5C. Anti-Fraud & Privacy

* **Privacy Constraint:** The Flutter app **MUST NOT** request contact list access permissions.
* **Malicious Report Defense:** Implement a **Disagreement Penalty** logic: If a user’s vote is consistently overturned by high-reputation users, their own reputation score is severely penalized or reset.
* **Velocity Check:** Implement **API Rate Limiting** on the `/search` and `/report` Edge Function endpoints to mitigate bot and spamming attempts.
* **Data Decay:** Negative reports **MUST** expire or decay over time (e.g., 18 months) to allow for name clearing.

---

## 6. UX/Design Requirements (4-Tier Status)

The search result must be instant and use a four-tier color-coded system:

| Status Icon | Color Code | Designation | Description |
| :--- | :--- | :--- | :--- |
| **🚨** | **🔴 RED** | **HIGH RISK/SCAM** | Confirmed malicious, fraudulent, or high-volume criminal activity. |
| **⚠️** | **🟡 YELLOW/AMBER** | **AGGRESSIVE/SPAM** | Unwanted commercial, telemarketing, or aggressive debt collection. |
| **⚪** | **⚪ GRAY/WHITE** | **NEUTRAL/LOW DATA** | Insufficient community reports to confidently assign a score. **The default state.** |
| **✅** | **🟢 GREEN** | **VERIFIED/SAFE** | Confirmed legitimate business, low/no complaints, or high positive reports. |

---

## 7. Launch Plan & Timeline

| Phase | Focus | Key Milestone | Estimated Duration |
| :--- | :--- | :--- | :--- |
| **1: MVP Development** | Core Search, Weighted Score, Basic Reporting, Anti-Fraud Systems. | Internal Beta Ready. | 14 weeks |
| **2: Open Beta & Growth**| Monetization Integration, Nuanced Tagging, Community Exoneration. | Launch Subscription; Gather 5,000 community reports. | 8 weeks |

---

## 8. Monetization Strategy

| Strategy | Detail |
| :--- | :--- |
| **Model** | **Freemium** (Pay Per Month Subscription). |
| **Price Target** | **\$4.99/month** (Competitively priced below key market apps). |
| **Free Tier** | Unlimited Basic Search, Report Contribution, Reputation Earning (Data Provider). |
| **Premium Tier**| **Automated Call Blocking** (RED/YELLOW), Advanced Filtering, Ad-Free Experience, Profile Look-up History (Convenience & Control). |
