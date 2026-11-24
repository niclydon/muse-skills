# Verify the Sign-In Route Before Claiming a Web Task

Many web tasks fail not because the task is impossible, but because the
sign-in route was wrong. Check the route before you promise the task, not
after you are stuck on a login page.

## Verify the route before the promise

- Identify how the target site signs the user in before saying the task can
  be done or starting the attempt.
- Check which of these applies: the site uses a single sign-on provider
  (such as Google or Apple), it uses a saved username and password, or the
  user is already signed in somewhere you can work from.
- Check the payment route the same way when the task ends in a purchase, and
  name any step only the user can complete before the work starts.
- Treat the task as unverified until the sign-in route, and the payment route
  if there is one, have both been checked against the live site.

## Route single sign-on through the signed-in browser

- Route single sign-on work through the browser profile where the user is
  already signed in to that provider. That profile exists precisely to handle
  sign-on prompts, multi-factor checks, and provider blocks on unfamiliar
  sign-ins.
- Do not start a single sign-on task in a fresh browser session backed by a
  saved password. A saved password is not a sign-on provider session, and
  that route fails in predictable ways.
- Treat a login prompt, a bot check, or a stalled sign-in in the fresh
  browser as a routing signal to switch routes, not as a blocker to report
  and wait on.
- Be honest about what browser work returns: usually text reports, not a
  live view of the screen. A claim that sign-in succeeded rests on what the
  returned state actually shows, nothing more.

## Fall back before stalling

- Check for a command-line tool, an API, or a connected service that covers
  the same operation before concluding that a gated task cannot proceed.
- Prefer a tool that is already authenticated over a new browser sign-in,
  when the tool covers the operation.
- If a task was sent down a route that turns out to be wrong, stop it and
  restart it on the verified route instead of retrying the failed one.
- Hand the user a step only when it is genuinely theirs, such as a
  multi-factor approval only they can tap, and name that single step exactly.

## Report what was actually verified

- Report the sign-in method you found, the route you used, and the evidence
  that the signed-in state was reached.
- Report a completed booking or other gated action only from its
  confirmation or read-back, never from having reached the checkout or login
  page.
- Name what remains unverified when a route could not be checked, instead of
  presenting the task as ready.

## Adapting this to your setup

- Keep a short note of which browser profile holds your signed-in provider
  sessions, so the assistant routes sign-on work there without asking.
- List the sites you use most with their sign-in method (single sign-on,
  password, or already signed in) to make the route check a lookup instead
  of an investigation.
- Note any command-line tools or connected services that are already
  authenticated on your machines; they are often the fastest fallback.
- If you have no persistent signed-in browser profile, say so in your setup
  notes, and expect sign-on-heavy tasks to need a step from you at the start.
