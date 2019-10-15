# Microservices

PeachCloud is built primarily with a [microservices architecture](https://microservices.io/). Each microservice utilises [JSON-RPC](https://www.jsonrpc.org/specification) - a stateless, light-weight remote procedure call protocol - as a means of exposing functionality and allowing interoperability. This approach produces a collection of services which are highly maintainable and testable, loosely coupled and independently deployable. HTTP and WebSockets are used as transports for the microservices.

The suite of PeachCloud microservices currently includes:

 - [peach-buttons](https://github.com/peachcloud/peach-buttons)
   - poll GPIO pins for button presses and emit events via pub-sub
 - [peach-menu](https://github.com/peachcloud/peach-menu)
   - monitor and interact with the device via the physical interface
 - [peach-network](https://github.com/peachcloud/peach-network)
   - query and configure network interfaces
 - [peach-oled](https://github.com/peachcloud/peach-oled)
   - write and draw to the OLED display
 - [peach-stats](https://github.com/peachcloud/peach-menu)
   - query system statistics
