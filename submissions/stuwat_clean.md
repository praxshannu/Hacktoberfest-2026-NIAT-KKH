# College Group Organizer

##  attendee

- Team name : `stuwat_clean`
- Members and GitHub usernames: `praxshannu`
- 

## Challenge

Select the challenge you are entering:

- [x] Best Open-Source AI Project
- [ ] Best Use of Gemma 4

Only the first box is checked. The project runs Gemma 4 locally through LM Studio, not through the Gemini API, which is what the Gemma 4 evidence section asks for. Before checking the second box, ask the organizers whether a locally run Gemma 4 model qualifies. If they say yes, check it and complete the Gemma 4 evidence section below.

## Project links

- Public GitHub repository: `https://github.com/praxshannu/my-hacktoberfest-project`
- Open-source license (link to the license file): `https://github.com/praxshannu/my-Hacktoberfest-project/blob/main/LICENSE` (Apache License 2.0)

## Problem and solution

**Who is this for?** Students in college WhatsApp groups who lose important deadlines, room changes, and form links in hundreds of forwards and repeated notices.

**What problem does it solve?** It turns a cluttered WhatsApp export into a focused local inbox showing what matters today.

**Main input → output workflow:**

1. Input: a WhatsApp group chat exported as a plain `.txt` file, without media.
2. `python3 build.py` parses the messages, merges duplicates, tags each notice (Deadline, Action, Schedule, Opportunity, Resource, Attendance, Noise), extracts dates, and masks private data. Output: `output/messages.json` and `output/summary.json`.
3. `python3 web/server.py` serves a local dashboard at `http://localhost:8000/web/`, with filters, search, calendar export (`.ics`), and an optional AI panel.
4. Output: a sorted, deduplicated feed with deadlines first, plus answers from the local model about the visible notices.

## Approach and technologies

**Architecture.** The core is deterministic. Parsing, tagging, deduplication, and date logic are plain Python code with rules in `rules.json`. The AI is an optional helper for reading and summarizing text. It is not responsible for deciding dates.

**Models.** Gemma 4 E2B (`gemma-4-e2b-it-qat`), an open-weight model, run locally through LM Studio's OpenAI-compatible API on `127.0.0.1`. The server refuses non-local endpoints. LM Studio itself is a third-party app and is not part of this repo.

**Tools and libraries.**
- Python 3, standard library only, for the build and server
- Vanilla HTML, CSS, and JavaScript for the dashboard, with no build step and no CDNs
- pytest for tests (the only external dependency, and only for running the tests)

**Why these choices.** Standard library only keeps setup simple, and no cloud service is needed. A small local model fits a laptop. Keeping date logic in code avoids relying on a small model for date math.

**AI-assisted development (disclosed).** Much of the code was written with AI coding assistants (Claude Code and the Claude chat assistant). The UI redesign ideas came partly from an AI-generated proposal. I reviewed, tested, and adjusted the code, but I want to be clear that AI tools wrote a significant share of it.

**Reused material.** No third-party code is copied into the repo. The Gemma model weights are downloaded separately through LM Studio. Test data is generated from exported chat samples, and real chats are not committed.

## Challenge evidence

### Best Open-Source AI Project

- **Open-source/open-weight AI component and its role:** Gemma 4 E2B, an open-weight model, runs locally. It has two roles: (1) summarizing and answering questions about the visible notices, and (2) extracting short task titles and steps from action notices (in progress). Dates are never taken from the model.
- **Code link showing the integration:** `https://github.com/<your-username>/<repo-name>/blob/main/web/server.py` (see `model_status`, `prompt_for`, and the `/api/chat` handler)
- **Agent Skill Open Standard compliance (if applicable):** Not applicable. The project does not use Agent Skills.
- **Original harness implementation or meaningful changes (if applicable):** The harness is original. It includes the WhatsApp parser, the rule-based tagger, the deduplication and date-bucket logic, the local proxy with guardrails (loopback-only, model validation, readable errors, token handling, reasoning turned off), and the dashboard. The task update logic (diff and incremental edits, in `web/tasks.py`) is in progress.

### Best Use of Gemma 4

Not claimed. Leave this section out unless the organizers confirm that local Gemma 4 use qualifies.

## Current status

**What works:**
- Parsing WhatsApp exports, including multi-line messages, forwards, deleted messages, and media placeholders. A real run on two group exports produced 1,210 messages and 866 unique notices.
- Rule-based tagging into seven categories, with editable rules.
- Date extraction for numeric and written dates and for relative words, including Telugu words like "repu" (tomorrow).
- Masking of phone numbers and roll-number lists in the UI.
- A local dashboard with tag filters, search, group counts, a collapsed past-deadlines section, and `.ics` calendar export.
- A working connection to LM Studio: model status, model selection, and chat answers about visible notices, with clear errors when the model is unavailable.

**Known limitations / incomplete features:**
- The AI task list is not finished. The first version generated one model call per notice, which was far too slow. It is being reworked to run only on new or changed notices and to edit the existing list.
- Chat answers can still come back empty if the model uses its whole token budget on reasoning. Reasoning is now turned off for these calls, but this needs retesting on every model change.
- Date awareness in AI answers is still being verified, so model answers about "today" should be checked against the dashboard.
- Gemma E2B recalls details poorly from very long inputs, so the prompts are kept small. The 128K context window is not used.
- No live WhatsApp sync. Exports are manual.
- The tagging rules are tuned for the example groups. They need adjustment for other colleges.
- Text only. Images, PDFs, and other attachments are not read.

**What you would improve next:**
- Finish the incremental task list, so only changed notices are sent to the model and checked tasks keep their state.
- Test the full AI flow end to end on the real data and record the results.
- Add a sample dataset with fully masked notices, so people can try the project without their own chats.
- Add a daily summary view.

## Submission checklist

- [ ] Project repository is public and links work. *(Check this yourself by opening the repo link in a private browser window.)*
- [ ] Required challenge evidence is included. *(Best Open-Source AI Project is complete. Gemma 4 is left out unless the organizers confirm it.)*
- [x] Project uses an open-source license where required by the challenge. *(Apache License 2.0 in `LICENSE`.)*
- [x] Work and reused materials are represented honestly. *(AI-assisted development is disclosed above.)*
- [ ] No API keys, tokens, passwords, or private data are included. *(Before submitting, run `git ls-files` and confirm `data/` and `output/` are not listed, and search the repo for `password` and `@` to confirm no real credentials or contact details are committed.)*
- [ ] I followed the organizers' build window and submission instructions. *(Check the dates and the deadline on the MLH page.)*
