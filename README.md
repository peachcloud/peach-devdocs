# peach-devdocs

[![Build Status](https://travis-ci.com/peachcloud/peach-devdocs.svg?branch=master)](https://travis-ci.com/peachcloud/peach-devdocs) ![Generic badge](https://img.shields.io/badge/version-0.2.0-<COLOR>.svg)

Developer documentation for [PeachCloud](https://github.com/peachcloud) in the form of a Markdown book.

## [Read online here >> :book:](http://docs.peachcloud.org)

![PeachCloud physical interface](./src/assets/peachcloud.jpg)

## Development

Dependencies:
- [mdBook](https://github.com/rust-lang/mdBook) (release binaries or cargo install)

```bash
$ git clone https://github.com/peachcloud/peach-devdocs
$ cd peach-devdocs
$ mdbook serve
```

This serves the current state of the book at [localhost:3000](http://localhost:3000).
When changes to any files are saved, the browser view will automatically refresh to reflect the new state.

**NOTES**:
- this assumes `mdbook` is in your `PATH`
  - the command for adding the default cargo bin directory is `export PATH=$PATH:~/.cargo/bin`.
  - if you downloaded the prebuilt binary then make sure the directory is in your `PATH`

## Build a release

```
$ mdbook serve
```

This builds the book into a static release ready for publishing.
Currently outputs to `book/` directory.

## Hosting

The PeachCloud developer documentation book is hosted at [docs.peachcloud.org](http://docs.peachcloud.org) using a simple Nginx deployment on a virtual server. HTTPS configuration is pending.

This tutorial from Digital Ocean describes the deployment process: [How To Set Up Nginx Server Blocks (Virtual Hosts) on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04).

### Licensing

AGPL-3.0
