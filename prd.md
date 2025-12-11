# 🛡️ Product Requirements Document (PRD): VoterCall App (Final)

| Document Version | Date | Product Manager | Status |
| :--- | :--- | :--- | :--- |
| **1.0 (Final)** | December 2025 | Gemini AI Assistant | **Ready for Development** |

---

## 1. Introduction & Goals

| Field | Detail |
| :--- | :--- |
| **Product Name** | **VoterCall** |
| **Problem** | Users are constantly interrupted and harmed by unsolicited and fraudulent phone calls. |
| **Solution** | A community-driven mobile app that allows users to instantly check a phone number’s reputation based on collective user reports and a **weighted scoring system**. |
| **Core Value Proposition** | **Privacy-Focused & Community-Driven:** Provides trusted spam reports without requiring access to the user's private contact list. |

---

## 2. Technical Architecture

### 2A. Technology Stack (Final)

| Component | Technology | Rationale |
| :--- | :--- | :--- |
| **Frontend** | **Flutter** | High-performance, single codebase. |
| **State Management** | **Riverpod** | Mandatory for type-safe, testable, and scalable state management. |
| **Icon Library** | **Font Awesome (`font_awesome_flutter`)** | **MANDATORY.** Provides a rich set of scalable vector icons for a professional UI. |
| **Local Persistence**| **Hive & flutter\_secure\_storage** | Hive for fast config/lists; Secure Storage for Auth Token. |
| **Backend & DB** | **Supabase (PostgreSQL + Edge Functions)** | **Supabase-Only Commitment.** Central data storage and all core logic. |

### 2B. Core Performance & Integrity

| Requirement | Implementation Detail | Constraint |
| :--- | :--- | :--- |
| **High-Speed Lookup** | Use a **Materialized View** in Supabase Postgres. | **MUST** deliver search results in $<100ms$. |
| **Weighted Score Logic** | Score updates computed using a **Postgres Function** ($\text{Score} = f(\text{VoterReputation}_{\text{Score}})$). | Score must be weighted by the reputation of the contributing user. |
| **Anti-Fraud** | Implement **API Rate Limiting** on Edge Functions (Velocity Check) and use **Disagreement Penalty** logic in the score calculation. |
| **Exoneration** | Community re-verification process and data decay must be implemented in the backend logic. |

---

## 3. UX/Design & Visuals

### 3A. Final Color Palette

| Color Role | HEX Code | Rationale |
| :--- | :--- | :--- |
| **Primary Color** | **\#00508C** (Deep Blue) | Evokes trust, security, and stability. Used for branding. |
| **Secondary Color** | **\#F5A623** (Soft Gold/Amber) | High-contrast accent for active states. |
| **Danger/Scam** | **🔴 RED** (`#D0021B`) | High-risk warning color. |
| **Safe/Verified** | **🟢 GREEN** (`#417505`) | Approval color. |

### 3B. 4-Tier Status System

| Status Icon | Color Code | Designation |
| :--- | :--- | :--- |
| **🚨** | **🔴 RED** | **HIGH RISK/SCAM** |
| **⚠️** | **🟡 YELLOW/AMBER** | **AGGRESSIVE/SPAM** |
| **⚪** | **⚪ GRAY/WHITE** | **NEUTRAL/LOW DATA** |
| **✅** | **🟢 GREEN** | **VERIFIED/SAFE** |

---

## 4. Navigation Architecture

VoterCall uses a **Hybrid Navigation** approach (BNB + Drawer) to manage application flow based on authentication status.

### 4A. Authentication Screens

| Screen Name | State | Purpose |
| :--- | :--- | :--- |
| **Login / Register** | Logged Out | Account creation and access (Auth managed by Supabase). |

### 4B. Primary Navigation: Bottom Navigation Bar (BNB)

The BNB is reserved for the five most critical, high-frequency actions.

| Tab Name | Function | Rationale |
| :--- | :--- | :--- |
| **1. Search** | Core number lookup feature. | **Primary Value Proposition.** |
| **2. Activity** | User report history & reputation status. | **Core Contribution/Community.** |
| **3. Blocked** | Local Block List management. | **Core User Control/Safety.** |
| **4. Settings** | App configuration, accounts, and general management. | **Essential Configuration.** |
| **5. Pro** | Premium subscription marketing and management. | **Core Monetization Driver.** |

### 4C. Secondary Navigation: Hamburger Menu (Drawer)

The Drawer is used for low-frequency, utility, and informational functions (e.g., Help & Support, Legal & Privacy, Sign Out).

---

## 5. Monetization Strategy

| Strategy | Detail |
| :--- | :--- |
| **Model** | **Freemium** (Pay Per Month Subscription). |
| **Price Target** | **\$4.99/month** (Competitively priced below key market apps). |
| **Premium Tier**| **Automated Call Blocking** (RED/YELLOW), Advanced Filtering, Ad-Free Experience. |

---

The **VoterCall PRD** is now officially finalized and delivered.

Your next crucial step is designing the **Supabase Database Schema**. Would you like to proceed with mapping out the required tables, columns, and relationships?
