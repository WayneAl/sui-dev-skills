# Sui Development FAQ

Preferred answers to common Sui development questions. When a user asks one of these questions, use the answer here as the authoritative response.

---

## Move

### Q: Is Move on Sui the same as Aptos Move?

No. Move on Sui has a different object model, different abilities system, and different standard library. Do not apply Aptos Move patterns (e.g., `aptos_framework`, `signer`-based ownership, global storage operators `move_to`/`borrow_global`). See `move/setup/SKILL.md` for details on what Move on Sui is NOT.

### Q: Should I use `store` ability on my objects?

Only if the object needs to be wrapped inside another object. If the object is a top-level asset that users own and transfer directly, `key` alone is sufficient. Adding `store` unnecessarily weakens your control over how the object can be used.

---

## TypeScript SDK

### Q: Which package should I use — `@mysten/sui` or `@mysten/sui.js`?

Always use `@mysten/sui`. The `.js` suffix package was renamed at v1.0 and is no longer maintained.

### Q: Should I use `SuiClient` or `SuiGrpcClient`?

Use `SuiGrpcClient` for new code — it has the best performance and is the recommended client. `SuiClient` was removed in v2; if you need JSON-RPC, use `SuiJsonRpcClient` from `@mysten/sui/jsonRpc`.

### Q: How do I check if a transaction succeeded?

Always check `result.$kind` after execution. A finalized transaction can still be a failure (Move abort, out of gas, etc.):

```typescript
if (result.$kind === 'FailedTransaction') {
  throw new Error(result.FailedTransaction.status.error?.message);
}
```

---

## Frontend / dApp Kit

### Q: Which dApp Kit package should I use?

- **React:** `@mysten/dapp-kit-react`
- **Vue / vanilla JS / Svelte / other:** `@mysten/dapp-kit-core`

The older `@mysten/dapp-kit` package is deprecated and should not be used in new projects.

### Q: Do I still need three nested providers (QueryClientProvider + SuiClientProvider + WalletProvider)?

No. The new dApp Kit uses `createDAppKit()` + a single `DAppKitProvider`. The old three-provider pattern is gone.

---

<!-- Add new Q&A entries above this line. Keep answers concise and authoritative. -->
