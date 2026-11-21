# Spec additions: users, bird categories and breeds

These add to the Chicken Incubator Tracker SRS. Everything stays offline: accounts, categories and breeds are stored locally in SQLite.

## New functional requirements

### FR-13 — User accounts and roles

There are two roles: **admin** and **general**. An admin can create, edit and deactivate user accounts and set each account's role. A general user can manage batches and records but cannot open the admin screen.

Acceptance criteria: an admin can create a general user and an admin user. A general user who signs in does not see the admin screen. The app always keeps at least one active admin, so the last admin cannot be deactivated or demoted.

### FR-14 — Sign-in

Users sign in with a username and password. Passwords are stored as salted hashes using Python's standard library, never as plain text. On first run, the app asks the user to create the first admin account.

Acceptance criteria: a correct username and password signs in. A wrong password or an unknown username is rejected with the same message. The users table contains no plain-text passwords.

### FR-15 — Bird categories (admin only)

An admin can add, edit and remove bird categories, such as chicken, duck, quail, turkey and peafowl. Each category has a name, an incubation period in days and suggested candling days.

Acceptance criteria: a new category with a valid name and a positive incubation period is saved and appears in the batch form. Duplicate names are rejected. A category that has batches cannot be removed.

### FR-16 — Breeds per category (admin only)

An admin can add, edit and remove breeds within a category, for example Brahma under chicken or Pekin under duck.

Acceptance criteria: a breed is saved under one category and is listed only for that category. Duplicate breed names within the same category are rejected. A breed that has batches cannot be removed.

## Changes to existing requirements

- **FR-01 / FR-02:** the batch form selects a bird category, then a breed from that category, in place of free-text breed entry.
- **FR-04:** expected hatch date = start date + the incubation period of the batch's bird category (21 days for chicken). Acceptance: a chicken batch started January 1 hatches January 22. A batch from a category set to 28 days, started January 1, hatches January 29.
- **FR-10:** the suggested candling days come from the batch's bird category (days 7 and 14 for chicken).
- **Section 13, Out of scope:** multi-user accounts move into scope as local accounts only. Online accounts, email and SMS stay out of scope.

## Database additions

- **Users:** user ID, username (unique), password hash, salt, role (admin or general), active flag.
- **Bird categories:** category ID, name (unique), incubation days, candling days.
- **Breeds:** breed ID, category ID, name (unique within its category).
- **Batches:** the breed text field is replaced by a breed ID, which links to the category through the breeds table.

## Starter data (fictional, editable by the admin)

| Category | Incubation days | Candling days |
|---|---|---|
| Chicken | 21 | 7, 14 |
| Duck | 28 | 7, 14, 21 |
| Quail | 17 | 5, 12 |
| Turkey | 28 | 7, 14, 21 |
| Peafowl | 28 | 7, 14, 21 |

Check these values against your own sources before you rely on them. Some breeds differ from their category: Muscovy ducks, for example, take about 35 days.

## Decisions still open

- Should a breed be able to override its category's incubation period, for breeds like Muscovy duck?
- Do batches record which user created them?

## Revised session plan

1. App setup, sign-in, first-admin setup and the users table (FR-13, FR-14).
2. Admin screen: bird categories and breeds (FR-15, FR-16).
3. Batch creation, editing and deletion with category and breed selection (FR-01 to FR-03).
4. Per-category hatch dates, incubation progress and the multi-batch dashboard (FR-04 to FR-06).
5. Temperature, humidity and egg-turning records (FR-07 to FR-09).
6. Candling notes, hatch results and history (FR-10, FR-11).
7. Full test pass across all 16 requirements, bug fixes, documentation and optional analytics (FR-12).
