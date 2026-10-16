# Pixelio Wired and Watch & Walk

## Wired catalogue

Open **Wired → Wired catalogue / Catálogo de Wireds**. It lists every Wired in Habbo's furni list (furnidata). The scanner downloads that list on every scan, and the catalogue re-reads it whenever the file changes, so new Wireds appear without any other source.

- **Kinds** come from Habbo's code prefixes: `wf_trg_` Trigger, `wf_act_` Effect, `wf_cnd_` Condition, `wf_slc_` Selector, `wf_xtra_` Extra, `wf_var_` Variable. `wf_test_`, `wf_proto_` and `test_` are unreleased **Prototypes**. Other Wired-line furni (buttons, counters, tiles) are **Other**.
- **Variable family:** the **Variable** filter also lists the extras made for variables (`wf_xtra_` codes with `var`: Extra Variável, Variável FX, text capture and field, filters by variable). Their kind stays Extra; the detail panel says so. Habbo's lists carry 10 variable boxes and 17 such extras.
- **Search** ignores accents, capitals and word order, like every other search in Pixelio: "variavel" finds "Variável", "pontos saude" finds "Pontos de Saúde". It matches the code, name, furni ID and kind.
- **New:** the first check records the current list without marking anything. Codes that appear in later lists are marked **★ New** with their first-seen date until **Mark all as seen**. The state lives in `wired-catalogue-v1.json` in the data folder.
- **Names:** placeholder text (`<code> name`, `<code> desc`) counts as not written yet. The English list from Try-On (`try-on-cache/furnidata-en.json`) is used when available; otherwise the row says **Not named yet**.
- **Export catalogue** saves code, furni ID, kind, name, description, new mark and first-seen date as JSON.
- **Picture and animation:** the selected Wired's picture shows beside the list and plays its switch-on animation once loaded. Clicking the row again or the picture replays it. The renderer's `--activation` mode picks the animation that transitions into state 1 (100 on Wired boxes), else state 1, and frames play at 24 fps. Artwork is downloaded on first view and cached in `wired-animations/` in the data folder.

The catalogue only says which Wireds exist. What each one does, and how they combine, isn't in Habbo's furni list.

## Ordinary floor-click timing — Pixelio 1.3.35

**Wired → Calibrar chegada → Medir cliques normais** starts passive recording of your regular floor clicks. It does not require a furniture selection or send any walks. Click while the avatar is stationary, then wait for the walk to finish before clicking again. Distant tiles are supported: timing stops at the first own-avatar walking update, not arrival at the distant tile. Include at least five walks whose first step is straight. Aim for at least eight valid click responses. The status shows the last result and why unsuitable clicks were excluded.

Starting Measure arrivals also starts this collector. Its Continuous learning tab displays the native click latency. Recording can continue while the avatar walks normally; room/account/connection changes reset it. Pause click measurement stops this collector. Pausing or closing Measure arrivals also pauses it. Closing calibration stops its automatic walk trial but retains passive click recording until paused or the connection changes. The latest 120 request rows are retained locally and can be exported.

To use the measurements for timing:

1. Learn the same trigger and moving furniture's three repeatable movements in Calibrar chegada, as before. The avatar must start one straight step from the target.
2. Select **Usar latência dos cliques para estimar o atraso da tentativa**. The preview shows the candidate and the observed range. If samples are insufficient or stale, collect more normal floor clicks; this can happen while resetting your position between trials.
3. Reset the pillow and return to the learned starting tile. Click **Armar próxima tentativa**, then use the selected trigger once. Pixelio sends one walk using the frozen candidate. Confirm **Normal**, **Bloqueou / interrompeu** or **Inconclusivo** afterward.

The equation is:

`delay after Use = median Use→furniture lag + furniture animation − median first straight-step duration − estimated request→first walking update`

The response estimate starts with the median of recent native floor requests. After three visually normal complete trials using click mode, a median native/automated response offset can refine it. This offset is inferred from observed response intervals, not known one-way transport delay. Ping is already included in those intervals and is not subtracted again. Candidate delays are rounded to 5 ms and bounded to 0–3000 ms, staying at least one 5 ms increment above the highest reported blocked delay for the learned profile. These bounds do not prove safety on the next trial. Clamping can leave a predicted late arrival, which is shown explicitly.

At least eight valid recent responses and five suitable measured first straight steps are required. Statistics use a three-minute window and expire after 60 seconds without a valid first response. Requests without a first response within three seconds remain unmeasured. Repeat clicks before movement begins, clicks while already moving, furniture-use companions and ambiguous automated overlap are excluded. The first normal step may be measured on a longer path; that step duration is separate from total travel time. No further clicks are generated by the collector.

This mode aims for a nominal arrival difference within ±20 ms, rather than the earlier calibration's early-arrival margin. Every trial requires arming and visual feedback. It stops after three near arrivals with normal animation confirmed, three consecutive inconclusive confirmations, or 12 attempts. Normal clicks measure response timing but cannot reveal the pillow's collision check, so an experimental candidate can still block it or miss a 500 ms update cycle. The observed range is not a confidence interval or guaranteed bound. Watch & Walk's existing automatic delay settings are not modified by this feature.

Export schema 4 in Measure arrivals includes `click_latency`; calibration exports include the raw click history and each trial's `click_timing` inputs, original native/automated timestamps, block bound and projected difference. The file records what was actually used, even if later samples change the model.

## Continuous learning / Aprendizado contínuo — Pixelio 1.3.34

Open **Wired → Measure arrivals / Medir chegada**, select the moving furniture, and start measuring. Open the **Continuous learning / Aprendizado contínuo** tab. It learns automatically from your own ordinary steps while measurement is running, including steps with no furniture movement. Watch & Walk can continue using its normal controls.

The tab shows step duration by straight/diagonal/height-changing movement, a candidate update cycle, prediction errors, request-to-walking-update delay, and available RTT observations. Five completed suitable straight steps are the minimum for a forecast; collecting five is not a guarantee of accuracy. Forecasts are frozen when furniture announces its move and later scored against received avatar arrivals. Where walking was already observed, that observed onset is used. Otherwise the forecast assumes the next fitted update opportunity; it cannot guarantee a new request can still reach that opportunity.

The cycle uses at most 60 suitable steps from the last three minutes. After 60 seconds without a suitable step, forecasts become stale. Shifted or inconsistent timing suspends forecasting and relearns. Room, account and connection changes reset learning; pausing retains history. Clear history clears both measurements and learning. There is no disk-restored timing phase; export before closing if you want to retain the observations.

With Watch & Walk running, its successful local engine submissions carry their original send timestamps and available RTT into the learner, even if the UI receives them later. Only a unique matching request and own-avatar step are paired; rapid repeated clicks remain unassociated. Native floor requests are measured from interception, separately from Pixelio submissions. Calibration's direct submissions are not currently paired here. Requests to distant tiles can be paired with the final matching step, so use one-step paths when studying response delay. Request-to-update time includes the local engine, transport, server processing and any earlier path steps; it is not outbound latency alone. The RTT correlation requires at least 10 fresh matched Pixelio requests and 20 ms RTT variation. It is an observational comparison, not a causal model or confidence guarantee.

From 1.3.34, direction on the floor and elevation are recorded independently. An ordinary straight step onto a raised cushion can train the cycle while remaining labelled as a height change. Version 1.3.33 excluded all height-changing steps; the supplied cushion session therefore stayed at zero training samples. Diagonals still do not train the straight-step cycle. The existing observed-duration, drift and staleness checks remain.

The ping comparison excludes diagonal steps and requests with earlier observed path steps. It uses the most represented eligible exact source/destination route, including both heights, and displays that route. This allows cushion steps without pooling unrelated elevations or paths. It still cannot account for unseen updates, server load or changes in path accessibility. Keep test routes fixed when comparing sessions.

Export schema 3 includes the current summary, latest 120 steps/requests, up to 100 scored forecasts, per-row forecasts and existing arrival measurements. Learning does **not** automatically adjust walk delays. Missing furniture motion still cannot establish the exact collision instant; visual blocked/normal feedback remains necessary for that experiment.

## Measure arrivals / Medir chegada — Pixelio 1.3.32

Open **Measure arrivals** inside Watch & Walk, or **Wired → Medir chegada**. This is a passive monitor that can remain open alongside Watch & Walk or arrival calibration. Opening, pausing or closing it does not change either walking tool's controls. It uses the selected account's built-in Pixelio engine and registers no hotkey.

1. Click **Capture click / Capturar clique**, then select the **moving pillow/furniture**, rather than its lever. The ID is filled automatically. If your own avatar has not been identified, re-enter the room and start again.
2. When opened from Watch & Walk, its selected fixed destination is copied into the monitor. Keep **Only this destination / Somente este destino** selected to omit the pillow's return/reset movement. Uncheck it to measure every destination of the selected furniture. A follow-destination rule supplies no fixed tile.
3. Click **Start measuring / Iniciar medição**, then use Watch & Walk normally. A slide creates one row. With a fixed destination, a completed own-avatar step onto that tile also creates a row before any furniture movement. The monitor waits up to two seconds to pair this arrival with a slide of the selected furniture to the same tile, while the avatar remains there. If no slide arrives, the avatar arrival stays visible with **No furniture movement observed / Sem movimento do mobi observado** and no arrival difference. **Avatar step / Passo do avatar** measures the interval between the received walking update and the received stationary/sitting update. It supports normal walking and simple Wired furniture slides lasting up to 5 seconds; rotations, height-only changes, curves and overshoot are not measured as ordinary slides.
4. Read the **Difference / Diferença**: **−100 ms** means the received avatar arrival was 100 ms before the nominal furniture endpoint; **+100 ms** means 100 ms after. **Near simultaneous / Quase simultâneo** means the difference falls within the chosen tolerance, initially **±20 ms**. This is a classification threshold, not a claim of measurement accuracy. The timing columns marked ¹ share the furniture announcement as their zero point; an arrival before that announcement has a negative elapsed time. Without a furniture announcement, the avatar column says **Received / Recebida** and the furniture endpoint/difference remain unavailable. An early difference appears immediately as **Provisional / Provisório** until the nominal animation endpoint. A new movement or conflicting position during the nominal slide cancels the pending comparison.
5. **Export measurements / Exportar medições** saves the latest 100 observations, timestamps, source/destination, duration, per-row tolerance, difference and inconclusive reason. History remains available after pausing. Account/connection changes require closing and reopening the window; room/identity refreshes require starting measurement again.

The furniture endpoint is **local announcement receipt + advertised animation duration**. Avatar arrival is a **received server update**, not a measured rendered frame. Packet transit, batching and client rendering can differ, so this does not establish an exact server collision window or exact visual simultaneity. Arrival at the destination tile also does not by itself prove that the pillow finished normally or that the avatar sat on it. Without furniture motion there is no second arrival to calculate a difference against, even if early occupancy caused a block. Missing avatar arrival within the animation plus 5 seconds is inconclusive, never automatically a block. An avatar already occupying the target without a recent observed completed step, or an update without a matching walking step, is also inconclusive. Pairing is a bounded temporal association; it does not prove that the two events share a Wired trigger. Old arrivals expire and are cleared on rearming, history clearing, room changes and departure from the tile. Without a fixed destination, no-furniture arrivals do not create rows for arbitrary avatar steps.

## Calibrar chegada — Pixelio 1.3.28

Open **Wired → Calibrar chegada**. This window shares the selected account's authenticated Pixelio engine; it has no manual port or external connection controls. Opening it closes Watch & Walk, and vice versa, so only one walking controller and F8 listener can run.

1. Re-enter the test room to receive its furniture and own-avatar identity. Capture the trigger ID and moving furniture ID with one click each.
2. Stand one straight step from the destination, leaving it clear. Click **Aprender 3 movimentos** and manually use the trigger. Return the furniture to its source and repeat, always from the same avatar tile, three times. Learning sends no walks.
3. Reset the furniture and avatar. Click **Armar próxima tentativa**, then use the trigger once within 30 seconds. You can select the trigger after arming: Pixelio 1.3.30 recognizes the walk request accompanying that selection without cancelling merely for the click. If your avatar actually starts walking or leaves the initial tile, the trial cancels. Pixelio sends exactly one walk at the candidate delay after **Use** and measures the furniture announcement, local send, first avatar step and target arrival.
4. Confirm **Normal**, **Bloqueou / interrompeu** or **Inconclusivo**. Normal late arrivals advance the next send; a reported block moves it later. Three normal arrivals with at least 20 ms nominal margin at the same delay validate only an observed candidate. Unknown results do not move the estimate. Each series is limited to 12 trials, including cancelled trials, with 5 ms candidate resolution.

**Parar / F8**, manual walking, another use during a trial, changed motion, identity loss, room/account changes and connection failure cancel further sends. No study or armed state is automatically saved. Export the measurements before changing rooms. Every trial must be armed manually; the tool does not run the trigger or reset furniture for you.

The arrival criterion is the received own-avatar stationary/sitting target update after a matching walking update, before furniture announcement plus animation duration. This is a **nominal packet-based window**, not measured rendered frames or the server's hidden collision decision. Movement absence is never automatically labelled a collision. RTT is recorded when fresh, but is not treated as one-way delay. Nonmonotonic outcomes remain in the history; no valid window is a possible result. The existing 23-trial audit produces 19 late arrivals and four inconclusive missing-movement trials, not 19 early successes.

Current calibration supports a repeatable floor-furniture path, fixed animation of up to 5 seconds, no curve/overshoot, one straight normal avatar step, and a movement announcement within 2.9 seconds of native Use. It observes a possible interaction association, not hidden Wired topology. It does not require room ownership for packets the client ordinarily receives.

Pixelio 1.3.23 includes a Wired tab and the full Watch & Walk tool.

## Wired inspection

1. Connect your account through Pixelio's built-in engine and open the Wired tab. The observer automatically follows the selected account. From version 1.3.26, Wired and its integrated Watch & Walk window use only the built-in engine; no manual port or external G-Earth connection is available in these tools.
2. Re-enter the room to receive a furniture list. The table counts Wired logic boxes, excluding switches, coloured tiles and other supporting furniture.
3. With the Wired tab open and observer connected, click a floor furniture in Habbo Classic to fill its ID automatically (Pixelio 1.3.24 or later). You can also enter an ID manually. Click Find linked Wireds. The server's configured-in-Wireds list can reveal boxes missing from the room furniture list.
4. Select a Wired and click Read configuration (or double-click its row). This requests its settings and may open the corresponding editor in Habbo. The tool does not save or modify its configuration.
5. View its selected and secondary furniture IDs and configured delay in seconds. Triggers have no action-delay field; an unqueried effect does not display a false zero.
6. Export Wired saves the current discovered records and inspection results as JSON.

Absence from the received furniture list is labelled as such; it is not treated as proof of invisibility. The count is the number discovered so far, not a guarantee that every hidden box has been found. Inspection depends on what Habbo permits and returns. No visitor-permission bypass is included.

## Live movement observations (1.3.27)

The Recent Wired movement table shows the affected furniture ID, source and destination coordinates, and the animation duration received from Habbo. It works from movement announcements without requesting Wired settings. A local furniture use within the preceding three seconds is shown as a possible trigger, with its observed elapsed time. This timing association does not confirm a connection or reveal a saved Wired delay. The packet does not contain the originating Wired box or its complete selection list.

The existing Furniture moves rule also recognizes these announcements and uses the tile explicitly selected in the mapping. Choose **Follow Wired destination** to use each announcement's destination instead; fixed coordinates are disabled in that mode. It ignores state-only updates and rotations/height changes that do not change the floor tile. A delayed follow is cancelled if that furniture moves again before dispatch. Saved configurations reopen disarmed.

For the earliest response, set Base walk delay to **0 ms** and arm the selected moving furniture. The engine reacts to the server announcement, before waiting for its animation to finish. In the controlled cushion test, standing on the destination before execution prevented the move; requesting the walk immediately after the announcement allowed the cushion to move and the avatar to sit on it. Server updates showed walking roughly 500 ms after the announcement and sitting roughly 1 second after it. This validates that setup, not every furniture collision rule, intervening tile or continuous movement chain. Habbo controls pathfinding, accessibility and avatar speed; no guarantee of beating other players is made.

The embedded engine now accepts the specific Wired read, latency and bounded walk requests these tools use. Earlier engine-only versions displayed these controls but the engine rejected their outgoing requests.

## Watch & Walk

Click Watch & Walk in the Wired tab. The full existing tool opens in a Pixelio window with the selected connection port:

- Multiple furniture-to-tile mappings, one-click furniture identification and floor-tile capture.
- Any state change, State becomes, and Furniture moves triggers.
- Base walk delay, adaptive ping timing, baseline ping, shared cooldown and first-trigger-wins pending dispatch.
- Explicit arm/stop controls, Escape and global F8 stop/resume, activity log and timing diagnostics.
- Saved hotel/room setups, restored disarmed. Existing HabboLiveData room profiles are retained.

Changing the Pixelio account/connection clears inspection and stops the tool's current connection. The integrated Watch & Walk reconnects to the selected built-in engine disarmed. Room changes and connection loss clear or disarm live results. Close the old standalone Watch & Walk if it already owns F8.

## Validation

232 regression tests passed before packaging, covering the new parsers/session, tab integration, Watch & Walk creation, movement/state rules, timing, persistence and existing watcher UI.

Delay conversion was checked against the client SliderValuePulses implementation: seconds = pulses / 2.
