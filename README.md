<p align=center><img src="./resources/capybara.png" alt="capy icon" height="150"/></p>

# The Capy Programming Language

A statically typed, compiled programming language, largely inspired by Jai, Odin, and Zig.
It even has arbitrary compile-time evaluation!

Now on all your favorite Operating Systems! Thanks [cranelift](https://cranelift.dev/)!

```capy
libc :: import "std/libc.capy";

to_print :: "Hello, World!";

main :: () -> i32 {
    // prints "Hello, World!" to the screen
    libc.puts(to_print);

    // exit with code 0
    0
}
```

*From [`examples/hello_world.capy`](./examples/hello_world.capy)*

## Features

(Make sure to put these snippets in a `main` function)

Static arrays,

```capy
my_array := [] i32 { 4, 8, 15, 16, 23, 42 };

my_array[2] = 10;
```

Pointers,

```capy
foo := 5;
bar :: ^mut foo;

bar^ = 10;
```

Structs,

```capy
Person :: struct {
    name: string,
    age: i32
};

gandalf := Person {
    name: "Gandalf",
    age: 2000,
};

// birthday!
gandalf.age = gandalf.age + 1;
```

Arbitrary Compile-Time Evaluation,

```capy
math :: import "std/math.capy";

powers_of_two := comptime {
    array := [] i32 { 0, 0, 0 };

    array[0] = math.pow(2, 1);
    array[1] = math.pow(2, 2);
    array[2] = math.pow(2, 3);

    array
};
```

First Class Functions,

```capy
add :: (x: i32, y: i32) -> i32 {
    x + y
};

mul :: (x: i32, y: i32) -> i32 {
    x * y
};

apply_2_and_3 :: (fun: (x: i32, y: i32) -> i32) -> i32 {
    fun(2, 3)
};

apply_2_and_3(add);
apply_2_and_3(mul);
```

... All compiled to machine code (I'm so proud of this).

Look at the [`examples`](./examples/) folder to see more.

## Getting Started

First clone this repo,

```shell
git clone https://github.com/capy-language/capy.git
```

Then install the capy command with cargo,

```shell
cargo install --path crates/capy
```

Make sure you have `gcc` installed,

Then compile and run your code,

```shell
capy run examples/hello_world.capy
```

Or if you just want to build the final executable,

```shell
capy build examples/hello_world.capy
```

## Limitations

Currently, `gcc` must be installed for the compiler to work.
It is used for linking to libc and producing a proper executable.

If you want to use libc functions, define them with `extern` (look in [`libc.capy`](./examples/std/libc.capy) for examples).
Variadic functions do not work. You *could* try explicitly defining `printf`
to take 3 arguments, but this won't work for floats, which are passed into
variadic functions differently depending on the calling convention.
Cranelift is [currently working on adding variadic support](https://github.com/bytecodealliance/wasmtime/issues/1030),
so that might be added in the future.

If you find any bugs in the compiler, please please be sure to [make an issue](https://github.com/capy-language/capy/issues) about it and I'll fix it as soon as I can.

## Shout Outs

Big shout out to [Luna Razzaghipour](https://github.com/lunacookies), the structure of this entire codebase is largely based on [gingerbread](https://github.com/gingerbread-lang/gingerbread) and [eldiro](https://github.com/lunacookies/eldiro).
Her help in teaching how programming languages really work is immeasurable and I'm very thankful.

Big shout out to [cranelift](https://cranelift.dev/). Trying to get LLVM on windows was just way too much effort for me and cranelift made all my dreams come true.

I know the cranelift documentation isn't the greatest, so if anyone wants to use this repo to see how I've implemented higher-level features such as arrays, structs, first class functions, etc. then it's all in [`crates/codegen`](./crates/codegen/).

## License

The Capy Programming Language is distributed under the terms of both the MIT license and the Apache License (Version 2.0).
See [LICENSE-APACHE](./LICENSE-APACHE) and [LICENSE-MIT](./LICENSE-MIT) for details.

The capybara logo was AI generated by [imagine.art](https://www.imagine.art/), who own the rights to it. It can be used in this non-commercial setting with attribution to them.
