# Final beacon

The ceremony closes after contribution 19. A public random beacon is applied on top of it, so the final key does not depend only on the contributors' secrets and no one could choose it in advance.

## Input

- Last contribution: `keys/ceremony_0019.zkey`, sha256 `7cbbbb45c5f05bb766e724a52816ddb63e2498a2875e54bb71642fcd333b8cc8` (the attestation hash of contribution 19).
- Circuit: `identity_with_merkle`, r1cs sha256 `aacae445b302be54d85268a212a1d231a9b6665693b18a4e794d98636df4fde0` (the `r1csHash` in `ceremony_state.json`), compiled from node `stabilisation` with circom 2.2.2.
- Phase 1: Hermez `powersOfTau28_hez_final_14.ptau`, blake2b `eeefbcf7c3803b523c94112023c7ff89558f9b8e0cf5d6cdcba3ade60f168af4a181c9c21774b94fbae6c90411995f7d854d02ebd93fb66043dbb06f17a831c1` (as listed in the snarkjs README).
- `snarkjs zkey verify` (snarkjs 0.7.2) on these three files reports `ZKey Ok!` with all 19 contributions.

## Beacon

- Source: drand mainnet, chain hash `8990e7a9aaed2ffed73dbd7092123d6f289930540d7651336225dc172e51b2ce`.
- Round: **6629366**, published at 2026-11-10 12:00:00 UTC. This file is committed before that round exists.
- Value: the round's `randomness` field (32 bytes, hex), from `https://api.drand.sh/8990e7a9aaed2ffed73dbd7092123d6f289930540d7651336225dc172e51b2ce/public/6629366`.
- Iterations: 2^10.

```
snarkjs zkey beacon keys/ceremony_0019.zkey keys/identity_with_merkle_final.zkey <randomness> 10 -n="drand mainnet round 6629366"
snarkjs zkey verify identity_with_merkle.r1cs powersOfTau28_hez_final_14.ptau keys/identity_with_merkle_final.zkey
snarkjs zkey export verificationkey keys/identity_with_merkle_final.zkey keys/verification_key_merkle.json
```

Anyone can repeat these commands and must get the same final key and verification key.
