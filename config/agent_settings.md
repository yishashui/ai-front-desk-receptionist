# Agent settings (Retell AI)

| Area | Setting | Value | Why |
|---|---|---|---|
| Language | Speech languages | Multilingual: English + Mandarin | Fewer languages = better recognition; Cantonese goes to a person |
| Voice | Voice | Custom voice clone of the builder (MiniMax, Speech 2.8 Turbo) | Natural native Mandarin; tested well in Mandarin. Expressive mode is not available for cloned voices |
| Transcription | Mode | Optimize for accuracy | Seniors speak slowly; accuracy over speed |
| Transcription | Boosted keywords | 接送, 交通, 预约, 看诊, 药物, 医疗用品, 加入, 出生日期, 电话号码, 重复, 不能来, 生病, 是, 对, 可以, 行, 不行, 好的, 陈, 李, 王, 张, 黄, 林, Sunrise PACE Center, transportation, enrollment, appointment, date of birth | Helps the AI hear common answers and names |
| Speech | Interruption sensitivity | 0.5 | Phone echo was cutting off the welcome menu at 0.9 |
| Speech | Response wait time | 0 ms | Responds right away |
| Speech | Reminder message | 5 sec, 2 times | 1st nudge repeats the menu; 2nd nudge says goodbye and hangs up. The script's own timing is ignored by the AI |
| Keypad | Keypad input | On | Menus use 1/2/3 and 0–9 |
| Keypad | Digit limit | 1 | Responds instantly to a single key press |
| Keypad | Timeout | 2.5 sec | |
| Call | End call on silence | 10 sec | Backup hang-up so the AI never sits idle on dead air |
| Call | Max call duration | 1 hour | |
| Transfer | Type | Cold transfer | Staff already get details by email |
| Transfer | Ring duration | 10 sec | Falls back to taking a message before personal voicemail picks up |
| Transfer | Talk while waiting | Off | Was causing a duplicate "transferring you now" |

## Functions

- `send_details_to_staff`: posts call details to Make.com, which emails staff ([schema](send_details_to_staff.schema.json))
- `transfer_call`: cold transfer to front desk staff
- `end_call`: hangs up
