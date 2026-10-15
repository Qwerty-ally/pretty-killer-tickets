# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) has ended. Its **See memories** button opens a photo slideshow (5 seconds per photo, looping, with the album cover and "Vintage will be remembered" at the end of each loop) and plays the song during the photos. The **Behind the Scenes** and **Countdown Live** buttons open the videos (videos under 3 minutes go in Behind the Scenes; longer ones go in Countdown Live). The files are in `media/vintage/`.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 13 seats: Center 3, Left Side 4, Right Side 4, Left Box 1 and Right Box 1. One ticket works for both the Pretty and the Killer side.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Tracklist:** its own page (`#tracklist`) shows 15 songs for the Pretty side (morning) and 15 for the Killer side (night). Empty spots show "To be announced". When every seat is taken, the Pretty Killer button on the home page changes to **Tickets sold out · See tracklist**. The admin page edits the names and can drag a song onto another (or use the arrows) to switch them, even between sides.
- **Can't make it?** From **My tickets**, people can mark that they can't make the morning show, the night show or both, and keep their seat, or give their ticket back, which frees the seat. The admin page can change anyone's shows with the dropdown in the Shows column. It only works on the device that claimed the ticket: the device keeps a secret key, and only a scrambled version (SHA-256) of it is saved in the database.
- **Admin:** open the site with your private `#admin-…` link (it isn't written anywhere in this repo; only its SHA-256 is, in `CONFIG.adminHash` and `firestore.rules`). From the admin page you can see the seat map with names, **move** people to another seat and **release** seats.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Show day and extras

- **Seat opened alert:** when a sold-out show gets a free seat, the button switches back to **Find tickets**, the card says how few are left, and anyone with the site open gets a heads-up.
- **Door QR code:** on the admin page (with a download). It opens `confirmed.html`, which is just the Ticket Confirmed picture on black.
- **Now playing:** each side can have one full audio file (`media/pretty-killer/set-pretty.mp3` and `set-killer.mp3`). On the admin page, play/pause it; guests' "Now playing" bar follows the audio (and shows "Paused"). To cut the audio into songs, pick a song and press **Mark start** right when it begins, then **Save tracklist** (or type start times like 3:42 in the tracklist). Without an audio file, Play runs on a timer (songs without a start time count as 3:30). **Next** / **Back** jump between songs, **Stop** ends it.
- **Song voting:** on the tracklist page guests pick up to 3 songs per side and send their votes once; the admin page shows the totals and can reset them.
- **Guestbook:** guests leave a message at `#guestbook`; it shows up only after you approve it on the admin page (Approve / Hide / Delete). Messages waiting for approval aren't shown on the site, but can be read by anyone who queries the database directly.

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
