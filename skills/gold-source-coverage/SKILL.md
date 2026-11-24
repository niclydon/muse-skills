# Census gold table source coverage

Many data pipelines organize tables into layers: raw data as it arrived (sometimes called bronze), cleaned and standardized tables (silver), and unified, ready-to-answer tables (gold). This skill audits one gold table: does it actually cover every source it claims to unify?

## Census the sources in the domain

- Start from the raw and silver inventories for the domain and list every ingested source that carries records of the kind the gold table claims to unify.

- Derive the source list from ingestion records and table names outside the gold build itself, because a census taken from the gold union definition can only find the sources that build already knows about.

- Record for each source its row or entity count, its newest content date and the identifier that joins its records to the gold table.

- Write down any source the domain deliberately does not cover as a declared boundary with its reason, so an exclusion reads as a decision rather than an omission.

## Compare gold against the census

- Count how many records from each censused source reach the gold table and compute the covered share for every source.

- Flag a source as stranded when it holds substantial records and the gold table holds none of them or only a named subset such as two providers out of many.

- Check for duplicate canonical forms inside gold, such as the same merchant, provider or place appearing under two spellings or two cases, and count both forms before calling the union unified.

- Check caps and silent limits in the gold build, such as a fixed row ceiling on one source, and record any cap as a declared boundary with its value.

- Treat a matching total across the whole domain as no proof of coverage, because one overcounted source can hide a stranded one in the sum.

## Set floors and test coverage

- Set a coverage floor per source before judging the result, using an exact match where one authoritative source defines the domain and a high floor such as 0.98 where a transform legitimately drops a small share.

- Fail the check loudly when any source falls below its floor, naming the source, its raw count, its gold count and its covered share.

- Make the census diff the acceptance test for any new or rebuilt gold union in the domain, and rerun it after any ingestion source is added.

- Pair the census with a per-stage content-date freshness check (see the pipeline-coverage-freshness skill if you use it) when a shortfall could be either a stranded source or a stalled stage, because the two failures need different fixes.

## Present the gaps

- Present the result as a ranked list of gaps, largest stranded share first, with the source name, its raw count, its gold count and its declared boundary or floor breach.

- State plainly which sources are fully covered, so the user can see what the gold table can be trusted to answer.

- Open one ticket in your tracker per stranded source or duplicate form, with the census numbers as the evidence and the coverage floor as the acceptance test.

- Never answer a domain question from a gold table alone while a censused source sits below its floor, and name the missing source in the answer when the question cannot wait.

## Alternatives for better results

- Run the census domain by domain in the order the user asks questions about those domains, because one pass across every domain at once buries the gaps that matter now.

- Spot check one known record from each stranded source by its own identifier before filing, because a join key mismatch and a missing union branch look alike in counts alone.

## Adapting this to your setup

- The method works with any layer naming (raw/clean/unified, bronze/silver/gold, staging/marts): what matters is that the census is built from the source inventory, never from the unified table's own definition.
- If some sources have no reliable join identifier to the gold table, record that as a finding in itself rather than estimating their coverage.
- Set floors per domain before the first run and keep them in the ticket or runbook, so later runs are judged against the same bar.
- If you have no tracker, present the ranked gap list as the deliverable and skip the ticket step.
