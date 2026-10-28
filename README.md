# First-trail Fun Club — Publish Setup

This package is designed to replace the current `First-trail` site's `index.html` and add a Supabase database schema. It includes colorful student dashboard, email/password accounts, daily polls and one vote per account per poll, daily feedback/rating, fun posts, group creation and member chat, For Sale posts, private complaints, and an admin panel.

## Important
- GitHub Pages hosts the website; Supabase stores shared data and accounts.
- Do not use a Supabase `service_role`/secret key in browser code. Only the public anon/publishable key belongs in `index.html`.
- Run the SQL in the Supabase project that you intend to use. If your existing project already has the `cc_*` tables, back up first; this schema uses `create table if not exists` but policy changes can affect existing setup.
- Test with two student accounts before sharing widely. This is a starter community app, not a professional security audit. Never post sensitive personal details.

## Step 1 — Supabase database
1. Sign in at https://supabase.com and open the project you want to use.
2. Open **SQL Editor → New query**.
3. Open `schema.sql` from this folder, copy all SQL, paste it into the editor and click **Run**.
4. If the final Realtime publication statements report a duplicate table, remove only the duplicate `alter publication` line(s) and run again, or enable `cc_messages` and `cc_poll_votes` in Database → Publications/Replication.

## Step 2 — Connect the website
1. In Supabase, open **Project Settings → API** or **Connect**.
2. Copy the **Project URL** and **public anon/publishable key**.
3. Open `index.html` in Notepad.
4. Find `SUPABASE_URL` and `SUPABASE_ANON_KEY` near the bottom and replace the placeholder text between quotes with your values.
5. Save the file. Do not share your database password or secret/service-role key.

## Step 3 — Upload to your existing GitHub repository
1. Open your existing `First-trail` repository on GitHub.
2. Download/keep a backup of the old `index.html` before replacing it.
3. Click **Add file → Upload files**.
4. Upload the new `index.html` from this package to the repository root and replace the old file. Upload `schema.sql` too if you want it stored with the website.
5. Click **Commit changes**.
6. Open **Settings → Pages**. Set source to **Deploy from a branch**, branch `main`, folder `/(root)`, then Save.

## Step 4 — Configure Supabase redirect URLs
1. In Supabase, open **Authentication → URL Configuration**.
2. Add your exact GitHub Pages website URL to **Site URL** and **Redirect URLs**. Example: `https://YOUR-USERNAME.github.io/First-trail/` (use your actual link).

## Step 5 — Create your admin account
1. Open your published website and create an account using your own email and password.
2. Confirm your email if prompted, then log in.
3. In Supabase → SQL Editor, run this query after replacing the email with the exact email you registered:

```sql
insert into public.cc_admins(user_id)
select id from auth.users where email = 'YOUR_EMAIL@example.com'
on conflict (user_id) do nothing;
```

4. Log out and log in again. The Admin menu will appear. Keep your admin account private.

## Step 6 — Test before sharing
- Create two test student accounts.
- Publish a poll from Admin and vote from both accounts; try voting twice from one account.
- Create a group, join it from the second account, and send messages.
- Submit a complaint from a student account and verify only that student and admin can see it.
- Share the GitHub Pages URL with classmates only after tests pass.
