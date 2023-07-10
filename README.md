# Dahlia Specification

- [Dahlia Specification](#dahlia-specification)
  - [Introduction](#introduction)
  - [Syntax](#syntax)
    - [Basic formatting](#basic-formatting)
    - [Custom colors](#custom-colors)
    - [Background colors](#background-colors)
    - [Escaping](#escaping)
  - [Formatting Reference](#formatting-reference)
    - [ANSI Templates](#ansi-templates)
    - [ANSI Color Codes](#ansi-color-codes)
    - [ANSI Style Codes](#ansi-style-codes)
  - [API](#api)
    - [Utility functions](#utility-functions)


## Introduction

Dahlia is a simple text formatting library for the terminal, inspired by text
formatting in Minecraft. Text formatting in Dahlia is done by typing a special
character (`&` by default) followed by a format code and finally the text to be
formatted, so it works just like in the game.

This document describes the specification for implementing the Dahlia library
in certain languages.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## Syntax

### Basic formatting
Basic formatting is done by 
- [ ] &[0-9a-g] for one of the basic 17 colors (maybe remove g? it's kinda useless)
- [ ] &[i-p] for text styles
- [ ] &r for reset

### Custom colors
- [ ] &[#FFFFFF] for custom color

### Background colors
- [ ] '~' after marker for background

### Escaping
- [ ] escaping (how???)



## Formatting Reference


### ANSI Templates

Category      | Foreground           | Background
---           | ---                  | ---
text styles   | `ESC[{}m`            | `ESC[{}m`
3-bit colors  | `ESC[{}m`            | `ESC[{}m`
4-bit colors  | `ESC[{}m`            | `ESC[{}m`
8-bit colors  | `ESC[38;5;{}m`       | `ESC[48;5;{}m`
24-bit colors | `ESC[38;2;{};{};{}m` | `ESC[48;2;{};{};{}m`


### ANSI Color Codes

Dahlia | 3-bit | 4-bit | 8-bit | 24-bit
:---:  | :---: | :---: | :---: | :---:
`0`    | `30`  | `30`  | `0`   | `0 0 0`
`1`    | `34`  | `34`  | `19`  | `0 0 170`
`2`    | `32`  | `32`  | `34`  | `0 170 0`
`3`    | `36`  | `36`  | `37`  | `0 170 170`
`4`    | `31`  | `31`  | `124` | `170 0 0`
`5`    | `35`  | `35`  | `127` | `170 0 170`
`6`    | `33`  | `33`  | `214` | `255 170 0`
`7`    | `37`  | `37`  | `248` | `170 170 170`
`8`    | `30`  | `90`  | `240` | `85 85 85`
`9`    | `34`  | `94`  | `147` | `85 85 255`
`a`    | `32`  | `92`  | `83`  | `85 255 85`
`b`    | `34`  | `96`  | `87`  | `85 255 255`
`c`    | `31`  | `91`  | `203` | `255 85 85`
`d`    | `35`  | `95`  | `207` | `255 85 255`
`e`    | `33`  | `93`  | `227` | `255 255 85`
`f`    | `37`  | `97`  | `15`  | `255 255 255`
`g`    | `33`  | `33`  | `184` | `221 214 5`


### ANSI Style Codes

Dahlia | ANSI  | Effect
:---:  | :---: | :---
`i`    | `7`   | Inverse mode
`j`    | `2`   | Dim mode
`k`    | `8`   | Hidden mode
`l`    | `1`   | Bold mode
`m`    | `9`   | Strikethrough mode
`n`    | `4`   | Underline mode
`o`    | `3`   | Italic mode
`p`    | `5`   | Blinking mode
`r`    | `0`   | Full reset



## API

**TODO:**
- [x] type/method structure for OOP-like
- [x] function sigs for the rest
- [ ] explain parameters
  - [x] depth
  - [x] no_reset
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
Target languages CAN support the concept of default parameter values.
If they do, they SHOULD stick to defaults in the above code block.

The `depth` parameter refers to color depth (3, 4, 8 and 24 refer to the number
of bits<!-- improve this -->) of should support at least 1 of the above types <!--the fuck is this sentence-->
(preferably all of them if possible). This parameter SHOULD default
to low depth (4). If a language only allows one type to be supported, the
preference is:
1. enum
2. integers
3. strings

> **Note**  
> If enums require hacky code or are hard to use in a given language, integers
> are preferred instead.

The `no_reset` parameter specifies whether the reset code (`\033[0m`; `&r` by
default) should be appended at the end of the string if not present yet.
Defaults to `false`.


`Dahlia` instances SHOULD be:
- hashable
- comparable: using `depth`, `no_reset` and `marker` values
- printable: preferably in a way that's valid syntax, e.g.:
  - Python: `Dahlia(depth=Depth.HIGH, no_reset=False, marker='&')`  
  - Swift: `Dahlia(depth: Depth.HIGH, no_reset: false, marker: '&')`

---

Other languages SHOULD implement the following functions:

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
