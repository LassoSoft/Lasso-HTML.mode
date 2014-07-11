Lasso-HTML.mode
===============

A Lasso syntax mode for [Coda](http://panic.com/coda). This is a replacement for
the original LassoScript-HTML mode included in Coda 2.0, and comes with improved
file recognition, syntax highlighting, code folding, autocompletion, support for
the navigator sidebar, and full support for Lasso 9 syntax.


Installation
------------

Download/extract the zip or clone this repo, rename to `Lasso-HTML.mode`, and
drop into `~/Library/Application Support/Coda 2/Modes`. If that folder doesn't
exist, create it.

Then, have Coda use the mode either by opening its Preferences and assigning the
".lasso" extension to Lasso-HTML under Editor > Custom Syntax Modes, or
opening up the application package and removing the old LassoScript-HTML mode
from Contents > Resources.

Features
--------

### Syntax highlighting

- Full support for Lasso 9 syntax, including all delimiter styles (`[ ... ]`,
`<?LassoScript` / `<?lasso` / `<?=` ... `?>`), tag literals, backticked strings,
doc comments, and define statements.

- Folding works on both 9-style braces blocks and 8-style container blocks, for
all container tags that Lasso 8 ships with. Will still fold if the block is
interspersed with sections of HTML. Also folds between Lasso delimiters.

  - The default fold level is 2, so the `⌘ command` + `⇧ shift` + `↑ up`
  shortcut will fold blocks inside delimiters, but not the delimiter blocks
  themselves.

- Ignores square brackets after `[no_square_brackets]` is encountered, and all
delimiters in a `[noprocess] ... [/noprocess]` block.

- Inherits from other modes to highlight HTML, CSS, and JavaScript where
appropriate.

- When a Lasso 9 hashbang, comment, tag literal, capture block, or define
statement is encountered outside delimiters, the highlighter assumes the file is
a library or shell script and doesn't require delimiters to highlight as Lasso.

- Highlights Lasso 8's built-in types and tags, and Lasso 9's built-in types,
traits, and unbound methods; as well as language keywords, variables, strings,
numerals, method calls, keyword parameters, tag literals, logical operators, and
comments.

### Navigator

The Navigator sidebar generates a clickable list of elements in the current file
for quickly jumping to points in the code. The following types of entries are
supported:

- `C`: `define_type` and Lasso 9 type definitions
- `T`: Lasso 9 trait definitions
- `F()`: `define_tag` and Lasso 9 unbound method definitions
- `M`: Lasso 9 member methods
- `E`: Lasso 9 externally-defined member methods
- `#`: `include` and `library` statements
- folder: `namespace_using`
- `!`: versioning conflicts
- bookmark: reminders in comments, indicated by `// CAPS:`
- `⚠`: important reminders in comments, indicated by `// CAPS!`
- (no icon): markers in comments, indicated by `//- mark Text` or `//! Text`

### Autocomplete

- Includes all built-in tags, types, traits, unbound methods, and keywords in
the autocomplete dictionary.

- Any type/trait/method definitions, imported trait references, or variable
definitions or references in the current file are added to the autocomplete
dictionary.

### Other

- Associated with the filename extensions ".lasso", ".las", & ".incl", plus
files starting with a Lasso delimiter or hashbang.

- Block comments can be added/removed with the `⌘ command` + `/ slash` shortcut.


Notes
-----

- Only boolean operators are highlighted, but I've left a commented-out regex in
SyntaxDefinition.xml that covers all operators.

- Coda's PHP mode already claims ".inc" files, so you'll want to change that in
the Preferences if you use that extension. (It also currently claims any files
containing `<?`, but that only applies to filenames without a recognized
extension.)


Known Issues
------------

- Doesn't currently work with SubEthaEdit 3.5 or 4.

- Folding of container tag blocks requires that if such a block is interrupted
by HTML, the closing tag needs to be enclosed with the same style of delimiters
(square vs. angle brackets) as the opening tag.

- When member method definitions are separated by commas rather than keywords, a
method definition not surrounded by braces will cause the next method to not
appear in the navigator sidebar.

- `[no_square_brackets]` will cause the highlighter to ignore square brackets in
plain HTML as well as inside `<script>` and `<style>` blocks, but not inside
HTML tags or tag attributes. (This is technically fixable, but would make the
highlighter far more complex.)

  - `[no_square_brackets]` appearing inside a `<script>` or `<style>`
  block will disable the block's JavaScript or CSS highlighting.
  
  - `[no_square_brackets]` isn't recognized when it appears inside a container
  block or braces block.

- The highlighter will mark everything as Lasso as soon as it encounters valid
Lasso code outside delimiters anywhere in the file, rather than only at the
beginning. This is due to limitations of the highlighter's state system.

- A JavaScript action attribute (e.g. `onClick`) or `href` or `src` inside an
HTML tag will be highlighted incorrectly if its value contains a Lasso delimiter
block.

- HTML tag blocks aren't foldable, since they frequently overlap Lasso delimiter
blocks.

- For comma-separated variable definitions (e.g. `local(a = 1, b = 2)`) or trait
imports (e.g. `import trait_custom1, trait_custom2`), the parser currently adds
only the first in the list to the autocomplete dictionary.
