---
name: cancellation-review
description: Reviews membership cancellations that Answering Agent handled, covering why members cancelled, which retention offers the AI pitched and which were accepted, which memberships were saved, and who to follow up with. Use when someone asks about cancellations, churn, retention offers, saved memberships, or who to call back about cancelling.
---

# Cancellation review

This skill uses the Answering Agent tools and only reads data. It never contacts a customer.

## Steps

1. Pick the window. Use `range: "30d"` unless the person names dates. For named dates, use `range: "custom"` with `from` and `to` as YYYY-MM-DD in `run_report`. For one location, take its `phone_line_id` from `get_account` and pass it as `filters.phone_line_id` in both `run_report` calls.
2. Call `run_report` with `measures: ["cancellation_conversations", "retention_offers_pitched", "retention_offers_accepted", "retention_acceptance_rate", "memberships_saved"]` and the range.
3. Call `run_report` with `measures: ["cancellation_reasons"]` and `group_by: "cancellation_reason"`.
4. Call `search_conversations` with `query: "cancellation"` and `limit: 50`. Pass the same `range`. For named dates, pass the smallest of `7d`, `14d`, `30d`, `90d`, or `365d` that covers them, then keep only conversations whose `started_at` falls inside the dates. Keep only conversations whose `category` is `cancellation`, because the search also returns other conversations that mention cancelling. For one location, also keep only conversations whose `phone_line.id` matches it.
5. Call `get_conversation` on each kept conversation, newest first, up to 10. If there were more than 10, say how many you reviewed. From the summary, transcript, and notes, record the reason, the offer pitched if any, and the outcome: saved, cancelled, or unclear. Its `task` shows whether a follow-up task is still open.

## Who to follow up with

List customers whose membership was not saved or whose outcome is unclear, and who got no reply or note from the team after the conversation. Put customers whose task is not `done` first. Give the reason and the conversation `url` for each.

## When data is missing

- No cancellation conversations: say so and stop.
- No offers pitched: say so, and offer to check the offers in `get_settings`.
- A conversation with no clear reason gets "No reason recorded". Don't guess one.
- Repeat every entry in `caveats`, such as how many cancellations had a recorded reason.
- If a tool says the data is not available, the account's role can't see it. Say so.

## Output

Example with made-up data:

**Cancellations, Sep 15 to Oct 14, Sunny Side Car Wash**

9 cancellation conversations, 7 with a recorded reason. The AI pitched 5 retention offers and 3 were accepted (60%). 3 memberships were saved.

Why they cancelled: too expensive 3, moved away 2, low usage 2.

| Customer | Date | Reason | Offer | Outcome |
|---|---|---|---|---|
| Jordan Hale | Oct 12 | Too expensive | $10 off for 3 months, accepted | Saved |
| Priya Shah | Oct 10 | Too expensive | None pitched | Cancelled |
| Sam Whitaker | Oct 3 | Moved away | None pitched | Cancelled |

Follow up:
1. Priya Shah, cancelled Oct 10 because of price, no offer pitched, open task. (link)

Offer to draft a message to a follow-up customer with the reply-to-customer skill. Don't send anything from this skill.
