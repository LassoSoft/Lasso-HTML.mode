Lasso-HTML.mode
===============

A Lasso syntax mode for [Coda 2.5](http://panic.com/coda). This is a replacement
for the original LassoScript-HTML mode included in Coda, and comes with improved
file recognition, syntax highlighting, code folding, autocompletion, support for
the navigator sidebar, and full support for Lasso 9 syntax.


Installation
------------

Download/extract the zip or clone this repo, rename the folder to
`Lasso-HTML.mode`, and double-click the resulting package. Or, drop into
`~/Library/Application Support/Coda 2/Modes`. If that folder doesn't exist,
create it.

Then, have Coda use the mode either by opening its Preferences and assigning the
".lasso" extension to Lasso-HTML under Editor > Custom Syntax Modes, or
opening up the application package and removing the old LassoScript-HTML mode
from `Contents/Resources/`.


Features
--------

### Syntax highlighting

- Full support for Lasso 9 syntax, including all delimiter styles (`[ ... ]`,
`<?LassoScript` / `<?lasso` / `<?=` ... `?>`), tag literals, backticked strings,
doc comments, and `define` statements.

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

- A file starting with an open square or angle bracket is assumed to be HTML
with Lasso between delimiters; otherwise, it's read as a Lasso library or shell
script.

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
- `()`: `define_tag` and Lasso 9 unbound method definitions
- `M`: Lasso 9 member methods
- `E`: Lasso 9 externally-defined member methods
- `#`: `include` and `library` statements
- folder: `namespace_using`
- `!`: versioning conflicts
- bookmark: reminders in comments, indicated by `// CAPS:`
- `⚠`: important reminders in comments, indicated by `// CAPS!`
- (no icon): markers in comments, indicated by `//- mark Text` or `//! Text`

### Autocomplete

- Includes all built-in types, traits, unbound methods, and keywords in the
autocomplete dictionary. (Generated using [this script][1].)

- Any type/trait/method definitions, imported trait references, or variable
definitions or references in the current file, or other open files referenced
with `include` statements, are added to the autocomplete dictionary on-the-fly.

### Other

- Associated with the filename extensions ".lasso", ".lasso8", ".lasso9",
".las", & ".incl", plus files starting with a Lasso delimiter or hashbang.

- Line or block comments can be added/removed with the `⌘ command` + `/ slash`
shortcut, depending on the current selection.


Notes
-----

- Only boolean operators are highlighted, but there's a commented-out regex in
SyntaxDefinition.xml that covers all operators.

- Lasso 8-only tags were left out of the autocomplete dictionary, but will still
highlight.

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
method definition not surrounded by braces will cause the next method not to
appear in the navigator sidebar.

- `[no_square_brackets]` will cause the highlighter to ignore square brackets
among plain HTML and within `<script>` and `<style>` blocks, but not inside
HTML tags or tag attributes. (This is technically fixable, but would increase
the highlighter's complexity considerably.)

- `[no_square_brackets]` isn't recognized when it appears among markup that's
inside a container block or braces block, nor in `<script>` or `<style>` blocks.

- A closing `[/noprocess]` among HTML/XML will work only if it's nested at the
same level as its opening `[noprocess]`.

- HTML tag blocks aren't foldable, since they frequently overlap Lasso delimiter
blocks.

- For comma-separated variable definitions (e.g. `local(a = 1, b = 2)` or
`var(x, y) = (:3, 4)`), the parser adds only the first in the list to the
autocomplete dictionary.

[1]: https://bitbucket.org/EricFromCanada/ericfromcanada.bitbucket.org/src/default/lasso/completions-generator.lasso
