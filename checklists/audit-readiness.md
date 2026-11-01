# Solana audit readiness checklist (12 things to prepare before the audit starts)

An audit is billed in engineer-days. Every day an auditor spends setting up your project or guessing what a function should do is a day not spent looking for bugs. These 12 items are what a well-prepared team hands over before the start date.

## Code
- [ ] Freeze a tagged commit. Keep new features on a separate branch until the report is final.
- [ ] Finish the features in scope. Cut unlaunched features from scope.
- [ ] Clean the build: compiles from a clean checkout with pinned Rust, Solana and Anchor versions, and no failing tests.

## Documentation
- [ ] Write a short spec: for each instruction, what it does, who may call it, which accounts it touches and what it must never allow.
- [ ] List your invariants (for example: total supply equals the sum of balances, only the authority can move funds, debt never exceeds collateral).
- [ ] Draw the trust model: which keys exist, who holds them, what the upgrade authority can do, which off-chain services the program relies on.

## Tests
- [ ] Ship tests that run in one command.
- [ ] Cover the failure paths: wrong signer, wrong owner, wrong account, zero and maximum amounts.
- [ ] Add fuzzing or property tests where money moves.

## Process
- [ ] Agree scope and exclusions in writing: programs, commit, what is out of scope (frontend, keepers, oracles, key management) and whether the fix review is included.
- [ ] Name a developer on call who answers auditor questions within hours.
- [ ] Plan the fix window: reserve your engineers for the days after the first report.

Do the spec and invariants first. Writing them often exposes a design problem before anyone is paid to find it.

Full article with FAQ: https://orivions.com/blog/solana-audit-readiness-checklist
