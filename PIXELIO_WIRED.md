# Pixelio Wired and Watch & Walk

Pixelio 1.3.23 includes a Wired tab and the full Watch & Walk tool.

## Wired inspection

1. Open Pixelio's Wired tab. Connect the observer. It uses the active account's connection port when available; an external G-Earth port can also be entered.
2. Re-enter the room to receive a furniture list. The table counts Wired logic boxes, excluding switches, coloured tiles and other supporting furniture.
3. Enter a furniture instance ID and click Find linked Wireds. The server's configured-in-Wireds list can reveal boxes missing from the room furniture list.
4. Select a Wired and click Read configuration (or double-click its row). This requests its settings and may open the corresponding editor in Habbo. The tool does not save or modify its configuration.
5. View its selected and secondary furniture IDs and configured delay in seconds. Triggers have no action-delay field; an unqueried effect does not display a false zero.
6. Export Wired saves the current discovered records and inspection results as JSON.

Absence from the received furniture list is labelled as such; it is not treated as proof of invisibility. The count is the number discovered so far, not a guarantee that every hidden box has been found. Inspection depends on what Habbo permits and returns. No visitor-permission bypass is included.

## Watch & Walk

Click Watch & Walk in the Wired tab. The full existing tool opens in a Pixelio window with the selected connection port:

- Multiple furniture-to-tile mappings, one-click furniture identification and floor-tile capture.
- Any state change, State becomes, and Furniture moves triggers.
- Base walk delay, adaptive ping timing, baseline ping, shared cooldown and first-trigger-wins pending dispatch.
- Explicit arm/stop controls, Escape and global F8 stop/resume, activity log and timing diagnostics.
- Saved hotel/room setups, restored disarmed. Existing HabboLiveData room profiles are retained.

Changing the Pixelio account/connection disconnects inspection and stops the tool's current connection. Room changes and connection loss clear or disarm live results. Close the old standalone Watch & Walk if it already owns F8.

## Validation

232 regression tests passed before packaging, covering the new parsers/session, tab integration, Watch & Walk creation, movement/state rules, timing, persistence and existing watcher UI.

Delay conversion was checked against the client SliderValuePulses implementation: seconds = pulses / 2.
