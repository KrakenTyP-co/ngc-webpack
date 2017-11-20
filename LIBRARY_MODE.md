Use the AOT compiler to compile angular libraries suitable for AOT and
JIT consumption.

  - Compile resources (HTML, CSS, SCSS etc..) through webpack loaders
  - Use same configuration for development and library production build.
  - Inline resources

## Background
The `@ngtools/webpack` package is built for webpack bundling and does not
support the process of generating libraries for angular.

Library compilation has a specific process where each TS file is compiled
to JS a file without bundling the output.

`@angular/compiler-cli` supports this mode. By using the proper
configuration you can compile TS to JS for libraries with full AOT support.

The problem with using the `@angular/compiler-cli` is that it does not
know about webpack, resources will not go through the loader chain and so
using formats not supported the the angular cli will not work (SCSS, LESS etc).

Additionally, `templareUrl` and `stylesUrls` are left as is which is not
suitable for libraries, they most be inlined into the sources code (JS)
and the AOT generated `metadata.json` files.

`ngc-webpack` library mode allows AOT compilation from libraries through
a CLI interface or directly using it via node API.

## How

Using library mode, `ngc-webpack` will use `webpack` to pass all resources
through the loader chain and passing the output to the angular AOT compiler.
Using `webpack` ensures consistency, whatever loaders you used to build
your demo app are also used to build the library.

`ngc-webpack` can also inline your resources to both JS output files and
to the `.metadata.json` files generated by angular. Inlining is automatically
applied based on your AOT configuration set on `tsconfig`

Library mode does not require specific configuration, it is configured
using existing configuration in `tsconfig` and `webpack.config`.


The output is controlled by the `tsconfig` supplied.

`ngc-webpack` will inline all resources to both `metadata.json` files and `js` source code.
If `skipTemplateCodegen` is set to **false** the compiler will emit source code for all resource in dedicated modules
and so `ngc-webpack` will not perform the inline operation.

TODO: Usage with node + usage with CLI