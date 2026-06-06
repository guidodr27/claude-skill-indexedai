# Example 3 — Multiple sites in parallel

## Command

```
/indexedai compare stripe.com and paddle.com webhook events
```

## Discovery phase (parallel)

```
Checking for MCP servers…
→ GET https://stripe.com/.well-known/mcp.json    [parallel]
→ GET https://paddle.com/.well-known/mcp.json   [parallel]

stripe.com  → MCP found ✓
paddle.com  → 404, using WebFetch

→ POST stripe MCP  tools/call  search("webhook events")   [parallel]
→ WebFetch https://developer.paddle.com/webhooks          [parallel]
```

## Expected output

```
[MCP: stripe.com]
Stripe webhook events: payment_intent.succeeded,
payment_intent.payment_failed, customer.subscription.created…

[Web: paddle.com]
Paddle webhook events: transaction.completed,
subscription.created, subscription.canceled…

Comparison: Both cover subscription lifecycle events.
Stripe has ~250 event types; Paddle has ~40 but covers the core flows.
```

## Notes

- Both discoveries run simultaneously — no sequential waiting
- Each source tagged independently
- Claude synthesizes a comparison after fetching both
