# Outreach Engine (internal)

Internal prospecting and outreach control surface. **Not client facing.**

Three surfaces:

**LinkedIn** — turns people who engaged with a post into reviewed, personalised outreach.
Anyone already in an active campaign is blocked automatically. Each person is filed as
Partner / Agency, B2B company, Membership body, or Needs a look, with the matching rule
shown next to them. You approve, edit the copy, then hand them to a campaign.

**Post engagement** — the trigger is somebody touching one of our posts. Reactions and
comments are both brought in, each person is filed into a group, and their copy is written.
A 1st-degree connection is messaged directly; everybody else gets a connection request with
a note first. In parallel, an address is looked up for them and, if one comes back, they are
added to an email campaign the team already wrote and approved. Where a group has campaigns
chosen for it, the hourly check can do all of this without anyone asking.

**Cold emails** — the team's email templates. Pick one, pick a campaign, and either load
the copy in or load it and start sending.

Nothing is ever sent by this tool directly, and a kill switch must be explicitly enabled
before anything can be handed to a sending system at all. The two channels have separate
switches, and sending without a human in the loop has a third.

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
- A post's engagement is split across two lists upstream. The comments one reports its size
  in a different field and zero in the obvious one, so a list's real size is the sum of both.
  Reading the obvious field alone silently ignores every commenter on the post.
- On a comments list the comment text arrives in the same field that carries the reaction
  type on a reactions list. Same field, two meanings.
- The connection degree arrives formatted two different ways depending on which list the
  person came from. Which channel action they get is derived from it, so it is normalised on
  the way in.
- An employer sometimes arrives as a URL rather than a name. It is merged into copy a real
  person reads, so a URL is converted back to a name or dropped.
- No email address is ever guessed or pattern-matched. Without one, the person is
  LinkedIn-only and the screen says so.
- Outreach copy never names which post someone engaged with: the only label available is
  whatever the operator typed as the list name. A commenter is quoted their own words.
