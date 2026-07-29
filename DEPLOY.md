# Deploy TutorApp — Get Your Link in 5 Minutes

## Option A: Netlify Drop (easiest — no account needed for temporary link)

1. Go to **https://app.netlify.com/drop**
2. Drag the entire `tutorapp/` folder onto the page
3. You get an instant live link like `https://random-name-123.netlify.app`
4. **To make it permanent:** create a free Netlify account and claim the site

---

## Option B: GitHub + Netlify (permanent, auto-updates)

1. Create a free GitHub account at https://github.com
2. Create a new repository called `tutor-app`
3. Upload all 4 files: `index.html`, `manifest.json`, `icon-192.png`, `icon-512.png`
4. Go to https://netlify.com → New site from Git → connect your repo
5. Deploy — you get `https://your-site-name.netlify.app` permanently

---

## Option C: GitHub Pages (also free and permanent)

1. Create GitHub repo (public)
2. Upload the 4 files to the root
3. Go to repo Settings → Pages → Source: main branch / root
4. Your link: `https://yourusername.github.io/tutor-app/`

---

## After deployment: Set up Supabase database

1. Go to **https://supabase.com** → New project (free tier is enough)
   - Choose a region close to Egypt (Europe West is good)
   - Note your project URL and anon key

2. In Supabase → **SQL Editor** → New query → paste and run this SQL:

```sql
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
```

3. Open your deployed app link
4. You'll see the setup screen — paste your Supabase URL and anon key
5. Enter your Teams email → Connect

---

## Install as app icon (Android)

1. Open the link in **Chrome on Android**
2. Tap the 3-dot menu → "Add to Home screen"
3. It installs like a native app — full screen, no browser bar

## Install on Windows

1. Open in **Chrome or Edge**
2. Click the install icon in the address bar (or menu → Install TutorApp)
3. It appears in your taskbar and Start menu

---

## Your data

All data is stored in your Supabase PostgreSQL database.
It syncs across all devices automatically.
The connection credentials are saved in your browser's localStorage.

To view your raw data: Supabase dashboard → Table Editor
