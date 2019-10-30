# peach-devdocs

Developer documentation for PeachCloud in the form of a Markdown book.

### Setup

[mdBook](https://github.com/rust-lang/mdBook) is required to build and serve the book locally. Installation can be achieved via the [release binaries](https://github.com/rust-lang/mdBook/releases) or directly via Cargo (assuming you have Rust version 1.35 or higher and Cargo installed):

`cargo install mdbook`

Once mdBook is installed, clone this repo:

`git clone https://github.com/peachcloud/peach-devdocs`

Move into the repo, add the Cargo bin directory to your `PATH`, build and serve:

`cd peach-devdocs`  
`export PATH=$PATH:~/.cargo/bin`  
`mdbook build`  
`mdbook serve`

The book is served on `localhost:3000` by default and refreshes automatically in-browser when changes to the documentation are saved.

### Licensing

AGPL-3.0
