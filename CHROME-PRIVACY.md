# Pixelio — Habbo Connect privacy policy

Last updated: 11 September 2026

> The Chrome integration was removed from Pixelio 1.3.20. This policy is retained for the earlier experimental extension; current Pixelio releases do not include that extension.

Pixelio — Habbo Connect is an optional browser companion for the Pixelio desktop
application on Windows. It helps open the installed Habbo Classic client using
your existing session on https://www.habbo.com.br. Pixelio is independent and is
not an official Habbo or Sulake product.

## Information used

When you click Connect account in Pixelio with Chrome launch selected, the
extension locates an open Habbo.com.br tab and asks that website for a fresh
client launch response. Chrome and Habbo handle the existing signed-in session.
The extension does not directly read browser cookies, saved passwords or password
manager data.

The response contains a client authentication ticket and may contain an
account or avatar identifier. The extension processes that response in memory,
validates the ticket and passes the required launch ticket to Pixelio's native
helper on the same computer. Pixelio uses it to start the selected Habbo Classic
executable. This is authentication information; it is not anonymous data.

The extension can see limited tab metadata needed to identify Habbo.com.br. Its
site permission is restricted to https://www.habbo.com.br/*. It does not access
the browser's history database, track visits to other sites, or collect browsing
activity for analytics.

## Use, transfer and retention

The authentication ticket is used only for the user-requested Habbo launch. The
extension and Pixelio helper do not persist tickets to files, browser storage or
logs, and do not send them to the Pixelio developer, analytics providers,
advertisers or an unrelated remote service.

The extension communicates with Habbo's website over HTTPS and with the Pixelio
helper through Chrome native messaging on the local computer. The desktop app
keeps launch preferences and temporary local connection-discovery metadata,
which do not contain the Habbo authentication ticket. During setup, the desktop
app also checks a public Pixelio release-status file on GitHub to determine whether
the store listing is published; this request contains no Habbo ticket or login data.

There is no advertising, sale of personal information, profiling, credit scoring
or unrelated use of this information. No data is sold or shared for advertising.
Habbo and Chrome operate under their own privacy policies.

## Control and removal

Choose Continue manually or Launch manually in Pixelio to stop using the Chrome
launch flow. Cancel connection discards pending launch results. Remove the
extension from chrome://extensions to disable its access. Pixelio's saved scans
and desktop preferences are separate from the extension.

## Contact and changes

Questions about this extension can be raised at:
https://github.com/CarlosPlacinta/Pixelio/issues

Do not include passwords, authentication tickets or other secrets in public
issues. If the extension's data handling changes, this policy will be updated.
