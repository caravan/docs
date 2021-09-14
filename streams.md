# Stream APIs

Streams provide a mechanism for routing Caravan Events through a processing workflow. This generally entails consuming from a Topic, performing some form of transformation or filtering of Events, and then sinking the results into another Topic, but it doesn't have to end there.

For example, the following Stream filters out integers that are odd.

```go
package main

import (
    "fmt"

    "github.com/caravan/essentials"
	"github.com/caravan/essentials/event"
	"github.com/caravan/essentials/sender"
    "github.com/caravan/streaming/stream/node"
    "github.com/caravan/essentials/topic/config"
)

func main() {
    in := essentials.NewTopic(config.Consumed)
    out := essentials.NewTopic(config.Permanent)

    s := streaming.NewStream(
        // Stream from the 'in' topic
        node.TopicSource(in),
        // Filter events coming from 'in' to only include
        // even numbers
        node.Filter(func(e event.Event) bool {
            return e.(int) % 2 == 0
        }),
        // Stream the remaining events to the 'out' topic
        node.TopicSink(out),
    )
    _ = s.Start()

    go func() {
        c := out.NewConsumer()
        for i := range c.Receive() {
            fmt.Println(i)
        }
        c.Close()
    }()

    go func() {
        p := in.NewProducer()
        for i := 0; i < 100; i++ {
			p.Send() <- i
        }
        p.Close()
    }()

    <- make(chan bool) // hit ctrl-c
}
```
