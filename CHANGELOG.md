## 1.3.122 — The Wired catalogue's Variable filter shows the whole family

**Fix:** filtering the Wired catalogue by **Variável** showed only the 10 variable boxes. The 17 extras Habbo makes for variables (Extra Variável, Variável FX, the text capture and field, the filters by variable) were filed under Extra only, so they looked missing. The Variable filter now lists all 27. Their kind stays Extra, and the detail panel says they're variable extras.

- Nothing was missing from the data: Habbo's lists (BR, EN and ES, checked live) and the game's own Wired editor all carry exactly these 10 variable boxes, and the catalogue had them all.
- **Search** in the catalogue now ignores accents, capitals and word order like the rest of Pixelio: "variavel" finds "Variável", "pontos saude" finds "Pontos de Saúde". It also matches the kind, so "variável" finds the variable boxes even before Habbo names them.

## 1.3.121 — Dropdown menus line up with their fields

**Fix:** since 1.3.116 a dropdown's menu could be wider than its field. It grew to fit its longest option, so its right edge stuck out past the field. Menus are exactly as wide as their field again, so both edges line up.

- So names still fit, only the selected row keeps space for its ✓, and the **Type** field is a little wider. "Todos os tipos" now shows in full.

## 1.3.120 — Wired pictures that play their animation

**The Wired catalogue now shows each Wired's picture, and plays its animation.** Select a row and a panel beside the list shows that Wired box, drawn from Habbo's own artwork at 3x. Once the picture loads, it plays the flash the box makes in a room when it fires, then settles back on the still.

- **Click the row again, or the picture,** to replay it.
- **The animation is Habbo's own.** Each Wired box's artwork contains a short "switch on" animation (animation 100, which leads into the box's "on" state), and that's what plays. It runs at Habbo's pace of 24 frames per second, so Move and rotate's panels slide and turn for about a second, and Says something's red light blinks for about a second and a half.
- Furni without a switch-on animation, such as some buttons and tiles, show their still with a note. Artwork that can't be downloaded or drawn says **Picture unavailable**.
- **First view needs the internet:** a Wired's artwork is downloaded from Habbo the first time it's shown, which usually takes under a second. It's then kept in `wired-animations` in Pixelio's data folder, about 100 KB per Wired.
- The animation plays even with Windows "Show animations" off, because here it's the content you asked to see, not decoration.
- **Under the hood:** Pixelio's furni renderer (`inventory_renderer`) has a new `--activation` option that exports the still and every animation frame on one canvas. It was rebuilt with Go 1.27.1, and the new checksum is in its README.

## 1.3.119 — Smoother dropdowns and precise Wired delays

### Dropdowns feel right

The dropdown animation from 1.3.116 felt off. A hard edge uncovered a menu that stood still, so mid-animation it had no bottom edge and its scrollbar looked cut off. The arrow squashed flat instead of turning, and the highlight lagged behind the mouse. Now:

- **The menu slides out from under its field** like a drawer, rounded edge first, and lands softly (ease-out, 0.26 s). It slides back in when it closes (ease-in, 0.17 s). Menus that open upwards slide up.
- **No flash:** the full menu could be drawn for a moment before the animation started. It now starts closed.
- **The arrow turns** half a circle with the menu.
- **The highlight** follows the mouse and arrow keys on a spring: it leaves at once, arrives softly and never wobbles.
- Measured in the app: a steady 60 frames a second while opening, hovering and closing, under 1 ms of work per frame.
- **Fix:** closing a window with a dropdown, such as the Connections dialog, removed all of that window's resize handlers, not just the dropdown's own.

### Precise Wired delays

**Wired effects set to a precise delay now load in the Wired tab.** Habbo's Wired editor has two delay modes. Standard mode saves half-second steps (0 to 60 s), and Pixelio already read those. Precise mode saves the delay in 50 ms steps and sends it as a negative number of milliseconds (for example -1250 for 1.25 s, down to -60000 for 60 s). Pixelio treated any negative delay as broken data, so those effects failed to load.

- A precise delay from 0.05 s to 60 s is now read and shown in seconds, such as **1.25 s**.
- Values outside both ranges are still rejected as invalid.

## 1.3.118 — Wired catalogue

**Wired → Wired catalogue** lists every Wired in Habbo's furni list, and marks the ones that appear after you start using it.

- **What's in it:** every Wired box and the furni sold with them: 306 entries in today's list. That's 30 triggers, 58 effects, 44 conditions, 20 selectors, 36 extras, 10 variables, 20 unreleased prototypes and 88 others (buttons, counters, coloured tiles). The kind comes from Habbo's own code prefix (`wf_trg_`, `wf_act_`, `wf_cnd_`, `wf_slc_`, `wf_xtra_`, `wf_var_`). Codes starting `wf_test_`, `wf_proto_` or `test_` are unreleased test boxes and show as **Prototype**.
- **It keeps itself up to date.** The scanner downloads Habbo's furni list on every scan. Whenever that file changes, the catalogue re-reads it in the background. No Discord or other source is needed.
- **New since last check:** the first check only records what exists, so nothing is marked new on a fresh install. Any Wired that appears later gets a gold **★ New** mark and the date it was first seen. **Mark all as seen** clears the marks. A Wired that leaves the list and comes back isn't marked new again. The Wired tab's summary line also says how many new Wireds are waiting, even while the catalogue is closed.
- **Names Habbo hasn't written yet:** Habbo often adds a Wired before its Portuguese name, leaving placeholder text like "wf_var_dailytask name". The catalogue uses the English name when Try-On has downloaded the English list, and otherwise shows **Not named yet**. Unnamed entries are often the newest ones.
- Search by name, code or furni ID, filter by kind or show only new ones. Selecting a row shows its description. **Export catalogue** saves everything, with the new marks and first-seen dates, as JSON.
- What's known is kept in `wired-catalogue-v1.json` in Pixelio's data folder.

## 1.3.117 — A scan button that locks on

The scan button is redrawn as an animation instead of a still picture with a sliding line.

- **Hover or Tab to it:** the corner brackets close in on the furni cube and the cube makes a quarter turn, like a scanner locking onto its target. Moving away lets go.
- **While a scan runs:** the cube spins and a laser sweeps up and down it. Above the laser the cube is still a see-through hologram; below it, it's solid. Little sparks drift up off the laser.
- **When a scan completes:** the brackets flash, the cube pops, a ring pulses out and a tick appears in the corner, then fades after about two seconds. A scan that was stopped or didn't finish ends quietly, without the tick.
- The inventory, room and Looks scan buttons all work this way.
- **Windows "Show animations" off:** the hover lock-on and the tick are skipped. The scanning animation still plays, as before, because it shows the scan is working.
- **Cost:** each frame is drawn the first time it's needed (about 1 ms) and reused after that. The button only runs a timer while something on it moves and it's on screen. The 2508 px scan picture is no longer bundled.

## 1.3.116 — Price history, forgiving search and smooth dropdowns

**Pixelio now keeps every furni's daily marketplace prices.** Each price check returns Habbo's last 30 days for that furni: the average sale price and how many sold on each day. Pixelio used to read those days and throw them away. Now it saves them, so each item's history keeps growing past Habbo's 30-day window with every scan.

- **Inspector:** a new **Price history** section under the marketplace prices of the selected furni:
  - its **all-time average**, with how many days had sales, how many sold, and since when;
  - a chart of the daily average price, with sales per day underneath and the all-time average as a dashed gold line. Hover over the chart to see a day's price and sales.
- **All-time average** is each day's price weighted by how many sold that day, the same way Habbo works out its 30-day average. One sale at an odd price barely moves it.
- **Days follow the hotel's clock** (Brasília time for Habbo BR), so a check late at night and one the next morning file their sales under the right days. A later check updates a day it already saw; today keeps changing until it's over.
- **Collecting starts now.** Older saved scans kept only Habbo's 30-day average, not the daily figures, so the history begins with each furni's next price check and goes 30 days back from there. Inventory, room and clothing scans all add to it.
- LTDs are priced from their individual listings, so they have no daily history. The section is hidden for them, for unmarketable furni, for several selected furni and in Custom mode.
- The daily prices live in a new table in the existing price database (`market-cache.sqlite3`), separate from the price checks, so they don't add to the size of each saved check.

### Search ignores accents and word order

Typing **arvore de cereja**, **ARVORE CEREJA** or **cereja arvore** now finds **Árvore de Cereja**. Before, accents and capitals had to match exactly.

- Each word you type just has to appear somewhere in the name, in any order.
- It works the same in every search: the Inventory and Room lists (together with the Type and other filters), My clothing, Try-On and the room history. In rooms it also matches owners' names.

### Smooth dropdowns

Every dropdown (furni filters, Type, accounts, rooms, owners, clothing filters…) now moves:

- **Opening:** the menu unfolds from its field, or upwards when it opens above it, and the arrow turns over with it.
- **Hover:** the highlight glides from row to row as you move the mouse or use the arrow keys.
- **Long lists** scroll smoothly with the wheel.
- **Closing:** choosing an option, pressing Esc or clicking elsewhere folds the menu away. If the window moves or the options change, it closes at once.
- **Width:** menus are now as wide as their longest option, so the narrow Type field no longer cuts off "Todos os tipos".
- With Windows' animation setting off, menus open and close instantly as before.

## 1.3.115 — The connect button powers on

The **Connect account** button now shows the connection with its plug icon.

- **Connecting:** the plug reaches up for its socket and current creeps up the cord.
- **Connected:** the plug snaps into the socket, sparks fly, the button gives a little, the tile fills with light from the bottom, a gold bolt pops into the plug, and a shine sweeps across the button. While you stay connected, a small spark runs up the cord every few seconds.
- **Hover:** on a connected button the plug eases out a little, as a hint that clicking unplugs it.
- **Disconnecting:** the light drains away and the plug drops back below the socket.
- A button that isn't on screen when the state changes (another tab, or a minimised window) just shows the new state, without replaying the animation. The idle spark pauses while the button is hidden. With Windows' "Animation effects" turned off, every state shows without motion.
- The same button is used on the Inventory, Room copy and connection screens.

## 1.3.114 — Pixelio knows what every furni is

Pixelio now tags each furni with its types, using Habbo's own furni list (which it already downloaded, but only read names from) plus what the scan sees.

**Types:** LTD, NFT, C.A. (Builders Club), Credit furni (Câmbio), Rare, HC, Wired, Game, Clothing, Music, Pet, Group, Trophy, Gift, Teleport, Diamonds, Duckets, Rental, Photo, Room decoration and Not tradeable. A furni can have several, e.g. a rare that can't be traded.

- **Badges:** each row shows its two most telling types as coloured badges: LTD purple, Câmbio gold, Rare pink, HC orange, Wired blue, Not tradeable red, the others grey.
- **Type filter:** a new dropdown next to the existing one. It lists only the types in the scan you're looking at, with how many furni each has (e.g. "Wired · 126").
- **Search:** typing a type's name ("wired", "raro", "câmbio") finds that kind of furni.
- **Inspector:** lists all of a furni's types under its name. Credit furni say what they exchange for, and a stack that's only partly tradeable says how many can't be traded.
- **Export:** the CSV has new `types` and `exchange_value` columns.
- Works for inventory scans, room scans and older saved scans.

**Fixes:**

- **Credit furni were undervalued.** Pixelio priced them only from the marketplace, so one with no offers counted as 0, even though it can always be exchanged for its credits. Now a credit furni is never valued below its exchange value. In your last scan this adds 710c: 2× Golden Orb (350c each) and a 10c gold coin that can't be traded but can still be exchanged. Custom prices and exclusions still win.
- **Diamond exchange furni** (Emerald Ring, the crowns…) exchange for diamonds, not credits, so they aren't given a credit value. The inspector says how many diamonds they're worth.
- **"Not tradeable" and "not sellable" were merged** into one "Not marketable". Inventory scans now record each item's tradeable flag separately.
- **Rares:** besides Habbo's rare lines, Pixelio recognises Habbo's `_r21_`-style rare codes and names starting with "Raro", e.g. the Waterfall Wall.

## 1.3.113 — A smoother, clearer scan progress bar

**The bar now glides.** Before, it jumped one step each time a price check finished. Now it moves continuously at the pace the checks are coming in, so a steady scan fills at a steady speed. Its rounded end moves in 1/16-pixel steps, so even a large inventory, where each check adds less than a pixel, creeps forward smoothly instead of ticking.

- **While prices are being checked,** a soft light sweeps across the filled part every few seconds and the leading edge glows gently, so you can tell the scan is still working even between checks.
- **While a scan is getting ready,** a short segment sweeps along the bar instead of it sitting empty.
- **Stopped or incomplete scans** turn the bar gold, as before.
- **Cost:** about 5% of one CPU core while a scan runs, and nothing once it's done. It runs at 60 frames a second when something moves fast, and at 30 for the slow creep between sweeps.

**Clearer information:**

| Where | Before | Now |
|---|---|---|
| Top left | Inventory · Checking furni prices | **Inventory** (dimmed) · **Checking furni prices** |
| Top right | 2% · 27 / 1,199 types checked · 1,172 left | **2.3%**, counting up with the bar |
| Under the bar | — | 27 of 1,199 types checked · 1,172 to go |
| Under the bar, right | — | **About 9 min left**, based on the last minute and a half of checks |

When a scan finishes, the right side shows when it finished.

## 1.3.112 — Your account is detected on its own

**Fix:** when Pixelio joined a Habbo session after its login, the header stayed on "Identifying account" until you opened Connections and clicked **Detect logged-in account**.

- Every 20 seconds the Habbo client checks its connection speed, and Habbo's server only answers that check for a logged-in account. Pixelio now waits for that answer and identifies your account by itself, usually within 20 seconds. It sends the same single read-only identity request as before, never during the login handshake.
- The waiting message now says this, instead of asking you to click Detect. The button stays in Connections as a fallback.

## 1.3.111 — Scanning works with Habbo's October client

**Fix:** after Habbo's client update, the header showed "Scan unavailable". Pixelio only scans client builds it has checked, so a new build pauses scanning (without sending anything to Habbo) until it's verified.

- **Verified:** `WIN63-202610151745-318700621` (15 October) and `WIN63-202610131318-860371703` (13 October). These builds rename most of the client's code, so they were checked by comparing the bytecode of every message reader and request Pixelio uses against the September build, with names ignored. Inventory, prices, marketplace offers, identity, Club, wardrobe and room messages are all unchanged.
- **One small change:** the four "furni picked up from the room" messages now end with an extra flag. Pixelio's room tracking already reads only the fields it needs from those messages, so room scans keep working; a test now covers it.

## 1.3.110 — Smooth tab switching

**Fix:** clicking the Looks tab (Visuais) made Pixelio stutter. The window froze for a moment, the tab underline stopped mid-slide and then jumped, and for a few seconds the list kept catching. With your wardrobe, measured from click to fully drawn:

| | Before | After |
|---|---|---|
| First time you open Looks | ~650 ms | ~270 ms |
| After that | ~225 ms | ~110 ms |
| Longest freeze in the next 3 s | 850 ms | 130 ms |

- **One paint:** switching tabs now lays the new page out with the window's drawing paused and paints it in one go, instead of showing the old page clearing and the new one filling in widget by widget. Every tab benefits.
- **The underline** starts its slide once the new page is on screen, so it slides all the way instead of jumping.
- **My clothing** no longer works out every look's prices again (619 of them, against the whole furniture catalogue) each time a picture, clothing type or effect arrives. Only a change to the wardrobe, its prices or the catalogue does that, and the catalogue's index is built once. The 11 MB furniture catalogue is read in the background shortly after start-up, so opening Looks doesn't wait for it.

## 1.3.109 — Faster, cleaner, and a bug report system

### Performance

Measured with your real data, Pixelio sitting idle on each tab:

| Tab | Before | After |
|---|---|---|
| My clothing | 29% of a core | 4% |
| Try-On | 5.5% | 2.7% |
| Inventory | 2.8% | 1.2% |

- **My clothing** used to redraw the whole gallery 30 times a second whenever clothing with effects was in view. Now only the animated pictures change, exactly when Habbo's effect frame changes, and loading spinners turn in place. The view is redrawn only when a picture actually arrives.
- **The furniture lists** do the same for loading icons: no full redraws while they spin, including the 15 that used to happen during start-up.
- **NFT prices:** each refresh used to recheck all ~1,800 items with a pattern match. Now the rows are rechecked only when they change, and the check is cached per item.
- **Try-On** no longer recomputes every tile's identity on each poll.
- **Start-up:** the scan button's scanning animation is built the first time a scan runs, not at every start-up.

### Animations

The tab underline, the clothing tiles' hover, the scan beam and the hanger's swing now run on the same steady 60-frames-a-second clock as scrolling and the colour blends: frames every 16.7 ms instead of drifting towards 17.5 ms with occasional skips.

### A consistent look

- **Wired and Copiar quarto** now have the same heading as the other pages, "Wired · Conta" and "Copiar quarto · Conta" with how to use them underneath, instead of their own title styles. Copiar quarto's language buttons show the active language like everywhere else, and Wired's furniture ID field has a visible border.
- **Every dialog** (NFT prices, Connections, Room history, Watch & Walk and the others) now has a dark title bar like the main window, instead of Windows' white one.
- **Every table** lost the bright white outline around it: it now has the panels' border colour.
- **Scrollbars** in Wired, the arrival tools and the room review use Pixelio's own dark scrollbar instead of the light Windows one.

### Bug reports

Unexpected errors now get a short code, such as PX-3F2A. They are caught in the interface, in background work and at start-up. A small notice in the corner names the code, with buttons to copy it and to open the reports folder. Each code is saved once in the data folder's `bug-reports`, with how many times it happened and when, and the same bug always gets the same code. Reports hold only Pixelio's own file names and the error details, never account data, and are never sent anywhere. The same code isn't shown again within a minute.

### Cleanup

- **Removed:** the old G-Earth start-up panel and launcher (replaced long ago by the Connections dialog), several unused functions and imports, and a test that could fail at random.
- **Kept:** the standalone tools that have their own launchers in the project folder (marketplace live feed, offer history, furniture placement walker, avatar viewer).
- **Checked:** a scripted sweep through every tab and its main actions (139 steps: filters, sorting, prices, exclusions, scrolling, language, dialogs, window sizes, Try-On categories, colours, gender, rotation, outfits) ran without a single error.

## 1.3.108 — Clothing effects at Habbo's speed in Try-On

**Fix:** clothing with effects played far too fast in Try-On, about 60 frames a second, five times faster than in the game. The hanger button's swing animation defined its own 60-frames-a-second timing under the same name as the effects' timing, and it silently replaced it. Try-On's clothing tiles and the avatar preview now play effects at Habbo's pace again. My clothing already used the right speed and is unchanged.

Habbo's pace was checked against the game client itself (HabboAir.swf, read only):

- an avatar in a room updates at most every 41 ms;
- its animation advances one frame every 2 updates;
- each update shows the next of the clothing's 8 standing frames.

That makes about 12 frames a second, a full effect loop in two thirds of a second.

## 1.3.107 — My clothing keeps your place

**Fix:** scrolling through My clothing could jump back to the top or stop mid-way. The list refreshes itself in the background every 30 seconds, and whenever prices, mannequins or effect scan results arrive, which is several times a second while your looks are loading. Each refresh scrolled back to the selected look, and from 1.3.106 it also stopped any glide in progress. Now a refresh with the same looks redraws them where they are: the view stays where you scrolled, and a glide carries on through it. Only a real change to the list (a search, a filter, a new sort) brings the selected look back into view.

The furniture lists in Inventory, Room and Saved scans get the same treatment: updated values for the same furniture no longer stop a glide.

Also, after Pixelio starts closing, clothing pictures that finish drawing late are no longer saved.

## 1.3.106 — Silkier scrolling, now in My clothing too

**My clothing scrolls smoothly** in all three views. In the gallery and shelves each wheel notch glides about a card's height and touchpads follow your fingers. In the list it glides a whole row at a time, and dragging the scrollbar settles on a row. Arrow keys glide to the look they select.

**Smoother everywhere:**

- **Spring motion:** scrolling now follows a damped spring instead of a fixed curve. It eases in and out, never overshoots, and when you turn the wheel again mid-glide it carries on at the speed it had instead of starting over. A fast flick reads as one continuous movement.
- **Even frame timing:** frames used to drift to about one every 17.5 ms. They now land on an even 60-a-second grid; measured on your inventory and wardrobe, a frame comes every 16.6 ms. The Try-On colour blends use the same clock.
- **Nothing redrawn mid-glide:** when a glide reaches further, only the rows or cards coming into view are added; the list is never redrawn on the way. Loading spinners and clothing effects wait the fraction of a second until it settles. Each frame takes about 0.2 ms (0.5 ms in the gallery), and the slowest frame went from 24 ms to under 21.

With Windows' "Show animations" setting off, lists move straight to their new position.

## 1.3.105 — Smooth scrolling

The Inventory, Room and Saved scans lists now glide from row to row instead of jumping:

- **Mouse wheel:** each notch glides one row in about a quarter of a second, starting quickly and settling softly. Turning the wheel faster carries on from where the list is, so a flick flows straight through several rows. Touchpad movements add up into rows the same way.
- **Arrow keys** glide too, keeping the selected row in view. **Dragging the scrollbar** follows the mouse exactly, then settles onto the nearest whole row.
- **Smooth to the eye:** the rows a glide will pass are drawn once when it sets off. After that, each frame only moves the list's view, which takes about 0.2 ms, so frames keep a steady 60 a second, measured on a 1,213-type inventory. Every glide still ends on whole rows.

With Windows' "Show animations" setting off, the list moves straight to the next row as before.

## 1.3.104 — Whole rows in the furniture list

The Inventory, Room and Saved scans lists no longer end in a row cut in half. Rows size themselves to the list's height (about 80 px, between 64 and 104), so it always shows whole rows: four at the usual window size, three in a shorter one, more in a taller one. The mouse wheel, the scrollbar and the arrow keys move a whole row at a time, so the list stays aligned, down to the last row.

## 1.3.103 — Smaller list icons

Furniture icons in the Inventory and Room lists are back to their own size: sharp, trimmed of empty margins, with the name right beside them. Only unusually large icons are reduced to fit. The loading spinner, the hover highlight and the selection bar from 1.3.102 stay.

## 1.3.102 — Inventory and Room, redesigned together

The Inventory and Room pages share one layout, now laid out the same way as My clothing:

- **Heading:** the empty band above the value cards now says what the page shows and whose it is ("Inventário · Conta", "Quarto · Conta"), with a line giving the item and type counts and when the scan was taken. On Room it also names the room.
- **Same list header on both pages:** Room's header row (room name, "Histórico de quartos", owner) now has an Inventory counterpart in the same place: "Mobis do seu inventário" with a "Scans salvos" button. The search, filter and buttons below sit at exactly the same height on both pages.
- **Sharper list:** furniture icons are drawn at twice their size with whole pixels, as on My clothing, instead of small 1× icons in tall rows. While an icon loads, its spot shows the same spinner as the rest of Pixelio, and an icon that can't be fetched shows a quiet mark. Rows highlight under the mouse, and the selected row has an accent bar.
- **Detail panel that fits:** the picture of small furniture (a cabbage, a plant) is enlarged up to 3× with whole pixels instead of sitting tiny in an empty box. "Excluir do total" now sits on the same line as the item count. "Restaurar preço da Feira Livre" only appears in Personalizado mode; in Feira Livre mode the main button already does the same. Together these make every control fit without scrolling, where "Restaurar preço da Feira Livre" and the exclude switch used to be cut off at the bottom.
- **Fix:** an empty list (e.g. a room with none of your furniture) no longer squeezes its column headings to the left.

## 1.3.101 — Smoother colour changes

Changing a colour in Try-On is now one smooth blend for the whole grid:

- **All at once:** the new colour used to ripple across the grid over about a second. Drawing a tile in a new colour took under half a millisecond, but saving it to disk took twenty times longer, one write per tile. Finished pictures are now saved together in one write about a second later, and the view collects them every frame instead of on the app's slower refresh. All 116 tiles of a hair grid now have their new colour within about 30 ms and blend together.
- **Steady 60 frames a second:** the blend used to stutter at 30–40 frames a second with the occasional long pause. Tk is slow to take a see-through picture: it traces the outline of every transparent pixel, which made one picture of the whole grid take 150 ms. While many tiles blend at once, the grid is now drawn as one solid picture, about 2 ms a frame, with the corner tags kept on top. Every change to the grid in a frame goes to Tk in one step, and while anything animates, the interface takes its turn back from background work within 1 ms instead of 5. In practice, frames land every 17–18 ms with no pauses.
- **Softer:** tiles blend over 0.3 s and the avatar over 0.36 s, easing in and out. The palette ring glides over 0.22 s.
- **Clicking quickly:** picking another colour while a blend is running continues from what's on screen instead of jumping back.

The same batched saving also speeds up the background "Saving clothes offline" work and the My clothing mannequins. Pictures still waiting to be saved are written when Pixelio closes.

## 1.3.100 — Colours that blend

Picking a colour in Try-On used to swap every picture at once. Now the change is a smooth transition:

- **Clothing tiles** blend from the old colour into the new one as each new picture arrives (about a fifth of a second).
- **The avatar** blends into its new colours or clothes too. Turning it around still swaps straight away, like in Habbo, so it never shows two poses at once.
- **The palette's white selection ring** glides from the old colour to the new one instead of jumping.

Pictures blend on the same spot where they'll stay, so nothing shifts when a blend ends. Edges blend cleanly instead of darkening half way. Pieces with effects pick their loop back up once the blend finishes. All of this follows Windows' "Show animations" setting: with it off, colours change at once as before.

## 1.3.99 — The Try-On avatar plays its effects

The large avatar in Try-On now plays clothing effects too: fireflies drift, wisps float and balloons sparkle while you try a look on. Habbo's imaging service can draw each of the eight frames of the standing loop. When the look you're viewing has a piece with effects, Pixelio fetches the other seven frames in the background and plays them about 12 a second, the same as in the game. Looks without effects make no extra requests.

The frames are cropped and enlarged exactly like the still picture, so the avatar doesn't move when the effects start, and a frame never shifts against the next. Rotating or changing clothes shows the new still picture first, then starts its own loop. Frames are saved like other previews, so a look you've seen plays straight away next time.

## 1.3.98 — Clothing with effects

Some Habbo clothing has visual effects: fireflies drifting around you, a floating wisp, a sparkling balloon, Aphrodite's hearts. In the game they loop through eight frames, about 12 a second. Pixelio used to show only their first frame. Now they play the same way:

- **My clothing:** pieces with effects carry a purple **VFX** badge on their card or row, and their mannequin plays the loop, in the cards and in the detail panel. A new filter, **Roupas com efeitos**, shows only them.
- **Try-On:** tiles in the clothing grid play their effects as well.

Pixelio now reads every frame of the clothing artwork straight from Habbo's figure libraries. It no longer extracts them with the separate renderer, and it saves each library with all its frames. Because the saved format changed, Pixelio fetches the libraries again once, in the background. Which pieces have effects is remembered, so the badges and the filter are ready straight away next time.

Tall pieces in the list view, like a balloon above the head, are shrunk to fit their row instead of spilling into the next one.

The large avatar preview in Try-On comes from Habbo's own imaging service, which sends still pictures, so it stays still.


## 1.3.97 — Every look on Habbo's white mannequin

Habbo's product icons mix two styles, the bare item or a small mannequin wearing it, and they're only 30 pixels, so My clothing had to enlarge them. Now every piece the Try-On knows is drawn on Habbo's white mannequin at full size, from the clothing artwork Pixelio keeps on this PC:

- **Sharp:** no enlarging in the cards, shelves and list, and an exact 2× in the detail panel.
- **Colours:** each piece takes its colours from its official icon, matched to the nearest colours in its own palette, so it looks the way Habbo shows it.
- **Back pieces:** backpacks, capes and wings are turned around when they show better from behind. Hair, hats and face pieces always face front.
- **Fallback:** pieces that can't be drawn keep their product icon.

Mannequins are drawn in the background, the ones in view first, and saved, so they appear straight away next time. Pictures no longer sit on a dark square: they're drawn on the same colour as the panel, and the cards share it.

## 1.3.96 — Gallery and shelves for My clothing

My clothing can now be browsed three ways, using the switch next to the filters. Pixelio remembers your choice.

- **Galeria (new default):** a grid of cards, several per row. Each card shows the piece's icon in a framed slot, its name on up to two lines, its type, and a price tag (gold for coins, orange for NFT values). NFT pieces are marked.
- **Por tipo:** shelves for each kind of clothing (hats, shirts, necklaces…) in the Try-On's order. Each shelf is headed by the Try-On's own category icon, how many looks it holds and their total value.
- **Lista:** the table, as before.

A new sort menu works in every view: name, highest or lowest value, highest average, type, or highest NFT value. The list's column headers still sort too. Hovering highlights a look, the arrow keys move through the grid (up and down keep to the same column), and double-clicking still opens the look in Try-On.

**Icons:** each icon now sits in its own slot at twice its size, with sharp pixels, instead of filling the whole row. The detail panel shows it at four times. While an icon loads, its slot shows the same spinner as the Try-On, and the icon fades in when it arrives. An icon that can't be fetched shows a quiet mark instead of spinning forever. Icons for the next screen and a little above are fetched ahead, so scrolling finds them ready. The original image is kept so it can be drawn at any size, and more icons are taken in per tick.

**Scrolling:** the mouse wheel didn't scroll the My clothing list in 1.3.95. It now scrolls in every view, also over the column headers.

The Try-On's clothing types now load in the background as soon as My clothing opens, so the Type column and the shelves fill in without visiting Try-On first.

## 1.3.95 — My clothing, redesigned

Looks > My clothing now matches the Inventory tab:

- **Header:** the account name, with a single line saying when the clothing and NFT avatars were last captured. This replaces the big summary box. The language buttons show which one is active, and Export has its icon.
- **Value cards:** drawn like the Inventory's, with Habbo icons, the amount, a divider, the converted value and how many looks are priced. This also fixes the oddly indented line on the NFT card.
- **List:** the list sits in a framed panel with a proper search box (icon and placeholder) next to the filter. The table is new:
  - headers line up with their columns and sort when clicked (name A→Z, values highest first);
  - rows have separators and a clear highlight for the selected one;
  - up and down arrows move the selection;
  - the footer counts the looks shown;
  - the white focus border around the old list is gone.
- **Type column:** shows what each piece is, from the Try-On catalogue, e.g. "Colares" or "Acessórios de cabeça +1". It used to repeat "Visuais desbloqueados" on every row.
- **Detail panel:** the selected look's picture at three times its size, its values as label and amount, and when its price was checked.
- **Experimentar (try it on):** a button, or a double-click on a row, opens the look in Try-On, worn on your own avatar and on its own category. It switches body type if the piece is made for the other one. For an NFT avatar it loads the whole look.

## 1.3.94 — Clothes ripple into place

When the wardrobe opens or closes and the clothing grid gains or loses columns, the tiles no longer all move at once and cross each other on the way. They set off one after another in reading order, so the change ripples through the grid: the top rows settle first while the wave travels down. The colour swatches move the same way. The slide now takes 0.38 s instead of 0.34 s to leave room for the ripple. Tile positions are also now moved at full precision, so tiles land exactly in their cells after many opens and closes.

## 1.3.93 — The name and avatar no longer jump when the hanger is pressed

When the wardrobe closed, the name appeared twice for about a tenth of a second and the avatar preview jumped out of its column, before the slide began. The end of opening had the same kind of flash. Windows shows every widget move the moment it happens, but Tk changes a layout in several steps, so its in-between states reached the screen. Pixelio now switches off drawing for the Try-On while the layout changes and repaints it once when it's done, so only the finished result is ever shown.

This was checked by recording the live window from a separate thread while Pixelio ran normally. The name appears exactly once in every grab, and both the name and the avatar move steadily, with no jumps back.

## 1.3.92 — No blink at the end of opening the wardrobe

When "Meu Guarda-Roupa" finished sliding in, the search box, the filter buttons and the avatar column blinked out for a moment as the Try-On switched to its narrower layout. Moving their placeholders made Windows blank them, and nothing repainted them until the next pass of the event loop. Pixelio now repaints them in the same step, so the end of the slide is seamless. Frame-by-frame recordings of opening and closing now change smoothly from one frame to the next, with no blinks or jumps.

## 1.3.91 — No more jumps when the wardrobe opens or closes

Pressing the hanger made the Try-On flash its new layout for a moment before the glide. The name, the avatar column and the buttons briefly appeared where they would end up, then jumped back and slid there. When opening, the avatar column, hanger and right-hand tiles also vanished for a moment, because the narrower editor cut them off. Both are gone:

- **Closing:** everything is held in place before the editor widens, so the first picture of the new layout looks exactly like the old one.
- **Opening:** the editor keeps its width while everything glides to where the narrower layout will put it. The layout only changes once the drawer is in, when nothing visibly moves.
- **Late pictures:** a clothing picture that finishes loading mid-glide now travels with its tile, instead of being left out of place.
- **Reversing:** pressing the hanger again mid-slide continues from where things are on screen.

The motion is smoother too:

- It uses the standard ease-in-out curve instead of a fast start, so no single frame carries a big jump.
- It runs at about 50 frames a second.
- Opening responds to the press in under 20 ms, closing in about 80 ms.

## 1.3.90 — Smoother animations

Pixelio's animations were held back by Windows' timer. By default it only wakes every 15.6 ms, so a frame meant to come after 16 ms often came after 31 ms. That kept the tab underline, clothing hover and scan button at about 44 frames a second, with visible hitches. While something animates, Pixelio now asks Windows for a 1 ms timer, and gives it back half a second after the last frame, so an idle Pixelio costs nothing extra. They now run at a steady 60.

In the Try-On:

- The hanger's swing is a smooth 60-frame damped rock instead of 6 steps.
- Loading spinners turn at about 30 frames a second instead of 12, timed by the clock so they turn evenly.
- The wardrobe drawer's glide runs at about 45 frames a second instead of 33. When the grid's column count changes, Pixelio now moves the tiles it has already drawn instead of decoding and redrawing every one, so the slide starts about 30 ms sooner.
- The hanger button shows its press immediately, before the layout work.
- The Try-On animations follow Windows' "Show animations in Windows" setting, as the tabs already did.

## 1.3.89 — The Try-On moves with the wardrobe drawer

When "Meu Guarda-Roupa" opened or closed, the drawer slid but the rest of the Try-On jumped to its new size at the end. Now the whole screen moves with it. On closing, the avatar, the search and filter, the Reset button and the status links glide right as the drawer leaves. The name above the avatar re-centres, the clothing tiles move into the wider grid, and the colour swatches move to their new rows. On opening, the drawer pushes everything back the same way.

Tk takes 60–90 ms to re-lay out the editor, far too slow to do every frame. So the layout now changes once, at the click, and everything that moved is drawn at its old position and eased to its new one. The right-hand groups float over stand-ins that keep their room in the layout, so moving them is cheap. Tiles and swatches move on their canvases in one batched call per frame. Each frame finishes its own repainting, so moving parts don't leave trails. The animation runs at about 35 frames a second and takes 0.28 s.

## 1.3.88 — The wardrobe drawer slides

The hanger button at the top right of the Try-On opens and closes "Meu Guarda-Roupa", the panel of saved-look slots. Before, the panel just appeared and disappeared. Now it slides in and out from the right like a drawer, easing to a stop, and the hanger rocks on its hook and settles each time you press it. When opening, the editor makes room first and the drawer slides into the gap. When closing, the drawer slides away first and the editor then widens. Either way the hanger stays in view, and pressing it again mid-slide reverses from where the drawer is.

The drawer is drawn on top of the layout instead of resizing it each frame. A frame now takes about 1.5 ms instead of about 85 ms, which would have made the slide jump. The clothing grid also stops fully redrawing on resizes that don't change its number of columns.

## 1.3.87 — Clothes saved offline, smoother loading

Pixelio now saves every clothing piece on this PC in the background. That's about 3,000 Habbo figure libraries, roughly 30 MB, plus a thumbnail of every piece in your current look's colours. Once it's done, the Try-On grid shows clothes straight away instead of downloading them as you scroll. It runs whether or not the game is connected, waits while a scan is running or while visible tiles are loading, and shows its progress in the Try-On status bar ("Salvando roupas offline · 19%"). The first pass takes about ten minutes; later launches only fill in anything new.

The cache limits used to force re-downloads: 4,096 libraries that also expired after a week, and 4,096 thumbnails. Libraries are now kept for as long as Habbo's client version doesn't change. Up to 65,536 thumbnails are kept on disk, and a thumbnail that isn't in memory is read from disk instead of being redrawn. The cache only scans its folder when it may be near a limit, rather than on every save.

Loading spinners replace the grey dots. They're a teal arc with a fading tail, drawn smoothly in Pixelio's colours. They show on clothing tiles, the avatar (in the corner while it updates), saved looks in the wardrobe, and while the clothing list loads. All of them share one animation timer. A piece that fails to load shows a dash, and a saved look that fails falls back to the empty-slot figure instead of spinning forever.

## 1.3.86 — Clearer Try-On colour palettes

The Try-On colour palettes are easier to read. Swatches are larger, sit in a grid with a gap between them, and fill the width. A small gold corner marks HC colours instead of a tag that covered the colour. Each palette is labelled "Cor 1" or "Cor 2", shows four rows, and only shows a scrollbar when it has more colours than fit. The chosen colour has an accent border. Hovering a colour outlines it and shows its slot, hex code and HC status on the info line. The header and clothing grid ask for less height, so the status line at the bottom no longer gets cut off in smaller windows.

## 1.3.85 — Opening Pixelio twice no longer leaves a hidden process

Starting Pixelio while it was already open showed an "already open" message from a hidden window. That message had no taskbar button, so it easily ended up behind other windows, and the second Pixelio kept running invisibly until it was ended in Task Manager. A second launch now brings the open Pixelio window to the front (restoring it if minimised) and exits immediately. If that window can't be found, a normal always-on-top message with its own taskbar button says Pixelio is already open.

## 1.3.84 — Habbo-style wardrobe in Pixelio's colours

The Habbo-style Try-On keeps the game's layout and artwork but now uses Pixelio's own dark palette, so it matches the rest of the app: navy panels and header, mint highlights and Pixelio's buttons, with Habbo's folder tabs, thick buttons and scrollbar drawn in those colours. Habbo's selection disc and wardrobe arrows are recoloured to the theme.

Wardrobe slots now show the whole saved avatar: Habbo's small render only drew a placeholder head for newer clothing, so slots use its medium render scaled to fit. The colour palette always follows the category being browsed, including right after a look loads, instead of staying on the face.

## 1.3.83 — Try-On looks like Habbo's avatar editor

Looks > Try-On now looks like the in-game avatar editor. Its layout comes from the AIR client and its colours from the game's editor: a teal header with the account's name and Habbo's hanger button, grey folder tabs (Body, Head, Torso, Legs, Misc) with the selected one in the editor's beige, and Habbo's sub-tab icons at their native size, grey outline until selected. Faces and the Menino / Menina choice live in the Body tab, and pets and extras in Misc, as in Habbo.

Clothing sits straight on the beige panel in Habbo's 50x50 grid with its own scrollbar. Removable parts start with Habbo's grey X cell to wear nothing; the worn piece rests on Habbo's grey disc; HC pieces carry Habbo's HC tag and buyable clothing its gold hanger, alongside Pixelio's LTD, NFT and not-owned badges. The hovered (or worn) piece's name, status and any saved market price appear below the grid. Colours use Habbo's 13x21 swatches: one palette across the width for single-colour pieces, two side by side otherwise. The avatar stands beside the grid with Habbo's rotate arrow, above the current-look list.

Saved looks become "Meu Guarda-Roupa": two columns of slots with small avatars, where ">" saves the current look into a slot (replacing it in place) and "<" wears it; right-click deletes a slot. Rostos uses Habbo's own bald-head render. Search, the owned-clothing filter and the current-look list are kept, and Restaurar visual sits where Habbo's Salvar mudanças does, since Pixelio only previews looks locally.

## 1.3.82 — Habbo's own wardrobe category icons

The Looks category menu now uses Habbo's original avatar editor icons, copied unchanged from the installed AIR client, instead of generated Habbo-inspired artwork. Like Habbo, each category shows a grey outline when unselected and its coloured icon when selected or hovered; the Head, Upper body, Lower body and Extras headers use Habbo's editor tab icons. Icons are shown at exactly twice their native size with whole-pixel enlargement, so the pixel art stays crisp; category tiles and headers are slightly taller to fit them. Habbo has no separate face icon, so Faces uses its generic tab figure. The unused generated artwork was removed, making the app about 3 MB smaller.

## 1.3.81 — Safer scan saves and update errors

The latest inventory and room scans are now saved by replacing the previous file only after the new one is completely written, so an interrupted save no longer leaves an unreadable latest scan. If an update download failed and its error note could not be written, the update window could stay in its downloading state; it now always reports the failure.

## 1.3.80 — Keep each tab's Portuguese labels separate

All tabs share one Portuguese translation table, and Room copy (1.3.78) redefined Furniture as "Mobis", which renamed the inventory's Mobi column. Room copy now uses its own Furni key; its count column, room details and preview label still read "Mobis", and English still shows "Furni". Its selected-item placeholder no longer overrides the Wired tab's "Mobis selecionados". Measure arrivals no longer replaces the shared Status, Pronto and Desatualizado labels used by Connections and scan status. A new check fails whenever two modules translate the same key differently.

## 1.3.79 — Accept extended bulk-pickup updates

ObjectRemoveMultiple and ItemRemoveMultiple now follow the AIR client's required prefix: item count, complete integer ID list and picker ID. Trailing extension fields no longer invalidate the room. Both the Room and Room copy views remove only listed items, preserve the other furniture kind and keep the room loaded after bulk pickup. Truncated or oversized batches still fail before any item is removed. A bulk pickup during a paste still pauses that paste before any further placement.

## 1.3.78 — Real names and owners in saved rooms

The saved-room list now shows room name, room owner and captured furniture count in separate columns. Room ID and capture date remain in the selected-room details. New copies retain the public room identity from Habbo; a hotel-scoped cache fills missing details in older copies when their source room is visited, without rewriting their furniture or checksums. Existing room-scan titles also fill older missing names. Room owner is never inferred from furniture ownership or the connected account. Missing owner information is shown as a dash until observed.

## 1.3.77 — Recover unconfirmed C.A. floor-cover tiles

Temporary C.A. lajotas wait for the live placement map rather than relying only on the floor-editor layout. A timeout automatically reloads the destination and reconciles its complete furniture lists. An existing tile is adopted by ID without borrowing again. If the tile is absent, only its catalogue page and a fresh borrowed count are checked; an unchanged recorded count permits one journalled retry. Changed room contents, missing proof, disconnects and a second failure stop recovery. No full C.A. scan is performed. Older journals without a recorded count can reconcile an existing tile but never automatically repeat an uncertain borrow.

## 1.3.76 — Keep room review dialogs within the screen

Paste review now uses a bounded, resizable dialog with a concise item/supply/missing/Wired summary, full scrollable details and a fixed action footer. Hundreds of unavailable Wired reads can no longer push the buttons off screen. Cancel, Escape and the window close button all cancel without starting a paste. Long copy errors and incomplete-Wired reports use the same bounded layout. The saved blueprint and all diagnostic details are retained.

## 1.3.75 — Save room copies immediately and unblock Wired reads

Copy saves the captured room before requesting Wired settings and enriches that same blueprint as replies arrive. Animated room updates no longer hold read-only settings requests behind a two-second quiet interval. After 30 seconds without a matching Wired response, the room and received settings remain saved with explicit incomplete-read details instead of waiting through hundreds of individual timeouts. Connection changes and the new Finish copy now action also retain the captured room. The progress bar shows copying rather than an unrelated earlier paste. Definitely-unsent queue races defer their reads without consuming a timeout attempt. This does not bypass Habbo read permissions or claim unavailable Wired settings were copied.

## 1.3.74 — Recover automatically from rejected staging piles

Staging no longer repeats an owned furniture drop onto the same pile after a timeout. A fresh complete inventory must first prove that exact item is still unplaced. Pixelio then moves the confirmed staged items to their saved positions and places the remainder directly at their original destinations, retaining furniture IDs, state restoration and C.A. height helpers. This also recovers older staging journals that already retried the blocked item. The fallback and adjusted progress totals survive restart. Missing inventory proof and uncertain C.A. borrows still pause without resending; the actual pause reason is no longer hidden by a generic waiting status.

## 1.3.73 — Optional staging mode, progress bar and completion time

The Room copy tab now offers Stage floor furniture first, off by default and saved with the speed preference. Review identifies how many compatible items will first be placed on the largest fitting C.A. covering tile and then moved to their original positions. Walls, Wireds, rollers, final stack tiles and furniture too large for that tile use direct placement. Staging waits for each shared-location placement to be identified, including identical C.A. offers, before placing the next; final moves reuse those same IDs and verify destination height/rotation before restoring states and settings. This optional two-pass mode adds moves and does not promise a speed improvement.

Both modes show a progress bar and elapsed wall-clock time, including pauses. Staging counts both initial placements and final positioning; the bar remains below 100% until settings restoration and temporary-tile cleanup finish. Start and finish timestamps persist in the journal and completion duration freezes at the end. Older journals without timestamps display an unavailable duration rather than an invented measurement. Recovery retains staged identities, uncertain sends and moves; owned retries require fresh inventory proof and uncertain C.A. borrows are never duplicated.

## 1.3.72 — Continuous same-height placement instead of short bursts

New pastes prepare independent sections of up to 64 items, then continuously refill a bounded window of at most six unconfirmed placements as individual acknowledgements arrive. Verified progress is saved incrementally instead of waiting for every item in a short wave. Sections still stop at overlaps, height changes, walls, Wireds and stack tools; their lajotas remain locked until all dependent writes resolve. Version 1 journals retain their original controller and remain recoverable.

The controller learns from individual replies, starts with 150 ms spacing, and retains a slower learned minimum after trouble instead of rapidly accelerating back to a failing rate. Clean acknowledgements must pass a 32-item cooldown before concurrency can grow again. Owned-item timeout detection adapts to measured latency within 3–12 seconds and still requires a fresh complete inventory proving the exact item is unplaced before a single retry. C.A. retains its full timeout and never repeats uncertain borrows. Late acknowledgements release obsolete inventory-recovery holds even when other stream replies are pending.

## 1.3.71 — Continue automatically when room updates arrive before a send

An incoming room acknowledgement could race the final pre-send queue check and pause an otherwise healthy concurrent paste. The bridge now distinguishes this definitely-unsent deferral from connection changes and uncertain socket writes. The paste consumes and validates queued updates, then continues automatically without discarding existing in-flight identities or repeating a sent C.A. borrow. Lajota edits, floor refreshes, inventory recovery and settings reads preserve their retry state when deferred. Catalogue reads remain queued instead of reporting a failed check. Real disconnects, lost updates and uncertain writes retain their pause/reconciliation safeguards.

## 1.3.70 — Adaptive concurrent furniture placement

New pastes using C.A. floor coverage can keep up to six independent floor furniture placements in flight at one saved height. Groups preserve placement order and stop at overlapping footprints, height changes, walls, Wireds, rollers and final stack tiles. All covering lajotas are prepared and verified before sending the group, then held until every placement resolves. The existing speed slider caps the controller; clean groups gradually increase concurrency and reduce spacing, while slow confirmations or placement errors reduce throughput.

Each command is journalled before sending and independently matched to its server response, including out-of-order C.A. IDs. Only verified items advance progress. Incorrect positions are repaired after outstanding placements resolve. A timed-out owned item may retry once after a fresh complete inventory proves that exact item is still unplaced; uncertain C.A. borrows are never repeated. Recovery preserves all pending identities across interruption, and allowance reservations include temporary covering tiles without double-counting early server count updates. Existing journals keep their original sequential behavior.

## 1.3.69 — Restore Habbo performs action and repair completed Wired setups

Added wf_trg_user_performs_action to supported scalar Wired configurations after verifying the native PerformAction definition and UpdateTrigger composer, including action codes and optional sign/dance text. Unsupported-type reports identify the actual class instead of describing every omitted type as a snapshot.

Completed pastes with unverified, supported Wired configurations can be recovered in their destination and repaired with Retry Wired (Reparar Wired). Repair checks account, ownership, destination, layout and placed identities, skips previously verified configurations, reads the remaining configuration before changing it, and verifies readback afterward. It never runs furniture placement, inventory checks, C.A. borrowing, state setters or temporary-tile cleanup. Interrupted repairs retain uncertain writes and resume with reads only. Existing room copies already contain the data needed for this trigger.

## 1.3.68 — Complete server-response Wired backups and configuration verification

New room copies archive full Wired configuration responses, room Wired permission masks/timezone, variable definitions including flags and text connectors, room variable values, and inspections for every floor furniture item. Each read is scoped to the source room and recorded in a manifest; missing reads retry up to three times. Unknown response schemas remain archived as raw data and are reported as incomplete. Decoded fields are checked against archived bytes when loading. Old copies remain readable; recopying is required for the additional backup data. Server-private data, inaccessible settings and runtime state not exposed by these reads cannot be claimed as captured.

Wired boxes no longer pass through the generic furniture state setter before configuration, including recovery of old jobs paused there. Supported scalar configurations restore advanced input sources using the existing verified composer, followed by up to three readback attempts without repeating uncertain writes. Reports distinguish verified Wired configurations from ordinary visual states. Snapshot effects, custom variable mapping, hidden inspection properties and room-wide preferences are preserved when available but unsupported restoration is explicitly reported.

## 1.3.67 — Cover rooms with C.A. lajotas before furnishing

New pastes cover every usable floor tile with temporary lajotas borrowed exclusively from C.A. The planner chooses the largest available size fitting each uncovered area, including smaller edge pieces where needed, and rejects incomplete coverage before placement. The review lists the required sizes. Helpers remain in their coverage positions; every lajota overlapping a furniture footprint is adjusted to its saved height and acknowledged before furniture placement, including across seams and overlapping coverage. Temporary borrowed tiles are removed after furniture states are restored; cleanup verifies removal without expecting an inventory return.

Catalogue reads include stack-tile offers even when the regular furniture C.A. fallback is disabled, reuse the existing cache and retain bounded read retries. Coverage placements bind unique server-assigned IDs and are journalled before sending; uncertain C.A. placements are never duplicated. Recovery validates coverage and tool identities. Existing saved pastes keep their original method and furniture progress.

## 1.3.66 — Pond placement and automatic C.A. read recovery

Live Classic BR observation identified ordinary furniture placements over Lagoa de lírios (val15_water) being rejected despite stackable metadata and an unblocked height map; the same owned IDs succeeded with a covering lajota. These ponds now require a helper for overlapping furniture. Direct placement requires both a compatible map and valid observed supports; grass and ordinary usable surfaces retain the faster path.

C.A. count, index and page reads automatically retry missing responses up to three attempts with bounded backoff. Only the missing read is repeated, cached catalogue pages are retained, and a final timeout identifies which read failed. Late count responses are consumed before timeout checks. Furniture placements, purchases and membership claims are not retried or inferred by this mechanism.

## 1.3.65 — Server stacking map and paste timing reliability

Paste now observes Habbo's live HeightMap and HeightMapUpdate packets, including blocked tiles and quantized stack heights. This map overrides metadata predictions when deciding whether a floor item can be placed directly. A lajota position/height acknowledgement alone no longer releases furniture placement while Habbo's map still disagrees. Fast pastes keep a 100 ms settling window for split room/map updates, and the displayed wait reflects it. Rotated footprints now follow Classic's rounding for all eight directions.

Furniture property inspections use the Classic client's 500 ms polling cadence and automatically retry missing read responses up to three times with backoff. Unconfirmed state writes and placements are not blindly resent. Temporary Windows locks on paste journals retry the atomic rename before a command is sent; persistent errors preserve the previous journal and stop further writes.

## 1.3.64 — Surface-first placement and natural furniture support

New room plans prioritise flat, stackable surfaces such as Grama within the same saved height, before overlapping objects that would obstruct them. Classification uses furniture stacking properties and captured stack height, not translated names. Height changes, wall items and final-room stack tiles remain ordering boundaries.

A floor or previously placed stackable surface can now replace a temporary lajota when every cell in the rotated furniture footprint supplies the exact saved height. The builder rechecks observed support heights before each placement; unknown or unstackable overlaps, incomplete supports and temporary lajotas keep the helper path. State-dependent support heights use room observations, not the metadata default. Height corrections and bounded placement retries still force helper use, and every placement still requires acknowledgement. Raised grass and other saved offsets are preserved.

## 1.3.63 — Smarter furniture routes and lajota reuse

New paste plans group nearby furniture within each height band while preserving the relative order of every overlapping footprint. Wall furniture, height changes and final-room stack tiles remain ordering boundaries; bounded groups keep planning responsive on large rooms. Existing journals retain their saved action order.

The builder reuses the observed footprint of an already deployed lajota instead of moving it to every furniture anchor. It prefers a covering tile already at the requested height. When a helper must move, it looks ahead up to 12 actions to select a valid position covering more upcoming furniture. Overlapping helper heights are still aligned, commands remain journalled before sending, and each placement and correction still waits for server confirmation. No furniture purchases or new controls were added.

## 1.3.62 — Faster verified room paste

Extended the live paste-speed slider to 100x. Above 40x, the running paste checks incoming confirmations every 10 ms; at 100x the room-update settling wait is 20 ms instead of 50 ms. Initial room loads and floor changes retain their full settling interval, split height updates retain their reconciliation window, and every placement still requires server confirmation. Existing speed preferences, acknowledgement timeouts, write-before-send journals and bounded recovery are preserved. Actual throughput depends on Habbo response time and the number of stacking operations.

## 1.3.61 — Prevent conflicting lajota heights

Room paste now aligns every overlapping temporary stack tile to the requested furniture height, waiting for each server acknowledgement before placement or correction. A larger tile left from an earlier action can no longer override the selected smaller tile with its old height. Known height conflicts on an already observed item recover once automatically using the same furniture ID; older paused jobs can recover through Resume. Other uncertain writes stay paused, and timeout reports include the expected and observed position, rotation and height.

## 1.3.60 — Restore automatic wall height during room paste

Removed an incorrect preflight blocker when the source uses automatic wall height and the destination uses a fixed height. Pixelio sends the same UpdateFloorProperties payload as the Classic floor editor with its fixed-height checkbox cleared: the optional custom-height field is omitted. The existing acknowledgement gate still requires the destination to report the captured geometry, including automatic height, before placing any furniture. Unconfirmed edits pause without resending.

## 1.3.59 — Original room finishes and water/background artwork

RoomProperty observations now retain floor, wallpaper and landscape identifiers in new room blueprints. The preview uses original Classic floor/wall textures and colour mappings, projected at captured floor heights with continuous wall patterns. Older blueprints explicitly report that finishes were not captured; saved files are not silently rewritten. This is preview support, not purchasing or applying wall/floor products during paste.

Water-area furniture preserves its full neighbour bitmask and uses the Classic shore-mask rules instead of clamping states and drawing every shore. Large room-background sprites now use a bounded 4-megapixel room-only path. Poster artwork uses the design-specific library. Credit denominations use the client base frame. Additive stars/glows are exported separately and composited against the room, preserving light that was previously discarded against transparency. A new cache version replaces stale approximate images, and selecting a room again retries transient artwork failures.

## 1.3.58 — Sharper, more accurate room previews

Floor and wall furniture now share a world-position depth ordering derived from the Classic client's shallow depth vector. Previously all wall items were sorted at the origin, and drawing depended on caller order. Raised floors now show internal step faces, floor and wall thickness follows the captured settings, walls have top edges without per-tile vertical seams, and the captured entrance leaves a doorway. Furniture anchors, saved positions, heights and states remain unchanged.

Native room sprites include their original shadow artwork. Unavailable directions use a deterministic nearest-direction fallback instead of always choosing the smallest direction, and unsupported visual states retain static artwork while being labelled approximate. A versioned cache separates these sprites from previous results. Inventory thumbnails keep their existing shadow-free rendering.

Filtered image levels are composed in the background for cleaner zoomed-out details; native-size/enlarged artwork retains crisp pixels. One bounded image at the current zoom makes ordinary panning a crop-only operation. Large zooms retain the bounded viewport path. Synthetic rendering/lifecycle checks and saved 58-, 275- and 1,608-item room samples validate the changes. Wallpaper/floor finishes, dynamic special-furniture visuals and animation timing remain outside this static preview's fidelity; saved room documents and live rooms are not modified.

Fit room excludes Wired boxes parked more than 16 levels above the highest floor from its camera bounds. Their original positions, rendered sprites and selection remain available by panning; tall decorative furniture remains included. This fixes the live 331-item room preview shrinking around two Wired boxes at heights 40 and 40.65.

## 1.3.57 — Automatically recover an unconfirmed inventory placement

When an owned floor-furniture placement times out during a paste, Pixelio now requests one fresh full inventory automatically and resumes through the existing lajota-assisted recovery checks. It adopts a late room acknowledgement without placing another copy, or verifies the exact item is still unplaced and prepares/acknowledges a fitting lajota before the single permitted retry. No Resume click is needed for this recoverable timeout. C.A. results with unknown identity, stack-tool placements and already-retried items remain paused.

Recovery is scoped to the current job, item and connection, with a 90-second overall deadline and the existing inventory-read timeout. Pause, Stop, connection/room changes and bulk pickups cancel it. Restoring a saved job does not automatically start writes. The progress area shows automatic recovery, and Pause remains usable while the inventory is being read. Regression tests cover complete/multipart inventory, late acknowledgements, exact identity, helper absence, bounded retries, timeout and cancellation; no live furniture placement is part of verification.

## 1.3.56 — Recover a rejected first furniture placement

Resume's single lajota-assisted retry no longer requires existing furniture to overlap the target tile. This fixes the saved Eco Banqueta job stopped at item 1/275 in an empty destination. Recovery still requires a complete inventory read requested after the pause, proving that the exact owned item is unplaced, an absent room instance, a fitting available helper and no pending room edit. The helper's placement and height must be acknowledged before resending that same item ID once. Unknown C.A. outcomes, stack-tool items and repeated retry failures remain excluded.

A read-only live inventory check confirmed stool 61162847 and lajota 810942556 were both unplaced. Offline replay of the saved job confirmed helper placement at (9,5), acknowledgement, then the same stool ID at (9,5). No live placement was sent during verification. Existing saved jobs can use Recover paste and Resume.

## 1.3.55 — Support wall locations with a= height metadata

The saved 275-item blueprint for room 153241046 includes 41 wall items, 13 of which use a trailing a= integer. These locations are now recognized and preserved in inventory and C.A. placement commands, including the embedded Java engine's command validation. Saved copies need no conversion. Height metadata must match in placement readback; it is never silently stripped to pass validation. The preview obtains wall direction from the side field rather than the end of the string and uses a/100 for explicit altitude. The room's values 1350, 940 and 540 correspond to 13.5, 9.4 and 5.4 levels; their local offsets agree with a common wall reference height of 15.625 within one pixel of rounding at scale 32.

Python and engine validation accept one optional bounded a= field and reject duplicate fields, non-integers, excessive values and appended commands. Offline checks exercise inventory/C.A. payloads, orientation, height and acknowledgement mismatches against the saved room; no live placements were sent during verification.

## 1.3.54 — Preserve unusual wall positions when copying rooms

Copy and saved-blueprint validation now retain the original bounded wall-position string instead of requiring every wall item to be immediately placeable. Empty, legacy and unknown position formats no longer abort the entire room backup. The preview renders supported items and explicitly counts wall items retained in the blueprint but not displayed, without guessing their location.

Recognized modern wall coordinates normalize signed numbers, leading zeroes and trailing spaces for inventory and C.A. placement, previews and placement verification; the saved source string remains unchanged. Unsupported or out-of-range placements are identified by furniture name, item ID and raw position during paste review, before any writes. This does not add automatic placement of arbitrary legacy/unknown wall formats. The exact wall position from the reported failure was not available; regression tests cover lossless capture/save/load, preview continuity, normalization and placement guards.

## 1.3.53 — Accept individual pickup message extensions

Individual ObjectRemove and ItemRemove replies now use a shared parser that validates their complete known prefix, then ignores any extension fields, matching the recovered Habbo AIR client. Previously, additional fields triggered an unknown-trailing-fields error and invalidated the room before removing the furniture. The extension's meaning is not assumed or replayed. Furniture IDs remain separated by floor/wall kind; truncated required fields and malformed IDs/booleans still fail before any room mutation. Bulk and furniture-state packet validation remains unchanged.

Validated with synthetic legacy and extended replies, malformed/truncated prefixes, floor/wall ID collisions, C.A. IDs and session event processing. The user's exact failing packet was not retained; live pickup verification is still required. A room already invalidated by the old build needs a fresh baseline once.

## 1.3.52 — Keep rooms synchronized after bulk pickups

Room copy and room valuation now subscribe to Habbo's ObjectRemoveMultiple and ItemRemoveMultiple messages. Server-confirmed bulk pickups (including :pickall) remove the reported floor or wall IDs immediately while preserving the current room's layout, ownership and complete baseline. Split batches, C.A. furniture and overlapping floor/wall IDs are handled independently; malformed packets cannot partially change a room. No room reload request or avatar re-entry is needed for these updates once the initial room data has loaded.

Bulk pickup invalidates a pending paste review and inventory availability. A running paste pauses before another write if bulk removal is observed. Saved blueprints remain unchanged. Connecting after the room's initial entry packets were sent can still require one re-entry to establish the initial baseline.

## 1.3.51 — Responsive cached room previews

Static room layers and furniture sprites are composed on a background worker into a bounded cached image. Panning and zooming crop and scale only the visible viewport instead of rebuilding thousands of Tk polygons and sprites. Camera updates are coalesced at 16 ms, zoom stays anchored beneath the pointer, and newly arriving artwork does not reset a camera the user has moved. Furniture selection uses the displayed sprite's alpha and original world coordinates, retaining rotation, altitude, state and layer order. Room switches discard stale composition results; workers retain no Tk objects. Source artwork remains cached with a 128 MiB bound, and composed scenes are limited to 16 megapixels.

On the existing 1,608-item room with 529 cached sprite variants at a 1,100 × 720 viewport, a local benchmark measured median pan work falling from approximately 251 ms to 15 ms, with wheel frames falling from approximately 278 ms to 33 ms including the benchmark's 20 ms event wait. Initial uncached artwork still requires download/extraction. Regression checks cover rendering, camera anchoring, alpha-aware selection, late artwork, room switching, cancellation and memory bounds.

## 1.3.50 — Restore saved furniture states before lajota cleanup

The numeric furniture-state pass now runs after placement and before temporary-tool pickup, so a cleanup failure cannot prevent that pass from starting. Every supported state write still uses a server-discovered writable property, verifies that the property exists on the target furniture and waits for the room state to match. Wired configuration remains after cleanup. No unknown state write is resent on Resume.

Restorable mismatches are displayed as pending while placement completes. The interface shows state-restoration progress separately from placement count and updates matching-state reports immediately after verification. Non-furniture Wired inspection replies now follow their own wire schema and are ignored by furniture-state restoration. Room/inventory parse failures identify the packet name for diagnosis; unknown trailing fields remain rejected.

Validated with the saved 56-item room in an offline recovery replay: 18 mismatched numeric states reach the captured values without any new placement, movement or pickup commands. Live read-only checks confirmed the writable @state property and that the temporary lajota had returned to inventory; the original failing cleanup packet was not retained, so its precise schema discrepancy remains unverified.

## 1.3.49 — Prepare lajotas for overlapping furniture and recover rejected placements

Room-paste planning now detects overlapping rotated floor-furniture footprints and reserves a fitting lajota before placement, even at floor height. Runtime checks also cover older saved jobs and prefer a suitable deployed tool. The tool's position and height must be acknowledged before furniture placement. Missing tools are reported before a new plan starts.

Resume can recover a missing owned floor item from an older overlapping placement when a new full inventory read requested after pausing confirms that exact furniture ID is still unplaced and the destination baseline contains no such item. It prepares a fitting lajota and permits one journalled retry of that same ID. Stale inventory, unknown C.A. placements and repeated failures never authorize retries. Late observed items are reconciled without re-placement.

Timeouts name the furniture and coordinates. Inventory/C.A. completion preserves the pause reason, failed Resume checks remain visible, the report shows the reason first, and the construction step advances to the current placement instead of displaying an earlier correction.

## 1.3.48 — Saved C.A. catalogue and on-demand refresh

Paste and Resume now reuse a persistent catalogue cache scoped to the account, hotel and client build. Every verified single-furniture offer on a read page is retained, including offers not needed by the current blueprint. New missing types search only unread pages, and fully searched pages remember unavailable types instead of scanning again. Results survive reconnects and restarts. Corrupt caches are ignored; Habbo catalogue-publication events invalidate saved results. Current furniture usage and available membership status remain live checks, never persisted entitlements.

Added a localized Refresh C.A. button in the Room copy sidebar. It explicitly rebuilds the complete visible C.A. catalogue without placing furniture. Refresh is unavailable during active placement or another read. Inventory-first selection, matching-type fallback, placement verification and uncertain-placement protection are unchanged.

## 1.3.47 — Room-loading completion and broader C.A. fallback

Fixed the stale "Reading room entrance and dimensions" status. Entrance reads now finish explicitly, retry up to three times and report unavailable data after bounded timeouts. Missing wall/floor thickness is requested using the room-settings read command instead of relying solely on an earlier unsolicited packet. Responses for another room are ignored. Copy, paste review and resume wait for complete geometry; no room dimensions are invented. A recovered paste's actual pause reason is restored after loading.

C.A. discovery now searches the live visible Builders Club catalogue for every missing furniture type, including missing/stale local bcofferid hints and catalogue pages whose index omits offer IDs. It prioritizes hinted pages, shows scan progress, and stops when all requested types are found. Inventory remains first; only matching verified single-furniture floor/wall offers are used, without purchases or arbitrary substitutions. Existing special-furniture and temporary inventory-lajota restrictions remain. On resume, unsent missing inventory assignments can use another unreserved owned copy or an available matching C.A. offer. Pending placements are never replaced. Live allowance is rechecked without double-counting already placed C.A. furniture. Unavailable catalogue items and server restrictions remain explicit.

The builder prefers a suitable already-deployed lajota over placing another helper. Recovery can switch an unconfirmed helper placement to an observed deployed lajota when a later inventory response proves the original tool remains unplaced. It does not resend the uncertain placement. If the helper is observed at a different position, it reconciles that same tool by moving it. Equivalent temporary-tile rotations are accepted when the footprint is unchanged; final furniture rotations are still checked exactly. This covers the saved Lanterna de Festa pause when a 2×2 helper is available in the destination.

## 1.3.46 — Faster verified room pasting

The paste-speed slider now reaches 40×, reducing the minimum per-update settling wait from 0.2 seconds to 0.05 seconds. Above 10×, the active paste polls at 20 ms and advances straight from a confirmed placement to the next construction step without an extra UI polling delay. It still sends at most one new command per pump and waits for each placement/stack-tool acknowledgement. A brief 0.2-second reconciliation window remains before correcting mismatched placement data. Initial and bulk room loading, floor reload waits, response deadlines and duplicate prevention are unchanged. Existing speed preferences and unfinished jobs remain compatible. The multiplier describes Pixelio's settling delay; live throughput depends on Habbo and the required construction steps.

## 1.3.45 — Adjustable room-paste speed

The Room copy tab now has a live 1×–10× paste-speed slider. It reduces the quiet wait after individual room updates from 2 seconds to 0.2 seconds; the caption shows the selected wait rather than promising a total completion speed. The preference survives app restarts and can change while pasting or paused. Faster settings also poll acknowledgements more frequently. Initial room loads, bulk baselines and floor-plan reloads retain their two-second settling check. Placement, helper and state acknowledgements, server timeouts, sequential commands and duplicate prevention are unchanged. Actual throughput still depends on Habbo responses and construction steps.

## 1.3.44 — Automatic height repair and saved furniture settings

Floor-level furniture now reserves an optional fitting lajota for server height mismatches. Old paused jobs can select a spare tool during recovery even when the previous version marked a height attempt without sending one. Repairs move the existing destination item, preserve its rotation, verify its height and return temporary helpers; they never duplicate a placement. Missing tools and server rejections remain explicit failures.

Paste restores numeric floor-furniture states through Habbo's writable built-in state property when exposed by the current server, after inspecting the destination item. It verifies each state from room data instead of repeatedly double-clicking furniture. Special text, wall variants and unavailable state setters remain reported limitations.

Copy now previews immediately and reads available Wired configurations before saving. Backups retain settings text, integer parameters, selected furniture, secondary selections, input sources, variable references and the original version-bound packet including hidden context. Supported basic triggers, actions and conditions are restored after placement with destination furniture IDs and verified by a fresh read. Advanced inputs, snapshot-based effects, variable setups, missing selections, inaccessible configurations and other unsupported settings are explicitly listed. Earlier blueprints cannot retroactively acquire Wired data; copy the source again. Wallpaper, permissions and hidden-area effects are still not restored. Protocol and recovery regression tests are offline validation, not proof of live-server acceptance.

## 1.3.43 — C.A. access when the membership packet was missed

Fixed Paste blocking active C.A. accounts, including NFT-avatar benefits, when the room-copy observer connected after the membership-status packet. The live C.A. count query returns the used-item count but does not necessarily resend the membership/limit packet. A verified count and verified Builders Club catalogue offers now permit placement with the server enforcing access and allowance. An unreported limit is shown as unknown, never assumed to be zero or infinite. Any reported limit still constrains planning, placement and recovery, including limits received later. Server warnings and unconfirmed placements still pause without automatic retries. The review distinguishes a known allowance from a server-enforced allowance. No live furniture was placed during validation.

## 1.3.42 — C.A. / Builders Club room-paste support

Room copy now includes **Use C.A. / Usar C.A.**, enabled by default. Paste uses available owned inventory first, then verified single-furniture offers from the account's live Builders Club catalogue. The review shows inventory/C.A. quantities and projected allowance usage. Missing furniture and exhausted C.A. allowance can be skipped with the existing option. No purchase command is used. Source furniture being borrowed or rented no longer prevents copying its layout with an eligible owned or C.A. replacement.

The engine permits bounded C.A.-only catalogue reads, allowance queries and Classic floor/wall C.A. placement commands. Normal catalogue purchases remain outside this scope. Each C.A. placement is journalled before sending; its server-assigned furniture ID is reconciled from new room objects before further placements. Ambiguity, warnings, timeouts or changed offers/allowance pause the job; recovery never blindly resends an uncertain placement. Available catalogue data and allowance are required; if unavailable, open C.A. in Habbo or reconnect and retry. Spare construction lajotas still come from owned inventory. Existing state/Wired/hidden-setting limitations remain. Offline regression tests do not establish live-server placement success.

## 1.3.41 — Captured height precision and flexible lajota placement

Room snapshots can contain heights such as 1.8001, although Classic stack-tile commands use integer hundredths. Paste now permits offsets up to 0.0001 from the nearest supported height, shows those adjustments in the review, and verifies room acknowledgements with the same tolerance. Original blueprint heights remain unchanged. Larger precision differences and heights outside 0–80 remain blocked, with the furniture name, position and height in the error.

Automatic stacking recognizes the official 1×1, 2×1, 2×2, 4×4, 6×6 and 8×8 lajotas. The planner chooses the smallest available spare tile that covers each furniture footprint, rotates rectangular tiles, and can offset larger tiles inward at room edges. The entire helper must fit real floor. Recovery validates that coverage and accepts offset helpers. Temporary lajotas are reused and returned to inventory after completion. Existing server-permission and rented/borrowed-source restrictions remain in force. Tests and read-only planning do not establish live-server placement success.

## 1.3.40 — Recognize available inventory when pasting rooms

Fixed a room-copy inventory filter that incorrectly required flat ID 0. Habbo Classic reports unplaced inventory with flat ID -1, so owned furniture was incorrectly reported as missing. Planning, spare stack-tile selection, resume checks and tool-return confirmation now share the corrected availability rule. Placed, rented and Builders Club items remain excluded. Regression fixtures now use the actual client sentinel and cover both floor and wall furniture, recovery and stack-tile returns.

## 1.3.39 — Optional paste with missing furniture

Enable **Skip missing furniture** beside Paste to build with the owned copies currently available in inventory. Shortages are counted per source item, so a room needing three identical items can place one owned copy and report the other two as skipped. The review lists missing quantities before confirmation; the operation journal and final report retain the full skipped list. The saved blueprint and preview remain complete. A partial result explicitly reports placed and skipped counts, including zero placements when nothing is available.

The option defaults to off. Inventory is rechecked against the reviewed option before starting; recovery preserves that choice and skipped items. Ownership, empty destination, server permissions, valid item data and required construction tools remain enforced. It does not purchase replacements or automatically fill the remaining gaps later.

## 1.3.38 — Room preview and automatic geometry / stacking

The Room copy tab centres on an isometric view of the captured room. Copy saves and displays the room immediately. Saved rooms appear in a side library; scroll to zoom, drag to pan, or select furniture to inspect its coordinates. Floor and wall finishes are illustrative; native Habbo furniture artwork is loaded with its direction, numeric visual state, and original sprite anchor. Missing or unsupported artwork is visibly marked. The preview is a static reconstruction, not an embedded Habbo game client.

Paste now plans changes to the destination floor plan, tile heights, entrance, floor/wall thickness and custom wall height. It sends the recovered floor-editor command, reloads the destination once where needed, and verifies the resulting room data before placing furniture. Custom stacking reserves spare stack tiles of a suitable size, places/moves them, sets their height, places the furniture, verifies altitude, and returns the temporary tools to inventory. Every operation is journalled before sending; uncertain writes are reconciled rather than blindly resent. A before-edit geometry snapshot remains in the operation journal.

**Requirements and limits:** the destination must be an empty room owned by the selected account. Inventory must contain the furniture and any required spare stack tiles. The server still enforces access to the floor editor and item placement. Missing commands/tools, unsupported dimensions and rejected changes stop the build. Resetting a custom wall height to automatic is not exposed by the inspected client command. Furniture text/arbitrary states, Wired configuration, wallpaper/landscape, permissions and hidden effects are not restored. Tests and offline packaged checks do not constitute live-server validation or a guarantee of a complete room clone.

## 1.3.37 — Dedicated Room copy tab

Room copy and paste now has its own **Room copy** tab beside Room, using the existing Pixelio interface. Saved blueprints, Copy, Paste, Pause/Resume, Stop and Recover are embedded in the main window. Switching tabs keeps the copy session and progress; account changes still pause active work. The tab includes connection and language controls, and the compact header accommodates the new navigation item. The previous Room toolbar button and separate window are removed. Layout-copy capabilities and limitations remain unchanged.

## 1.3.36 — Room layout copy and paste in the existing interface

The Room tab now opens **Copy / paste room**. Open it before re-entering the source room, then save a named local blueprint. Enter a different empty room owned by the selected account with the same floor plan, select the blueprint, and click **Paste selected**. Pixelio refreshes inventory and shows a review before placing anything. It uses existing inventory copies; it does not duplicate or buy furniture.

Blueprints retain floor coordinates, direction, altitude, wall locations and observed furniture data separately from valuation scans. Placement runs one item at a time using named client metadata and a narrow validated engine command. Each placement is checked against the server response. Altitude or other layout mismatches pause for manual correction. Missing inventory, borrowed/rental items, sticky notes, unknown metadata, incompatible floor plans and occupied destinations block the relevant plan.

Pause, Stop and Recover paste retain a local operation journal. Uncertain placements are never resent automatically; recovery rereads the destination and checks inventory before continuing. Updates are blocked while a paste is running or paused.

**Scope:** this is a layout copier, not a complete room clone. Furniture states/text are captured and compared but not written. Wired configuration, links, variables, room settings and hidden settings are not restored. Special stacking may require manual correction. The rejected UI redesign is not included. Protocol fixtures, executor failure cases and packaged offline checks do not constitute live server validation.

## 1.3.35 — Measure ordinary floor clicks and use them in arrival calibration

Ordinary floor requests now have a separate first-step latency measurement. A distant tile click is timed to the first received own-avatar walking update; the remaining path is excluded. The collector records the first ordinary step duration separately and explains excluded clicks, including clicks while moving, rapid overlapping requests, furniture selections/uses, missing responses, Wired avatar motion and overlapping automated sends. Late Watch & Walk telemetry can retract an ambiguous sample. This is native outgoing-request to received-update time, including transport and scheduling, rather than physical mouse latency or one-way ping.

Calibrar chegada adds Medir cliques normais and an optional Use click latency mode. With eight recent valid click responses, five suitable completed first straight steps, and the existing three learned furniture movements, it computes a trial delay as learned Use-to-furniture lag + animation duration − avatar step duration − observed request response. It does not subtract RTT again. Three visually normal complete calibration trials can estimate the difference between native and automated response timings. Reported blocks bound later candidates; each candidate is frozen when armed, and stale or invalidated samples cancel before sending. Existing F8/manual cancellation and one-send-per-trial behavior remain.

The displayed historical range is an empirical estimate, not a safe collision boundary. Each trial still needs explicit arming and visual normal/blocked feedback. Click mode targets ±20 ms and ends after three confirmed near arrivals, three inconclusive results or 12 trials. Measure arrivals export schema 4 includes the bounded native-click history, and calibration exports include the exact equation inputs used in each trial. No click-timing controller was added to Watch & Walk; its existing scheduling remains unchanged.

## 1.3.34 — Learn ordinary steps onto raised furniture

In the supplied cushion session, all 120 retained ordinary steps changed height from 0.25 to 0.30 or back. Version 1.3.33 labelled them as height changes and excluded every one from cadence learning, so the live model stayed at zero samples and made no forecasts. Floor direction and elevation are now independent: straight steps can train the cycle while retaining their true heights and height-change category. Diagonals and unsuitable durations remain excluded.

Ping comparison now accepts eligible straight cushion steps but holds the complete source/destination route and heights constant, using the most represented route and displaying it. Request ambiguity, fresh-sample and variation gates remain. No automatic walking or delay changes were added.

The retained session contains 50 forward furniture movements: two near arrivals, 47 late arrivals and one inconclusive observation; another 50 rows are reset moves. A chronological replay with the corrected learner scored 36 later arrivals: 34 errors below 3.2 ms and two approximately 500 ms misses. This confirms that fitting the cadence alone does not guarantee which update cycle walking will use. Regression and packaged selfchecks include actual height-changing cushion steps.

## 1.3.33 — Continuous movement learning and ping observations

Measure arrivals now includes a Continuous learning tab. While measurement is running, the selected account's ordinary completed steps update a bounded receipt-cadence model. Straight, diagonal and height-changing step durations are kept separately. At least five suitable straight steps are needed; a stale or shifted pattern suspends forecasts and triggers relearning. Each furniture-arrival forecast is frozen before the avatar's arrival, then scored against the subsequent observation. Room/account/connection changes clear the model. This observes timings without changing walking delays or claiming a safe collision window.

Watch & Walk supplies its original local engine submission timestamp and a snapshot of RTT metadata. Unique matching own-avatar steps expose request-to-walking-update intervals; ambiguous clicks and requests from another room/account are excluded. The learning tab compares fresh matched RTT and response intervals only after sufficient samples and variation, without assuming RTT/2 is outbound delay. Export schema 3 includes the latest 120 steps and requests, up to 100 scored forecasts and the current model. The model is held in memory and does not restore an old timing phase after a restart.

Validation includes chronological replay of the supplied capture (nine later arrivals predicted with less than 2.4 ms error), phase drift/staleness, interrupted motion, timestamp handoff, click association, both interface languages and native engine regressions. Replay accuracy describes those recorded packet arrivals, not future live simultaneity or a tested send-time policy.

## 1.3.32 — Record avatar arrivals even when furniture does not move

With a fixed destination selected, Measure arrivals now records a completed own-avatar step onto that tile immediately, even before a furniture announcement. It waits up to two seconds for the selected furniture to announce a slide to the same tile. If none arrives, the row remains visible with the avatar's received arrival and step duration, an explicit missing-movement reason and no fabricated arrival difference. This covers the reported case where early avatar occupancy blocked the pillow at its source; missing traffic alone is not labelled a confirmed collision.

A recent arrival can be paired once with a subsequent slide while the avatar remains on the same tile. Negative announcement-relative times are supported. Early differences appear immediately as provisional until the nominal slide endpoint. Rearming, room/identity changes, stale arrivals, leaving the tile and cancelled steps cannot reuse an old arrival for a later test. Without a fixed destination, a recent completed step is retained briefly and a row is created only for matching furniture motion. Export schema 2 includes provisional status, nullable missing-furniture times and the two-second pairing bound. The UI adds an Avatar step column.

## 1.3.31 — Passive arrival differences alongside Watch & Walk

Wired → Measure arrivals (Medir chegada), also available inside Watch & Walk, measures the selected moving furniture separately from the trigger lever. It identifies the selected account's own avatar and compares its received arrival after a matching walking step with the furniture announcement plus advertised animation duration. The signed difference, both elapsed times and a configurable near-simultaneous tolerance (default ±20 ms) appear automatically for each observed slide. These are nominal packet timings, not rendered-frame simultaneity or proof that the slide completed normally.

The monitor uses Pixelio's existing engine without sending walks, changing Watch & Walk's rules or registering another F8 listener. A destination filter excludes reset moves. Up to 100 observations can be exported with their timing basis and per-observation tolerance. Missing arrivals, unsupported/interrupted motion and identity/room/connection changes remain inconclusive; changes stop measurement until it is started again. Early arrivals remain provisional through the nominal slide endpoint, so a reset during the slide cannot retain a false match. Tests cover measured timing traces, signed boundaries, other avatars, stale/missing steps, resets, identity changes, passive controls and both-language window layout.

## 1.3.30 — Selecting the armed trigger no longer cancels arrival calibration

Habbo emits ClickFurni followed by MoveAvatar when selecting a lever, even if the avatar remains still. While waiting for the selected trigger's Use, arrival calibration now recognizes one companion walk request within 100 ms of that trigger click. Native packets remain unchanged. Selecting the trigger does not start a trial or extend its 30-second arm period; timing still starts at UseFurniture.

A separate floor click, another furniture's click/walk sequence, an expired click association, or any manual walk during an active trial still cancels. Own-avatar updates also cancel immediately when the avatar starts moving or leaves its learned starting position while armed. The exception is cleared after use, cancellation, rearming and room changes. Regression coverage includes the native single/double-click packet sequence, unchanged forwarding, actual movement, cancellation and one scheduled send.

## 1.3.29 — Explain why arrival learning stays at 0/3

Arrival learning now acknowledges each furniture use, identifies a different trigger when used, and reports which other furniture moved after the selected trigger. A mismatched moving-furniture ID no longer leaves the initial learning message unchanged. Other furniture is only a temporal observation; the selected ID, learned samples and walking authorization are never changed automatically. A matching move in the same batch takes precedence over the diagnostic.

## 1.3.28 — Adaptive arrival calibration inside Pixelio

Wired → Calibrar chegada learns three repeatable furniture movements and adjusts a trigger-relative walk delay from observed avatar arrival and visual feedback. It identifies the selected account's own avatar from current room packets, uses Pixelio's existing engine and precise timer, and sends at most one walk per manually armed trial. The calibration is bounded to 12 attempts and cancels on F8, manual intervention, changed motion, room/account transitions, missing identity or dropped data. Watch & Walk and calibration windows run mutually exclusively.

Arrival is compared with the movement packet's nominal animation duration, separately from walk onset. Missing movement is inconclusive until the user confirms a block. Results and variability can be exported; the UI does not claim the exact server collision time or guaranteed early arrival. The previous controlled-room data contain no arrival before the nominal animation end (0 of 19 observed arrivals).

## 1.3.27 — Wired movement observations and engine requests

Added decoding of Wired movement announcements, including source, destination and animation duration. The Wired tab displays recent movements and labels nearby interactions as possible associations. Follow Wired destination walks to the announced tile; the existing Furniture moves mode keeps its selected tile. The embedded engine now accepts bounded walking, Wired inspection and latency requests; previous builds rejected those requests despite displaying the controls.

In a controlled cushion test, early occupancy blocked movement; a walk requested just after the server movement announcement reached the cushion successfully. Other collision setups remain unverified. Delayed follows cancel if the watched furni moves again, and saved setups reopen disarmed.

## 1.3.26 — Engine-only Wired and Watch & Walk

Removed manual/advanced connection controls from Wired and the integrated Watch & Walk window. Both use only the selected account's authenticated Pixelio engine. External sources are ignored. Watch & Walk follows account changes and reconnects disarmed, with bounded retry attempts.

## 1.3.25 — Automatic Wired connection

The Wired observer automatically follows the selected connected Pixelio account, including its built-in engine credential. It reconnects after connection changes, clears the previous account's selection and waits when no account is connected. Manual port controls are under Advanced connection for external G-Earth use. Watch & Walk also receives the selected engine credential.

## 1.3.24 — Click furniture to fill the Wired ID

Clicking a floor furniture in Habbo Classic now fills the Furniture ID field while the Wired tab is open and its observer is connected. Manual entry remains available. The click only selects an ID; linked-Wired lookup still uses its existing button. Wall items, invalid IDs and stale connection events are ignored.

## 1.3.23 — Wired inspection and Watch & Walk

Added a Pixelio Wired tab for furniture-linked Wired discovery, configuration selection and configured delay in seconds. Includes the full Watch & Walk tool window with state/movement triggers, adaptive delay, cooldown, F8 controls and saved room mappings. Room/account transitions clear inspection requests and stop the watch connection. Package includes the watch UI assets. See PIXELIO_WIRED.md.

## 1.3.22 — Rounded furniture-panel edges (2026-09-12)

- Round the furniture-list and item-detail panel borders, search field, and custom-price inputs.
- Keep all existing layout dimensions, row spacing, controls, icons, selection and pricing behavior.
- Validation: before/after native widget geometry is identical in both languages at 1180 and 1536 pixels; 17 currency and language checks pass.

## 1.3.21 — BRL values inside the summary cards (2026-09-12)

- Move current and average BRL amounts into their corresponding credit-value cards; remove both standalone BRL blocks.
- Show the conversion rate directly below the current BRL amount only, using the same conversion constant as the totals.
- Preserve all four summary icons, unpriced counts, NFT values and the USD/BRL controls.
- Validation: 17 currency and language checks; native layout verified in English and Portuguese at 1180, 1351 and 1536 pixels.

## 1.3.20 — Restore the original connection flow (2026-09-11)

- Removed the Chrome launch alternative, extension installation prompt, launch-method selector, native messaging bridge, Web Store routing and drag installation.
- Connect account prepares the built-in connection directly; the user enters Habbo Classic manually as before.
- Ignore old Chrome launch preferences. Preserve saved accounts, scans and normal Windows connection setup.
- Remove browser-extension assets and the drag dependency from distribution. Rebuild the connection engine without Chrome-specific readiness hooks.
- Validation: 65 connection, setup, scanner and account checks; packaged checks also verify that retired Chrome modules and assets are absent.

## 1.3.19 — Recover Chrome setup and drag to install (2026-09-11)

- Fix a retry timer race that could leave Chrome installation completion unprocessed and its button disabled.
- Replace folder selection and path pasting with a native card dragged from Pixelio onto Chrome's extensions page after enabling Developer mode.
- Reopen Chrome without restarting helper installation. Recover unexpected setup errors, preserve cancellation and defer connection completion until dragging ends.
- Bundle and verify native drag support. Retain manual launch and folder-copy fallback.
- Register Web Store Item ID pfcippmfdghhdiekeadpgbnkklbhbpml in the native helper. Use the store installation flow only when the fixed public status file reports publication; retain developer setup while the listing is a draft.
- Validation: 100 Python checks, JavaScript behavior checks, native Tk drag registration and English/Portuguese layout checks. Real Chrome installation and signed-in launch remain unverified.

## 1.3.18 — Chrome setup from Connect account (2026-09-11)

- First Connect account now offers Chrome extension installation or the existing manual workflow, and remembers the choice.
- Setup opens Chrome's extensions page and copies the unpacked extension folder path. Pixelio detects readiness and continues automatically.
- Returning Chrome users connect directly when the extension responds. Manual users are not prompted again. Setup can be cancelled with the same Connect button.
- Readiness probes do not request a Habbo ticket. Added onboarding, cancellation, saved-choice and native-protocol checks. Chrome still requires one-time Load unpacked installation; a signed-in live launch remains unverified.
## 1.3.17 — optional Chrome launch (2026-09-11)

- Added a saved manual/Chrome launch choice to Connections. Manual remains the default; both modes use the existing Connect/Cancel/Disconnect button.
- Automatic launch waits for authenticated scanner attachment, routing and all proxy listeners. Chrome uses the signed-in Habbo BR tab; credentials stay on the website.
- Added an optional Manifest V3 extension, per-user native helper setup, bounded local messaging, cancellation, request expiry and manual fallback. No browser policy installation or store publication.
- Added Python/JavaScript behavior checks and packaged native-helper verification. See PIXELIO_CHROME.md for preview setup and live-test limitations.

# Pixelio — Complete development changelog

**Last updated: 11 September 2026**

**Order: oldest first, from the original inventory scanner to the current Pixelio app.**

This history covers the recoverable features, visual changes, experiments, reversals, fixes, packaging work, and releases. It was reconstructed from the original development conversation, saved source copies, current implementation and tests, local build manifests, and GitHub release records. The development checkout had no source commits when this history was reconstructed, so this is a development history rather than a commit-by-commit export. The public GitHub repository tracks distribution documentation and releases separately.

Times below use **Europe/London (BST, UTC+1)**. Time ranges generally run from the request to its reported completion. Some changes were developed together or overlapped. Early work had no release number; several later local builds retained **1.2.0** rather than receiving separate patch numbers.

## Release map

| Version / period | Date | Main milestone | Release status |
|---|---|---|---|
| Unversioned inventory scanner | 8 September 2026 | Inventory valuation, redesigned interface, accounts, rooms, wardrobe, currencies, and Pixelio branding | Local development |
| 1.0.0 | 9 September 2026 | First self-contained Windows launcher | Confirmed local build |
| 1.0.1 | 9 September 2026 | Removed large page titles and reclaimed vertical space | Confirmed local build |
| 1.1.0 | 9 September 2026 | Startup update popup and in-app download/restart flow | Confirmed local build; no separate public release currently listed |
| 1.1.1 | 9 September 2026 | Redesigned Connect button; first verified public update | Published on GitHub and installed through the updater |
| 1.2.0 | 9 September 2026 | Embedded connection engine, one-time connection setup, connection fixes, animated Scan control, and full changelog; developed through successive local builds | Public release package; supersedes 1.1.1 |
| 1.2.1 | 9 September 2026 | Exclude borrowed Builders Club copies from room valuation | Installed locally; not published |
| 1.2.2 | 9 September 2026 | Marketplace availability count/filter and complete Builders Club omission | Installed locally; not published |
| 1.2.3 | 9 September 2026 | Retain every furniture batch in large rooms | Installed locally; not published |
| 1.2.4 | 9 September 2026 | Prioritize requested scans and gradually recover price pacing | Installed locally; not published |
| 1.2.5 | 9 September 2026 | Recover connection setup without repeating a successful installation | Installed locally; not published |
| 1.2.6 | 11 September 2026 | Persistent price estimates, shared observations and artwork history index | Installed locally; not published |
| 1.2.7 | 11 September 2026 | NFT and BC/CA tags, category filters and ordering | Included in 1.2.8 |
| 1.2.8 | 11 September 2026 | Windows installer, recurring update notifications and verified renderer rebuild | Published on GitHub and installed locally |
| 1.2.9 | 11 September 2026 | Exclusive orange NFT badge; filters and valuation unchanged | Published on GitHub |
| 1.2.10 | 11 September 2026 | Remove redundant interface guidance and legends | Published on GitHub |
| 1.2.11 | 11 September 2026 | Compact dashboard captions, relocated counts and Média title | Published on GitHub |
| 1.2.12 | 11 September 2026 | Matching mint-and-gold summary icons | Published on GitHub |
| 1.2.13 | 11 September 2026 | Habbo-inspired summary icons and balanced card spacing | Published on GitHub |
| 1.2.14 | 11 September 2026 | Integrated spacious window header and aligned controls | Published on GitHub |
| 1.2.15 | 11 September 2026 | Coherent dropdowns aligned to their fields | Published on GitHub |
| 1.2.16 | 11 September 2026 | Smooth navigation tab transitions | Confirmed local build |
| 1.3.0 | 11 September 2026 | Integrated clothing Try-On and local outfits | Published on GitHub |
| 1.3.1 | 11 September 2026 | Scrollable clothing grid, category-specific pieces, responsive previews and smooth card borders | Published on GitHub |
| 1.3.2 | 11 September 2026 | Split colour palettes and restored clothing-product icons across categories | Published on GitHub |
| 1.3.3 | 11 September 2026 | Category-focused fallback thumbnails for standard and unmapped pieces | Published on GitHub |
| 1.3.4 | 11 September 2026 | Grouped wardrobe navigation with 15 transparent icons, Pets and Accessories | Published on GitHub |
| 1.3.5 | 11 September 2026 | Refined Extras icon and Shoes under Lower body | Local build; not published |
| 1.3.6 | 11 September 2026 | Account-scoped Habbo Club detection in Looks and Try-On | Local build; not published |

**v1.1.1**, published at **13:12 BST** on 9 September 2026, was the first public update. **v1.2.0** packages the later connection and Scan improvements for the public updater, together with this changelog. Each release provides the executable, portable ZIP, and ZIP checksum. [Version 1.1.1](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.1.1) · [Version 1.2.0](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.0).

## Contents

- [Before Pixelio: the existing foundation](#before-pixelio-the-existing-foundation)
- [8 September — inventory scanner to complete account valuation](#8-september-2026--inventory-scanner-to-complete-account-valuation)
- [9 September — portable releases, connections, and Scan refinements](#9-september-2026--portable-releases-connections-and-scan-refinements)
- [Current behavior and unfinished capabilities](#current-behavior-and-unfinished-capabilities)
- [Verification history](#verification-history)
- [Evidence and future entries](#evidence-and-future-entries)

## Before Pixelio: the existing foundation

The project already contained a separate **Habbo Marketplace Observed Offer History** tool. Its connection and marketplace parsing code provided a starting point for the inventory scanner.

- Read marketplace responses through a local G-Earth connection.
- Distinguished the lowest current offer from the reported average price.
- Matched furniture identities using the correct floor/wall category and type ID.
- Maintained observed-offer history in SQLite and supported a terminal feed.
- Had already established that grouped marketplace responses do not reveal every individual listing or a true median of individual sales.

The marketplace feed remained a separate tool. Its standalone watchlist, terminal history, and polling controls did not become Pixelio features merely because they shared this workspace.

## 8 September 2026 — inventory scanner to complete account valuation

### 01. Original inventory scanner — 13:25–13:39

**Added**

- Created the first inventory scanner for the connected Habbo account.
- Read the complete inventory, including repeated copies of the same furniture type.
- Assembled inventory fragments before starting valuation; missing or malformed pages could not count as a complete inventory.
- Grouped matching furniture and displayed its name, quantity, lowest current price, reported average price, and total value.
- Added separate overall current-price and average-price estimates.
- Matched prices using furniture identity rather than relying on names alone, including floor/wall ID separation.
- Showed price coverage and left unknown prices unpriced instead of assigning zero.
- Added scan/stop behavior, saved JSON/CSV output, and a desktop results window.
- Added `Scan Inventory.cmd` and the ability to reopen saved results.
- Added client-version and packet-format validation, unchanged forwarding of intercepted packets, and bounded read requests.

**Clarified**

- The original request used “median.” The available Habbo field was an **average**, so the implementation and labels used average value.
- LTDs and other furniture requiring individual appraisal initially remained unpriced. LTD support was added later.

**Verified:** the first live run received 14,909 items across 716 furniture types. The reported initial test run passed 59 checks.

### 02. Furniture icons, exclusions, and bulk prices — 13:41–13:50

**Added**

- Furniture icons beside inventory rows, loaded from official Habbo artwork.
- Background image loading and caching so artwork did not block price processing.
- **Exclude selected** and **Restore selected** actions that changed the valuation without removing furniture from Habbo.
- Custom bundle pricing, including rates such as **100 items = 1 credit**.
- Custom rates applied to both current and average estimates.
- Persistent exclusions and custom prices across rescans and application restarts.
- Export fields for the original marketplace values, adjusted values, exclusions, and custom bundle rates.

### 03. Refined interface concept and implementation — 13:52–14:15

**Designed and implemented**

- Replaced the initial basic presentation with a dark navy dashboard and mint accents.
- Added summary cards for current value, average value, items included, and custom prices.
- Added search, sortable columns, filters, and checkbox selection.
- Added a dedicated furniture inspector with artwork and valuation details.
- Added custom-price previews before applying a bundle rate.
- Added reversible exclusion controls and restoration within the redesigned layout.
- Added navigation for inventory, adjustments, and saved scans as the app expanded.
- Kept totals based on all included items, independent of the visible search/filter results.
- Supported keyboard and range selection, and an independently scrollable inspector for smaller windows.
- Kept the application native; the main interface did not require a web server.

The visual concept was shown before implementation. Existing saved inventory and adjustments were retained.

### 04. Multiple accounts and account-specific history — 14:19–14:35

**Added**

- A **Connections** manager for multiple local G-Earth extension ports.
- An account selector in the top bar.
- Verified account identity using the hotel and public numeric account ID, rather than display name alone.
- Dated saved scans for each account.
- Separate exclusions and custom prices per account.
- Independent connection state and progress; identical furniture IDs on different accounts could not share the wrong prices.
- Up to eight external connections, with duplicate-port rejection.
- Migration of the original unassigned snapshot into **Previous saved scan**.
- **Assign previous scan** to attach that legacy snapshot to a verified account while retaining its adjustments.
- Read-only history browsing that did not replace or interrupt an active scan.

At this stage, simultaneous accounts required separate G-Earth instances/ports. The app did not log into Habbo for the user.

### 05. Missing furniture names and connection diagnosis — 14:51–14:55

**Fixed**

- A crash caused by missing, blank, or invalid furniture names.
- Added fallback names using the classname, then the furniture ID when metadata was unavailable.
- Kept scans and archiving functional when individual metadata entries were incomplete.

**Diagnosed**

- Distinguished a stopped/missing G-Earth listener from a scanner failure. One reported second-account failure was a port with no listener.

### 06. Accounts already logged in and overlapping inventory refreshes — 15:02–15:11

**Fixed and added**

- Added **Connections → Detect logged-in account** for cases where Pixelio attached after the login event had already happened.
- Sent a bounded read-only identity request after the account was known to be fully loaded.
- Kept authentication readiness separate from the mere existence of a G-Earth connection, avoiding requests during the game handshake.
- Prevented a previous connection’s identity or responses from being reused after reconnecting.
- Detected overlapping client inventory refreshes, discarded mixed partial pages, waited for a quiet interval, and retried the inventory baseline.
- Limited baseline retries and allowed Stop scan to cancel a pending retry.

**Verified:** both connected accounts loaded their inventories successfully; 88 checks passed in that revision.

### 07. Cleaner controls and original-artwork previews — 15:26–15:38

**Changed**

- Smoothed checkbox edges and removed selection artifacts.
- Cleaned up the exclusion toggle.
- Replaced rough Exclude and Restore symbols with antialiased artwork and improved their spacing.
- Added larger furniture previews using the original artwork instead of stretching the small catalog thumbnail.
- Preserved transparent backgrounds, interior cutouts, and furniture color variants.
- Added an offline furniture renderer for original Habbo SWF artwork.
- Used a separate preview worker and cache, with distinct identities for revision, color, and direction.
- Ensured a newer selection superseded queued work for an older selection.

### 08. Original Habbo inventory symbol — 15:44–15:54

**Changed**

- Replaced the generic inventory cube beside the app title with Habbo’s original inventory symbol.
- Extracted and retained the original **44 × 41 px** bitmap at native resolution.
- Kept its transparency and pixel-art appearance.

A genuine higher-resolution source was not found. This change used the original artwork rather than claiming a newly discovered HD asset. The title-area symbol was later replaced by the Pixelio wordmark.

### 09. Current-value icon: coins → 10c coin → 20c bag — 15:44–15:54

**Changed in two steps**

1. Replaced the generic coin-stack symbol with Habbo’s gold **10-credit coin**.
2. Replaced that coin with the requested **20-credit money bag**.

The final bag uses the original transparent artwork at its native **48 × 57 px** size. The old 10c PNG remains an unused asset; it is no longer the current-value icon.

### 10. Icons matched to each action and statistic — 15:49–15:54

**Changed**

- **Average value:** generic chart → Habbo marketplace stall.
- **Items included:** generic cube → inventory box.
- **Custom prices:** generic document → pencil/edit artwork.
- **Connect account:** generic scan-like symbol → person with a plus sign.
- Used native or integer-scaled pixel artwork with transparent backgrounds.

The person-plus connection icon was later replaced by the plug design in version 1.1.1.

### 11. Excluded status badge — 15:47–15:54

**Changed**

- Reworked the small, rough **Excluded** label into a cleaner badge.
- Improved its shape, spacing, and legibility within furniture rows.
- Preserved the distinction between an item being present in the inventory and excluded from the valuation total.

### 12. PT-BR / ENG language controls — 15:55–16:11

**Added**

- Brazilian Portuguese as the default interface language.
- **PT-BR / ENG** controls beside Export.
- **Mobi / mobis** in Portuguese and **furni** in English.
- Persistent language preference across restarts.
- Translated labels, filters, dialogs, scan messages, dates, and number formatting.
- Immediate language changes without losing account selection, filters, selected rows, live connections, or unfinished custom-price input.
- Decimal inputs accepting either comma or period while keeping saved/exported data independent of the display language.

Official furniture names retained their Habbo BR names.

### 13. Room scanner and owner separation — 16:16–16:38

**Added**

- A **Room / Quarto** valuation view.
- Capture of the current room’s floor and wall furniture.
- An owner selector defaulting to **My furni / Meus mobis**.
- The ability to select another owner or **Everyone / Todos**.
- Separate rows for different owners even when they owned the same furniture type.
- Owner names in room rows and owner identity in exports.
- Current and average room valuations using the selected owner scope.
- Per-room history, exclusions, and custom prices kept separate from inventory adjustments.
- Room settings scoped by account, room, and furniture owner.
- Shared price lookup results for matching furniture types within a room scan.
- Stale-state detection when furniture, ownership, or the room changed.
- Complete floor and wall baselines, including empty lists, before treating a room capture as complete.

The implementation observed room state without moving the avatar, picking up items, or automatically re-entering the room.

### 14. Named room history — 16:45–16:54

**Added**

- **Room history / Histórico de quartos**.
- Search by saved room name or room ID.
- One room entry with its latest scan date and count of saved snapshots.
- Reopening the latest saved valuation by double-clicking or using Open saved scan.
- Access to older scans of the same room, with captured names and dates.
- Offline owner filtering and valuation adjustments when viewing history.
- A fallback name for rooms without a captured title.

Opening history does not teleport the account or rescan the live room; it reopens the saved evaluation.

### 15. Removed the low-quality preview flash — 16:48–16:54

**Fixed**

- Removed the inspector’s fallback that briefly enlarged a catalog thumbnail before the original artwork arrived.
- Kept a transparent placeholder until the full-quality artwork was ready.
- Continued using compact icons in the table, where they remained appropriate.

### 16. Scrollbar redesign — 16:57–17:03

**Changed**

- Replaced thin native-looking bars with rounded thumbs.
- Increased the area available to grab and drag.
- Added hover feedback.
- Removed tiny arrows and ridged grips.
- Applied the treatment to the furniture table, inspector, and room history.

### 17. Faster price requests: 250 ms experiment — 20:27–20:33

**Experiment, later replaced**

- Reduced the default request spacing from **600 ms to 250 ms** for inventory and room valuation.
- Started the first request promptly and removed an unnecessary delay after the last reply.
- Retained automatic slowdown on timeouts.
- Kept requests tied to the exact furniture identity and connection generation.

The faster setting caused missed replies and pauses in a live scan. It was not retained as the final default.

### 18. Price-loading stability repair — 20:39–20:46

**Fixed**

- Restored **600 ms** pacing after observing five request timeouts.
- Removed automatic acceleration after a timeout; a slowed scan remained slower for its remaining work.
- Added one retry for missing prices after the first pass, allowing other furniture to continue first.
- Left unresolved prices explicitly unavailable and marked the scan partial when appropriate.
- Used timeout backoff and a cooldown rather than immediately sending more requests.
- Measured request spacing from the actual send to avoid bursts after processing a large inventory.

**Verified:** a live check received **30/30** prices without timeouts.

### 19. Final default of 500 ms — 20:48–20:50

**Changed**

- Set the default request gap to **500 ms**, between the original 600 ms and the unstable 250 ms experiment.
- Preserved bounded retries, timeout backoff, and the single outstanding request limit.
- Applied the setting to both inventory and room scans.

**Verified:** a live trial received **60/60** prices without timeouts. The suggested 450 ms setting was not adopted.

### 20. Visible scan progress — 20:51–20:56

**Added**

- A progress bar for inventory and room price scans.
- Percentage complete, furniture types checked, and types remaining.
- Preparation messaging while the inventory/room list was still being collected.
- Explicit incomplete/cancelled progress states and recorded progress for saved scans when available.
- Retry work remained in the remaining count until resolved.
- Progress stayed tied to the full scan rather than changing with owner or search filters.

Progress counts distinct price lookups, not the number of physical furniture copies.

### 21. Simultaneous inventory and room scans — 21:00–21:25

**Added**

- Inventory and room scans running at the same time on one connection.
- Independent progress, results, Stop scan actions, and saved histories.
- Two labeled progress bars when both jobs were active.
- Fair alternation of marketplace work while sharing one outstanding request and the minimum **500 ms** gap.
- Timeout slowdown shared across the connection rather than allowing another job to bypass it.
- Cancellation that drained a pending response safely before another job could use the request slot.
- Disconnection handling that prevented old responses from crossing into a new account/session.
- Preservation of saved values during reconnect until a new scan was available.

### 22. Whole-credit display — 21:08–21:25

**Changed**

- Rounded displayed credit values to the nearest whole credit, with **.5 rounding up**.
- Preserved exact values in calculations, stored observations, editable bundle rates, and exports.
- Kept small fractional rates useful for bulk pricing instead of rounding them to zero before calculation.

### 23. LTD valuation by nearest serial — 21:08–21:25

**Added**

- Retained owned LTD serial numbers and series sizes in new inventory and room captures.
- Requested individual LTD listings instead of treating all unique copies as one combined listing.
- Matched the same furniture identity and series size.
- Estimated each owned LTD from the closest returned listing serial, as requested.
- Broke equal-distance ties by lower asking price, then serial and offer ID.
- Summed the estimates for the owned copies; kept the reported sales average as a separate measure.
- Showed serial-match information in the inspector and saved/exported the full match details.
- Marked limited marketplace result sets explicitly and left unmatched copies unpriced.
- Preserved existing exclusions and custom-price keys.

**Verified:** a live nearest-serial lookup was completed. “Closest” means closest among the listings returned by Habbo, not a guarantee about listings outside a capped result set.

### 24. BRL valuation cards — 21:47–22:05

**Added**

- Two BRL cards above the current and average credit cards.
- The requested fixed reference rate: **1,000 credits = R$200**, or **R$0.20 per credit**.
- Whole-real display with precise underlying calculations and exports.
- A visible explanation of the conversion rate.

This is the user’s chosen credit-to-BRL reference rate, not a live credit exchange rate or an official cash-out rate.

### 25. NFT quotes and live USD/BRL conversion — 21:47–22:05

**Added**

- A separate NFT valuation summary with priced/unpriced coverage.
- Product-specific collectible listing quotes from Immutable’s marketplace data.
- Validation of product code, supported contract, chain, and single-unit listing amount before using a quote.
- Fee-inclusive USD values supplied by the provider, including conversions of supported token-denominated listings.
- Live USD/BRL reference quotes from AwesomeAPI.
- An **NFT prices / USD → BRL** details window, listing links, and a conversion calculator.
- NFT refreshes every five minutes and FX refreshes every minute, with manual refresh controls.
- A separate HTTP worker so these requests did not consume the G-Earth marketplace request slot.
- Cached/stale quote labels when a refresh failed or provider data was old.
- Exclusions, custom-price handling, and room owner filtering applied consistently to NFT totals.
- NFT estimates kept separate from credit estimates to avoid double counting.

The initial live result priced **202 of 206** inventory NFT items. NFT figures are listing-based estimates; no wallet login, purchase, sale, or transfer feature was added.

### 26. Account wardrobe / Looks tab — 22:06–22:26

**Added**

- **Looks / Visuais** as a separate valuation area.
- Captured unlocked clothing, identifiable NFT clothing, and Habbo-reported NFT avatar/wardrobe records.
- Search, categories, artwork, and CSV export.
- Independent saved wardrobe records per verified account.
- PT-BR and ENG support.
- Identification from bound product codes in Habbo’s unlocked-figure response.
- Additional identification by matching all required unlocked figure parts to official product metadata.
- Labels distinguishing inferred identifications from directly bound products.
- Protection against treating a partial outfit match as a complete outfit.
- Separation from inventory and room totals.

This recorded availability in the wardrobe. It did not establish a history of when an outfit was worn or prove blockchain ownership of a token.

### 27. Scan looks control and initial-capture handling — 22:27–22:38

**Changed and fixed**

- Added an explicit **Scan looks / Escanear visuais** button.
- Reloaded the latest captured unlocked-clothing list and requested Habbo’s NFT wardrobe list when the runtime mapping was available.
- Bounded the NFT wardrobe request frequency and handled timeouts without discarding the previous saved data.
- Replaced misleading empty-state behavior with a clear “not captured yet” / initial synchronization status.
- Fixed the assumption that zero NFT avatars meant the complete clothing wardrobe had arrived.
- Clarified the reconnect sequence: the scanner must be listening before Habbo sends the login-time unlocked list.

**Still limited:** a fresh complete unlocked-clothing list could not be forced in one click after login. Scan looks can reuse and later reprice the captured list; a newer unlocked list still depends on the server sending it.

### 28. Clothing value evaluation — 22:40–23:03

**Added**

- Current and average marketplace pricing for one copy of each identified unlocked clothing product.
- Independent clothing scan progress, cancellation, and saved valuation state.
- A third valuation job sharing the same fair, single-request **500 ms** marketplace schedule with inventory and room scans.
- BRL current/average totals at the same fixed credit rate.
- Separate NFT clothing quotes and USD/BRL conversion through the existing quote worker.
- Coverage, price-source timestamps, and valuation details in wardrobe exports.
- Repricing of already captured clothing without another login.
- Deduplication so a directly identified tradable product was not also counted as its inferred untradable duplicate.

Wardrobe credit totals describe the replacement cost of unused clothing products. Consumed clothing is not added to the app’s sellable furniture total, and NFT avatar values are not inferred from ordinary clothing prices.

### 29. Larger wardrobe icons and a simpler table — 22:45–23:03

**Changed**

- Increased wardrobe icons to **64 px** within **72 px** rows.
- Removed the visible **Product / token** column.
- Kept product identifiers internally for matching and export while presenting a cleaner list of names and categories.

### 30. Missing icons and incorrect NFT previews — 22:47–23:03

**Fixed**

- Diagnosed valid official PNGs, including **Bóia Donut**, being rejected because oversized Adobe/XMP metadata exceeded the old image limit.
- Allowed bounded larger downloads, validated PNG dimensions, and re-encoded the image without unnecessary metadata.
- Preserved the actual pixels and transparency.
- Fixed NFT clothing previews displaying a generic blue box from their furniture SWF.
- Used the actual transparent clothing artwork for those NFT clothing previews.
- Left ordinary furniture on the original-artwork rendering path.

**Verified:** Bóia Donut displayed correctly after the change.

### 31. Pixelio name and wordmark development — 23:04–23:32

**Brand decision**

- Renamed the product from an inventory-value tool to **Pixelio**, reflecting inventory, room, and wardrobe evaluation.
- Chose a name intended to work in both English and Portuguese.

**Design iterations, in order**

| Step | Change | Outcome |
|---|---|---|
| Initial symbol | Dimensional pixel-art P in mint/turquoise with a gold accent | Concept shown |
| Full wordmark | Extended the same dimensional treatment across “Pixelio” | Concept shown |
| Transparency request | Attempted transparent versions; some previews contained a baked-in checkerboard | Identified as previews needing cleanup |
| Habbo H inspiration | Tried chunkier lettering, stepped corners, and heavier outlines | Revised concept |
| Closer lettering reference | Recreated tightly packed yellow lettering with orange depth and dark outlines | Revised concept |
| Edge refinement | Smoothed the wordmark’s outlines and corners | Preserved the dimensional lettering |
| Checkerboard removal | Removed the baked-in background and verified a real alpha channel | Transparent PNG saved |
| App integration | Replaced the title-area inventory symbol/text and renamed the window Pixelio | Installed in the app |
| Theme colors | Changed the logo to mint faces, teal depth, and navy outlines | Final theme-matched wordmark installed |

The app retained the full-resolution transparent source and used high-quality display scaling.

## 9 September 2026 — portable releases, connections, and Scan refinements

### 32. Version 1.0.0 — portable Windows launcher — 00:04–00:19

**Added**

- A self-contained **Pixelio.exe** for Windows 10/11 x64.
- Bundled Python, Tkinter, Pillow, interface artwork, and the native furniture renderer.
- A portable ZIP and `Start Pixelio.cmd` launcher in the project.
- Application/window icon resources based on Pixelio’s P.
- Startup that reopened saved evaluations without automatically scanning.
- A persistent per-user profile outside the executable folder, so replacing the app preserved saved data.
- Distribution contents that excluded personal accounts, scans, caches, and valuation settings.
- Offline packaged-installation checks using temporary data.
- Verification of a folder bundle, a single executable, and a relocated executable in a path containing spaces and accented characters.
- Dependency notices and build information alongside the distribution.

At this point Habbo and a separate G-Earth installation were still needed for live scanning. The target was specifically **Windows 10/11 x64**; “any Windows PC” was not expanded into verified x86 or ARM64 support.

### 33. Version 1.0.1 — removed oversized page titles — 00:22–00:31

**Changed**

- Removed the large Inventory value, Room value, Account looks, and saved-view headings.
- Reclaimed their vertical space for controls and content.
- Tightened the remaining header spacing while retaining the top navigation.
- Rebuilt the portable launcher with the layout changes.

The local build manifest confirms the **1.0.1** version number.

### 34. Version 1.1.0 — update popup and in-app installation — 00:35–00:52

**Added**

- Startup update checks against **CarlosPlacinta/Pixelio** on GitHub.
- A localized **Update now / Later** popup.
- Background checking so startup and the interface were not blocked by network failures.
- Stable-version comparison that skipped prereleases and downgrades.
- Download of the exact release executable, with size and SHA-256 verification.
- An offline installation check on the downloaded application before replacement.
- A separate updater process that waited for Pixelio to exit, replaced the executable, and reopened the app.
- Staging beside the installed executable and retention of the previous executable for recovery.
- Preservation of the existing data directory and relevant restart arguments.
- PyInstaller runtime-environment handling for independent restarts.
- Graceful handling of no release, failed checks, or deferred updates.

The first GitHub publication was still pending at this stage. Friends using older builds needed an update-enabled executable once before they could receive future popups.

### 35. Version 1.1.1 — Connect redesign and first public update — 13:02–13:19

**Changed**

- Replaced the person-plus icon with a clear **plug icon in a teal tile**.
- Refined rounded corners, borders, and button depth.
- Added brighter hover feedback, keyboard focus feedback, and a pressed state.
- Changed mouse activation to happen on release inside the button; dragging away cancelled the action.
- Applied the same design to the main window and the account-connections dialog.

**Released and verified**

- Published **v1.1.1** with the executable, ZIP, and checksum.
- Verified that the Desktop copy detected the update popup.
- Completed a real update through **Atualizar agora** and confirmed the restarted application version.
- Verified that saved scans and wardrobe records survived the update.

This was the first public release and was later superseded by 1.2.0. [Release and download assets](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.1.1).

### 36. First local 1.2.0 approach — automatically open G-Earth — 13:20–13:45

**Added, then superseded as the main connection approach**

- Opened an installed G-Earth automatically when Pixelio opened, following the requested preference.
- Reused an existing G-Earth instance rather than launching duplicates.
- Added G-Earth configuration and an option to disable automatic opening.
- Built and tested a local **1.2.0** package.

This still used a separate G-Earth application. The next change replaced that main workflow with an embedded engine. This local 1.2.0 was not published.

### 37. Embedded G-Earth connection engine — 13:46–14:20

**Added**

- Included G-Earth’s connection core and its Java runtime inside Pixelio.
- Added connection preparation, waiting, connected, cancellation, disconnection, and error states to Pixelio’s controls.
- Connected through Pixelio without a separate G-Earth installation or visible G-Earth window.
- Retained external G-Earth connections in the advanced connection controls.
- Extracted the embedded runtime into a verified cache when needed.
- Validated archive paths, sizes, hashes, and cache contents before execution.
- Used an OS-selected loopback port and a per-session authentication cookie for the internal scanner bridge.
- Limited the bridge to Pixelio’s supported read requests rather than exposing a generic packet interface.
- Stopped the owned engine when Pixelio exited and handled cancellation during preparation.
- Retained upstream license notices for the embedded engine and runtime.

**Changed from the previous approach**

- Normal startup showed saved data; the live engine waited for **Connect account**.
- The first implementation offered Windows authorization when protected routing needed it. That repeated Java authorization was replaced by the one-time setup in entry 43.

The first live test did not connect successfully. The following entries record the fixes rather than treating the initial build as a fully verified live connection.

### 38. Fixed the missing javaw.exe path — 14:20–14:40

**Fixed**

- Diagnosed the Windows “specified path does not exist” error during authorized engine startup.
- Resolved redirected AppData/runtime paths to their physical locations before passing them to the elevated process.
- Kept engine configuration, runtime, and session paths consistent across the launch boundary.
- Improved launch-error reporting and authorization retry behavior.

The administrator startup check passed after the repair. End-to-end Habbo scanning was verified in the next stage.

### 39. Restored Scan and fixed live mapping initialization — 14:45–15:17

**Fixed**

- Restored **Scan inventory** as a separate control from Connect/Disconnect.
- Kept Scan visible even when the connection was unavailable, with an explanation of why it was disabled.
- Ensured a Scan action could not silently become a disconnect action.
- Routed Scan/Stop to the selected inventory, room, or wardrobe context.
- Added support for the verified current September 9 AIR client build.
- Restored G-Earth’s native **Brotli decoder initialization** before loading compressed packet-name mappings.
- Reported empty/unsupported mappings clearly instead of continuing with an unusable connection.
- Prevented the updater from trying to reconnect to an expired internal engine port.

**Verified:** the live Desktop test identified the account, loaded 1,142 packet mappings, received the complete **27,803-item** inventory, and began receiving price responses with zero parsing errors. The recorded relevant regression run passed 118 checks and the three packaged installation checks.

### 40. Additional P and Scan artwork studies — 9 September, separate artwork task

**Created as separate artwork deliverables**

- An additional mint-and-teal P taskbar concept, exported as a transparent PNG and a Windows ICO containing 16–256 px sizes.
- A separate dark-teal targeting-frame Scan icon concept.

These were delivered in the separate **Create Pixelio taskbar icon** task. They are recorded as artwork deliverables, not as proof that each concept was installed into the production interface. The Scan control ultimately used the animated cube design below.

### 41. Scan icon and animation design iterations — 15:21–15:58

**Preview work, before app implementation**

| Revision | Change |
|---|---|
| Static concept | Mint scanner corners surrounding an isometric cube, with a scanning beam |
| First animation | Looping beam moving across the cube; animated preview exported |
| Refined v2 | Smoother edges, cleaner highlights, and 60 fps preview motion |
| Transparent v3 | Removed dark outlines and introduced real transparency while retaining the animation |
| Refined transparent v4 | Rebuilt rough cutout contours as smooth geometric curves, preserving the mint shading and transparent background |

GIF, MP4, transparent WebM, PNG, and SVG assets were produced during these iterations. The previews did not change the app until implementation was explicitly requested.

### 42. Animated Scan control installed — 16:10–16:27

**Added**

- Replaced the text Scan button with the approved transparent cube-and-scanner icon.
- Applied the control to inventory, room, and wardrobe scans.
- Animated the beam while that control’s scan was active.
- Clicking the active icon stopped its own scan.
- Added localized Scan/Stop tooltips and keyboard activation.
- Used cached native Tk/Pillow frames instead of embedding a video player or browser into the app.
- Shared artwork between controls while keeping their animation timers independent.
- Stopped animation work when a control was hidden, disabled, completed, or destroyed.
- Retained the full-resolution **2508 × 2508** transparent source asset in the package.

The first installed control used a **68 × 60 px** button area. Its height and active background were refined in entry 44.

### 43. One-time connection setup; normal-user Java — 16:30–17:00

**Added**

- **Set up connection / Configurar conexão** inside Pixelio.
- A bundled native **PixelioConnection.exe** helper installed after ordinary Windows administrator approval.
- A per-Windows-user routing service in a protected Program Files location.
- Normal Connect operations that reused the installed helper instead of requesting Java elevation again.
- Standard-user execution of Pixelio and the Java engine after setup.
- Local service status/start checks and clear setup failure/retry/cancellation handling.
- **Remove connection setup / Remover configuração** in Connections.
- Bundled service notices and offline self-checks that did not install the service themselves.

**Connection-service implementation**

- Limited privileged work to temporary hosts mappings for the ten official Habbo Classic game endpoints and loopback addresses.
- Used a restricted local named pipe and verified the service process on the client side.
- Preserved unrelated hosts-file contents and Windows file permissions during atomic replacement.
- Assigned routing to individual client leases so one client could not remove another client’s entries.
- Cleaned up owned routing after disconnect, client exit, missed heartbeats, service restart, or uninstall, with cleanup retries.
- Started the service at boot to clear stale owned routing; this did not automatically connect to Habbo.
- Left Windows UAC settings unchanged. Users did not need to configure Windows Settings manually.

**Verified live:** the service was installed and running; Pixelio connected and received inventory/price responses; Java and its bridge client were confirmed unelevated; temporary routes were removed and the hosts file matched its pre-setup contents.

Normal subsequent connections no longer required the recurring `javaw.exe` administrator prompt. Installing, repairing, replacing, or removing the privileged helper can still require Windows approval.

### 44. Matched Scan height, transparent states, and a stronger sweep — 17:02–17:10

**Changed**

- Resized the Scan control from **68 × 60 px** to **44 × 44 px**, matching Connect’s 44 px height.
- Cropped only empty padding from the source artwork so the visible icon filled the smaller control cleanly.
- Removed the filled panel behind the icon when hovered, focused, pressed, or scanning.
- Made the scan beam wider and brighter, with a mint glow and pale white center.
- Changed the animation loop from **3.0 seconds to 2.4 seconds**.
- Temporarily used a small focus underline in place of the filled focus background.

**Fixed**

- Corrected frame indexing on the return sweep: cached frames covered half the cycle, but the animation could request a frame beyond that range and stop.
- Added a regression check spanning multiple complete sweep cycles.

The updated Desktop launcher was installed after restart approval. The relevant 30-test run and all packaged checks passed, and saved data was preserved.

### 45. Removed the small bar after clicking Scan — 17:11–17:15

**Fixed**

- Identified the bar beneath the clicked icon as the focus underline introduced in the previous refinement.
- Removed that drawn underline and its unused color import.
- Kept the icon, transparency, animation, click-to-stop behavior, and keyboard actions.
- Rebuilt the launcher and installed the fix in the Desktop copy after restart approval.

The four Scan-control tests and three packaged checks passed. All 20 saved scans and the three stored wardrobe records were preserved across replacement.

### 46. Full historical changelog — 9 September 2026

**Added**

- This chronological history, beginning with the first inventory scanner request.
- Explicit records of experiments and designs that were later replaced.
- A release map separating local builds from the published release.
- Notes distinguishing implemented behavior from capabilities still requiring additional work.
- A copy beside the Desktop launcher and inclusion of the changelog in portable distributions.

### 47. Version 1.2.0 — GitHub release and published history — 9 September 2026

**Release**

- Consolidated the embedded connection engine, one-time Windows setup, connection fixes, and final animated Scan refinements from the local 1.2.0 builds into the public 1.2.0 release package.
- Added the full historical changelog to the GitHub repository and linked it prominently in the README and release notes.
- Updated the English and Portuguese download instructions for the built-in G-Earth engine, bundled Java runtime, and one-time setup workflow.
- Included the changelog in the portable ZIP alongside the launcher, instructions, and third-party notices.
- Supplied the standalone `Pixelio.exe`, portable ZIP, and checksum for downloads and startup updates from versions 1.1.0 and later.
- Kept application and Windows executable version metadata aligned at **1.2.0**; earlier builds using that number were local development builds.

This release retains the existing local data profile. Updating closes the built-in Habbo connection; users reconnect afterward. A local copy already reporting 1.2.0 will not offer another 1.2.0 update.

**Verified before publication:** 38 selected updater, launcher, connection, setup, and Scan-control tests passed. The rebuilt folder bundle, single executable, and relocated executable passed all three packaged checks using disposable data, including the embedded engine handshake and connection-helper verification.

[GitHub release and downloads](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.0).

### 48. Version 1.2.1 — Builders Club valuation exclusion — 9 September 2026

**Fixed**

- Recognize borrowed Builders Club floor and wall copies using Habbo's instance-ID marker, including room baselines and later furniture updates.
- Keep borrowed copies separate from ordinary owned copies of the same furniture type and owner.
- Skip their marketplace and LTD price requests, credit/BRL totals, NFT quotes, and missing-price counts.
- Keep these rows visible with a **Builders Club** badge and an explanation in English and Portuguese.
- Prevent Restore or custom prices from including borrowed copies in a valuation, including mixed selections and previously stored overrides.
- Preserve the exclusion in new saved room scans and CSV exports. Ordinary owned copies remain eligible for valuation.

**Validation:** 91 selected regression tests passed, covering floor/wall ID boundaries, incremental updates, shared price requests, owner filtering, credit/BRL/NFT totals, overrides, exports, and the room controls in both languages.

**Existing history:** room scans captured before this fix do not retain the individual item IDs needed to identify borrowed copies. Re-enter the room and scan it again with 1.2.1 to obtain a corrected valuation. Old historical snapshots remain unchanged.

**Release status:** installed in the Desktop copy after restart approval; not published to GitHub. All three packaged checks passed, and 26 saved scans plus three wardrobe records were preserved.

### 49. Version 1.2.2 — Marketplace availability and complete Builders Club omission — 9 September 2026

**Added**

- A visible **Not on marketplace / Fora da Feira Livre** counter, with furniture quantity and distinct type count, for the current inventory or selected room owners.
- Click the counter to show those rows; click again to return to all furniture. Unpriced, unlisted, non-marketable, and unknown furniture remain in the full list.
- Separate **Availability unknown** filtering and counts for requests still pending, failed lookups, and unresolved appraisals.
- Count confirmed zero-offer replies and furniture that cannot be listed without mistaking failed requests or incomplete LTD searches for missing listings.
- Keep availability separate from custom values and manual valuation exclusions. Unlisted and non-marketable furniture can still receive a custom value.
- Marketplace availability in CSV exports and compact labels on unavailable rows.

**Changed**

- Completely omit borrowed Builders Club copies from room baselines, incremental changes, scan rows, quantities, owner choices, exports, and pricing work. BC-only changes no longer invalidate an otherwise unchanged room scan.
- Hide identified BC rows when displaying a scan saved by 1.2.1, without rewriting historical records. This replaces 1.2.1's visible BC exclusion badges.
- Continue valuing ordinary owned copies of the same furniture types. Room scans from before 1.2.1 still need a fresh capture because their borrowed copies cannot be identified retrospectively.

**Validation:** 114 selected regressions passed across scanning, marketplace availability, Builders Club handling, saved-room views, valuation overrides, currencies, LTDs, wardrobe, localization, and progress. The new counter and labels were reviewed in an offline PT-BR preview at the minimum supported window size.

**Release status:** installed in the Desktop copy after restart approval; not published to GitHub. All three packaged checks passed, and 27 saved scans plus three wardrobe records were preserved.

### 50. Version 1.2.3 — Missing furniture in rooms sent in multiple batches — 9 September 2026

**Fixed**

- Identified a capture bug that predated the Builders Club filter: each new floor or wall furniture batch replaced the previous list, dropping whole groups of items in large rooms.
- Combine all `Objects` and `Items` batches within the current room by their individual furniture IDs, matching Habbo's own client behavior.
- Avoid duplicate quantities when batches overlap or repeat, preserve explicit item updates/removals, and clear accumulated items on room boundaries and disconnects.
- Enforce the supported item limit across the accumulated batches, not just each individual packet.
- Continue ignoring Builders Club copies and retaining unlisted furniture. All retained batches feed owner grouping, price requests, totals, saved scans, and exports.

**Evidence:** a temporary passive diagnostic observed two floor batches containing 2,379 and 1,184 items, followed by 495 wall items. The first floor batch contained 944 non-BC items, including Guitarra Quebradeira and Guitarra Banzai. The previous implementation discarded that batch. The full captured room contained 1,529 non-BC items. The diagnostic saved counts only, sent no game requests, and was stopped after capture.

**Validation:** 46 selected room, Builders Club, marketplace availability, history, and concurrent-scan tests passed. New regressions cover multiple floor/wall batches, duplicates, room re-entry, updates/removals, cumulative bounds, price queues, totals, saved scans, and CSV exports.

**Live verification after installation:** a fresh scan of room #149974221 captured 1,529 non-BC items across 545 owner/type rows, compared with 585 items across 232 rows before the fix. All 11 guitar types were present, including Guitarra Quebradeira and Guitarra Banzai, and no Builders Club rows remained. This capture check was completed while price requests were still running.

**Existing scans:** re-enter and scan the room again with 1.2.3. Older snapshots cannot recover furniture from discarded packets.

**Release status:** installed in the Desktop copy after restart approval; not published to GitHub. All three packaged checks passed. The installed executable matches the build's SHA-256, and 28 saved scans plus three wardrobe records were preserved.

### 51. Version 1.2.4 — Slow room price checks — 9 September 2026

**Diagnosed**

- The first complete large-room scan checked 545 furniture types and finished in 13 minutes 7 seconds. One price lookup required a retry; all 545 eventually received responses without parse errors.
- An automatic wardrobe valuation checked another 445 clothing types on the same request lane. That background job competed equally with the room scan.
- After the timeout, the lane retained its increased one-second gap for the rest of both scans, even after hundreds of successful replies.

**Changed**

- Automatically captured wardrobe valuations now yield while inventory or room scans are active, then resume with their existing progress. An outstanding request is allowed to finish before switching jobs.
- Explicit wardrobe refreshes retain fair scheduling alongside other requested scans; background captures do not demote a manually started valuation.
- Keep the 500 ms minimum and one outstanding marketplace request. After a timeout, reduce the slower interval by just 100 ms once at least 30 valid replies and 30 seconds have passed. Each recovery step starts a new observation period; another timeout resets recovery and increases the delay again.
- Apply recovery across the entire connection so an idle or cancelled job cannot leave every other scan permanently slowed. Keep bounded retries, the timeout cooldown, reply matching, and LTD checks.
- Show a translated waiting message for automatic wardrobe pricing and include the effective price interval in scan status data for diagnostics.

**Validation:** 135 selected tests passed across price pacing, background/manual scheduling, cancellation/disconnection, LTDs, wardrobe, room batches, Builders Club filtering, marketplace availability, history, accounts, progress, and localization.

**Release status:** installed in the Desktop copy after restart approval, with all three packaged checks passed. The installed executable matches the build's SHA-256, and 29 saved scans plus three wardrobe records were preserved. Not published to GitHub.

### 52. Version 1.2.5 — Repeating connection setup prompt — 9 September 2026

**Observed:** after installing 1.2.4, connection setup reported failure and returned to Set up connection. The helper executable and service registration were present and valid, but the service was stopped. Starting and verifying the existing service succeeded without elevation. The installer failure's underlying Windows error was not captured.

**Fixed**

- Recheck an existing installation and verify its service handshake before requesting another Windows setup prompt. A stale Set up connection button can now recover a working service without reinstalling it.
- If installation returns an error after registration, attempt the normal service start and verified handshake before reporting setup failure. An unhealthy or unverified service still fails; explicit setup can still repair it.
- Preserve cancellation and all existing helper integrity, service configuration, process identity, and authorization checks. No changes to the privileged service or its permissions.
- Retain the scan scheduling and gradual pacing recovery from 1.2.4.

**Validation:** 20 connection/setup tests passed, including stale setup state, failed installer exit with healthy/unhealthy service outcomes, repair, cancellation, and the bundled engine handshake.

**Release status:** installed in the Desktop copy after the user's installation request. All three packaged checks passed, the installed executable matches the build's SHA-256, and 29 saved scans plus three wardrobe records were preserved. Not published to GitHub.

### 53. Version 1.2.6 — Early estimates and a persistent price database — 11 September 2026

**Added**

- A local `market-cache.sqlite3` database shared by inventory, room, and wardrobe scans. The latest confirmed observations supply provisional totals as soon as the furniture list is captured.
- Each new scan still refreshes earlier prices. Matching observations received after a scan begins can serve concurrent scans of the same item without a duplicate request. Fresh lookups retain the 500 ms floor, timeout recovery, and one outstanding request per connection.
- Current and average estimate labels, saved-price provenance and timestamps, and progress coverage distinguishing fresh checks, saved observations, and unchecked items. Failed requests keep saved estimates explicitly provisional; confirmed absence of listings replaces the current value with unavailable rather than zero.
- Append-only successful price observations for future charts. Latest quotes are keyed by hotel and furniture identity, validated against the class name. Custom prices, ownership, exclusions, and Builders Club items never enter the shared market-price records.
- Exact LTD offer snapshots are saved separately from normal furniture statistics. Early LTD estimates recalculate the nearest listed serial for the units in the new scan, including exact series matching and limited-result disclosure.
- Successful NFT floor quotes and USD/BRL observations are also recorded for future history. Failed refreshes are not added as new prices; existing NFT/FX display caches remain supported.
- Valid ordinary price observations from existing saved inventory/room scans and wardrobe valuations are imported once. Old snapshots are unchanged. Historical LTD snapshots without the original offer set are not imported as reusable quotes.
- Existing icon/full-size image files are reused, with revision, URL, path, and checksum indexed in the database when accessed. Full-size previews are no longer deleted after 256 entries; in-memory image limits remain. Metadata changes invalidate in-memory icons so revised artwork is fetched when needed.
- CSV exports include price source and observation identity. Price database errors are reported without stopping live scanning or valid image display.

**Validation:** 169 tests passed covering persistence, early totals, shared responses, bounded retries, no-listing results, custom overrides, hotel/type/class isolation, LTD matching, append-only history, localization, images, and existing scanner behavior. Migration against a temporary copy of the user's data recovered 15,505 observations and 2,583 latest item quotes in 0.26 seconds, with database integrity verified and all original saved snapshots unchanged.

**Scope:** history data is collected now; charts and a shared online database are future work. Images are retained as original local PNG files with database references. Items never priced remain unknown until their first successful check; previously saved totals remain estimates until refreshed.

**Release status:** installed in the Desktop copy. All three packaged checks passed, including database persistence. The installed executable matches the build SHA-256. The new database contains 15,505 observations and 2,583 latest quotes; all 31 saved scans and three wardrobe records were preserved. Not published to GitHub.

### 54. Version 1.2.7 — Furniture tags, filters and list ordering — 11 September 2026

- Add an orange NFT identity tag alongside marketplace availability and manual valuation badges. All tags share the same rounded pill, height, padding and typography; the NFT tag uses a subtle tinted fill, muted border and bright orange text to match the existing badge style.
- Restore borrowed Builders Club rows at the user's request, with a gold BC tag in English and CA in Brazilian Portuguese. Owner selection and saved scans retain these rows; they remain excluded from credit/NFT valuations, price queries and reusable price observations.
- Clear stale BC prices from older saved views, disable custom valuation controls for BC-only selections and export BC rows with blank prices. Saved source snapshots remain unchanged.
- Add NFT and Builders Club / Clube do Arquiteto filters alongside Not on marketplace and existing filters. Marketplace absence counts continue to exclude borrowed BC copies.
- Order regular furniture first, then non-NFT furniture not on the marketplace, then NFTs, then BC/CA. The selected column/direction and owner ordering remain effective within each category.
- Give names, owners and multiple badges their own space; shorten long badge labels when the window is narrow.

**Scope:** a new room capture is required to recover BC copies omitted by earlier versions. Existing saved scans cannot reconstruct furniture they never retained.

**Validation:** 171 relevant tests passed, including NFT/BC filters in both languages, category ordering in both sort directions, borrowed-only rooms, stale saved prices, room batches, exports, price caching and concurrent scans. The offline layout was inspected with NFT, marketplace and CA badges.

**Release status:** the initial package was blocked by Windows rejecting the former renderer. These changes are included in 1.2.8 after the source rebuild and verification described below.

### 55. Version 1.2.8 — Windows installation and updates while open — 11 September 2026

- Add a per-user Windows installer with Start menu integration, an optional desktop shortcut, Installed Apps registration and an uninstaller. Start/taskbar pinning uses a stable app identity shared by the launcher and shortcuts.
- Install under LocalAppData/Programs/Pixelio so the existing in-app updater can replace the executable without elevating the app. Saved accounts, scans, preferences and cache remain in their existing profile location. Portable distribution remains supported.
- Check GitHub releases at launch and every ten minutes while open. Offline or failed checks retry later without duplicate workers. Later dismisses the current version for the session; a newer version can still appear.
- Show a non-modal new-version popup even during a scan. Download/restart requires the user's action, and active scans must finish or stop before installation. Cancelled download events cannot affect a later popup.
- Keep installed-version metadata current after in-app updates. A portable copy cannot overwrite the installed application's metadata. Installer/uninstaller detect the new app's running mutex and request that it close instead of terminating scans.
- Installer compilation requires a matching successful packaged-check manifest and executable hash. Full build checks remain mandatory. The compiler was downloaded from the official project and its publisher signature verified.

**Validation:** 37 recurring notification, cancellation, scan protection, update integrity, launcher, Windows app identity, installer validation and localization regressions passed. An isolated installer test with inert files and a separate identity verified Start shortcut/AppUserModelID, Installed Apps registration, uninstall and preservation of unrelated files. The inert test package is not a Pixelio release.

**Renderer resolution:** Windows identified the former renderer as Wacatac.C!ml and quarantined it without execution. Rebuilt from reviewed source, a checksum-verified official Go 1.27.1 toolchain and freshly verified pinned dependencies. Added limits on actual SWF decompression and graceful malformed-artwork errors; retained build metadata. The replacement passed an updated Defender custom scan with real-time protection enabled. This does not establish why the previous binary was classified. No quarantined file was restored and no exclusions were added.

**Validation before packaging:** 197 application regressions and three native renderer tests passed. Real furniture samples (floor, colour variant and wall guitar) rendered full-size PNGs with transparency.

**Release verification:** all three packaged application checks passed. The rebuilt renderer and complete release passed Defender custom scans with protection enabled. The packaged updater completed verification, replacement and restart in a disposable profile, retaining its data and rollback file. The Windows installer then installed 1.2.8 with Start/Desktop shortcuts and preserved all 35 saved scans and three wardrobe records. Installer and portable downloads were published as v1.2.8 on 11 September 2026. Every GitHub asset digest matched the verified local file. The real update client detects 1.2.8 from an older version and correctly returns no update when already running 1.2.8.

### 56. Version 1.2.9 — Exclusive NFT badge — 11 September 2026

- NFT furniture displays only its orange NFT badge. Marketplace absence, custom
  pricing and exclusion badges no longer appear alongside it.
- Marketplace availability, filters, sorting and valuation rules remain intact;
  this change affects the visible badges only. BC/CA identity remains separate.

**Validation:** 15 badge, marketplace-availability and Builders Club regression tests passed. The Windows package passed its bundled self-check. Installer and portable packages were published as v1.2.9 on 11 September 2026. All five GitHub asset hashes and sizes matched the local release. The actual update client detects 1.2.9 from 1.2.8 and returns no update when already on 1.2.9.

### 57. Version 1.2.10 — Simplify interface guidance — 11 September 2026

- Remove the Marketplace / Custom / Excluded colour legend from the furniture-list footer in both languages and the inventory/room views.
- Retain the row and selection counts in the footer.
- Remove repeated wardrobe capture explanations, selection instructions, custom-rate summaries and generic valuation notes. Keep one concise consumed-clothing warning.
- Show inspector guidance only for missing prices, borrowed BC/CA items, invalid input or unapplied changes. Avoid repeating BC explanations in the same inspector.
- Keep the BRL conversion rate once; omit zero-count missing-price and exclusion notices, and the redundant custom-price card description.
- Keep a single scan progress header and remaining count. Display the extra price-cache caption only when saved estimates or a cache warning are present, retaining estimate age.
- Preserve icon-only Scan tooltips, connection/setup guidance, incomplete-scan warnings, NFT/FX freshness and valuation controls in both languages.
- Replace the obsolete wardrobe instruction to open G-Earth with a concise instruction to connect through Pixelio.

**Validation:** 59 relevant regressions and all three packaged checks passed. Room and wardrobe layouts were inspected in an isolated native preview. Published as v1.2.10 on 11 September 2026. All five GitHub asset hashes and sizes matched the verified local package. The real update client detects 1.2.10 from 1.2.9 and returns no update when already on 1.2.10.

### 58. Version 1.2.11 — Compact dashboard captions — 11 September 2026

- Remove the top furniture/account subtitle, generic connect-to-scan instruction, estimates/before-fees note, separate partial-scan caption and permanent saved-scan/excluded-count footer.
- Remove the space reserved for these captions. Preserve scan progress, connection controls and contextual setup/error messages.
- Move total furniture quantity and type count into the Items included card caption: “x mobis · x tipos de mobis” in PT-BR, with the ENG equivalent. The large included-item value remains unchanged; counts follow the selected room owners.
- Retain action feedback, such as export confirmation, only when there is a message to display.
- Shorten the average-value card title to “Média” in PT-BR and “Average” in ENG.

**Validation:** 45 relevant regressions and all three packaged checks passed. Published as v1.2.11 on 11 September 2026. All five GitHub asset hashes and sizes matched the verified local package. The real update client detects 1.2.11 from 1.2.10 and returns no update when already on 1.2.11.

### 59. Version 1.2.12 — Matching summary-card icons — 11 September 2026

- Replace the four summary-card icons with the approved Pixelio set: credit stack, average-price bars, checked box and adjustment sliders.
- Use consistent mint shapes, gold accents and transparent backgrounds. Downsample the 256-pixel assets to matching 56-pixel slots with smooth edges.
- Include editable SVG masters and validate all four transparent PNGs in packaged checks. Inventory and room cards share the same icons.

**Validation:** all three packaged checks passed, including verification of all four transparent icons. The native summary cards were visually reviewed. Published as v1.2.12 on 11 September 2026. All five GitHub asset hashes and sizes matched the verified local package. The real update client detects 1.2.12 from 1.2.11 and returns no update when already on 1.2.12.

### 60. Version 1.2.13 — Approved Habbo-inspired icon set — 11 September 2026

- Replace the mint summary icons with the user's chosen pixel-art set: gold credit sack, gold bars with a mint average marker, furniture box with teal chair and check badge, and beige price tag with yellow pencil.
- Preserve the approved artwork and black outlines. Extract real transparent backgrounds directly from the selected source rather than using the image generator's simulated checkerboard output.
- Preserve the original sprite proportions with 48-pixel icons in compact cards and 56-pixel icons in wider cards. Keep full-resolution transparent masters alongside the approved source in the development workspace.
- Give all four cards consistent inner padding, a 16-pixel icon-to-text gap, aligned titles and values, and more vertical space for supporting text. Adapt long amounts to the available width and wrap supporting text within each card.
- Update packaged checks to validate the four new RGBA assets.

**Validation:** all three packaged checks passed, including verification of the four RGBA icons. Cutouts were inspected on light and dark backgrounds, and the native cards were reviewed in compact and maximized windows. Published as v1.2.13 on 11 September 2026. All five GitHub asset hashes and sizes matched the verified local package. The real update client detects 1.2.13 from 1.2.12 and returns no update when already on 1.2.13.

### 61. Version 1.2.14 — Integrated window header — 11 September 2026

- Integrate minimize, maximize/restore and close into Pixelio's navigation header, with equal button widths and centered control icons.
- Match the remaining Windows frame to the header background on Windows 11, removing the contrasting strip along the top edge.
- Preserve native resizing, taskbar minimize/restore and the system menu. The logo and unused header space can move the window; the close control follows the existing app cleanup path.
- Apply the approved spacious header: 82-pixel height, a larger compact logo, increased logo padding and a wider gap before the tabs. Keep navigation and window controls centered, with adaptive widths for the minimum window size.

**Validation:** native Windows integration checks passed for resize limits, minimize, maximize, restore and close handling. The compact and maximized header and frame colour were reviewed in an isolated preview. All three packaged checks passed and the installer checksum was verified. Published as v1.2.14 on 11 September 2026. All five GitHub asset hashes and sizes matched the tested local package. The real update client detects 1.2.14 from 1.2.13 and returns no update when already on 1.2.14.

### 62. Version 1.2.15 — Coherent dropdown menus — 11 September 2026

- Replace the seven native combobox locations with a shared Pixelio dropdown: account selection, saved scans, room owners, furniture filters, wardrobe filters, connection ports and scan assignment.
- Use dark fields and menus, rounded borders, mint selection with a checkmark, matching chevrons and consistently spaced options.
- Name the PT-BR Builders Club filter "Clube do Arquiteto", without the redundant "(CA)" suffix.
- Match every menu to its field width and align both edges vertically, without expanding sideways for long labels. Keep menus inside their window, opening above the field when needed, with scrolling for long lists. Widen the furniture filter for readable labels.
- Preserve selection events, translated options, editable connection ports, keyboard navigation, type-to-find, Escape cancellation and modal-dialog focus/grab restoration.

**Validation:** 22 targeted tests passed, covering dropdown interactions, room history, furniture tags and localization. The native menu and NFT filter selection were visually checked. All three packaged checks passed for the final build, including the aligned menu widths and PT-BR filter-label refinement. The installer checksum was verified. Published as v1.2.15 on 11 September 2026. All five GitHub asset hashes and sizes matched the final tested package. The real update client detects 1.2.15 from 1.2.14 and returns no update when already on 1.2.15.

### 63. Version 1.2.16 — Animated navigation tabs — 11 September 2026

- Implement the approved tab preview in the native app: one mint underline slides between tabs over 280 ms, with a 180 ms text-colour transition.
- Keep tab content responsive; rapid selections retarget from the current indicator position. Initial layout and header resizing place the underline immediately at the correct tab bounds.
- Respect the Windows client-area animation preference and cancel animation callbacks when navigation is hidden or closed.
- Preserve the spacious header, existing page behaviour, language switching and keyboard activation.

**Validation:** 28 targeted native UI and regression tests passed, including six animation tests covering interrupted motion, resize alignment, translations, reduced motion and cleanup. All three packaged checks passed (folder build, standalone executable, and relocated executable in a path containing spaces and accents). The installer and executable checksums were verified. This version has not been published or installed.

### 64. Version 1.3.0 — Integrated clothing Try-On — 11 September 2026

- Keep the existing wardrobe scanning, valuation, search and exports under My clothing / Minhas roupas. Add Try-On / Experimentar in the same Looks / Visuais tab.
- Add a category sidebar, searchable paged clothing grid, body types, multiple colour slots, rotation, optional-piece removal and a large real Habbo avatar preview. Use Pixelio's shared controls and English/Brazilian Portuguese translations.
- Parse official figure definitions and palettes; map clothing products and unlocked sets to wearable categories. Handle multi-part products, gender alternatives, required parts, hidden layers and individual variants from bundled products.
- Distinguish owned, standard, HC, restricted, unowned and unknown-ownership clothing. Ownership uses the latest captured unlocked wardrobe and never treats a try-on selection as ownership.
- Load the account's public current appearance when available, retaining an account-specific cached fallback and a usable default. Name, save, load and delete local outfits per account. Keep unsaved drafts isolated and preserve outfits when assigning legacy scans.
- Keep public metadata, profile and artwork requests on background workers with caching, bounded queues, selection debouncing and stale-response rejection across account changes. Do not send appearance-change packets to Habbo.

**Validation:** 106 focused tests passed across Try-On, wardrobe, clothing valuation, accounts, scanner/concurrency, translations, dropdowns and tab navigation. Composition was exercised against current official data containing 3,267 sets and 1,966 fully mapped products in both body types. Real artwork was inspected in integrated native previews at 1536×970 and 1180×790. All three packaged checks passed (folder build, standalone executable, and relocated executable). Installer and executable checksums were verified. Published as v1.3.0 on 11 September 2026. All five uploaded asset hashes and sizes match the tested local build. The actual update client detects 1.3.0 from 1.2.15, verifies the executable digest, and returns no update when already on 1.3.0. Not installed during publication.

## Current behavior and unfinished capabilities

| Area | Current state |
|---|---|
| Inventory, room, and wardrobe valuation | Implemented, with separate results and account data |
| Multiple valuation jobs | Requested scans share the per-connection 500 ms minimum gap; automatic wardrobe pricing yields while room/inventory scans are active |
| Room owner selection | Implemented; defaults to the connected account’s furniture |
| Large room capture | 1.2.3 combines all floor/wall furniture batches; older incomplete snapshots need a fresh room scan |
| Builders Club room copies | Visible with BC/CA tags and a filter in 1.2.7; always unvalued; scans that omitted them require a fresh room capture |
| Furniture without marketplace listings | Visible with quantity/type counts and a dedicated filter; unresolved availability is counted separately |
| Saved room history | Implemented; reopens saved valuations, not an automatic room-travel feature |
| Credit display | Whole credits; precise rates/calculations/exports retained |
| Average vs. median | Reported average is implemented; a true individual-sale median is not |
| LTD nearest serial | Implemented within the returned matching listing set; limited result sets are identified |
| Credit → BRL | Fixed user rate of 1,000c = R$200 |
| NFT and USD/BRL quotes | Implemented for supported products, with coverage and freshness; unsupported/unlisted products remain unpriced |
| Fresh complete wardrobe after login | Still depends on receiving Habbo’s unlocked list; Scan can reuse and reprice captured data |
| Historical outfit usage | Not implemented; available wardrobe data does not provide a wear-history timeline |
| NFT avatars | Captured separately; values are not fabricated from clothing prices |
| Embedded connection | Implemented for the supported Habbo Classic AIR workflow; external G-Earth connections remain available |
| Repeated Java UAC popup | Replaced by one-time in-app connection setup; privileged setup changes may still prompt |
| Windows compatibility | Installer and portable Windows 10/11 x64 builds; separate x86/ARM64 builds and independent clean-PC verification are not established here |
| Public updates | 1.2.8 includes the installer and recurring checks while open; earlier updater-enabled versions receive it when next opened |
| Current Scan control | 44 px high, transparent, brighter animated sweep, no click-induced underline |

## Verification history

These are historical checkpoints reported during development, not a claim that every test suite was rerun for this documentation change. Counts refer to the selected suites at each checkpoint and are not a monotonically increasing project-wide total.

| Checkpoint | Recorded verification |
|---|---|
| First scanner | 59 checks; complete initial inventory received |
| Icons and adjustments / refined UI | 65 checks and UI review |
| Account support and late attachment | 75 → 82 → 88 checks as identity, missing-name, and refresh fixes were added |
| Artwork and localization | 94 checks after visual fixes; 103 after language support |
| Rooms, named history, scrollbars | 120 → 124 → 127 checks |
| Price pacing repair | 30/30 live replies at restored 600 ms; 60/60 at 500 ms; 158 checks |
| Progress / concurrent scans / LTDs | 161 checks for progress; 173 after concurrency, rounding, and LTD work; live LTD lookup |
| BRL / NFT quotes / wardrobe capture | 182 checks after currencies/NFT work; 189 after initial wardrobe support |
| Portable releases | Folder bundle, one-file bundle, and relocated one-file checks with temporary data |
| Published 1.1.1 update | Real popup/download/restart; version and saved records verified |
| Embedded engine and Scan restoration | 118 relevant checks; three packaged checks; complete live inventory and price replies |
| One-time service setup | 70 selected Python regressions, five native service tests, packaged checks, and live unelevated Java/connection verification |
| Scan size/transparency/sweep refinement | 30 relevant checks plus packaged checks and a native preview |
| Scan underline removal | Four Scan-control tests, three packaged checks, installed-binary verification, and saved-data comparison |
| Public 1.2.0 package | 38 selected release regressions and all three rebuilt packaged checks with disposable data |
| Builders Club exclusion in 1.2.1 | 91 selected room, valuation, currency, LTD, scanner, language, concurrency, and history regressions |
| Marketplace availability and BC omission in 1.2.2 | 114 selected regressions plus an offline minimum-size PT-BR layout review |
| Multiple room batches in 1.2.3 | 46 selected regressions and three packaged checks; installed live scan confirmed 1,529 non-BC items and all 11 guitar types |
| Slow scans in 1.2.4 | 135 selected tests; automatic wardrobe priority and gradual recovery from timeout delays |

## Evidence and future entries

The main chronology comes from the retained **Add furni inventory value scanner** development conversation, starting on 8 September 2026 at 13:25 BST. The separate **Create Pixelio taskbar icon** conversation supplies the additional artwork-deliverable entry.

Implementation details were checked against these development files and their tests:

| Area | Evidence in the development checkout |
|---|---|
| Original scanner and valuation | `inventory_model.py`, `inventory_scanner.py`, `inventory_valuation.py`, `INVENTORY_SCANNER.md` |
| Dashboard and controls | `inventory_app.py`, `inventory_ui.py`, `inventory_artwork.py`, `inventory_scrollbar.py` |
| Account isolation and history | `inventory_accounts.py`, `inventory_connections.py`, `inventory_accounts_ui.py` |
| Icons and previews | `inventory_icons.py`, `inventory_previews.py`, `inventory_renderer/`, `assets/inventory/README.md` |
| Translation | `inventory_i18n.py` |
| Room capture and history | `room_inventory.py`, `room_scanner.py`, `room_inventory_ui.py`, `room_history_ui.py` |
| Pacing, concurrency, progress, and LTDs | `inventory_concurrent.py`, `inventory_progress.py`, `inventory_ltd.py`, earlier copies under `work/price-*` |
| BRL, NFT, and FX | `inventory_money.py`, `inventory_money_ui.py` |
| Wardrobe and clothing prices | `inventory_wardrobe.py`, `inventory_wardrobe_ui.py`, `inventory_clothing_values.py` |
| Branding and animation | `assets/pixelio/`, `outputs/scan-icon-animation/`, `inventory_scan_ui.py` |
| Portable builds and updates | `pixelio_launcher.py`, `pixelio_release.py`, `pixelio_updates.py`, `pixelio_update_installer.py`, `pixelio_distribution/` |
| Embedded connection and setup | `pixelio_connection.py`, `pixelio_connection_ui.py`, `pixelio_setup.py`, `pixelio_engine/`, `pixelio_service/` |
| Release numbers | Retained local `BUILD-INFO.json` manifests for 1.0.0, 1.0.1, 1.1.0, 1.1.1, and successive 1.2.0 builds |
| Public publication | [GitHub release list](https://github.com/CarlosPlacinta/Pixelio/releases), [v1.1.1](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.1.1), and [v1.2.0](https://github.com/CarlosPlacinta/Pixelio/releases/tag/v1.2.0) release records |

Some older manuals retain instructions from earlier stages, such as opening external G-Earth or authorizing Java. The later entries above describe the current implementation. Files named here refer to the development checkout; the portable distribution does not include all source or the development conversation.

For future entries, append the date, actual version/build, concrete additions or fixes, relevant validation, and publication status. Keep local fixes distinct when a version number is reused, and mark reverted experiments or preview-only work explicitly. This changelog is a maintained document; it is not an automatic change recorder.



















## 11 September 2026 — 1.3.1 Try-On refinements

- Replace clothing pagination with continuous vertical scrolling and thumbnails loaded as they enter view. Preserve scroll position when trying on a piece.
- Display and apply only the relevant part of multi-category products. Visual Utopia Solar exposes its hair, top and necklace separately, preserving unrelated outfit selections.
- Keep the current avatar visible while an uncached preview loads. Show memory-cached previews immediately, preload adjacent viewing angles and give avatar requests a dedicated background worker.
- Render clothing-card borders with Pixelio's antialiased artwork instead of smoothed Tk polygons.
- Validation: 25 focused wardrobe/Try-On tests passed; 5,952 cached clothing/category entries checked; card borders inspected at 1400×820 and 1180×790.
## 11 September 2026 — 1.3.2 Colour palettes and clothing icons

- Display Colour 1 on the left and Colour 2 on the right, separated by a centre divider. Each palette scrolls independently and updates only its corresponding colour.
- Disable the second palette for single-colour pieces, retain fixed-colour messaging, and show any additional supported colour slots below.
- Retain official clothing-product icons for split products, linked variants and unlocked sets across all 15 categories. Applying a piece still changes only its relevant category. Sets without a known product retain the rendered fallback.
- Validation: 30 focused wardrobe/Try-On tests passed; 4,879 product-linked entries checked across 15 categories; official Alice hairstyle icon and both colour palettes visually checked at 1400×820 and 1180×790.
## 11 September 2026 — 1.3.3 Category-focused fallback thumbnails

- Use head-only previews for standard faces and other head categories without clothing-product icons, instead of full-body mannequins in the grid.
- Focus other fallback thumbnails on the relevant body region across the remaining categories. Official clothing-product icons remain unchanged.
- Keep processed thumbnail cache entries separate from full-avatar previews so older mannequin images cannot be reused as the new thumbnails.
- Validation: 34 focused wardrobe/Try-On tests passed, with public Habbo fallback artwork rendered for all 15 categories and representative images visually checked.
## 11 September 2026 — 1.3.4 Visual wardrobe navigation

- Group the category menu into Head, Upper body, Lower body, Accessories, Feet and Pets. Head includes glasses and head/face accessories. Each group remembers its last selection and remains scrollable in shorter windows.
- Replace all 15 category icons with the approved Habbo-inspired artwork and real transparent PNG backgrounds, displayed proportionally at 44 pixels. Preserve the exact approved Rostos face.
- Label companion pieces as Pets, with the approved paw-and-sparkle icon. Label handheld props as Accessories, with the cushion-and-cane icon.
- Bundle all icons in the Windows executable and verify their alpha channels and the approved face in the packaged installation check.


## 11 September 2026 — 1.3.5 Extras and lower-body categories

- Rename the general accessories group and category to Extras in both Portuguese and English.
- Use three evenly spaced gold pixel squares with refined shading, crisp outlines and a transparent background for Extras.
- Move Shoes / Sapatos under Lower body / Pernas alongside bottoms and belts. Remember the selected shoes category when reopening the group.

## 11 September 2026 — 1.3.6 Habbo Club detection

- Check HC after account identification and refresh while connected, using the read-only ScrGetUserInfo request and server membership responses.
- Silently show HC-only clothing and colours only for a connected account with fresh confirmed membership. No HC status text or check button; standard HC clothing is included in Owned clothing.
- Keep accounts and connection generations isolated. Do not infer current HC from clothes, badges, historical membership or VIP history.
- Handle expiry, stale observations, malformed responses, missing mappings and timeouts without falsely reporting no HC.
- Rebuild the embedded connection engine to allow only the additional named read request.
- Validate protocol parsing, account isolation, background refresh, clothing and colour filtering. Live Habbo response validation remains pending because no game session was connected.

## 11 September 2026 — 1.3.7 Native wardrobe sprites

- Render every Try-On category from original Habbo clothing layers, including standard and product-linked pieces.
- Preserve the wardrobe’s sprite direction, layer order, registration offsets and colour channels.
- Centre complete artwork at native scale, replacing clipped and enlarged avatar crops and furniture product icons.
- Keep faces head-only and retain all companion and Extras layers.
- Cache sprite libraries independently from old thumbnails; preserve silent HC availability checks.
- Validation: live public artwork from all 15 categories and the seven reported shoe sets, 48 affected Python tests and the Go renderer tests.

## 11 September 2026 - 1.3.8 Clothing hover animation

- Fade clothing tiles to a mint highlight and lift their original sprites by three pixels over 150 ms on hover.
- Preserve native sprite scale, selected outlines and scroll position. Refresh hover targeting after scrolling or rebuilding the grid.
- Respect Windows reduced-motion settings and cancel animation timers when the grid is hidden, rebuilt or closed.
- Validation: 23 existing regression tests and a native Tk smoke check of hover, leave, reduced motion, redraw and cleanup.

## 11 September 2026 - 1.3.9 Clothing badges, names and offline HC

- Add the approved transparent Habbo-inspired NFT marker, crowned HC badge and red unowned exclamation mark inside Try-On clothing tiles. Keep badges above loaded and animated sprites.
- Show the original item name without category prefixes or generated variant prefixes. Remove the redundant Unowned / Não possui caption when the red badge is present.
- Keep server-confirmed HC clothing and both colour palettes available offline until the recorded subscription expiry. Recheck membership on reconnect, honour live revocations, and keep saved membership isolated by account.
- Retain confirmed subscription time through delayed refreshes and retry failed background checks sooner.
- Validation: 46 wardrobe, sprite, badge and HC regression tests passed, including offline expiry, account isolation and both colour palettes.

## 11 September 2026 - 1.3.10 Responsive wardrobe and clothing order

- Replace the body-type dropdown with male and female icon buttons using Habbo artwork. Keep the active selection synchronized with resets, account changes and saved looks.
- Preserve loaded tiles and scroll position when selecting clothing. Update selection highlighting without rebuilding the grid.
- Apply the selected colours to compatible clothing thumbnails, preserving fixed-colour details and both colour channels. Reuse cached layers and replace existing sprites only when the new render is ready; discard stale colour responses.
- Order clothing as HC, regular, unlockable products, then NFT. Sort unlockable and NFT products newest first using numeric item IDs as approximate addition order, because Habbo furnidata does not provide release dates. Ownership no longer overrides this ordering.
- Validation: all 51 affected wardrobe, sprite, gender-button, ordering and HC regression tests passed.
- Release verification also exposed intermittent Windows access-denied errors when renaming the extracted connection runtime. Retry only Windows access-denied/sharing locks with a bounded backoff, preserving archive and file integrity checks and refusing to overwrite an existing destination. All 23 connection/setup regression checks passed.

## 11 September 2026 - 1.3.11 Owned clothing first

- Keep owned and standard clothing together before all unowned or unconfirmed pieces in Try-On.
- Preserve HC, regular, unlockable clothing, then NFT ordering within each ownership block. Unlockable and NFT pieces remain newest first using approximate item addition order.
- Apply the same order to both gender options and search results; retain the Owned clothing filter.
- Validation: all 37 wardrobe composition and browsing regression tests passed.
## 11 September 2026 - 1.3.12 Avatar trait badges and connected HC recovery

- Identify 107 verified NFT Avatar figure traits across 11 categories with the original cyan Habbo Pixels cube. Retain the regular NFT marker for product-linked clothing, and sort Avatar traits with NFTs within their ownership block.
- Keep authenticated HC login rights valid throughout their live connection instead of expiring them after five minutes. Continue to respect explicit subscription expiry, newer revocations, disconnects and account isolation.
- Accept both habbo_club and club_habbo subscription response identifiers, as the official Habbo client does.
- Validation: 41 wardrobe and badge checks passed; 48 affected HC, concurrent scanner, wardrobe UI and badge checks passed after the HC fix.

## 11 September 2026 — 1.3.13 performance improvements

Reduced repeated ownership lookups and redundant wardrobe selection redraws. Added bounded colour-table reuse for native sprite recolouring and a single-pass marketplace availability summary. Measurements and before/after parity checks are recorded in PERFORMANCE.md. Updated an obsolete room subtitle test to cover the current localized owner selector. Component benchmarks reduced catalogue processing time by 87%, selection redraw time by 92%, sprite recolouring time by 14%, and marketplace counting time by 23%. The marketplace benchmark also reduced peak temporary allocation by 82%. These are individual-operation measurements, not overall application speed or memory claims. Validation covered 158 regression tests, 1,200 catalogue comparisons, 512 identical native sprite PNGs, and 201 inventory summary comparisons.

## 11 September 2026 — 1.3.14 fast-scroll image scheduling

Fixed newly visible wardrobe tiles waiting behind requests for tiles far offscreen. Visible artwork now gets priority, obsolete queued work and stale results are removed, and nearby rows are prefetched within a bounded queue. Loaded sprites, colour changes, hover effects, and background account work are preserved. First-time artwork downloads can still show loading placeholders.

## 11 September 2026 — Sprite loading and cache corrections (1.3.15)

Removed the global lock around sprite library downloads and extraction, allowing unrelated loads to proceed independently. Display cached tile sprites directly instead of waiting for worker slots. Increased the library cache file allowance from 128 to 4,096 while retaining the 64 MB storage limit, and kept cache updates safe during concurrent loads. First-time uncached artwork still needs downloading and extraction.

## 11 September 2026 — LTD clothing badge (1.3.15)

Recognize the official `clothing_ltdYY_...` product-code family and show the approved compact silver Habbo-inspired LTD badge on its clothing tiles, including split outfit pieces and linked variants. Avoid false positives on LTD-logo merchandise or generic rarity/name fields. Traje Mulher Barbada (`clothing_ltd24_ladybeard`) includes face accessory 5513 and top variants 5514/5515. The current figure catalogue maps 13 LTD products to 48 tiles across both body types. This identifies LTD product origin, not a consumed item's serial number; ownership, NFT/HC badges, and ordering remain unchanged. Detection, badge rendering, and badge layering are covered by tests.

## 11 September 2026 — Sharper HC tile badge (1.3.15)

Keep the approved crowned HC artwork and 23-pixel size, but use nearest-neighbour scaling for the HC status badge to avoid blurred pixel edges from Lanczos smoothing. Other badge rendering is unchanged. Before/after previews were checked at native tile size.

## 11 September 2026 — Aligned gender selector (1.3.15)

Center the visible male and female symbols in equal 24-by-28-pixel slots, ignoring unequal transparent source padding. Preserve every source pixel, translated labels, and gender switching. Verified with a native Tk preview and the existing gender selector regression check.

## 11 September 2026 — Persistent clothing thumbnails and improved HC badge (1.3.16)

- Save finished clothing thumbnails across launches and restore them during background catalogue loading. Retain up to 4,096 thumbnails within a 16 MiB PNG budget; preserve asset, layer and colour-specific identities.
- Use spare queue slots to prepare remaining clothing in the selected category, keeping visible and nearby tiles first. Avoid allocating offscreen Tk images during this preparation.
- Replace the HC badge with the approved simpler crown and bolder HC lettering, without floating sparkles. Preserve the approved 23-pixel tile rendering with Lanczos sampling; nearest-neighbour reduction in 1.3.15 lost crown and letter detail.
- Validation: 76 affected checks passed. In 256 real cached clothing cases, all PNGs matched and all thumbnails survived a simulated restart; the previous renderer retained 128. Cache timings are documented in PERFORMANCE.md. First-time uncached artwork still needs downloading and extraction.

## 11 September 2026 — Earlier wardrobe preparation and visible-row rendering (unreleased)

- Prepare up to 512 owned and standard clothing pieces in the background for the selected connected account before Try-On opens. Share ready catalogue and sprite caches with the view; respect HC availability and pause during active scans or foreground Try-On use.
- Create tile surfaces, labels, badges and Tk photos only for visible/nearby rows. Release offscreen UI resources while keeping the full ordered clothing list and persistent artwork cache.
- Preserve scrolling, selection, hover, colours, badge layering, saved looks and account isolation. First-time uncached downloads remain dependent on public asset availability.
- Validation: 107 affected checks passed. A 2,000-item cached Tk benchmark reduced grid drawing from about 142 ms to 29 ms and repeated scrolling from 34 ms to 24 ms, with unchanged viewport content/geometry. Details are in PERFORMANCE.md.

## 11 September 2026 — Responsive wardrobe colours and foreground loading (unreleased)

- Reuse decoded native sprite layers for colour changes within a bounded cache, preserving exact artwork output.
- Reserve one existing clothing worker for foreground work and preload small eligible batches from neighboring categories after visible tiles settle.
- Add bounded local timing diagnostics for opening, scrolling, categories, gender and colours, with background atomic writes and no account or item identifiers.
- Validation: 112 affected checks passed. A controlled 320-recolour profile improved from 236.61 ms to 166.76 ms with identical PNGs; cached grid content and geometry also matched. See PERFORMANCE.md for scope and limitations. No build or publication performed.
