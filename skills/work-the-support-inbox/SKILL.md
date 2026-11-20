---
name: work-the-support-inbox
description: Go through the customer support conversations waiting on the team in UserJot, summarize them, and reply, leave a note, assign, or resolve. Use when asked who is waiting on support, to catch up on the inbox, to answer a customer, or to turn an answer into a help article.
---

# Work the support inbox

UserJot keeps customer conversations from the chat widget and from email in one inbox. This skill goes through the ones waiting on the team and acts on them with the UserJot tools.

What the person asks for comes first. If they want a summary only, do not reply, resolve, or assign anything.

## 1. Find who you are acting as

Replies, notes, status changes, and assignments are recorded under a team member, passed as `actor`. Call `listMembers` with `type` set to `owner`, and again with `admin`, to see the team. If the person has not said who they are and more than one teammate comes back, ask which one to act as. Do not guess.

## 2. List what is waiting

Call `listConversations` with `inbox_state` set to `needs_team`. These are open conversations where the customer is waiting on a person. `ai_handling` means UserJot's assistant is still answering, and `waiting_on_customer` means the team already replied.

If the list is empty, say so. A workspace that has not turned Conversations on has no conversations.

## 3. Read before answering

For each conversation you will act on, call `listConversationTimeline`. It returns messages, internal notes, and handoffs, oldest first. Read the whole thread, including notes from teammates, before drafting anything.

To learn who the customer is, use `getMember` with the participant's id, and `getCompany` if they belong to one.

## 4. Check the help center

Before writing an answer about how the product works, call `searchHelpArticles` with the customer's question. If an article covers it, base the reply on that article and link it. An article with `team` visibility is internal: use it to inform the reply, and do not link or quote it to a customer.

## 5. Reply, note, assign, or resolve

Show the person the draft and wait for a yes before sending. A reply is emailed to the customer and cannot be taken back.

- `createConversationReply` sends a public reply as the actor. Replying to a resolved conversation reopens it.
- `createConversationNote` leaves an internal note that the customer never sees. Use it for context a teammate will need.
- `updateConversationAssignee` gives the conversation to a teammate, or clears the assignee with `null`.
- `updateConversationStatus` with `resolved` closes it once the customer has their answer.

Write replies the way the team already writes in the thread: same language as the customer, short, and specific to what they asked. Do not promise dates, refunds, or features that nobody on the team has confirmed in the thread.

## 6. Turn a repeated answer into an article

When the same question has come up more than once and `searchHelpArticles` found nothing, offer to draft a help article. `createHelpArticle` creates a draft from markdown; leave `publish` off so the person can review it in UserJot first.

## When you are done

Say what you did per conversation: replied, noted, assigned, resolved, or left alone and why. List anything still waiting on a decision from the person.
