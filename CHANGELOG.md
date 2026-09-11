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

**Validation:** native Windows integration checks passed for resize limits, minimize, maximize, restore and close handling. The compact and maximized header and frame colour were reviewed in an isolated preview. All three packaged checks passed and the installer checksum was verified. Installer ready locally; not published or installed.

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


















