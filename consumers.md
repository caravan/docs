# Consumers

Consumers allow you to receive Events from the Log. You can do this using functions like `Poll`, which allows for a timeout, using `Receive`, which will block indefinitely, or retrieving the underlying channel and pulling Events directly from it.

Each Consumer instantiated from a Topic maintains an independent index into its Log. In this way, a Caravan Topic acts a bit like a Fanout Exchange in a Message Broker. The difference between Caravan and a Message Broker is that, in Caravan, Consumers can be instantiated at any time, and they will start consuming at the first retained Event in the Log, even if that was the first Event ever produced. In a Message Broker, you only have access to Messages produced after your Queue has been plumbed into the Topic.

## Poll

The `Poll` function allows the developer to query the Consumer for pending Events. If no Event is immediately available, the call will block up to the specified amount of time.

```go
package main

import (
    "fmt"
    "time"

    "github.com/caravan/essentials"
	"github.com/caravan/essentials/message"
)

func main() {
    top := essentials.NewTopic()
    // ... Code that produces Events
    c := top.NewConsumer()
    if e, ok := message.Poll(c, time.Millisecond * 50); ok {
        fmt.Println("Received: ", e)
    } else {
        fmt.Println("Nothing Received")
    }
}
```

## Receive

The `Receive` function will block indefinitely until an Event is returned. If the Consumer has been closed, this boolean result will be `false`.

```go
package main

import (
    "fmt"

    "github.com/caravan/essentials"
    "github.com/caravan/essentials/message"
)

func main() {
    top := essentials.NewTopic()
    // ... Code that produces Events
    c := top.NewConsumer()
    if e, ok := message.Receive(c); ok {
        fmt.Println("Received: ", e)
    } else {
        fmt.Println("Consumer was closed!")
    }
}
```
