# CIA Intelligence Portal — Roleplay Edition

A responsive, bilingual (English/Arabic) single-page portal for a fictional CIA-inspired community. It includes recruitment applications, a scored QCM quiz, an admin dashboard, mission/task management, access-code generation, JSON export, and optional Supabase persistence.

> This is a fan-made roleplay project. It is not affiliated with the real U.S. Central Intelligence Agency or any government body.

## Files

- `index.html` — website and client-side logic.
- `config.js` — optional Supabase public project configuration.
- `supabase/schema.sql` — database tables and Row Level Security policies.
- `README.md` — deployment and setup guide.

## Quick preview (no backend)

Open `index.html` in a browser, or deploy the folder to a static host. The site runs in local demo mode. Applications, tasks and generated codes are stored in that browser only using `localStorage`. They do not sync to another device and should not be treated as secure records.

## Recommended free hosting: GitHub Pages

1. Create a GitHub repository, e.g. `cia-intelligence-portal`.
2. Upload `index.html`, `config.js`, and the `supabase/` folder.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/ (root)`, then save.
6. Wait for the published URL shown in the Pages settings.

GitHub Pages serves static files for free. It does not provide a database or server-side authentication by itself.

## Enable shared database with Supabase (free tier subject to provider limits)

1. Create a Supabase project.
2. Open **SQL Editor**, paste the complete contents of `supabase/schema.sql`, and run it.
3. Open **Project Settings → API** and copy the Project URL and anon/public key.
4. Edit `config.js`:
   ```js
   window.CIA_CONFIG = {
     supabaseUrl: "https://YOUR_PROJECT_REF.supabase.co",
     supabaseAnonKey: "YOUR_SUPABASE_ANON_PUBLIC_KEY"
   };
   ```
5. In **Authentication → Users**, create an admin user with an email and password.
6. Copy that user's UUID. In the SQL Editor, run:
   ```sql
   insert into public.user_roles (user_id, role)
   values ('PASTE-ADMIN-USER-UUID-HERE', 'admin')
   on conflict (user_id) do update set role = 'admin';
   ```
7. Publish the updated files again. Visit the site and sign in from the Administration section.

### Security notes

- Only use the Supabase **anon/public** key in `config.js`; this key is designed to be exposed in browser apps when Row Level Security is configured correctly.
- **Never** put the `service_role` key, database password, or private credentials in frontend files or GitHub.
- The SQL enables RLS. Anonymous visitors can submit applications but cannot read them. Only user accounts with an `admin` row in `user_roles` can manage applications, tasks, and access codes.
- Keep the admin account secure and only grant admin to trusted people.
- The generated access codes are an admin-managed feature/list, not a complete login or authorization system for end users. Do not claim these codes secure private pages unless a server-side validation flow is added.
- Public forms can receive spam. For a real public community, consider enabling Supabase CAPTCHA / rate limiting and add moderation.
- Supabase policies and app code should be reviewed before using real or sensitive data. Avoid collecting sensitive personal information.

## Function checklist

- [x] Responsive navigation and mobile layout
- [x] English / Arabic interface and RTL layout
- [x] Recruitment form validation and submit feedback
- [x] Local demo persistence
- [x] Optional Supabase application insert and admin login
- [x] Admin application review: approve, reject, delete
- [x] QCM quiz with instant score
- [x] Create, complete/reopen, and delete tasks
- [x] Generate/revoke access-code records
- [x] Export admin data as JSON
- [x] Setup status panel
- [ ] Cross-device persistence until Supabase credentials and admin role are configured
- [ ] Automated email/Discord notifications (not included)
- [ ] One-time access-code redemption and expiry (not included)
