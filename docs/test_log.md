# Test log

All test calls used fake data.

| Version | Change | What testing found |
|---|---|---|
| V1–V3 | First practice receptionist for a specialty clinic (ENT): office info, appointments, new patients by insurance type, refills, records | Proved the basic flow: collect details → email staff → transfer |
| V4 | Rebuilt for a PACE senior center: "participants," language menu, transportation/absence/appointment/medicine options, Cantonese → staff | Mandarin speech OK; built-in Cantonese voice not usable |
| V5 | English only, cold transfer, 10-sec ring, end_call added to every ending, read back only once | AI had been repeating details 4 times and never hanging up; both fixed |
| V6 | Re-confirm only the corrected item; turned off "talk while waiting" | "Transferring you now" had played twice |
| V7 | "Van ride" → "Transportation" everywhere (script, tags, email) | Clearer wording for callers and staff |
| V8 | English + Mandarin, boosted keywords, accuracy mode, silence settings | Mandarin understanding improved; DOB, phone number and "对的" recognized correctly. Names still hard (e.g. heard as similar-sounding words). |
| V9 | Interruption sensitivity 0.9 → 0.5; name rule (ask once more, then accept the closest guess); back to 2 languages | Phone echo had cut off the welcome message |
| V10 | Cloned the builder's own voice (MiniMax); 2nd silence nudge now says goodbye and hangs up; end call on silence 30 → 10 sec | After 2 nudges the AI had gone idle on a silent line. Cloned voice sounds natural in Mandarin but still noticeably "AI"; expressive mode isn't available for cloned voices |

## Lessons learned

- **The AI can't keep time.** "Wait 5 seconds" in a script does nothing; the platform's silence settings control timing.
- **Tell the AI which tool to use.** "End the call" isn't enough; it needs "use the end_call function."
- **One instruction, one place.** Repeating a rule in several sections makes the AI repeat itself.
- **Design around weak spots.** Cantonese voice and name recognition are hard, so route to people and let staff confirm.
- **Test like a real caller.** Declining a transfer on a cell phone sends it to voicemail, which a real clinic line wouldn't do.
