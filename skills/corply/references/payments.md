# Corply Payments

Corply Payments is the hosted way a company gets paid: a checkout portal at
a public URL, shareable magic links, and machine-readable checkout that AI
agents can pay without a browser. It is distinct from the payment-route
onboarding tools (`create_payment_route_draft`, `get_payment_pipeline_status`,
…), which handle provider merchant onboarding — read this file for portals,
links, and revenue monitoring.

## Operating rules

- `create_payment_portal` returns the portal URL and its agentEndpoint.
  Present both as markdown links; the URL is customer-facing as-is.
- `create_payment_link` makes a magic link — fixed amount for invoices, no
  amount to let the payer choose. Single-use links close after one payment.
- Revenue truth comes only from `list_portal_payments` (charges, totals, the
  agent share of revenue). Never assert that a link was paid without it.
- Portals and links are reversible to create; no extra confirmation is
  needed. Confirm before disabling or changing anything a customer already
  has in hand.
- Every portal is agent-payable by default: agents discover requirements at
  the agentEndpoint (HTTP 402) and pay with any card, including a Corply
  agent-wallet card. Mention this when the founder asks how agents buy.
