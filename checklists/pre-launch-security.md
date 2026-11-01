# Solana pre-launch security checklist (15 checks)

An audit is necessary, but it is not a launch plan. Many incidents have nothing to do with a clever exploit in the program: a leaked upgrade key, a compromised frontend, an unaudited last-minute change, or nobody noticing a drain for hours. These 15 checks cover what usually goes wrong around the code. Assign a name to each line: a check without an owner is a check nobody did.

## Keys and authorities
- [ ] 1. Move the program upgrade authority to a multisig (signers on separate devices, never the development agency).
- [ ] 2. Decide your upgrade policy in public: upgradeable, behind a timelock or immutable.
- [ ] 3. Review token mint and freeze authorities. Revoke what you do not need, document what you keep and why.
- [ ] 4. Audit every admin instruction (fees, pauses, parameters, withdrawals): right signer, controlled by the multisig.

## Code and deployment
- [ ] 5. Deploy exactly the audited commit. Anything changed after the audit is unaudited code.
- [ ] 6. Publish a verifiable build so anyone can confirm the on-chain program matches your public source.
- [ ] 7. Test failure paths: wrong signer, wrong owner, duplicated accounts, zero and maximum amounts.
- [ ] 8. Rehearse the launch on devnet with production settings, including multisig steps.
- [ ] 9. Plan account migrations before you need them: reserve space or version your accounts.

## Operations
- [ ] 10. Set up monitoring and alerts for unusual outflows, failed transaction spikes, admin instructions and authority changes.
- [ ] 11. Use more than one RPC provider, with a fallback and rate limits.
- [ ] 12. Protect the frontend and domain: registrar lock, two-factor authentication, restricted deploy access, pinned third-party scripts.
- [ ] 13. Write an incident response plan: who can pause, how they reach each other, who talks to users, which partners to call.
- [ ] 14. Launch a bug bounty sized relative to the funds at risk.
- [ ] 15. Cap risk at launch with deposit caps or rate limits, and raise them as confidence grows.

## Verify that deployed code matches the audited code
Build the audited commit and compare it with the program on chain using `solana-verify get-program-hash` and `solana-verify verify-from-repo`. Once remote verification succeeds, explorers such as Solana Explorer, SolanaFM and Solscan show the program as verified.

Full article with FAQ: https://orivions.com/blog/solana-pre-launch-security-checklist
