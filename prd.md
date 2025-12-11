# 🛡️ Product Requirements Document (PRD): VoterCall App

| Document Version | Date | Product Manager | Status |
| :--- | :--- | :--- | :--- |
| **1.0 (Final Draft)** | December 2025 | Gemini AI Assistant | **Ready for Development** |

---

## 1. Introduction & Goals

| Field | Detail |
| :--- | :--- |
| **Product Name** | **VoterCall** |
| **Problem** | Users are constantly interrupted and financially harmed by unsolicited and fraudulent phone calls. |
| **Solution** | A community-driven mobile app that allows users to instantly check a phone number’s reputation based on collective user reports and a weighted scoring system. |
| **Core Value Proposition** | **Privacy-Focused & Community-Driven:** Get trusted spam reports without requiring access to the user's private contact list. |

---

## 2. Success Metrics (MVP)

| Metric Type | Metric Name | Goal (Target) |
| :--- | :--- | :--- |
| **North Star** | **Daily Number Searches (DNS)** | High Volume |
| **Quality** | **Average Confidence Score** | >85% |
| **Business** | **Premium Conversion Rate** | 1-2% of Free Users converting to a paid plan. |

---

## 3. User Stories & Features (Selection)

| ID | User Story | Priority |
| :--- | :--- | :--- |
| **US-1.1** | **As a user, I want to quickly search a number** (via manual input or copy-paste) so that I can see its instant reputation status. | P1 (Core) |
| **US-1.3** | **As a high-reputation user, I want my votes to carry more weight** than a new user’s votes so that the final score is trustworthy. | P1 (Integrity) |
| **US-2.1** | **As a subject user, I want to submit a formal Request for Exoneration** so that a community review is triggered to clear my number's reputation. | P2 (Ethical) |

---

## 4. Technical Architecture (Supabase-Only)

### 4A. Technology Stack

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Frontend** | **Flutter** | High-performance, single codebase. |
| **State Management** | **Riverpod** | Mandatory for type-safe, testable, and scalable state management. |
| **Local Storage** | **Hive & flutter_secure_storage** | Hive for fast configuration/lists. Secure Storage for Auth Token. |
| **Backend & DB** | **Supabase (PostgreSQL + Edge Functions)** | **Supabase-Only Commitment.** Central data storage and complex scoring logic. |

### 4B. Core Integrity & Performance

| Requirement | Implementation Detail | Constraint |
| :--- | :--- | :--- |
| **High-Speed Lookup** | Use a **Materialized View (`mv_phone_status`)** in Supabase Postgres as the primary lookup index. | **MUST** deliver search results in $<100ms$. |
| **Weighted Score** | The weighted score formula must be computed in a **Postgres Function** and pushed to the Materialized View. | Score must be weighted by the reputation of the contributing user ($\text{VoterReputation}_{\text{Score}}$). |
| **Data Freshness** | Use a **Supabase Database Trigger** to asynchronously initiate a `REFRESH MATERIALIZED VIEW CONCURRENTLY` upon high-reputation report submission. | Essential for keeping scores up-to-date without locking the database. |
| **Anti-Fraud** | Implement **API Rate Limiting** on the `/search` and `/report` Edge Function endpoints (Velocity Check) and use **Disagreement Penalty** logic in the score calculation. |

---

## 5. UX/Design Requirements

### 5A. Final Color Palette

| Color Role | HEX Code | Rationale |
| :--- | :--- | :--- |
| **Primary Color** | **\#00508C** (Deep Blue) | Evokes trust, security, and stability. |
| **Secondary Color** | **\#F5A623** (Soft Gold/Amber) | High-contrast accent for highlights and active states. |
| **Danger/Scam** | **🔴 RED** (`#D0021B`) | Standard warning color. |
| **Safe/Verified** | **🟢 GREEN** (`#417505`) | Standard approval color. |

### 5B. 4-Tier Status System

| Status Icon | Color Code | Designation |
| :--- | :--- | :--- |
| **🚨** | **🔴 RED** | **HIGH RISK/SCAM** |
| **⚠️** | **🟡 YELLOW/AMBER** | **AGGRESSIVE/SPAM** |
| **⚪** | **⚪ GRAY/WHITE** | **NEUTRAL/LOW DATA** |
| **✅** | **🟢 GREEN** | **VERIFIED/SAFE** |

---

## 6. Monetization Strategy

| Strategy | Detail |
| :--- | :--- |
| **Model** | **Freemium** (Pay Per Month Subscription). |
| **Price Target** | **\$4.99/month** (Competitively priced below key market apps). |
| **Premium Tier**| **Automated Call Blocking** (RED/YELLOW), Advanced Filtering, Ad-Free Experience. |
