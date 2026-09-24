# GridLab bilingual landing and Telegram attribution contract

Status: prepared on the feature branch; not live.

## Language routing

- `/` — English, the default landing.
- `/es.html` — Spanish version with a switch back to English.
- `/en.html` — compatibility redirect to the English root.
- Switching languages preserves the video and UTM/click attribution parameters.

## Tagged link for the first English YouTube video

```
https://gridlab.academy/?video_id=GL-V01&lang=en&utm_source=youtube&utm_medium=organic&utm_campaign=GL-V01
```

The English root sets the payload language to `en`; `/es.html` sets it to `es`. Direct visits fall back to source `direct`, medium `organic`, and campaign `evergreen`. The CTA placement supplies the content field. The landing itself does not send third-party analytics events; funnel milestones are expected to be recorded by the Telegram bot.

## Telegram start payload

Exactly eight underscore-delimited fields:

```
gl1_{video_id}_{lang}_{source}_{medium}_{campaign}_{content}_{click_id}
```

Example: `gl1_GL-V01_en_youtube_organic_GL-V01_hero-prima_a1b2c3d4`

| Field | Meaning | Maximum |
|---|---|---:|
| `gl1` | Schema version | 3 |
| `video_id` | Video/content identifier; `direct` if absent | 11 |
| `lang` | Landing language | 2 |
| `source` | UTM source or YouTube referrer fallback | 7 |
| `medium` | UTM medium or `organic` | 7 |
| `campaign` | UTM campaign or `evergreen` | 9 |
| `content` | CTA placement | 10 |
| `click_id` | Supplied id or per-session random id | 8 |

Values are limited to ASCII letters, digits, and hyphens; underscores are reserved as separators. The maximum complete payload is exactly 64 ASCII characters. No personal information is placed in the payload.

## Current bot evidence and verification gate

A project changelog dated 2026-09-21 reports that the VPS bot persists landing attribution and funnel milestones in Google Sheets, and that a controlled pre-payment test recorded `bot_start`, `offer_view`, and `invoice_created`. This is historical project evidence, not a fresh check of the current VPS.

Before release, re-check the live bot's `/start` parser and storage against both `en` and `es` payloads. Confirm the current landing deployment source/branch, then verify the deployed root defaults to English, the language toggle preserves tags, and each CTA reaches the correct bot payload. Do not merge/deploy or change the YouTube description until the owner approves the release step. A successful landing-link test does not prove payment or access behavior.

## Release checklist

- Review the English default and Spanish toggle on desktop and mobile.
- Verify both language routes and attribution carry-over in a browser.
- Verify current bot parsing/logging on the VPS.
- Confirm whether merging to `main` publishes automatically.
- After release approval, add the tagged root URL to the private GL-V01 YouTube description and verify the recorded attribution.
