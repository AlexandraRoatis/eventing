# Motivation

The goal of Knative Eventing is to define composable primitives to enable
late-binding of event sources and event consumers.

<!-- TODO(n3wscott): [Why late-binding] -->

Knative Eventing is designed around the following goals:

1. Knative Eventing services are loosely coupled. These services can be
   developed and deployed independently on and across a variety of platforms
   (for example Kubernetes, VMs, SaaS or FaaS).

1. Event producers and event consumers are independent. Any producer (or
   source), can generate events before there are active event consumers that are
   listening. Any event consumer can express interest in an event or class of
   events, before there are producers that are creating those events.

1. Other services can be connected to the Eventing system. These services can
   perform the following functions:

   - Create new applications without modifying existing event producers or event
     consumers.
   - Select and target specific subsets of events from particular producers.

1. Ensure cross-service interoperability. Knative Eventing is consistent with
   the [CloudEvents](https://github.com/cloudevents/spec) specification that is
   developed by the
   [CNCF Serverless WG](https://lists.cncf.io/g/cncf-wg-serverless).

Kubernetes has no primitives for event processing, yet events are an essential
component of serverless workloads. Knative Eventing introduces high-level
primitives for event production and delivery with an initial focus on push over
HTTP. If a new event source or type is required by your application, the effort
required to plug them into the existing Eventing framework will be minimal and
will integrate with CloudEvents middleware and message consumers.

Knative Eventing implements common components of an event delivery ecosystem:

- enumeration and discovery of event sources,
- configuration and management of event transport, and
- declarative binding of events (generated either by storage
  services or earlier computation) to further event processing and persistence.

The Knative Eventing API is intended to operate independently, and interoperate
well with the [Serving API](https://github.com/knative/serving) and
[Build API](https://github.com/knative/build).

---

_Navigation_:

- **Motivation and Goals**
- [Resource Types Overview](overview.md)
- [Interface Contracts](interfaces.md)
- [Object Model Specification](spec.md)
- [Channel Specification](channel.md)
- [Sources Specification](sources.md)
