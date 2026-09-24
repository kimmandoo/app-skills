---
name: mobile-gesture-interactions
description: Use when a mobile screen combines taps, drags, scrolling, pinching, selection, links, or system navigation and accidental gesture overlap causes wrong actions.
---

# Mobile gesture interactions

## Define ownership

1. Inventory every gesture and explicit control on the surface. For each touch target define the winning action, when it becomes committed, what cancels it, and what the parent view must not receive. Respect system back/swipe edges, accessibility navigation, and interactive child controls.
2. Establish a precedence table for target activation, text selection or links, drag/scroll, pinch/pan, long press, tap, and cancellation. The product's actual interaction design determines priority. Do not let a tap become an edit after motion crosses a drag threshold or after a second pointer starts a pinch; a child control must not trigger its parent's navigation.
3. Resolve coordinate systems explicitly: window insets and safe areas, viewport padding, content zoom/pan, orientation, and any image/document transform. If moving a selection, compute the total delta from the gesture's start state rather than accumulating transformed intermediate positions. Clamp the selected object while preserving its intended size; distinguish moving the content from moving an object on the content.
4. Preserve accessible alternatives for essential actions. A gesture-only back, page turn, selection, or reveal action excludes some users; provide named controls and a sufficient touch target for the supported platforms. Avoid overriding native text selection, links, or scrolling when those are part of the product's expected behavior.

## Implement and verify

Use one coordinated gesture arena/pointer stream or explicitly ordered recognizers, not independent handlers that each commit the same contact. Give each interaction a clear begin/update/commit/cancel boundary; group an entire drag or slider gesture into one undoable operation if the domain supports undo. A zoom, rotation, or resize should preserve the user's semantic selection/reading anchor, not store a transient zero-size layout as progress.

On the actual app, exercise tap versus drag, double-tap versus single-tap, drag versus pinch, nested scroll versus selection/link, interrupted/cancelled contact, system edge back, and long-press versus scroll on small and large/resized windows. Verify exactly one intended action, no accidental parent action, retained content position after reflow, and operability with screen reader or switch/keyboard alternatives where supported. Record a gesture trace and screen evidence; passing pointer-unit tests alone does not prove physical interaction.
