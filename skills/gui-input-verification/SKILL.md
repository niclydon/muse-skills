# Verify the Front Window Before GUI Input

Clicks and keystrokes go to whatever window happens to be in front. If you
have not just looked at the screen, you do not know where your input will
land. Look first, every time.

## Capture before you act

- Capture the target screen fresh, with your screen-capture tool, before the
  first input of a session.
- Read the capture itself rather than trusting a session label, a window
  title from an earlier turn, or a report that the machine is sitting at a
  known screen.
- Name the front window or field the capture actually shows before you send
  anything, and compare it with the window or field your next input assumes.

## Gate every input batch

- Send input only when the fresh capture shows the exact window and focus
  your clicks or keystrokes expect.
- Stop without typing when the capture shows a lock screen, a disk unlock
  prompt, a password field, or any window other than the intended one, and
  report what the capture shows instead.
- Treat commands and credentials as equally unsafe to type blind: keystrokes
  that land in a password or unlock field are both lost and exposed in the
  wrong place.
- Keep each input batch small, then capture again and confirm the screen
  moved the way the batch intended before sending the next batch.

## Recheck after every state change

- Capture again after a reboot, an unlock, an app launch or quit, a window
  switch, a dialog appearing, or any pause long enough for the screen to
  have changed on its own.
- Never reuse a capture from before such a change to justify input after it,
  however recent that capture feels.
- When a capture fails or returns a black or stale frame, say the screen
  state is unverified and do not send input until a working capture confirms
  it.

## Recover when the window was wrong

- Halt input immediately when a capture shows your earlier input landed
  somewhere unintended, and state plainly what was sent and where it landed.
- Do not compound the error with corrective keystrokes until a fresh capture
  shows the current state and what recovery would actually touch.
- Record the wrong-window incident in the work record for that session, so
  the next person or session driving the same screen sees it first.

## Prefer a non-GUI route when one exists

- Prefer an API, a command line, or a config file edit whenever one reaches
  the same result, and reserve driven GUI input for steps that genuinely
  have no other path.
- Ask the user to perform a step themselves only when the step is truly
  theirs, such as entering a password only they hold, and name the exact
  screen they should be looking at when they do it.

## Adapting this to your setup

- Note which screen-capture tool you have for each machine you drive, and how
  the assistant should call it, so capturing is a habit rather than a search.
- List any screens where input must never be typed blind in your setup, such
  as unlock prompts or banking windows, beyond the defaults named here.
- Decide how large an input batch may be before a re-capture is required;
  a few actions at a time is a good default.
- Keep a work record for remote-control sessions, even a simple dated note
  file, so wrong-window incidents are visible to the next session.
