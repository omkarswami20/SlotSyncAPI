# SlotSync API

A production-grade, concurrent appointment booking and scheduling REST API built with Node.js 22, Express 5, TypeScript, and raw parameterised SQL on SQLite / Turso.

Designed to handle real-world backend engineering challenges: concurrency-safe slot reservations, multi-timezone availability calculation, and strict role-based access control.

---

## Key Features

- **Concurrency-Safe Slot Booking**: Eliminates double-booking race conditions at the database level using atomic transactions and database triggers (`BEFORE INSERT` overlap prevention).
- **Time Zone & Interval Precision**: Stores all timestamps as UTC epoch milliseconds (`INTEGER`), utilizing half-open intervals `[start, end)` with Luxon for timezone conversions across IANA time zones.
- **Consultant Availability Engine**: Computes bookable slots dynamically from recurring weekly schedules, buffer times, and blocked dates.
- **Raw Parameterised SQL**: Built deliberately without an ORM to maintain full visibility over execution plans, query performance, and transactional boundaries.
- **Security & RBAC**: Multi-role authorization (Customer, Consultant, Admin) with granular ownership validation (customers cannot access or probe other users' reservations).
- **Type-Safe Validation**: Strict request payload validation and sanitization using Zod.

---

## Tech Stack

| Component | Technology |
| :--- | :--- |
| **Runtime & Language** | Node.js (>=22), TypeScript (strict mode, ES modules) |
| **Framework** | Express 5 |
| **Database** | Turso (hosted libSQL) / local SQLite for development & testing |
| **Database Access** | Parameterised raw SQL queries via `@libsql/client` (Zero ORM) |
| **Time Handling** | UTC epoch milliseconds + Luxon |
| **Validation** | Zod |
| **Testing** | Vitest + Supertest |
| **Code Quality** | ESLint + Prettier |

---

## Project Structure

```text
src/
├── app.ts                 # Express application configuration & base routes
├── server.ts              # HTTP server entrypoint
├── config/                # Environment and app configuration
├── db/                    # libSQL client and transaction management
├── lib/                   # Shared utilities (errors, time helpers)
├── middleware/            # Auth, rate limiting, error handling, request ID
└── modules/
    ├── admin/             # Platform administration & approvals
    ├── auth/              # Authentication & session management
    ├── availability/      # Consultant availability computation
    ├── bookings/          # Slot reservations & double-booking prevention
    ├── consultants/       # Consultant profiles, services & schedules
    ├── services/          # Service definitions (duration, pricing, buffer)
    └── users/             # User accounts & profile management
```

---

## Getting Started

### Prerequisites

- Node.js (>= 22.0.0)
- npm (>= 10.0.0)

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/omkarswami20/SlotSyncAPI.git
   cd SlotSyncAPI
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Setup environment variables:
   ```bash
   cp .env.example .env
   ```

4. Start development server:
   ```bash
   npm run dev
   ```

The API will be available at `http://localhost:3000`.

---

## Available Scripts

- `npm run dev` - Start development server with file watching (`tsx watch`)
- `npm run build` - Compile TypeScript to `dist/`
- `npm start` - Run production build
- `npm run typecheck` - Run TypeScript compiler check without emitting files
- `npm run lint` - Lint codebase using ESLint
- `npm run format` - Format files using Prettier

---

## API Endpoints (Health Check)

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/health` | Returns server health status |

---

## License

UNLICENSED (All rights reserved).
