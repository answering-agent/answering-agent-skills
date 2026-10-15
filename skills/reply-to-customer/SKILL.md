---
name: reply-to-customer
description: Drafts a short text or email reply to a customer on an existing Answering Agent conversation, shows the exact message, and sends it only after the person says yes. Use when someone asks to text, email, reply to, follow up with, or message a customer.
---

# Reply to a customer

This skill uses the Answering Agent tools. The customer receives what you send, so one rule comes first.

Never call `reply_to_conversation` until the person has seen the exact message and answered with a clear yes, such as "yes", "send it", or "go ahead". The request to write a reply is not a yes. A question or an edit is not a yes. After you show a draft, end your turn and wait.

## Steps

1. Call `search_conversations` with the customer's name, phone, or email as `query` and `limit: 5`. If several match, list each one's customer, channel, date, and title, and ask which. If none match, say that this tool can only reply on an existing conversation.
2. Call `get_conversation` with its `conversation_id`. Read the summary, the latest messages in `written`, and the notes.
3. Pick the channel. A text works on a call with a phone number or when `channels` includes `text`. An email works only when `channels` includes `email`. If both work and the person didn't say, ask.
4. Draft the message. Keep a text under 320 characters and an email to a few short sentences. Use the customer's first name. Name the business in a text. Leave the sign-off out of an email, because the email adds one. Use only facts from the conversation or from the person. Don't promise a refund, credit, booking, or price unless the person confirms it happened or was approved. Add a link only if the person gave it.
5. Show the draft and stop:

   > To: Jordan Hale, by text
   > Message: Hi Jordan, thanks for calling Sunny Side Car Wash. Your $10 discount starts on your next billing date. Reply here with any questions.
   >
   > Send this text? Say yes to send it, or tell me what to change.

6. If the person changes anything, show the new draft and ask again.
7. After a clear yes, call `reply_to_conversation` with `conversation_id`, `channel`, `body` exactly as shown, and an `idempotency_key` such as `reply-184532-a7f3`. Use a new key for each new message, because a reused key sends nothing and returns the earlier message. If the call times out or says "The request failed. Try again.", retry once with the same key, so the customer gets one message.
8. Report the result from `delivery_status`. For `queued` or `sent`, say it is on its way. For `delivered`, say it arrived. For `failed`, show `error` if there is one and say the customer did not get it. For `unknown`, say you couldn't confirm delivery.

## Errors

Every error below means nothing was sent.

- If the conversation can't get a text or an email, offer the other channel, show the draft again, and ask again.
- If there were too many replies, wait the seconds the error gives, then ask before you try again.
- If the account can't message the customer, its role doesn't allow replies. Say so.

For several customers, show one draft and get one yes per customer. Never send in bulk.
