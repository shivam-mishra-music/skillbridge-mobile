# SkillBridge

A mobile app that helps students find teammates for projects and hackathons, and exchange skills with each other.

**Tech stack:** React Native (Expo) · JavaScript · React Navigation · Supabase (PostgreSQL + Auth)

## Features

- **Sign up / log in** with email and password
- **Profile** with skills you have and skills you want to learn, each with a level (Beginner / Intermediate / Advanced)
- **Create projects** (Hackathon / Academic / Personal) with required skills and a minimum level
- **Home feed** of recommended projects, sorted by your **match score**
- **Explore**: search projects by title or skill, and students by skill and minimum level
- **Apply to join** a project; owners **accept or reject**; the project closes automatically when the team is full
- **Skill Swap**: finds students where the exchange works both ways, and lets you send swap requests
- **Requests**: all applications and swap requests (sent and received) in one place

## Setup

1. Create a free project at [supabase.com](https://supabase.com).
2. In Supabase, open **SQL Editor**, paste the contents of `supabase/schema.sql`, and click **Run**.
3. In **Authentication → Sign In / Providers**, turn off **Confirm email** (for testing).
4. Copy `.env.example` to `.env.local` and fill in your Project URL and publishable key
   (**Project Settings → API Keys**).
5. Install and run:
   ```bash
   npm install
   npx expo start
   ```
6. Scan the QR code with the **Expo Go** app.

## Folder structure

```
App.js                 Navigation (login screens vs. main tabs)
supabase.js            Database connection
context/AuthContext.js Who is logged in (shared with every screen)
constants/             Colors, skill levels, skill suggestions
components/            Reusable UI: AppButton, Input, Card, SkillChip, ProjectCard, StudentCard
screens/               One file per screen
utils/matching.js      Match score and skill-swap logic
supabase/schema.sql    Database tables and security rules
```

## Database

| Table | Stores |
|---|---|
| `profiles` | One row per student |
| `user_skills` | Skills a student has (`have`) or wants to learn (`want`), with level |
| `projects` | Projects looking for teammates |
| `project_skills` | Skills each project needs, with minimum level |
| `applications` | Requests to join a project (`pending` / `accepted` / `rejected`) |
| `swap_requests` | "I teach you X, you teach me Y" requests |

Team members = the project owner + applications with status `accepted`.

## How matching works

- **Levels become numbers:** Beginner = 0, Intermediate = 1, Advanced = 2.
- A student **meets** a required skill if they have a skill with the same name (case doesn't matter) at a level ≥ the minimum.
- **Match score** = (required skills met ÷ total required skills) × 100.
- **Skill swap match:** they have a skill you want (at your wanted level) **and** you have a skill they want (at their wanted level).

## Security

Row Level Security (RLS) is on for every table: any logged-in user can read data, but users can only change their own rows. Only a project's owner can accept or reject its applications, and swap requests are visible only to the two students involved.
# skillbridge-mobile
