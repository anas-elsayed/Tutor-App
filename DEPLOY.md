# TutorApp — Permanent Free Link (GitHub Pages)

## Why GitHub Pages?
- 100% free, forever — no trial, no expiry
- Your link: https://YOUR_USERNAME.github.io/tutorapp/
- Works on every device instantly

---

## Step 1 — Get your permanent link (10 min, one time)

1. Go to https://github.com → Sign Up (free)
2. Click + → New repository → name it exactly: tutorapp → Public → Create
3. Click Add file → Upload files → drag all 4 files → Commit changes
4. Settings tab → Pages → Source: Deploy from branch → main / root → Save
5. Your link: https://YOUR_USERNAME.github.io/tutorapp/
   (live in ~2 minutes, never expires)

---

## Step 2 — Supabase database (5 min)

1. https://supabase.com → New project → Europe West region
2. SQL Editor → New query → paste and run:

-- PASTE THIS IN SUPABASE SQL EDITOR:
create table if not exists universities (id bigserial primary key, name text unique not null);
create table if not exists departments (id bigserial primary key, university_id bigint references universities(id) on delete cascade, name text not null, unique(university_id,name));
create table if not exists subjects (id bigserial primary key, department_id bigint references departments(id) on delete cascade, name text not null, term text, unique(department_id,name));
create table if not exists students (id bigserial primary key, name text unique not null, email text, default_rate numeric default 0);
create table if not exists sessions (id bigserial primary key, subject_id bigint references subjects(id) on delete cascade, date date not null, filename text, t_meeting_min numeric default 0, t_absent_min numeric default 0, t_charge_min numeric default 0, rate numeric default 0, total_worth numeric default 0, notes text, created_at timestamptz default now());
create table if not exists session_students (id bigserial primary key, session_id bigint references sessions(id) on delete cascade, student_id bigint references students(id) on delete cascade, student_type text default 'live', amount numeric default 0, paid boolean default false, unique(session_id,student_id));
create table if not exists absence_periods (id bigserial primary key, session_id bigint references sessions(id) on delete cascade, from_ts text, to_ts text, duration_min numeric, talkers text);
create table if not exists name_mappings (id bigserial primary key, raw_identifier text unique not null, real_name text not null);
create table if not exists rate_mappings (id bigserial primary key, student_id bigint references students(id) on delete cascade, subject_id bigint references subjects(id) on delete cascade, rate numeric not null, unique(student_id,subject_id));
create table if not exists app_settings (key text primary key, value text not null);
insert into app_settings(key,value) values('your_email','anas.abdelsamad@feng.bu.edu.eg'),('silence_threshold_sec','180') on conflict(key) do nothing;
alter table universities enable row level security;
alter table departments enable row level security;
alter table subjects enable row level security;
alter table students enable row level security;
alter table sessions enable row level security;
alter table session_students enable row level security;
alter table absence_periods enable row level security;
alter table name_mappings enable row level security;
alter table rate_mappings enable row level security;
alter table app_settings enable row level security;
create policy "allow_all" on universities for all using (true) with check (true);
create policy "allow_all" on departments for all using (true) with check (true);
create policy "allow_all" on subjects for all using (true) with check (true);
create policy "allow_all" on students for all using (true) with check (true);
create policy "allow_all" on sessions for all using (true) with check (true);
create policy "allow_all" on session_students for all using (true) with check (true);
create policy "allow_all" on absence_periods for all using (true) with check (true);
create policy "allow_all" on name_mappings for all using (true) with check (true);
create policy "allow_all" on rate_mappings for all using (true) with check (true);
create policy "allow_all" on app_settings for all using (true) with check (true);

3. Project Settings → API → copy Project URL + anon public key

---

## Step 3 — Connect
Open your GitHub Pages link → setup screen → paste URL + key + your email → Connect

---

## Install as icon
Android Chrome: menu → Add to Home screen
Windows Chrome/Edge: install icon in address bar

---

## To update the app
Upload new index.html to GitHub repo → auto-deploys in 1 min.
Your Supabase data is never touched by updates.
