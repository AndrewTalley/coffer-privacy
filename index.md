# Coffer — Privacy Policy

\_Last updated: May 17, 2026

## Short version

Coffer stores your data **on your own device**. We have no server. We do not collect, transmit, or sell any of your data. We never see your conversations.

## Who we are

Coffer is built and maintained by Andrew Talley as an independent developer. Questions can be sent to the email at the bottom of this page.

## What Coffer handles

When you click "Stash" on an AI response, or "Save full chat" from the floating button, Coffer reads the messages visible on that page and stores them locally in your browser (`chrome.storage.local`). This data never leaves your device.

It also reads:

- The page URL of the conversation, for "Open original"
- The conversation's title shown on the page

It does **not** read:

- Your account credentials
- Cookies or session tokens
- Data on websites other than chatgpt.com, claude.ai, and gemini.google.com

## What we collect

Nothing. There is no analytics, no telemetry, no remote server. Coffer makes no network requests of its own.

## Permissions explained

- **storage** — save your snippets and chats to local browser storage
- **activeTab / scripting** — read the conversation visible in your active tab when you trigger an action
- **downloads** — write Markdown exports to your computer when you click Export
- **host permissions on chatgpt.com, claude.ai, gemini.google.com** — the extension only runs on these sites

## Third-party code

Coffer includes the open-source Turndown library (MIT licensed) for converting HTML to Markdown. Turndown runs entirely in your browser and does not connect to any external server.

## Future features

Some future features (such as cloud sync between devices) may require sending data to a server. If we ever add such a feature, this policy will be updated _before_ it ships, and any such feature will be strictly opt-in.

## Your data, your control

Delete any snippet, chat, or folder from the vault. To remove everything, uninstall the extension — Chrome clears `chrome.storage.local` automatically.

## Data export

You can export any saved chat as Markdown using the "Export .md" button in the vault. Saved data lives in `chrome.storage.local` and is portable through standard Chrome extension storage tools.

## Changes to this policy

We may update this policy as Coffer evolves. The "Last updated" date at the top will reflect any change. For material changes (like adding analytics or cloud sync), we will notify users via an update note in the extension itself.

## Contact

Questions or concerns: cofferai2026@gmail.com
