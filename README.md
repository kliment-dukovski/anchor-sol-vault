# Solana Vault

A native SOL vault built with Anchor for the Turbin3 Week 2 assignment. Each wallet can initialize its own vault, deposit lamports, withdraw a specified amount, and close the vault to recover the remaining balance.

## Instructions

| Instruction | What it does |
| --- | --- |
| `initialize` | Creates the vault state, funds the vault with its rent-exempt minimum, and saves both PDA bumps. |
| `deposit(amount)` | Transfers the specified number of lamports from the signing wallet into its vault. |
| `withdraw(amount)` | Transfers the specified number of lamports from the vault back to the signing wallet. The program signs for the vault PDA during the System Program CPI. |
| `close` | Transfers the vault's remaining lamports to the user and closes the state account, returning its lamports as well. |

## Accounts

- `vault_state` is a program-owned PDA derived from `b"state"` and the user's public key. It stores the two PDA bumps.
- `vault` is a System Program account at a PDA derived from `b"vault"` and the same public key. It holds the SOL.

## Build and test

Install Rust with rustup, Anchor CLI, and Solana's `cargo build-sbf` tool. The commands below were used with Anchor CLI 1.2.1 and `cargo-build-sbf` 4.1.0 (platform-tools v1.54).

Run these commands from the repository root:

```sh
cargo build-sbf --arch v0 \
  --manifest-path programs/q3_26_vault/Cargo.toml \
  --sbf-out-dir target/deploy
cargo test -p q3_26_vault
```

The integration test loads the compiled program into LiteSVM. Building with `--arch v0` resolved a program-loading error with this toolchain. LiteSVM runs locally without a separate validator.

The integration test initializes the vault, deposits 0.5 SOL, withdraws 0.1 SOL, and closes it. It checks the vault balance after each step and verifies that the state account is gone after closing.

## Credits

Based on the [Anchor vault starter](https://github.com/moses7054/anchor_vault_starter_q3_26) by moses7054.
