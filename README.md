# Finn Shapiro Tickets

Free ticket site for Finn Shapiro's shows at 15 Freeman Parkway, in the same Pretty (AM) / Killer (PM) style as the merch shop.

- **The Vintage Tour** (Saturday, September 19, 2026) has ended. Its **See memories** button opens a photo slideshow (5 seconds per photo, looping, with the album cover and "Vintage will be remembered" at the end of each loop) and plays the song during the photos. The **Behind the Scenes** and **Countdown Live** buttons open the videos (videos under 3 minutes go in Behind the Scenes; longer ones go in Countdown Live). The files are in `media/vintage/`.
- **Pretty Killer Tour** (no set date) has a seat map based on the venue chart. It has 15 seats: Center 3, Left Side 4, Right Side 4, Left Box 1, Right Box 1, and two extra squares next to the boxes, Left Box 2 and Right Box 2 (1 seat each). One ticket works for both the Pretty and the Killer side.
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

## Show day info and photo booth

- **`info.html`** is the show day page: date, Pretty and Killer show times, address with a Maps link, how tickets work, the seats, the merch stand, what happens during the show and what to bring. Fill in `INFO` at the top of its script (`date`, `pretty`, `killer`, `bring`); anything left empty says "To be announced".
- **`booth.html`** is the photo booth: pick the **Pretty** or **Killer** frame, start the camera (or use a photo), and it counts down 3, 2, 1, takes the picture and puts the frame on it. Pick **1 photo** or **Photo strip · 3 photos**: the strip counts down for each of the 3 pictures and stacks them in a tall photo-booth strip with the frame (with **Use 3 photos**, if fewer are picked the last one repeats). **Save photo** downloads it and **Share** opens the phone's share menu. Switching frames after taking the picture re-frames the same photo. The camera asks for its highest resolution, uses the phone's full-quality still photo when the browser supports it (Chrome on Android), and saves a sharp JPEG (2160×2700 for one photo, 1800×6120 for a strip). For selfies the whole screen turns white for a moment while each picture is taken, to light up faces in a dark room, and camera photos get a light grain smoothing. The hint under the buttons shows the camera's resolution. Under the buttons, **Send to maybe be featured in the music video/movie** (ticked by default) sends a copy to Finn. The copy is locked (encrypted) on the guest's phone with a public key, and only the private admin link can unlock it, so nobody else can see guests' photos even by reading the database. With the box unticked, the photo stays on the phone.
- **Admin photo gallery:** the admin page has a **Photo booth** card with every photo guests sent, newest first. Tap one to open it full size, **Save** to download it, or **Delete** (tap twice). If the admin link's secret is ever changed, the photo key (`PHOTO_KEY` in `booth.html` and the same x/y in `index.html`) must be made again from the new secret, or new photos won't unlock.

## Pretty vs Killer battle

- **`battle.html`** (not linked anywhere yet): every day one Pretty song goes against one Killer song (round 1 is Pretty song 1 vs Killer song 1, and so on, from the tracklist). Fans play a snippet of each and vote once per round; the results show after voting or when the round ends. The scoreboard counts days won, and after 15 rounds the side with more wins gets the crown. Upcoming rounds stay "? vs ?".
- Set `START` at the top of its script to start the battle (each round lasts 24 hours). Empty = "The battle starts soon".
- Snippets go in `media/battle/`, named after the song in lowercase with dashes (`anne.mp3`, `pk.mp3`). Each plays up to 15 seconds. Songs without a file say "Snippet coming soon".
- Votes are in the `battleVotes` collection (one per device per round).

## Loudest cheer prize pages

- Two hidden pages (not linked anywhere), one per show: `loudest-pretty-14b8a717.html` (morning) and `loudest-killer-81492493.html` (night). The random part of the name keeps people from guessing them. Each says "You cheered the loudest!" with confetti in that side's colors and the date it was opened.
- The admin page has a **Loudest cheer · Pretty** and a **Loudest cheer · Killer** card with each page's QR code (and a download). Show it to whoever cheered the loudest.
- **Prize:** an exclusive song download (a song that isn't on the tour). Put it at `media/prize-14b8a717/song.mp3` (Pretty) and `media/prize-81492493/song.mp3` (Killer), and set its name in `SONG` at the top of each page. Until the file is there, the page says the song is on its way.
- To add a prize line (like "Show this at the merch stand for a free poster"), set `PRIZE` at the top of each page's script.

## Tour simulator

`tour.html` (on the hub) is a little game that shows guests the way from the front door to the show room: down the front hallway, through the door at the end, down the stairs, across the red tiles and through the wooden door. Each step has a drawn scene and choices (wrong ones shake and say why). In the show room they tap the merch table (by the fireplace), the stage (blue tape) and the front seating (red bean bag). Edit the route in `STEPS` at the top of its script.
