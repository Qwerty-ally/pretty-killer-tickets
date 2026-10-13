# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) is shown grayed out, since it has ended.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 13 seats: Center 3, Left Side 4, Right Side 4, Left Box 1 and Right Box 1. One ticket works for both the Pretty and the Killer side.
- **Starry Night Tour** (date to be decided, after Pretty Killer) is listed but grayed out until tickets go on sale. To open it, remove `soon:true` from its entry in `SHOWS`.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Admin:** open **https://qwerty-ally.github.io/pretty-killer-tickets/#finn-admin-tf49bh5u96**. There's no login, and nothing on the site links there. Change the link with `CONFIG.adminLink` in `index.html`.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Firebase

The site is connected to the Firebase project **finn-pretty-killer-tix** (Firestore database plus security rules).

- Each seat is one document, so a seat can only be claimed once.
- Anyone can see which seats are taken, but not who took them.
- The admin page has no login. It opens only from the secret link above, but the ticket list (names and friend/family) can be read by anyone who knows how to query the database directly.
- Nobody can change or delete tickets from the site. To free a seat, open [Firestore in the Firebase console](https://console.firebase.google.com/project/finn-pretty-killer-tix/firestore) and delete the seat's document in both `seats` and `tickets` (for example `prettykiller__C2`).

If Firebase can't load (or the `apiKey` in `CONFIG` is cleared), the site falls back to **demo mode**, where tickets save only in the current browser.

## Hosting on GitHub Pages

GitHub Pages is turned on for the `main` branch, so the site is live at **https://qwerty-ally.github.io/pretty-killer-tickets/** and updates a minute or two after each push. The admin page is **https://qwerty-ally.github.io/pretty-killer-tickets/#finn-admin-tf49bh5u96**.

## Changing the seats

The seat layout is `LAYOUT` near the top of the script in `index.html`. Each area has a name, a box on the map and a list of seat positions. Add or remove a position to change how many people an area holds.
