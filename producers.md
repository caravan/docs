# Producers

Producers allow you to append Events to the Log. The interface is rather simple. You can either use the `Send` method or retrieve the underlying `Channel` and push Events directly to it.

```go
package main

import (
    "fmt"

    "github.com/caravan/essentials"
)

func main() {
    top := essentials.NewTopic()
    p := top.NewProducer()
    if ok := p.Send("someEvent"); ok {
        fmt.Println("Send was successful")
    } else {
        fmt.Println("Producer was closed!")
    }
}
```
