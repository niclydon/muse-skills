# For the Muse reading this

A person has pointed you at this repo and wants to take some of it into their own Muse setup. This file is your instructions. The human-facing overview is in README.md; the menu is in catalog.yaml.

## Step 0: Clone the repo first

This repo is a git repo, and the steps below copy folders out of it, so browsing its pages on the website is not enough. Before anything else, clone it and work from that checkout:

```
git clone https://github.com/niclydon/muse-skills.git
cd muse-skills
```

Run every later step from inside that checkout. If git is genuinely unavailable in your environment, download the archive from the GitHub page (Code, then Download ZIP), unpack it, and work from the unpacked folder instead.

## What this repo is

A collection of skills, patterns, and templates built by one Muse user for their own setup, generalized so another setup can use them. Nothing here is a product. Every item stands on its own; there is no framework to install and no required order, except where `pairs_with` in the catalog says two items work well together.

## Step 1: Present the menu

1. Read catalog.yaml in full.
2. Present it to your user as a menu, grouped by the catalog's `group` field. For each item give: the title, the one-line `summary`, what it `needs`, and the `effort` in plain words (drop-in means it works as written; light adaptation means a short setup conversation; setup project means you will be building something together, not copying a file).
3. Point out the `starter_set` from the catalog as the recommended first install, and say why: small, no connectors needed, useful immediately.
4. Ask your user to pick. Accept names, numbers, a group ("all of Shipping code"), or "the starter set". Do not install anything before they pick, and do not install items they did not pick as a bonus.

## Step 2: Install each picked skill

For each skill the user picked:

1. Copy the item's folder from `skills/<id>/` in this repo into your user's custom skills folder (`~/workspace/custom-skills/<id>/`), with both SKILL.md and skill.yaml.
2. Read the "Adapting this to your setup" section at the end of that SKILL.md. It lists what needs customizing: names, tools, folders, thresholds, vocabulary.
3. Resolve each customization from what you already know about your user and their setup. Ask about the ones you genuinely cannot settle, grouped into one short set of questions per skill, not a drip of single questions.
4. Apply the edits to the installed copy, never to this repo's copy.
5. Verify the install: confirm the folder and both files exist, and check whether your skill search now finds the skill. Report the result in the summary (step 4), including anything you could not verify.

## Step 3: Handle patterns and templates differently

Patterns are not files to copy; they are ways of working to set up together.

- For a pattern (journal-and-morning-paper, standing-orders, drafts-gate): read its README with your user, agree on the local shape (cadence, file locations, which chats take part), then create the files and any scheduled jobs it needs. Scheduled jobs and anything that posts publicly need your user's explicit approval for that specific setup, as usual.
- For the persona template (templates/persona): copy the three template files to your user's home directory only if they do not already have files with those names; if they do, offer the guide's structure as additions instead of overwriting. Fill the placeholders from a short conversation with your user. Never invent personal facts to fill a placeholder; leave it bracketed and note it as still to do.

## Step 4: Close with a summary

Tell your user, in one message:

- What was installed, by name.
- What you customized for their setup.
- What is still placeholder or needs their input later.
- One suggestion for a first use of each installed skill, in their own terms.

## Rules

- The user's picks are the scope. Suggesting one addition in the summary is fine; installing one is not.
- These skills were written for a different person's setup and then generalized. If an instruction in a skill conflicts with your user's own standing instructions or files, your user's version wins, and note the conflict in the summary.
- Some skills reference connectors (Gmail, Plaid, GitHub). If your user does not have one connected, install the skill anyway only if it still makes sense without it, and say what will not work until it is connected.
- Keep the license and attribution: this repo is MIT licensed (see LICENSE). If your user builds on these skills in something they publish, a credit line back to this repo is the whole obligation beyond the license itself.
