# Codebase Tour: HabitRPG/habitica

*Live queried via the GitHub API on 2026-09-30 â€” https://github.com/HabitRPG/habitica (branch: `develop`). Used as a real-world reference codebase to pressure-test the Streakly Comeback screen spec against an actual production streak/habit system.*

## 1. PM-Level Tour

**What it does, in one sentence:** Habitica is a habit- and task-tracking app that gamifies real-life productivity â€” your to-dos become an RPG where you level up a character, earn gold and gear for consistency, and take damage when you skip commitments.

**Codebase organization (monorepo):**

| Folder | What it does |
|---|---|
| `website/server` | Node/Express API â€” controllers, Mongoose models, middleware. The backend source of truth. |
| `website/client` | Vue 2 SPA (Vite, Vue Router, Bootstrap-Vue) â€” the actual app UI. |
| `website/common` | **Isomorphic shared logic** â€” runs on both client and server. Scoring, cron/reset logic, and content definitions (spells, gear, quests) all live here so the client can score optimistically and the server can later validate the same way. |
| `migrations/` | Database schema/data migrations â€” evidence that model changes are a tracked, deliberate process, not ad hoc. |
| `test/` | Mocha test suite, mirrors source structure (`test/api/`, `test/common/`, `test/content/`). |
| `scripts/`, `kubernetes/`, `.ebextensions/`, `Dockerfile*` | Deployment/infra â€” confirms this ships as a containerized, orchestrated service, not a simple monolith. |

**The 3 most important files as a PM:**

1. **[`website/common/script/ops/scoreTask.js`](https://github.com/HabitRPG/habitica/blob/develop/website/common/script/ops/scoreTask.js)** â€” the actual streak increment/reset/reward logic. This is the single file that defines, in code, what "breaking a streak" means numerically and behaviorally. Any feature touching streak behavior touches this file.
2. **[`website/server/models/task.js`](https://github.com/HabitRPG/habitica/blob/develop/website/server/models/task.js)** â€” the Task schema (habit/daily/todo/reward types). `streak` is a field here, not on the user.
3. **[`website/server/controllers/api-v3/cron.js`](https://github.com/HabitRPG/habitica/blob/develop/website/server/controllers/api-v3/cron.js)** (+ `website/server/models/user/schema.js`) â€” the daily trigger endpoint that applies damage/resets, and where per-user protective state (`stats.buffs.streaks`) and permanent achievement counters (`achievements.streak`) live.

**Key data models & what they reveal about product decisions:**

- `streak` lives on the **Daily task itself**, not on the user. A single user can be running many independent streaks in parallel (one per daily habit) â€” a meaningfully different model from a single account-level streak. Worth flagging if Streakly's data model assumes one streak per user.
- `user.stats.buffs.streaks` (Boolean, confirmed in schema) is Habitica's actual "freeze" mechanic: casting the Wizard spell **"Chilling Frost"** (level 14, 40 mana, cast *before* the daily reset runs) sets this flag true for that cron cycle, and the reset logic explicitly checks it (`if (!user.stats.buffs.streaks ...) task.streak = 0;`). This is a **same-day, all-dailies, transient buff** â€” not a bankable, stockpiled resource, and not something a new user can access (it's gated behind reaching character level 14). This tells you Habitica deliberately chose to make streak protection something you *earn through progression*, not something granted to protect new users early â€” the opposite instinct from Streakly's proposed starter-freeze kit.
- `user.achievements.streak` is a **separate, permanent counter** (incremented every time a task's streak crosses a multiple of 21), distinct from the task's own `streak` number, which resets to 0. This is Habitica's real analog to "best streak, kept forever" â€” direct validation that the pattern in Streakly's Comeback screen (permanent record + resettable counter) is an established, working idea in a real, similar product.

## 2. Mapping the Comeback Screen Feature to This Codebase

**Where it would live:**
- **Server:** a new persistent field (freeze-bank count, with a max and an earn-rate) would need to be added â€” most naturally near `task.streak` in `task.js` or as a new user-level field, since Habitica has no existing "stockpile" concept to extend. The reset check itself would be a new branch inserted at the exact line in `scoreTask.js` where `task.streak = 0` currently happens unconditionally (once the buff check fails). A new notification type would be added to `NOTIFICATION_TYPES` in `userNotification.js`, alongside the existing `STREAK_ACHIEVEMENT` entry.
- **Client:** a new Vue view/modal, most naturally reusing the existing pattern Habitica already has for surfacing milestone notifications (e.g., how `STREAK_ACHIEVEMENT` or `LOGIN_INCENTIVE` trigger a popup) rather than building new UI plumbing from scratch.

**Existing components it would touch or depend on:**
- **The cron job** (`libs/cron.js` + `common/script/cron.js` + the `/cron` API endpoint) â€” the trigger point for *all* streak-related state changes, run for every active user daily. Touching this is inherently high-risk by nature of what it is.
- **`scoreTask.js`** â€” shared/isomorphic. A bug here doesn't stay contained to "the comeback feature" â€” it runs on both the client's optimistic local scoring and the server's authoritative scoring for every task type, every user.
- **`userNotification.js` / `pushNotifications.js`** â€” for surfacing the comeback/freeze-saved moment.
- **`user.stats.buffs`** â€” an existing precedent for "temporary protective state" exists, but it's same-day/transient, not persistent. A genuinely bankable, multi-day freeze is new territory, not a drop-in extension of what's already there.

**Blast radius:** `scoreTask.js` and the cron pipeline are on the critical path for the entire app's core loop â€” not an isolated corner. A bug introduced here risks miscalculating gold/XP rewards, incorrectly zeroing unrelated streaks, or double-firing notifications **app-wide**, not just within the new feature's surface area. This is about as close to the core engine as a change can get.

## 3. What Would Affect How I Write the Spec

**Data that doesn't exist yet:**
- A persistent, bankable freeze-count field with a max cap and an earn-rate. Habitica's only existing analog (`buffs.streaks`) is a same-day boolean, not a stockpile â€” this is net-new schema, not an extension.
- Nothing tracks "days active"/tenure directly for computing an earn-rate; it would need to be derived from account-creation date or a new counter.

**Existing constraints to call out in the ticket:**
- Real-world precedent shows streak protection is often **progression-gated** (Habitica requires level 14) as a deliberate design choice, not an oversight. If the spec proposes giving equivalent protection *free to brand-new users* (the opposite philosophy), that divergence should be stated explicitly, not left implicit â€” someone will ask why.
- The reset logic lives inside a shared, isomorphic scoring function on the critical path for the core loop. Any change here needs test coverage on **both directions** (completing vs. un-completing a task â€” `scoreTask.js` has separate branches for each), not just the new "break" case.
- An existing test suite mirrors the source layout (`test/common/`, `test/api/`) â€” the spec should require new tests land in the matching paths, not rely on manual QA alone.

**The one thing engineers will ask before kickoff:** *"Is the freeze count a new persistent field with a defined max and earn-rate, and is it computed client-optimistic or server-authoritative?"* This is the same open question already flagged for Raj in [triad-session.md](../03-build/triad-session.md) â€” and real precedent here confirms it's a genuine architectural decision (Habitica's client does optimistic local scoring later reconciled server-side), not a minor detail. Answering it before kickoff avoids re-litigating architecture live in the room.
