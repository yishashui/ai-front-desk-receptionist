# AI Front Desk Receptionist (PACE Center Demo)

A bilingual (English + Mandarin) AI phone receptionist built for the front desk of a PACE center (Program of All-Inclusive Care for the Elderly) that serves Chinese-American seniors.

> **Demo only.** "Sunrise PACE Center" is a fictional name. Every test call used fake data. This is a personal portfolio project and is not affiliated with or endorsed by any healthcare organization.

**Built by:** Yisha Shui, a medical front desk professional (scheduling, prior authorization, bilingual patient support)
**Tools:** Retell AI (voice agent) · Make.com (automation) · Gmail (staff alerts)

---

## The problem

The front desk at a senior care center gets heavy call volume, and the calls are mostly routine:

- Transportation (ride pickup, changes, cancellations)
- "I can't come to the center today"
- Appointments
- Medicine and supplies
- Families asking about enrolling a parent

Many callers are elders with a language barrier who speak slowly and need patience. When the desk is busy, calls go to voicemail and get missed.

## What it does

1. **Language menu:** press 1 for English, 2 for Mandarin, 3 for Cantonese
2. **Main menu:** transportation, absences, appointments, medicine and supplies, enrollment, or anything else (callers can press **or** say the option)
3. **Collects the basics:** participant name, date of birth and callback number, one question at a time
4. **Reads the details back once.** If something is wrong, it re-confirms **only** the corrected item.
5. **Emails staff first, then transfers the call** to a real person
6. **If no one answers** (10 seconds), it takes a message tagged by type, like `[Transportation]` or `[Nurse – Sick]`
7. **Emergency rule:** any sign of chest pain, a fall, stroke and so on → "Please hang up and call 911."

See [docs/call_flow.md](docs/call_flow.md) for the full flow diagram.

## Design decisions and trade-offs

| Decision | Why |
|---|---|
| **Cantonese goes straight to a real person** | I tested the built-in Cantonese voice and it wasn't usable. A natural-sounding Cantonese AI takes a specialized voice provider. For a demo, routing to a Cantonese-speaking staff member is more reliable and still serves the caller. |
| **English + Mandarin only (not 3 languages)** | Fewer languages means better speech recognition. Cantonese callers don't need the AI to understand them, since they go to a person. |
| **Boosted keywords** | Added common words (是、对、可以、不行、好的、接送、预约、出生日期 and common surnames) to help the AI understand seniors' Mandarin. |
| **Names are "close enough"** | Names are the hardest thing for speech AI. The AI asks at most once more, then moves on. Staff confirm the name at callback, using the DOB and callback number, which matter most. |
| **"Participants," never "patients"** | Matches PACE culture and language. |
| **Cold transfer** | Staff just want the call. The details already arrive by email, so no spoken summary is needed. |
| **Email before transfer** | Staff see who is calling **before** they pick up. |
| **My own cloned voice** | Native, natural Mandarin for Chinese-speaking elders. Cloned with MiniMax, which is strong in Chinese. |
| **Callers can't easily cut off the welcome** | Phone echo was being treated as the caller talking, which cut the greeting off. I lowered interruption sensitivity so the menu finishes. |

## What testing found (and how I fixed it)

Built and tested over 10 versions. Highlights:

- The AI **repeated the caller's details over and over** → the script had 4 places telling it to read back; reduced to one read-back.
- The AI **said goodbye but never hung up** → the script said "end the call" but never told it to use the `end_call` function.
- **"Transferring you now" played twice** → a "talk while waiting" setting was adding a second message.
- A declined transfer **went to personal voicemail** → a limit of cold transfer when testing on a cell phone; letting it ring out correctly falls back to taking a message.
- The AI **went silent after two nudges** on a dead line → the 2nd nudge now says goodbye and hangs up, with a 10-sec silence backup.
- **Script timing didn't match reality** → the AI can't keep time from the script; silence timing is controlled by the platform's reminder setting.

Full log: [docs/test_log.md](docs/test_log.md)

## Files

| File | What it is |
|---|---|
| [`prompt/agent_prompt.md`](prompt/agent_prompt.md) | The full agent script (system prompt) |
| [`prompt/welcome_message.md`](prompt/welcome_message.md) | The opening language menu |
| [`config/agent_settings.md`](config/agent_settings.md) | Voice, language, timing and transfer settings |
| [`config/send_details_to_staff.schema.json`](config/send_details_to_staff.schema.json) | Data the AI sends to staff after each call |
| [`integrations/make_email_template.html`](integrations/make_email_template.html) | The staff alert email (Make.com → Gmail) |
| [`docs/call_flow.md`](docs/call_flow.md) | Call flow diagram |
| [`docs/test_log.md`](docs/test_log.md) | Version-by-version test log |

## What's next

- A dedicated Mandarin agent if recognition needs to improve further
- A better Cantonese voice provider to automate Cantonese calls
- Structured call data (call type, DOB, callback number) feeding directly into the center's own EMR or task system
