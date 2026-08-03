# Outreach Engine (internal)

Internal prospecting engine for Edbound. **Not client facing.**

Turns LinkedIn post engagers into reviewed, personalised outreach:

1. Konnector scrapes a post's likers/commenters into a `post_detail` lead list.
2. This tool pulls that list, and blocks anyone already in an active outreach campaign.
3. Each person is filed as **Partner / Agency**, **B2B company**, **Membership body**,
   **Needs a look**, or **Excluded** — with the matching rule shown next to them.
4. You approve, edit the copy, then push into a Konnector campaign.

Nothing is ever sent by this tool. Push only adds leads to a Konnector campaign, and that
campaign sends only when it is active.

## Notes

- Backend: Supabase edge function `engage` (project `qypevxpscdhdrzlelolt`), API docs at
  `/functions/v1/engage/api/meta`.
- Classification rules live in `app_config.engagement_rules_v2` and are tunable with one
  SQL update — no redeploy, no code change.
- Dedupe keys on the LinkedIn URN (`profile_id`). Profile URL formats differ between
  Konnector surfaces and cannot be compared directly.
- Classification deliberately ignores profile "about" text: bios name coaches as clients,
  which would misfile agencies as coaches.
- Only the Supabase anon key is in this repo. It grants nothing on its own — every table is
  RLS-on with no policies and the API authorises the signed-in user server-side.
