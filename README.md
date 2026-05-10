# Rehab Safe Vault

Recovery is a long road, and like any long journey it helps to have a companion to reflect with — someone who remembers what you did last week, notices what's slowly changing, and asks the right questions when you stop noticing yourself.

This is an attempt at that companion. A structured journal you keep on your own computer, that an AI assistant can read and update with you. You describe your injury and what bothers you; together you sketch out a 10-day program; after each session it asks how things felt; if you record a video, it can compare your technique to a reference. Over time the journal becomes the memory you don't have to carry yourself.

And even if you skip the AI part entirely, [`05_Rehab_Protocols/Exercise_Database/`](05_Rehab_Protocols/Exercise_Database/) is a small, browsable library of exercises with reference videos, key cues, common errors, and conditions for adding them to a cycle. No sign-up, no app — just markdown files you can read on GitHub.

---

## Before you start

A few honest caveats:

- **Not a replacement for a physiotherapist.** This does not diagnose, prescribe, or treat anything. A physiotherapist watches you move, puts hands on you, adjusts in real time — none of that happens here. Keep seeing yours. This tool is for the time *between* appointments, when you're alone with your protocol and your memory.
- **N=1.** Built by one person (me) recovering from shoulder surgery. Use with your own judgment.
- **Shoulder-focused.** The exercise database currently covers shoulder rehab only. Other body parts need to be added (PRs welcome).
- **Not medical advice.** Run any program changes by your PT before acting on them.
- **Tested with `gemini-3-flash-preview`** (May 2026). The Gemini CLI picks a default model on its own — you don't need to configure this. Newer or different models will likely work, but I haven't verified them.

---

## What you need

- A computer (Mac, Windows, or Linux)
- [Node.js](https://nodejs.org) v18 or newer
- A free Gemini API key (instructions below)
- [Obsidian](https://obsidian.md) — optional, for nicer browsing of your notes

---

## Setup

### 1. Get a free Gemini API key

Gemini is the AI that runs your sessions and analyzes videos. Free tier is plenty for personal use.

1. Open [aistudio.google.com](https://aistudio.google.com) in your browser.
2. Sign in with a Google account.
3. Click **Get API key** (top left) → **Create API key**.
4. Copy the key — it starts with `AIza...`.

Now save it so the CLI can find it.

**Mac / Linux** — open Terminal:

```bash
echo 'export GEMINI_API_KEY="paste-your-key-here"' >> ~/.zshrc
source ~/.zshrc
```

(If you use bash instead of zsh, replace `~/.zshrc` with `~/.bashrc`.)

**Windows** — open PowerShell:

```powershell
[System.Environment]::SetEnvironmentVariable('GEMINI_API_KEY', 'paste-your-key-here', 'User')
```

Then close and reopen PowerShell.

### 2. Install the Gemini CLI

```bash
npm install -g @google/gemini-cli
```

### 3. Download the vault

```bash
git clone https://github.com/<your-username>/rehab-safe-vault.git
cd rehab-safe-vault
```

(Or download the ZIP from the GitHub page if you don't use git.)

### 4. (Optional) Open in Obsidian

Install [Obsidian](https://obsidian.md), click **Open folder as vault**, and point it at the `rehab-safe-vault` folder. This gives you a nice UI for reading your notes — but everything works fine without it.

---

## First run — onboarding

Inside the vault folder, in your terminal:

```bash
gemini
```

Type: **start onboarding**

![Onboarding process](assets/onboarding.avif)

Gemini will ask you questions one at a time — what you're recovering, current phase, restrictions, what bothers you, equipment you have, time per day. Based on your answers it builds a 10-day program with 3–5 exercises tailored to your situation.

---

## Daily use

### Run a session

```bash
gemini
```

Type: **let's start a session**

Gemini walks you through your 3–5 exercises one by one. After each one, you can drop a video file into the terminal — it gets saved and queued. Gemini then analyzes your technique against the reference video and writes detailed feedback.

You can also skip recording and just answer the subjective questions (pain, ROM, how it felt).

### Review your cycle

After 2 weeks:

```bash
gemini
```

Type: **review cycle**

Gemini summarizes regularity, pain trends, range of motion, recurring technique errors, and proposes what to keep, change, or replace for the next cycle.

---

## Starting over

If you've already gone through onboarding once and want to start fresh — say after a break, a new injury, or just because you want to reset — open Gemini and say **"start onboarding"**. The agent notices that you already have a profile and cycle, lists what exists, and offers three options: update only specific profile fields, keep history but build a new cycle, or do a full reset. Nothing is overwritten without your explicit confirmation, and your past session logs are never touched automatically — they stay as a record of where you've been.

If you'd rather wipe everything by hand, the relevant files are `00_Profile.md`, `00_Home.md`, and `05_Rehab_Protocols/Current_Cycle.md` in the vault root. Past sessions live in `07_Session_Log/`.

## Privacy

Nothing leaves your computer. The following are gitignored and never pushed:

- `00_Profile.md` — your medical context
- `00_Home.md` — your personal dashboard
- `05_Rehab_Protocols/Current_Cycle.md` — your active program
- `07_Session_Log/` — all sessions, videos, and analysis

Only the vault structure, templates, and the shared exercise database are tracked in git.

---

## Adding new exercises

Open Gemini and say: **"add this exercise: <YouTube link or description>"**. It drafts an entry in `05_Rehab_Protocols/Exercise_Database/` and asks you to confirm difficulty, weight, and cues before saving. The structure supports any body part — knee, hip, back, ankle — not just shoulder.

---

## Contributing

PRs welcome, especially:

- Exercise databases for other body parts (knee, hip, back, ankle)
- Workflow improvements in `AGENT.md`
- Translations of `AGENT.md`

Open an issue if anything is unclear or broken.
en.
