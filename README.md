# JigSass Utils Visibility
[![NPM version][npm-image]][npm-url]  [![Dependency Status][daviddm-image]][daviddm-url]   

  > A collection of dynamically generated css visibility related utility classes.

Class names follow the [Emmet](http://docs.emmet.io/cheat-sheet/) abbreviation
syntax, with colons (':') replaced by two dashes (`--`) to follow BEM naming
conventions.
E.g., the `visibility: hidden` utility class name is `.u-v--h` and
the `backface-visibility: visible` one is `.u-bfv--v`.

## Available classes

  - `.u-v--h` (visibility: hidden)
  - `.u-v--v` (visibility: visible)
  - `.u-v--inher` (visibility: inherit)
  - `.u-c--init` (visibility: initial)
  - `.u-v--un` (visibility: unset)

  - `.u-bfv--h` (backface-visibility: hidden)
  - `.u-bfv--v` (backface-visibility: visible)

Additionally, JigSass Visibility provides the following stateful helpers:
  - `.u-is-hidden`: Hides elements visually and from screen readers.
  - `.u-is-visually-hidden`: Hides elements visually but leaves them accesible for screen readers.
  - `.u-is-visually-hidden.u-is-focusable`: Reveal focusable visually hidden elements on focus
    (with keyboard navigation or js).

## Installation

Using npm:

```sh
npm i -S jigsass-utils-visibility
```

## Usage
Import JigSass Utils Visibility into your main scss file near its very end, together with all
other utilities (utilities should always be the last to be imported).

```scss
@import 'path/to/jigsass-utils-visibility/scss/index';
```

Like all other JigSass Utils, JigSass Visibility does not automatically generate any CSS
when imported. You would need to explicitly indicate that each individual visibility
class should actually be generated in each component or object it is used in
(clarification: This will include style declarations inside `.foo` and .`bar`):

```scss
// _c.foo.scss
.foo {
  @include jigsass-util(u-v, $modifier: h); // <-- visibility: hidden

  ...
}
```

```scss
// _c.bar.scss
.bar {
  @include jigsass-util(u-bfv, $modifier: h);  // <-- backface-visibility: hidden
  @include jigsass-util(
    u-bfv,
    $modifier: visible,
    $from: large
  ); // <-- backface-visibility: visible from large bp an on.

  ...
}
```

```scss
// _c.baz.scss
.baz {
  @include jigsass-util(u-is-hidden, $from: medium); // <-- display: none from medium bp an on.

  ...
}
```

Doing so helps us a great deal with portability, as no matter where we import component or object
partials, the correct utility classes will be generated. Think of it as a poor man's dependency
management.

Developer communication is also assisted by including "dependencies" wherever they are required,
as anyone going through a partial, can easily understand how it should be marked up with just a
glance.

As far as bloat goes, just don't worry about it - the actual styles will only be generated once,
at the location in the cascade where the Jigsass Visibility partial was imported into the main file.


JigSass Visibility classes are responsive-enabled, using [JigSass MQ](https://txhawks.github.io/jigsass-tools-mq/)
and the breakpoints defined in the [$jigsass-breakpoints](https://txhawks.github.io/jigsass-tools-mq/#variable-jigsass-breakpoints) variable.

Based on the breakpoint arguments passed to `jigsass-util` when including a visibility class, responsive
modifiers are generated according to the following logic:

```scss
.u-v--<modifier>[-[-from-<breakpoint-name>][-until-<breakpoint-name>][-misc-<breakpoint-name>]]
```

So, assuming the `medium`, `large` and `landscape` breakpoints are defined in `$jigsass-breakpoints`
as `600px`, `1024px` and `(orientation: landscape)` respectively,

```scss
@include jigsass-util(u-v, $modifier: h);
```
will generate the `.u-v--h` class, which is not limited to any media-query.

```scss
@include jigsass-util(u-v, $modifier: h, $until: medium);
```

will generate the `.u-v--h--until-medium` class, which will be in effect at
`(max-width: 37.49em)` and will override styles in the default class until that point.

```scss
@include jigsass-util(u-v, $modifier: h, $from: large, $misc: landscape);
```

will generate the `.u-v--h--from-large-when-landscape` class, which will go into
effect at `(min-width: 64em) and (orientation: landscape)` and will override styles in the default
class under these  conditions.


## Documentation

The full documentation was generated using mdcss, and is available at 
[https://txhawks.github.io/jigsass-utils-visibility/](https://txhawks.github.io/jigsass-utils-visibility/)

## Contributing

It is a best practice for JigSass modules to *not* automatically generate css on `@import`, but 
rather have the user explicitly enable the generation of specific styles from the module.

Contributions in the form of pull-requests, issues, bug reports, etc. are welcome.
Please feel free to fork, hack or modify JigSass Visibility in any way you see fit.

#### Writing documentation

Good documentation is crucial for usability, scalability and maintainability. When 
contributing, please do make sure that both its Sass functionality (functions, mixins, 
variables and placeholder selectors), as well as the CSS it generates (selectors, 
concepts, usage exmples, etc.) are well documented.

Jigsass Visibility uses Jonathan Neal's [mdcss](//github.com/jonathantneal/mdcss).

When styles and documentation comments are not automatically generated by your module on `@import`,
please use the `sgSrc/sg.scss` file to enable their generation.

In addition, any file in `sgSrc/assets` will be available for use in the style guide.



## File structure
```bash
┬ ./
│
├─┬ scss/ 
│ └─ index.scss # The module's importable file.
│
├─┬ sgSrc/      # Style guide sources
│ │
│ ├── sg.scc    # It is a best practice for JigSass 
│ │             # modules to not automatically generate 
│ │             # css and documentation on `@import.` 
│ │             # Please use this file to enable css
│ │             # and documentation comments) generation.
│ │
│ └── assets/   # Files in `sgSrc/assets` will be 
│               # available for use in the style guide
│
└─┬ docs/      # Documention
  │
  └── styleguide/ # Generated documentation 
                  # of the module's CSS
```





**License:** MIT



[npm-image]: https://badge.fury.io/js/jigsass-utils-visibility.svg
[npm-url]: https://npmjs.org/package/jigsass-utils-visibility

[daviddm-image]: https://david-dm.org/TxHawks/jigsass-utils-visibility.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/TxHawks/jigsass-utils-visibility
