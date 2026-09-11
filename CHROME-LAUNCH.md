# Optional Chrome launch

Pixelio 1.3.17 supports two saved choices under **Connections → Habbo launch**.

- **Launch manually (current flow)** is the default: click Connect account, then enter Habbo Classic yourself as before.
- **Launch through Chrome** uses the same Connect account button to prepare Pixelio and open the installed Habbo Classic client using your signed-in Habbo.com.br tab.

## One-time setup

1. Install or update Pixelio from the [latest release](https://github.com/CarlosPlacinta/Pixelio/releases/latest).
2. Open **Connections → Set up Chrome launch**. Pixelio registers its local helper and opens the extension folder.
3. In Chrome, open `chrome://extensions`, enable **Developer mode**, click **Load unpacked**, and choose that folder.
4. Keep Habbo.com.br signed in on a tab in that Chrome profile. Install the extension in just the profile whose account you want to launch.
5. Choose **Launch through Chrome** in Pixelio. The latest installed official AIR client is detected; **Choose Habbo Classic** lets you select another installed AIR executable.
6. Use **Connect account**. The extension has no daily button to click. Chrome and the Habbo tab can stay minimized.

You enter your login details only on the Habbo website. Chrome launch requires Chrome to be running. To switch back, choose **Launch manually (current flow)**; the extension is not required for manual launch.

## Timing and cancellation

Pixelio waits for the scanner's authenticated attachment, Habbo routing and all connection listeners before requesting a fresh ticket. The same button cancels or disconnects. A late website response cannot launch Habbo after cancellation. A failed Chrome request leaves manual entry available. Pixelio does not close an existing Habbo client automatically.

## Preview status and data

Chrome launch is experimental and currently supports Windows and Habbo BR. The extension has not been published to the Chrome Web Store; installation uses the developer workflow above. No silent installation or enterprise policy change is performed.

The extension requests only Habbo.com.br site access, scripting, native messaging and reconnect alarms. It uses Habbo's own request service and does not extract cookies or passwords. The launch ticket passes locally in memory to the installed client; Pixelio does not save it or send it to another service.

Automated checks passed with synthetic tickets, including cancellation, missing tabs, server validation, the compiled native helper and packaged apps. A real signed-in Habbo launch remains to be verified after extension installation.

To remove the extension, remove Pixelio from `chrome://extensions`. The native helper can be unregistered by deleting only `com.pixelio.habbo` under `HKCU\Software\Google\Chrome\NativeMessagingHosts`. Saved Pixelio scans are separate.
