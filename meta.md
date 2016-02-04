# C++ template meta-programming utilities library proposal

## Introduction

This paper describes a library support for template meta-programming in C++ language.

Current C++ standard (C++14) and the draft of C++1z (aka C++17) do not have real tools for template meta-programming apart from type traits from `<type_traits>` and some tools, notably `std::integer_sequence`, from `<utility>` header.

For that reason, we want to propose a meta-programming library that will go further than defining a typelist type (e.g. `template <typename... T> struct typelist {};`).

The presence of lots of libraries, well known and widely used, attests the necessity of a standardised meta-programming library.

- [Boost.MPL] [1]
 - C++-98 compliant
 - supports many compilers
 - heavy macro magic
- [Boost.Fusion] [6]
 - advantages and inconviences similar to Boost.MPL
 - compile-time and run-time glue
- [Eric Niebler's *Tiny Metaprogramming Library*] [2] ([source] [12])
- [Peter Dimov's *Simple C++11 metaprogramming*] [3]
- [Edouard's *Brigand* library] [4]

### Goals

We set the following, very general and subjective, goals for the library:

- macro-free
- easy to use
 - contains convenience meta-functions
- (relatively) fast to compile

[1]: http://www.boost.org/doc/libs/release/libs/mpl/doc/index.html
[2]: http://ericniebler.com/2014/11/13/tiny-metaprogramming-library/
[3]: http://pdimov.com/cpp2/simple_cxx11_metaprogramming.html
[4]: https://github.com/edouarda/brigand
[5]: https://akrzemi1.wordpress.com/2012/03/19/meta-functions-in-c11/
[6]: http://www.boost.org/doc/libs/release/libs/fusion/doc/html/index.html

[9]: https://isocpp.org/files/papers/n3996.pdf
[10]: https://github.com/HeliumProject/Reflect

[12]: https://github.com/ericniebler/meta

## Motivation and Scope

This proposal includes changes to the library facilities **only**.

The basic brick in the library is a type *sequence*: a type alias or template class accepting a template parameter. **TODO**: equivalence with `std::tuple`.

## `std::meta` namespace

The proposed library resides in `std::meta` namespace and is divided into the following categories:
- sequences (data structures)
- algorithms
- types (helpers, placeholders, aliases)
- functions (helpers, type traits - TODO: move to <type_traits>)
