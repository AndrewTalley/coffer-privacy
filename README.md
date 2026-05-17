# Coffer — v0.2.0

Save the answer, not the whole chat. Bookmark useful responses from ChatGPT, Claude, and Gemini in one searchable vault.

## What's in this build

- **Per-message Stash button** on assistant responses across ChatGPT, Claude, and Gemini
- **Floating action button** on every supported site with three actions: save full chat, export as Markdown, open Coffer
- **Local-first storage** (`chrome.storage.local`) — your data never leaves your computer
- **Tabbed vault UI** — Snippets (default) and Chats
- **Search** across titles, tags, message text
- **Folders** for organizing saved chats
- **Auto-titled snippets** — each Stash uses your matching question as the title
- **Clean Markdown extraction** — code blocks with language tags, lists, headings, bold, links all preserved
- **Rendered snippet view** — code blocks with monospace + language badges, inline code styling, headings, horizontal rules, clickable links
- **Quick-delete trash icon** on snippet rows (hover to reveal)
- **Copy + Open original** actions on saved snippets

## File layout

    manifest.json              Manifest V3 config
    background.js              Service worker (install bootstrap + message router)
    content/
      chatgpt.js               ChatGPT adapter (CodeMirror-aware extractor)
      claude.js                Claude adapter
      gemini.js                Gemini adapter (handles <code-block> custom element)
      sidebar.css              Styles for injected UI (FAB, toast, save buttons)
    lib/
      content-helpers.js       Shared: FAB, toast, save-button injection, observer
      storage.js               chrome.storage wrapper (chats, folders, snippets)
      exporter.js              Chat → Markdown + download
      turndown.js              Bundled HTML→Markdown converter (MIT, by Dom Christie)
    popup/
      popup.html               Vault UI
      popup.css                Vault styles (warm stone palette, serif headings)
      popup.js                 Vault logic + markdown renderer
    icons/                     16/48/128 PNGs
    README.md
    PRIVACY.md

## Install for local testing

1. `chrome://extensions` → toggle Developer mode on
2. Click **Load unpacked** → select this folder
3. Pin the extension to the toolbar
4. Visit chatgpt.com, claude.ai, or gemini.google.com
5. Hover any assistant response — a "Stash" button appears top-right
6. Click it. Open the toolbar icon to see your vault.

## Architecture notes

Each platform has a thin content-script adapter that delegates to `lib/content-helpers.js` for shared behavior (button injection, toast, observer, error handling).

Each adapter provides:

- `source`: a string (`'chatgpt'` / `'claude'` / `'gemini'`)
- `findAssistantNodes()`: returns DOM nodes that should get a Stash button
- `getChatTitle()`: returns the current conversation's title
- `findPromptFor(node)`: returns the user message that produced this response (used as the snippet title)
- `extractContent(node)`: platform-specific HTML→Markdown extraction
- `parseConversation()`: returns `[{ role, content }, ...]` for the whole chat

The `extractContent` functions are where the per-platform DOM logic lives:

- **ChatGPT**: handles CodeMirror viewers (`.cm-editor`), pulls language label from the sticky header, converts `<br>` to newlines.
- **Claude**: handles standard `<pre>` blocks with sibling language-label divs inside the `group/copy` wrapper.
- **Gemini**: handles Angular `<code-block>` custom elements, strips `.code-block-decoration` chrome, removes `.sr-only` screen-reader text.

All three pre-process the host DOM into clean `<pre><code class="language-x">` and then hand off to **Turndown** (`lib/turndown.js`) for the actual Markdown conversion (headings, lists, links, emphasis, etc.).

To add a new platform: write a ~50-line adapter following the pattern above and add a content_scripts entry in `manifest.json`.

## Markdown rendering in the popup

Saved snippets are stored as Markdown. The popup's `renderSnippetBody()` function in `popup.js` parses fenced code blocks out, renders them in styled boxes with language badges, and handles inline code, bold, italic, headings, links, and horizontal rules with a small regex-based renderer.

## Third-party code

- **Turndown** (MIT) — bundled at `lib/turndown.js`. Converts HTML to Markdown. Runs entirely client-side; makes no network requests. https://github.com/mixmark-io/turndown

## Known limitations

- No "move chat to folder" UI yet (chats land in Inbox).
- No tag editor.
- Cloud sync is not implemented (data is local-only on this device).
- Parsers will break when the host sites update their DOM. Selectors are isolated per adapter file to keep fixes small and platform-specific.
- The markdown renderer in the popup handles common cases (headings, lists, code, bold, italic, links, hr) but is not a full CommonMark parser. Complex tables or deeply nested lists may not render perfectly.

## Versions

- **0.2.0** — Snippets feature, Turndown-based Markdown extraction across all three platforms, per-snippet trash icon, refreshed UI (warm stone palette, serif headings, JetBrains Mono code), shared content-helpers refactor, brand rename to Coffer.
- **0.1.0** — First working version: per-message save, vault popup with snippets/chats tabs, local-first storage.
