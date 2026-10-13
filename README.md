# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) is shown grayed out, since it has ended.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 13 seats: Center 3, Left Side 4, Right Side 4, Left Box 1 and Right Box 1. One ticket works for both the Pretty and the Killer side.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Admin:** open the site with `#admin` at the end of the address, like `https://…/index.html#admin`. Nothing on the site links there.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Demo mode vs. Firebase

Until Firebase is connected, the site runs in **demo mode**. Tickets save only in the browser you're using, so other people won't see taken seats and the admin page only lists tickets from your own browser.

To make it work for everyone:

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com).
2. **Build → Firestore Database → Create database** (production mode).
3. **Build → Authentication → Get started → Email/Password → Enable**. Then **Users → Add user** with the email and password you'll use for the admin page.
4. **Project settings → Your apps → Web app (`</>`)**. Copy the `firebaseConfig` values into `CONFIG.firebase` at the top of the script in `index.html`, and put your admin email in `CONFIG.adminEmail`.
5. Open `firestore.rules`, replace `YOUR-ADMIN-EMAIL@example.com` with the same admin email, and paste the rules into **Firestore Database → Rules → Publish**.

The rules make each seat claimable only once, let anyone see which seats are taken (without names), and let only the admin account read names or release seats.

## Hosting on GitHub Pages

In the repo on GitHub: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / root → Save**. GitHub shows the site address after a minute or two. On a free GitHub account the repo has to be public for Pages to work.

## Changing the seats

The seat layout is `LAYOUT` near the top of the script in `index.html`. Each area has a name, a box on the map and a list of seat positions. Add or remove a position to change how many people an area holds.
