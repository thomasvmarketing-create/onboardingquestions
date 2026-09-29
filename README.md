# Onboarding Questions

Client interview tools for Videos That Convert, built as Claude artifact pages.

## What's here

| File | What it is |
| --- | --- |
| `onboarding.html` | **Channel Onboarding Interview.** 17 onboarding questions, one per screen. Clients talk or type their answers; nothing responds to them. Answers save as they go. The owner sees a private dashboard of every client's responses. |
| `Channel Onboarding Interview.html` | The same onboarding page as published (includes the page wrapper Claude adds at publish time). |
| `script-interview.html` | **Script Interview.** A paged video brief plus a live Claude interviewer that asks one question at a time and turns the answers into a context pack for scripting. |
| `skills/client-script-interview/SKILL.md` | The Claude skill behind the script interview, for use directly in Claude chats. |
| `tests/onboarding.test.js` | End-to-end browser tests for the onboarding page. |

## How the pages run

Both pages are written for Claude's artifact runtime (`window.claude.use(...)`):

- `onboarding.html` uses the artifact database (`db`) to save each client's answers under `submissions/<their id>`, `user` to tell the owner from clients and show client names, and `downloads` for the copy/download fallback. Access rules published with it:
  - `submissions`: read and write by the owner only
  - `submissions/{self}`: each signed-in client reads and writes only their own answers
- `script-interview.html` uses `sample` (Claude answers on the viewer's account), `artifact` (the owner edits and republishes the brief), `downloads`, and `user`.

Opened outside Claude (for example straight from this repo), `onboarding.html` still works but keeps answers on the device only, and `script-interview.html` can show the brief but can't run the interviewer.

## Sending the onboarding interview to a client

1. Open the published page in Claude and use the Share menu.
2. Add the client's email with **Editor** access so their answers save to you. They need a free Claude account.
3. Send them the link. They only see their own interview.

Clients with view-only access can still complete it; at the end they copy or download their answers and send them over.

## Running the tests

Requires Node and Playwright with Chromium.

```bash
npm install playwright
npx playwright install chromium
node tests/onboarding.test.js
```

The tests simulate the Claude runtime and cover: autosave while typing, navigation, resume on a new device, sending, the view-only fallback, the owner dashboard, and phone-width layout.
