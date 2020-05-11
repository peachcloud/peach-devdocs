# Web Interface

[peach-web](https://github.com/peachcloud/peach-web) provides a web interface for monitoring and interacting with the PeachCloud device. This allows administration of the single-board computer (ie. Raspberry Pi) running PeachCloud, as well as the ssb-server and related plugins.

### Design

`peach-web` is written primarily in Rust and presents a web interface for interacting with the device. The stack currently consists of [Rocket](https://rocket.rs/) (Rust web framework), [Tera](https://tera.netlify.com/docs/installation/) (Rust template engine inspired by Jinja2 and the Django template language), HTML, CSS and JavaScript. Additional functionality is provided by JSON-RPC clients for the `peach-network` and `peach-stats` microservices.

HTML is rendered server-side. Request handlers call JSON-RPC microservices and serve HTML and assets. A JSON API is exposed for remote calls and dynamic client-side content updates via vanilla JavaScript following [unobstructive design principles](https://www.w3.org/wiki/The_principles_of_unobtrusive_JavaScript). A basic Websockets server is included, though is not currently utilised. Each Tera template is passed a context object. In the case of Rust, this object is a `struct` and must implement `Serialize`. The fields of the context object are available in the context of the template to be rendered.

### Directory Tree

```
.
├── Cargo.lock
├── Cargo.toml
├── docs
│   ├── api_docs.md
│   ├── FEATURES.md
│   ├── js_docs.md
│   └── RESOURCES.md
├── .gitignore
├── README.md
├── Rocket.toml
├── src
│   ├── device.rs
│   ├── error.rs
│   ├── lib.rs
│   ├── main.rs
│   ├── network.rs
│   ├── stats.rs
│   ├── structs.rs
│   ├── tests.rs
│   └── ws.rs
├── static
│   ├── css
│   │   ├── peachcloud.css
│   │   └── _variables.css
│   ├── favicon.ico
│   ├── icons
│   ├── js
│   │   ├── network_card.js
│   │   ├── shutdown_menu.js
│   └── templates
│       ├── base.html.tera
│       ├── device.html.tera
│       ├── index.html.tera
│       ├── nav.html.tera
│       ├── network_add.html.tera
│       ├── network_card.html.tera
│       ├── network_detail.html.tera
│       ├── network_list.html.tera
│       ├── network_modify.html.tera
│       ├── not_found.html.tera
│       └── shutdown.html.tera
```

### Environment

The web application deployment mode is configured with the `ROCKET_ENV` environment variable:

`export ROCKET_ENV=stage`

Other deployment modes are `dev` and `prod`. Read the [Rocket Environment Configurations docs](https://rocket.rs/v0.4/guide/configuration/#environment) for further information.

The WebSocket server port can be configured with `PEACH_WEB_WS` environment variable:

`export PEACH_WEB_WS=2333`

When not set, the value defaults to `5115`.

Logging is made available with `env_logger`:

`export RUST_LOG=info`

Other logging levels include `debug`, `warn` and `error`.

### Setup

Clone this repo:

`git clone https://github.com/peachcloud/peach-web.git`

Move into the repo and compile:

`cd peach-web`  
`cargo build --release`

Run the tests:

`cargo test`

Run the binary:

`./target/release/peach-web`

_Note: Networking functionality requires peach-network microservice to be running._


