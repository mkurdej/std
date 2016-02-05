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
- [Eric Niebler's _Tiny Metaprogramming Library_] [2] ([source] [12])
- [Peter Dimov's _Simple C++11 metaprogramming_] [3]
- [Edouard's _Brigand_ library] [4]

### Goals

We set the following, very general and subjective, goals for the library:

- macro-free
- easy to use
 - contains convenience meta-functions
- (relatively) fast to compile

## Motivation and Scope

This proposal includes changes to the library facilities **only**.
It is heavily based on [Brigand] [4] library.

The basic brick in the library is a type _sequence_: a type alias or template class accepting a template parameter pack.

## `std::meta` namespace

The proposed library resides in `std::meta` namespace and is divided into the following categories:
- sequences (data structures)
- algorithms
- types (helpers, placeholders, aliases)
- functions (helpers, type traits - TODO: move to <type_traits>)

### Sequences

- `list`: a possibly non-unique list/chain/vector of types
 - aka `packer` ([N4115 proposal][4115]) or `typelist` ([Eric Niebler][2])
 - no separation of list and vector (cf. [MPL] [1])
- `map`: a structure allowing to associate one type (key) to another type (value)
 - keys are unique
- `set`: a unique list of types
- `pair`: used for `map`

#### Basic operations

##### Modifying

**NOTE:** Actually, nothing can be modified, only a new type can be created.

- `clear<Sequence>`
- `append<Sequences...>`
- `insert<Sequence, Type>`
- `erase<Sequence, Type>`
- `push_front<Sequence, Type>` ?
- `pop_front<Sequence, Type>` ?
- `push_back<Sequence, Type>`
- `pop_back<Sequence, Type>`
- `drop` ?

##### Querying

- `size<Sequence>`
- `contains<Sequence, Type>`
 - aka `is_contained_in<Type, T... P>` ([N4115 proposal][4115])
- `at<Sequence, Index>`
- `front<Sequence> == at<Sequence, 0>`
- `back<Sequence> == at<Sequence, size<Sequence> - 1>`
- `has_key<Sequence, KeyType>`
- `index_of`, `index_if` (see [`mp_find_index`][3])

#### Algorithms
- `all_if<Sequence, Predicate>`
- `any_if<Sequence, Predicate>`
- `find_if<Sequence, Predicate>`
- `flatten<Sequences...>`
- `fold<>`
- `for_each<>`
- `partition<>`
- `stable_partition<>`
- `sort<>`
- `replace<>`
- `reverse<>`
- `split<>`
- `transform<>`
- `unique`

#### Functions

- `identity`
- `inherit`
- `wrap<Sequence, template <typename...> typename Sequence>` (see [`mp_rename`][3])

##### Convenience helpers

These are handy aliases to `algorithm<Sequence, is_same<Type, _>>`, where `_` is a placeholder.

- `all_of<Sequence, Type>`
- `any_of<Sequence, Type>`
- `find<Sequence, Type>`

#### Placeholders
- `_`: used in `find`, `all`, `any`, etc.
- `_state`, `_element`: used in `fold`, `for_each`

## References

1. Boost.MPL library: [http://www.boost.org/doc/libs/release/libs/mpl/] [1]. Accessed: 2016-02-04.
1. Eric Niebler's meta library: [https://github.com/ericniebler/meta] [12]. Accessed: 2016-02-04.
1. Peter Dimov's _simple C++ metaprogramming_: [http://pdimov.com/cpp2/simple_cxx11_metaprogramming.html] [3]. Accessed: 2016-02-05.
1. Brigand library: [https://github.com/edouarda/brigand] [4]. Accessed: 2016-02-04.
1. Boost.Fusion library: [http://www.boost.org/doc/libs/release/libs/fusion/] [6]. Accessed: 2016-02-04.
1. N3996 proposal: [https://isocpp.org/files/papers/n3996.pdf] [9]. Accessed: 2016-02-04.
1. N4115 proposal: [http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4115.html] [4115]. Accessed: 2016-02-05.
1. D4128 proposal: [https://ericniebler.github.io/std/wg21/D4128.html] [4128]. Accessed: 2016-02-04.

[1]: http://www.boost.org/doc/libs/release/libs/mpl/
[2]: http://ericniebler.com/2014/11/13/tiny-metaprogramming-library/
[3]: http://pdimov.com/cpp2/simple_cxx11_metaprogramming.html
[4]: https://github.com/edouarda/brigand
[5]: https://akrzemi1.wordpress.com/2012/03/19/meta-functions-in-c11/
[6]: http://www.boost.org/doc/libs/release/libs/fusion/
[8]: https://github.com/ldionne/mpl11
[9]: https://isocpp.org/files/papers/n3996.pdf
[11]: https://github.com/boostorg/hana
[12]: https://github.com/ericniebler/meta
[13]: http://pdimov.com/cpp2/simple_cxx11_metaprogramming_2.html
[14]: https://github.com/dabrahams/mpl11
[100]: https://github.com/HeliumProject/Reflect
[4115]: http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4115.html
[4128]: https://ericniebler.github.io/std/wg21/D4128.html
