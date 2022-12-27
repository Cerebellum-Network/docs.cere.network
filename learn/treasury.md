# Treasury

### Treasury

The Cere Network Treasury is a pool of funds derived from transaction fees, slashing, and staking inefficiencies. The Cere Network Treasury conforms to a 1 day budgeting spend period, which serves as a waiting period before any funds can be spent via Treasury proposals. Cere's Treasury is ultimately controlled by the Council, and how the funds are spent is up to the Council's majority judgment.

### Budgeting Period

Cere's default budgeting period is set to 24 days. This duration is subject to change via [governance ](governance.md)proposals. During the budgeting period, the funds held in the Treasury pool may be spent by submitting a spending proposal and will need to be approved by the Council. If the spending proposal is approved, the spending proposal then enters a spending queue. If the Treasury pool ends a budgeting period without spending all of its funds, it suffers a burn of a percentage of the pool's funds.

### Treasury Funding

The sources for funding the Treasury consist of a list of events and actors participating in the Cere Network:

* Slashing: When any slashing event occurs, the amount is sent to the Treasury with some reward going to the entity that reported the validator to be slashed. The reward is taken from the total slash amount and varies per occurrence and number of reporters.
* Transaction Fees: A portion of each block's transaction fees goes to the Treasury, with the remainder going to the block author.

### Treasury Proposals

The proposer has to deposit 5% of the requested disbursement amount as an anti-spam measure. This deposit is burned if the proposal is rejected, or refunded otherwise. The deposit fee percentage is subject to governance and may change over time.

To minimize storage on-chain, proposals need not include contextual information. Because of this, our Treasury proposals will be announced on one of our community forums to inform the community and explain in detail the proposal.

To create a proposal, you can navigate to the Cere Network Treasury dashboard [here](https://explorer.cere.network/#/treasury). From the dashboard, you can submit a proposal to a list of accounts known to the Cere Network. Once a proposal has been submitted, you will need to send the proposal to the Council to perform a vote. At this point, a Council member can create a motion to accept or reject the Treasury proposal. It is possible that one motion to accept and another motion to reject are both created. The threshold for accepting a Treasury proposal is at least three-fifths of the Council. On the other hand, the threshold for rejecting a proposal is at least one-half of the Council.
