# <img src="http://elm-lang.org/assets/logo.svg" width="26"> Elm Syntax Reference

## Navigation Index
1. [Bootstrap Example](#bootstrap-example)
2. [Inline Documentation](#inline-documentation)
3. [Package Structure](#package-structure)
    * [Dependency Resolution](#dependency-resolution)
4. [CLI Utilities](#cli-utilities)
    * [Interactive Shell](#interactive-shell)
    * [Build Pipeline](#build-pipeline)
    * [Registry Management](#registry-management)
        * [Version Control](#version-control)
        * [API Docs](#api-docs)
5. [Runtime Integration](#runtime-integration)
6. [Core Primitives](#core-primitives)
    * [Numeric Types](#numeric-types)
    * [Text Literals](#text-literals)
    * [Boolean Logic](#boolean-logic)
    * [Type Constraints](#type-constraints)
7. [Data Structures](#data-structures)
    * [Sequential Collections](#sequential-collections)
    * [Fixed-size Containers](#fixed-size-containers)
    * [Key-Value Maps](#key-value-maps)
    * [Additional Types](#additional-types)
8. [Function Definitions](#function-definitions)
    * [Lambda Expressions](#lambda-expressions)
    * [Operator Notation](#operator-notation)
9. [Custom Types](#custom-types)
    * [Discriminated Unions](#discriminated-unions)
    * [Optional Values](#optional-values)
10. [Aliasing Types](#aliasing-types)
11. [Type Signatures](#type-signatures)
12. [Expression Operators](#expression-operators)
    * [Math Operations](#math-operations)
    * [Bit Manipulation](#bit-manipulation)
    * [Value Comparison](#value-comparison)
    * [Boolean Algebra](#boolean-algebra)
    * [Pipeline Composition](#pipeline-composition)
    * [Miscellaneous](#miscellaneous)
13. [Branching Logic](#branching-logic)
    * [Conditional Expressions](#conditional-expressions)
    * [Pattern Matching](#pattern-matching)
    * [Scoped Bindings](#scoped-bindings)
14. [JavaScript Interop](#javascript-interop)

## Language Overview
 - functional paradigm enforcement
 - compile-time type checking
 - zero runtime crashes
 - rendering performance exceeds competitors
 - integrated dependency manager
 - compiler toolchain included
 - seamless JS/CSS/HTML integration
 - minimal syntax overhead
 - frontend development enjoyable again

## Bootstrap Example
File `Bootstrap.elm`:
```elm
import Html exposing (h1, text)
import Html.Attributes exposing (id)

-- Minimal working example
main =
  h1 [id "bootstrap"] [text "Hello World!"]
```

## Inline Documentation
```elm
-- Single-line annotation

{-
Multiline
annotation block
-}
```

Documentation for [API generation](#api-docs)

## Package Structure
```elm
-- Export all module contents
module ProjectModule exposing (..)

-- Selective entity exports
module ProjectModule exposing (CustomType, staticValue)

-- Union type variant control
module ProjectModule exposing
    ( ErrorType(Unauthorized, ServiceDown)
    , DataType(..)
    )

type ErrorType
    = Unauthorized String
    | ServiceDown String
    | NotAvailable String
```

#### Dependency Resolution
```elm
-- namespace imports
import String                       -- String.toUpper, String.repeat
import String as Str                -- Str.toUpper, Str.repeat

-- direct imports
import ProjectModule exposing (..)                     -- ErrorType, DataType
import ProjectModule exposing ( ErrorType )            -- ErrorType
import ProjectModule exposing ( ErrorType(..) )        -- ErrorType, Unauthorized, ServiceDown
import ProjectModule exposing ( ErrorType(Unauthorized) ) -- ErrorType, Unauthorized
```

## CLI Utilities
#### Interactive Shell
`elm repl` / `elm-repl`

Load dependencies
```elm
> import List
```

Inspect signatures
```elm
> List.map
<function> : (a -> b) -> List a -> List b
> (++)
<function> : appendable -> appendable -> appendable
```

Evaluate expressions
```elm
> 1 + 1
2 : number
```

Multiline with backslash `\`
```
> fn a b = \
|    a + b
<function> : number -> number -> number
```
 
#### Build Pipeline
`elm make` / `elm-make`

```bash
# Standard build
elm make Bootstrap.elm -> index.html

# Named output
$ elm make Bootstrap.elm --output bootstrap.js

# Multi-file compilation
$ elm make Bootstrap.elm ProjectModule.elm --output bundle.js

# Verbose mode
$ elm make Bootstrap.elm --warn

# HTML generation
$ elm make Bootstrap.elm --output app.html
```

#### Registry Management
`elm package` / `elm-package`

Automatic `elm-package.json` updates during install.
```bash
# Add dependency
$ elm-package install evancz/elm-html

# Version pinning
$ elm-package install evancz/elm-html 1.0.0

# Compare releases
$ elm-package diff evancz/virtual-dom 2.0.0 2.1.0
Comparing evancz/virtual-dom 2.0.0 to 2.1.0...
This is a MINOR change.

------ Changes to module VirtualDom - MINOR ------

    Added:
        attributeNS : String -> String -> String -> VirtualDom.Property
```

#### API Docs
Publishing requires complete documentation coverage.

```elm
module ApiDocumentation exposing
    ( CustomType
    , staticValue
    , helperFunction
    )

{-| Top-level module description

# Primary section header
@docs CustomType, staticValue

# Utility Functions
@docs helperFunction

-}

import Random exposing (int)

{-| Type documentation block -}
type CustomType = Bool

{-|-}
-- Minimal doc comment
staticValue : Int
staticValue = 1 + 1

{-| Extended function description -}
helperFunction : Generator Int
helperFunction =
    int 0 64

{-| Unexported values don't require doc comments -}
-- Standard comment syntax acceptable
privateValue =
    "No documentation needed"
```

* Doc comments: `{-|` opening, `-}` closing
* Module docs: after module declaration, before imports
* Section grouping via `@docs <identifiers>` with Markdown formatting
* All exported entities require doc comments

#### Version Control
Add `README.md` (required for publishing)<br/>
Initial version: `1.0.0`<br/>
Semantic versioning: `MAJOR.MINOR.PATCH`<br/>
* `PATCH` - no API changes, safe updates
* `MINOR` - additive changes only, backward compatible
* `MAJOR` - breaking changes, removals or modifications

`elm-package.json` configuration
* `source-directories` - source lookup paths within workspace
* `exposed-modules` - public API surface after publishing

```bash
# elm-package.json
"source-directories": [
    ".",
    "DirOne",
    "DirTwo"
]
"exposed-modules": [
    "DirOne",
    "PublicModule",
    "CoreModule"
]

├── DirOne
│   └── DirOne.elm        # Compiled + exposed
├── DirTwo
│   └── DirTwo.elm        # Compiled, not exposed
├── DirThree
│   └── DirThree.elm      # Ignored by compiler
├── README.md
├── elm-package.json
├── PublicModule.elm      # Compiled + exposed
└── CoreModule.elm        # Compiled + exposed
```

Initial release
```bash
$ git tag -a 1.0.0 -m "initial version"
$ git push --tags

$ elm-package publish
```

Version bump
```bash
$ elm-package bump

$ git tag -a 1.0.1 -m "patch update"
$ git push --tags

$ elm-package publish
```

## Runtime Integration
Fullscreen mode
`elm-make Bootstrap.elm` -> `elm.js`
```html
<script type="text/javascript" src="elm.js"></script>
<script type="text/javascript">
    Elm.Main.fullscreen();
</script>
```

Targeted embedding
```html
<script type="text/javascript" src="elm.js"></script>
<script type="text/javascript">
    var container = document.getElementById('elm-container');

    Elm.Main.embed(container);
</script>
```

Headless execution
```javascript
Elm.Main.worker();
```

## Core Primitives
#### Numeric Types
`Int` and `Float`, generic `number` encompasses both:
```elm
> 1
1 : number
> 2.0
2 : Float
> truncate 0.1
0 : Int
> truncate 1
1 : Int
```

#### Text Literals
`Char` for single characters, `String` for sequences
```elm
> 'a'
'a' : Char
> "Hello"
"Hello" : String
```

Multiline strings
```elm
"""
Line one
Line two
"""
```

Character literals require single quotes
```elm
> 'ab'
-- SYNTAX PROBLEM --
> "ab"
"ab" : String
```

#### Boolean Logic
```elm
> True
True : Bool
> False
False : Bool
```

#### Type Constraints
`comparable` - `ints`, `floats`, `chars`, `strings`, `lists`, `tuples`
<br/>
`appendable` - `strings`, `lists`, `text`
<br/>
Generic types: `a`, `b`, `c` accept any value including functions

## Data Structures
Immutability enforced across all data structures.

#### Sequential Collections
`list` contains homogeneous comma-separated values in brackets:
```elm
> []
[] : List a
> [1,2,3]
[1,2,3] : List number
> ["a", "b", "c"]
["a","b","c"] : List String
```

Construction methods
```elm
> List.range 1 4
> [1,2,3,4]
> 1 :: [2,3,4]
> 1 :: 2 :: 3 :: 4 :: []
```

#### Fixed-size Containers
Tuples group 2-9 heterogeneous expressions.
Type signature reflects arity and component types.
```elm
> (1, "2", True)
(1,"2",True) : ( number, String, Bool )
```

Comma as [prefix function](#operator-notation)
```elm
> (,,,) 1 True 'a' []
(1,True,'a',[]) : ( number, Bool, Char, List a )
```

Destructuring syntax
```elm
(x, y) = (1, 2)
> x
1 : number
```

#### Key-Value Maps
`record` structure similar to JS objects
```elm
configuration =
 { theme = "Dark",
   version = 2,
   enabled = True
 }
```

Property access
```elm
> configuration.theme
"Dark" : String
> .theme configuration
"Dark" : String
```

Immutable updates
```elm
> updated = { configuration | theme = "Light", version = 3, enabled = False }
> configuration.theme
"Dark" : String
> updated.theme
"Light" : String
```

Destructuring records
```elm
{ theme, version, enabled } = configuration
> theme
"Dark" : String
```

#### Additional Types
Standard library includes:
 * [Array](http://package.elm-lang.org/packages/elm-lang/core/latest/Array)
 * [Dict](http://package.elm-lang.org/packages/elm-lang/core/latest/Dict)
 * [Set](http://package.elm-lang.org/packages/elm-lang/core/latest/Set)

## Function Definitions
Basic syntax
```elm
-- identifier | parameters = implementation
add a b = a + b

-- tuple parameter grouping
add (a, b) = a + b
```

Default currying behavior.<br/>
Multi-argument functions are chained single-argument functions:
```elm
-- Equivalent expressions
myFunction arg1 arg2
((myFunction arg1) arg2)

-- Partial application example
> subtract x y = (-) x y
<function> : number -> number -> number
> subtract5 = subtract 5
<function> : number -> number
> subtract5 20
15 : number
```

#### Lambda Expressions
Anonymous function syntax
```elm
-- (\parameters -> implementation)
-- parenthesized, backslash prefix
(\n -> n < 0)
(\x y -> x * y)
```

#### Operator Notation
_Prefix_ applies [operators](#expression-operators) as standard functions via parentheses.
```elm
-- Standard infix
> "abc" ++ "def"
"abcdef" : String

-- Prefix application
> (++) "abc" "def"
"abcdef" : String
```

## Custom Types
#### Discriminated Unions
_Union types_ define custom discriminated unions.<br/>
Values match one variant from the definition. Pairs naturally with [pattern matching](#pattern-matching).
```elm
type Direction = North | South | East | West
```

Tagged variants with associated data
```elm
type Direction
    = North Int
    | South Int
    | East Bool
    | Coordinates (Float, Float)

-- Function application
navigate ( Coordinates (45.7, 67.5) )
```

Parameterized union types
```elm
type Entity a
  = FirstName String
  | LastName String
  | Age Int
  | Metadata a
```

#### Optional Values
`Maybe` handles nullability, optional parameters, error scenarios.
```elm
-- Import Maybe
import Maybe exposing ( Maybe(..) )

-- Generic optional wrapper
type Maybe a = Just a | Nothing
```

[Type signature](#type-signatures) documents optional return:
```elm
validateId : Int -> Maybe Int
validateId id =
  if id >= 0 then
    Just id
  else
    Nothing
```

## Aliasing Types
`type alias` creates named type synonyms
```elm
type alias Username = String
type alias Birthdate = String

type alias UserRecord = { username: Username, birthdate: Birthdate }
```

Alias usage in annotations
```elm
userRecord : UserRecord
userRecord =
    { username = "alice", birthdate = "15/03/1995" }
```

Aliases equivalent to original types
```elm
type alias Username = String

username : Username
username =
  "alice"

alternateUsername : String
alternateUsername =
  "alice"

-- True
username == alternateUsername
```

## Type Signatures
Automatic type inference for most declarations.<br/>
```elm
-- identifier : arg1Type -> arg2Type -> returnType
operation : Int -> List -> Int
```
Generic signatures: function accepting __a__ returning __b__, transforming `List a` to `List b`
```elm
map: (a -> b) -> List a -> List b
```

Record field pattern matching
```elm
-- Requires fields x and y
scale {x,y} =
    x * y
```

Record annotations
```elm
position : { x : Float, y : Float }
position =
    { x = 0,
      y = 0
    }
```

## Expression Operators
Operators are binary _functions_.

#### Math Operations
|Symbol|Purpose|Signature|
|------|-------|---------|
|`+`|addition|`number -> number -> number`
|`-`|subtraction|`number -> number -> number`
|`*`|multiplication|`number -> number -> number`
|`/`|float division|`Float -> Float -> Float`
|`//`|integer division|`Int -> Int -> Int`
|`^`|power|`number -> number -> number`
|`%`|remainder|`Int -> Int -> Int`

#### Bit Manipulation
|Symbol|Purpose|Signature|
|------|-------|---------|
|`and`|AND operation|`Int -> Int -> Int`
|`or`|OR operation|`Int -> Int -> Int`
|`xor`|XOR operation|`Int -> Int -> Int`

#### Value Comparison
|Symbol|Purpose|Signature|
|------|-------|---------|
|`==`|equality|`comparable -> comparable -> Bool`
|`/=`|inequality|`comparable -> comparable -> Bool`
|`<`|less than|`comparable -> comparable -> Bool`
|`<=`|less or equal|`comparable -> comparable -> Bool`
|`>`|greater than|`comparable -> comparable -> Bool`
|`>=`|greater or equal|`comparable -> comparable -> Bool`

#### Boolean Algebra
|Symbol|Purpose|Signature|
|------|-------|---------|
|`&&`|conjunction|`Bool -> Bool -> Bool`
|`\|\|`|disjunction|`Bool -> Bool -> Bool`
|`not`|negation|`Bool -> Bool`

#### Pipeline Composition
|Symbol|Purpose|Signature|
|------|-------|---------|
|`<\|`|backward application `f <\| x == f x`|`(a -> b) -> a -> b`
|`\|>`|forward application `x \|> f == f x`|`a -> (a -> b) -> b`
|`<<`|right-to-left composition|`(b -> c) -> (a -> b) -> a -> c`
|`>>`|left-to-right composition|`(a -> b) -> (b -> c) -> a -> c`

#### Miscellaneous
|Symbol|Purpose|Signature|
|------|-------|---------|
|`++`|concatenation|`appendable -> appendable -> appendable`|
|`::`|list prepend|`a -> List a -> List a`|
|`as`|value aliasing `(x, y) as t == t = (x, y)`|`a -> a`|

## Branching Logic
#### Conditional Expressions
Branch types must unify.
```elm
if a < 1 then
    "Zero"
else
    "Nonzero"

-- Chained conditions
if y > 0 then
    "Positive"
else if x /= 0 then
    "Nonzero"
else
    "Zero"
```

Type coercion not supported.<br/>
Conditions must be strictly boolean.
```elm
> if 1 then "invalid" else "also invalid"
- TYPE MISMATCH --
```

#### Pattern Matching
Case expression matches values against patterns
```elm
type Account
    = Active
    | Suspended

handleAccount status =
  case status of
    Active ->
      -- activate logic
    Suspended ->
      -- suspend logic
```

Tagged union destructuring
```elm
type Account
    = Active Int
    | Suspended (Int, String)

handleAccount status =
  case status of
    Active userId ->
      -- process userId
    Suspended details ->
      -- handle details

handleAccount ( Active 42 )
handleAccount ( Suspended (0, "inactive") )
```

#### Scoped Bindings
`let` defines local bindings.
```elm
let
  x = 3 * 8
  y = 4 ^ 2
in
  x + y
```

Expression simplification
```elm
let
  validUsers = List.filter (\u -> u.status /= 0) model.users
in
  { model | users = validUsers }
```

## JavaScript Interop
Bidirectional communication mechanism.

#### Inbound (JS → Elm)
```elm
-- port declaration
port module Main exposing (..)

-- define receiver
port inboundPort : (String -> msg) -> Sub msg
```

```javascript
var app = Elm.Main.embed(container);

// send data
app.ports.inboundPort.send("Data from JS");
```

#### Outbound (Elm → JS)
```elm
port outboundPort : String -> Cmd msg
```

```javascript
function handleData(data) {
    console.log(data);
}

// listen for events
app.ports.outboundPort.subscribe(handleData);

// cleanup
app.ports.outboundPort.unsubscribe(handleData);
```

#### Type Mapping
|JavaScript|Elm|
|----------|---|
|Booleans|Bool|
|Numbers|Int, Float|
|Strings|Char, String|
|Arrays|List, Array, Tuples|
|Objects|Records|
|JSON|[Json.Encode.Value](http://package.elm-lang.org/packages/elm-lang/core/latest/Json-Encode#Value)|
|null|Maybe Nothing|

# PR Merge: 2025-11-26 01:55:20
