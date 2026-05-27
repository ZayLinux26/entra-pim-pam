# Entra ID PIM — Privileged Access Management Implementation

> Tier-based privileged access management for **Meridian Health Partners** using Microsoft Entra ID **Privileged Identity Management (PIM)**, Conditional Access, and Bitwarden credential vaulting.

**Status:** 🚧 In Progress  
**Trial window:** [activation date] – [expiration date]

---

## Project Context

This is Project 2 of 10 in my IAM Analyst portfolio, built around the same fictional healthcare organization — **Meridian Health Partners** — as Project 1. The healthcare framing is deliberate: HIPAA enforcement requires least-privilege administrative access, full auditability of privileged actions, and credential isolation that maps directly to PAM controls.

Project 1 (User Lifecycle Management) established the workforce, departmental groups, and lifecycle baseline. Project 2 takes that baseline and adds the privileged access layer — separating administrative identities from standard user identities, eliminating standing admin access, and enforcing just-in-time elevation for every privileged action.

### Why PAM as Project 2 instead of Project 5?

My target specialization is Privileged Access Management with CyberArk. Shipping PAM as Project 2 reorders this portfolio around the PAM specialization story rather than treating PAM as a late-stage capstone. The Entra PIM concepts implemented here (JIT activation, approval workflows, time-bound elevation, audit logging) map directly to CyberArk's session brokering, dual control, and credential rotation primitives — explicit mappings are documented at the end of this README.

---

## Tier Model (Microsoft Enterprise Access Model)

| Tier | Purpose | Meridian implementation |
|---|---|---|
| **Tier 0** | Identity infrastructure | One sealed break-glass account, **excluded** from PIM, MFA on a separate authenticator |
| **Tier 1** | Privileged operations | 4 admins, each with a distinct Entra directory role assigned as **eligible-only** via PIM |
| **Tier 2** | Workforce | All 15 active Meridian users from Project 1 — **zero** admin entitlements |

### Governing rules
- Higher tiers manage lower tiers; never the reverse
- Admin accounts sign in only to systems at or below their tier
- Admin accounts are never used for non-admin work (no email, no web browsing)

---

## Privileged Account Inventory

| Standard account | Admin account | Entra role | Activation policy |
|---|---|---|---|
| Jennifer Williams *(IT, Identity Ops Lead)* | `adm-jwilliams` | **Privileged Role Administrator** | 4 hrs / MFA / justification / **approval required** |
| Robert Kim *(IT)* | `adm-rkim` | **User Administrator** | 8 hrs / MFA / justification |
| Priya Sharma *(IT)* | `adm-psharma` | **Helpdesk Administrator** | 8 hrs / MFA / justification |
| Thomas Mitchell *(IT)* | `adm-tmitchell` | **Application Administrator** | 4 hrs / MFA / justification |
| — | `breakglass` | **Global Administrator** (permanent) | Excluded from PIM and Conditional Access. MFA on separate authenticator. Credentials sealed in Bitwarden. |

### Separation of duties

- **Jennifer (PRA)** can grant other people roles, but cannot reset their passwords
- **Robert (User Admin)** can create/modify users, but cannot grant himself Tier 0
- **Priya (Helpdesk)** can reset non-admin passwords, but cannot touch admin accounts (a built-in HD Admin restriction)
- **Thomas (App Admin)** can manage applications, but cannot touch users or grant roles
- **No single Tier 1 admin** can compromise the directory unilaterally. Even Jennifer's high-privilege role requires approval; tenant-level changes still require break-glass.

---

## Architecture

[Mermaid diagram coming — tier separation + PIM activation flow + break-glass isolation]

---

## Implementation Walkthrough

### 1. P2 Trial Activation
_To be completed during Phase 2._

### 2. Privileged Account Creation
_To be completed during Phase 3._

### 3. PIM Eligible Role Assignments
_To be completed during Phase 3._

### 4. Activation Policies & Approval Workflow
_To be completed during Phase 3._

### 5. Conditional Access Layering
_To be completed during Phase 3._

### 6. Break-Glass Account Design and Sealing
_To be completed during Phase 3._

### 7. Activation Demo (End-to-End)
_To be completed during Phase 3 — Jennifer requests PRA, Robert approves, full audit trail captured._

### 8. Access Reviews of Privileged Roles
_To be completed during Phase 3._

### 9. Standing Privilege Audit
_To be completed during Phase 3 — sweep of all 15+ accounts confirming zero admin entitlements on Tier 2._

---

## Mapping to CyberArk PAM Concepts

| CyberArk concept | Entra PIM equivalent in this project |
|---|---|
| Just-in-time access | Eligible-only PIM assignments; activation request → MFA → elevation |
| Dual control / approval workflow | PIM activation approval (configured on Privileged Role Admin) |
| Privileged Session Manager (PSM) | Session-bound elevation tied to PIM activation window |
| Vault (CPM credential rotation) | Bitwarden seal + manual rotation procedure (documented) |
| Audit & session monitoring | PIM audit logs + Entra sign-in logs + Identity Protection signals |
| Break-glass / firecall account | Sealed permanent Global Administrator, excluded from PIM and CA |

---

## Lessons Learned

_To be filled in during and after implementation. Planned topics:_
- P2 license economics: trial vs. production cost at scale
- What persists vs. what is wiped at P2 trial expiration
- Why break-glass must be excluded from Conditional Access
- PIM approvers don't need to hold the role they approve (and why that matters)
- Calibrating activation duration to blast radius
- Mapping PIM controls to CyberArk PAM concepts for interview conversations

---

## Project Artifacts

- `docs/tier-model-design.md`
- `docs/activation-policy-matrix.md`
- `docs/breakglass-seal-procedure.md`
- `docs/lessons-learned.md`
- `architecture/pam-architecture.mmd`
- `screenshots/`

---

**Project 2 of 10** in my [IAM Analyst Portfolio](https://github.com/ZayLinux26/iam-analyst-portfolio).
