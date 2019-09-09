# API to query Service Worker Client state
An explainer to define ability to query the Service Worker client lifecycle state.

## Motivation

Service Workers do not have any information for determining if a client is
[frozen](https://wicg.github.io/page-lifecycle/#frozenness-state).

Two use cases exist:
* A Service Worker may continuously postMessage to a client that is frozen. The message payloads will stack up, effectively
leaking memory causing a premature discard of the client.
* Being able to see frozen clients is desirable for ServiceWorkers that handle push notifications and preferentially invoke
Client.focus() instead of using Clients.openWindow() to avoid tab proliferation.

## Changes

Since the [Service Worker](https://w3c.github.io/ServiceWorker/) specification does not depend on
[Page Lifecycle](https://wicg.github.io/page-lifecycle/#serviceworker-mod) and not all browsers implement that
specification [patched changes](https://wicg.github.io/page-lifecycle/#serviceworker-mod) are available.

Primarily a [ClientLifecycleState](https://wicg.github.io/page-lifecycle/#enumdef-clientlifecyclestate) was
introduced to indicate the state of the client at the time the client was created. (ie. snapshot).



