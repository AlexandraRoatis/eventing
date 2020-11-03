# Event Consumer Interfaces

To enable delivery to multiple types of services, Knative Eventing defines two
generic interfaces that can be implemented by multiple Kubernetes resources,
namely `Addressable` and `Callable`.

## Addressable

`Addressable` resources are able to **receive** and **acknowledge** an event
delivered over a network transport (currently only HTTP is supported) to an
address defined in their `status.address.url` field. As a special case, the core
[Kubernetes Service object](https://v1-18.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#service-v1-core)
also fulfils the `Addressable` interface.

The `Addressable` object returns success when it has successfully handled the
event (for example, by committing it to stable storage). When used as an
`Addressable`, only the acknowledgement or return code is used to determine
whether the event was handled successfully. One example of an `Addressable` is a
[`Channel`](channel.md).

### Control Plane

An `Addressable` resource MUST expose a `status.address.url` field. The
_hostname_ value is a cluster-resolvable
[DNS](https://en.wikipedia.org/wiki/Domain_Name_System) name which is **capable
of receiving event deliveries**. `Addressable` resources may be referenced in
the `reply` section of a [`Subscription`](spec.md#kind-subscription), and by
other custom resources acting as an event [`Source`](sources.md).

### Data Plane

An `Addressable` resource **will only respond to requests with success or
failure**. Any payload (including a valid
[`CloudEvent`](https://github.com/cloudevents/spec)) returned to the sender will
be ignored. An `Addressable` **may receive the same event multiple times** even
if it previously indicated success.

---

## Callable

`Callable` objects are able to **receive** an event delivered over a network
transport (currently only HTTP is supported) and **transform** the event,
**returning 0 or 1 new events** in the response. These returned events may be
further processed in the same way that events from an external event source are
processed. One example of a `Callable` is a function. Note that all `Callable`
resources are `Addressable` (they accept an event and return a status code when
completed), but not all `Addressable` resources are `Callable`.

### Control Plane

A `Callable` resource MUST expose a `status.address.url` field (like
`Addressable`). The _hostname_ value is a cluster-resolvable
[DNS](https://en.wikipedia.org/wiki/Domain_Name_System) name which is **capable
of receiving event deliveries** and of **returning a resulting event** in the
reply. `Callable` resources may be referenced in the `subscriber` section of a
[`Subscription`](spec.md#kind-subscription).

<!-- TODO(evankanderson):

What other properties separate a callable from an Addressable. We have talked
about using an annotation like `eventing.knative.dev/returnType = any` to
represent the return type of the _Callable_.

--->

### Data Plane

The `Callable` resource receives one event and returns zero or a single event in
the response. A returned event is not required to be related to the received
event. The `Callable` should return a successful response if the event was
processed successfully.

The `Callable` is **not responsible for ensuring successful delivery** of any
received or returned event. It **may receive the same event multiple times**
even if it previously indicated success.

---

_Navigation_:

- [Motivation and Goals](motivation.md)
- [Resource Types Overview](overview.md)
- **Interface Contracts**
- [Object Model Specification](spec.md)
- [Channel Specification](channel.md)
- [Sources Specification](sources.md)
