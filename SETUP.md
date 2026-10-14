# S.K. TAILOR'S Production Manager — setup guide

This package is a ready-to-configure web app. It is **not yet live**: it needs to be connected to a Firebase project and deployed. Once deployed, all phones use the same online database.

## Important details
- Each worker gets a separate tab/sheet.
- Each daily row tracks shirt/pant quantities assigned and completed; pending is calculated automatically.
- All signed-in users can view and edit all production records, as requested.
- Only the owner account can add worker accounts.
- Worker PINs are Firebase Authentication passwords and must be at least 6 characters. Prefer longer, unique PINs and share them privately.
- Export all worker records as CSV (opens in Excel).

## Part 1 — Create the Firebase project
1. Ask a parent/guardian or the business owner to create/sign into a Google account if needed.
2. Open https://console.firebase.google.com and create a project named `sk-tailors-production`.
3. In Project Overview, click the web icon `</>` to register a web app. Give it a name like `SK Tailors Web`.
4. Firebase shows a config object containing `apiKey`, `authDomain`, `projectId`, and `appId`. Keep this page open.
5. In `index.html`, find `const firebaseConfig = { ... }` near the top of the script. Replace the four `PASTE_...` values with the exact values Firebase gave you. Do not paste private service-account keys into this file.

## Part 2 — Enable sign-in and database
1. In Firebase Console → Build → Authentication → Get started → Sign-in method, enable **Email/Password**.
2. Go to Build → Firestore Database → Create database. Choose production mode and a location suitable for your business. The database location cannot easily be changed later.
3. Open the Firestore Rules tab and publish the contents of this package's `firestore.rules`.
4. In Authentication → Users → Add user, create the owner's email and a strong password. Record the owner account's **UID** shown in the users list.
5. In Firestore → Data, create a collection named `users`. Add a document whose document ID is exactly the owner's UID. Add a field:
   - `role` (string) = `owner`
   You may also add `name` (string) = `Owner`.
   This owner profile is required for owner-only worker management.

## Part 3 — Deploy the website
Recommended easiest route if using a computer:
1. Install Node.js from https://nodejs.org if it is not already installed.
2. Open a terminal/command prompt in this package folder.
3. Install Firebase CLI: `npm install -g firebase-tools`
4. Sign in: `firebase login`
5. Run `firebase deploy` from this folder. When prompted to select a project, select the Firebase project created above.
6. Firebase will print a public `https://....web.app` address. Open it on your phone and add it to the home screen using your browser menu.

Firebase's official Hosting guide: https://firebase.google.com/docs/hosting/quickstart

## Part 4 — Add workers
1. Open the deployed website and choose Owner login.
2. Sign in with the owner's email/password.
3. Tap `+ Add worker`; enter the worker's name and a PIN/password of at least 6 characters.
4. Give each worker their name and PIN privately. Workers choose Worker login, select their name, and enter their PIN.
5. Because you selected option C, every signed-in worker can edit any worker's production records. Be careful: changes/deletions affect the shared records for everyone.

## If you only have a phone
The first-time setup/deployment is much easier on a computer. You can still create the Firebase project from a phone, but deploying the files needs a browser-based terminal or a computer. Do not share your Google password or Firebase console access with workers.

## Troubleshooting
- “Setup needed”: the Firebase configuration values in `index.html` have not been replaced.
- Worker list doesn't load: check Firestore is created and the rules are published.
- Owner login succeeds but Add worker is hidden: check `users/{owner UID}` has `role: owner`.
- Login fails: confirm Email/Password sign-in is enabled and that the worker was created from the app.
- Never change Firestore rules to allow unrestricted public access. These rules require a signed-in account.
