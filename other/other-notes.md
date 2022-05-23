# Other Notes

* Validators should only run the Polkadot binary, and they should not listen on any port other than the configured p2p port.
* Validators should run on bare-metal machines, as opposed to VMs. This will prevent some of the availability issues with cloud providers, along with potential attacks from other VMs on the same hardware. The provisioning of the validator machine should be automated and defined in code. This code should be kept in private version control, reviewed, audited, and tested.
* Session keys should be generated and provided in a secure way.
* Polkadot should be started at boot and restart if stopped for any reason (supervisor process).
* Polkadot should run as a non-root user.
* Must have infrastructure that protects the validator's signing keys so that an attacker cannot take control and commit slashable behavior.
* Must be high availability.

Session keys used by a validator should always be isolated to just a single node. Replicating session keys across multiple nodes could lead to equivocation slashes, or soon to parachain validity slashes which can make you lose 100% of your staked funds.

The good news is that 100% uptime of your validator is not really needed, as it has some buffer within eras in order to go offline for a little while and upgrade.\
