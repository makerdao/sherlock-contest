# Sherlock Contest Preparations


## Scope
https://jetstreamgg.notion.site/Maker-Endgame-Launch-Sherlock-Audit-Contest-Scope-641baee4028548ccbb3783f2278c3215

## Known Issues:

Note that disclaimers here can override other details or disclaimers in the Readmes or source code.

- Known issues that have been discussed publicly (e.g. in pull requests) are out of scope.
- Issues/comments from the Readmes should be taken into account.
- All previous audits content should be taken into account for known issues.

## Severity Definitions:

We are building on top of the current Sherlock severity definitions apart from changes explicitly mentioned (for example see about functionality breaking). However, we are giving guidance to the words that describe loss.

Sherlock's Medium definition:

> Causes a loss of funds but requires certain external conditions or specific states, or a loss is highly constrained. The losses must exceed small, finite amount of funds, and any amount relevant based on the precision or significance of the loss.
> 

"a loss is highly constrained". As a guideline for this contest, a highly constrained loss is a loss of up to 5% of the affected party (both protocol and user).

"The losses must exceed small, finite amount of funds". As a guideline for this contest, small, finite amount of funds are 0.5% of the affected party (both protocol and user).

With this clarification

- Any loss smaller than 0.5% can never be Medium
- Any loss between 0.5% and 5% loss can be Medium at most
- Any issue larger than 5% can be Medium or High (depending on constraints)
- If a single attack can cause a 0.01% loss but can be replayed indefinitely (assuming little to no costs), it will be perceived as a 100% loss. Note that in most modules governance can step in within hours (aka Mom contracts), or otherwise if needed  plus the governance pause delay to halt the system. This should be taken into account when determining if replaying indefinitely is possible.

For protocol losses, it must be demonstrated that the losses exceed the above percentage assuming protocol reserves of 100m+ (rough estimation of POL = SB + SBE).
For user losses, it must demonstrate those losses with the user's 10k+ of value locked/vulnerable as part of the attack.

In the lockstake case, the maximal locked MKR is assumed at $250M and the maximal line is assumed as 100m.
A user in the lockstake case is a single urn.

For the SNST case, the maximal locked NST amount is assumed as 1B DAI.

## Rules:

The protocol chooses to override at least the following from https://docs.sherlock.xyz/audits/judging/judging. Other changes may also be communicated through the competition website or elsewhere.

- "Breaks core contract functionality, rendering the contract useless or leading to loss of funds."
**Breaking any kind of functionality is not a medium or high severity issue for this competition, only loss of funds.**

- "The protocol team can use the README (and only the README) to define language that indicates the codebase's restrictions and/or expected functionality."
**Any public material should be valid as known issues for this competition.**

- "Issues that break these statements, irrespective of whether the impact is low/unknown, will be assigned Medium severity."
**Even if an intended functionality is broken but there is no material loss of funds then it is not a medium or high issue for this competition.**

 - "Example: The README states "Admin can only call XYZ function once" but the code allows the Admin to call XYZ function twice; this is a valid Medium"
**This shouldn't be a valid medium if there is no loss of funds.**

- "EIP Compliance: For issues related to EIP compliance, the protocol & codebase must show that there are important external integrations that would require strong compliance with the EIP's implemented in the code. The EIP must be in regular use or in the final state for EIP implementation issues to be considered valid"
**There is no commitment with regards to EIP compliance. On top of that, any loss of funds incurred in an integrating contract is out of scope**. 

- "User input validation: User input validation to prevent user mistakes is not considered a valid issue. However, if a user input could result in a major protocol malfunction or significant loss of funds could be a valid high."
**Any user mistakes resulting in their own funds being lost is out of scope.**

## General Disclaimers
- Only regular tokens that are examined by Maker should be incorporated in the system. For example, they are assumed to:
    - revert on failure
    - be non-reentrant
    - not support hooks
    - have standard decimals
    - not implement fee on transfer
    - not have rebase logic.
- Note that the lack of any weird feature from the above list of token features does not mean it is supported, these are just examples.
- ERC-20 tokens - issues stemming from upgradability of these tokens (e.g USDC) are out of scope.
- Inaccurate specifications are out of scope if they do not lead to direct loss of funds.
- Lack of documentation is out of scope.
- Gas inefficiencies are out of scope.
- EIP incompatibilities are out of scope if they do not lead to direct loss of funds.
- Copyright and licensing issues are out of scope.
- The contracts are assumed to be deployed on mainnet.
- Unused code is not considered a submittable issue.
- Redunant checked arithmetic it not a submittable issue.
- Wrong/unexpected revert strings (or lack of such) is out-of-scope.
- Discrepancies in handling of zero/non-actionable input amounts (no-op, revert, etc..) are out of scope.
- Issues in other Makerdao repositories are out of scope, even in case they are relevant to interactions with the contracts in scope. For example - dss, the Chief contract.
- As the initiation of the endgame components is not considered time-critical, any permissionless action that can cause the spells to revert is not considered a medium or high severity issue (as another spell can be crafted). It can be assumed that no other mission-critical tasks are included in the same spell.
- Issues where there is damage to the protocol but attack cost exceeds damage caused/funds stolen significantly are considered low severity.
- Some third parties are trusted - Uniswap, Circle (USDC).
- Potential user errors are out of scope.
- Tests and mock contracts are excluded.
- Deployment trust model - deployed contracts are assumed to be checked as part of Maker's spellcrafting process. Deployment of the contracts is assumed to be done with special care taken that all contracts have been deployed correctly. It is assumed that the initcode, bytecode, traces and storage (e.g. mappings) are checked for unintended entries, calls or similar. This is especially crucial for any value stored in a mapping array or similar (e.g. could break access control, could lead to stealing of funds). Additionally, it is checked that no allowance is given to unexpected addresses.
- Minor rounding errors leading to missing some fees, getting more fee share compared to someone else, or fees locked in the protocol are considered low severity at best.
- DoS issues where no funds would be locked or protocol functionality is blocked but one one can still withdraw should be considered low severity at best.
- Oracles are trusted to provide non-stale and correct information.
- NGT market price is assumed to reflect MKR market price (and vice-versa) scaled by the conversion factor.
- Aggregation of dust amounts in contracts is disregarded.
- The ability to increase `line` / `Line` right away by an additional `gap` amount when setting the autoline is known.
- `uint32` is assumed enough for storing timestamps.
- `uint96` is generally assumed suitable for storing token amounts.
- `spotter.par()` is assumed to stay as `RAY`.
- Handling of token donations to the protocol is not guranteed in any module. A lack of such handling is not considered an issue.
- Some of the modules have backward compatability getters like `dai()`, `daiJoin()`, etc.. The lack of such in other modules should not be treated as an issue.
- Oracle value updates outside of the delta governance considered when choosing the parameters of the system are out of scope.
This includes trivial issues in the main stable coin system, roughly speaking e.g. when an extreme price drop exceeds the collateral's liquidation ratio, so that a position would become unhealthy after the update and a liquidation wouldn’t raise sufficient funds leading to bad debt.  Constructions describing how “loss of funds” (bad debt) could occur in such a scenarios are out of scope.
- Issues present in the current Maker protocol deployment which are disclosed to the Maker team privately or publicly through the ImmuneFi bug bounty program during the Sherlock public audit contest window cannot be used in the execution pathways when reporting issues for the new codebases in the Sherlock public audit contest

## Governance Related:
- Governance configurations are assumed to be set with extreme care. Lack of sanity check issues are not viable submissions.
- Governance is assumed as non-malicious with regards to all matters.
- To be extra clear about governance being trusted, it is assumed that `wards` in all contracts are fully trusted, as well as other priveleged roles.
- Even when possible, governance is assumed to not confiscate/manipulate specific user funds/positions without a good reason. This means that reports that claim that governance can take specific users funds are not considered issues.
- Subdao proxies, facilitators and permissioned keepers are assumed fully trusted.
- Emergency shutdown is assumed to not happen.
- There is assumed to be governance delay of at least 16 hours.
- Gathering enough MKR for governance actions is assumed to be possible within a few hours.
- The option of avoiding future governance actions towards NST and SNST holders by moving to vat.dai is known.
- Grouping of specific governance actions if needed (or permissionless actions with governance ones), are assumed to be implemented in the spell or dss-exec-lib level.
- Chainlog maintenance is assumed to be possible also after components have been added or removed. Therefore missing/extra/wrong/inconsistent Chainlog values are assumed a non-issue.
- Certain models have Mom contracts to allow governance to pefrom actions without gov delay. Choosing which contracts or actions should have this ability is assumed to be done by the protocol and pointing out a lack of it for a certain module is out of scope.
- Any user mistake resulting in their own funds being lost is out of scope.


## Specific Repos Known Issues and Disclaimers:

### univ2-pool-migrator

* The deployment of nst and ngt should be done before the spell, as well as creating the univ2 pool through the factory. The migration is supposed to run at the spell, where nst and ngt are init. Since until that point ngt is not in circulation the pool can not be deposited to, and hence its total supply would still be 0.
* The migration code will be called in the same spell as the flapper replacement.

### vote-delegate

* The need for expiration logic is not considered important enough anymore, social arguments against that are considered out of scope.
* The factory's getAddress function will return an address even though the delegate contract does not exist. This is expected.
* Anything that could be considered a bug and is existing in the current [VoteDelegateFactory](https://github.com/makerdao/vote-delegate/blob/c2345b78376d5b0bb24749a97f82fe9171b53394/src/VoteDelegateFactory.sol) and [VoteDelegate](https://github.com/makerdao/vote-delegate/blob/c2345b78376d5b0bb24749a97f82fe9171b53394/src/VoteDelegate.sol) is out of scope.

### lockstake

* Any issue mentioned here (or in the recursively linked material) is out of scope - https://security.makerdao.com/liquidations-2.0
The fact that this resource is specifically mentioned does not mean that any other public information can not be regarded as a source of truth.
* Anything that could be considered a bug and is existing in the original [Clipper](https://github.com/makerdao/dss/blob/fa4f6630afb0624d04a003e920b0d71a00331d98/src/clip.sol) is not valid.
* Delaying liquidations in the order of tens of minutes is assumed valid, there is already a big delay in Maker's oracle design.
* `wipeAll` and `wipe` do not drip because it is actually not convenient for the user to do a drip call on wipping. Then, if we force the drip, we are incentivizing users to repay directly to the vat (which is possible) instead of using the engine for that.
We are mimicking the old proxy actions behaviour, where we drip for drawing, as otherwise the user can lose money, but not forcing the drip on wiping so users actually use this function.
* By the time lockstake goes live an OSM is assumed to be onboarded for MKR. Currently in Maker the MKR price is only used for the smart burn engine (dss-flappers), so doesn't yet need an OSM.
* Issues related to creating a large amount of auctions are assumed to be mitigated by setting dust amount carefully.
* The formulas and calculations in the Readme's `Exit Fee on Liquidation` may not be accurate and are given for rough illustrative purposes.
* On certain market conditions such as extreme price movements some of the exit fee may not be burned on liquidations. This is a known attribute of the system.
* As getting liquidated is considered a state that should generally be avoided, it is expected that in such cases the urn owner becomes limited in various ways. For example, it cannot delegate or stake during liquidations and in some situations, it can take more time than the urn owner expects due to more liquidations or liquidations taking a lot of time. Other limitations may apply and are not considered an issue unless significant funds are lost permanently.
* Using the "stopped" states in the lockstake clipper is assumed to be used by wards in an exterme emergency. It is a known risk that some of the system attributes and functionality may not hold afterwards, including risking user and system funds. This includes also LSE special functionality (allowing exit of auctions leftover, not burning fees, delegating, staking, etc..).
* As the Maker protocol has the ability to mint MKR and NGT tokens and also to migrate the Lockstake engine, any situation of locked user MKR/NGT should not be regarded as a high severity issue.
* Using yank() in the lockstake clipper is assumed to only happen as part of a shutdown procedure. Since this is out of scope, it is assumed not to happen.
* As with other collaterals, "tip" and "chip" are assumed to be chosen very carefuly, while taking into account the dust parameter and with having in mind incentive farming risks.

### dss-flappers
* Inefficiencies due to configurations or oracle frequencies/pricing are out of scope.
* Slow/Unavoidable delay of withdrawing/redeeming the protocol funds in case of an emergency is known.
* Possible sandwich attacks and MEV extraction scenarios are known issues. Maker's risk unit is assumed to be setting parameters while having these in mind.
* The fact that the SBE funds are not taken into account in protocol accounting or for flops auction triggering is known.
* It is expected that governance sets parameters carefully during the module's life cycle, including maintaining splitter.hop as the same value as the farm's reward duration.

### endgame-toolkit
* Any issue that exists in the original non-Maker [Syntetix staking rewards](https://github.com/Synthetixio/synthetix/blob/5e9096ac4aea6c4249828f1e8b95e3fb9be231f8/contracts/StakingRewards.sol) contract is out of scope.
* SDAO domain separator might be out of date when the token name and/or symbol changes.
