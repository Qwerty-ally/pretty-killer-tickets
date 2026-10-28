# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) has ended. Its **See memories** button opens a photo slideshow (5 seconds per photo, looping, with the album cover and "Vintage will be remembered" at the end of each loop) and plays the song during the photos. The **Behind the Scenes** and **Countdown Live** buttons open the videos (videos under 3 minutes go in Behind the Scenes; longer ones go in Countdown Live). The files are in `media/vintage/`.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 15 seats: Center 3, Left Side 4, Right Side 4, Left Box 2 and Right Box 2. One ticket works for both the Pretty and the Killer side.
- To get a ticket, pick a seat, enter your name and choose **Friend** or **Family**. Claimed seats get an ✕ and nobody else can take them.
- **Tracklist:** its own page (`#tracklist`) shows 15 songs for the Pretty side (morning) and 15 for the Killer side (night). Empty spots show "To be announced". When every seat is taken, the Pretty Killer button on the home page changes to **Tickets sold out · See tracklist**. The top of the tracklist page plays the **Official Pretty Killer Intro** (`media/pretty-killer/movie.mp4`). The admin page edits the names and can drag a song onto another (or use the arrows) to switch them, even between sides.
- **Can't make it?** From **My tickets**, people can mark that they can't make the morning show, the night show or both, and keep their seat, or give their ticket back, which frees the seat. The admin page can change anyone's shows with the dropdown in the Shows column. It only works on the device that claimed the ticket: the device keeps a secret key, and only a scrambled version (SHA-256) of it is saved in the database.
- **Admin:** open the site with your private `#admin-…` link (it isn't written anywhere in this repo; only its SHA-256 is, in `CONFIG.adminHash` and `firestore.rules`). From the admin page you can see the seat map with names, **move** people to another seat and **release** seats.
- **Testing:** type `am` or `pm` anywhere on the page to switch sides, and `auto` to follow the clock again.

## Show day and extras

- **Seat opened alert:** when a sold-out show gets a free seat, the button switches back to **Find tickets**, the card says how few are left, and anyone with the site open gets a heads-up.
- **Intro songs:** the admin page has an **Intro songs** card with the Pretty intro and the Killer intro (`media/pretty-killer/intro-songs/`). They aren't shown anywhere else on the site.
- **Door QR code:** on the admin page (with a download). It opens `confirmed.html`, which is just the Ticket Confirmed picture on black.
- **Now playing:** each song can have its own audio file in `media/pretty-killer/songs/`, named after the song in lowercase with dashes (for example `anne.mp3` for "Anne"); it's used on whichever side the song is in the tracklist, and the admin tracklist shows **♪** next to songs that have audio. Each side's Now playing starts with its intro song as **0.** (Pretty Intro or Killer Intro, from `media/pretty-killer/intro-songs/`), then song 1 and on. On the admin page, press **Play** once: the songs play one right after another. The "Now playing" bar only shows on the admin page; guests don't see it. Songs without a file run on a 3:30 timer. **Next** / **Back** jump between songs, **Stop** ends it. Keep the admin page open during the show.
- **Song voting:** on the tracklist page guests pick up to 3 songs per side and send their votes once; the admin page shows the totals and can reset them.
- **Guestbook:** opens **50 minutes after the Pretty show starts** and again **50 minutes after the Killer show starts**, and each time it closes **5 hours** after opening. A show's start time is set automatically the first time you press Play on that side (or with **Pretty show started now** / **Killer show started now** in the admin Guestbook card, which also has a **Reset** for each). While it's closed, the guestbook page says when it opens next and the database refuses new messages. Messages show up only after you approve them on the admin page (Approve / Hide / Delete). Messages waiting for approval aren't shown on the site, but can be read by anyone who queries the database directly.

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
