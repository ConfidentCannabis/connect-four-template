# Connect Four take-home

Thank you for taking the Confident LIMS engineering exercise. You will build a small
Connect Four game:

1. **Stage 1 (required, about 2 hours):** a basic React frontend and a basic Python
   backend with only one endpoint, written by hand with no AI.
2. **Stage 2 (optional, no more than 1 hour):** improve your project with an AI coding
   agent of your choice.

There is no time limit. The times above are what we expect an experienced engineer to need.

**What we value.** We prefer a small amount of excellent work to a large amount of rough
work. Work that is bug-free, thoughtful, clean and tested scores higher than extra features.

Our email to you contains **your personal Zoom link**, used to record Stage 1, and the link
to our **submission form**. Keep the form link: you'll come back to it to submit when you're
done. Please don't share your Zoom link.

## Setup (about 10 minutes)

1. Click **Use this template** at the top of this page and create a **private** repo in your
   own GitHub account.
2. Invite our reviewers **@salbarran**, **@KN3LL2K**, **@eatpie75** and **@JosephChou124**,
   and our grading account **@confident-interview-bot**, as collaborators
   (**Settings → Collaborators → Add people**). GitHub gives collaborators on a personal repo
   write access; we only read your code and never push to it.
3. Clone your repo and check that both apps run (commands below). Keep these install, run
   and test commands working: we use them to run and test your code.
4. Install the free [Zoom app](https://zoom.us/download) if you don't have it. You don't need
   a Zoom account.

You need [Node.js](https://nodejs.org/) 20 or newer and [Python](https://www.python.org/) 3.10
or newer.

| Folder | Install | Run | Test |
|---|---|---|---|
| `frontend/` | `npm install` | `npm run dev` (http://localhost:5173) | `npm test` |
| `backend/` | `pip install -r requirements.txt` | `uvicorn main:app --port 8080` | `pytest` |

We suggest a virtual environment for the backend:

```bash
cd backend
python3 -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --port 8080
```

The frontend dev server forwards requests for `/evaluate_board_state` to
`http://localhost:8080`, so the browser can call the backend without any CORS setup. Call it
with a relative URL, for example `fetch("/evaluate_board_state", ...)`.

## Rules

- **Stage 1 must be your own code.** Do not use AI assistants, AI code completion or AI chat.
  Documentation, search and package docs are fine. Turn off AI features in your editor before
  you start.
- **Record all of Stage 1 on your personal Zoom link.** Before you start Stage 1, join the link
  and share your **entire screen** (not a single window). Recording starts automatically and
  is saved to our account; there is nothing to upload. Camera and microphone are optional. To
  take a break, leave the meeting and rejoin the same link when you continue. Close anything
  private before you share your screen. End Stage 1 by running `git log --oneline -3` on
  screen (see *Commits and tags*), then leave the meeting. You don't need to record Stage 2.
- **Commit as you go**, at least every 30 minutes, and push to your repo.
- At the technical interview you'll walk us through your Stage 1 code and make a small change
  to it without AI.
- In Stage 2, use any AI tool with your own account. Do not connect it to any employer or
  company systems, repos or data.
- Keep your repo private.

## Commits and tags

Your git history tells us which code is hand-coded and which is AI-assisted, so please follow
these steps exactly:

1. Start every Stage 1 commit message with `stage1:`.
2. When Stage 1 is done, make your final hand-coded commit, tag it and push:

   ```bash
   git add .
   git commit --allow-empty -m "stage1: FINAL hand-coded submission"
   git tag stage1-final
   git push origin main stage1-final
   git log --oneline -3   # show this on the Zoom recording, then leave the meeting
   ```

3. If you do Stage 2, start every Stage 2 commit message with `stage2:`, and when you are
   done:

   ```bash
   git tag stage2-final
   git push origin main stage2-final
   ```

Do not rewrite history after tagging `stage1-final` (no rebase, amend or force-push).

## Stage 1: Connect Four (hand-coded, no AI)

Build a Connect Four game that you can play in the browser, with a React frontend in
`frontend/` and a Python backend in `backend/`. If you aren't familiar with the game, see the
[rules](http://mathworld.wolfram.com/Connect-Four.html) and this
[video](https://www.youtube.com/watch?v=H3FYRM9a0i4).

### Frontend

- The board has 6 rows and 7 columns.
- One person plays both colors on one screen: blue (player 1) and red (player 2). Blue always
  goes first, and the players take turns.
- A chip dropped in a column lands in the lowest open slot. A full column takes no more chips.
- Chips need not be circular and the board need not be realistic, but use simple, intuitive
  graphics.
- **After every move, the frontend sends the board to the backend** and uses the response to
  show whose turn it is, the winner and the winning chips, or a draw. Play stops when the game
  is over. If the backend rejects the board, show the error.
- No saved state is needed; losing the game on refresh is fine. You may use any libraries.

### Backend

A simple server in Python with a single API call that evaluates the board. No database is
needed. Use port `8080`. We will run tests against your server.

**POST** `/evaluate_board_state`

It accepts a game state as JSON:

- `0` is an empty slot
- `1` is a blue piece (player 1)
- `2` is a red piece (player 2)

```json
{
    "board": [
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 2, 0, 0],
        [0, 0, 0, 2, 1, 1, 0],
        [0, 0, 0, 1, 1, 2, 0]
    ]
}
```

The route handler should:

- Validate the request body and return errors accordingly.
- Evaluate the board: whether the game is in progress (and whose turn it is), won (by whom,
  and which pieces form the win) or drawn, or whether the board is impossible under the rules.

You choose the response format.

Game rules:

- Blue (player 1) always goes first.
- A win occurs when at least 4 pieces of the same color appear in at least one horizontal row,
  vertical column, or diagonal.
- A draw occurs when the board is filled with pieces and there are no rows, columns or
  diagonals with 4 or more consecutive pieces of the same color.

**Write the code as if you were opening a pull request for a colleague to review.** Add a
short section to this README on how to run the app and its tests. Within the time available,
favor a working game over an unfinished one.

### What we look for

- [ ] a game that works without bugs
- [ ] a clear, usable, and visually appealing UI
- [ ] correct, robust board evaluation that handles edge cases
- [ ] useful API responses
- [ ] sensible structure, with game logic separate from request handling
- [ ] clean, readable code
- [ ] tests

## Stage 2: improve it with AI (optional, no more than 1 hour)

Use an AI coding agent of your choice to improve anything you like in your Stage 1 project.
You decide what matters most. Please spend no more than an hour.

- Start every commit message with `stage2:` and tag your last commit `stage2-final`.
- Save and commit the complete, unedited AI session transcripts in `ai-transcripts/` (see
  *Exporting AI transcripts* below).
- Add a few lines to this README under a **Stage 2** heading describing what you changed and
  why.
- Commit a document (for example `NEXT_STEPS.md`) describing what you would do if you had more
  time (UI/UX improvements, new features, bug fixes, infrastructure, etc.).

### What we look for

- [ ] good choices about what to improve
- [ ] work that is correct and verified
- [ ] effective use of the AI agent (good context, reviewing and testing its output, and
  catching its mistakes)
- [ ] thoughtful and self-aware suggestions for further work if given more time

### Exporting AI transcripts

- **Claude Code:** run `/export` in each session and save the file to `ai-transcripts/`. Raw
  session logs are also stored as JSONL files under `~/.claude/projects/`.
- **Codex CLI:** session logs are stored as JSONL files under `~/.codex/sessions/`. Copy the
  sessions for this project.
- **Cursor:** use the chat panel's export option for each chat, or copy the full chat.
- **GitHub Copilot Chat (VS Code):** run **Chat: Export Chat...** from the command palette.
- **Any other tool:** use its export feature, or copy and paste the full conversation into a
  Markdown file.

## Submit

When you are done, open the submission form (the link in our email) again and fill it in once.
It asks for your name, your email, your repo URL, the `stage1-final` commit SHA, the
`stage2-final` commit SHA (leave it blank if you skipped Stage 2), and a statement that Stage 1
is your own work without AI.

**Use the same email address you used in your job application.** We use it to match your
submission to your application.

Questions? Reply to the email we sent you.
