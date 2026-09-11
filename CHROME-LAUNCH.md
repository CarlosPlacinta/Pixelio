# Chrome launch — 1.3.18

Click **Connect account**. On first use, Pixelio offers **Install Chrome extension**
or **Continue manually**. Your choice is remembered.

## One-time Chrome setup

1. Click **Install Chrome extension**. Pixelio prepares its helper, copies the
   extension folder path, and opens Chrome's extensions page.
2. Turn on **Developer mode**, click **Load unpacked**, and paste the copied folder
   path. If Pixelio is already listed after an update, click **Reload** instead.
3. Keep Habbo.com.br signed in in that Chrome profile. Pixelio detects the
   extension and continues the connection automatically; there is no Done button.

After setup, use **Connect account** as usual. Pixelio waits for the scanner,
routing and listeners before launching Habbo Classic. Chrome must be running
with a signed-in Habbo.com.br tab; it can stay minimized. Your login stays on
Habbo's website, with no password entry in Pixelio.

**Continue manually** keeps the existing workflow and stops future setup prompts.
Change your choice later under **Connections → Habbo launch**. Chrome failures
also leave manual entry available. The same Connect button cancels setup or
disconnects an active connection.

The extension currently requires Chrome's one-time **Load unpacked** step; it is
not yet published in the Chrome Web Store. Pixelio cannot silently approve
extension installation.

Pixelio detects the newest installed official AIR client. **Choose Habbo Classic**
in Connections lets you select another installed AIR client. Install the extension
in the Chrome profile containing the Habbo account you want to use.

## Implementation and limits

The extension has only Habbo.com.br host access, scripting, native messaging and
reconnect alarms. Its injected function calls Habbo's own `$http` service, using
the site's existing fingerprint/session interceptors. It does not inspect cookies
or passwords. The native host accepts only this extension's stable ID. An
attempt-specific authenticated loopback connection carries the ticket in memory;
only temporary discovery metadata is persisted. No shell or external protocol
handler receives the launch command.

Cancellation and disconnect invalidate pending results. A late website response
cannot launch the game after cancellation. A second Pixelio window cannot replace
an active launch request. Timeout does not trigger a new client launch.

The initial implementation is for Windows and Habbo BR, matching Pixelio's current
scanner support. Habbo's public website script and installed launcher code were
inspected to verify the endpoint, protocol and native arguments. Automated tests
use synthetic tickets; a real signed-in launch remains to be verified after the
one-time Chrome installation. No store submission or automatic extension install
was performed.

## Verification

- Python connection, scanner, account, Chrome and first-connect onboarding checks.
- JavaScript tests for response validation, missing tab, cancellation, retry,
  host scope, readiness detection and the actual injected request function.
- Real local sockets with native messaging, including the compiled helper.
- Rebuilt engine; listener readiness is emitted only after binding all listeners.
- Packaged one-folder, single-file and relocated executable installation checks.

To remove the extension, remove Pixelio from chrome://extensions. The native helper can be unregistered by deleting only com.pixelio.habbo under HKCU\Software\Google\Chrome\NativeMessagingHosts. Saved Pixelio scans are separate.
