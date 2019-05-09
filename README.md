# Feature Policy for freezing iframes.
An explainer to define ability to [freeze](https://wicg.github.io/page-lifecycle/spec.html) an iframe.

## Motivation

Suppose a page contains an iframe. It is possible that the embedding document
does not wish the iframe to consume CPU or network resources while not actively
displayed. Controlling the behavior between two frames requires active communication
and co-ordination between the frames. 

Co-ordination between frames can be difficult because the only way to communicate
between frames is via `postMessage`. So the each side has to co-ordiante
on an API that allows each side to indicate when they should restrict their
CPU usage.

## Abstract

To improve this situation we extend the [Page Lifecycle](https://wicg.github.io/page-lifecycle/spec.html)
such that individual frames can be frozen.

There are two situations in which frames can be frozen:
* Frame is scrolled out of viewport
* Frame is not rendered (ie: display: none)

Since frames can begin out of the viewport and it is desirable that they be ready
when scrolled into view frames will not be frozen until the `load` event is
dispatched to the revelant frame's document.

When a frame is frozen all media (webaudio, video, audio) will be paused
and begin playing again when a frame is resumed. These policies can be
controlled by the feature policy values.

Doing this based on a feature policy removes the requirement for
embedder to do complicated work. ie. The policy can be
set and the browser will control the freezing behaviour based
on those policies.

Frames when frozen will be dispatched the [freeze](https://wicg.github.io/page-lifecycle/spec.html#dom-document-onfreeze)
and [resume](https://wicg.github.io/page-lifecycle/spec.html#dom-document-onresume) events. If
documens wish to control pausing the media themselves it is suggested that they
do that in their freeze and resume events.

Same origin or different origin iframes should be treated the same according to this
policy. Same origin documents could present problems being directly scriptable from
the parent frame. However, since the policy is explicitly opt-in it is a negotiation
between a embedder and embeddee.

## Feature Policy

Introduce two feature policy values:
* `execution-while-out-of-viewport` indicates freezing frames that aren't in the viewport
* `execution-while-not-rendered` indicates freezing frames that aren't rendered

Media that was paused will automatically be resumed for
`execution-while-out-of-viewport` frames that become visible in the viewport.

Media that was paused will remain paused for `execution-while-not-rendered`
frames when they become rendered.

```
<iframe allow="execution-while-out-of-viewport 'none'">
<iframe allow="execution-while-not-rendered 'none'">
```
