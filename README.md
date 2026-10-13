# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) is shown grayed out, since it has ended.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 13 seats: Center 3, Left Side 4, Right Side 4, Left Box 1 and Right Box 1. One ticket works for both the Pretty and the Killer side.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Admin:** open the site with `#admin` at the end of the address, like `https://…/index.html#admin`. Nothing on the site links there.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Firebase

The site is connected to the Firebase project **finn-pretty-killer-tix** (Firestore database plus security rules).

- Each seat is one document, so a seat can only be claimed once.
- Anyone can see which seats are taken, but not who took them.
- Only the admin account (`finnshapiro49@gmail.com`) can read names or release seats.

**One-time admin setup (in the Firebase console):**

1. Open [the project's Authentication page](https://console.firebase.google.com/project/finn-pretty-killer-tix/authentication).
2. Click **Get started → Email/Password → Enable → Save**.
3. Go to **Users → Add user**, enter `finnshapiro49@gmail.com` and a password of your choice.
4. Open the site with `#admin` on the end and sign in with that email and password.

To use a different admin email, change `CONFIG.adminEmail` in `index.html` and the email in `firestore.rules`, then run `firebase deploy --only firestore:rules`.

If Firebase can't load (or the `apiKey` in `CONFIG` is cleared), the site falls back to **demo mode**, where tickets save only in the current browser.

## Hosting on GitHub Pages

In the repo on GitHub: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / root → Save**. GitHub shows the site address after a minute or two. On a free GitHub account the repo has to be public for Pages to work.

## Changing the seats

The seat layout is `LAYOUT` near the top of the script in `index.html`. Each area has a name, a box on the map and a list of seat positions. Add or remove a position to change how many people an area holds.
