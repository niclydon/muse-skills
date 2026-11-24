# Verify against production-shaped fixtures

## Inventory the production shape first

- List the constraints, grants, column types and response shapes the production system actually enforces before building any test fixture or scratch store.

- Read that shape from the live system or its applied migrations, never from the original design or the first migration that created the table.

- Note every place the verification environment is weaker than production, such as a missing check constraint, a broader grant or a fixture row no real writer can produce.

## Mirror the shape in the test setup

- Build fixtures only from shapes the real wire or the real writers can produce, and reject a fixture that models an impossible message or row even when it makes a test pass.

- Apply the same constraints and the same least-privilege grants in the test store as production carries, so a seed that violates a production rule fails at setup instead of passing silently.

- Seed configuration and reference rows from the current production definitions, and refresh them when a later migration changes the shape.

- Record any deliberate difference between the test shape and production in the work record, with the reason and the risk it leaves uncovered.

## Exercise every claimed path

- Run each read and write path the verification claims to cover at least once against the mirrored shape, including search and list paths that a happy-path build can skip.

- Treat a path that was never executed as unverified, and say so in the report instead of letting a green suite imply coverage it does not have.

- When a test disagrees with production behavior, check the fixture against the real wire shape before changing the code, because the test can be the wrong one.

## Report parity gaps as findings

- Report each gap between the test shape and production as its own finding, naming the defect it could hide.

- Open a ticket in your issue tracker for a gap that needs a schema or fixture change, and keep the verification claim scoped to what the mirrored shape actually proved.

## Alternatives for better results

- Run one harmless read against production itself when a mirrored fixture and the code still disagree, because the live shape outranks both.

- Prefer a disposable copy of the production schema over a hand-built scratch schema when the system offers one, because hand-built copies drift first.

## Adapting this to your setup

- Identify where your production shape actually lives: applied migrations, the live database schema, or the real API's responses, and read it from there.
- List the grants your tests run under and compare them with the least-privilege grants production uses; that difference is the most common hidden gap.
- Decide where parity gaps get recorded (your issue tracker, or the work record) before the first run, so findings have a home.
- Prerequisites: read access to the production schema or a faithful schema export, and a disposable test store you can rebuild.
