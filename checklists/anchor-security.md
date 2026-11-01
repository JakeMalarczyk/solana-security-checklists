# Anchor security checklist (16 checks before your audit)

Anchor turns account validation into declarations, which removes whole classes of bugs. But Anchor only checks what you tell it to check. Run these 16 checks on any Anchor program before sending it to an auditor. This list is preparation for an audit, not a replacement for one.

## Accounts and signers
- [ ] Use `Signer` for every account that must authorize the instruction. A plain account or `UncheckedAccount` is not checked for a signature.
- [ ] Tie stored authorities to the signer with `has_one` or an explicit constraint, for example `#[account(mut, has_one = authority)]`.
- [ ] Justify every `UncheckedAccount` and `AccountInfo`: validate owner, key or data yourself.
- [ ] Prevent the same account being passed twice into two mutable slots, for example `from.key() != to.key()`.

## PDAs and initialization
- [ ] Validate PDAs with `seeds` and `bump` in the account constraint, and store the canonical bump.
- [ ] Design seeds so users cannot collide: include the user or the parent account.
- [ ] Avoid `init_if_needed` unless you have a reason, and check it cannot reset balances or owners.
- [ ] Close accounts with the `close` constraint instead of draining lamports by hand.

## Tokens and CPIs
- [ ] Constrain token accounts by mint and authority (`token::mint`, `token::authority` or associated token constraints).
- [ ] Use `Program<>` or `Interface<>` types for programs you call, so the program ID is checked.
- [ ] Decide on Token-2022 support explicitly and review extensions such as transfer fees and transfer hooks.
- [ ] Scope PDA signer seeds tightly: separate PDAs for vaults, authorities and configuration.

## Math, upgrades and testing
- [ ] Use checked arithmetic and keep `overflow-checks = true` in the release profile. Round in the protocol's favour.
- [ ] Validate `remaining_accounts` by hand: Anchor does no validation on them.
- [ ] Plan account layout changes before upgrades: version accounts or reserve space, use `realloc` carefully.
- [ ] Test the failure paths (wrong signer, wrong mint, duplicated accounts, zero and maximum amounts) and fuzz the rest, for example with Trident.

## What to do with the results
Fix what you find, write down the invariants your program must always keep, and send both to your auditor with a frozen commit. Before mainnet, also go through the pre-launch checklist in this repository.

Full article with FAQ: https://orivions.com/blog/anchor-security-checklist
