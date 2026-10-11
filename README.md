# ParcelMate AI

> Smart campus parcel management powered by AI agent orchestration.

**Hackerdorm Track 2 · WorkBuddy AI**
**Group:** 404 Not Founders
**Campus:** Swinburne University of Technology

![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind-3-06B6D4?logo=tailwindcss&logoColor=white)
![Express](https://img.shields.io/badge/Express-4-000000?logo=express&logoColor=white)
![Jest](https://img.shields.io/badge/Tests-84%20passing-4ADE80?logo=jest&logoColor=white)

---

## What it does

ParcelMate AI is an end-to-end parcel locker system for university campuses:

- **Register** a parcel with a tracking number, recipient phone, and size
- **Deposit** it in a smart locker — the agent assigns the compartment and generates a 6-digit pickup PIN
- **Notify** the student via simulated SMS/email
- **Collect** at a kiosk using the PIN, with automatic late-fee calculation (RM1/hour after 60 min free)
- **Edit / cancel** a deposit from the receipt before collection
- **Privacy-safe** lookup: PINs are only revealed for the specific tracking number the student enters

The entire workflow is orchestrated by a **WorkBuddy AI agent** with 20 specialized tools — each phase validated and audited before the next begins.

---

## Live Demo

The app is deployed and publicly accessible:

**[Live Demo →](https://1b3c8c27998b424386551a95d0599434.sg.agentos-app.run)**

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite 5, Tailwind CSS 3, TypeScript |
| Backend | Express 4, TypeScript (tsx runtime), JSON DB |
| Testing | Jest, ts-jest — 84 automated tests |
| Agent | WorkBuddy AI — 20 tools, 5-phase workflow |

---

## Quick Start

```bash
# Install dependencies
npm install

# Run dev server (frontend + API on http://localhost:3000)
npm run dev

# Build production bundle
npm run build

# Run tests (84 tests)
npm test
```

The database auto-seeds on first run — no setup required.

---

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   React UI      │ ──> │   Express API    │ ──> │   JSON DB       │
│   (Vite)        │ <── │   (router.ts)    │ <── │   data/db.json  │
└─────────────────┘     └──────────────────┘     └─────────────────┘
                              │
                              ▼
                       ┌──────────────────┐
                       │  WorkBuddy Agent │
                       │  20 tools        │
                       └──────────────────┘
```

### Agent Workflow (5 phases, 20 tools)

1. **Validate & Register**
   - `validate_parcel_information` — check tracking number, recipient phone, size
   - `find_compatible_locker` — match parcel size to available locker
   - `register_parcel` — create parcel record, reserve locker

2. **Generate PIN & Confirm Deposit**
   - `generate_pickup_code` — crypto-secure 6-digit PIN
   - `confirm_deposit` — mark locker OCCUPIED, stamp deposit time
   - `send_student_notification` — simulated SMS with PIN

3. **Kiosk Authentication**
   - `validate_pickup_code` — verify PIN, check expiry
   - `calculate_late_fee` — RM1/hour after 60 min free period

4. **Payment & Collection**
   - `process_payment` — simulated DuitNow for overdue fees
   - `unlock_locker` — authorize compartment access
   - `collect_parcel` — mark COLLECTED, revoke PIN, free locker

5. **Verify Final State**
   - `verify_final_state` — audit trail, locker status, PIN revocation
   - Plus: `edit_parcel`, `cancel_parcel`, `lookup_parcel`, `faq_search`

---

## Testing

- **84 automated tests** — all passing
- Coverage spans:
  - Registration & locker assignment
  - Deposit & PIN generation
  - Late-fee math (free period, hourly rate, max cap)
  - Collection & PIN revocation
  - Edit/cancel flows (v2)
  - Privacy-safe filtered notification inbox (v2)
  - Locker details with PIN display (v2)
  - FAQ privacy (no sensitive data leaked)
  - Regression (24 lockers after operations)

```bash
npm test
```

---

## Project Structure

```
parcelmate-ai/
├── src/
│   ├── components/           # React UI
│   │   ├── RegisterParcel.tsx     # Register + deposit + edit/cancel
│   │   ├── LockerWall.tsx         # 24-locker grid + details panel
│   │   ├── StudentLookup.tsx      # Tracking lookup + filtered inbox
│   │   ├── KioskCollection.tsx    # PIN entry + payment + collection
│   │   ├── FAQAssistant.tsx       # FAQ with privacy filtering
│   │   ├── ActivityHistory.tsx    # Audit trail viewer
│   │   └── NavBar.tsx             # Navigation
│   ├── server/
│   │   └── router.ts              # Express API router (all endpoints)
│   ├── lib/
│   │   ├── agent.ts               # WorkBuddy agent (20 tools)
│   │   ├── db.ts                  # JSON DB with auto-seed
│   │   ├── crypto-util.ts         # Crypto-secure PIN generation
│   │   ├── fee.ts                 # Late-fee calculation
│   │   └── faq.ts                 # FAQ knowledge base
│   ├── types/
│   │   └── index.ts               # Shared TypeScript types
│   ├── App.tsx                    # Root component
│   ├── main.tsx                   # React entry point
│   └── globals.css                # Tailwind directives
├── tests/
│   └── parcelmate.test.ts         # 84 Jest tests
├── public/                        # Built frontend (for deployment)
│   ├── assets/
│   │   ├── index-Bgp9XwR6.js      # React bundle (200KB)
│   │   └── index-CApLgcps.css     # Tailwind CSS (17KB)
│   └── index.html
├── data/                          # Runtime DB (auto-seeds)
├── pitch/                         # Presentation materials
│   ├── presentation.html          # 7-slide HTML deck
│   ├── ParcelMate_AI_Pitch.pdf    # PDF version
│   ├── STORY.md                   # Narrative structure
│   └── DESIGN.md                  # Visual spec
├── server.ts                      # Express entry point
├── vite.config.ts                 # Vite config
├── tsconfig.json                  # TypeScript config
├── tailwind.config.cjs            # Tailwind config
├── jest.config.cjs                # Jest config
├── postcss.config.cjs             # PostCSS config
├── package.json
├── .env.example                   # Environment variable reference
└── README.md
```

---

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/lockers` | List all 24 lockers |
| GET | `/api/lockers/:id/details` | Locker details (enriched for occupied) |
| POST | `/api/parcels/register` | Register a new parcel |
| POST | `/api/parcels/deposit` | Confirm deposit + generate PIN |
| POST | `/api/parcels/edit` | Edit parcel details |
| POST | `/api/parcels/withdraw` | Cancel/withdraw a deposit |
| GET | `/api/parcels/lookup?tracking_number=X` | Lookup parcel by tracking number |
| GET | `/api/notifications?tracking_number=X` | Filtered notifications (PIN shown) |
| GET | `/api/notifications` | All notifications (PINs stripped) |
| POST | `/api/kiosk/validate-pin` | Validate PIN at kiosk |
| POST | `/api/kiosk/collect` | Collect parcel with PIN |
| POST | `/api/kiosk/process-payment` | Process late-fee payment |
| POST | `/api/reset` | Reset DB to seed state |
| GET | `/api/faq` | FAQ search |

---

## License

MIT — built for Hackerdorm Track 2 competition.

---

## Team

**404 Not Founders** · Swinburne University of Technology
