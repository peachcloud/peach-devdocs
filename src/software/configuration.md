# Configuration

The microservices and other components comprising the PeachCloud software are configurable via the following environment variables:

| Microservice | Variable | Description | Default |
| --- | --- | --- | --- |
| peach-network | `PEACH_NETWORK_SERVER` | HTTP server address and port for JSON-RPC | `127.0.0.1:5110` |
| peach-buttons | `PEACH_BUTTONS_SERVER` | WebSocket server address and port for JSON-RPC | `127.0.0.1:5111` |
| peach-oled | `PEACH_OLED_SERVER` | HTTP server address and port for JSON-RPC | `127.0.0.1:5112` |
| peach-stats | `PEACH_STATS_SERVER` | HTTP server address and port for JSON-RPC | `127.0.0.1:5113` |
| peach-web | `ROCKET_ENV` | Web application deployment mode | `prod` |
| peach-web | `ROCKET_TEMPLATE_DIR` | Tera template directory | `static/templates` |
| peach-web | `PEACH_WEB_WS` | WebSocket server port | `5115` |
