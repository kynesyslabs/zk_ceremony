# Ceremony report

The ceremony is closed. The final keys are in `final/` and are the only ones
the Demos node and SDK use for identity proofs.

## Result

| File | SHA-256 |
|------|---------|
| `final/identity_with_merkle_final.zkey` | `591c50c67cb669c09874b0a9cb544c678594b0fd4fee612539c86fb0a97bec56` |
| `final/verification_key_merkle.json` | `e02d5858493ffe323d2eb78c67ea02dd1d0f80662f68f702fa8cdda356af41fd` |
| `final/identity_with_merkle.wasm` | `b55958f21696a3095e280cb222756c5fee6beea935aaf5d30c4c906de9d4ade3` |

## How it was made

1. Phase 1: Hermez `powersOfTau28_hez_final_14.ptau` (blake2b in `BEACON.md`).
2. Phase 2: the initial key and 19 contributions, `keys/ceremony_0000.zkey`
   to `keys/ceremony_0019.zkey`, with attestations in `attestations/`.
3. Beacon: announced in `BEACON.md` (PR #28, opened 2026-11-10 10:45 UTC)
   before drand mainnet round 6629366 was published at 12:00 UTC. The round is
   saved as `final/drand-6629366.json`; its randomness
   `f8c94d61e783316938c0b7f811272a919d9b8db030fb93edb1d4e3edfacb808d` is
   SHA-256 of the round signature, and the drand.sh and Cloudflare relays
   returned the same value.

   ```
   snarkjs zkey beacon keys/ceremony_0019.zkey final/identity_with_merkle_final.zkey f8c94d61e783316938c0b7f811272a919d9b8db030fb93edb1d4e3edfacb808d 10 -n="drand mainnet round 6629366"
   ```

   Running it again gives a byte-identical file.

## Checks

- `snarkjs zkey verify` (snarkjs 0.7.2) of the final key against the circuit
  (r1cs sha256 `aacae445…fde0`, as recorded in `ceremony_state.json`) and the
  Phase 1 file: `ZKey Ok!`, 19 contributions and the beacon. Output in
  `final/zkey_verify.log`.
- The wasm is circom 2.2.2's build of `identity_with_merkle.circom` from node
  `stabilisation`, byte-identical to the one served at
  `files.demos.sh/zk-circuits/v1/`.
- `vk_gamma_2` differs from `vk_delta_2`.
- End to end: a proof made with the SDK's snarkjs package and input encoding
  from the final zkey passes the node's `ProofVerifier`; the same proof with a
  changed Merkle root or context fails, and a proof from the previous
  development key fails.

## What this replaces

Before this, the SDK proved with a single-party development key from
November 2025 and the node verified with yet another one, so no SDK proof
could pass the node (node#1009). Both now use the key above.
