# Huvath Fuel Sales

Phone-friendly sales app for Huvath fuel station (GA. Kanduhulhudhoo): cash and credit sales, meter readings, cash count, bank-slip checks, staff roles, and the daily sales report as PDF or Excel.

It's a static website (hosted free on GitHub Pages) backed by **Firebase** (Google's free database and login).

## One-time setup (about 15 minutes)

### 1. Create the Firebase project
1. Go to <https://console.firebase.google.com> and sign in with your Google account.
2. **Add project** → name it `huvath-fuel` → you can turn Google Analytics off → **Create project**.
3. Stay on the free **Spark** plan. Nothing here needs a paid plan.

### 2. Turn on email login
1. Left menu → **Build → Authentication** → **Get started**.
2. **Sign-in method** → **Email/Password** → enable the first switch → **Save**.
3. **Settings → Authorized domains** → **Add domain** → add `YOUR-GITHUB-NAME.github.io`.

### 3. Create the database and paste the security rules
1. Left menu → **Build → Firestore Database** → **Create database** → pick a location close to the Maldives (e.g. `asia-south1` Mumbai) → **Start in production mode**.
2. Open the **Rules** tab, delete everything, paste the whole of [`firestore.rules`](firestore.rules), then **Publish**.
   These rules are what really enforce the roles (cashiers can't change other people's sales, only you can give access, and so on).

### 4. Connect the website to Firebase
1. **Project settings** (gear icon) → **Your apps** → click the **`</>` Web** icon → name it `huvath-web` → **Register app** (no hosting needed).
2. Copy the values shown in `firebaseConfig` into [`firebase-config.js`](firebase-config.js) and commit the change.
   These values are not secret. The security rules protect the data.

### 5. Turn on GitHub Pages
Repository **Settings → Pages** → *Build and deployment*: **Deploy from a branch**, branch `main`, folder `/ (root)` → **Save**.
After a minute the site is live at `https://YOUR-GITHUB-NAME.github.io/REPO-NAME/`.

### 6. Create your owner account
1. Open the site → **Create account** → sign up with **unooshahmed@gmail.com**.
2. Open the verification email and click the link, then tap **I've verified** in the app.
   You are now the **Owner** with full control.
3. On first use, go to **More → Station settings** and add the station's **bank account number** (needed for slip checks), then check prices, meters and tanks.
4. Your 17 credit customers and cash prices from the Excel sheet load automatically on your first sign-in. Check them under **More → Credit customers**.

## Adding staff
1. Send staff the website address.
2. They tap **Create account** and sign up with their own email and password.
3. You open **More → Users & roles** and set each person as **Manager**, **Cashier** or **Accountant**.
   Set **No access** to remove someone.

| Role | Can do |
|---|---|
| Owner | Everything, including who has access |
| Manager | All sales, readings, slips, reports, customers, prices; close/reopen a day; edit or delete any entry |
| Cashier | Record today's sales, credit, payments, meter readings, stock deliveries, expenses, cash count and slips. Can't edit or delete anything once saved, and can't see reports or money totals |
| Accountant | View any day, approve/reject slips, download reports. Can't record sales |

## Install on a phone
Open the site in Chrome (Android) or Safari (iPhone) → menu → **Add to Home screen**. It opens like an app.

## Good to know
- **Works offline.** If the internet drops, sales are kept on the phone and sent automatically when it comes back. Don't clear the browser data or sign out before it reconnects.
- **Meter vs sales check.** If the meter readings and the recorded sales differ by more than the tolerance (0.5 L, change it in Station settings), every page shows a red warning, and the daily report, PDF and Excel mark it in red. Closing the day is still allowed. When it's closed, the owner gets an email with the figures and any explanation. The emails are sent through the free FormSubmit service (formsubmit.co). **The very first time, FormSubmit sends unooshahmed@gmail.com an "Activate form" email. Click the link once,** or later alerts won't arrive.
- **Slip checking** reads the slip text on the phone itself (free, no account needed). The first check downloads the reader (a few MB). It works best on clear, straight, well-lit photos and screenshots. It checks that the station's account number and the amount appear on the slip. It does not confirm the payment with the bank. Anything unclear is marked **Needs review** for a manager or accountant to approve by hand.
- **Free limits.** Firebase's free plan allows 50,000 reads and 20,000 writes per day and 1 GB of storage. Slip photos are compressed (~150 KB), so the free plan holds several thousand slips.
- To change the owner email, edit it in both `firebase-config.js` and `firestore.rules` (and publish the rules again).

## Files
- `index.html`: the whole app
- `firebase-config.js`: your Firebase settings and the owner email
- `firestore.rules`: database security rules (paste into Firebase)
- `manifest.json`, `icon*.png`, `icon.svg`: home-screen app icon
