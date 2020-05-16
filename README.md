# ~Typeclassify~

*Refer to Type-Classes-on-data as is they are the Type of the data*

**TLDR:** idea not considered viable. In Dotty, the power of compiler plugins has been greatly restricted to avoid creating dialects of Scala (as proposed here!). Only "research plugins" have the power required to implement the ideas powered here, and they are limited to use on dotty snapshots. See [gitter discussion](https://gitter.im/lampepfl/dotty?at=5eaec2f60b23797ec05c8dac).

## Overview

The goal of this experimental Dotty compiler plugin is to make ubiquitous use of typeclasses easier. 

The use cases are libraries like [Spire](https://typelevel.org/spire/guide.html), which models numbers as a nicely factored "numeric tower". Current Scala adds heavy syntactic overhead to use Spire typeclasses, vs primitive number types like `Double` where the operations are innate on the datatype.

The plugin aims to automatically desugar method signatures that include a special new type prefix `#`. For example:

```
def maximum(xs: List[#Ord]): #Ord = xs.reduceLeft(max)
```
..desugars to:
```
def maximum[T](xs: List[T])(using Ord[T]): T = xs.reduceLeft(max)
```

`#` denotes a typeclass constraint on an underyling generic type, which can be ommitted and the compiler plugin will automatically introduce it.

## TODO

- Write up all the syntactic cases it supports, eg what about mixed use of some explicit context bounds and some `#` notation?
- Implement a plugin :P
