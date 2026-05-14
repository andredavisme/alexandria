# alexandria — Project Training Manual

> This manual covers what is unique to the alexandria repo. All global standards live in andredavisme/warrior-x-docs/operations/training-manual.md.

---

## What This Repo Is

alexandria is the database and data pipeline repo for the Warrior X ecosystem. All schemas, migrations, and data architecture decisions live here. It is the foundation that skunk-works and other frontend repos are built on.

## Who It Serves

All Warrior X projects that need structured data storage. Managed by André and any contributors working on data architecture.

## Tech Stack

- PostgreSQL (via Supabase)
- SQL migrations
- Supabase Row Level Security (RLS) policies
- Supabase Vault (for secrets)

## Schema Organization

- `public` — General app data (Skunk Works projects)
- `alexandria` — Reference data, training content, civic data

## Migration File Convention

All migration files must follow this exact format:

```
YYYYMMDD_NNN_description.sql
```

Examples:
```
20260514_001_create_locations_table.sql
20260514_002_add_rls_to_locations.sql
```

- Never edit a migration file once it has been applied to production
- Every migration that creates a table must be followed by an RLS policy migration
- Store migration files in the `/migrations` directory

## RLS Policy Standard

Every table must have RLS enabled. Minimum policies:
- Public read: allowed for non-sensitive community data
- Write: restricted to authenticated users or service role via Vault
- Never expose write access to anonymous users

## How to Contribute

1. Create a GitHub issue describing the schema change
2. Branch from main: `schema/<short-description>`
3. Write the migration SQL file following the naming convention
4. Include RLS policy in the same or a follow-up migration
5. Commit: `schema: <short description>`
6. Push and open a PR
7. Do NOT merge without review
8. Never apply to production without testing on a staging project first

## Security Rules

- No secrets, passwords, or API keys in any SQL file or commit
- Service role key goes in Supabase Vault only
- RLS must be enabled on every table — no exceptions
- Never grant public write access without explicit justification and review

---

## Chapter 15 — Post-Mortems & Lessons Learned

*Schema incidents, rollback events, and data decisions get recorded here.*

---

*Last updated: 2026-05-14 — Initial project training manual seeded.*
