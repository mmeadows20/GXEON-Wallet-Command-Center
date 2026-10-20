# Bounty Objective: Money Truth: reconcile Stripe LIVE orders and prove first external buyer checkout→credits→delivery
Target: xpex-systems-ai/GXEON-Wallet-Command-Center Issue #57
URL: https://github.com/xpex-systems-ai/GXEON-Wallet-Command-Center/issues/57

## Issue Description
## GXEON first paid customer — Money Truth / Stripe audit (2026-10-08)

**Objective:** demonstrate a third-party purchase resulting in provider-confirmed BRL settlement, credited balance, verified execution, and delivered result, while preserving zero-spend/no-self-purchase policy.

### Evidence from production (GET /api/integration-status, observed 2026-10-08)
- `stripeEnvironment=LIVE`, `stripeConfigured=true`, `webhookConfigured=true`, `liveWebhookConfigured=true`, `firestoreConnected=true`. These flags reflect runtime configuration, **not Stripe provider verification**.
- Internal snapshot: `successfulPayments=0`, `pendingPayments=2`, `stripeNetRevenue=R$0.00`, `creditsSold=0`, `creditsConsumed=0`, `jobsDelivered=0`, `buyers=0`.
- Exactly two entries counted as pending from local `orders` with state `CHECKOUT_CREATED` (2026-09-28 and 2026-10-02); **not evidence of Stripe charges awaiting settlement**.
- A separate order has local `PAYMENT_SUCCEEDED` state and one `checkout.session.completed` event record (2026-09-27) but the money-truth aggregate remains **zero real confirmed payments**. Investigate metadata/livemode/payment_status/currency/amount matching and provider state before treating this as revenue or a bug. The event and internal state alone do not establish money received.
- USDC machine settlement remains 0. `x402` is not treasury-enabled in the current `.well-known/gxeon-agent.json`.
- The public MCP is read-only discovery; authenticated execution requires buyer key and prepaid settled credits.

### Independent provider reconciliation gates
- [ ] If authorized Stripe connector is accessible, cross-check `checkout.session`, `payment_intent`, `charge`, `balance_transaction`, disputes/refunds and `livemode` for the local event. Do not expose PII or secrets.
- [ ] Compare account's actual payouts/available Stripe balance with local events without silently rewriting records.
- [ ] Reconcile orders vs paid webhook events and credit ledger with idempotency/replay tests; be explicit why local PAYMENT_SUCCEEDED does not count.
- [ ] Confirm verified webhook signature / amount/currency / metadata and fail closed on unsupported events.
- [ ] Verify authenticated buyer onboarding, payment Checkout, webhook credit award and consumption **in test mode** before any customer transaction.
- [ ] Document permitted demo and independent buyer activation; do not buy packs from the owner's funds.
- [ ] Add public sales explanation and sample JSON validation request/results; no fabricated testimonials or sales stats.
- [ ] Track first external buyer request → provider-verified paid transaction → credits → completed job → delivery as a single evidence chain.
- [ ] Confirm pricing/taxes/Stripe fees in account before statements about margins or payouts.

**Rules:** no simulated revenue, no self-purchases, no real charges or emails without explicit authorization; no trading/transfers/USDC signing. `THE EXECUTOR DOES NOT APPROVE ITS OWN DELIVERY.`

**Related:** #56 official Base wallet/MCP PR, `src/agent-economy/billingCatalog.ts`, `api/integration-status.ts`, `/api/v1/mcp?view=public-market`.


## Instructions for Kuro (Autonomous Solver):
1. Use skills: `systematic-debugging` and `code-review-and-quality`.
2. Inspect the repository structure, locate relevant files, and understand the bug/feature.
3. Apply the minimal, robust fix or implementation.
4. Run any existing test suites or linters in the workspace.
5. Ensure the fix adheres to the project's existing code style.
6. When done, output a summary of files modified and verification results.
