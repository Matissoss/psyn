<div align=center>
    <h1>psyn</h1>
</div>

## about

`psyn` (`POLON Syntax Notation`) is minimalistic syntax description format. `psyn` is:

- Format for people, not machines
- Very easy to adapt/learn (you can learn `psyn` in ~10 minutes)
- Very simple (there are only 3 core components, rest are derivatives)
- Universal (`psyn` can fit into tiny CLI's as well as into assemblers/languages)

`psyn` **DOES/IS NOT**:

- Standalone file format
- Format to generate ready parsers
- Inform user of everything (it only presents shape of syntax, rest is informed by author)

It aims to solve following problem:

> How to inform user of syntax of tool in a way that is expressive, minimal and easy to understand?

## entirety of format

### psyn v1

`psyn` v1 is currently latest version of `psyn`.

#### core nodes

- `<VALUE>` - needed, variable
- `[VALUE]` - optional, variable
- `VALUE` - needed, static

To use `<>[]` as characters, add `\` in front of them (`\<\>\[\]`).

#### reserved nodes

> Variants here use `psyn` core nodes and few reserved.

- `...` - something
- `[...]` - loop
- `<!>` - end of line
- `?NAME:TYPE` - variable node of name `NAME` and type `TYPE`
- `OPTION0/OPTION1` - Switching Node

#### abbreviations

Abbreviations are basically `*ABBR*` node.

- `*ABBR* = ?Node:*Node*` - abbreviation assigment
- `*ABBR*` - reference to abbreviation

If `*ABBR*` has some variable data (like `[VALUE]`/`<VALUE>`), then it can be used as a "template".

Example for `*LOOP_TEMPLATE* = <VALUE>[...]`

- `*LOOP_OF_4* = *LOOP_TEMPLATE* where: VALUE = 4`

Rule for this is `*ABBR* = *OTHER_ABBR* where: <?ARG0:Arg0Type>[...]<?ARGN:ArgNType>`.

You can also use nested/inline templates. They are not needed if your project is very small and does not require a lot of dependencies.

##### nested templates

`*ABBR* = *OABBR* where: <?Abbreviation:Template> where: ([<?ARG0:Name> = <?VALUE:ArgType>][...]), [...],`

Example:

- `*OTEMP0* = <VALUE>`
- `*OTEMP1* = *OTEMP0*[VALUE]`
- `*ABBR* = *OTEMP1* where: *OTEMP0* where: (VALUE = 20), VALUE = 10`

##### inline templates

`*NESTED_TEMPLATE* = (\</\[/ )(*TEMPLATE_EXTENSION*)(\</\]/ )`

Example:

- `*OTEMP0* = <VALUE>`
- `*OTEMP1* = (*OTEMP0* where: VALUE = 10)[VALUE]`
- `*ABBR = *OTEMP1* where: VALUE = 20`

#### examples

##### psyn node syntax

`*PSYN_NODE* = (\</\[/ )[?][N.] VALUE [:<TYPE>](\>/\]/ )`

##### rust function syntax

`*RUST_FN* = [extern "<ABI>"] [unsafe] [async] fn <FN_NAME>([?ARG0:*RustFnArg*], [...]) [-> ?RETURN:*RustType*] {}`

`*RustFnArg* = <NAME>: <*RustType*>`

##### c function syntax

`*C_FN* = <?RETURN:*C_TYPE*> <FN_NAME>([?ARG0:*CFnArg*], [...]) {}`

`*CFnArg* = <*C_TYPE*> <VAR_NAME>`

##### x86 pasm assembly syntax

```
*X86_INS* = <*MNEMONIC*> [?op0:*Operand*], [...]

*Operand* = <?Imm:*Immediate*>/<?Reg:*Register*>/<?SymbolRef:*SymbolRef*>

*Immediate* = <?Val:Number>

*Register* = <?Reg:Register>

*SymbolRef* = @<SYMB_NAME>[:<?Reltype:*RelType*>][:<?Addend:*Immediate*>]
```

## credits

`psyn` was made by matissoss for `POLON` project and licensed under MIT license.
