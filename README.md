# Flow — Tasks

A minimal, dependency-free to-do list app that runs entirely from a single HTML file. Works offline out of the box, with optional real-time sync across devices via Supabase.

**Live demo:** _add your GitHub Pages link here once deployed_

## Features

- ✅ Add, complete, and delete tasks
- 🔍 Filter by All / Open / Done
- 🌙 Light and dark mode, saved automatically
- ☁️ Optional cloud sync — join a "room code" to share a live task list across multiple devices
- 📦 Export/import your tasks as a portable `.json` file
- 📱 Fully responsive, built for mobile
- 🚫 Zero build tools, zero npm install — just open the HTML file

## Getting started

### Option 1: Use it locally
1. Download `index.html`
2. Open it in any browser — no server or install required
3. Your tasks save automatically to that browser via local storage

### Option 2: Deploy it with GitHub Pages
1. Upload `index.html` to this repo
2. Go to **Settings → Pages**
3. Under **Source**, select **Deploy from a branch** → branch `main`, folder `/ (root)`
4. Your app will be live at `https://yourusername.github.io/your-repo-name/`
5. Any future commit to `main` redeploys automatically — no manual step needed (allow ~1–2 minutes, and hard-refresh your browser if you don't see changes)

## Cloud sync setup (optional)

Real-time multi-device sync is powered by [Supabase](https://supabase.com) (free tier).

1. Create a Supabase project
2. In the **SQL Editor**, run:
   ```sql
   create table tasks (
     id text primary key,
     room_code text not null,
     text text not null,
     done boolean default false,
     created_at bigint,
     done_at bigint
   );

   alter table tasks enable row level security;
   create policy "allow all" on tasks for all using (true) with check (true);

   alter publication supabase_realtime add table tasks;
   ```
3. Go to **Project Settings → API** and copy your **Project URL** and **anon public key**
4. In `index.html`, set:
   ```js
   var SUPABASE_URL = "your-project-url";
   var SUPABASE_ANON_KEY = "your-anon-key";
   ```
5. In the app, tap **New** to generate a room code, or enter one and tap **Join**. Anyone who joins the same code sees the same live list.

> Note: the default setup uses an open access policy (`using (true)`), meaning anyone with a valid room code can read/write that room's tasks. This is fine for casual personal or small-group use; tighten the policy if you need stricter access control.

## How it works

- Tasks are stored locally first (instant, offline-friendly), then synced to Supabase in the background if a room is joined
- A live Supabase subscription pushes changes from other devices in the same room automatically — no refresh needed
- If the cloud is unreachable, the app falls back to the local copy without breaking

## Tech

Plain HTML, CSS, and JavaScript — no frameworks, no build step. Supabase JS client loaded via CDN for optional sync.


