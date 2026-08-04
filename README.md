# Outreach Engine (internal)

Internal prospecting and outreach control surface. **Not client facing.**

Two surfaces:

**LinkedIn** — turns people who engaged with a post into reviewed, personalised outreach.
Anyone already in an active campaign is blocked automatically. Each person is filed as
Partner / Agency, B2B company, Membership body, or Needs a look, with the matching rule
shown next to them. You approve, edit the copy, then hand them to a campaign.

**Cold emails** — the team's email templates. Pick one, pick a campaign, and either load
the copy in or load it and start sending.

Nothing is ever sent by this tool directly, and a kill switch must be explicitly enabled
before anything can be handed to a sending system at all.

## Notes

- Backend: Supabase edge functions, self-describing at `/api/meta`.
- Classification rules live in config and are tunable with one SQL update — no redeploy.
- Deduplication keys on the LinkedIn URN, not the profile URL: URL formats differ between
  sources and cannot be compared directly.
- Classification deliberately ignores profile "about" text: bios name coaches as clients,
  which would misfile agencies as coaches.
- Only the Supabase anon key is in this repo. It grants nothing on its own — every table is
  RLS-on with no policies and the API authorises the signed-in user server-side.
- Vendor names are deliberately absent from all user-facing copy, including error strings.
