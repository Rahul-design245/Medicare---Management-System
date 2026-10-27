# MediCare HPMS — Hospital Patient Management System (Phase 2)

A responsive, accessible web front end for **MediCare General Hospital**, built from the Phase 1 Figma designs (`MediCare-HPMS-Figma-UI`).
Patients, doctors, nurses and administrators can sign in and use role-specific dashboards for patient registration, appointment booking,
electronic medical records, online prescriptions, laboratory reports and online payments.

> **Academic prototype.** All names, IDs, amounts and clinical values are fictional. Authentication, data storage and payments are
> **simulated in the browser** — see [Security & scope](#security--scope).

**Technologies:** HTML5 · CSS3 (custom properties, grid, flexbox) · vanilla JavaScript (ES2017+) · Git & GitHub. No frameworks or build step.

**Repository:** `https://github.com/<your-username>/medicare-hpms` ← replace with your repository URL

---

## Run it in VS Code

**Option A — Live Server (recommended)**
1. Open the `medicare-hpms` folder in VS Code (*File → Open Folder…*).
2. Install the recommended extension **Live Server** (VS Code will prompt you; it's listed in `.vscode/extensions.json`).
3. Right-click `index.html` → **Open with Live Server**. The app opens at `http://127.0.0.1:5500/index.html`.

**Option B — npm**
```bash
npm start          # serves the folder on http://localhost:5500 (needs Node.js)
```

**Option C — no tools:** double-click `index.html` to open it directly in Chrome, Edge or Firefox.

## Demo accounts

Password for every account: **`MediCare@2026`** · Staff 2-step verification code: **`246810`**

| Role | Sign in as | Email / ID |
|---|---|---|
| Patient | Patient | `alex.perera@example.com` or `MC-2026-04817` |
| Patient 2 | Patient | `sam.silva@example.com` |
| Doctor | Doctor | `nimal.silva@medicare.lk` |
| Nurse | Nurse | `malini.dias@medicare.lk` |
| Administrator | Admin | `kasun.j@medicare.lk` |

The sign-in page also has a **“Demo accounts”** panel with one-click fill. You can register a new patient via **Create an account**.

**Test cards (Payments):** `4242 4242 4242 4242` → success · `4000 0000 0000 0077` → pending · `4000 0000 0000 0002` → declined.
Use any future expiry (e.g. `12/29`) and any 3-digit CVV. Online banking and LankaQR return *pending*, which confirms after ~8 seconds (Refresh status).

To restore the original data, use **Profile & Settings → Reset demo data** (patient) or **System Settings → Reset demo data** (admin).

## Project structure

```
medicare-hpms/
├── index.html                 Login (01-Login / 01b-Validation)
├── pages/
│   ├── register.html          Patient registration
│   ├── patient-dashboard.html 02-Patient-Dashboard
│   ├── appointments.html      Patient appointment list (view / cancel / reschedule)
│   ├── book-appointment.html  04-Appointment-Booking + 04b-Booking-Confirmation
│   ├── medical-records.html   05-Medical-Records
│   ├── prescriptions.html     Online prescriptions + refill requests
│   ├── lab-reports.html       Laboratory report access
│   ├── payments.html          06-Online-Payment
│   ├── payment-status.html    06b-Payment-Status (success / pending / failed)
│   ├── profile.html           Patient profile & settings
│   ├── admin-dashboard.html   03-Administrator-Dashboard
│   ├── admin.html             Admin views: appointments, patients, staff, departments, billing, reports, audit log, settings
│   └── staff-dashboard.html   Doctor & nurse dashboard (assigned patients, chart, notes, vitals)
├── css/
│   ├── variables.css          Design tokens from 00-Design-System (colours, type scale, spacing)
│   ├── base.css               Reset, typography, focus styles, utilities
│   ├── components.css         Buttons, forms, badges, cards, tables, tabs, modals, toasts
│   ├── layout.css             App shell (sidebar, top bar) + responsive breakpoints + print
│   ├── auth.css               Login & registration
│   └── pages.css              Page-specific styles
├── js/
│   ├── icons.js               Lucide icon sprite
│   ├── data.js                Fictional seed data (dates relative to today)
│   ├── auth.js                Demo session handling, salted SHA-256, inactivity limit
│   ├── store.js               Simulated back end (mock API with role & ownership checks, audit log)
│   ├── ui.js                  Shared UI helpers (modal focus trap, toasts, validation, downloads)
│   ├── layout.js              Shared sidebar/top bar component, role guard, search, notifications
│   └── pages/*.js             One script per page
├── assets/favicon.svg
├── docs/                      Development notes (Markdown sources of the PDFs)
├── package.json               `npm start` convenience script
└── .vscode/                   Recommended extensions & Live Server settings
```

## Main features

| Requirement | Where |
|---|---|
| Patient registration | `pages/register.html` — full validation, error summary, password strength, consent |
| Appointment booking | 4-step flow, live availability, past dates & booked slots blocked, double-booking & duplicate-submit protection, unique reference, `.ics` export, cancel / reschedule |
| Electronic medical records | List + detail, filters, search, print, sample PDF download, secure share dialog, access-checked by ID |
| Online prescriptions | Active / past prescriptions, refill requests |
| Laboratory report access | Status filter, processing state, sample PDF reports |
| Online payments | Card (Luhn, expiry, CVV), online banking, LankaQR; success / pending / failed; duplicate-payment prevention; receipts |
| Role-based interfaces | Patient, Doctor, Nurse, Administrator dashboards; page guard + API-level checks |
| Administration | Live statistics from data, appointment status management, reassignment, patients, staff, departments, billing, reports, CSV export, audit log, settings |

## Accessibility (WCAG 2.2 AA)

Skip links · semantic landmarks · visible focus rings · full keyboard support (tabs, calendar arrow keys, menus, dialogs) ·
focus trapped in dialogs and returned on close · labelled fields with linked error messages and error summaries · `aria-live`
announcements for toasts and results · status never shown by colour alone · alt text / `aria-label` for icons ·
contrast checked with axe-core (0 violations on all pages) · responsive from 320 px to desktop with no horizontal scroll ·
`prefers-reduced-motion` respected · print styles.

## Security & scope

This is a **front-end prototype**. What is real and what is simulated:

| Area | In this prototype | Needed in production |
|---|---|---|
| Authentication | Demo accounts, salted SHA-256 hashes, generic errors, 5-attempt lockout, staff 2-step code, 15-min inactivity sign-out | Server-side authentication, secure HTTP-only session cookies, real MFA |
| Authorisation | Page guard + every mock-API call checks role and record ownership; denied attempts are audited | The same checks on the **server** for every request — browser checks can be bypassed |
| Data | `localStorage` in the browser, fictional data | Encrypted database, backups, retention policy |
| Payments | Simulated gateway; only card last-4 kept; full number & CVV never stored | PCI-DSS payment provider, server-verified callbacks |
| Audit log | Append-only in the UI | Tamper-evident server-side logging |

A hidden menu item or a redirect is **not** a security boundary — this is documented in the code comments as well.

## Version control

```bash
git remote add origin https://github.com/<your-username>/medicare-hpms.git
git push -u origin main
```
The repository already contains a commit history (one commit per feature).
