---
name: morning-task-triage
description: Triages open Answering Agent follow-up tasks, showing what is open and overdue, who owns each task, and what to do first. Use when someone asks what's on the list today, what's overdue, which tasks to work first, or wants a morning task rundown.
---

# Morning task triage

This skill uses the Answering Agent tools. It only reads data unless the person asks for a change.

## Steps

1. Call `get_account` for today's date and the team members with their ids.
2. Call `run_report` with `measures: ["open_tasks"]`, `range: "today"`, and `group_by: "task_age"`. Call it again with `group_by: "team_member"`.
3. Call `search_tasks` with `status: "not_done"` and `limit: 50`. While `next_page` is not null, call again with `page`, up to 3 pages. Add `assigned_to_me: true` when the person asks only about their own tasks.
4. Call `get_task` on the first 5 tasks of the do-first list to read their full descriptions.

## Overdue and order

Tasks have no due date. Count a task as overdue when it was created 1 day ago or more, unless the person names another cutoff. Say which cutoff you used.

Order the do-first list by priority, high first, and within each priority by age, oldest first. Put `on_hold` tasks last.

## Changing a task

Change nothing unless the person asks. Before each change, say what will change and wait for a yes. Then call `update_task` with `task_id`, the new `status` or `assigned_to` (a user id from `get_account`), and `expected_status` set to the status you last saw. A conflict error means a teammate changed the task. Read it again with `get_task` and ask again.

## When data is missing

- No open tasks: say the list is clear.
- A task with no assignee is Unassigned.
- If `get_task` says a task was merged, use the task it names.
- If a tool says the data is not available, the account's role can't see it. Say so.

## Output

Example with made-up data:

**Open tasks, Oct 14: 18 open, 7 overdue (open 1 day or more)**

Do first:
1. Double charge on membership. High, 3 days old, Unassigned. Jamie Rivera wants the extra charge refunded. Next step: check the charge and call back. (link)
2. Lost sunglasses at the vacuums. Medium, 2 days old, Dana Lee. Next step: check lost and found and text the customer. (link)

By owner: Unassigned 6 (2 overdue), Dana Lee 7 (3 overdue), Luis Ortiz 5 (2 overdue).
By age: under 1 day 11, 1 to 3 days 4, 3 to 7 days 2, 7 days or more 1.

Each task line gives the title, priority, age, owner, customer, the next step from the description, and the task's `url`. Offer to assign the unassigned tasks.
