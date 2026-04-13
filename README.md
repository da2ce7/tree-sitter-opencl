# tree-sitter-opencl

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

OpenCL C grammar for [tree-sitter](https://github.com/tree-sitter/tree-sitter).

Adapted from [tree-sitter-c](https://github.com/tree-sitter/tree-sitter-c), with the addition of OpenCL C keywords, types, qualifiers, and built-in types from the OpenCL C specification.

## Features

- Full C language grammar support (inherited from tree-sitter-c)
- OpenCL C kernel and function qualifiers (`kernel`, `__kernel`)
- Address space qualifiers (`global`, `local`, `constant`, `private`, `generic` and their `__` prefixed variants)
- Access qualifiers (`read_only`, `write_only`, `read_write`)
- OpenCL scalar types (`half`, `uchar`, `ushort`, `uint`, `ulong`, `size_t`, `ptrdiff_t`, `intptr_t`, `uintptr_t`)
- OpenCL vector types (`float4`, `int8`, `char16`, etc.)
- OpenCL built-in types (`image2d_t`, `sampler_t`, `event_t`, `queue_t`, etc.)
- `pipe` type qualifier
- Syntax highlighting queries

## Installation

### Node.js

```sh
npm install tree-sitter-opencl
```

### Rust

Add to your `Cargo.toml`:

```toml
[dependencies]
tree-sitter-opencl = "0.0.1"
```

## Usage

### Node.js

```javascript
const Parser = require("tree-sitter");
const OpenCL = require("tree-sitter-opencl");

const parser = new Parser();
parser.setLanguage(OpenCL);

const sourceCode = `
__kernel void vector_add(__global const float *a,
                         __global const float *b,
                         __global float *result) {
    int gid = get_global_id(0);
    result[gid] = a[gid] + b[gid];
}
`;

const tree = parser.parse(sourceCode);
console.log(tree.rootNode.toString());
```

### Rust

```rust
let mut parser = tree_sitter::Parser::new();
parser
    .set_language(tree_sitter_opencl::language())
    .expect("Error loading OpenCL grammar");

let source_code = r#"
__kernel void vector_add(__global const float *a,
                         __global const float *b,
                         __global float *result) {
    int gid = get_global_id(0);
    result[gid] = a[gid] + b[gid];
}
"#;

let tree = parser.parse(source_code, None).unwrap();
println!("{}", tree.root_node().to_sexp());
```

## Development

### Prerequisites

- [Node.js](https://nodejs.org/)
- [tree-sitter CLI](https://github.com/tree-sitter/tree-sitter/blob/master/cli/README.md)

### Generating the parser

```sh
npm install
npm run generate
```

### Running tests

```sh
npm test
```

## Contributing

Feel free to create an issue or pull request if you experience problems or would like to improve OpenCL C spec coverage.

## License

[MIT](LICENSE)
