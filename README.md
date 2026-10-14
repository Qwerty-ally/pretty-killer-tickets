# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) has ended. Its **See memories** button opens a photo slideshow (5 seconds per photo, looping, with the album cover and "Vintage will be remembered" at the end of each loop) and plays the song during the photos. The **Behind the Scenes** and **Countdown Live** buttons open the videos (videos under 3 minutes go in Behind the Scenes; longer ones go in Countdown Live). The files are in `media/vintage/`.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 13 seats: Center 3, Left Side 4, Right Side 4, Left Box 1 and Right Box 1. One ticket works for both the Pretty and the Killer side.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Can't make it?** People can give their own ticket back from **My tickets**, which frees the seat. It only works on the device that claimed the ticket: the device keeps a secret key, and only a scrambled version (SHA-256) of it is saved in the database.
- **Admin:** open the site with your private `#admin-…` link (it isn't written anywhere in this repo; only its SHA-256 is, in `CONFIG.adminHash` and `firestore.rules`). From the admin page you can see the seat map with names, **move** people to another seat and **release** seats.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Firebase

The site is connected to the Firebase project **finn-pretty-killer-tix** (Firestore database plus security rules).

- Each seat is one document, so a seat can only be claimed once.
- Anyone can see which seats are taken, but not who took them.
- The admin page has no login. It opens only from the secret link above, but the ticket list (names and friend/family) can be read by anyone who knows how to query the database directly.
- Only the ticket holder (with **Can't make it?**) or the admin (with the private admin link) can delete or move a ticket. If the admin moves or releases a seat, the person's **My tickets** updates the next time they open the site.

If Firebase can't load (or the `apiKey` in `CONFIG` is cleared), the site falls back to **demo mode**, where tickets save only in the current browser.

## Hosting on GitHub Pages

GitHub Pages is turned on for the `main` branch, so the site is live at **https://qwerty-ally.github.io/pretty-killer-tickets/** and updates a minute or two after each push. The admin page uses your private `#admin-…` link, which is kept out of this repo.

## Changing the seats

The seat layout is `LAYOUT` near the top of the script in `index.html`. Each area has a name, a box on the map and a list of seat positions. Add or remove a position to change how many people an area holds.
