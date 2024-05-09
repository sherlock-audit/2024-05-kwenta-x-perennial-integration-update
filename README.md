
# Kwenta x Perennial Integration Update contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
Currently deployed to Arbitrum and Base.
___

### Q: If you are integrating tokens, are you allowing only whitelisted tokens to work with the codebase or any complying with the standard? Are they assumed to have certain properties, e.g. be non-reentrant? Are there any types of <a href="https://github.com/d-xo/weird-erc20" target="_blank" rel="noopener noreferrer">weird tokens</a> you want to integrate?
Currently only DSU in the markets and USDC (Both USDC/USDC.e on Arbitrum, native USDC on other chains) is used by the extensions.

___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED? If these integrations are trusted, should auditors also assume they are always responsive, for example, are oracles trusted to provide non-stale information, or VRF providers to respond within a designated timeframe?
DSU - TRUSTED
Pyth (or other oracle providers) - TRUSTED
___

### Q: Are there any protocol roles? Please list them and provide whether they are TRUSTED or RESTRICTED, or provide a more comprehensive description of what a role can and can't do/impact.
Protocol admin: TRUSTED

Markets have Coordinators which can update parameters for that specific market - these coordinators have a large amount of flexibility within their own market but should not be able to adversely affect other markets or the overall protocol.
___

### Q: For permissioned functions, please list all checks and requirements that will be made before calling the function.
There are many types of permissioned functions - most are protected to only be called by certain callers (either other protocol contracts or the above mentioned protocol roles)

There are a few permissioned functions that can only be called by accounts being operated on, or by operators that the account has approved
___

### Q: Is the codebase expected to comply with any EIPs? Can there be/are there any deviations from the specification?
No

___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, arbitrage bots, etc.)?
Yes - there are keepers for oracle updates + settlements, liquidations, and order types

___

### Q: Are there any hardcoded values that you intend to change before (some) deployments?
Not currently
___

### Q: If the codebase is to be deployed on an L2, what should be the behavior of the protocol in case of sequencer issues (if applicable)? Should Sherlock assume that the Sequencer won't misbehave, including going offline?
Sequencer downtime is not handled and Perennial also does not provide grace periods for users to cure their positions when these systems do come back up. Sherlock should assume that the Sequencer won't misbehave, including going offline.
___

### Q: Should potential issues, like broken assumptions about function behavior, be reported if they could pose risks in future integrations, even if they might not be an issue in the context of the scope? If yes, can you elaborate on properties/invariants that should hold?
Yes - function behavior is defined in the natspec comments and if they pose integration risk we would like to be aware of that.
___

### Q: Please discuss any design choices you made.
Nothing stands out - Perennial is a complex codebase so we encourage auditors to thoroughly read documentation and comments, as well as ask us any questions about design decisions that they might have.
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
As stated above - market coordinators can do many things within their markets which could adversely affect user funds within those markets. However, they should not be able to affect other markets

Flywheel being down due to external downtime - sequencer downtime does not have special case handling. Perennial also does not provide grace periods for users to cure their positions when these systems do come back up
___

### Q: We will report issues where the core protocol functionality is inaccessible for at least 7 days. Would you like to override this value?
We'd like to know of any issues which leave the functionality inaccessible for at least 24 hours
___

### Q: Please provide links to previous audits (if any).
https://github.com/equilibria-xyz/perennial-v2/tree/main/audits

___

### Q: Please list any relevant protocol resources.
V2 Docs: https://docs.perennial.finance/
V2 Mechanism 1-pager: https://docs.google.com/document/d/1f-V_byFYkJdJAHMXxN2NiiDqysYhoqKzZXteee8BuIQ/edit
___

### Q: Additional audit information.
The base of this audit should be this commit: https://github.com/equilibria-xyz/perennial-v2/pull/263/commits/04be089de24097d87f576b45c64f559b858c0f15

The goal of this mini-update is to allow an account to approve operators to act on their behalf in the MultiInvoker extension. 

This is not fully gasless, as it still requires the user to send a transaction to approve the operator to act on their behalf

USDC or DSU is always pulled from the account (not msg.sender). The msg.sender is expected to provide the ETH update fee required for COMMIT actions
___



# Audit scope


[perennial-v2 @ ce68eed261a39341705f949d8a5bceebab182ce8](https://github.com/equilibria-xyz/perennial-v2/tree/ce68eed261a39341705f949d8a5bceebab182ce8)
- [perennial-v2/packages/perennial-extensions/contracts/MultiInvoker.sol](perennial-v2/packages/perennial-extensions/contracts/MultiInvoker.sol)


