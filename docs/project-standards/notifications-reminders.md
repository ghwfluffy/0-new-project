# Notifications And Reminders

Use this standard when the project needs reminders, in-app notifications, scheduled prompts, or user-visible system messages.

## Model

Reminder rules should be separate from completion, compliance, or progress rules.

Applications should:

- model reminder sources such as domain entities, metrics, tasks, or system events
- generate durable notification records
- respect user timezone
- use source type and source ID to tie notifications to domain records
- track notification state
- keep the model extensible for email or other channels later

## States

Default notification states:

- pending
- delivered
- read
- dismissed
- expired
- failed

## Noise Control

Reminder behavior should be explicit and bounded.

Reminders should stop once the required action is completed. Repeated reminders should be constrained. Quiet hours, snooze, and channel preferences may be added when a project needs them.

## First Channel

Start with in-app notifications unless the project requires email, SMS, or push notifications.

External channels should use an outbox-style design once reliability matters.
