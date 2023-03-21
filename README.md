# Dahlia Specification

- [Syntax](#syntax)
- [Formatting Reference](#formatting-reference)
- [API](#api)
  - [Utility functions](#utility-functions)

## Syntax


**TODO:**
- [ ] &[0-9a-g] for one of the basic 17 colors (maybe remove g? it's kinda
  useless)
- [ ] &[#FFFFFF] for custom color
- [ ] &[i-p] for text styles
- [ ] escaping (how???)
- [ ] &r for reset
- [ ] '~' after marker for background

## Formatting Reference

**TODO:**
- [ ] format codes table (i: invert, l: bold, o: italic, etc.)
- [ ] 3/4/8/24-bit color tables for 0-g (or 0-f?)
- [ ] include ANSI codes for all of these

## API

**TODO:**
- [x] type/method structure for OOP-like
- [x] function sigs for the rest
- [ ] explain parameters
  - [x] depth
  - [ ] no_reset
  - [ ] no_color (incl. NO_COLOR env var)
  - [ ] marker
- [ ] explain how print should be close to built-in solution
- [ ] explain that specific impls can have print/printf/println if that's
  consistent with the language (like Rust having `dprint` and `dprintln`)
- [ ] mention that implementations should follow the language's naming conventions
- [ ] TERM env var

For object-oriented languages (and those capable of implementing an equivalent
model, e.g. Rust with `struct` + `impl`):
```ts
enum Depth {
    TTY = 3,
    LOW = 4,
    MEDIUM = 8,
    HIGH = 24
}
type DepthString = "tty" | "low" | "medium" | "high"
type DepthInt = 3 | 4 | 8 | 24


class Dahlia {
    constructor(
        depth: Depth | DepthString | DepthInt = Depth.LOW,
        no_reset: boolean = false,
        no_color: boolean? = null,
        marker: char = '&'
    ) {}

    convert(str: string): string {}
    input(prompt: string): string {}
    print(...) {}
    reset() {}
    test() {}
}
```
Target languages do not have to support the concept of default parameter values.
If they do, they should stick to defaults in the above code block.

The `depth` parameter refers to color depth (3, 4, 8 and 24 refer to the number
of bits<!-- improve this -->) of should support at least 1 of the above types
(preferably all of them if possible). If a language only allows one type to be
supported, the preference is:
1. enum
2. integers
3. strings

> **Note**  
> If enums require hacky code or are hard to use in a given language, integers
> are preferred instead.


If possible, `Dahlia` instances should be:
- hashable
- comparable: using `depth`, `no_reset` and `marker` values
- printable: preferably in a way that's valid syntax, e.g.:
  - Python: `Dahlia(depth=Depth.HIGH, no_reset=False, marker='&')`  
  - Swift: `Dahlia(depth: Depth.HIGH, no_reset: false, marker: '&')`

---

Other languages should implement the following functions:

Method           | Function
:---:            | :---:
`Dahlia.convert` | `dahlia`
`Dahlia.print`   | `dprint`
`Dahlia.input`   | `dinput`
`Dahlia.test`    | `dtest`
`Dahlia.reset`   | `dreset`

```ts
function dahlia(
  str: string,
  depth: Depth | DepthString | DepthInt = Depth.LOW,
  no_reset: boolean = false,
  no_color: boolean? = null
): string {}

function dprint(
  ...,
  depth: Depth | DepthString | DepthInt = Depth.LOW,
  no_reset: boolean = false,
  no_color: boolean? = null,
  ...
) {}

function dinput(
  prompt: string,
  depth: Depth | DepthString | DepthInt = Depth.LOW,
  no_reset: boolean = false,
  no_color: boolean? = null
): string {}
```

### Utility functions

**TODO:**
- [ ] clean (for removing dahlia codes)
- [ ] clean_ansi (for removing ANSI codes)
- [ ] quantize_ansi (maybe make it optional?)
