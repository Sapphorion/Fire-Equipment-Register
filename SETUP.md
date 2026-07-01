# Setup guide — Fire Equipment Register

One-time setup, roughly 10-15 minutes. You'll need a Google account.

## 1. Create a Firebase project
1. Go to https://console.firebase.google.com
2. Click **Add project**, give it a name (e.g. "FireWorkx Equipment Register")
3. You can disable Google Analytics for this project — not needed
4. Click **Create project**

## 2. Turn on email/password login
1. In the left sidebar: **Build → Authentication**
2. Click **Get started**
3. Click **Email/Password**, toggle it **Enabled**, click **Save**

## 3. Turn on the database
1. Left sidebar: **Build → Firestore Database**
2. Click **Create database**
3. Choose **Start in production mode**
4. Pick a location close to South Africa (e.g. `eur3` / europe-west, or whichever region your console offers that's nearest) — this can't be changed later, but it won't materially affect performance for this app
5. Click **Enable**
6. Once created, click the **Rules** tab, delete the placeholder content, and paste in the contents of `firestore.rules` (included alongside this app). Click **Publish**.

## 4. Turn on photo storage
1. Left sidebar: **Build → Storage**
2. Click **Get started**, accept the defaults, click **Done**
3. Click the **Rules** tab, delete the placeholder content, paste in the contents of `storage.rules`. Click **Publish**.

## 5. Connect the app to your project
1. Click the gear icon (top left, next to "Project Overview") → **Project settings**
2. Scroll to **Your apps**, click the **</>** (Web) icon
3. Give it a nickname (e.g. "Equipment Register Web"), click **Register app** — you don't need Firebase Hosting for this
4. You'll see a code block with a `firebaseConfig` object like:
   ```
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "yourproject.firebaseapp.com",
     projectId: "yourproject",
     ...
   };
   ```
5. Copy that whole object and paste it into `index.html`, replacing the placeholder `firebaseConfig` block near the top of the `<script>` section.

## 6. Create your first administrator account
The app has no public sign-up screen on purpose — only an admin can create logins. So the very first admin account needs to be created manually, once:

1. Firebase Console → **Authentication → Users → Add user**
   Enter your own email and a password. Click **Add user**.
2. Copy the **User UID** shown next to the new user (looks like `aB3xY...`)
3. Firebase Console → **Firestore Database → Data**
4. Click **Start collection**, collection ID: `technicians`
5. For **Document ID**, paste the User UID you copied (don't let it auto-generate one)
6. Add these fields:
   | Field | Type | Value |
   |---|---|---|
   | name | string | Your name |
   | email | string | The email you used above |
   | role | string | `admin` |
7. Click **Save**

## 7. Open the app
Double-click `index.html` to open it in a browser (or host it anywhere — Netlify, GitHub Pages, your own web server all work fine, since it's just static files). Log in with the email and password from step 6.

You're now the administrator. From here, use the **Techs** tab to create technician accounts (name, email, a password you set and share with them, SAQCC number) and the **Sites** tab to add sites and tick which technicians are assigned to each one.

## Notes
- Technician passwords are set by the admin when creating the account — there's currently no "forgot password" or self-service reset flow in the app itself. If a technician forgets theirs, you can reset it manually: Firebase Console → Authentication → Users → find them → the "⋮" menu → Reset password.
- The free Firebase tier (Spark plan) comfortably covers this scale of use — you'd need a genuinely high volume of daily records before hitting any limits.
- Technicians' devices cache data locally, so they can keep working with no signal on-site; anything they add or edit syncs automatically once they're back in range.
