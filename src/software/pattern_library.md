# peach-patterns

[![GitHub logo](/assets/github_logo.png "peach-patterns GitHub repository")](https://github.com/peachcloud/peach-patterns)

A pattern library for building and maintaining PeachCloud user interfaces.

`index.html` currently serves as the primary aggregator and displayer of patterns in the form of atoms and molecules (see [Atomic Design by Brad Frost](http://atomicdesign.bradfrost.com/) for more information on this approach to building design systems).

`css/css_class_names` contains a simple list of all the custom css class names used in the PeachCloud design system. This list builds on `css/_variables.css`, which contains css variables drawn from the [Tachyons](http://tachyons.io/) library. The content of `css/_variables.css` will be pruned of unneeded code once the design system stabilizes.

`css/peachcloud.css` contains all the custom css class definitions used in the PeachCloud design system.

_Note: This is a work-in-progress._

### Directory Tree

```
.
├── css
│   ├── css_class_names     // list of all custom class names
│   ├── peachcloud.css      // custom css class definitions and styles
│   └── _variables.css      // css variables from Tachyons
├── icons                   // all icon files (svg & png)
├── index.html              // markup of pattern library
├── README.md
```

### Setup

For now you can simply clone the repo and open the `index.html` file in your browser to view the pattern library.

### Licensing

AGPL-3.0
