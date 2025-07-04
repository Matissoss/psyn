<div align=center>
    <h1>psyn</h1>
</div>

## about

`psyn` is minimalistic format for defining syntax. It focuses on composition rather than complexity.

It was made as a very important part for syntax explaination in `POLON` toolchain.

## entirety of format

This is spec for latest `psyn` version (`psyn v1`):

- `<VALUE>` - needed and variable value
- `[VALUE]` - optional and variable value
- `VALUE` - needed and static value

If you want to use one of chars: `[`, `]`, `<`, `>` then add `\` in front of them.

Rest of nodes are derivatives of them.

Here are the reserved ones:

- `...` = something (most commonly means this is only one fragment)
- `[...]` = repeating pattern
- `(NODE0/NODE1/[...])` = switching pattern (one of them can be used)
- `(\[/\<) NAME (\]/\>)` = optional/needed static value
- `...? NAME:TYPE...` = (WITH SPACE) typed variable of name `NAME` and type `TYPE`
- `...N. NODE...` = (WITH SPACE) if `N`th pattern of switching pattern is chosen, then `NODE` is a optional/needed/static
- `...*NAME*...` = needed/optional abbreviation
- `*ABBR* = NODE` - abbreviation definition
- `*ABBR* = *OABBR* where: <? EVERY_ARGUMENT:TheirTypes>` = abbreviation definition using other abbreviation as template
- `<!>` = nothing else can be on the same line.

If you want the whole `psyn` syntax explaination in `psyn` then here it is: 

- without abbreviations: `(\</\[/ )[?][N.] VALUE[1. :<TYPE>](\</\[/ )[...]<!>`
- with abbreviations:
    - `*NODE_START* = (\</\[/ )`
    - `*NODE_END* = (\>/\]/ )`
    - `<*NODE_START*>[?][N.] VALUE[1. :<TYPE>] <*NODE_END*>[...]<!>`

## credits

`psyn` was made by matissoss and licensed under MIT license.
