---
name: weekly-front-desk-report
description: Builds a weekly report of the calls, texts, website chats, and emails that Answering Agent handled, with the AI-resolved rate, the top reasons a person had to follow up, open tasks, and a chart. Use when someone asks for a weekly report, last week's numbers, a front desk recap, or how the AI did this week.
---

# Weekly front desk report

This skill uses the Answering Agent tools and only reads data.

## Steps

1. Pick the window. For "last week", use `range: "custom"` with `from` as last Monday and `to` as last Sunday, as YYYY-MM-DD. Use `range: "7d"` only for "the past 7 days". For other dates, use `custom`.
2. Call `get_account` for the business name. For one location, take its `phone_line_id` from `phone_lines` and pass it as `filters.phone_line_id` in every `run_report` call below.
3. Call `run_report` with `measures: ["conversations", "ai_resolved_conversations", "ai_resolved_rate", "asked_for_person_conversations", "tasks_created"]` and the range.
4. Call `run_report` with `measures: ["conversations"]`, `group_by: "channel"`, and `include_percent_of_total: true`. The rows give calls, texts, chats, and emails. Its `chart` field holds the chart data.
5. Call `run_report` with `measures: ["task_categories"]`, `group_by: "category"`, and `top_n: 5`. These are the top reasons a person had to follow up. The Form category means a website form, not a conversation.
6. Call `run_report` with `measures: ["open_tasks"]`, `range: "today"`, and `group_by: "team_member"`.

Make one call per step and add no other measures.

## When data is missing

- If `status` is `unsupported_combination`, retry once with an alternative the result lists.
- If conversations is 0, say there were no conversations in the window and leave out the rate.
- Under the table, show the `caveats` that explain a number you show. Leave out caveats about terms the report doesn't use.
- Use only the numbers the tools return. Don't work out a rate yourself.
- If a tool says the data is not available, the account's role can't see it. Say so.

AI-resolved means nobody on the team had to step in. It doesn't mean the customer got what they wanted. Say this if someone questions the rate.

## Output

Use the dates from `range.from` and `range.to`. Example with made-up data:

**Sunny Side Car Wash, Oct 5 to Oct 11**

| Measure | Value |
|---|---|
| Conversations | 212 |
| Calls, texts, chats, emails | 140, 38, 29, 5 |
| AI-resolved | 164 (77.4%) |
| Asked for a person | 12 |
| Tasks created | 41 |

Top reasons a person followed up: Billing 14, Scheduling 9, Lost items 6, Sales 5, Account update 4.

Open tasks now: 18. Unassigned 6, Dana Lee 7, Luis Ortiz 5.

Then draw a bar chart of conversations by channel from the `chart` in step 4. If you can't draw charts, skip it, because the table has the numbers. End with one or two sentences on what stands out, using only the numbers above.
