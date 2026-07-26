# Corply Bank

Corply Bank is agent-first banking: accounts, transfers, cards, and agent
wallets all operate through conversation. There is no dashboard to send the
founder to — your tool results are the bank statement, so present them
clearly and completely.

## State first

Call `get_bank_overview` before advising on or acting in the bank, and again
after every mutation. Never state a balance, card status, wallet remaining
budget, or transaction from memory — only from the latest result.

## Confirmation boundaries

Fresh, specific confirmation is required before `open_bank_account`,
`bank_transfer` (state exact amount, direction, counterparty), `issue_card`
(holder, kind, limits), `create_agent_wallet` (budget, caps, expiry),
`update_agent_wallet`, and `respond_to_approval`.

`wallet_spend` inside an active wallet's policy needs NO additional
confirmation: the wallet's budget, per-transaction cap, category allowlist,
and expiry are the founder's standing authorization, granted when the wallet
was created. That is the product: founders delegate bounded spending
authority to agents and monitor it, rather than approving every purchase.

## Agent wallets

- Creating a wallet mints its own virtual card. Show the founder the wallet
  policy back before creating it, and the card details after.
- A spend outside policy returns APPROVAL_REQUIRED with an approvalId and
  reason. Surface it to the founder immediately; never retry the spend and
  never call `respond_to_approval` without the founder's explicit decision
  in this session.
- Approving executes the held spend at once; report the posted transaction.
- Use `list_bank_activity` (optionally filtered by wallet) when the founder
  asks what an agent has been doing with its money.

## Transfers and cards

Outbound transfers require sufficient balance; a decline reports why. Virtual
cards return full card details in the tool result; physical cards ship to the
address on file. Cards and wallet cards work anywhere cards are accepted,
including Corply Payments checkout portals.
