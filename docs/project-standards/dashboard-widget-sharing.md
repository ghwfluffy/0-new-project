# Dashboard, Widget, And Sharing

Use this pattern when the product needs saved presentation, dashboards, widgets, public sharing, or read-only external views.

## Dashboards And Widgets

Applications should:

- store dashboards as user-owned layout records
- store widgets as user-owned configuration records
- allow widgets to target one domain entity, many entities, a source-data stream, or an aggregate scope
- keep widget rendering reusable between app dashboards and public/share views
- save desktop and mobile layout/order separately when needed
- keep raw/source history inspectable independently of any current wrapper object

## Public Sharing

Public sharing must be explicit and token-based.

Share links should:

- use random public tokens or UUID-style public identifiers
- target a single shareable entity per link
- support expiration and revocation
- avoid exposing internal database identifiers
- be manageable by users through create/list/copy/inspect/revoke UI
- default to expiring unless the user deliberately chooses no expiration

Public pages must be read-only.

## Preview Rendering

Server-render public share pages or preview images when social/chat unfurling matters.

Preview rendering should avoid exposing private data beyond the explicitly shared target.
