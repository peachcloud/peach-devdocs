# peach-devdocs

[![Build Status](https://travis-ci.com/peachcloud/peach-devdocs.svg?branch=master)](https://travis-ci.com/peachcloud/peach-devdocs)

Developer documentation for [PeachCloud](https://github.com/peachcloud) in the form of a Markdown book.

## [Read online here >> :book:](https://mixmix.github.io/peach-devdocs)

:construction: _TODO - find a nice place to publish this online!_ :construction:

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

:construction: _TODO - describe how and where this is published / hosted_ :construction:

### Licensing

AGPL-3.0
