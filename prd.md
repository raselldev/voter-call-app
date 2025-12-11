# 🛡️ Product Requirements Document (PRD): VoterCall App

| Document Version | Date | Product Manager | Status |
| :--- | :--- | :--- | :--- |
| **1.0 (Final)** | December 2025 | Gemini AI Assistant | **Ready for Development** |

---

## 1. Introduction & Goals

| Field | Detail |
| :--- | :--- |
| **Product Name** | **VoterCall** |
| **Problem** | Users are constantly interrupted and financially harmed by unsolicited and fraudulent phone calls. |
| **Solution** | A community-driven mobile app that allows users to instantly check a phone number’s reputation based on collective user reports and a **weighted scoring system**. |
| **Core Value Proposition** | **Privacy-Focused & Community-Driven:** Provides trusted spam reports without requiring access to the user's private contact list. |

---

## 2. Success Metrics & Goals (MVP)

| Metric Type | Metric Name | Goal (Target) |
| :--- | :--- | :--- |
| **North Star** | **Daily Number Searches (DNS)** | High Volume |
| **Quality** | **Average Confidence Score** | >85% |
| **Business** | **Premium Conversion Rate** | 1-2% of Free Users converting to a paid plan. |

---

## 3. User Stories & Features

### Core Features

| ID | User Story | Priority |
| :--- | :--- | :--- |
| **US-1.1** | **As a user, I want to quickly search a number** so that I can see its instant reputation status. | P1 (Core) |
| **US-1.3** | **As a high-reputation user, I want my votes to carry more weight** than a new user’s votes so that the final score is trustworthy. | P1 (Integrity) |
| **US-2.1** | **As a subject user, I want to submit a formal Request for Exoneration** so that a community review is triggered to clear my number's reputation. | P2 (Ethical) |

### Tagging System (Nuanced Reporting)

Reports must be categorized beyond a binary Scam/Not Scam:

* **🚨 Scam/Fraud:** Clearly malicious activity (e.g., impersonation, phishing).
* **⚠️ Aggressive/Spam:** Unwanted, high-volume, but potentially legal calls (e.g., telemarketing, debt collection).
* **👤 Personal/Dispute:** Reports related to private conflicts (e.g., an ex or private debt) that are isolated and carry a low public weight.

---

## 4. Technical Architecture (Supabase-Only)

### 4A. Technology Stack

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Frontend** | **Flutter** | High-performance, single codebase. |
| **State Management** | **Riverpod** | Mandatory for type-safe, testable, and scalable state management. |
| **Local Storage** | **Hive & flutter_secure_storage** | Hive for fast configuration/lists; Secure Storage for Auth Token. |
| **Backend & DB** | **Supabase (PostgreSQL + Edge Functions)** | **Supabase-Only Commitment.** Central data storage and all core logic. |

### 4B. Core Performance & Integrity

* **High-Speed Lookup:** The primary lookup index will be a **Materialized View (`mv_phone_status`)** in Supabase Postgres to guarantee search results in **<100ms**.
* **Weighted Score Logic:** Score updates are computed using a **Postgres Function** based on the formula: $\text{Score} = f(\text{VoterReputation}_{\text{Score}})$.
* **Anti-Fraud:** Implement **API Rate Limiting** on Edge Functions (Velocity Check) and use **Disagreement Penalty** logic in the score calculation.
* **Data Decay:** Negative reports must expire or decay over time (e.g., 18 months).

---

## 5. UX/Design Requirements

### 5A. Color Palette

| Color Role | HEX Code | Rationale |
| :--- | :--- | :--- |
| **Primary Color** | **\#00508C** (Deep Blue) | Evokes trust, security, and stability. Used for branding. |
| **Secondary Color** | **\#F5A623** (Soft Gold/Amber) | High-contrast accent for active states. |
| **Danger/Scam** | **🔴 RED** (`#D0021B`) | High-risk warning color. |
| **Safe/Verified** | **🟢 GREEN** (`#417505`) | Approval color. |

### 5B. 4-Tier Status System (Color-Coding)

| Status Icon | Color Code | Designation | Description |
| :--- | :--- | :--- | :--- |
| **🚨** | **🔴 RED** | **HIGH RISK/SCAM** | Confirmed malicious, fraudulent activity. |
| **⚠️** | **🟡 YELLOW/AMBER** | **AGGRESSIVE/SPAM** | Unwanted commercial or aggressive activity. |
| **⚪** | **⚪ GRAY/WHITE** | **NEUTRAL/LOW DATA** | Insufficient community reports. (Default state). |
| **✅** | **🟢 GREEN** | **VERIFIED/SAFE** | Confirmed legitimate number. |

---

## 6. Navigation Architecture

VoterCall uses a hybrid system combining the BNB (high frequency) and a Drawer (low frequency).

### 6A. Authentication Screens

| Screen Name | State | Purpose |
| :--- | :--- | :--- |
| **Login / Register** | Logged Out | Account creation and access (Auth managed by Supabase). |

### 6B. Primary Navigation: Bottom Navigation Bar (BNB)

| Tab Name | Function | Rationale |
| :--- | :--- | :--- |
| **1. Search** | Core number lookup feature. | **Primary Value Proposition.** |
| **2. Activity** | User report history &
