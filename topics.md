# Topics

Topics are the nexus of Event streaming in Caravan. They manage the Log and are how you instantiate Producers and Consumers.

## The Log

Each Topic configures its own Log. The Log is an append-only data structure that _may_ drop initial Events, subject to a Retention Policy. Caravan implements four policies out of the box.

- Permanent - All Events are retained, even if you eventually run out of memory for storing them. This is the default policy, but probably not the best idea for long-lived Topics
- Consumed - Events that have already been seen by the active set of Consumers may be discarded. If no Consumers are active, Events will be dropped immediately
- Counted - Only a maximum specified number of recent Events will be retained by the Log
- Timed - Only Events that have been produced in that last specified duration will be retained by the Log

It's important to note that Caravan drops what are called "segments" rather than individual Events. So if a policy should determine that an Event can be discarded, it will only become inaccessible if _all_ of the Events that are part of its segment can also be discarded. The default size of a segment in Caravan is 32 entries.
