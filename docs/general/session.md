<!--- Hugo front matter used to generate the website version of this page:
linkTitle: Session
--->

# Semantic conventions for session

**Status**: [Development][DocumentStatus]

This document defines semantic conventions to apply to client-side applications when tracking sessions.

Session is defined as the period of time encompassing all activities performed by the application and the actions
executed by the end user.

Consequently, a Session is represented as a collection of Logs, Events, and Spans emitted by the Client Application
throughout the Session's duration. Each Session is assigned a unique identifier, which is included as an attribute in
the Logs, Events, and Spans generated during the Session's lifecycle.

When a session reaches end of life, typically due to user inactivity or session timeout, a new session identifier
will be assigned. The previous session identifier may be provided by the instrumentation so that telemetry
backends can link the two sessions (see [Session Start Event](#event-sessionstart) below).

## Attributes

<!-- semconv session-id -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`session.id`](/docs/registry/attributes/session.md) | string | A unique id to identify a session. | `00112233-4455-6677-8899-aabbccddeeff` | `Opt-In` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`session.previous_id`](/docs/registry/attributes/session.md) | string | The previous `session.id` for this user, when known. | `00112233-4455-6677-8899-aabbccddeeff` | `Opt-In` | ![Development](https://img.shields.io/badge/-development-blue) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

## Session Events

### Event: `session.start`

<!-- semconv event.session.start -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

The event name MUST be `session.start`.

Indicates that a new session has been started, optionally linking to the prior session.

For instrumentation that tracks user behavior during user sessions, a `session.start` event MUST be emitted every time a session is created. When a new session is created as a continuation of a prior session, the `session.previous_id` SHOULD be included in the event. The values of `session.id` and `session.previous_id` MUST be different.
When the `session.start` event contains both `session.id` and `session.previous_id` fields, the event indicates that the previous session has ended. If the session ID in `session.previous_id` has not yet ended via explicit `session.end` event, then the consumer SHOULD treat this continuation event as semantically equivalent to `session.end(session.previous_id)` and `session.start(session.id)`.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`session.id`](/docs/registry/attributes/session.md) | string | The ID of the new session being started. | `00112233-4455-6677-8899-aabbccddeeff` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |
| [`session.previous_id`](/docs/registry/attributes/session.md) | string | The previous `session.id` for this user, when known. | `00112233-4455-6677-8899-aabbccddeeff` | `Conditionally Required` [1] | ![Development](https://img.shields.io/badge/-development-blue) |

**[1] `session.previous_id`:** If the new session is being created as a continuation of a previous session, the `session.previous_id` SHOULD be included in the event. The `session.id` and `session.previous_id` attributes MUST have different values.

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

### Event: `session.end`

<!-- semconv event.session.end -->
<!-- NOTE: THIS TEXT IS AUTOGENERATED. DO NOT EDIT BY HAND. -->
<!-- see templates/registry/markdown/snippet.md.j2 -->
<!-- prettier-ignore-start -->
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->

**Status:** ![Development](https://img.shields.io/badge/-development-blue)

The event name MUST be `session.end`.

Indicates that a session has ended.

For instrumentation that tracks user behavior during user sessions, a `session.end` event SHOULD be emitted every time a session ends. When a session ends and continues as a new session, this event SHOULD be emitted prior to the `session.start` event.

| Attribute  | Type | Description  | Examples  | [Requirement Level](https://opentelemetry.io/docs/specs/semconv/general/attribute-requirement-level/) | Stability |
|---|---|---|---|---|---|
| [`session.id`](/docs/registry/attributes/session.md) | string | The ID of the session being ended. | `00112233-4455-6677-8899-aabbccddeeff` | `Required` | ![Development](https://img.shields.io/badge/-development-blue) |

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- END AUTOGENERATED TEXT -->
<!-- endsemconv -->

[DocumentStatus]: https://opentelemetry.io/docs/specs/otel/document-status
