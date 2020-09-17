# Microservices

PeachCloud is built primarily with a [microservices architecture](https://microservices.io/). Each microservice utilises [JSON-RPC](https://www.jsonrpc.org/specification) - a stateless, light-weight remote procedure call protocol - as a means of exposing functionality and allowing interoperability. This approach produces a collection of services which are highly maintainable and testable, loosely coupled and independently deployable. HTTP and WebSockets are used as transports for the microservices.

The suite of PeachCloud microservices currently includes:

 - **peach-buttons** [(repo)](https://github.com/peachcloud/peach-buttons)
   - poll GPIO pins for button presses and emit events via pub-sub
 - **peach-menu** [(repo)](https://github.com/peachcloud/peach-menu)
   - monitor and interact with the device via the physical interface
 - **peach-monitor** [(repo)](https://github.com/peachcloud/peach-monitor)
   - monitor network data usage and set alert flags
 - **peach-network** [(repo)](https://github.com/peachcloud/peach-network)
   - query and configure network interfaces
 - **peach-oled** [(repo)](https://github.com/peachcloud/peach-oled)
   - write and draw to the OLED display
 - **peach-stats** [(repo)](https://github.com/peachcloud/peach-menu)
   - query system statistics
