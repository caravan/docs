# Caravan Documentation

Caravan is an in-process event streaming library for Go applications. Think Kafka, but for the internal workings of your software. With it, you can decouple the emission of observable events from the act of consuming them. In Caravan, the events that a producer generates may be consumed by hundreds of downstream consumers, or none at all.

## Caravan Basics

There are four basic concepts to wrap one's mind around when working with Caravan.

### Events

Events are the messages that are passed around by Caravan. At the moment an Event can take any form (they're based on the `interface{}` type), but a future version of Caravan will allow you to restrict the type per Topic using [Go Type Parameters](https://go.googlesource.com/proposal/+/refs/heads/master/design/go2draft-type-parameters.md).

### Topics

[Topics](./topics.md) are the nexus of Event streaming in Caravan. They manage the Log and are how you instantiate Producers and Consumers.

### Producers

[Producers](./producers.md) allow you to append Events to the Log. The interface is rather simple. You can either use the `Send` method or retrieve the underlying `Channel` and push Events directly to it.

### Consumers

[Consumers](./consumers.md) allow you to receive Events from the Log. You can do this using methods like `Poll`, which allows for a timeout, using `Receive`, which will block indefinitely, or retrieving the underlying `Channel` and pulling Events directly from it.
